# 模型運行框架 - Ollama

# [Ollama](https://github.com/ollama/ollama)

Ollama 是一個可以在本地端運行與管理大型語言模型（LLM）的開源工具，支援 macOS、Windows、Linux 以及 Docker 環境。它的特點是輕量化安裝、方便下載與切換模型，並且提供 REST API 與 CLI（命令列介面）來與模型互動。透過 Ollama，你可以快速在本地執行如 LLaMA、Gemma、GPT-OSS 等知名模型，離線運行以保護資料隱私，同時支援多種硬體加速（如 GPU）。

### 安裝方式

**macOS**

```bash
<https://ollama.com/download/Ollama.dmg>

```

**Windows**

```bash
<https://ollama.com/download/OllamaSetup.exe>

```

- [*Linux ** - 手動安裝說明](https://github.com/ollama/ollama/blob/main/docs/linux.md)

```bash
curl -fsSL <https://ollama.com/install.sh> | sh

```

[**Docker**](https://hub.docker.com/r/ollama/ollama)
CPU Only 範本 CLI

```bash
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama

```

### 常用 CLI 指令

| 功能 | 指令範例 |
| --- | --- |
| 執行模型 | `ollama run llama3.2` |
| 下載模型 | `ollama pull llama3.2` |
| 刪除模型 | `ollama rm llama3.2` |
| 複製模型 | `ollama cp llama3.2 my-model` |
| 查看模型資訊 | `ollama show llama3.2` |
| 列出本機模型 | `ollama list` |
| 列出已載入模型 | `ollama ps` |
| 停止運行中的模型 | `ollama stop llama3.2` |
| 啟動 Ollama 服務 | `ollama serve` |

### REST API 範例

**生成文字回應**

```bash
curl <http://localhost:11434/api/generate> -d '{
  "model": "llama3.2",
  "prompt": "Why is the sky blue?"
}'

```

**聊天模式**

```bash
curl <http://localhost:11434/api/chat> -d '{
  "model": "llama3.2",
  "messages": [
    { "role": "user", "content": "why is the sky blue?" }
  ]
}'

```

### [官方模型儲存庫](https://ollama.com/)

從此處可直接拉取模型，無須進匯入和自訂模型的操作，實現開箱級用
Ollama 官方會主動將熱門的開源大模型進行適配後立即釋出儲存庫
並且 Ollama 會根據 GPU 自主切換權重 `bit` (若模型支援)

```bash
ollama run gpt-oss

```

### 匯入與自訂模型

**匯入 GGUF 模型Modelfile**

```
FROM ./vicuna-33b.Q4_0.gguf

```

**CLI**

```
ollama create example -f Modelfile
ollama run example

```

**修改系統提示**

```docker
FROM llama3.2
PARAMETER temperature 1
SYSTEM """
You are Mario from Super Mario Bros.
"""
```

## docker-compose.yml - 在 Nvidia GPU 可參考的範本

```yaml
# 預設設定
x-default-opts: &default-opts
  restart: unless-stop # 每次開機重啟，不然每次開啟都會重啟
  tty: true            # TTY 允許
  stdin_open: true     # 標準模式啟動
  privileged: false    # 不允許特權
  ipc: private         # IPC 與主機不共用

# Docker Compose 啟用服務
services:
  ollama:   # ollama 服務
    image: ollama/ollama:latest      # 最新的 ollama Docker 鏡像
    container_name: ollama           # 容器名稱: ollama
    <<: *default-opts                # 採用預設設定
    ports:                           # Port 對應
      - "11434:11434"                # 主機 11434 對應 ollama 預設 port(11434)
    volumes:                         # 對應卷設定
      - ollama:/root/.ollama         # 採用建立的 ollama 對應卷到 /root/.ollama
    networks:                        # 網路設定
      - ollama_network               # 採用建立的 ollama_network 子網
    runtime: nvidia                  # 運行時宣告成 nvidia (官方有時候會建議這樣)
    deploy:                          # 修改 deploy 使用的資源，可設定 Nvidai GPU
      resources:
        reservations:
          devices:
          - driver: nvidia
            count: "all"
            capabilities: [gpu]

# 建立 Docker 網路
networks:
  ollama_network: # 不宣告就是 bridge 子網
# 建立對應卷 保存資料
volumes:
  ollama: {}      # {} 是不宣告採用預設的意思

```