# n8n 自定义镜像

基于官方 `ghcr.io/n8n-io/n8n`，保持官方启动逻辑不变（不修改 entrypoint/CMD）。


证书信任（自签 CA）：
- 建议按官方方式挂载到 `/opt/custom-certificates`，而不是打进镜像。