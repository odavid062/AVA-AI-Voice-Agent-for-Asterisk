<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/banner_dark_mode.png">
  <img alt="AVA - Agente de Voz com IA para Asterisk" src="assets/banner_light_mode.png" width="100%">
</picture>

# 🤖 AVA — Agente de Voz com IA (PT-BR)

**Atendente telefônico por Inteligência Artificial para Asterisk, configurado para Português do Brasil usando Google Gemini Live.**

![Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Python](https://img.shields.io/badge/python-3.11+-blue.svg)
![Docker](https://img.shields.io/badge/docker-compose-blue.svg)
![Asterisk](https://img.shields.io/badge/asterisk-18+-orange.svg)
![IA](https://img.shields.io/badge/IA-Google%20Gemini%20Live-4285F4.svg)

</div>

---

## 📌 Sobre o projeto

O **AVA** é um agente de voz com IA que **atende ligações telefônicas e conversa em linguagem natural, em português, em tempo real**. Quando alguém liga, a IA atende, entende a fala, responde por voz e pode executar ações (transferir, agendar, encerrar a chamada, etc.).

Esta é a minha implementação e customização do projeto open-source **AVA — AI Voice Agent for Asterisk**, ajustada para:

- 🇧🇷 **Conversa em Português do Brasil** — saudação e prompts em PT-BR
- 🧠 **Google Gemini Live** como cérebro de voz (STT + LLM + TTS num único provedor de baixa latência)
- ☎️ **Asterisk (PJSIP)** para a telefonia, com o softphone **MicroSIP** para testes
- 🐳 **Docker** rodando em **WSL2 (Ubuntu)** no Windows 11

> 💼 **Faz parte do meu portfólio.** Meu objetivo é oferecer **agentes de voz inteligentes como serviço** para empresas — atendimento, qualificação de leads, agendamento e SAC automatizados.

---

## 🧩 Como funciona (arquitetura)

```
   Ligação (SIP / MicroSIP)
            │
            ▼
     Asterisk (PJSIP)  ──Stasis / ARI──►  AVA AI Engine (Python, Docker)
            │                                       │
            │◄──────────── AudioSocket ────────────►│
                                                    ▼
                                        Google Gemini Live (IA de voz)
```

- O **Asterisk** recebe a ligação e a entrega ao app Stasis `asterisk-ai-voice-agent`.
- O **AVA AI Engine** transporta o áudio via **AudioSocket** e conversa com o **Google Gemini Live**.
- A IA responde por voz e pode chamar **ferramentas** (encerrar chamada, transferir, agendar, etc.).

---

## 🛠️ Stack

| Camada      | Tecnologia                                   |
| ----------- | -------------------------------------------- |
| Telefonia   | Asterisk 18+ (PJSIP), AudioSocket            |
| IA de voz   | Google Gemini Live (`google_live`)           |
| Motor       | Python 3.11+ (asyncio, ARI)                  |
| Infra       | Docker Compose, WSL2 (Ubuntu) / Windows 11   |
| Teste       | Softphone MicroSIP (ramal **100**)           |

---

## 🚀 Status atual

- ✅ Asterisk + PJSIP configurados (ramal/softphone **online**)
- ✅ Engine integrado ao **Google Gemini Live**, com saudação em português
- 🔧 Em ajuste fino de **estabilidade do serviço** e **áudio bidirecional**

---

## 📋 Pré-requisitos

Para executar o AVA localmente ou em servidor, são necessários:

- **Docker** e **Docker Compose v2** (suporte ao comando `docker compose` ou utilitário `docker-compose`).
- **Asterisk 18+** com o módulo ARI (Asterisk REST Interface) ativado e driver PJSIP configurado.
- **Python 3.11+** (caso execute scripts auxiliares, CLI ou testes diretamente na máquina host).
- **Chave de API do Google Cloud** com suporte ao Gemini Live (`GOOGLE_API_KEY`) ou credencial de Service Account (`GOOGLE_APPLICATION_CREDENTIALS`).
- **Softphone SIP** para testes de chamadas (ex.: **MicroSIP** configurado no ramal local).
- Ambiente testado: **Linux** (Ubuntu 20.04+, Debian 11+) e **Windows 11 via WSL2 (Ubuntu)**.

---

## ⚙️ Variáveis de Ambiente

As configurações de ambiente e credenciais devem ser definidas no arquivo `.env` (gerado a partir de `.env.example`).

> ⚠️ **Atenção:** Nunca compartilhe ou versione valores de credenciais e chaves secretas.

Principais variáveis utilizadas:

| Variável | Descrição / Finalidade |
| -------- | ---------------------- |
| `COMPOSE_PROJECT_NAME` | Nome do projeto para isolamento dos containers no Docker Compose (padrão: `asterisk-ai-voice-agent`). |
| `ASTERISK_HOST` | Endereço IP ou hostname do servidor Asterisk para conexão ARI (ex.: `127.0.0.1`). |
| `ASTERISK_ARI_PORT` | Porta de comunicação da API ARI do Asterisk (padrão: `8088`). |
| `ASTERISK_ARI_USERNAME` | Nome de usuário configurado no Asterisk para autenticação no ARI. |
| `ASTERISK_ARI_PASSWORD` | Senha de autenticação do usuário ARI. |
| `ASTERISK_UID` / `ASTERISK_GID` | UID e GID do usuário Asterisk no host para alinhamento de permissões de arquivos de mídia gerados. |
| `GOOGLE_API_KEY` | Chave de API para o provedor Google Gemini Live (`google_live`). |
| `GOOGLE_APPLICATION_CREDENTIALS` | Caminho do arquivo JSON de credenciais de Service Account Google Cloud/Vertex AI (opcional). |
| `JWT_SECRET` | Chave secreta usada pelo backend da Admin UI para assinatura de tokens de sessão. |
| `UVICORN_PORT` | Porta HTTP da interface administrativa web Admin UI (padrão: `3003`). |
| `HEALTH_BIND_HOST` / `HEALTH_BIND_PORT` | Endereço e porta de escuta do endpoint de saúde do motor (padrão: porta `15000`). |
| `LOG_LEVEL` | Nível de detalhamento dos logs do motor (`debug`, `info`, `warning`, `error`). |
| `CALL_HISTORY_ENABLED` | Habilita o registro e persistência do histórico de chamadas (`true` ou `false`). |
| `CALL_HISTORY_DB_PATH` | Caminho do banco SQLite do histórico de chamadas (padrão: `data/call_history.db`). |

---

## 🚀 Como instalar e rodar localmente

### 1. Clonar e configurar o ambiente

Execute o assistente de pré-checagem para validar o ambiente e gerar o `.env`:

```bash
# Executar pré-checagem com correção automática de permissões e geração de chaves
./preflight.sh --apply-fixes
```

Ou configure manualmente a partir do modelo de exemplo:

```bash
cp .env.example .env
# Em seguida, edite o .env configurando o ASTERISK_HOST, ARI e GOOGLE_API_KEY
```

### 2. Iniciar os serviços via Docker Compose

```bash
# Iniciar os containers principais (ai_engine e admin_ui) em segundo plano
docker compose up -d

# Ou via Makefile
make up
```

### 3. Acessar a Interface Administrativa (Admin UI)

Abra no navegador:
- **URL local:** `http://localhost:3003`
- **Login padrão inicial:** `admin` / `admin` *(altere a senha imediatamente no primeiro acesso)*

Pelo painel é possível auditar a conexão com o ARI, visualizar o status dos serviços e acompanhar os logs.

### 4. Verificar o status e saúde do motor

```bash
# Consultar o endpoint de saúde do ai_engine
curl http://localhost:15000/health
# Resposta esperada: {"status":"healthy"}

# Acompanhar logs em tempo real
docker compose logs -f ai_engine
```

### 5. Encerrar os serviços

```bash
docker compose down
# Ou via Makefile
make down
```

---

## 📜 Scripts e Comandos Disponíveis

O projeto inclui atalhos de automação via `Makefile` e scripts de apoio na pasta `scripts/`:

### Makefile

- `make build`: Constrói ou reconstrói as imagens Docker dos serviços.
- `make up`: Inicia os containers em segundo plano.
- `make down`: Para e remove os containers (`--remove-orphans`).
- `make logs`: Exibe e acompanha os logs do serviço `ai_engine` (use `make logs-all` para todos).
- `make engine-reload`: Reinicia o container `ai_engine` para aplicar novas configurações.
- `make ps`: Exibe o estado e portas dos serviços em execução.
- `make test-health`: Valida o endpoint de saúde `http://127.0.0.1:15000/health`.
- `make test-local`: Executa os testes nos containers `ai_engine` e `local_ai_server`.
- `make provider-switch provider=<nome>`: Atualiza o provedor padrão no arquivo `config/ai-agent.yaml`.

### Scripts utilitários

- `./preflight.sh`: Valida compatibilidade do sistema operacional, dependências Docker e permissões do Asterisk.
- `./install.sh`: Assistente interativo para setup guiado do sistema.
- `scripts/switch_provider.py`: Utilitário para troca programática de provedor na configuração.
- `scripts/validate_externalmedia_config.py`: Valida configurações de ExternalMedia e RTP.
- `scripts/test_externalmedia_call.py`: Executa teste de fluxo de chamada via endpoint `/health`.
- `scripts/capture_test_logs.py` e `scripts/analyze_logs.py`: Captura e análise de logs estruturados de chamadas.

---

## 🧪 Testes e Verificação

- **Verificação rápida de integridade:**
  ```bash
  make test-health
  # Ou: curl http://localhost:15000/health
  ```
- **Execução de testes locais em container:**
  ```bash
  make test-local
  make test-integration
  ```
- **Teste com chamada real:**
  Conecte o softphone MicroSIP ao Asterisk (ramal `100`), ligue para a aplicação Stasis configurada no dialplan (`asterisk-ai-voice-agent`) e valide a saudação em português e a resposta do Google Gemini Live.

---

## 📂 Estrutura de Pastas

```text
.
├── admin_ui/         # Interface web administrativa (FastAPI + React/Vite) na porta 3003
├── assets/           # Imagens, diagramas e banners do projeto
├── asterisk_media/   # Diretório compartilhado para gravações e áudios gerados
├── config/           # Configurações do agente (ai-agent.yaml, perfis e baselines)
├── data/             # Persistência local (banco SQLite call_history.db e modelos customizados)
├── docs/             # Guias completos de instalação, arquitetura, provedores e operações
├── local_ai_server/  # Servidor opcional para modelos de IA locais (Vosk, Piper, llama.cpp, etc.)
├── scripts/          # Utilitários de setup, migração, diagnóstico e análise de logs
├── src/              # Código-fonte Python do motor principal (ARI, AudioSocket, sessões e agentes)
├── Dockerfile        # Imagem Docker do motor principal (ai_engine)
├── docker-compose.yml# Orquestração dos containers (ai_engine, admin_ui, local_ai_server)
├── Makefile          # Automação de comandos de build, execução, logs e testes
├── preflight.sh      # Script de verificação e preparação do ambiente
└── install.sh        # Script de instalação e configuração inicial
```

---

## 📚 Documentação completa (projeto original)

Toda a documentação técnica detalhada (instalação, provedores, pipelines, CLI) está **preservada**:

- 📖 **[README.upstream.md](README.upstream.md)** — README completo do projeto original
- 📁 **[docs/](docs/)** — guias de instalação, configuração e integração (ex.: `INSTALLATION.md`, `Configuration-Reference.md`)

---

## 🙏 Créditos e licença

Este projeto é baseado no excelente trabalho open-source:

> **[AVA — AI Voice Agent for Asterisk](https://github.com/hkjarral/AVA-AI-Voice-Agent-for-Asterisk)** — por **hkjarral** e colaboradores.

Licenciado sob a **Licença MIT** — © 2025 *Asterisk AI Voice Agent contributors*. Consulte o arquivo [LICENSE](LICENSE).
As customizações deste fork (configuração PT-BR, integração e ajustes operacionais) são de **odavid062**.

---

<div align="center">

Feito com ☕ e 🤖 no Brasil • por [**@odavid062**](https://github.com/odavid062)

</div>
