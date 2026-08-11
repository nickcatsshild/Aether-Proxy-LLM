# Docker

Execute o Aether Proxy LLM em um container. Imagem publicada: [`nickcatsshild/aether-proxy-llm`](https://hub.docker.com/r/nickcatsshild/aether-proxy-llm) — multi-platform `linux/amd64` + `linux/arm64`.

---

# 👤 Para Usuários

## Início Rápido

```bash
docker run -d \
  -p 20128:20128 \
  -v "$HOME/.aether-proxy-llm:/app/data" \
  -e DATA_DIR=/app/data \
  --name aether-proxy-llm \
  nickcatsshild/aether-proxy-llm:latest
```

A aplicação escuta na porta `20128`. Abra: http://localhost:20128

## Gerenciar o Container

```bash
docker logs -f aether-proxy-llm        # visualizar logs
docker stop aether-proxy-llm           # parar
docker start aether-proxy-llm          # iniciar novamente
docker rm -f aether-proxy-llm          # remover
```

## Persistência de Dados

```bash
-v "$HOME/.aether-proxy-llm:/app/data" \
-e DATA_DIR=/app/data
```

Sem `DATA_DIR`, a aplicação usa `~/.aether-proxy-llm/` (macOS/Linux) ou `%APPDATA%\aether-proxy-llm\` (Windows). No container, `DATA_DIR=/app/data` faz o bind mount funcionar.

Estrutura de dados em `$DATA_DIR/`:

```text
$DATA_DIR/
├── db/
│   ├── data.sqlite       # banco de dados SQLite principal
│   └── backups/          # backups automáticos
└── ...                   # certificados, logs, configs de runtime
```

Caminho no host: `$HOME/.aether-proxy-llm/db/data.sqlite`
Caminho no container: `/app/data/db/data.sqlite`

## Variáveis de Ambiente Opcionais

```bash
docker run -d \
  -p 20128:20128 \
  -v "$HOME/.aether-proxy-llm:/app/data" \
  -e DATA_DIR=/app/data \
  -e PORT=20128 \
  -e HOSTNAME=0.0.0.0 \
  -e DEBUG=true \
  --name aether-proxy-llm \
  nickcatsshild/aether-proxy-llm:latest
```

## Headroom Sidecar Opcional

A imagem do Aether Proxy LLM não inclui Python ou Headroom. Para usar Headroom no Docker, execute-o como um serviço separado e aponte o Aether Proxy LLM para esse proxy:

```yaml
services:
  aether-proxy-llm:
    image: nickcatsshild/aether-proxy-llm:latest
    ports:
      - "20128:20128"
    volumes:
      - "$HOME/.aether-proxy-llm:/app/data"
    environment:
      DATA_DIR: /app/data
      HEADROOM_URL: http://headroom:8787
    depends_on:
      - headroom

  headroom:
    image: ghcr.io/chopratejas/headroom:latest
    ports:
      - "8787:8787"
```

No painel, abra `Endpoint` → `Token Saver` → `Headroom`, confirme que a URL é `http://headroom:8787`, verifique o status, depois habilite o Headroom.

Se o Headroom rodar no host Docker em vez de sidecar, use `http://host.docker.internal:8787` no macOS/Windows. No Linux, adicione `--add-host=host.docker.internal:host-gateway` ou a entrada `extra_hosts` equivalente no compose.

## Atualizar para a Última Versão

```bash
docker pull nickcatsshild/aether-proxy-llm:latest
docker rm -f aether-proxy-llm
# execute novamente o comando de início rápido
```

---

# 🛠 Para Desenvolvedores

## Construir a Imagem Localmente (Teste)

```bash
docker build -t aether-proxy-llm .

docker run --rm -p 20128:20128 \
  -v "$HOME/.aether-proxy-llm:/app/data" \
  -e DATA_DIR=/app/data \
  aether-proxy-llm
```

## Publicar (Automático via CI)

Envie uma tag `v*` → GitHub Actions constrói multi-platform (amd64+arm64) e envia para:
- `ghcr.io/nickcatsshild/aether-proxy-llm:v{version}` + `:latest`
- `nickcatsshild/aether-proxy-llm:v{version}` + `:latest`

```bash
# Use scripts/release.js (recomendado)
node scripts/release.js "Título do Release" "Notas"

# Ou manualmente
git tag v1.0.0 && git push origin v1.0.0
```

Workflow: `.github/workflows/docker-publish.yml`