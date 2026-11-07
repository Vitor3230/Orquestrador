# 🚀 Orquestrador Multi-Agente de IA para Diagnóstico de Carreira e Tutoria em TI

[![n8n](https://img.shields.io/badge/n8n-v2.46.6+-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-API-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://aistudio.google.com/)
[![Docker](https://img.shields.io/badge/Docker-Linux%20Mint-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)

Um pipeline de automação visual desenvolvido no **n8n** que orquestra múltiplos agentes de Inteligência Artificial autônomos (via **Google Gemini API**). O sistema realiza a leitura contextual de um currículo profissional de TI, identifica lacunas técnicas (*gaps*) em relação ao mercado e gera automaticamente um plano de estudos personalizado acompanhado de simulados práticos de fixação.

---

## 📐 Arquitetura da Solução

O fluxo de dados é totalmente orquestrado dentro de um ambiente local isolado no Docker:

```text
  ┌──────────────────────────┐
  │ Gatilho Manual / Webhook │
  └─────────────┬────────────┘
                │
                ▼
  ┌──────────────────────────┐
  │ Entrada: Texto Currículo │
  └─────────────┬────────────┘
                │ (Passagem do perfil profissional)
                ▼
  ┌──────────────────────────────────────────────────────────┐
  │ Agente 1: Analista de Currículo (@analise-curriculo-ti)   │
  │ Model: Google Gemini 1.5                                 │
  └─────────────┬────────────────────────────────────────────┘
                │ (Diagnóstico estruturado & Mapeamento de Gaps)
                ▼
  ┌──────────────────────────────────────────────────────────┐
  │ Agente 2: Tutor Pedagógico de TI (@tutor-estudos-ti)     │
  │ Model: Google Gemini 1.5                                 │
  └─────────────┬────────────────────────────────────────────┘
                │
                ▼
  ┌──────────────────────────────────────────────────────────┐
  │ Output Final: Relatório + Plano de Ação + Simulados MD   │
  └──────────────────────────────────────────────────────────┘
```

---

## 🤖 Papéis dos Agentes de IA

### 1. Agente Analista de Currículo (`@analise-curriculo-ti`)
* **Função:** Realiza a leitura e o parsing do perfil técnico do candidato (experiência, conhecimentos em redes, sistemas operacionais, suporte e automação).
* **Entregável:** Diagnóstico sucinto apontando os pontos fortes e, principalmente, as lacunas técnicas (*gaps*) necessárias para evolução na carreira de TI.

### 2. Agente Tutor de Estudos (`@tutor-estudos-ti`)
* **Função:** Consome a análise gerada pelo Agente Analista sem necessidade de reintervenção do usuário.
* **Entregável:** 
  1. Cronograma de estudos focado nos pontos fracos identificados.
  2. Roteiro prático de laboratório.
  3. Mini-simulado com 3 questões de fixação no formato múltipla escolha com gabarito comentado.

---

## 🛠️ Tecnologias Utilizadas

* **Orquestração Visual:** [n8n](https://n8n.io) (v2.46.6+)
* **Provedor de LLM:** Google Gemini API (`gemini-1.5-flash` / `gemini-1.5-pro`)
* **Infraestrutura:** Docker & Docker Volumes
* **Sistema Operacional de Execução:** Linux Mint
* **Engenharia de Prompt:** System Messages Estruturadas & Formatação Markdown/JSON

---

## 🚀 Como Executar o Projeto Localmente

### Pré-requisitos
* Sistema operacional Linux (testado no Linux Mint) ou Windows/macOS.
* **Docker** instalado e ativo.
* **API Key** gratuita obtida no [Google AI Studio](https://aistudio.google.com).

### 1. Subir o ambiente n8n via Docker

```bash
# 1. Criar diretório de persistência de dados
mkdir -p ~/.n8n

# 2. Subir o container oficial do n8n
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  --restart always \
  n8nio/n8n
```

### 2. Configurar no n8n

1. Acesse o painel no navegador: `http://localhost:5678`
2. Crie sua conta local de administrador.
3. Acesse **Credentials** > **Add Credential** > Selecione **Google Gemini API**.
4. Cole sua chave obtida no Google AI Studio e salve.

### 3. Importar o Workflow

1. No n8n, crie um novo fluxo em **Workflows** > **Import from File**.
2. Selecione o arquivo `workflows/orquestrador_mentoria_ia.json` disponível neste repositório.
3. Conecte sua credencial da Google Gemini API nos nós de modelo de linguagem.
4. Clique em **Execute Workflow** para testar.

---

## 📁 Estrutura do Repositório

```text
.
├── README.md                           # Documentação técnica do projeto
├── workflows/
│   └── orquestrador_mentoria_ia.json   # Exportação do fluxo n8n para importação
└── prompts/
    ├── analista_curriculo_system.txt   # Prompt de instrução do Agente Analista
    └── tutor_estudos_system.txt        # Prompt de instrução do Agente Tutor
```

---

## 👨‍💻 Autor

**Vitor Henrique Evangelista Sardinha**
* LinkedIn: [vitor-henrique-evangelista](https://linkedin.com/in/vitor-henrique-evangelista)
* GitHub: [Vitor3230](https://github.com/Vitor3230)