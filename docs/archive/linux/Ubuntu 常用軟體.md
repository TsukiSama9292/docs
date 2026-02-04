# Ubuntu 常用軟體

# VSCode

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/
rm packages.microsoft.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update
sudo apt install code -y
```

# DBeaver

```bash
sudo mkdir -p /etc/apt/trusted.gpg.d
curl -fsSL https://dbeaver.io/debs/dbeaver.gpg.key \\
  | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/dbeaver.gpg
echo "deb [signed-by=/etc/apt/trusted.gpg.d/dbeaver.gpg] https://dbeaver.io/debs/dbeaver-ce /" \\
  | sudo tee /etc/apt/sources.list.d/dbeaver.list
sudo apt update
sudo apt install dbeaver-ce -y
```

# Steam

```bash
wget -O steam.gpg https://repo.steampowered.com/steam/archive/stable/steam.gpg
cat steam.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/steam.gpg > /dev/null
rm steam.gpg
sudo tee /etc/apt/sources.list.d/steam-stable.list <<'EOF'
deb [arch=amd64,i386 signed-by=/usr/share/keyrings/steam.gpg] https://repo.steampowered.com/steam/ stable steam
deb-src [arch=amd64,i386 signed-by=/usr/share/keyrings/steam.gpg] https://repo.steampowered.com/steam/ stable steam
EOF
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install steam -y
```

# uv

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```