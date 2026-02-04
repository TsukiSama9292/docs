# Tailscale

# Tailscale 運行在 Docker Container (僅在有特權環境)

## docker-compose.yml

```
x-default-opts: &default-opts
  restart: unless-stopped
  tty: true
  stdin_open: true
  privileged: true
  ipc: private
  network_mode: "host"

services:
  tailscale:
    <<: *default-opts
    image: tailscale/tailscale:stable
    container_name: tailscale
    volumes:
      - /var/lib/tailscale:/var/lib/tailscale   # 持久化 Tailscale 狀態
      - /dev/net/tun:/dev/net/tun               # 允許使用 TUN 設備
    environment:
      TS_STATE_DIR: "/var/lib/tailscale"        # 持久化狀態目錄
      TS_USERSPACE: "0"                         # 使用內核模式（可支援 outgoing 連線）
    command: ["tailscaled", "--state=/var/lib/tailscale/tailscaled.state"]

```

## 操作

### 啟動容器

```bash
docker compose up -d

```

### 進入容器

```bash
docker exec -it tailscale sh

```

### 啟用登入

```bash
tailscale up

```