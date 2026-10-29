# 🐸 Frog VPS KeepAlive & HY2 Proxy

一个基于 **GitHub Actions** 的青蛙 VPS 自动化保活工具。支持通过 **Hysteria2 (HY2)** 或标准 **Socks5/HTTP 代理** 安全连接，并内置 **Telegram 实时通知**，让你随时掌握 VPS 的运行状态与保活结果。

---

## 📋 功能特点

1. **自动保活**：定时通过 SSH 登录青蛙 VPS 执行系统健康检查命令（如 `uptime` 和 `df -h`），防止因长期不登录而被回收。
2. **多协议代理支持**：完美支持 `hy2://`（Hysteria2）、`socks5://` 及 `http://` 代理，轻松应对各种复杂的网络环境和防火墙限制。
3. **实时 TG 通知**：保活成功或失败都会第一时间通过 Telegram 机器人向你发送详细报告。
4. **零服务器成本**：完全依托 GitHub Actions 免费云端算力运行，无需自己额外准备中转服务器。

---

## ⚙️ 所需环境变量 (GitHub Secrets)

在使用本工作流之前，需要在你的 GitHub 仓库中配置以下 **Secrets**。

> **配置路径**：进入你的 GitHub 仓库 -> **Settings** -> **Secrets and variables** -> **Actions** -> 点击 **New repository secret** 添加。

| 变量名 | 必填 | 说明 | 示例 |
| :--- | :---: | :--- | :--- |
| `FROG_HOST` | **是** | 青蛙 VPS 的 IP 地址或域名 | `192.168.1.100` 或 `vps.example.com` |
| `FROG_PORT` | **是** | 青蛙 VPS 的 SSH 端口 | `22` |
| `FROG_USER` | **是** | SSH 登录用户名 | `root` |
| `FROG_PASS` | **是** | SSH 登录密码 | `your_ssh_password` |
| `HY2_PROXY_URL` | 否 | 代理节点链接（支持 `hy2://`、`socks5://`、`http://`） | `hy2://passwd@ip:port?sni=...` |
| `TG_BOT_TOKEN` | 否 | Telegram 机器人的 Token（用于接收通知） | `7362991201:AAFOUWJEd2p...` |
| `TG_CHAT_ID` | 否 | 你的 Telegram 用户数字 ID | `6689509569` |

---

## 🚀 使用方法

### 第一步：创建工作流文件
在你的 GitHub 仓库中，新建目录与文件 `.github/workflows/frog-keepalive.yml`，并将写好的工作流代码粘贴进去并提交。

### 第二步：配置 Secrets
按照上表的说明，在仓库的 **Secrets** 中填入你的 VPS 连接信息及可选的代理、TG 通知变量。

### 第三步：手动测试运行
1. 打开 GitHub 仓库的 **Actions** 页面。
2. 在左侧选择 **`🐸 Frog VPS KeepAlive & HY2 Proxy`** 工作流。
3. 点击右侧的 **`Run workflow`** 按钮手动触发一次测试。
4. 检查运行日志或你的 **Telegram**，确认是否成功收到保活成功的通知。

---

## ⏰ 定时任务说明

默认的自动运行周期写在工作流文件的 `schedule` 字段中：
```yaml
schedule:
  - cron: '0 0 */15 * *' # 默认每 15 天自动运行一次
