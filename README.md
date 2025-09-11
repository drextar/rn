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
