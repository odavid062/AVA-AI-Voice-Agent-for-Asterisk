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
