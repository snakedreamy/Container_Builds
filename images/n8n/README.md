# n8n 自定义镜像

基于官方 `docker.n8n.io/n8nio/n8n`，保持官方启动逻辑不变（不修改 entrypoint/CMD）。

本镜像的目的：为社区节点（例如需要 git 拉取依赖的节点）提供 `git`，且适配官方 Docker Hardened Images（可能没有 apk/apt）。

实现方式：
- 使用同版本 Alpine 的 builder 阶段安装 `git`
- 将 `git` 及其运行依赖库拷贝到最终 n8n 镜像中
- workflow 会自动探测 n8n 底座 Alpine 的 `VERSION_ID`（如 3.22），让 builder 永远跟随底座版本，避免库/符号不匹配

证书信任（自签 CA）：
- 建议按官方方式挂载到 `/opt/custom-certificates`，而不是打进镜像。