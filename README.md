# 📢 Release Notes

*Deployed: • Status: **Em produção***

---

## 🚀 Destaques

|                                      | Entrega                | Valor Gerado                              |
| ------------------------------------ | ---------------------- | ----------------------------------------- |
| 📈 **Autoscaling SQS**               | **Produto-MS**         | Escala por backlog • picos sem fila       |
| ⚡ **Cache de Frete**                 | **Simulacao-MS**       | 98 % hit • –90 % chamadas ao Seller       |
| ✏️ **Write Scope + Regex Hardening** | **Suggestion-MS**      | Suporte Catálogo • validações reforçadas  |
| 🔀 **Proxy Cross-Account**           | Bridge (Conta A → B)   | P95 integração **697 ms → 55 ms** (-92 %) |
| 💾 **Camadas de Cache**              | Gateways & Frete       | Latência menor • CPU –15 %                |
| 📊 **Datadog Itaú**                  | Migração concluída     | Métricas + Log + Trace num único lugar    |
| 💰 **FinOps Sprint**                 | ECS & Clusters Dev/Hom | Custos ECS **-78 %** (tabela abaixo)      |

---

## 📦 Mudanças por Microserviço

| MS                | Principais Itens Entregues                               |
| ----------------- | -------------------------------------------------------- |
| **Produto-MS**    | **Autoscaling SQS** — 1 task ↔ 250 msgs (min 2 / max 12) |
| **Simulacao-MS**  | Cache Redis (TTL 5 min) para retorno de frete            |
| **Suggestion-MS** | Write scope ativado • Validações Regex reforçadas        |

---

## 🔄 Melhorias Cross-Service

* **Proxy de Arquitetura** — Bridge consome Gateway externo (Conta A), ajusta payload e roteia para Gateway interno (Conta B).
* **Caches Estratégicos** —

  * Gateways interno & externo (evita authorizer)
  * Frete Seller (TTL 5 min, 98 % hit)
  * BaseURL Seller centralizada (cluster Redis compartilhado)
* **Datadog Itaú** — todos os MS reportando métricas, logs e traces no tenant Itaú
* **FinOps** —

  * Clusters Dev/Hom desligados quando *idle*
  * Rightsizing Fargate ARM64
  * Filtragem de logs DEBUG/TRACE fora de produção
* **IUConfia** — auditoria contínua, 100 % compliant

---

## 💸 FinOps – Custos ECS

| Período          | Antes         | Agora             | Economia            |
| ---------------- | ------------- | ----------------- | ------------------- |
| **Dia**          | US\$ 15,25    | **US\$ 3,31**     | –US\$ 11,94 (-78 %) |
| **Mês** (\~30 d) | US\$ 457,50   | **US\$ 99,30**    | –US\$ 358,20        |
| **Ano** (365 d)  | US\$ 5 566,25 | **US\$ 1 208,15** | –US\$ 4 358,10      |

---

## 📊 Métricas “Antes × Depois”

| Métrica                                   | Antes     | Depois    | Δ                   |
| ----------------------------------------- | --------- | --------- | ------------------- |
| **P95 integração** (Marketplace → Seller) | 697 ms    | **55 ms** | **-92 %**           |
| **TPS Simulacao-MS**                      | 140       | **494**   | **+253 %**          |
| **Chamadas Seller evitadas** (frete)      | —         | **-90 %** | via cache TTL 5 min |
| **Autorizador Gateway**                   | 100 % req | **-85 %** | cache autenticação  |

---

## 📋 Conclusão de Homologação de Sellers

* [x] **LL**

🚀 Destaques de Performance e Eficiência
1️⃣ Integração Marketplace → Seller
📉 Tempo de resposta 92% mais rápido
De 697 ms para 55 ms → experiências mais fluidas para parceiros e clientes.

2️⃣ Simulador de Preços e Frete
⚡ +253% de capacidade de processamento
De 140 para 494 TPS → suporte a picos de acesso sem degradação.

3️⃣ Redução de Carga no Seller
🛑 90% menos chamadas desnecessárias
Uso inteligente de cache (TTL 5 min) → menor custo e menor risco de gargalo.

4️⃣ Autorizador de API
🔓 85% menos requisições para autenticação
Cache de credenciais → ganho de velocidade e economia de infraestrutura.

💡 Mensagem final no slide:

“Mais rápido, mais estável e mais barato — performance que gera valor para o negócio e para o cliente.”
* [x] **Bravium**
* [x] **Netshoes**
* [x] **Grand Cru**
* [x] **Lojas Sages**
