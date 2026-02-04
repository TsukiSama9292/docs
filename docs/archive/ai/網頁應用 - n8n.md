# 網頁應用 - n8n

建立時間: 2026年2月3日 下午7:51
類別: AI, Web APP
最後更新時間: 2026年2月3日 下午7:51

# [n8n](https://github.com/n8n-io/n8n)

## 什麼是 n8n？

n8n 是一款開源工作流程自動化平台，結合無程式碼拖拉式設計與程式碼擴展彈性。使用者能快速串接各種服務與 API，自動化繁瑣且重複的工作。它同時支援在節點中撰寫 JavaScript 或 Python，並可引入 npm 套件，適合從初學者到專業開發團隊使用。

## 核心特點

- **視覺化＋程式碼雙軌設計**：拖拉節點快速搭建流程，複雜邏輯可用程式碼實現。
- **原生 AI 與 LangChain 支援**：無縫串接大型語言模型與知識庫，打造智慧客服、文件處理等應用。
- **完全自我部署**：支援本地、私有雲部署，保障資料自主與安全。
- **企業級安全管理**：權限控管、單一登入（SSO）、空氣隔離部署等功能。
- **豐富整合生態**：400+ 節點涵蓋 API、資料庫、雲端服務、社群媒體等，並支持自訂節點開發。

## 快速上手

- **本地快速啟動**：安裝 Node.js 後執行 `npx n8n`，在瀏覽器訪問 [[http://localhost:5678](http://localhost:5678/)。]([http://localhost:5678](http://localhost:5678/)。)
- **Docker 部署推薦**：

```bash
docker volume create n8n_data
docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

建議搭配反向代理與 HTTPS 部署於生產環境。

## 常見應用場景

- **資料自動整合**：跨系統同步資料，訂單通知，自動化付款與物流流程。
- **智能客服**：結合 LangChain 打造多輪對話與自動分類問題。
- **DevOps 自動化**：CI/CD 流程自動化、系統監控與告警通知。
- **行銷活動管理**：自動發送郵件、社群發布與數據追蹤分析。

## 社群與資源

- 官方論壇：[community.n8n.io](https://community.n8n.io/)
- 文件與範例：[docs.n8n.io](https://docs.n8n.io/)
- 上千套工作流程模板，快速套用與學習。

## 授權與商業模式

n8n 採公平原始碼授權，支持自由部署與自訂開發。企業版提供更多安全與支援功能。

## 額外參考文獻

[N8N 繁體中文筆記](https://www.darrelltw.com/n8n-tutorial-resources/)