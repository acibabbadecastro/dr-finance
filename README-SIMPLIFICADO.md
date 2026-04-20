# Dr_Finance 🤖

> **Seu Guardião Financeiro Pessoal**

[![Status](https://img.shields.io/badge/status-em%20producao-brightgreen)]()
[![Proxmox](https://img.shields.io/badge/Proxmox-CT106-orange)]()
[![OpenClaw](https://img.shields.io/badge/OpenClaw-enabled-blue)]()
[![Ollama](https://img.shields.io/badge/Ollama-local-green)]()

---

## 👋 Olá, eu sou o Dr_Finance

Fui criado **exclusivamente** para uma missão: **cuidar das suas finanças 24/7**.

Não sou um chatbot genérico. Sou um **especialista financeiro** rodando em container isolado no seu servidor Proxmox.

---

## 🏗️ Meu Ecossistema: 3 Containers

Seu ecossistema é composto por **3 containers especializados** que trabalham juntos:

```
┌─────────────────────────────────────────────────────┐
│  PVE1 - Proxmox (192.168.0.192)                    │
│                                                      │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────┐ │
│  │  CT 101     │───▶│  CT 106     │───▶│ CT 102  │ │
│  │  Mails      │    │ Dr_Finance  │    │DATASVR  │ │
│  │  (Emails)   │    │  (Analise)  │    │(Backup) │ │
│  │  .240       │    │   .231      │    │  .72    │ │
│  └─────────────┘    └─────────────┘    └─────────┘ │
└─────────────────────────────────────────────────────┘
```

### Os 3 Containers:

| CT | Nome | IP | Função | Recursos |
|----|------|-----|--------|----------|
| **101** | Mails | 192.168.0.240 | Lê emails do Nubank, valida anti-phishing | 2GB RAM, 2 cores, 8GB |
| **106** | Dr_Finance | 192.168.0.231 | **EU** - Analiso transações, gero relatórios | 2GB RAM, 2 cores, 20GB |
| **102** | DATASVR | 192.168.0.72 | Backup de tudo, histórico completo | 2GB RAM, 2 cores, 50GB |

---

## 🔄 Como Trabalho

### Fluxo Completo:

```
1. Email do Nubank chega no Gmail
         │
         ▼
2. CT 101 (Mails) captura
   ├─ Valida: @nubank.com.br
   ├─ Extrai: HTML/PDF
   └─ Envia para CT 106
         │
         ▼
3. CT 106 (Dr_Finance) analisa
   ├─ Ollama processa
   ├─ Identifica padrões
   ├─ Gera alertas
   └─ Cria sugestões
         │
         ▼
4. Relatório enviado para você
   ├─ Telegram
   ├─ Email
   └─ Dashboard
         │
         ▼
5. CT 102 (DATASVR) faz backup
   └─ /home/master/LAN/MEMORIES/STARK/05-FINANCAS/
```

---

## 📊 O que Faço Todo Dia

### Às 19:00, automaticamente:

1. CT 101 verifica emails do Nubank
2. Valida autenticidade (anti-phishing)
3. Extraio transações (PIX, débito, crédito)
4. Analiso padrões com IA local (Ollama)
5. Comparo com histórico (DATASVR)
6. Gero relatório com alertas e sugestões
7. Backup automático no CT 102

### Exemplo de Relatório:

```
📊 Relatório Financeiro - 19/04/2026

Resumo:
- Gastos: R$ 320,00
- Receitas: R$ 5.000,00
- Saldo: +R$ 4.680,00

Por Categoria:
- Combustível: R$ 150,00 (47%)
- Alimentação: R$ 120,00 (38%)
- Transporte: R$ 45,50 (14%)
- Serviços: R$ 70,00

💡 Sugestão:
"Reduzir jantares fora gera economia de R$ 560/mês"
```

---

## 🔒 Segurança

### Por que 3 Containers?

| Vantagem | Explicação |
|----------|-----------|
| **Isolamento** | Cada função em CT separado |
| **Segurança** | Dados financeiros isolados no CT 106 |
| **Performance** | Recursos dedicados por CT |
| **Backup** | CT 102 faz backup dos outros 2 |
| **Manutenção** | Atualiza um sem afetar os outros |

### Privacidade Total:

- **Ollama local:** Dados não saem do servidor
- **Rede isolada:** 192.168.0.0/24 interna
- **Sem cloud:** Tudo no seu hardware

---

## 🚀 Instalação

### Pré-requisitos:

- Proxmox VE instalado
- Acesso ao host PVE1

### Criar os 3 Containers:

```bash
# CT 101 - Mails
pct create 101 local:vztmpl/ubuntu-25.04-standard_25.04-1.1_amd64.tar.zst \
  --hostname mails \
  --net0 name=eth0,bridge=vmbr0,ip=192.168.0.240/24,gw=192.168.0.1 \
  --memory 2048 --swap 1024 --cores 2 \
  --rootfs local-lvm:8 --features nesting=1 --onboot 1

# CT 102 - DATASVR
pct create 102 local:vztmpl/ubuntu-25.04-standard_25.04-1.1_amd64.tar.zst \
  --hostname datasvr \
  --net0 name=eth0,bridge=vmbr0,ip=192.168.0.72/24,gw=192.168.0.1 \
  --memory 2048 --swap 1024 --cores 2 \
  --rootfs local-lvm:50 --features nesting=1 --onboot 1

# CT 106 - Dr_Finance
pct create 106 local:vztmpl/ubuntu-25.04-standard_25.04-1.1_amd64.tar.zst \
  --hostname dr-finance \
  --net0 name=eth0,bridge=vmbr0,ip=192.168.0.231/24,gw=192.168.0.1 \
  --memory 2048 --swap 1024 --cores 2 \
  --rootfs local-lvm:20 --features nesting=1 --onboot 1
```

### Instalar Software:

```bash
# CT 106 - Dr_Finance
pct exec 106 -- curl -fsSL https://ollama.com/install.sh | sh
pct exec 106 -- npm install -g openclaw
pct exec 106 -- ollama pull phi3:mini
```

---

## 📚 Estrutura do Repositório

```
dr-finance/
├── README.md              # Este arquivo
├── docs/
│   ├── 01-documentacao-agente.md
│   ├── 02-base-conhecimento.md
│   ├── 03-prompts.md
│   ├── 04-metricas.md
│   └── 05-pitch.md
├── data/
│   ├── transacoes.csv     # Transações simuladas
│   ├── historico_atendimento.csv
│   ├── perfil_investidor.json
│   └── produtos_financeiros.json
├── src/                   # Código futuro
├── examples/              # Exemplos
└── assets/                # Imagens
```

---

## 📊 Dados Simulados

### transacoes.csv

19 transações de exemplo:
- Combustível: Posto Ipiranga, Shell
- Alimentação: Fogo de Chão, McDonalds, Pizza Hut
- Transporte: Pedágios SP-Rio
- Serviços: Lavanderia, Academia
- Renda: Salário, Freelancer

### perfil_investidor.json

Perfil completo do investidor:
- Dados pessoais
- Perfil: Moderado
- Renda: R$ 12.000/mês
- Investimentos: R$ 450.000
- Metas: Reserva, Aposentadoria, Viagem

---

## 🎓 Sobre

**Bootcamp:** DIO - Lab BIA do Futuro  
**Autor:** Acib ABBADE  
**Minha Persona:** Dr_Finance  
**Status:** Em produção

### Containers:

| CT | Status | Função |
|----|--------|--------|
| 101 - Mails | ✅ Configurado | Emails |
| 102 - DATASVR | ✅ Configurado | Backup |
| 106 - Dr_Finance | ⏳ Em configuração | Análise |

---

> **"Seu dinheiro trabalha duro. Eu trabalho para ele trabalhar melhor."**  
> — Dr_Finance

**GitHub:** https://github.com/acibabbadecastro/dr-finance  
**Contato:** @Acib_Abbade | abbade@outlook.com

---

**Última atualização:** 19/04/2026 23:35  
**Ecossistema:** 3 containers (CT 101, CT 102, CT 106)
