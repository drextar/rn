# 📢 Release Notes

*Deployed: • Status: **Em produção***

---

## 🚀 Destaques — **Novas Features**

|                                           | Feature           | Valor Gerado                                                                                                                                                                                                                          |
| ----------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 🛡️ **Guardrail de Frete > R\$ 1.000,00** | **Simulation-MS** | Validação de outliers de frete: quando o Seller retorna valor acima de **R\$ 1.000,00**, a RN classifica o evento, registra auditoria e aciona o fluxo definido (revisão/ fallback), evitando erros de integração e custos indevidos. |
| 💸 **Guardrail de Preço ±60%**            | **Produto-MS**    | Proteção contra preços anômalos: bloqueia ou roteia para revisão quando o preço enviado pelo Seller é **>160%** ou **<40%** do preço original; dispara alerta operacional e trilha de auditoria.                                      |

---

## 📦 Mudanças por Microserviço

### **OrderStatus-MS**

* 🗃️ **Cache para fluxo de cancelamento de pedido**

  * Evita recomputo e reprocesso em curto intervalo; respostas idempotentes a repetidos.

### **Seller-MS**

* 🧭 **Definição do fluxo de Cadastro EQ3**

  * Padrões, contratos e estados mapeados para onboarding conforme EQ3.

> *Obs.: As novas features de **Simulation-MS** e **Produto-MS** estão detalhadas em **Destaques**.*

---

## 🔄 Melhorias Cross-Service

* 🧰 **Novo template de logs**

  * **JSON estruturado** com `traceId`, `correlationId`, `service`, `endpoint`, `latencyMs`, `statusCode`.
  * **Mascaramento de PII** e padronização de níveis (`INFO` core, `DEBUG` fluxo).
  * Facilita troubleshooting e correlação entre serviços.
* 📈 **Migração de dashboards — Datadog Itaú**

  * Painéis por domínio (Pedido, Seller, Produto, Simulation) com latência, taxa de erro, throughput e saturação.
  * Alertas unificados, tags padronizadas e nomenclatura consistente.
 
⏱ Jornada & Core Time

Core time fixo (já discutido).

Reuniões apenas dentro do core time.

“No meeting day” (1x por semana sem reuniões).

Direito a 1h de foco ininterrupto/dia (Deep Work Block).

💬 Comunicação

Usar Slack/Teams com regras:

Dentro do core time → responder rápido.

Fora do core time → comunicação assíncrona, sem obrigação imediata.

Definir canal único por tema (ex.: bugs, deploys, produto).

Sempre deixar decisões registradas no Jira/Confluence, para evitar perda de contexto.

🔍 Qualidade & Entregas

PRs revisados por pelo menos 1 dev + QA quando aplicável.

Tempo máximo para revisão de PR → até 24h úteis.

Definição de “Definition of Ready” e “Definition of Done” claros.

Automatização mínima para garantir qualidade (CI rodando testes antes do merge).

🎯 Engajamento & Performance

Daily curta (15 min) → foco em bloqueios e alinhamentos, não status report.

Checkpoint semanal rápido para falar de moral do time e engajamento (“retro light”).

Métrica coletiva > métrica individual (time ganha junto, erra junto).

Acordo de feedback contínuo (não só na retro).

📚 Aprendizado & Crescimento

1h por sprint dedicada a Tech Talks internas (qualquer dev apresenta algo).

Rotação de papéis nas cerimônias (um sprint o dev X facilita a daily, no outro é o dev Y).

Pares diferentes em pair programming para aumentar conhecimento cruzado.

🚀 Entregas & Deploys

Deploys só em horário definido (ex.: manhãs), para evitar incêndios no fim do dia.

Post-mortem leve para qualquer incidente crítico → sem caça às bruxas, só aprendizado.

Documentar aprendizados relevantes no Confluence.

⚖️ Bem-estar & Cultura

Respeito ao horário fora do core (sem pressão em msg fora do horário).

Acordo de “câmera ligada em reuniões importantes” (planning, retro, review).

Reconhecimento público (cada sprint alguém destaca algo positivo de outro membro).

Pequenos rituais de engajamento (ex.: “meme da sprint” no Slack, ou “ice breaker” na retro).

👉 A ideia não é adotar todos de uma vez, mas levar a lista para o time e deixar que eles escolham juntos os mais relevantes. Isso cria senso de pertencimento e aumenta muito o engajamento.

Quer que eu monte isso em formato visual de mural/working agreement (ex.: quadro dividido em blocos com ícones e bullets), pronto para você colar no Confluence ou mostrar no próximo planning?



import okhttp3.Interceptor;
import okhttp3.Response;
import okhttp3.HttpUrl;

import org.springframework.stereotype.Component;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;

import jakarta.annotation.PreDestroy;

