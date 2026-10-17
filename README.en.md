# SUMA Remote AI Bridge

### Cross-platform P2P Remote AI Model Secure Connection & Sharing Tool

SUMA Remote AI Bridge is a high-performance, secure cross-platform P2P remote AI model connection and sharing solution. Utilizing Tailcat / TSnet (WireGuard) encrypted peer-to-peer tunnels, it enables hosts to run local AI models (such as Ollama, llama.cpp, vLLM, etc.) and allows client computers to directly invoke remote models via standard local HTTP endpoints (e.g., `http://127.0.0.1:18080/v1`) without complicated router port forwarding or public IP exposure. Developed by [LOCALSOFT Digital Studio](https://suma.tw).

---

## 📦 Download & Installation

This project provides both a Windows Desktop edition (Tauri v2 + WebView2) and a Linux Server/Compute Node Headless CLI edition. You can download precompiled binaries from GitHub Releases or build from source.

### 1. Download Precompiled Releases from GitHub

Visit the project's [GitHub Releases Page](https://github.com/marchfun1/SUMA-Remote-AI-Bridge/releases) to download the latest installer or archives:

- **Windows Desktop Full Edition (GUI + Core)**:
  - Download `SUMA-Remote-AI-Bridge-Setup-<version>.exe`.
  - Run the setup wizard. You can perform a full installation (with desktop shortcuts, start menu items, and automatic WebView2 Runtime detection).
  - The wizard also supports custom shortcuts for "Full Edition", "Client Connector Only", and "Host Server Only".
- **Linux Server / Compute Node CLI Edition (Headless)**:
  - Download the archive matching your target CPU architecture: `sumabridge-v<version>-linux-amd64.tar.gz` or `sumabridge-v<version>-linux-arm64.tar.gz`.
  - Extract to your system directory and grant executable permissions:
    ```bash
    tar -zxvf sumabridge-v*.tar.gz
    sudo install -m 755 sumabridge /usr/local/bin/sumabridge
    ```

---

## 🚀 Quick Start

### Mode A: Share Local AI Models (Host Server)
1. **Windows Desktop**:
   - Launch SUMA Remote AI Bridge and go to the "**Share**" tab.
   - Verify your upstream AI model port (e.g., Ollama defaults to `11434`, vLLM defaults to `8000`).
   - Click "**Start Sharing**". The system generates a dedicated and secure **Connection Token**.
   - Send this Token to your clients or team members.
2. **Linux CLI**:
   - On the Linux compute machine running Ollama or vLLM, simply run:
     ```bash
     sumabridge
     ```
   - The terminal will display two tokens:
     - **Connect Token**: For client inference and model invocation.
     - **Admin Token**: For Windows admin management, node monitoring, and dynamic port configuration.

### Mode B: Connect to Remote AI Models (Client Connector)
1. Launch SUMA Remote AI Bridge and navigate to the "**Connect**" tab.
2. Paste the **Connection Token** provided by the host and specify your local mapping port (defaults to `18080`).
3. Click "**Connect**". Once connected, a local OpenAI/Ollama compatible standard endpoint is established:
   - **Base URL**: `http://127.0.0.1:18080/v1`
   - **API Key**: Enter any string (or leave blank).
4. In any AI tool (Chatbox, NextChat, Open WebUI, coding extensions, etc.), set the API URL to `http://127.0.0.1:18080/v1` to seamlessly access the remote model.

---

## 🌟 Key Features

### 🛡️ End-to-End Encryption & Security Protection
- **Tailcat P2P Connectivity**: WireGuard-based encrypted tunnels prioritize direct UDP peer-to-peer transmission, with automatic fallback to DERP relays in symmetric NAT environments.
- **Device Public Key Authorization**: Ed25519 public key fingerprint verification supporting whitelist authorization, pending approval queues, one-click approvals, and access revocations.
- **Envelope Encrypted Secret Storage**: Combines OS-native keystores (Windows DPAPI, macOS Keychain, Linux Secret Service) with AES-256-GCM to ensure sensitive credentials are never stored in plaintext.
- **Strict Boundary Isolation**: Local API binds strictly to Loopback (`127.0.0.1`) with origin checks, DNS rebinding mitigation, and 256-bit dynamic Session Tokens.
- **Log Masking**: Built-in sensitive information scrubbing ensures tokens, private keys, and conversational prompts are never logged to disk.

### 💻 Dual-Mode Operation & Headless Linux CLI
- **Simultaneous Dual-Mode**: A single node can act as both a sharing host and a connecting client at the same time.
- **Minimalist Linux CLI**: Tailored for Linux servers, Docker containers, and cloud GPU instances with zero overhead, quiet logs by default, and real-time streaming output.
- **Remote Admin Control**: Administrators can enter the Linux node's Admin Token on Windows to visually monitor node health, dynamically change upstream ports, review device authorization, and stream server logs.

### 🎨 Modern Desktop User Experience
- **Tauri v2 + Lightweight Web Frontend**: Ultra-light footprint (10-25MB), rapid startup, with an intuitive and responsive interface.
- **6 Core Panels**:
  1. **Dashboard**: Real-time connection status, quick switches, traffic stats, and live core log streaming.
  2. **Share**: Upstream port settings, token generation & copy, and active client metrics.
  3. **Connect**: Remote connection token setup, local port mapping, and one-click Base URL copying.
  4. **Devices**: Authorized device list, pending requests, approval/rejection, and revocation controls.
  5. **Diagnostics & Logs**: Channel connectivity tests, model API health checks, and aggregated historical log export.
  6. **Settings**: Startup options, background tray execution, reconnect policies, update intervals, and language switching.
- **Trilingual Localization (i18n)**: Native runtime switching between Traditional Chinese, Simplified Chinese, and English.
- **System Tray Integration**: Minimize to tray on close, background watchdog protection, and quick tray menu actions.

---


## 📋 Versioning & Updates

- You can configure the update frequency (daily, weekly, monthly) under "Settings > Check for Updates" to automatically check for new releases on GitHub.
- **Beta Releases**: Beta releases may have defined testing lifecycles to ensure stability and network boundary improvements are promptly evaluated. Please check GitHub Releases regularly for updates.

---

## 📜 License

For detailed copyright and software license terms, please refer to:
* [doc/LICENSE_zh-TW.md](doc/LICENSE_zh-TW.md) (Traditional Chinese)
* [doc/LICENSE_zh-CN.md](doc/LICENSE_zh-CN.md) (Simplified Chinese)
* [doc/LICENSE_en.md](doc/LICENSE_en.md) (English)

---

## 🏢 About the Author & Support

- **Author / Developer:** LOCALSOFT Digital Studio (域創數位工作室)
- **Website:** [https://suma.tw](https://suma.tw)
- **Source Code Repository:** [GitHub Repository](https://github.com/marchfun1/SUMA-Remote-AI-Bridge)
- **Issue Tracker & Feedback:** Feel free to open issues or suggestions on GitHub Issues.

---

Copyright 2026 [LOCALSOFT Digital Studio](https://suma.tw). All rights reserved.
