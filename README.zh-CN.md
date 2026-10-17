# SUMA 远端 AI 桥接器 (SUMA Remote AI Bridge)

### 跨平台 P2P 远端 AI 模型安全连接与共享工具

SUMA 远端 AI 桥接器（SUMA Remote AI Bridge）是一套高效、安全的跨平台 P2P 远端 AI 模型连接与共享工具。透过 Tailcat / TSnet (WireGuard) 点对点加密通道技术，让使用者在主机端执行本地 AI 模型（如 Ollama, llama.cpp, vLLM 等），并在客户端电脑透过标准的本地 HTTP 端口（如 `http://127.0.0.1:18080/v1`）直接调用远端模型，无需繁琐的路由器端口转发或暴露公网 IP。由 [域创数位工作室 (LOCALSOFT Digital Studio)](https://suma.tw) 开发。

---

## 📦 下载与安装 (Download & Installation)

本专案支援 Windows 桌面图形版 (Tauri v2 + WebView2) 以及 Linux 伺服器/算力主机无头 CLI 版。你可以直接由 GitHub Releases 下载编译完成的安装包，或自行从原始码建置。

### 1. 从 GitHub Releases 下载预编译版本

前往专案的 [Releases 发布页面](https://github.com/marchfun1/SUMA-Remote-AI-Bridge/releases) 下载最新安装档或压缩档：

- **Windows 桌面完整版 (GUI + Core)**：
  - 下载 `SUMA-Remote-AI-Bridge-Setup-<版本号>.exe`。
  - 运行安装精灵，可选择完整安装（含桌面快捷方式、开始功能表启动项目、自动侦测 WebView2 Runtime）。
  - 安装向导亦提供「完整版」、「客户端连线专用」、「主机端分享专用」之弹性模式快捷方式建立。
- **Linux 伺服器 / 运算节点 CLI 版 (Headless)**：
  - 下载对应架构的压缩档：`sumabridge-v<版本号>-linux-amd64.tar.gz` 或 `sumabridge-v<版本号>-linux-arm64.tar.gz`。
  - 解压缩至系统目录并赋予执行权限：
    ```bash
    tar -zxvf sumabridge-v*.tar.gz
    sudo install -m 755 sumabridge /usr/local/bin/sumabridge
    ```

---

## 🚀 快速开始 (Quick Start)

### 模式 A：分享本地 AI 模型 (Host Server)
1. **Windows 桌面版**：
   - 启动 SUMA Remote AI Bridge，前往「**分享模型 (Share)**」分页面。
   - 确认本地模型的上游连接端口（如 Ollama 预设为 `11434`，vLLM 预设为 `8000`）。
   - 点击「启动分享」，系统将自动产生一组合法的安全**连接 Token**。
   - 将该 Token 提供给欲连接的客户端或工作伙伴。
2. **Linux CLI 版**：
   - 在执行 Ollama 或 vLLM 的 Linux 算力节点上直接运行：
     ```bash
     sumabridge
     ```
   - 终端机将立即输出两组 Token：
     - **连接 Token (`Connect Token`)**：提供给客户端进行模型连线推论。
     - **管理 Token (`Admin Token`)**：提供给 Windows 管理端进行远端状态监控与连接端口动态设定。

### 模式 B：连接远端 AI 模型 (Client Connector)
1. 启动 SUMA Remote AI Bridge，前往「**连接模型 (Connect)**」分页面。
2. 贴上主机端提供的**连接 Token**，并指定本地映射端口（预设为 `18080`）。
3. 点击「建立连线」，连线成功后本地即出现兼容 OpenAI / Ollama 的标准 API 端点：
   - **Base URL**：`http://127.0.0.1:18080/v1`
   - **API Key**：输入任意字串（或免填）即可。
4. 在任何 AI 工具（Chatbox, NextChat, Open WebUI, 程式码扩充套件等）中将 API 网址指向 `http://127.0.0.1:18080/v1`，即可流畅调用远端模型。

---

## 🌟 核心特性 (Key Features)

### 🛡️ 安全加密传输与隐私保护
- **Tailcat P2P 连线**：基于 WireGuard 核心的点对点加密通道，优先建立直接 UDP 直连；在对称型 NAT 复杂网络环境下自动启用 DERP 中继备援。
- **装置公钥授权机制**：基于 Ed25519 装置公钥指纹的双向信任审核，支援白名单授权、待审核（Pending）提示、一键核准与拒绝撤销。
- **信封加密机密保护**：结合操作系统原生安全库（Windows DPAPI、macOS Keychain、Linux Secret Service）与 AES-256-GCM，本地敏感设定绝不以明文留存。
- **边界隔离与防护**：本地 API 严格绑定 Loopback 回环位址（`127.0.0.1`），具备严格的 Origin 白名单验证与 256-bit 动态 Session Token 认证。
- **日志脱敏遮罩 (Log Masking)**：核心日志内建敏感资讯过滤，Token、密钥与对话内容 Prompt 绝不落盘记录。

### 💻 双模式运作与 Linux CLI 无头支援
- **双向模式支援**：单一实例可同时作为分享主机与连接客户端，既可调用远端算力，亦可对外分享本地特定模型。
- **Linux 极简 CLI 模式**：专为 Linux 伺服器、Docker 容器与云端 GPU 实例设计，极致轻量无头运行，预设安静日志并支援即时串流输出。
- **远端管理员连线 (Remote Admin)**：管理者可于 Windows 桌面端输入 Linux 节点之 Admin Token，远端视觉化监控节点状态、即时调整上游端口、审核装置与读取远端串流日志。

### 🎨 现代化桌面使用者介面
- **Tauri v2 + 轻量 Web 前端**：极致轻巧（约 10-25MB）、启动迅速，介面直观且流畅。
- **6 大核心功能页面**：
  1. **首页 (Dashboard)**：即时连线状态、快速开关、流量统计与核心即时日志串流。
  2. **分享模型 (Share)**：上游端口设定、Token 生成与复制、连线数监控。
  3. **连接模型 (Connect)**：远端连线 Token 设定、本地映射端口设定与 Base URL 一键复制。
  4. **装置授权 (Devices)**：已授权装置检视、待审核清单、核准/拒绝与随时撤销控制。
  5. **状态诊断与日志 (Diagnostics & Logs)**：通道连线检测、模型 API 健康检查、全历来日志聚合导出。
  6. **系统设定 (Settings)**：开机启动、背景常驻、断线自动重试、更新频率与语系切换。
- **多语系 (i18n) 支援**：内建繁体中文 (Traditional Chinese)、简体中文 (Simplified Chinese) 与英文 (English) 即时切换。
- **系统匣 (System Tray) 支援**：关闭视窗常驻系统匣、背景静默守护与快速托盘功能表控制。

---


## 📋 版本说明与检查更新

- 本软件于「系统设定」提供「检查更新」功能，可设定每天、每周或每月自动检查 GitHub / 官方最新释出版本。
- **Beta 测试版本**：Beta 测试版本可能设有版本测试周期，目的在于确保测试期间能即时修正潜在稳定性与网络边界问题。请定期造访 GitHub Releases 取得最新版本。

---

## 📜 授权条款 (License)

详细的版权声明与软件授权条款请参阅：
* [doc/LICENSE_zh-TW.md](doc/LICENSE_zh-TW.md) (繁体中文)
* [doc/LICENSE_zh-CN.md](doc/LICENSE_zh-CN.md) (简体中文)
* [doc/LICENSE_en.md](doc/LICENSE_en.md) (English)

---

## 🏢 关于作者与技术支援

- **开发团队：** 域创数位工作室 (LOCALSOFT Digital Studio)
- **官方网站：** [https://suma.tw](https://suma.tw)
- **专案源码：** [GitHub Repository](https://github.com/marchfun1/SUMA-Remote-AI-Bridge)
- **问题反馈与建议：** 欢迎至 GitHub Issues 提出反馈或建言。

---

Copyright 2026 [LOCALSOFT Digital Studio](https://suma.tw). All rights reserved.
