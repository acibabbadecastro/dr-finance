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

### **📊 Diagrama 1: Arquitetura do Ecossistema**

```mermaid
graph TB
    subgraph PVE1["PVE1 - Proxmox Virtual Environment<br/>192.168.0.192 | Ryzen 5 5600X | 30GB RAM"]
        direction TB
        
        subgraph REDE["Rede Interna 192.168.0.0/24 - vmbr0"]
            direction LR
            
            CT101["📧 CT 101 - Mails<br/>IP: 192.168.0.240<br/>2GB RAM | 8GB Disk<br/>• Gmail IMAP<br/>• HTML Parser<br/>• Anti-Phishing"]
            
            CT106["🤖 CT 106 - Dr_Finance<br/>IP: 192.168.0.231<br/>2GB RAM | 20GB Disk<br/>• OpenClaw<br/>• Ollama 0.21<br/>• Node.js 20"]
            
            CT102["💾 CT 102 - DATASVR<br/>IP: 192.168.0.72<br/>2GB RAM | 50GB Disk<br/>• Samba<br/>• Rsync<br/>• Backup"]
        end
    end
    
    GMAIL["🌐 Gmail/Nubank<br/>Internet"]
    USER["📱 Usuário<br/>Telegram/Email"]
    
    GMAIL -->|IMAP SSL| CT101
    CT101 -->|JSON API| CT106
    CT106 -->|Backup| CT102
    CT106 -->|Relatório| USER
    
    style PVE1 fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    style REDE fill:#fff3e0,stroke:#e65100,stroke-width:2px
    style CT101 fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    style CT106 fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    style CT102 fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    style GMAIL fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style USER fill:#fce4ec,stroke:#c2185b,stroke-width:2px
```

---

### **🔄 Diagrama 2: Fluxo de Dados (10 Passos)**

```mermaid
sequenceDiagram
    participant G as Gmail/Nubank
    participant M as CT 101 Mails
    participant D as CT 106 Dr_Finance
    participant B as CT 102 DATASVR
    participant U as Usuario
    
    Note over G,U: 19:00 - Cron Job Dispara
    
    G->>M: 1. Email chega
    M->>M: 2. Valida dominio
    M->>M: 3. Anti-phishing
    M->>M: 4. Extrai dados
    M->>M: 5. Estrutura JSON
    M->>D: 6. Envia dados
    D->>B: 7. Consulta historico
    D->>D: 8. Analisa com IA
    D->>U: 9. Envia relatorio
    D->>B: 10. Backup
    
    Note over G,U: Conclusao em ~30 segundos
```

---

### **🔧 Diagrama 3: Arquitetura Técnica por Camadas**

```mermaid
graph TD
    subgraph CT101["CT 101 - Mails (192.168.0.240)"]
        direction TB
        A1["📨 Gmail IMAP<br/>Port 993 SSL"]
        A2["🔍 HTML Parser<br/>BeautifulSoup"]
        A3["📄 PDF Extractor<br/>PyPDF2"]
        V1["✅ Validação Domínio"]
        V2["🚫 Anti-Phishing"]
        E1["📤 JSON Export<br/>POST :18789"]
        
        A1 --> V1
        A2 --> V1
        A3 --> V1
        V1 --> V2
        V2 --> E1
    end
    
    subgraph CT106["CT 106 - Dr_Finance (192.168.0.231)"]
        direction TB
        O1["🧠 OpenClaw<br/>Orquestrador"]
        O2["🤖 Ollama 0.21<br/>phi3:mini 3.8B"]
        O3["📊 Skills Python<br/>analyzer.py"]
        O4["⏰ Cron Jobs<br/>19:00 diário"]
        
        O1 --> O2
        O2 --> O3
        O3 --> O4
    end
    
    subgraph CT102["CT 102 - DATASVR (192.168.0.72)"]
        direction TB
        B1["🗂️ Samba SMB<br/>\\\\192.168.0.72\\LAN\\"]
        B2["📁 Estrutura<br/>05-FINANCAS/"]
        B3["🔄 Rsync<br/>Incremental"]
        B4["📊 Logs Auditoria"]
        
        B1 --> B2
        B2 --> B3
        B3 --> B4
    end
    
    E1 -->|JSON| O1
    O4 -->|Backup| B1
    
    style CT101 fill:#e8f5e9,stroke:#2e7d32,stroke-width:3px
    style CT106 fill:#fff3e0,stroke:#ef6c00,stroke-width:3px
    style CT102 fill:#e3f2fd,stroke:#1565c0,stroke-width:3px
```

---

### **📊 Resumo dos 3 CTs:**

