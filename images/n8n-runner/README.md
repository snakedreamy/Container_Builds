# n8n-runner 自定义镜像

基于官方 `n8nio/runners` 扩展：

- JavaScript runner：安装 `moment`、`uuid`
- Python runner：安装 `numpy`、`pandas`
- 同时写入 allowlist 配置，确保 Code 节点可用这些包

构建策略：
- 以 `n8nio/runners:latest` 为稳定最新版基准
- 自动构建并推送到 GHCR
- 支持 linux/amd64 与 linux/arm64