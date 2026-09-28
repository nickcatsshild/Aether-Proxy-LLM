<div align="center">
  <img src="./images/logo.png" alt="Aether Proxy LLM" width="600"/>

# Aether Proxy LLM

  **Gateway Universal de IA Autônomo, Servidor LLM Local e Proxy de Alta Performance**  
  *Compatibilidade Nativa OpenAI (`/v1/*`) e Ollama (`/api/*`) com Resiliência Avançada e Isolamento por Planos*

  [![GitHub release](https://img.shields.io/github/v/release/nickcatsshild/Aether-Proxy-LLM?style=flat-square)](https://github.com/nickcatsshild/Aether-Proxy-LLM/releases)
  [![Download Windows Release](https://img.shields.io/badge/download-Aether--Windows--Release.zip-blue?logo=windows&style=flat-square)](https://github.com/nickcatsshild/Aether-Proxy-LLM/releases/latest)
  [![Docker Pulls](https://img.shields.io/docker/pulls/catsavengers/aether-proxy-llm.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/catsavengers/aether-proxy-llm)
  [![Licença](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)

  [🚀 Download Windows](#-download-rápido-windows-x64) • [⚡ Recursos](#-principais-recursos) • [🧠 Provedores](#-provedores-e-modelos) • [🛠 Como Usar](#-como-usar-nas-suas-ferramentas) • [🐳 Docker](#-docker--casaos)
</div>

---

### 🚀 Download Rápido (Windows x64)

Ideal para desenvolvedores que querem rodar o Aether localmente sem necessidade de Node.js, Python ou Docker:

1. Baixe o pacote oficial **[Aether-Windows-Release.zip](https://github.com/nickcatsshild/Aether-Proxy-LLM/releases/latest)** na aba **[Releases](https://github.com/nickcatsshild/Aether-Proxy-LLM/releases)**.
2. **IMPORTANTE:** Extraia o arquivo `.zip` para uma pasta de sua escolha (ex: `C:\Aether` ou `D:\Aether`).
3. Dê dois cliques em **`Aether.exe`** para iniciar.
4. O servidor iniciará automaticamente e o painel de controle abrirá em: **`http://localhost:20128`** *(se solicitado pelo Windows, permita o acesso na rede local)*.

---
## 📑 Sumário

- [Visão Geral](#-visão-geral)
- [Principais Recursos](#-principais-recursos)
- [Arquitetura do Sistema](#-arquitetura-do-sistema)
- [Jornada de Configuração](#-jornada-de-configuração-recomendada)
- [Modelos Locais GGUF & Hardware](#-modelos-locais-gguf--aceleração-de-hardware)
- [Combos & Resiliência](#-combos--cadeias-de-resiliência-failover)
- [Planos & Modelos Virtuais Personalizados](#-planos--modelos-virtuais-personalizados)
- [Memória Contínua (ai-memory)](#-memória-contínua-ai-memory)
- [MCP Tools Hub](#-mcp-tools-hub-model-context-protocol)
- [Voz & Áudio Offline](#-voz--áudio-offline-tts--stt)
- [Suíte DeepSeek Harness & Token Saver](#-suíte-deepseek-harness--token-saver)
- [Endpoints da API](#-endpoints-da-api)
- [Como Usar nas suas Ferramentas](#-como-usar-nas-suas-ferramentas)
- [Docker & CasaOS](#-docker--casaos)
- [Compilação & Build Portável](#-compilação--build-portável)
- [Segurança & Boas Práticas](#-segurança--boas-práticas)

---

## 🌟 Visão Geral

O **Aether Proxy LLM Core** é uma plataforma corporativa completa para orquestração, governança e inferência de Modelos de Linguagem (LLMs). Ele une o melhor de dois mundos:

1. **Roteamento em Nuvem Multi-Provedores**: Suporte transparente a dezenas de provedores (OpenAI, Anthropic, Google Gemini, Groq, Nvidia NIM, Z-AI, OpenCode, DeepSeek, Cerebras, Cohere, Together, etc.) com pooling de contas, rotação automática, balanceamento de carga e failover automático contra erros de cota (HTTP 429) e quedas (HTTP 502/504).
2. **Servidor Local LLM Autônomo**: Motor `llama-server` nativo com aceleração de hardware (Vulkan / GPU NVIDIA / AMD / Intel / CPU), auto-cálculo de VRAM e suporte plug-and-play a modelos `.gguf` no disco.
3. **Camada de Planos & IAs Fictícias**: Exponha nomes personalizados de modelos (ex: `jarvis-coder`, `glm-5.3-turbo`) para seus clientes (OpenCode, Cursor, Continue, Obsidian), ocultando os modelos e chaves reais com isolamento estrito.

---

## 🚀 Principais Recursos

- **Compatibilidade Universal**:
  - **OpenAI Standard**: `/v1/chat/completions`, `/v1/models`, `/v1/audio/speech`, `/v1/audio/transcriptions`, `/v1/embeddings`.
  - **Ollama Standard**: `/api/tags`, `/api/version`, `/api/chat`, `/api/generate`, `/api/show`.
- **Nomes Personalizados de Modelos**:
  - Crie modelos virtuais com o nome que desejar. Clientes autenticados enxergam apenas as IAs configuradas no plano.
  - Normalização inteligente: suporta sufixos `:latest` e nomes livres sem erros 404/403.
- **Resiliência Extrema**:
  - Cadeias de failover com prioridades (`P1`, `P2`, `P3`), health check canário em tempo real e simulação de quedas.
  - Estratégias de Combo: *Fallback*, *Round-Robin* e *Fusion* (múltiplas IAs com modelo juiz).
  - *Capacity Adapters*: Detecção automática de modalidades (se a mensagem contiver imagens e o modelo principal for texto puro, redireciona dinamicamente para um modelo multimodal com visão).
- **Provedores Gratuitos e Híbridos**:
  - OpenCode Zen / Free com assinatura de ferramentas canônicas e cabeçalhos oficiais.
  - Z-AI GLM-5.3 e Nvidia NIM com suporte a modelos de alta performance a custo zero.
- **Suíte DeepSeek Harness (Economia de 50% a 90% de Tokens)**:
  - *Tool Result Pruner*: Poda saídas gigantescas de comandos antigos preservando o histórico recente.
  - *Repeat Tool Guard*: Bloqueador de loops infinitos de chamadas de ferramentas.
  - *Image Offload*: Converte imagens base64 antigas em metadados leves evitando HTTP 413.
  - *Prefix Cache Alignment*: Alinhamento de blocos de contexto para obter até 90% de desconto no cache da DeepSeek/Anthropic.
  - *Auto-Compaction*: Sumarização automática de históricos massivos (> 60k tokens).
- **Memória Contínua (ai-memory 2.4.1)**:
  - Motor compilado em Rust nativo para persistência de conhecimento contínuo em formato Karpathy-wiki Markdown com busca FTS e exportação para Fine-Tuning SFT (CSV).
- **Hub MCP (Model Context Protocol)**:
  - Inicialize servidores MCP externos (STDIO/SSE) e converta suas ferramentas dinamicamente para o padrão `tools` da OpenAI.
- **Voz & Áudio Offline**:
  - TTS offline nativo (Windows SAPI / Piper) com latência inferior a 150ms.
  - STT offline com Whisper local.
- **Interface Visual Moderna**:
  - Dashboard intuitivo e responsivo em Next.js com temas escuros premium, gráficos em tempo real, inspetor de requisições e telemetria de hardware.
  - Aplicativo Desktop integrado para Windows via WebView2 (.NET 8).

---

## 🏛 Arquitetura do Sistema

```
                        [ Clientes Externos ]
       OpenCode · Cursor · Continue · Obsidian · Scripts · CLI
                                 │
                     ┌───────────┴───────────┐
                     ▼                       ▼
            OpenAI API (/v1/*)      Ollama API (/api/*)
                     │                       │
                     └───────────┬───────────┘
                                 ▼
                 [ Aether Security & Plan Guard ]
             Autenticação SQLite · Cotas · Planos Virtuais
                                 │
                     ┌───────────┴───────────┐
                     ▼                       ▼
           [ Roteador de Combos ]    [ DeepSeek Harness ]
            Fallback / Round-Robin   Pruner · Repeat Guard
            Fusion / Capacity Adapt   Cache Alignment · RTK
                     │                       │
         ┌───────────┴───────────┐           │
         ▼                       ▼           ▼
[ Llama Server Local ]  [ Provedores Nuvem ]  [ Extensões ]
- Vulkan / GPU / CPU    - OpenAI / Claude     - ai-memory
- Modelos GGUF          - Gemini / Groq       - MCP Tools Hub
- Detecção de Hardware  - OpenCode / Z-AI     - Voz / TTS / STT
- Auto-NGL              - DeepSeek / Nvidia   - DuckDuckGo Search
```

---

## 🧭 Jornada de Configuração Recomendada

Os menus da Sidebar foram estruturados exatamente na ordem lógica do fluxo de implantação:

1. **Providers** (`/dashboard/providers`): Cadastre suas chaves de API de nuvem ou ative provedores gratuitos (OpenCode, Nvidia, Groq).
2. **Local LLMs** (`/dashboard/local-models`): Detecte sua GPU, configure camadas de VRAM (auto-NGL) e carregue modelos GGUF locais.
3. **Combo & Vision Adapter** (`/dashboard/combos`): Agrupe modelos em combos de execução com estratégias de contingência.
4. **Failover Chains** (`/dashboard/failover-chains`): Configure cadeias de alta disponibilidade com nós de prioridade P1, P2 e P3.
5. **Plans (Virtual Models)** (`/dashboard/plans`): Crie planos atribuindo nomes personalizados de IAs aos combos e configurando limites de cotas.
6. **Virtual Keys** (`/dashboard/virtual-keys`): Crie chaves de API vinculadas aos planos para entregar aos clientes ou membros da equipe.
7. **Endpoint & Keys** (`/dashboard/endpoint`): Obtenha a URL base (`http://localhost:20128/v1`), configure a Chave Mestra e ative a **Exigência de Chave de API** (`Require API Key`).

---

## 🖥 Modelos Locais GGUF & Aceleração de Hardware

O Aether possui um subsistema autônomo para gerenciamento do `llama-server`:

- **Detecção Real de GPU**: Suporte a GPUs NVIDIA (CUDA / nvidia-smi), AMD Radeon (aceleração Vulkan nativa), Intel e CPU.
- **Calculadora Dinâmica de VRAM**: Avalia o overhead do KV Cache, tamanho do contexto (`ctx_size`), batch paralelo e reserva do SO para calcular o número ideal de camadas (`ngl`), impedindo erros de Out-Of-Memory (OOM).
- **Explorador de Pastas com Breadcrumb**: Navegue em qualquer disco (`C:\`, `D:\`, `G:\`) e selecione arquivos `.gguf` diretamente.
- **Varredura Automática**: Detecta automaticamente centenas de modelos em pastas comuns como `G:\Modelos`, `G:\Bionic-Modelos`, `D:\Modelos` e `data/models`.
- **Controle de Ciclo de Vida**: Inicialização, parada, reinicialização rápida, ping de latência e visualização de logs em tempo real pelo painel.

---

## 🛡 Combos & Cadeias de Resiliência (Failover)

- **Estratégia Fallback**: Tenta o modelo principal; se houver erro (HTTP 429, 500, 502, 504), chave esgotada ou timeout, salta imediatamente para o próximo modelo sem perder o contexto do usuário.
- **Estratégia Round-Robin**: Alterna uniformemente as requisições entre os modelos do combo, ideal para distribuir carga em contas gratuitas com limites de requisições por minuto.
- **Estratégia Fusion**: Executa o prompt em múltiplos modelos simultaneamente e utiliza um modelo juiz para selecionar e enriquecer a melhor resposta.
- **Capacity Adapters**: Se uma requisição multimodal contendo imagens for enviada a um modelo que suporta apenas texto, o Aether remaneja a chamada instantaneamente para o nó com visão do combo.

---

## 🎭 Planos & Modelos Virtuais Personalizados

O Aether permite criar sua própria oferta de modelos de IA com isolamento total:

- **Nomes Customizados**: Defina nomes como `jarvis-coder`, `glm-5.3-turbo` ou `ia-empresa-dev` no campo de **Modelos Virtuais**.
- **Isolamento `/v1/models`**: Quando um cliente autentica com uma chave atrelada ao plano, ele enxerga **somente** os modelos que você definiu. Modelos reais e provedores subjacentes ficam 100% ocultos.
- **Roteamento Inteligente**:
  - Aceita chamadas pelo nome do modelo virtual.
  - Aceita chamadas com sufixo `:latest` (padrão de muitos editores).
  - Aceita chamadas com o nome do plano.
  - **Fallback Permissivo**: Se o desenvolvedor configurar no OpenCode qualquer nome livre, o sistema roteia com sucesso para o combo do plano ativo em vez de recusar com 404/403.

---

## 🧠 Memória Contínua (ai-memory)

- Integrado ao core em Rust nativo (`ai-memory.exe`, porta `49374`).
- Guarda decisões arquiteturais, conceitos, regras e gotchas do projeto em arquivos Markdown padronizados (`Karpathy-wiki`).
- Busca semântica e Full-Text Search (FTS).
- **Bridge com Fine-Tuning**: Exporta as memórias acumuladas em formato RFC4180 CSV pronto para treinamento SFT (ForgeTrain, Axolotl, Unsloth, H2O LLM Studio).

---

## 🔌 MCP Tools Hub (Model Context Protocol)

- Cliente nativo JSON-RPC 2.0 integrado para servidores de ferramentas no padrão da Anthropic.
- Conecte servidores MCP via comando local (`STDIO`) ou endpoint remoto (`SSE`).
- O Aether traduz automaticamente as ferramentas MCP registradas para a especificação `tools` da OpenAI, permitindo que qualquer modelo (local ou nuvem) use ferramentas de filesystem, fetch, git e banco de dados.

---

## 🎙 Voz & Áudio Offline (TTS & STT)

- **Síntese Vocal Offline (TTS)**:
  - Baseado no Windows Speech/SAPI e Piper local.
  - Gera áudios WAV 16-bit 22kHz com latência inferior a 150ms sem chamadas de rede.
  - Rota padrão: `POST /v1/audio/speech`.
- **Transcrição de Áudio (STT)**:
  - Transcrição precisa via Whisper local.
  - Rota padrão: `POST /v1/audio/transcriptions`.

---

## ⚡ Suíte DeepSeek Harness & Token Saver

Otimizações de ponta para reduzir drasticamente o consumo de tokens e evitar estouro de contexto em sessões longas de desenvolvimento:

| Recurso | Descrição | Economia / Benefício |
| :--- | :--- | :--- |
| **Tool Result Pruner** | Poda saídas massivas de comandos antigos no histórico preservando os turnos recentes | 50% a 90% dos tokens de input |
| **Repeat Tool Guard** | Detecta chamadas idênticas consecutivas de ferramentas e injeta orientação de parada | Impede loops e gastos descontrolados |
| **Image Offload** | Remove imagens base64 de turnos antigos convertendo em metadados descritivos | Elimina erros HTTP 413 (Payload Too Large) |
| **Prefix Cache Alignment** | Organiza blocos de sistema e ferramentas no formato aceito pelo Prompt Caching | Até 90% de desconto via DeepSeek / Anthropic |
| **Auto-Compaction** | Condensa históricos muito extensos (>60k tokens) em sumários estruturados | Elimina erros HTTP 400 (`context_length`) |
| **RTK (Rust Tool Kit)** | Reduz verbosidade de comandos git, ls, grep e logs | 60% a 80% menos tokens em ferramentas |

---

## 📡 Endpoints da API

### OpenAI Compatible (`http://localhost:20128/v1`)
- `POST /v1/chat/completions` — Inferência de chat com streaming SSE ou resposta agregada.
- `GET /v1/models` — Listagem de modelos (filtrada por plano do usuário).
- `POST /v1/audio/speech` — Síntese de voz offline (TTS).
- `POST /v1/audio/transcriptions` — Transcrição de fala para texto (STT).
- `POST /v1/embeddings` — Vetorização de texto.

### Ollama Compatible (`http://localhost:20128/api`)
- `GET /api/tags` — Lista de modelos locais e virtuais no formato Ollama.
- `GET /api/version` — Versão do motor Ollama.
- `POST /api/chat` — Chat compatível com Ollama (streaming NDJSON).
- `POST /api/generate` — Conclusão de texto compatível com Ollama.
- `POST /api/show` — Metadados detalhados de modelo.

---

## 📦 Versões e Distribuição

O Aether é distribuído como um pacote portátil pré-compilado para Windows x64:

- **`Aether.exe`**: Executável launcher desktop integrado com WebView2 (.NET 8).
- **`AetherProxy.exe`**: Servidor de alta performance embutido.
- **Portabilidade Total**: Baixe o `Aether-Windows-Release.zip` na aba [Releases](https://github.com/nickcatsshild/Aether-Proxy-LLM/releases/latest), extraia e use imediatamente. Não deixa rastros no registro do sistema.

---


## 🛠 Como Usar nas suas Ferramentas

O Aether emula com fidelidade absoluta as APIs da **OpenAI** e do **Ollama**, sendo plug-and-play em qualquer ferramenta de desenvolvimento:

| Ferramenta | Como Configurar | Endpoint | Modelo |
| :--- | :--- | :--- | :--- |
| **OpenCode** | `opencode.json` ou Configurações do Provedor | `http://localhost:20128/v1` | Nome do Modelo Virtual ou Combo |
| **Cursor IDE** | `Settings → Models → Override OpenAI Base URL` | `http://localhost:20128/v1` | Nome do Modelo Virtual ou Combo |
| **Claude Code** | Definir `anthropic_api_base` em `~/.claude/config.json` | `http://localhost:20128/v1` | Nome do Modelo Virtual |
| **Cline / Roo Code** | Provedor OpenAI Compatible / Ollama | `http://localhost:20128/v1` | Nome do Modelo Virtual |
| **Continue (VS Code / JetBrains)** | `config.json` com provider `openai` | `http://localhost:20128/v1` | Nome do Modelo Virtual |
| **Obsidian** (Copilot / Smart Connections) | Custom OpenAI Endpoint | `http://localhost:20128/v1` | Nome do Modelo Virtual |

> 🔑 **Chave de API**: Utilize uma chave virtual gerada no painel (**Virtual Keys**) vinculada ao plano desejado, ou a Chave Mestra configurada em **Endpoint & Keys**.

---

## 🐳 Docker & CasaOS

Para servidores locais, VPS ou NAS (Synology, CasaOS, Unraid):

```yaml
version: "3.8"
services:
  aether-proxy-llm:
    image: catsavengers/aether-proxy-llm:latest
    container_name: aether-proxy-llm
    ports:
      - "20128:20128"
    volumes:
      - /DATA/AppData/aether/data:/app/data
    environment:
      - DATA_DIR=/app/data
      - PORT=20128
      - HOSTNAME=0.0.0.0
    restart: unless-stopped
```

---

## 🔒 Segurança & Boas Práticas

- **Controle de Acesso Soberano**: A ativação da exigência de chave de API (`Require API Key`) é gravada no banco SQLite e tem precedência sobre qualquer valor estático de variáveis de ambiente.
- **Proteção contra SSRF**: Validação rigorosa em endpoints de proxy e conexões externas.
- **Zero-Fake Data**: 100% das métricas exibidas nos painéis (custos, latências, quotas e SLAs) são extraídas diretamente do banco de dados em tempo real.
- **Zero Vazamento de Credenciais**: As chaves dos provedores de nuvem ficam seguras no servidor local; os clientes externos utilizam apenas chaves virtuais geradas pelo Aether.

---

<div align="center">

Desenvolvido para máxima soberania, resiliência e inteligência em ambientes locais e corporativos.

</div>
