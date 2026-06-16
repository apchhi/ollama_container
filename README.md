Использованная схема системы:

* Arch Linux (host)
* fish shell
* Podman
* Distrobox (Ubuntu 22.04)
* Ollama
* локальные LLM для DevOps / coding

---

```markdown
# Local LLM DevOps Stack (Ollama + Podman + Distrobox)

Локальная среда для работы с LLM (DevOps, coding, architecture) на базе:

- Arch Linux (host)
- fish shell
- Podman (rootless containers)
- Distrobox (Ubuntu 22.04 runtime)
- Ollama (LLM server)

---

# 📦 Архитектура

```

Host (Arch + fish)
│
├── Podman
│     ├── Ollama container (LLM server)
│     └── Dev containers (Distrobox)
│
└── Browser / CLI / API

````

---

# 🚀 Установка Ollama (Podman)

## 1. Создать директорию для моделей

```bash
mkdir -p ~/.local/share/containers/llm/models
````

---

## 2. Запустить Ollama контейнер

```bash
podman run -d \
  --name ollama \
  --network host \
  -v ~/.local/share/containers/llm/models:/root/.ollama:Z \
  docker.io/ollama/ollama
```

---

## 3. Проверка сервера

```bash
curl http://localhost:11434/api/tags
```

---

# 📥 Установка моделей

## Скачать модель

```bash
podman exec -it ollama ollama pull qwen2.5-coder:7b
```

## Скачать архитектурную модель

```bash
podman exec -it ollama ollama pull llama3.1:8b
```

---

# 🧠 Доступные модели

## Coding / DevOps

* `qwen2.5-coder:7b` → код, Docker, Kubernetes, CI/CD, scripting

## Architecture / Reasoning

* `llama3.1:8b` → system design, архитектура, анализ

---

# 💬 Запуск моделей (CLI)

## Запуск coder модели

```bash
podman exec -it ollama ollama run qwen2.5-coder:7b
```

## Запуск architecture модели

```bash
podman exec -it ollama ollama run llama3.1:8b
```

Выход:

```
/bye
```

---

# ⚡ Лаконичные команды (fish shell)

## Алиасы для fish

Добавить в:

```bash
~/.config/fish/config.fish
```

### Ollama shortcuts

```fish
function llm
    podman exec -it ollama ollama run qwen2.5-coder:7b
end

function llm-arch
    podman exec -it ollama ollama run llama3.1:8b
end

function llm-list
    podman exec -it ollama ollama list
end

function llm-pull
    podman exec -it ollama ollama pull $argv
end
```

Перезагрузка:

```fish
source ~/.config/fish/config.fish
```

---

# 🔥 Использование

## Coding (DevOps)

```bash
llm
```

## Architecture / design

```bash
llm-arch
```

## Список моделей

```bash
llm-list
```

## Скачать модель

```bash
llm-pull llama3.1:8b
```

---

# 🧩 API (без CLI)

## Генерация ответа

```bash
curl http://localhost:11434/api/generate \
  -d '{
    "model": "qwen2.5-coder:7b",
    "prompt": "write docker compose for nginx + postgres"
  }'
```

---

# 🧹 Управление контейнером

## Остановить

```bash
podman stop ollama
```

## Запустить

```bash
podman start ollama
```

## Удалить контейнер

```bash
podman rm ollama
```

## Удалить модель

```bash
rm -rf ~/.local/share/containers/llm/models
```

---

# 🧠 DevOps workflow

## 1. Архитектура

```
llm-arch → проектирование системы
```

## 2. Реализация

```
llm → код, docker, k8s, terraform
```

---

# ⚠️ Примечания

* Модели занимают 3–8 GB каждая
* Первый `pull` всегда долгий
* После загрузки работают локально (offline)
* GPU используется автоматически (если доступен)

---

# 📌 Рекомендация

Минимальный стек:

* qwen2.5-coder:7b
* llama3.1:8b

```

---

Если хочешь дальше улучшить систему — можно добавить:

- Open WebUI (GUI как ChatGPT)
- Neovim интеграцию (Avante / Copilot-style)
- multi-model router (автовыбор coder/arch)
- GPU tuning (если у тебя NVIDIA/Intel/AMD)

Скажи — можно собрать тебе “production-grade local AI stack”.
```
