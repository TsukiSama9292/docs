# Ubuntu 桌面 - 推薦軟體

建立時間: 2026年2月3日 下午7:51
類別: Linux
最後更新時間: 2026年2月3日 下午7:51

# 安裝 VSCode

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/
rm packages.microsoft.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/packages.microsoft.gpg] <https://packages.microsoft.com/repos/code> stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update
sudo apt install code -y
```

# 安裝 DBeaver

```bash
sudo mkdir -p /etc/apt/trusted.gpg.d
curl -fsSL https://dbeaver.io/debs/dbeaver.gpg.key \\
  | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/dbeaver.gpg
echo "deb [signed-by=/etc/apt/trusted.gpg.d/dbeaver.gpg] <https://dbeaver.io/debs/dbeaver-ce> /" \\
  | sudo tee /etc/apt/sources.list.d/dbeaver.list
sudo apt update
sudo apt install dbeaver-ce -y
```

# 安裝 uv

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```