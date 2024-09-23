---
title: 整合 LobeChat 與 Ollama 以增強語言模型
description: >-
  學習如何配置和部署 LobeChat，以在地端利用 Ollama 的
  大語言模型。請按照以下程序，在您的系統上運行 Ollama 和 LobeChat。
tags:
  - Ollama
  - LobeChat
  - 邱彥敏
  - 的
  - old
  - 2
---

# 與 Ollama 的整合

Ollama簡介： 用於在本地運行大型語言模型（LLM）的框架，支持包括 Llama 2、Mistral 等多種語言模型。現在，LobeChat 支持與 Ollama 的整合，這意味著您可以輕鬆使用 Ollama 提供的語言模型來增強 LobeChat 中的應用程序。

以下將指導您如何配置和部署 LobeChat 以使用 Ollama：

## 本地運行 Ollama

首先，您需要安裝 Ollama。關於安裝和配置 Ollama 的詳細步驟，請參閱 [Ollama 官方網站](https://ollama.com)。

## 本地運行 LobeChat

1. 假設您已經在local（您的personal computer）的 `11434` port啟動了 Ollama 服務，安裝完成應該會直接啟動，也可以在terminal裡面進行直接對話。
(可以前往:http://localhost:11434/)
(確認您的瀏覽器顯示“Ollama is running”)



2. 運行以下 Docker 命令以在本地啟動 LobeChat：
（要先安裝/註冊/登入/運行 docker hub，[DockerHub](/docs/usage/providers/ollama)）

現在，您可以使用 LobeChat 與本地的 LLM 進行對話。

3. !!本地運行的網址：!!
http://localhost:3210

有關在 LobeChat 中使用 Ollama 的更多信息，請參閱 [Ollama 使用](https://hub.docker.com/)。

## 從非本地位置訪問 Ollama

當您首次啟動 Ollama 時，它僅配置為允許來自本地機器的訪問。要啟用來自其他域的訪問並設置端口監聽，您需要相應地調整環境變量 `OLLAMA_ORIGINS` 和 `OLLAMA_HOST`。

### Ollama 環境變量

| 環境變量 | 描述 | 默認值 | 附加信息 |
| --- | --- | --- | --- |
| `OLLAMA_HOST` | 指定綁定的主機和端口 | "127.0.0.1:11434" | 使用 "0.0.0.0:端口" 使服務可從任何機器訪問 |
| `OLLAMA_ORIGINS` | 允許的跨來源源的逗號分隔列表 | 限制為本地訪問 | 設置為 "\*" 以避免 CORS，請根據需求設置 |
| `OLLAMA_MODELS` | 模型所在目錄的路徑 | "~/.ollama/models" 或 "/usr/share/ollama/.ollama/models" | 可根據需求自定義 |
| `OLLAMA_KEEP_ALIVE` | 模型在 GPU 記憶體中保持加載的時間 | "5m" | 動態加載和卸載模型可以減少 GPU 負載但可能增加磁碟 I/O |
| `OLLAMA_DEBUG` | 設置為 1 以啟用額外的調試日誌 | 通常禁用 |  |

### 在 Windows 上設置環境變量

在 Windows 上，Ollama 繼承您的用戶和系統環境變量。

1. 首先，通過點擊系統托盤圖標並選擇退出來關閉 Ollama。
2. 從控制面板編輯系統環境變量。
3. 為您的用戶帳戶編輯或創建 `OLLAMA_HOST`、`OLLAMA_ORIGINS` 等新變量。
4. 點擊確定/應用以保存。
5. 重新啟動 Ollama。

### 在 Mac 上設置環境變量

如果 Ollama 作為 macOS 應用程序運行，應使用 `launchctl` 來設置環境變量：

1. 對於每個環境變量，使用 `launchctl setenv` 命令。
   
   ```bash
   launchctl setenv OLLAMA_HOST "0.0.0.0"
   launchctl setenv OLLAMA_ORIGINS "*"
   ```

2. 重新啟動 Ollama 應用程序。

### 在 Linux 上設置環境變量

如果 Ollama 作為 systemd 服務運行，應使用 `systemctl` 來設置環境變量：

1. 通過運行 `sudo systemctl edit ollama.service` 編輯 systemd 服務。
   
   ```bash
   sudo systemctl edit ollama.service
   ```

2. 對於每個環境變量，在 `[Service]` 部分下添加一行 `Environment`：
   
   ```ini
   [Service]
   Environment="OLLAMA_HOST=0.0.0.0"
   Environment="OLLAMA_ORIGINS=*"
   ```

3. 保存並退出。
4. 重新加載 `systemd` 並重新啟動 Ollama：
   
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl restart ollama
   ```

### 在 Docker 上設置環境變量

如果 Ollama 作為 Docker 容器運行，您可以將環境變量添加到 `docker run` 命令中。

有關配置的進一步指導，請參閱 [Ollama 官方文檔](https://github.com/ollama/ollama/blob/main/docs/faq.md#how-do-i-configure-ollama-server)。
