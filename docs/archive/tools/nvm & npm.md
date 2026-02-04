# nvm & npm

建立時間: 2026年2月3日 下午7:51
類別: 工具使用
最後更新時間: 2026年2月3日 下午7:51

## 安裝 nvm (0.40.2) 和 npm ( 22 LTS)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash
export NVM_DIR="$HOME/.nvm" && \
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
nvm install node && \
nvm install 22 && \
nvm alias default 22
```