/**
 * Interceptor que aplica fair-share de concorrência por host.
 * - Se 1 host estiver ativo: ele pode usar até GLOBAL_MAX.
 * - Se N hosts estiverem ativos: cada um recebe roughly GLOBAL_MAX / N.
 *
 * Observações:
 * - Usamos Semaphore com crescimento (release) ao subir o limite.
 *   Ao reduzir, deixamos "convergir" naturalmente (sem reducePermits)
 *   para evitar edge cases; o novo limite passa a valer nas próximas aquisições.
 */
@Component
public class FairShareInterceptor implements Interceptor {

    // ---------- Parâmetros ajustáveis ----------
    private static final int GLOBAL_MAX = 1000;           // capacidade total por instância
    private static final int MIN_PER_HOST = 50;           // piso por host para não "matar" parceiros frios
    private static final int MAX_PER_HOST = 1000;         // teto por host (igual ao GLOBAL_MAX, por default)
    private static final int REBALANCE_INTERVAL_SEC = 1;  // intervalo de rebalanceamento
    private static final int ACTIVE_WINDOW_SEC = 5;       // janela para considerar um host "ativo"
    // -------------------------------------------

    private final ConcurrentHashMap<String, HostLimiter> limiters = new ConcurrentHashMap<>();
    private final ScheduledExecutorService scheduler =
            Executors.newSingleThreadScheduledExecutor(r -> {
                Thread t = new Thread(r, "fairshare-rebalance");
                t.setDaemon(true);
                return t;
            });

    public FairShareInterceptor() {
        scheduler.scheduleAtFixedRate(this::rebalance, 0, REBALANCE_INTERVAL_SEC, TimeUnit.SECONDS);
        scheduler.scheduleAtFixedRate(this::cleanupStale, 30, 30, TimeUnit.SECONDS); // limpeza periódica
    }

    @Override
    public Response intercept(Chain chain) throws IOException {
        HttpUrl url = chain.request().url();
        String host = url.host();

        HostLimiter limiter = limiters.computeIfAbsent(host, h -> new HostLimiter());
        limiter.markActive();

        limiter.acquire();
        try {
            return chain.proceed(chain.request());
        } finally {
            limiter.release();
        }
    }

    private void rebalance() {
        long now = System.nanoTime();
        long activeWindowNanos = TimeUnit.SECONDS.toNanos(ACTIVE_WINDOW_SEC);

        List<Map.Entry<String, HostLimiter>> ativos = new ArrayList<>();
        for (Map.Entry<String, HostLimiter> e : limiters.entrySet()) {
            HostLimiter hl = e.getValue();
            if (hl.recentlyActive(now, activeWindowNanos)) {
                ativos.add(e);
            }
        }

        int n = Math.max(ativos.size(), 1);
        int fair = Math.max(MIN_PER_HOST, Math.min(MAX_PER_HOST, GLOBAL_MAX / n));

        for (Map.Entry<String, HostLimiter> e : ativos) {
            e.getValue().setLimit(fair);
        }
    }

    private void cleanupStale() {
        long now = System.nanoTime();
        long staleWindowNanos = TimeUnit.SECONDS.toNanos(ACTIVE_WINDOW_SEC * 6L); // 6x a janela ativa
        for (Map.Entry<String, HostLimiter> e : limiters.entrySet()) {
            if (!e.getValue().recentlyActive(now, staleWindowNanos)) {
                // remover limiters que ficaram muito tempo sem tráfego
                limiters.remove(e.getKey(), e.getValue());
            }
        }
    }

    @PreDestroy
    public void shutdown() {
        scheduler.shutdownNow();
    }

    // ----------------- Helper -----------------
    static class HostLimiter {
        private final Semaphore sem = new Semaphore(0, true);
        private final AtomicInteger limit = new AtomicInteger(0);
        private volatile long lastTouch = System.nanoTime();

        void markActive() {
            lastTouch = System.nanoTime();
        }

        /**
         * "Ativo" se tocado dentro da janela.
         */
        public boolean recentlyActive(long now, long windowNanos) {
            return (now - lastTouch) < windowNanos;
        }

        /**
         * Ajusta o limite de concorrência.
         * Aumentos liberam novos permits; reduções não “tomam de volta” permits já disponíveis,
         * mas o novo limite passa a valer conforme os permits forem sendo consumidos.
         */
        public void setLimit(int newLimit) {
            int old = limit.getAndSet(newLimit);
            int delta = newLimit - old;
            if (delta > 0) {
                sem.release(delta);
            }
            // Se delta < 0, deixamos convergir naturalmente.
        }

        public void acquire() {
            try {
                sem.acquire();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                // Se interrompido, negue a passagem para não "furar" o limiter.
                throw new RuntimeException("Interrupted while acquiring fair-share permit", e);
            }
        }

        public void release() {
            sem.release();
        }
    }
}
