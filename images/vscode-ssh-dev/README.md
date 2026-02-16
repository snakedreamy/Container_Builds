# vscode-ssh-dev (Ubuntu + OpenSSH, for VS Code Remote-SSH)

基于 `lscr.io/linuxserver/baseimage-ubuntu:noble` 的远程开发容器：
- 仅暴露 SSH（默认 2222）
- 用户 home 目录持久化为 `/config`
- 适合 VS Code Remote-SSH，在远程容器里跑 AI code，隔离风险

## 启动（Docker Compose 示例）

```yaml
services:
  vscode-dev:
    image: ghcr.io/<your_owner>/vscode-ssh-dev:latest
    container_name: vscode-dev
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Shanghai
      - SUDO_ACCESS=true
      - PASSWORD_ACCESS=false
      - PUBLIC_KEY_URL=https://github.com/<your_github>.keys
    volumes:
      - ./config:/config
    ports:
      - "2222:2222"
    restart: unless-stopped