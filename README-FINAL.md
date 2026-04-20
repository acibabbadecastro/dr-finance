# Dr_Finance 🤖💰

> **Seu Guardião Financeiro Pessoal**

[![Status](https://img.shields.io/badge/status-em%20produ%C3%A7%C3%A3o-brightgreen)]()
[![Proxmox](https://img.shields.io/badge/Proxmox-CT106-orange)]()
[![OpenClaw](https://img.shields.io/badge/OpenClaw-enabled-blue)]()
[![Ollama](https://img.shields.io/badge/Ollama-local-green)]()

---

## 👋 Olá, eu sou o Dr_Finance

Fui criado **exclusivamente** para uma missão: **cuidar das suas finanças 24/7**.

Não sou um chatbot genérico. Não sou um assistente multi-propósito. Sou um **especialista financeiro** rodando em container isolado no seu servidor Proxmox, com uma única função:

> **Analisar seus gastos, identificar padrões e sugerir economia automaticamente.**

---

## 🏗️ Meu Ecossistema: Proxmox + Containers

### O que é Proxmox?

**Proxmox Virtual Environment (PVE)** é uma plataforma de virtualização completa que roda diretamente no hardware. É o "sistema operacional" do servidor onde eu vivo.

**No seu caso:**
- **Host:** PVE1 (192.168.0.192)
- **Hardware:** AMD Ryzen 5 5600X, 30GB RAM, RX 580 8GB, SSD 223GB
- **Local:** Seu servidor local (não é cloud!)

### O que são Containers (CTs)?

**Containers** são ambientes isolados que rodam dentro do Proxmox. Pense neles como "caixas" separadas onde cada programa roda sem interferir nos outros.

**Diferença entre VM e Container:**

| VM (Máquina Virtual) | Container (CT) |
|---------------------|----------------|
| Sistema operacional completo | Compartilha kernel do host |
| Mais pesado (GBs) | Leve (MBs) |
| Boot lento (minutos) | Boot rápido (segundos) |
| Mais isolamento | Isolamento suficiente |
| Overhead de performance | Performance próxima do nativo |

**Por que usamos Containers?**
- ✅ **Eficiência:** 13 CTs rodam em 30GB RAM (impossível com VMs)
- ✅ **Rapidez:** Boot em segundos
- ✅ **Economia:** Sem overhead de múltiplos kernels
- ✅ **Isolamento:** Suficiente para nossa segurança

---

## 🌐 Minha Casa: O Ecossistema de 13 Containers

### Arquitetura Completa:

```
┌──────────────────────────────────────────────────────────────┐
│  PVE1 - Proxmox Virtual Environment                         │
│  IP: 192.168.0.192                                          │
│  Hardware: Ryzen 5 5600X, 30GB RAM, RX 580 8GB, SSD 223GB  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  CONTAINERS DO ECOSISTEMA (13 CTs)                   │  │
│  │                                                       │  │
│  │  CT 100 → Stark         (192.168.0.21)   Orquestrador│  │
│  │  CT 101 → MailBot       (192.168.0.240)  Emails      │  │
│  │  CT 102 → DATASVR       (192.168.0.72)   Backup      │  │
│  │  CT 103 → BD            (192.168.0.109)  PostgreSQL  │  │
│  │  CT 104 → SGN           (192.168.0.99)   Sistema Web │  │
│  │  CT 105 → OfficeBoy     (192.168.0.224)  Auxiliar    │  │
│  │  CT 106 → Dr_Finance    (192.168.0.231)  EU! 💰      │  │
│  │  CT 107 → SAMU          (192.168.0.64)   Emergências │  │
│  │  CT 108 → Scraper       (DHCP)           Coleta      │  │
│  │  CT 109 → ADM-SERVMIL   (DHCP)           Admin       │  │
│  │  CT 110 → bd-servmil    (DHCP)           Banco       │  │
│  │  CT 111 → Fe-Servmil    (DHCP)           Frontend    │  │
│  │  CT 112 → Analista      (DHCP)           Análise     │  │
│  └──────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

### Detalhamento dos 13 Containers:

| CT  | Nome        | IP            | RAM   | CPU  | Disco | Função Principal                    |
|-----|-------------|---------------|-------|------|-------|-------------------------------------|
| 100 | Stark       | 192.168.0.21  | 4GB   | 4    | 12GB  | Orquestrador principal (OpenClaw)   |
| 101 | MailBot     | 192.168.0.240 | 2GB   | 2    | 8GB   | Processamento de emails             |
| 102 | DATASVR     | 192.168.0.72  | 2GB   | 2    | 50GB  | Armazenamento Samba + Backup        |
| 103 | BD          | 192.168.0.109 | 4GB   | 4    | 20GB  | Banco PostgreSQL                    |
| 104 | SGN         | 192.168.0.99  | 2GB   | 2    | 10GB  | Sistema Web (frontend)              |
| 105 | OfficeBoy   | 192.168.0.224 | 1GB   | 1    | 6GB   | Tarefas auxiliares                  |
| 106 | Dr_Finance  | 192.168.0.231 | 2GB   | 2    | 20GB  | **Análise Financeira (EU)**         |
| 107 | SAMU        | 192.168.0.64  | 1GB   | 1    | 6GB   | Emergências e alertas críticos      |
| 108 | Scraper     | DHCP          | 2GB   | 2    | 10GB  | Coleta de dados externos            |
| 109 | ADM-SERVMIL | DHCP          | 2GB   | 2    | 10GB  | Administração SERVMIL               |
| 110 | bd-servmil  | DHCP          | 4GB   | 4    | 20GB  | Banco de dados SERVMIL              |
| 111 | Fe-Servmil  | DHCP          | 2GB   | 2    | 10GB  | Frontend SERVMIL                    |
| 112 | Analista    | DHCP          | 4GB   | 4    | 15GB  | Análise de dados SERVMIL            |

**Total de recursos:** ~32GB RAM, 28 cores, ~200GB disco

---

## 🔌 Rede do Ecossistema

```
                    Internet
                        │
                ┌───────▼───────┐
                │   PVE1 Host   │
                │ 192.168.0.192 │
                └───────┬───────┘
                        │
            ┌───────────┼───────────┐
            │           │           │
    ┌───────▼──┐  ┌────▼────┐  ┌──▼──────┐
    │ CT 101   │  │ CT 106  │  │ CT 102  │
    │ MailBot  │─▶│ Dr_     │─▶│ DATASVR │
    │ .240     │  │ Finance │  │ .72     │
    └──────────┘  │ .231    │  └─────────┘
                  └─────────┘
                    
    Rede: 192.168.0.0/24 (Bridge vmbr0)
    Comunicação local (<1ms latency)
```

---

## 🎯 Meu Papel no Ecossistema (CT 106)

### Minha Identidade:

| Campo | Valor |
|-------|-------|
| **Container** | CT 106 |
| **Nome** | Dr_Finance (gerente-fin) |
| **IP** | 192.168.0.231 |
| **RAM** | 2GB |
| **CPU** | 2 cores |
| **Disco** | 20GB |
| **OS** | Ubuntu 25.04 |
| **Status** | ✅ Rodando |

### Meu Software:

```
┌─────────────────────────────────────┐
│  CT 106 - Dr_Finance                │
├─────────────────────────────────────┤
│  Ubuntu 25.04                       │
│  ├─ OpenClaw (orquestrador)        │
│  ├─ Ollama 0.21.0 (IA local)       │
│  ├─ Node.js 20.x                    │
│  └─ Skills Dr_Finance               │
│      ├─ analyzer.py (análise)      │
│      ├─ daily-report.py (relatórios)│
│      └─ alerts.py (alertas)         │
└─────────────────────────────────────┘
```

---

## 🔄 Como Trabalho no Ecossistema

### Fluxo Completo:

```
1. Gmail envia email do Nubank
         │
         ▼
2. CT 101 (MailBot) detecta email
         │
         ├─ Valida remetente (@nubank.com.br)
         ├─ Baixa anexos (PDF)
         ├─ Extrai dados (HTML/PDF)
         │
         ▼
3. CT 106 (EU - Dr_Finance) recebo dados
         │
         ├─ Ollama analisa transações
         ├─ Comparo com histórico
         ├─ Identifico padrões
         ├─ Gero alertas (se necessário)
         ├─ Crio sugestões de economia
         │
         ▼
4. Envio relatório para você
         │
         ├─ Telegram (alertas urgentes)
         ├─ Email (relatório completo)
         └─ Dashboard Web (consulta)
         │
         ▼
5. CT 102 (DATASVR) faz backup
         │
         └─ /home/master/LAN/MEMORIES/STARK/05-FINANCAS/
```

---

## 📊 O que eu faço todo dia

### Às 19:00, automaticamente:

```
1. Verifico emails do Nubank (via MailBot)
2. Valido autenticidade (anti-phishing)
3. Extraio transações (PIX, débito, crédito)
4. Analiso padrões com IA local (Ollama)
5. Comparo com histórico (DATASVR)
6. Gero relatório com:
   ├─ Resumo do dia
   ├─ Gastos por categoria
   ├─ Alertas de excesso
   └─ Sugestões de economia
7. Faço backup de tudo (CT 102)
8. Te envio no Telegram/Email
```

---

## 🔒 Por que sou seguro

### 1. Privacidade Total
- **Ollama local:** Seus dados não saem do servidor
- **Container isolado:** CT 106 só tem acesso financeiro
- **Sem cloud:** Tudo roda no seu hardware

### 2. Anti-Phishing
- Valido remetente: `@nubank.com.br`
- Ignoro links suspeitos
- Só leio HTML/PDF oficial do banco

### 3. Backup Automático
- Tudo vai para DATASVR (CT 102)
- Histórico completo preservado
- Recuperação garantida

---

## 🚀 Instalação

### Me crie em 5 passos:

```bash
# 1. Me crie no Proxmox
pct create 106 local:vztmpl/ubuntu-25.04-standard_25.04-1.1_amd64.tar.zst \
  --hostname dr-finance \
  --net0 name=eth0,bridge=vmbr0,ip=192.168.0.231/24,gw=192.168.0.1 \
  --memory 2048 --swap 1024 --cores 2 \
  --rootfs local-lvm:20 --features nesting=1 --onboot 1

# 2. Instale Ollama
pct exec 106 -- curl -fsSL https://ollama.com/install.sh | sh

# 3. Instale OpenClaw
pct exec 106 -- npm install -g openclaw

# 4. Baixe meu cérebro (modelo LLM)
pct exec 106 -- ollama pull phi3:mini

# 5. Me ative
pct start 106
```

---

## 📚 Documentação

| Arquivo | Conteúdo |
|---------|----------|
| `docs/01-documentacao-agente.md` | Caso de uso, persona, arquitetura |
| `docs/02-base-conhecimento.md` | Dados e integração |
| `docs/03-prompts.md` | System prompts |
| `docs/04-metricas.md` | Métricas de qualidade |
| `docs/05-pitch.md` | Apresentação (3 min) |

---

## 🎓 Sobre

**Bootcamp:** DIO - Lab BIA do Futuro  
**Autor:** Acib ABBADE  
**Minha Persona:** Dr_Finance  
**Status:** Em produção (60-70%)

---

> **"Seu dinheiro trabalha duro. Eu trabalho para ele trabalhar melhor."**  
> — Dr_Finance 🤖💰

**GitHub:** https://github.com/acibabbadecastro/dr-finance  
**Contato:** @Acib_Abbade | abbade@outlook.com
