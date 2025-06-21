# Granah.ai – Assistente Financeiro Automatizado via WhatsApp

![Status](https://img.shields.io/badge/status-em%20uso-brightgreen)
![Privacidade](https://img.shields.io/badge/privacidade-100%25%20-blue)
![OCR + IA](https://img.shields.io/badge/extracao-OCR%20%2B%20LLM-purple)

---

## 📌 Visão Geral

**Granah.ai** é um sistema inteligente de controle financeiro que opera exclusivamente via **WhatsApp**, permitindo que usuários registrem gastos com máxima simplicidade: basta enviar uma foto de uma nota fiscal ou escrever uma mensagem de texto. O sistema interpreta a informação, classifica o lançamento e o registra em planilhas organizadas por categoria, com visualização por meio de um dashboard interativo.

Granah é uma solução **completa, autônoma e segura**, pensada para quem precisa controlar as finanças pessoais com mínimo esforço.

---

## ⚙️ Funcionalidades Principais

### ✅ Entrada Inteligente de Dados

- **Por Imagem:**  
  1. Realiza OCR local com **Tesseract**.
  2. Se necessário, ativa fallback com modelos de IA multimodal via **Ollama** (`LLaVA`, `DeepSeek-VL`, etc).
  3. Decodifica **QR Codes** automaticamente e realiza **scraping da NFC-e** via Selenium.
  4. Extrai campos essenciais com IA:
     - Valor Total
     - Data
     - CNPJ
     - Nome do Estabelecimento

- **Por Texto:**  
  Mensagens como “gastei 23 no mercado” são interpretadas com NLP e modelos locais como `phi` ou `gemma`.

---

### 🧠 Classificação e Organização

- O usuário escolhe a categoria do gasto após a extração.
- Os dados são registrados em planilhas `.xlsx`, separadas por usuário e por mês.
- Cada usuário tem sua estrutura de diretório e seus próprios arquivos.

---

### 📊 Dashboard Interativo

Acesso via comando `/dashboard` no WhatsApp. Inclui:

- Tabela dinâmica com lançamentos
- Gráficos de categorias e variação mensal
- Edição inline de lançamentos
- Exportação em PDF/Excel
- Compartilhamento de planilhas com permissões

---

## 🔄 Fila de Processamento Inteligente

Granah implementa uma **fila assíncrona de tarefas** para:

- Processamento de imagens e textos
- Execução de OCR e scraping
- Registro e atualização de planilhas

Essa fila melhora a performance do sistema e isola falhas, garantindo maior **resiliência** e **segurança operacional**. Também reduz o tempo de resposta no WhatsApp, garantindo fluidez para o usuário final.

---

## 🧱 Arquitetura Técnica

- **Backend:** FastAPI  
- **OCR:** Tesseract com fallback utilizando modelos de IA via Ollama  
- **NLP e Extração:** Modelos LLM locais como `phi`, `gemma` e `llava`  
- **Scraping NFC-e:** Selenium com suporte a múltiplos estados e fallback OCR  
- **Fila de Processamento:** Orquestra OCR, scraping e gravações de forma assíncrona e isolada  
- **WhatsApp API:** Twilio  
- **Dashboard:** FastAPI + Jinja2 + Plotly.js + HTML5  
- **Armazenamento local:** Estrutura hierárquica por número de telefone:
- usuarios/
- └── 551199999999/
- ├── mercado_familia.xlsx
- └── casa.xlsx


---

### 👥 Compartilhamento Colaborativo de Planilhas

Granah permite que usuários compartilhem suas planilhas com terceiros (como familiares, cônjuges ou sócios), com **controle total de permissões**:

- Ao compartilhar, o sistema envia uma mensagem via WhatsApp solicitando ao convidado que aceite ou recuse o convite.
- O dono da planilha define o nível de acesso:
  - 🔒 Somente visualização
  - ✏️ Visualização + edição
- É possível revogar acessos a qualquer momento.
- A interface do dashboard exibe:
  - Convites pendentes
  - Lista de usuários com acesso
  - Botão para **revogação imediata**
- Tudo é auditável e restrito ao ambiente do usuário, garantindo total segurança.

Esse recurso torna o Granah ideal para **finanças em grupo**, como casais, famílias, projetos compartilhados e pequenos negócios.

---

### 🚫 Detecção Automática de Abusos e Moderação de Conteúdo

Para garantir a integridade da plataforma e evitar usos indevidos, o Granah utiliza IA local para detectar comportamentos suspeitos ou inapropriados, tanto em **imagens quanto em mensagens de texto**:

- Toda imagem recebida passa por um modelo de moderação que detecta:
  - Nudez
  - Conteúdo sexual
  - Violência
  - Conteúdo malicioso ou fora de contexto

- Mensagens com termos inapropriados ou fora do escopo do uso financeiro também são analisadas e classificadas:
  - Linguagem ofensiva
  - Spam
  - Intenções de uso fora da proposta do Granah

- Em ambos os casos, o sistema pode:
  - ⚠️ Emitir advertência automática ao usuário
  - ⛔ Bloquear a mensagem/imagem imediatamente
  - 🚫 Aplicar suspensão temporária ou banimento

Essa abordagem garante que o Granah permaneça um ambiente **ético, funcional e seguro**, preservando o foco no controle financeiro e no uso responsável da tecnologia.

---



## 🔐 Segurança e Privacidade

- **Isolamento de usuários:** Cada número possui seu próprio espaço de dados.
- **Controle de acesso:** Compartilhamentos com convite e aceite explícito.
- **Autenticação:** Acesso ao dashboard protegido por **token exclusivo** por usuário.
- **Banco relacional com validação:** SQLModel com proteção contra SQL Injection.
- **Proteção contra XSS:** Todos os campos editáveis são tratados com `escape()` backend/frontend.
- **Execução 100% local:** Nenhum dado é enviado para APIs externas. Todos os modelos de IA e OCR funcionam offline.

---

## 📈 Escalabilidade e Robustez

- Suporte a múltiplos usuários concorrentes
- Processamento assíncrono com filas dedicadas
- Contabilização de lançamentos por plano (Padrão, Premium, Deluxe)
- Painel 100% responsivo, ideal para uso em smartphones
- Modularidade para inclusão de novas funcionalidades

---

## 🚧 Status do Projeto

Granah está em operação, com uso diário e testes reais.  
Este repositório não disponibiliza código-fonte, pois a aplicação possui componentes proprietários e integrações privadas.

O objetivo é apresentar sua arquitetura, complexidade técnica e capacidade de resolver problemas reais.

---

## 📎 Capturas de Tela (em breve)

---

## 🧩 Features em desenvolvimento

- 🔔 Alertas de vencimento via WhatsApp
- 🎯 Metas mensais de controle
- 🤖 Chat de dúvidas com IA financeira
- 🔁 Backup automático e restauração de planilhas
- 💳 Pagamento automático via Pix com liberação de planos

---

