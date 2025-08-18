# 📢 Release Notes

*Deployed: • Status: **Em produção***

---

## 🚀 Destaques

|                                   | Entrega            | Valor Gerado                                                                |
| --------------------------------- | ------------------ | --------------------------------------------------------------------------- |
| ❌ **Cache de cancelamento**       | **OrderStatus-MS** | Menos chamadas repetidas no fluxo de cancelamento • resposta mais rápida    |
| 🛍️ **Cadastro EQ3**              | **Seller-MS**      | Fluxo definido para onboarding conforme padrão EQ3                          |
| ⚖️ **RN frete > R\$1.000**        | **Simulation-MS**  | Tratamento/validação para cenários de frete atípico                         |
| 💸 **RN variação de preço ±60%**  | **Produto-MS**     | Guardrails contra preços fora do esperado (maior/menor que 60% do original) |
| 📑 **Template de logs unificado** | **Cross-services** | Observabilidade padronizada • troubleshooting mais simples                  |
| 📊 **Dashboards Datadog Itaú**    | **Cross-services** | Painéis migrados e padronizados por domínio                                 |

---

## 📦 Mudanças por Microserviço

### **OrderStatus-MS**

* 🗃️ **Cache para fluxo de cancelamento de pedido**

  * Evita reprocessamento de cancelamentos em curto intervalo.
  * Respostas idempotentes para requisições repetidas.

### **Seller-MS**

* 🧭 **Definição do fluxo de Cadastro EQ3**

  * Regras, contratos e estados mapeados para o onboarding de sellers no padrão EQ3.

### **Simulation-MS**

* 📏 **Tratamento de RN quando frete retornado pelo Seller > R\$ 1.000,00**

  * Sinalização/validação de outliers e encaminhamento conforme regra de negócio.

### **Produto-MS**

* 🧮 **Tratamento de RN para variação de preço**

  * Bloqueio quando preço enviado pelo Seller é **> 60%** ou **< 60%** do preço original.
  * Gatilhos de auditoria e alertas operacionais associados.

---

## 🔄 Melhorias Cross-Service

* 🧰 **Novo template de logs**

  * **JSON estruturado** com `traceId`, `correlationId`, `service`, `endpoint`, `latencyMs`, `statusCode`.
  * **Mascaramento de PII** e classificação de níveis (`INFO` core, `DEBUG` fluxo).
  * Padrão Definido no OrderStatus-MS e será adotado em todos os MS.
* 📈 **Migração de dashboards — Datadog Itaú**

  * Painéis por domínio (Pedido, Seller, Produto, Simulation) com latência, taxa de erro, throughput e saturação.

---

## ✅ Checklist Pós-Deploy

* [x] Smoke tests (Prod · Homol · Dev)
* [x] Dashboards Datadog publicados e vinculados aos serviços
* [x] Logs padronizados ativos (amostragem e mascaramento validados)
* [x] Regras de negócio (frete/price) monitoradas com métricas e alertas

---

💬 **Dúvidas ou incidentes?** Abra uma *issue* ou mencione **@infra-eng** no Slack.
