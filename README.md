# 📢 Release Notes — Sprint 23

*Deployed: **06-06-2025** • Status: **Em produção***

> Repositório: `/infra/microservices`

---

## 🚀 Destaques

|                                  | Descrição                                                                | Benefício                           |
| -------------------------------- | ------------------------------------------------------------------------ | ----------------------------------- |
| ⚙️ **Escalabilidade horizontal** | +1 instância em **MS-NotificaPedido**, **MS-Seller** e **MS-Simulation** | +100 % de capacidade de atendimento |
| 🛠️ **Refatoração de logs**      | Fluxo → `DEBUG` • Core → `INFO` • Dados sensíveis mascarados             | –40 % de volume de log              |
| 🚀 **Migração HTTP**             | OpenFeign + Apache → **Feign + OkHttp**                                  | Latência –25 % • Throughput +30 %   |
| 🛒 **Cupom em pedidos**          | Novo contrato em **MS-Pedido**                                           | Suporte a campanhas de desconto     |

---

## 📦 Mudanças por Microserviço

### MS-NotificaPedido

* 🔄 **2 instâncias**
* 🔒 Logs com máscara de dados sensíveis
* 🧩 Cast dinâmico para `OrderPlacement`
* 🚀 Upgrade **Feign + OkHttp**

### MS-Seller

* 🔄 **2 instâncias**
* 🔗 Contrato inalterado (back-compatible)

### MS-Produto

* 🛠️ `@ControllerAdvice` centralizado
* ✨ Logs restritos à camada de handler
* 🔌 **API interna para confirmação de produto** ← *novo*

### MS-Pedido

* 📃 Suporte a **cupom** no payload
* ✔️ Testes E2E aprovados

### MS-Simulation

* 🔄 **2 instâncias**
* 🏋️ Pré-teste de carga sustentou **40 req/s**
* 🚀 Upgrade **Feign + OkHttp**

### MS-Suggestion

* 📑 Documentação OpenAPI atualizada
* 🔤 Ajustes de nomenclatura

---

## 🔄 Melhorias Cross-Service

* **Reclassificação de logs:**

  * Fluxo ⇒ `DEBUG`
  * Core ⇒ `INFO`
  * **Resultado:** dashboards mais limpos e –40 % de armazenamento

---

## 🔧 Upgrade HTTP — Feign + OkHttp

| Métrica         | Antes (Apache) | Depois (OkHttp)                                 | Variação |
| --------------- | -------------- | ----------------------------------------------- | -------- |
| Latência média  | 230 ms         | **172 ms**                                      | –25 %    |
| Throughput máx. | 31 req/s       | **40 req/s**                                    | +30 %    |
| Timeouts        | Global 1 s     | `connect = 500 ms`, `read = 1 s`, `write = 1 s` | —        |

**Ganhos técnicos**

1. Pool de conexões com reuse ≥ 98 %
2. Dispatcher assíncrono → menos threads bloqueadas
3. Timeouts granulares evitam *thread starvation* em parceiros lentos

---

## ✅ Checklist Pós-Deploy

* [x] Smoke tests concluídos
* [x] Dashboards Kibana atualizados
* [x] Alertas de latência recalibrados
* [x] Documentação publicada na Confluence

---

💬 Dúvidas ou incidentes? Abra uma *issue* ou mencione `@infra-eng` no Slack.
