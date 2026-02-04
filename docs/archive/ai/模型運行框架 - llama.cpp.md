# 模型運行框架 - llama.cpp

建立時間: 2026年2月3日 下午7:51
類別: AI
最後更新時間: 2026年2月3日 下午7:51

# LLM 模型服務

```bash
sudo nano /etc/systemd/system/llama_cpp_llm.service
```

## 模型設定檔

```
[Unit]
Description=llama.cpp running llm
After=network.target

[Service]
# 執行命令
ExecStart=./llama-server -hf unsloth/gpt-oss-20b-GGUF:F16 --ctx-size 131072 --jinja -b 4096 -ub 4096 -ngl 99 -fa --host 0.0.0.0 --port 8080

# 如果程式崩潰，自動重啟
Restart=always
RestartSec=5

# 設定執行身份
User=user
Group=user

# 指定工作目錄
WorkingDirectory=/home/user/workspace/llama.cpp/build/bin

# 輸出到 journalctl
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target

```

# Embedding 模型服務

```bash
sudo nano /etc/systemd/system/llama_cpp_embeddinggemma.service 
```

## 設定檔內容

```
[Unit]
Description=llama.cpp running embedding
After=network.target

[Service]
# 執行命令
ExecStart=./llama-server -hf ggml-org/embeddinggemma-300M-qat-q4_0-GGUF --embeddings --pooling last -ngl 1000 -fa --host 0.0.0.0 --port 8082

# 如果程式崩潰，自動重啟
Restart=always
RestartSec=5

# 設定執行身份
User=user
Group=user

# 指定工作目錄
WorkingDirectory=/home/user/workspace/llama.cpp/build/bin

# 輸出到 journalctl
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```