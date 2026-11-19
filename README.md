<div align="center">

<img src="src-ui/assets/logo.png" alt="屿岸别 Logo" width="104" />

# 屿岸别

**QQ 官方机器人控制台**

在一个桌面终端里管理官机、查看 CPU 与内存、检查实时事件和运行日志。

`v1.2.33` · Rust + Tauri 2 + React

[下载最新版](https://github.com/RO-Series/YuAnBie/releases/latest) · [全部发行版](https://github.com/RO-Series/YuAnBie/releases)

</div>

## 当前版本

`v1.2.33` 是现行 Windows 正式版本，主要修复业务中心四个 Tab 内容无法滚动、底部内容被裁切的问题：

- 「指令与词库」在宽屏下使用左侧列表与右侧编辑器独立滚动；小窗口统一由页面内容区滚动。
- 「变量」「AI 配置」「任务与发送」共用页面滚动区域，切换 Tab 时自动回到顶部。
- 变量表格和任务表格保留横向滚动，并为底部内容留出空间，避免被滚动条遮挡。

已知限制：在 `1279px ↔ 1280px` 响应式断点之间切换时，业务中心滚动位置可能重置。

## 产品范围

屿岸别面向在程序内部处理指令消息的 QQ 官方机器人。桌面端只负责控制、监控和排障，QQ 客户端的聊天体验仍由 QQ 提供。

主窗口提供五个主功能入口，并在侧栏底部保留独立的设置入口：

- **概览**：官机舰队状态、最近事件、主机 CPU / RAM。
- **机器人**：创建、配置、连接和停止官机 Bot。
- **调试台**：查看 API 路由、请求结果、实时事件和 `message_create` 消息内容。
- **业务中心**：查看群授权、积分业务任务和持久化记录，切换查书、番茄、点歌、词库与 AI 业务。
- **个人**：澄知账号登录、授权状态、贡献与积分资料。
- **设置**：数据目录、日志、界面偏好、授权重试与版本信息。

聊天页、聊天弹窗、账号托盘、原生截图和群文件体系已在阶段 5a 从产品与代码中移除。查看官机消息请使用**调试台事件流**；运维日志请使用机器人日志页。

业务中心的 AI 支持两种模式。官方模型按 OpenAI 兼容响应的实际 `total_tokens` 扣除站内积分；自定义模型接入用户自己的 OpenAI 兼容服务，不扣站内积分。所有 API Key 只保存到系统 SecretStore，不进入前端配置或普通日志。

## 下载与安装

推荐从 [GitHub Releases](https://github.com/RO-Series/YuAnBie/releases/latest) 下载 Windows 安装包：

1. 下载 `YuAnBie-1.2.33-x64.msi`。
2. 双击安装并允许 Windows UAC 提示；MSI 使用 `perMachine` 安装，因此需要管理员权限。
3. 安装完成后从桌面或开始菜单启动**屿岸别**。
4. 在「个人」页面登录澄知账号；授权成功后才能连接 QQ 官机。

免安装版 `YuAnBie-1.2.33-x64.exe` 也已随 Release 发布。程序需要 Windows 10 / 11 x64 和 Microsoft Edge WebView2；Windows 10/11 通常已自带 WebView2。

应用通过以下清单检查更新：

```text
https://github.com/RO-Series/YuAnBie/releases/latest/download/update.json
```

## 授权与受限模式

应用使用澄知文档站的 OAuth2 Authorization Code + PKCE 登录：

- 桌面端是 Public Client，不包含 `client_secret`；授权码与 PKCE 会话绑定。
- 本地回调地址固定为 `http://127.0.0.1:14201/callback`。
- 授权范围是 `profile activity points offline_access`。
- Access Token、Refresh Token 与到期时间以单个原子 JSON 会话写入系统 `SecretStore`。
- Refresh 成功后，新的 Refresh Token 会与 Access Token 一起原子替换。

未登录或授权失效时，主界面可查看，但 Bot 连接进入受限模式。顶部红条会提示“未登录澄知账号，Bot 连接受限”，点击进入**个人**页面登录或重试授权。

构建时可通过 `CLEARZHI_OAUTH_CLIENT_ID` 注入 Client ID；运行环境中的同名变量也可覆盖构建值。澄知后台必须登记上述本地回调地址。

## 数据与日志

Windows 默认数据根为 `%ProgramData%\屿岸别`，Linux 默认使用 XDG 数据目录下的 `屿岸别`。旧默认目录会在启动时迁移或补齐到新目录；用户主动迁移过的自定义路径保持优先。

官机日志可按 Bot 查看，文件采用 JSON Lines，并按天轮转。物理删除 Bot 后，历史日志在 7 天保留期内仍可查询。

## 运行环境

- Windows 10 / 11 x64 或 Windows Server 2016+
- Microsoft Edge WebView2
- 登录后由澄知 OAuth 自动拉取官机密钥；未登录或缺少 `qqbot:keys` 权限时进入受限模式，仍可查看 UI、日志与测试数据
- 从源码构建需要 Rust stable、Node.js 与 pnpm

## 开发与验证

```powershell
pnpm.cmd install --frozen-lockfile
pnpm.cmd ts-bindings
pnpm.cmd typecheck
pnpm.cmd lint
pnpm.cmd build
cargo check --workspace --all-targets
cargo test --workspace
```

只构建 Tauri 主程序：

```powershell
pnpm.cmd tauri build --no-bundle
```

Tauri 的直接输出为 `target/release/NapCatQQ-Desktop.exe`。对外交付时复制为：

```powershell
Copy-Item target/release/NapCatQQ-Desktop.exe target/release/屿岸别.exe -Force
```

不要用单独执行的 `cargo build --release` 覆盖 Tauri 产物，否则可能把开发服务器地址写进前端资源。

构建 MSI 并准备发布资产：

```powershell
pnpm.cmd tauri:build:msi
pnpm.cmd release:prepare-yuanbie
```

发布到 `RO-Series/YuAnBie`：

```powershell
$env:GITHUB_TOKEN = '<具备 RO-Series/YuAnBie contents:write 权限的令牌>'
pnpm.cmd release:publish-yuanbie
Remove-Item Env:GITHUB_TOKEN -ErrorAction SilentlyContinue
```

Release 只上传四个资产：

- `YuAnBie-<version>-x64.msi`
- `YuAnBie-<version>-x64.exe`
- `update.json`
- `SHA256SUMS.txt`

`README.md` 只作为 Release Notes，不作为额外资产上传。GitHub Actions 使用仓库秘密 `YUANBIE_RELEASE_TOKEN`，并会清理 Release 中不在白名单内的旧资产。

架构与阶段记录位于 [`docs/architecture/`](docs/architecture/)。阶段 5a 的删除范围、保留边界和遗留任务见 [`stage5a-cleanup.md`](docs/architecture/stage5a-cleanup.md)。
