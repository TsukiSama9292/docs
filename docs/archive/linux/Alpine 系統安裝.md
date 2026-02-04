# Alpine 系統安裝

建立時間: 2026年2月3日 下午7:51
類別: Linux
最後更新時間: 2026年2月3日 下午7:51

## 1. 下載最新鏡像

[官方網站](https://alpinelinux.org/downloads/)
鏡像大小：約 68 MB

## 2. 建立 VM，與其他 VM 相同邏輯

預設使用者：`root`
預設無密碼

## 3. 啟動系統安裝

```bash
setup-alpine
```

多數參數為預設就行

### 安裝中的注意事項

1. 記得改 `DNS` 的 nameserver(s)
2. （可選）新增 USER
3. 安裝完，記得關閉 VM，移除 ISO

## 4.啟動 VM 後，建議操作

1. 修改 SSH 設定
    - `PermitRootLogin yes`：允許 Root 登入
    - `PasswordAuthentication yes`：允許密碼登入
    - `AllowTcpForwarding yes`：允許反向代理 ( 尤其是 VSCode Remote SSH 需要 )
2. 修改 `/etc/apk/repositories`，把社群套件的註解移除掉
3. 安裝 sudo，`apk update && apk add sudo`
4. 修改 `/etc/sudoers.d/wheel`，加入 `%wheel ALL=(ALL) ALL`
5 `adduser username wheel` 加入群組
5. 推薦用 USER 執行

## 5. 安裝常用工具

```bash
# 常用工具
sudo apk add bash tar curl htop git

# (可選) 安裝 glibc 相容性套件
sudo apk add gcompat

# (可選) VSCode Remote SSH 依賴 (+ glibc 也要安裝)
sudo apk add libstdc++ libc6-compat

# (可選) QEMU 客機工具
sudo apk add qemu-guest-agent qemu-guest-agent-openrc
```

1. (可選) 使用 QEMU 客機 (僅在 VM 使用)
    
    ```bash
    sudo rc-update add qemu-guest-agent default
    sudo rc-service qemu-guest-agent start
    ```