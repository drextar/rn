# 📢 Release Notes

*Deployed: **2025-07-17** • Status: **Em produção***

---

## 🚀 Destaques

|                                        | Descrição                                                          | Benefício                                                 |
| -------------------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------- |
| 🛍️ **Rota de provisionamento Seller** | **Seller-MS**                                                      | Pré-requisito para DPCP • onboarding automático via Cadastro Seller 2.0           |
| ❌ **Cancelamento parcial**             | **OrderStatus-MS** (definitivo) · **Pedido-MS** (versão paliativa) | Envio de Pedido para time de atendimento via email         |
| ⚙️ **Cache & Timeout Handling**        | **Simulacao-MS**                                                   | GET /baseurl → menor latência • cálculo de frete mais resiliente |
| 📚 **ReadOnly Scope**                  | **Suggestion-MS**                                                  | Suporte ao time de Catálogo (consultas sem lock)          |
| 💾 **Elasticache cluster**             | Todos os MS                                                        | Hit rate 92 % • CPU –15 %                                 |
| 💰 **FinOps**                          | Logs · Clusters · Storage                                          | –10 % custos mensais nesses pilares                                     |
| 📊 **Dashboards operacionais**         | Integrações & Tempos de frete                                      | Visão seller-a-seller • alertas em tempo real             |
| ✅ **IUConfia Compliance**              | 100 % aderente às boas práticas                                    | Auditoria green-light                                     |

---

## 📦 Mudanças por Microserviço

### Seller-MS

* 🛍️ **/provision** — rota de provisionamento de seller (DPCP ready)
* 🔐 Chaves Vault já entram no ciclo de rotação 24 h

### OrderStatus-MS

* ❌ **/cancel** — cancelamento **parcial**

### Pedido-MS

* ❌ **/cancel** — cancelamento **parcial (paliativo)**

  * A solicitação de cancelamento dispara um e-mail para o time de Atendimento com as informações do pedido

### Simulacao-MS

* ⚡ **Elasticache** para GET /baseUrl — hit rate 92 %
* ⏱️ Mapeia `HttpTimeoutException` do Seller → `422 UNPROCESSABLE_ENTITY` consistente

### Suggestion-MS

* 📚 **ReadOnly scope** ativado

---

## 🔄 Melhorias Cross-Service

* **Cluster Elasticache (Redis 7)** — cache compartilhado - disponível para integração de todos microserviços.
* **FinOps contínuo**

  * Filtragem de logs DEBUG/TRACE fora de produção.
  * Rightsizing de tarefas Fargate ARM64.
  * Limpeza automática de volumes efêmeros.
* **Dashboards Datadog**

  * *Integrações* (latência & erros por endpoint).
  * *Tempos de frete* com drill-down seller.
* **IUConfia** — todas as recomendações atendidas (auth, logging, secrets, métricas).

---
