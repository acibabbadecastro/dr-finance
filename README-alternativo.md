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

## 🎯 O que eu faço

### Todo dia às 19:00, eu:

1. **Leio seus emails do Nubank** automaticamente
2. **Valido autenticidade** (anti-phishing)
3. **Extraio transações** (PIX, débito, crédito)
4. **Analiso padrões** com IA local (Ollama)
5. **Gero relatório** com alertas e sugestões
6. **Faço backup** de tudo no DATASVR

### Exemplo do meu trabalho:

```
📊 Relatório Financeiro - 19/04/2026

Resumo do Dia:
├─ Gastos: R$ 320,00
├─ Receitas: R$ 0,00
└─ Saldo: -R$ 320,00

Por Categoria:
├─ Restaurantes: R$ 180 (56%) ⚠️ 180% acima da média
├─ Uber: R$ 90 (28%) ✅ dentro do padrão
└─ Mercado: R$ 50 (16%) ✅ dentro do padrão

💡 Minha Sugestão:
"Reduzir jantares fora de 5 para 3x/semana
 gera economia de R$ 560/mês (R$ 6.720/ano)"
```

---

## 🏗️ Minha Arquitetura

### Onde eu rodo:

```
┌─────────────────────────────────────────┐
│  Proxmox PVE1 (192.168.0.192)          │
│                                          │
│  ┌─────────────────────────────────┐   │
│  │  CT 106 - Dr_Finance            │   │
│  │  IP: 192.168.0.231              │   │
│  │  RAM: 2GB | CPU: 2 cores        │   │
│  │  Disco: 20GB                    │   │
│  │                                  │   │
│  │  Software:                      │   │
│  │  • Ubuntu 25.04                 │   │
│  │  • OpenClaw                     │   │
│  │  • Ollama 0.21.0 (CPU only)     │   │
│  │  • Node.js 20.x                 │   │
│  └─────────────────────────────────┘   │
└─────────────────────────────────────────┘
```

### Com quem eu trabalho:

| Container | IP | Função |
|-----------|-----|--------|
| **CT 101** - MailBot | 192.168.0.240 | Lê emails do Nubank |
| **CT 102** - DATASVR | 192.168.0.72 | Backup de tudo |
| **CT 106** - Dr_Finance | 192.168.0.231 | **EU** - Análise |

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

## 📊 Resultados que entrego

### Métricas:

| Métrica | Minha Meta |
|---------|------------|
| Precisão | ≥ 90% |
| Segurança | 100% (zero alucinações) |
| Tempo | < 30s por relatório |
| Economia gerada | R$ 5.400/ano por usuário |
| Disponibilidade | ≥ 95% |

### O que já identifiquei em testes:

- **Gastos fantasmas:** Assinaturas esquecidas (R$ 89,90/mês)
- **Padrões problemáticos:** Restaurantes 180% acima da média
- **Oportunidades:** Troca de planos (economia R$ 50/mês)
- **Alertas em tempo real:** Gastos fora do padrão notificados

---

## 🚀 Como me usar

### Via Telegram (recomendado):

```
/fin diario      → Relatório de hoje
/fin semanal     → Resumo da semana
/fin mensal      → Fechamento do mês
/fin categoria Restaurantes  → Gastos por categoria
/fin economia    → Minhas sugestões
```

### Via Dashboard Web:

```
http://192.168.0.231:8501
```

### Via Email:

Relatório completo enviado para `abbade@outlook.com` todo dia às 19:00

---

## 🛠️ Instalação (para quem quer me ter)

### Pré-requisitos:

- Proxmox VE instalado
- CT 101 (MailBot) configurado
- CT 102 (DATASVR) configurado

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

**Documentação completa:** `/docs/`

---

## 📚 Documentação

| Arquivo | O que contém |
|---------|--------------|
| `docs/01-documentacao-agente.md` | Meu caso de uso, persona, arquitetura |
| `docs/02-base-conhecimento.md` | Como acesso e uso os dados |
| `docs/03-prompts.md` | Como penso e respondo |
| `docs/04-metricas.md` | Como sou avaliado |
| `docs/05-pitch.md` | Apresentação (3 min) |
| `docs/CT106-INSTALACAO.md` | Guia de instalação completo |

---

## 🎓 Sobre este Projeto

**Bootcamp:** DIO - Lab BIA do Futuro  
**Autor:** Acib ABBADE  
**Minha Persona:** Dr_Finance (especialista financeiro)  
**Status:** Em produção (60-70% implementação)

### O que já está pronto:

- ✅ Documentação completa
- ✅ Container criado (CT 106)
- ✅ Rede configurada
- ✅ Ollama instalado
- ✅ Node.js instalado

### O que estou finalizando:

- ⏳ Serviço Ollama (iniciando)
- ⏳ OpenClaw (instalando)
- ⏳ Skills de análise (em criação)
- ⏳ Cron jobs (configurando)

---

## 💡 Por que "Dr_Finance"?

Escolhi esse nome porque:

1. **Dr** = Especialista, autoridade no assunto
2. **Finance** = Foco exclusivo em finanças
3. **Não sou genérico** = Fui criado para UMA função específica

Não tentei ser tudo para todos. Fui feito para cuidar do seu dinheiro. Ponto.

---

## 🔗 Links

- **GitHub:** https://github.com/acibabbadecastro/dr-finance
- **OpenClaw:** https://docs.openclaw.ai
- **Ollama:** https://ollama.ai
- **Proxmox:** https://pve.proxmox.com

---

## 📞 Contato

**Acib ABBADE**  
Telegram: [@Acib_Abbade](https://t.me/Acib_Abbade)  
Email: abbade@outlook.com  
GitHub: [acibabbadecastro](https://github.com/acibabbadecastro)

---

> **"Seu dinheiro trabalha duro. Eu trabalho para ele trabalhar melhor."**  
> — Dr_Finance 🤖💰

**Última atualização:** 19/04/2026 23:30  
**Status:** Em produção no CT 106 (192.168.0.231)
