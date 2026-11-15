# 屿岸别

本仓库仅发布屿岸别安装包、更新参数和发布说明，不存放主程序源码。

## 最新版本

- 版本：v1.2.32
- 安装包：`YuAnBie-1.2.32-x64.msi`
- SHA256：`9778ab6df48a4392578f5ad5467079594d25c754ae9ca9b490d541299e085114`
- 下载：[YuAnBie-1.2.32-x64.msi](https://github.com/RO-Series/YuAnBie/releases/download/v1.2.32/YuAnBie-1.2.32-x64.msi)

## 安装

下载 MSI 后双击安装。应用程序通过本仓库 Release 中的 `update.json` 检查更新。

应用数据默认保存在 `%ProgramData%\屿岸别`。

安装或运行问题请在 [RO-Series/YuAnBie](https://github.com/RO-Series/YuAnBie/issues) 提交。

## 说明

屿岸别 v1.2.32

- 修复未安装 NapCat 时无法保存 QQ 官机配置的问题。
- QQ 官机保存、启动与状态提示不再探测本机 NapCat、QQ 或 SnowLuma 运行时。
- 官机即使残留 Docker 字段，也不会被 NapCat 镜像门禁错误阻断。
- 保留 NapCat/SnowLuma 旧协议配置原有的组件门禁行为。
- 新增官机运行时节门禁回归测试。
