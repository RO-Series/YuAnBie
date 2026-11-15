# 屿岸别

本仓库仅发布屿岸别安装包、更新参数和发布说明，不存放主程序源码。

## 最新版本

- 版本：v1.2.30
- 安装包：`YuAnBie-1.2.30-x64.msi`
- SHA256：`733e8e28cab606a58a990a7772e2e6b0ceb1f13f5aa3c7e7f2de9e9b53be57c5`
- 下载：[YuAnBie-1.2.30-x64.msi](https://github.com/RO-Series/YuAnBie/releases/download/v1.2.30/YuAnBie-1.2.30-x64.msi)

## 安装

下载 MSI 后双击安装。应用程序通过本仓库 Release 中的 `update.json` 检查更新。

应用数据默认保存在 `%ProgramData%\屿岸别`。

安装或运行问题请在 [RO-Series/YuAnBie](https://github.com/RO-Series/YuAnBie/issues) 提交。

## 说明

屿岸别 v1.2.30

- 新增 QQ 官机 AppSecret 自动同步与连接自愈。
- 登录、启动、每 30 分钟及鉴权失败时自动核对官机凭据。
- Bot 卡片和业务中心显示 AppSecret 同步状态，并支持立即同步。
- 修复并发初始化时 SQLite v1 到 v2 迁移冲突。
- 主程序和安装包元数据为 v1.2.30，发布资产不包含主程序源码。
