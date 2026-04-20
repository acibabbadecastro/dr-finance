# Pitch Dr_Finance - 3 Minutos

**Bootcamp:** DIO - Lab BIA do Futuro  
**Apresentador:** Acib ABBADE  
**Tempo:** 3 minutos  
**Formato:** GitHub + Demonstração

---

## ⏱️ ROTEIRO CRONOMETRADO

### **[0:00-0:30] Abertura - O Problema**

**O que falar:**
> "Quantas vezes você recebeu um email do banco, só olhou o valor e ignorou?
> 
> E no fim do mês, se perguntou: 'para onde foi meu salário?'
> 
> A verdade é que **90% das pessoas não têm controle real dos seus gastos**.
> 
> Recebemos dezenas de notificações por dia, mas elas não nos ajudam a tomar decisões.
> 
> **E se pudéssemos analisar gastos automaticamente, com privacidade total?**"

**Slide/Screen:** Mostrar tela inicial do GitHub

---

### **[0:30-1:00] A Solução - Dr_Finance**

**O que falar:**
> "Apresento o **Dr_Finance** - seu guardião financeiro pessoal.
> 
> Ele lê automaticamente os emails do Nubank, extrai todas as transações,
> valida se são legítimas e às 19h manda um relatório com:
> 
> - O que você gastou hoje
> - Se está acima da sua média
> - **Sugestões reais de economia**
> 
> **Exemplo:** 'Seus gastos com restaurantes estão 180% acima.
> Reduzir de 5 para 3 jantares/semana economiza R$ 560 por mês.'
> 
> Tudo rodando no seu próprio servidor, dados não saem da sua rede."

**Slide/Screen:** Mostrar exemplo de relatório no README

---

### **[1:00-1:45] Como Funciona - Os 3 Containers**

**O que falar:**
> "O Dr_Finance é parte de um ecossistema de **3 containers no Proxmox**:
> 
> **CT 101 - Mails:** Lê emails do Nubank, valida anti-phishing
> 
> **CT 106 - Dr_Finance:** Analisa com IA local (Ollama), gera relatórios
> 
> **CT 102 - DATASVR:** Backup de tudo, histórico completo
> 
> **Fluxo:** Email → Mails → Dr_Finance → DATASVR → Você
> 
> **Vantagens:**
> - Privacidade total (Ollama local)
> - Container isolado (segurança)
> - Backup automático
> - Custo zero (hardware próprio)"

**Slide/Screen:** Mostrar diagrama dos 3 CTs no README

---

### **[1:45-2:30] Demonstração - Dados Reais**

**O que falar:**
> "Vou mostrar como funciona na prática.
> 
> **Temos 19 transações simuladas** de um usuário real:
> 
> - Combustível: Posto Ipiranga, Shell - R$ 150, R$ 145
> - Alimentação: Fogo de Chão, McDonalds, Pizza Hut
> - Transporte: Pedágios SP-Rio - R$ 45,50 cada
> - Serviços: Lavanderia, Academia
> - Renda: Salário de R$ 5.000
> 
> **O Dr_Finance analisa e diz:**
> - 'Combustível: 47% dos gastos'
> - 'Alimentação: 38% dos gastos'
> - 'Sugestão: Reduzir jantares fora economiza R$ 560/mês'
> 
> **Perfil do investidor também está cadastrado:**
> - Moderado, R$ 12k/mês, R$ 450k investidos"

**Slide/Screen:** Mostrar arquivos data/transacoes.csv e data/perfil_investidor.json

---

### **[2:30-3:00] Fechamento - Call to Action**

**O que falar:**
> "O Dr_Finance não é só um projeto de bootcamp.
> 
> É uma **prova de conceito** de que podemos ter:
> - IA financeira inteligente
> - Com privacidade total
> - Sem depender de cloud
> - Rodando no seu próprio hardware
> 
> **Status:**
> - Documentação: 100% completa
> - Infraestrutura: CT 106 criado
> - Implementação: 60-70%
> 
> **Economia estimada:** R$ 5.400 por ano por usuário.
> 
> O código está no GitHub, a arquitetura está documentada.
> 
> **Quem quer ter controle total das próprias finanças?**
> 
> Obrigado!"

**Slide/Screen:** Mostrar link do GitHub + contato

---

## 📊 SLIDES/SCREENS SUGERIDOS

### **Screen 1 (0:00):**
- README do GitHub
- Título: "Dr_Finance - Seu Guardião Financeiro Pessoal"
- Badges: Status, Proxmox, OpenClaw, Ollama

### **Screen 2 (0:30):**
- Seção "O que Faço Todo Dia"
- Exemplo de relatório

### **Screen 3 (1:00):**
- Diagrama dos 3 Containers (CT 101, CT 102, CT 106)
- Tabela com Mails, DATASVR, Dr_Finance

### **Screen 4 (1:45):**
- Arquivo data/transacoes.csv
- Mostrar 19 transações

### **Screen 5 (2:30):**
- Link do GitHub
- Contato: @Acib_Abbade

---

## 💡 DICAS DE APRESENTAÇÃO

### **Postura:**
- Firme e confiante
- Contato visual com a câmera
- Mãos livres para gestos

### **Tom de Voz:**
- **Início:** Provocativo (problema)
- **Meio:** Entusiasmado (solução)
- **Fim:** Convincente (call to action)

### **Erros para Evitar:**
- ❌ Ler slides
- ❌ Falar muito rápido
- ❌ Pedir desculpas
- ❌ Termos técnicos demais

### **O que Enfatizar:**
- ✅ **Privacidade** (dados locais)
- ✅ **Arquitetura** (3 containers Proxmox)
- ✅ **Resultados** (economia real)
- ✅ **Custo zero** (sem cloud)

---

## ❓ PERGUNTAS QUE PODEM VIR

### **P1: "E se o email não chegar?"**
**Resposta:**
> "O Dr_Finance notifica: 'Nenhum email do Nubank recebido.
> Verifique se há transações.' Transparência total."

### **P2: "Como valida se é phishing?"**
**Resposta:**
> "Três camadas:
> 1. Valida domínio (@nubank.com.br)
> 2. Não clica em links, só lê HTML/PDF
> 3. Se falhar, descarta e alerta usuário"

### **P3: "Por que não usa API do banco?"**
**Resposta:**
> "Dois motivos:
> 1. Segurança: API exige credenciais bancárias
> 2. Complexidade: Cada banco tem API diferente
> Email é universal e mais seguro para MVP"

### **P4: "Quanto custa rodar isso?"**
**Resposta:**
> "Zero em software (Ollama grátis, Python grátis).
> Hardware: qualquer PC com 4GB RAM roda.
> Meu servidor tem 13 containers, Dr_Finance usa ~2GB RAM."

---

## ✅ CHECKLIST PRÉ-APRESENTAÇÃO

- [ ] Testar compartilhamento de tela
- [ ] Abrir README do GitHub
- [ ] Abrir arquivos de dados (transacoes.csv)
- [ ] Cronometrar (3 minutos)
- [ ] Ensaiar em voz alta (2-3 vezes)
- [ ] Preparar ambiente (silêncio, iluminação)
- [ ] Ter link do GitHub à mão

---

**Tempo Total:** 3 minutos  
**Palavras:** ~450 palavras (150 palavras/minuto)  
**Tom:** Profissional e direto (como o Dr_Finance)

---

**Autor:** Acib ABBADE  
**Data:** 19/04/2026  
**GitHub:** https://github.com/acibabbadecastro/dr-finance