| CT | Nome | IP | Função | Recursos | Software Principal |
|----|------|-----|--------|----------|-------------------|
| **101** | Mails | 192.168.0.240 | Coleta e valida emails | 2GB RAM, 2 cores, 8GB | Gmail API, HTML Parser, Anti-Phishing |
| **102** | DATASVR | 192.168.0.72 | Backup e histórico | 2GB RAM, 2 cores, 50GB | Samba, Rsync, ZFS |
| **106** | Dr_Finance | 192.168.0.231 | Análise com IA | 2GB RAM, 2 cores, 20GB | OpenClaw, Ollama, Node.js |

---

## 🛠️ Tecnologias que Usei

### **Infraestrutura:**

**3 Containers Proxmox:**

| CT | RAM | CPU | Disk | Função |
|----|-----|-----|------|--------|
| 101 | 2GB | 2 cores | 8GB | Coleta emails |
| 102 | 2GB | 2 cores | 50GB | Backup |
| 106 | 2GB | 2 cores | 20GB | Análise IA |
| **Total** | **6GB** | **6 cores** | **78GB** | - |

### **Stack Tecnológico:**

| Camada | Tecnologia | Versão | Finalidade |
|--------|------------|--------|------------|
| **Hipervisor** | Proxmox VE | 8.x | Virtualização |
| **Container** | LXC | 5.x | Linux Containers |
| **SO** | Ubuntu | 25.04 | Sistema base |
| **Orquestrador** | OpenClaw | 1.0 | Agente AI |
| **IA Local** | Ollama | 0.21.0 | LLM runner |
| **Modelo** | phi3:mini | 3.8B | Análise texto |
| **Runtime** | Node.js | 20.x | JavaScript |
| **Backup** | Samba | 4.x | Compartilhamento |
| **Rede** | Bridge vmbr0 | - | 192.168.0.0/24 |

### **Dados Mockados:**
- **19 transações** simuladas (gasolina, pedágio, almoço, lavanderia)
- **Perfil de investidor** completo (moderado, R$ 12k/mês, R$ 450k investidos)
- **6 produtos financeiros** cadastrados

---

## 📊 Como Funciona na Prática

### **Rotina Diária Automática (19:00):**

```mermaid
journey
    title Rotina Diária do Dr_Finance
    section Coleta
      Verifica emails Nubank: 5: CT 101
      Filtra @nubank.com.br: 5: CT 101
      Valida anti-phishing: 5: CT 101
      Extrai transações: 5: CT 101
    section Análise
      Recebe dados JSON: 5: CT 106
      Compara histórico: 5: CT 106
      Ollama analisa: 5: CT 106
      Identifica padrões: 5: CT 106
      Gera sugestões: 5: CT 106
    section Entrega
      Envia Telegram: 5: CT 106
      Envia Email: 5: CT 106
      Backup automático: 5: CT 102
```

### **Exemplo de Relatório:**

```
📊 Relatório Financeiro - 19/04/2026

Resumo:
• Gastos: R$ 320,00
• Receitas: R$ 5.000,00
• Saldo: +R$ 4.680,00

Por Categoria:
• Combustível: R$ 150,00 (47%) ⚠️
• Alimentação: R$ 120,00 (38%)
• Transporte: R$ 45,50 (14%)
• Serviços: R$ 70,00

💡 Sugestão:
"Reduzir jantares fora gera economia de R$ 560/mês"
```

---

## 🔒 Por que Escolhi Essa Arquitetura

### **Privacidade Total:**

```mermaid
graph LR
    subgraph LOCAL["🏠 Ambiente Local"]
        direction TB
        CT101["CT 101"]
        CT106["CT 106"]
        CT102["CT 102"]
    end
    
    INTERNET["🌐 Internet"]
    NUVEM["☁️ Cloud APIs"]
    
    INTERNET -->|Apenas leitura| CT101
    CT101 -.->|NÃO ENVIA| NUVEM
    CT106 -.->|NÃO ENVIA| NUVEM
    CT102 -.->|NÃO ENVIA| NUVEM
    
    style LOCAL fill:#e8f5e9,stroke:#2e7d32,stroke-width:3px
    style NUVEM fill:#ffebee,stroke:#c62828,stroke-dasharray: 5 5
```

### **Segurança em Camadas:**

| Camada | Proteção | Implementação |
|--------|----------|---------------|
| **1** | Validação de Domínio | @nubank.com.br obrigatório |
| **2** | Anti-Phishing | SPF/DKIM + Blocklist |
| **3** | Container Isolado | CT 106 separado para finanças |
| **4** | Rede Interna | 192.168.0.0/24 sem acesso externo |
| **5** | Backup Recuperável | CT 102 com histórico |

### **Custo Zero:**
- ✅ Hardware próprio (sem mensalidades de cloud)
- ✅ Software open-source (Ollama, OpenClaw, Python)
- ✅ Sem APIs pagas de banco

---

## 📁 Estrutura do Repositório

