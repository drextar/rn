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
