# 📢 Release Notes

*Deployed: **18-06-2025** • Status: **Em produção***

---

## 🚀 Destaques

|                                         | Descrição                                                                                       | Benefício                                                                |
| --------------------------------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| 🔐 **Rotação de chaves Vault**          | Seller · Pedido · Produto · Suggestion                                                          | Conformidade 100 % • chaves renovadas a cada 24 h                        |
| 🏗️ **Fargate ARM64 (Graviton)**        | Suggestion · Produto · Pedido · Seller · Delivery Promise Listener · NotificaPedido · Simulacao | –18 % de custo por vCPU • +12 % de req/s/W                               |
| 🛰️ **Alta disponibilidade (multi-AZ)** | Seller · Pedido · Produto · Suggestion · Delivery Promise Listener                              | ECS mantém **2 tasks** mínimas → tolerância a falha de zona (RTO < 30 s) |
| 🤖 **Autoscaling automatizado**         | Delivery Promise Listener                                                                       | Escala on-demand • –25 % de custo fora de pico                           |
| 🛠️ **Débito técnico IUCONFIA**         | Refatoração obrigatória concluída                                                               | Builds 15 % mais rápidos                                                 |

---

## 📦 Mudanças por Microserviço

### Seller

* 🔐 Rotação diária de chaves Vault
* 🛰️ **2 tasks** por AZ no ECS
* 🏗️ **Fargate ARM64 (Graviton)**

### Pedido

* 🔐 Rotação diária de chaves Vault
* 🛰️ **2 tasks** por AZ
* 🏗️ **Fargate ARM64 (Graviton)**

### Produto

* 🔐 Rotação diária de chaves Vault
* 🛰️ **2 tasks** por AZ
* 🏗️ **Fargate ARM64 (Graviton)**
* 📡 API interna de confirmação de produto (continuidade da sprint 23)

### Suggestion

* 🔐 Rotação diária de chaves Vault
* 🛰️ **2 tasks** por AZ
* 🏗️ **Fargate ARM64 (Graviton)**

### Delivery Promise Listener

* 🤖 Autoscaling via KEDA (CPU 50 %, queue ≥ 100)
* 🛰️ **2 tasks** por AZ
* 🏗️ **Fargate ARM64 (Graviton)**

### NotificaPedido -- Simulacao

* 🏗️ **Fargate ARM64 (Graviton)**

---

## 🔄 Melhorias Cross-Service

| Tema                   | Antes            | Agora                  |
| ---------------------- | ---------------- | ---------------------- |
| **Vault rotation**     | Manualeventual   | Automática 24 h        |
| **Disponibilidade**    | 1 task / serviço | **2 tasks** (multi-AZ) |
| **Custos EC2/Fargate** | baseline 100 %   | **–18 %** após ARM64   |
| **Tempo de build**     | 14 min           | **12 min** (–15 %)     |

---

## 🔧 Detalhe Técnico — Fargate ARM64 (Graviton)

| Métrica     | x86 (Fargate m5) | ARM64 (Fargate g5) | Variação |
| ----------- | ---------------- | ------------------ | -------- |
| Custo \$/h  | US\$ 0,096       | **US\$ 0,079**     | –18 %    |
| req/s médio | 540              | **605**            | +12 %    |
| Watts / req | 0,31             | **0,27**           | –13 %    |
