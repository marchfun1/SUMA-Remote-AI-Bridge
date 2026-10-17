# SUMA 遠端 AI 橋接器 (SUMA Remote AI Bridge)

### 跨平台 P2P 遠端 AI 模型安全連線與分享工具

SUMA 遠端 AI 橋接器（SUMA Remote AI Bridge）是一套高效、安全的跨平台 P2P 遠端 AI 模型連線與共享工具。透過 Tailcat / TSnet (WireGuard) 點對點加密通道技術，讓使用者在主機端執行本機 AI 模型（如 Ollama, llama.cpp, vLLM 等），並在客戶端電腦透過標準的本機 HTTP 埠號（如 `http://127.0.0.1:18080/v1`）直接調用遠端模型，無須繁瑣的路由器連接埠轉發或暴露公網 IP。由 [域創數位工作室 (LOCALSOFT Digital Studio)](https://suma.tw) 開發。

---

## 🌎 README Languages / 說明文件語言
* 繁體中文 (Traditional Chinese): [README.zh-TW.md](README.zh-TW.md)
* 简体中文 (Simplified Chinese): [README.zh-CN.md](README.zh-CN.md)
* English: [README.en.md](README.en.md)

---

## 📦 下載與安裝 (Download & Installation)

本專案支援 Windows 桌面圖形版 (Tauri v2 + WebView2) 以及 Linux 伺服器/算力主機無頭 CLI 版。你可以直接由 GitHub Releases 下載編譯完成的安裝包，或自行從原始碼建置。

### 1. 從 GitHub Releases 下載預編譯版本

前往專案的 [Releases 發布頁面](https://github.com/marchfun1/SUMA-Remote-AI-Bridge/releases) 下載最新安裝檔或壓縮檔：

- **Windows 桌面完整版 (GUI + Core)**：
  - 下載 `SUMA-Remote-AI-Bridge-Setup-<版本號>.exe`。
  - 執行安裝精靈，可選擇完整安裝（含桌面捷徑、開始功能表啟動項目、自動偵測 WebView2 Runtime）。
  - 安裝精靈亦提供「完整版」、「客戶端連線專用」、「主機端分享專用」之彈性模式捷徑建立。
- **Linux 伺服器 / 運算節點 CLI 版 (Headless)**：
  - 下載對應架構的壓縮檔：`sumabridge-v<版本號>-linux-amd64.tar.gz` 或 `sumabridge-v<版本號>-linux-arm64.tar.gz`。
  - 解壓縮至系統目錄並賦予執行權限：
    ```bash
    tar -zxvf sumabridge-v*.tar.gz
    sudo install -m 755 sumabridge /usr/local/bin/sumabridge
    ```

---

## 🚀 快速開始 (Quick Start)

### 模式 A：分享本機 AI 模型 (Host Server)
1. **Windows 桌面版**：
   - 啟動 SUMA Remote AI Bridge，前往「**分享模型 (Share)**」分頁。
   - 確認本機模型的上游連接埠（如 Ollama 預設為 `11434`，vLLM 預設為 `8000`）。
   - 點擊「啟動分享」，系統將自動產生一組專屬的安全**連線 Token**。
   - 將該 Token 提供給欲連線的客戶端或工作夥伴。
2. **Linux CLI 版**：
   - 在執行 Ollama 或 vLLM 的 Linux 算力節點上直接執行：
     ```bash
     sumabridge
     ```
   - 終端機將立即輸出兩組 Token：
     - **連線 Token (`Connect Token`)**：提供給客戶端進行模型連線推論。
     - **管理 Token (`Admin Token`)**：提供給 Windows 管理端進行遠端狀態監控與連接埠動態設定。

### 模式 B：連接遠端 AI 模型 (Client Connector)
1. 啟動 SUMA Remote AI Bridge，前往「**連接模型 (Connect)**」分頁。
2. 貼上主機端提供的**連線 Token**，並指定本機映射連接埠（預設為 `18080`）。
3. 點擊「建立連線」，連線成功後本機即出現相容 OpenAI / Ollama 的標準 API 端點：
   - **Base URL**：`http://127.0.0.1:18080/v1`
   - **API Key**：輸入任意字串（或免填）即可。
4. 在任何 AI 工具（Chatbox, NextChat, Open WebUI, 程式碼擴充套件等）中將 API 網址指向 `http://127.0.0.1:18080/v1`，即可流暢調用遠端模型。

---

## 🌟 核心特性 (Key Features)

### 🛡️ 安全加密傳輸與隱私保護
- **Tailcat P2P 連線**：基於 WireGuard 核心的點對點加密通道，優先建立直接 UDP 直連；在對稱型 NAT 複雜網路環境下自動啟用 DERP 中繼備援。
- **裝置公鑰授權機制**：基於 Ed25519 裝置公鑰指紋的雙向信任審核，支援白名單授權、待審核（Pending）提示、一鍵核准與拒絕撤銷。
- **信封加密機密保護**：結合作業系統原生安全庫（Windows DPAPI、macOS Keychain、Linux Secret Service）與 AES-256-GCM，本機敏感設定絕不以明文留存。
- **邊界隔離與防護**：本機 API 嚴格繫結 Loopback 回環位址（`127.0.0.1`），具備嚴格的 Origin 白名單驗證與 256-bit 動態 Session Token 認證。
- **日誌脫敏遮罩 (Log Masking)**：核心日誌內建敏感資訊過濾，Token、金鑰與對話內容 Prompt 絕不落盤記錄。

### 💻 雙模式運作與 Linux CLI 無頭支援
- **雙向模式支援**：單一實例可同時作為分享主機與連接客戶端，既可調用遠端算力，亦可對外分享本機特定模型。
- **Linux 極簡 CLI 模式**：專為 Linux 伺服器、Docker 容器與雲端 GPU 執行個體設計，極致輕量無頭運行，預設安靜日誌並支援即時串流輸出。
- **遠端管理員連線 (Remote Admin)**：管理者可於 Windows 桌面端輸入 Linux 節點之 Admin Token，遠端視覺化監控節點狀態、即時調整上游埠號、審核裝置與讀取遠端串流日誌。

### 🎨 現代化桌面使用者介面
- **Tauri v2 + 輕量 Web 前端**：極致輕巧（約 10-25MB）、啟動迅速，支援深色 (Dark) 與清新亮色 (Light) 主題切換。
- **6 大核心功能頁面**：
  1. **首頁 (Dashboard)**：即時連線狀態、快速開關、流量統計與核心即時日誌串流。
  2. **分享模型 (Share)**：上游埠號設定、Token 生成與複製、連線數監控。
  3. **連接模型 (Connect)**：遠端連線 Token 設定、本機映射埠設定與 Base URL 一鍵複製。
  4. **裝置授權 (Devices)**：已授權裝置檢視、待審核清單、核准/拒絕與隨時撤銷控制。
  5. **狀態診斷與日誌 (Diagnostics & Logs)**：通道連線檢測、模型 API 健康檢查、全歷來日誌聚合匯出。
  6. **系統設定 (Settings)**：開機啟動、背景常駐、斷線自動重試、更新頻率與語系切換。
- **多語系 (i18n) 支援**：內建繁體中文 (Traditional Chinese)、簡體中文 (Simplified Chinese) 與英文 (English) 即時切換。
- **系統匣 (System Tray) 支援**：關閉視窗常駐系統匣、背景靜默守護與快速托盤選單控制。

---

## 🛠️ 自行建置與開發 (Build from Source)

如果你希望自行從 GitHub 原始碼編譯與建置本專案：

### 系統需求
- **Go**: 1.22 或更高版本
- **Node.js**: 18.x 或更高版本 (含 npm)
- **Rust**: 1.75+ (建議使用 `rustup` 安裝最新穩定版)
- **C/C++ 編譯器** (Windows 需 MSVC 或 MinGW-w64，Linux 需 `build-essential`)

### 1. 複製程式庫 (Clone Repository)
```bash
git clone https://github.com/marchfun1/SUMA-Remote-AI-Bridge.git
cd SUMA-Remote-AI-Bridge
```

### 2. Linux CLI 伺服器版本編譯
```bash
# 給予建置腳本執行權限並編譯
chmod +x build-linux.sh
./build-linux.sh

# 編譯產物將輸出於 release/ 目錄，亦可直接使用 go build：
cd core
go build -trimpath -ldflags "-s -w" -o ../release/sumabridge cmd/cli/main.go
```

### 3. Windows 桌面版開發與打包
```powershell
# 1. 編譯 Go 核心為 DLL / 執行檔
cd core
go build -trimpath -buildmode=c-shared -ldflags "-s -w" -o ../release/BridgeCore.dll ./bridge_c.go

# 2. 安裝前端依賴
cd ../frontend-tauri
npm install

# 3. 本機開發模式執行 (熱重載)
npm run tauri dev

# 4. 正式打包安裝程式 (Tauri Bundle / NSIS)
npm run tauri build
```

---

## 📋 版本說明與檢查更新

- 本軟體於「系統設定」提供「檢查更新」功能，可設定每天、每週或每月自動檢查 GitHub / 官方最新釋出版本。
- **Beta 測試版本**：Beta 測試版本可能設有版本測試週期，目的在於確保測試期間能即時修正潛在穩定性與網路邊界問題。請定期造訪 GitHub Releases 取得最新版本。

---

## 📜 授權條款 (License)

詳細的版權聲明與軟體授權條款請參閱：
* [doc/LICENSE_zh-TW.md](doc/LICENSE_zh-TW.md) (繁體中文)
* [doc/LICENSE_zh-CN.md](doc/LICENSE_zh-CN.md) (简体中文)
* [doc/LICENSE_en.md](doc/LICENSE_en.md) (English)

---

## 🏢 關於作者與技術支援

- **開發團隊：** 域創數位工作室 (LOCALSOFT Digital Studio)
- **官方網站：** [https://suma.tw](https://suma.tw)
- **專案原始碼：** [GitHub Repository](https://github.com/marchfun1/SUMA-Remote-AI-Bridge)
- **問題回報與建議：** 歡迎至 GitHub Issues 提出反饋或建言。

---

Copyright 2026 [LOCALSOFT Digital Studio](https://suma.tw). All rights reserved.