```
dr-finance/
├── README.md              # Este arquivo (explicação do projeto)
├── PITCH-3MINUTOS.md      # Roteiro da apresentação (3 min)
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
- Acesso ao host PVE1 (192.168.0.192)
- Credenciais root (não compartilhar)

### **Comandos que Usei:**

```bash
# ═══════════════════════════════════════════════════════════════
# CT 101 - MAILS (Coletor de Emails)
# ═══════════════════════════════════════════════════════════════
pct create 101 local:vztmpl/ubuntu-25.04-standard_25.04-1.1_amd64.tar.zst \
  --hostname mails \
  --net0 name=eth0,bridge=vmbr0,ip=192.168.0.240/24,gw=192.168.0.1 \
  --memory 2048 --swap 1024 --cores 2 \
  --rootfs local-lvm:8 --features nesting=1 --onboot 1

# ═══════════════════════════════════════════════════════════════
# CT 102 - DATASVR (Armazenamento e Backup)
# ═══════════════════════════════════════════════════════════════
pct create 102 local:vztmpl/ubuntu-25.04-standard_25.04-1.1_amd64.tar.zst \
  --hostname datasvr \
  --net0 name=eth0,bridge=vmbr0,ip=192.168.0.72/24,gw=192.168.0.1 \
  --memory 2048 --swap 1024 --cores 2 \
  --rootfs local-lvm:50 --features nesting=1 --onboot 1

# ═══════════════════════════════════════════════════════════════
# CT 106 - Dr_Finance (Análise com IA)
# ═══════════════════════════════════════════════════════════════
pct create 106 local:vztmpl/ubuntu-25.04-standard_25.04-1.1_amd64.tar.zst \
  --hostname dr-finance \
  --net0 name=eth0,bridge=vmbr0,ip=192.168.0.231/24,gw=192.168.0.1 \
  --memory 2048 --swap 1024 --cores 2 \
  --rootfs local-lvm:20 --features nesting=1 --onboot 1

# ═══════════════════════════════════════════════════════════════
# INSTALAÇÃO DE SOFTWARE NO CT 106
# ═══════════════════════════════════════════════════════════════
# Instalar Ollama (IA local)
pct exec 106 -- curl -fsSL https://ollama.com/install.sh | sh

# Instalar OpenClaw (orquestrador)
pct exec 106 -- npm install -g openclaw

# Baixar modelo LLM (phi3:mini - 3.8B parâmetros)
pct exec 106 -- ollama pull phi3:mini

# Configurar modo CPU-only (GPU AMD ROCm com problemas)
pct set 106 --env OLLAMA_CPU_ONLY=1
```

---

## 📊 Dados Mockados que Criei

### **transacoes.csv (19 transações):**

| Data | Descrição | Categoria | Valor |
|------|-----------|-----------|-------|
| 2026-04-19 | Posto Ipiranga | Combustível | R$ 150,00 |
| 2026-04-19 | Shell Box | Combustível | R$ 145,00 |
| 2026-04-18 | Fogo de Chão | Alimentação | R$ 280,00 |
| 2026-04-18 | McDonald's | Alimentação | R$ 45,00 |
| 2026-04-17 | Pedágio SP-Rio | Transporte | R$ 45,50 |
| 2026-04-17 | Lavanderia | Serviços | R$ 70,00 |
| 2026-04-15 | Salário | Renda | R$ 5.000,00 |

### **perfil_investidor.json:**
- **Perfil:** Moderado
- **Renda:** R$ 12.000/mês
- **Investimentos:** R$ 450.000
- **Metas:** Reserva de emergência, Aposentadoria, Viagem anual

---

## 🎓 Sobre o Projeto

**Bootcamp:** DIO - Lab BIA do Futuro  
**Autor:** Acib ABBADE  
**Problema Escolhido:** Controle de gastos pessoais  
**Solução:** Agente que analisa emails do Nubank e sugere economia

### **Status:**
- ✅ Documentação completa
- ✅ 3 containers criados (CT 101, CT 102, CT 106)
- ✅ Dados mockados cadastrados
- ✅ 6 diagramas Mermaid profissionais
- ✅ Pitch de 3 minutos

### **Próximos Passos:**
- ⏳ Finalizar instalação do OpenClaw no CT 106
- ⏳ Configurar cron jobs (19:00 diário)
- ⏳ Testar integração completa (Mails → Dr_Finance → DATASVR)

---

## 📞 Contato

**Acib ABBADE**  
Telegram: [@Acib_Abbade](https://t.me/Acib_Abbade)  
Email: abbade@outlook.com  
GitHub: [acibabbadecastro](https://github.com/acibabbadecastro)

---

> **"Desenvolvi este projeto para automatizar o controle de gastos pessoais, garantindo privacidade total dos dados usando IA local."**  
> — Acib ABBADE

**Última atualização:** 20/04/2026 00:10  
**Ecossistema:** 3 containers (CT 101-Mails, CT 102-DATASVR, CT 106-Dr_Finance)
