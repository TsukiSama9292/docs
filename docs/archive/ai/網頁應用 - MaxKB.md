# 網頁應用 - MaxKB

# [MaxKB](https://github.com/1Panel-dev/MaxKB) — 開源企業級智慧代理平台

**MaxKB**（Max Knowledge Brain）是一款開源的企業級智能體（Agent）平台，整合了 **檢索增強生成（Retrieval-Augmented Generation, RAG）** 流程、強大的工作流引擎，以及 **MCP 工具調用** 能力，適用於智慧客服、企業內部知識庫、學術研究與教育等多種場景。
其設計理念是透過可擴展、模型無關的架構，協助企業與開發者快速構建具備多模態、可整合、低幻覺率的 AI 系統，並且能零程式碼接入現有業務系統。

## 核心特色

### 1. **RAG 知識檢索增強**

- 支援直接上傳文件或自動爬取網頁內容
- 內建自動文本切分與向量化功能
- 透過知識檢索降低大型語言模型的幻覺現象，提升問答準確度
- 適用於知識庫問答、專業知識查詢等應用

### 2. **智能體工作流（Agentic Workflow）**

- 具備可視化工作流引擎與函式庫
- 支援 MCP（Model Context Protocol）工具使用
- 能靈活編排 AI 任務流程，處理複雜業務邏輯
- 適合需要多步推理、外部 API 調用的應用場景

### 3. **無縫整合**

- 提供零程式碼快速對接第三方業務系統
- 讓既有系統具備即時智能問答能力
- 快速部署到企業內部或雲端

### 4. **模型無關性（Model-Agnostic）**

- 支援多種大型語言模型，包括：
    - 私有部署模型：DeepSeek、Llama、Qwen 等
    - 公有 API 模型：OpenAI、Claude、Gemini 等
- 使用者可依需求靈活切換模型

### 5. **多模態支援**

- 原生支援文字、圖片、音訊與影片的輸入輸出
- 能滿足跨媒體的 AI 互動需求

## 技術架構

- **前端**：[Vue.js](https://vuejs.org/)
- **後端**：[Python / Django](https://www.djangoproject.com/)
- **LLM 框架**：[LangChain](https://www.langchain.com/)
- **資料庫**：[PostgreSQL + pgvector](https://www.postgresql.org/)

## 快速安裝（Docker）

執行以下指令即可快速啟動 MaxKB 容器：

```bash
docker run -d --name=maxkb --restart=always \\
  -p 8080:8080 \\
  -v ~/.maxkb:/opt/maxkb \\
  1panel/maxkb

```

預設登入資訊：

- 帳號：`admin`
- 密碼：`MaxKB@123..`
Web 介面網址：`http://<伺服器IP>:8080`

## 應用場景

- **智慧客服**：快速回應客戶問題，減少人力成本
- **企業知識庫**：集中管理內部文件，員工可快速查詢
- **教育與研究**：作為教學 AI 助手或資料檢索工具
- **多步推理業務流程**：自動化任務分配與資料處理

## 授權協議

MaxKB 採用 **GNU GPLv3** 授權，允許自由使用與修改，但需遵循相同授權條款。