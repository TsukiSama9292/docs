# GitHub Actions

# 安裝 Github Actions ( 務必直接操作 Github 網頁獲取最新版本和單次 token )

## 建立一個資料夾

```bash
mkdir github-actions
cd github-actions

```

## 下載並且解壓縮檔案

```bash
curl -o actions-runner-linux-x64-2.321.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.321.0/actions-runner-linux-x64-2.321.0.tar.gz
echo "ba46ba7ce3a4d7236b16fbe44419fb453bc08f866b24f04d549ec89f1722a29e  actions-runner-linux-x64-2.321.0.tar.gz" | shasum -a 256 -c
tar xzf ./actions-runner-linux-x64-2.321.0.tar.gz

```

## 進行授權認證

```bash
./config.sh --url https://github.com/k12edu --token BADCU6CSE2R3YDCBPP7AWQLHLP7OO

```

## 設定開機後自動啟動，這樣每次開機都會自動啟動服務

```bash
crontab -e

```

## 最後一行添加

```bash
@reboot <檔案路徑>/run.sh

```