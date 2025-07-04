# 💰 Granah AI – Assistente Financeira via WhatsApp com OCR, IA e Painel Interativo

**Granah AI** é uma plataforma inteligente de automação financeira integrada ao WhatsApp, desenvolvida para transformar mensagens, fotos de notas fiscais e áudios em **lançamentos financeiros organizados**, usando **OCR local**, **inteligência artificial**, **fila de tarefas assíncronas** e um **painel de controle moderno e interativo**.  

> ⚠️ Este repositório é apenas demonstrativo. O código-fonte não está disponível por questões de privacidade e segurança.

---

## 🧠 Principais Funcionalidades

### 📥 Entrada de Dados Multicanal
- **Imagens de notas fiscais** (via WhatsApp)
- **Áudios** com descrições de gastos (ex: “gastei 30 reais no mercado”)
- **Mensagens de texto** estruturadas ou livres

### 🔄 Fila de Processamento Assíncrona (OCR e IA)
- Sistema robusto de **filas com Redis + workers Python** para processar:
  - Imagens (notas, cupons, boletos)
  - Áudios (conversão para texto + IA)
- Cada item passa por validações, pré-processamento e análise via IA.
- Fila resiliente com controle de timeout, paralelismo e logs detalhados.

### 📸 Extração Inteligente com IA e OCR
- **OCR local com Tesseract** + pipeline de pré-processamento com PIL
- Extração por IA com:
  - **Gemini Pro (Google Cloud Vision multimodal)**
  - **Fallback local com modelos como `phi` e `gemma` via Ollama**
- Extração de:
  - CNPJ do estabelecimento
  - Nome da loja
  - Valor total
  - Data da compra
- Quando há QR Code: scraping inteligente da **NFC-e da SEFAZ** com Selenium.

---

## 📊 Painel Web Interativo (FastAPI + Jinja2 + Tailwind)

### Telas disponíveis:
- **Relatórios financeiros por mês**
- **Dashboard com gráficos interativos** (gastos por categoria, metas, evolução)
- **Edição de lançamentos** via modal
- **Definição de metas com alertas visuais**
- **Compartilhamento de planilhas** com controle de acesso
- **Controle de planos e limites por usuário**

### Painel Admin:
- 🔧 Gestão de usuários, limites, planos
- 👁️ Visualização em tempo real das **filas de OCR e IA**
- 🧠 Histórico de **análises financeiras geradas por IA**
- ⚙️ **Painel de configurações dinâmico**:
  - Número de threads de OCR
  - Tempo máximo por tarefa
  - Modo silencioso
  - Log detalhado (DEBUG)

---

## 🧬 Estrutura Técnica

### 🔂 Backend (FastAPI)
- APIs REST para receber, processar e servir dados em tempo real
- Webhooks integrados com **WhatsAppJS**
- Lógica de rotas separada em módulos organizados
- Worker OCR separado para evitar bloqueios no bot principal

### 🗃️ Banco de Dados (PostgreSQL com SQLModel)
- Estrutura de dados avançada, incluindo:
  - `Usuario`, `Plano`, `Lancamento`, `Planilha`, `ArquivoOCR`, `AnaliseFinanceira`, `Convites`, `Advertencias`
- Relacionamentos completos e integridade referencial
- Suporte a múltiplos acessos por planilha (com status: convidado, aceito, pendente)
- Criação automática de diretórios e organização por usuário/mês

---

## 🤖 Análise Financeira com IA
- Quando o usuário envia "analise minhas finanças", o sistema:
  - Gera um resumo personalizado com:
    - Média de gastos
    - Categorias dominantes
    - Sugestões de economia
    - Anomalias de consumo
  - Utiliza **Gemini CLI local** com prompt avançado
  - Salva o resultado no banco e exibe no painel

---

## 💼 Planos de Uso
- **Padrão**: 1 planilha, até 3 lançamentos por dia
- **Premium**: até 3 planilhas, 10 lançamentos por dia
- **Deluxe**: até 10 planilhas, 30 lançamentos por dia + relatórios mensais
- Gerenciamento de **planos temporários**, **upgrade via comando**, e **rebaixamento automático**

---

## 📤 Exportações e Relatórios
- Geração de **relatórios mensais em PDF e Excel**
- Tela exclusiva `relatorio.html` com visual moderno e responsivo
- Indicadores de uso, usuários com acesso e convites pendentes
- Estimativa de receita por plano com base em métricas reais

---

## 🛡️ Segurança e Escalabilidade
- Sanitização de entradas com `escape()` para prevenir XSS
- Controle de acesso por planilha e rota
- Painel preparado para escalar horizontalmente com workers assíncronos

---

## 🚧 Status
- ✅ Em produção e 100% funcional
- 🔒 Código fechado
- 🧪 Em constante evolução com novos módulos (PIX, metas colaborativas, etc.)

---

## 🧑‍💼 Autor

Este projeto foi desenvolvido inteiramente por mim como parte do meu portfólio profissional. Ele reflete minha experiência prática em:
- Desenvolvimento backend com Python/FastAPI
- Automação inteligente usando OCR e modelos de IA
- Arquitetura robusta com filas, banco de dados relacional e APIs REST
- Integrações com sistemas externos como WhatsApp, Google Cloud Vision e Selenium
- Construção de dashboards modernos e funcionais para usuários e administradores
