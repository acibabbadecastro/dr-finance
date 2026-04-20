# Dr_Finance - Agente de Controle de Gastos Pessoais

> **Projeto desenvolvido por Acib ABBADE**  
> Bootcamp DIO - Lab BIA do Futuro

[![Status](https://img.shields.io/badge/status-em%20producao-brightgreen)]()
[![Proxmox](https://img.shields.io/badge/Proxmox-CT106-orange)]()
[![OpenClaw](https://img.shields.io/badge/OpenClaw-enabled-blue)]()
[![Ollama](https://img.shields.io/badge/Ollama-local-green)]()

---

## 👋 Apresentação

Olá, sou **Acib ABBADE** e desenvolvi o **Dr_Finance** como projeto do Bootcamp DIO - Lab BIA do Futuro.

Escolhi criar um **agente de controle de gastos pessoais** porque identifiquei que 90% das pessoas não têm controle real dos seus gastos. Recebemos extratos do banco todos os dias, mas não analisamos padrões.

Minha solução automatiza essa análise usando IA local, garantindo privacidade total dos dados.

---

## 🏗️ Minha Arquitetura: 3 Containers Proxmox

Decidi usar **3 containers no Proxmox** para isolar cada função do sistema:

### **Diagrama 1: Arquitetura do Sistema**

```
┌─────────────────────────────────────────────────────────────┐
│  PVE1 - Proxmox Virtual Environment                        │
│  IP: 192.168.0.192                                         │
│  Hardware: Ryzen 5 5600X, 30GB RAM, SSD 223GB             │
│                                                             │
│  ┌───────────────────────────────────────────────────┐    │
│  │  REDE INTERNA: 192.168.0.0/24 (Bridge vmbr0)     │    │
│  │                                                    │    │
│  │   ┌──────────┐      ┌──────────┐      ┌────────┐ │    │
│  │   │ CT 101   │─────▶│ CT 106   │─────▶│ CT 102 │ │    │
│  │   │ Mails    │      │ Dr_      │      │DATASVR │ │    │
│  │   │ .240     │      │ Finance  │      │ .72    │ │    │
│  │   │ (Emails) │      │ (.231)   │      │(Backup)│ │    │
│  │   └──────────┘      └──────────┘      └────────┘ │    │
│  │                                                    │    │
│  └───────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### **Diagrama 2: Fluxo de Dados**

```
┌─────────────┐
│  Gmail      │  1. Email do Nubank chega
│  (Internet) │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  CT 101     │  2. Capturo e valido email
│  Mails      │  3. Filtra @nubank.com.br
│  .240       │  4. Extraio dados (HTML/PDF)
└──────┬──────┘
       │ (envia dados)
       ▼
┌─────────────┐
│  CT 106     │  5. Ollama analisa transações
│  Dr_Finance │  6. Identifico padrões
│  .231       │  7. Gero relatório + sugestões
└──────┬──────┘
       │ (envia relatório)
       ▼
┌─────────────┐
│  CT 102     │  8. Backup automático
│  DATASVR    │  9. Armazeno histórico
│  .72        │ 10. Disponível para consulta
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  Você       │  Relatório no Telegram/Email
│  (Telegram) │
└─────────────┘
```

---

## 📋 Os 3 Containers que Criei

### **Diagrama 3: Detalhe de Cada Container**

```
┌──────────────────────────────────────────────────────────────┐
│  CT 101 - MAILS (192.168.0.240)                             │
│  Função: Coletor de Emails                                  │
│  Recursos: 2GB RAM, 2 cores, 8GB disco                      │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  • Lê Gmail automaticamente                          │  │\n│  │  • Filtra @nubank.com.br                             │  │\n│  │  • Valida anti-phishing                              │  │\n│  │  • Extrai HTML/PDF                                   │  │\n│  │  • Envia para CT 106                                 │  │
│  └──────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────────────┐
│  CT 106 - Dr_Finance (192.168.0.231)                        │
│  Função: Análise Financeira com IA                          │
│  Recursos: 2GB RAM, 2 cores, 20GB disco                     │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  • OpenClaw (orquestrador)                           │  │\n│  │  • Ollama 0.21.0 (IA local)                          │  │\n│  │  • Node.js 20.x                                      │  │\n│  │  • Analisa transações                                │  │\n│  │  • Identifica padrões                                │  │\n│  │  • Gera relatórios (19:00 automático)                │  │\n│  │  • Sugere economia                                   │  │
│  └──────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────────────┐
│  CT 102 - DATASVR (192.168.0.72)                            │
│  Função: Armazenamento e Backup                             │
│  Recursos: 2GB RAM, 2 cores, 50GB disco                     │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  • Samba (\\192.168.0.72\LAN\)                       │  │\n│  │  • Backup automático dos CTs 101 e 106               │  │\n│  │  • Histórico financeiro (anos)                       │  │\n│  │  • /home/master/LAN/MEMORIES/STARK/05-FINANCAS/      │  │\n│  │  • Logs de auditoria                                 │  │
│  └──────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

### Resumo dos 3 CTs:

| CT | Nome | IP | Função | Recursos |
|----|------|-----|--------|----------|
| **101** | Mails | 192.168.0.240 | Coleta emails do Nubank | 2GB RAM, 2 cores, 8GB |
| **102** | DATASVR | 192.168.0.72 | Backup e histórico | 2GB RAM, 2 cores, 50GB |
| **106** | Dr_Finance | 192.168.0.231 | Análise com IA | 2GB RAM, 2 cores, 20GB |

---

## 🛠️ Tecnologias que Usei

### **Infraestrutura:**
- **Proxmox VE** - Hipervisor para containers
- **LXC** - Containers Linux (mais leves que VMs)
- **Rede 192.168.0.0/24** - Rede interna isolada

### **Software:**
- **Ubuntu 25.04** - Sistema operacional dos CTs
- **OpenClaw** - Orquestrador de agentes
- **Ollama 0.21.0** - IA local (dados não saem do servidor)
- **Node.js 20.x** - Runtime JavaScript

### **Dados Mockados:**
- **19 transações** simuladas (gasolina, pedágio, almoço, lavanderia)
- **Perfil de investidor** completo
- **6 produtos financeiros** cadastrados

---

## 📊 Como Funciona na Prática

### **Rotina Diária (19:00):**

1. **CT 101 (Mails)** verifica emails do Nubank
2. Valida autenticidade (@nubank.com.br)
3. **CT 106 (Dr_Finance)** recebe dados
4. Ollama analisa padrões de gastos
5. Comparo com histórico (CT 102 - DATASVR)
6. Gero relatório com:
   - Resumo do dia
   - Gastos por categoria
   - Alertas de excesso
   - Sugestões de economia
7. Envio para Telegram/Email
8. Faço backup no CT 102

### **Exemplo de Relatório que Gero:**

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

💡 Minha Sugestão:
"Reduzir jantares fora gera economia de R$ 560/mês"
```

---

## 🔒 Por que Escolhi Essa Arquitetura

### **Privacidade Total:**
- Ollama local: dados não saem do servidor
- Rede isolada: 192.168.0.0/24 interna
- Sem cloud: tudo no hardware local

### **Segurança:**
- Container isolado (CT 106) para dados financeiros
- Validação anti-phishing
- Backup automático no CT 102

### **Custo Zero:**
- Hardware próprio (sem mensalidades)
- Software open-source
- Sem APIs pagas

---

## 📁 Estrutura do Repositório

```
dr-finance/
├── README.md              # Este arquivo (explicação do projeto)
├── PITCH-3MINUTOS.md      # Roteiro da apresentação
├── docs/                  # Documentação técnica
│   ├── 01-documentacao-agente.md
│   ├── 02-base-conhecimento.md
│   ├── 03-prompts.md
│   ├── 04-metricas.md
│   └── 05-pitch.md
├── data/                  # Dados mockados
│   ├── transacoes.csv           (19 transações)
│   ├── historico_atendimento.csv (14 atendimentos)
│   ├── perfil_investidor.json   (Perfil completo)
│   └── produtos_financeiros.json (6 produtos)
├── src/                   # Código futuro
├── examples/              # Exemplos
└── assets/                # Imagens
```

---

## 🚀 Como Instalar (se quiser replicar)

### **Pré-requisitos:**
- Proxmox VE instalado
- Acesso ao host PVE1

### **Comandos que Usei:**

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

# Instalar software no CT 106
pct exec 106 -- curl -fsSL https://ollama.com/install.sh | sh
pct exec 106 -- npm install -g openclaw
pct exec 106 -- ollama pull phi3:mini
```

---

## 📊 Dados Mockados que Criei

### **transacoes.csv:**
- 19 transações de exemplo
- Categorias: Combustível, Alimentação, Transporte, Serviços, Renda
- Valores reais de mercado

### **perfil_investidor.json:**
- Perfil: Moderado
- Renda: R$ 12.000/mês
- Investimentos: R$ 450.000
- Metas: Reserva, Aposentadoria, Viagem

---

## 🎓 Sobre o Projeto

**Bootcamp:** DIO - Lab BIA do Futuro  
**Autor:** Acib ABBADE  
**Problema Escolhido:** Controle de gastos pessoais  
**Solução:** Agente que analisa emails do Nubank e sugere economia

### **Status:**
- ✅ Documentação completa
- ✅ CT 106 criado no Proxmox
- ✅ Dados mockados cadastrados
- ✅ 3 diagramas de arquitetura
- ✅ Pitch de 3 minutos

### **Próximos Passos:**
- ⏳ Finalizar instalação do OpenClaw no CT 106
- ⏳ Configurar cron jobs (19:00 diário)
- ⏳ Testar integração completa

---

## 📞 Contato

**Acib ABBADE**  
Telegram: [@Acib_Abbade](https://t.me/Acib_Abbade)  
Email: abbade@outlook.com  
GitHub: [acibabbadecastro](https://github.com/acibabbadecastro)

---

> **"Desenvolvi este projeto para automatizar o controle de gastos pessoais, garantindo privacidade total dos dados usando IA local."**  
> — Acib ABBADE

**Última atualização:** 19/04/2026 23:52  
**Ecossistema:** 3 containers (CT 101-Mails, CT 102-DATASVR, CT 106-Dr_Finance)
