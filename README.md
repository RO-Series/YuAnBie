<p align="center">
  <img src="assets/logo.png" alt="YuAnBie Logo" width="120" height="120" />
</p>

<h1 align="center">屿岸别 (YuAnBie)</h1>

<p align="center">一款基于 Tauri 2 与 Rust 打造的纯血 QQ 官方机器人桌面控制台</p>

<p align="center">
  <a href="https://github.com/RO-Series/YuAnBie/releases/latest"><img src="https://img.shields.io/badge/Download-v1.2.34-blue?style=flat-square" alt="Download" /></a>
  <img src="https://img.shields.io/badge/Platform-Windows-0078D6?style=flat-square" alt="Platform" />
  <img src="https://img.shields.io/badge/License-GPL--3.0-green?style=flat-square" alt="License" />
  <img src="https://img.shields.io/badge/Tauri-2-24C8DB?style=flat-square" alt="Tauri 2" />
  <img src="https://img.shields.io/badge/Rust-2024-orange?style=flat-square" alt="Rust" />
</p>

---

## ✨ 核心特性

- ⚡ **纯血官机**：原生对接 QQ 开放平台 API v2（WebSocket 网关 + HTTP OpenAPI），无需安装 NapCat、无需填写 QQID，AppID + AppSecret 即可上线。
- 🧠 **极强扩展**：内置 SQLite 持久化与自研 AST 模板引擎，模板支持嵌套标签、参数替换、条件分支与整套异步业务流（`[cmd.…]` → 任务队列 → Outbox 重试）。
- 🎁 **开箱即用**：随包预置 **13 条内置指令**——`菜单`、`查书`、`选择`、`番茄`、`点歌`、`选歌`、`签到`、`积分`、`积分记录`、`积分重试`、`授权查询`、`授权本群`、`取消授权`。
- 🤖 **智能 Agent**：群级人格与唤醒词、上下文记忆（按群共享、可裁剪）、官方/自定义模型双模式，模型可调用 `findbook.search`、`findbook.download`、`fanqie.parse`、`book.variable.get` 等工具。
- 💻 **桌面级体验**：极客终端风 UI、内置多标签终端（自定义 shell / 工作目录 / 启动参数 / 环境变量）、全局代理、系统托盘、主题与缩放记忆。
- 🛡 **可控可观测**：任务、Outbox、变量、日志、通知全链路可查；AI 失败落库并在 Bot 卡片亮红点；日志页支持按级别/来源筛选、清屏与脱敏导出。
- 🔐 **凭据安全**：AppSecret / Token / 代理密码统一进系统凭据库（SecretStore），导出配置自动剔除敏感字段。

<!-- 截图: 概览页 -->

## 🚀 快速开始

1. 前往 [Releases](https://github.com/RO-Series/YuAnBie/releases/latest) 下载 `.msi` 安装包或免安装版 `.exe`。
2. 打开应用，在 **个人** 页点击「登录澄知账号」，浏览器完成 OAuth2 授权（PKCE，无 client_secret）。
3. 登录后自动拉取官机列表与密钥，回到 **机器人** 页一键连接；状态依次经过 `connecting → identifying → ready`。

> 系统要求：Windows 10/11 + WebView2 Runtime（系统通常自带）。数据默认落在 `%ProgramData%\屿岸别`。

<!-- 截图: 机器人页 -->

## 🧩 指令与模板示例

「查书」指令的完整模板只有两行，却同时完成了即时回复与异步任务派发：

```text
[send.markdown.**正在查书：**[arg]
[cmd.查书.搜索.[arg]]]
```

- `[send.…]` 是唯一的外发出口，所有可见输出必须写在里面；
- `[arg]` 取触发词之后的全部参数，查询词里的点号（如 `云烟.v2`）会原样保留；
- `[cmd.查书.搜索.[arg]]` 不直接回复，而是落一条任务，由后台 worker 执行搜索、写 `book.list` 变量，再由 `查书.下载` 发文件。

支持的输出段：`text` / `markdown` / `image` / `record` / `video` / `file` / `json` / `button`，配合 `[newline]`、`[or]`、`[random]`、`[judge]`、`[userid]`、`[username]`、`[groupid]`、`[time.format]`、`[book.list]`、`[book.result.path]` 等标签组合。

<!-- 截图: 业务中心 · 指令与词库 -->

## 🏗 技术栈

| 层 | 选型 |
|---|---|
| 桌面壳 | Tauri 2（WebView2、单实例、系统托盘、自动更新） |
| 核心 | Rust 2024（`ncd-domain` / `ncd-qqofficial` / `ncd-business` / `ncd-runtime`） |
| 前端 | React 19 + TypeScript + Vite + Tailwind CSS |
| 数据 | SQLite（WAL，schema 迁移至 v4）+ 系统凭据库 |
| 协议 | QQ 开放平台 API v2（网关 WS + OpenAPI HTTP） |

## 📦 目录速览

```text
crates/
  ncd-domain/       跨边界数据模型（消息、协议、配置、终端）
  ncd-qqofficial/   官方协议实现（网关、鉴权、事件归一化、脱敏）
  ncd-business/     业务引擎（AST、指令/词库、任务、Outbox、查书/番茄/点歌、AI Agent）
  ncd-runtime/      运行时（Bot 生命周期、终端、网络、迁移）
src-tauri/          Tauri 命令层与窗口/托盘/更新
src-ui/             React 控制台界面
```

## 🗺 路线图

- [x] 官方协议直连与多 Bot 并发
- [x] SQLite 业务引擎与 AST 模板
- [x] AI Agent（记忆、工具调用、失败可观测）
- [x] 内置指令与授权体系
- [ ] 更细粒度的权限模型
- [ ] Linux / macOS 打包

## 📄 开源协议

本项目以 **GNU General Public License v3.0** 发布，详见 [LICENSE](https://github.com/RO-Series/YuAnBie/blob/main/LICENSE)。

## 🙏 致谢

- [Tauri](https://tauri.app/) · [Rust](https://www.rust-lang.org/) · [React](https://react.dev/) · [Vite](https://vitejs.dev/) · [SQLite](https://sqlite.org/)
- 图标、协议与业务实现均来自本项目自身资产，未使用第三方品牌素材。

---

<p align="center">
  <sub>本仓库仅发布安装包与更新元数据，主程序源码不在本仓库。</sub>
</p>
