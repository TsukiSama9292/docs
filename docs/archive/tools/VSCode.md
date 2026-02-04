# VSCode

建立時間: 2026年2月3日 下午7:51
類別: 工具使用
最後更新時間: 2026年2月3日 下午7:51

## 1. 安裝 VSCode 本體

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/
rm packages.microsoft.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update
sudo apt install code -y
```

## 2. 安裝實用插件