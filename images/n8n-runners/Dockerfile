# syntax=docker/dockerfile:1

ARG RUNNERS_VERSION=latest
# 使用 GHCR 上的官方 runners 镜像
FROM ghcr.io/n8n-io/runners:${RUNNERS_VERSION}

USER root

# --- JavaScript Runner 扩展 ---
# 安装常用库，方便 Function 节点调用
RUN cd /opt/runners/task-runner-javascript && \
    pnpm add moment uuid lodash

# --- Python Runner 扩展 ---
# 安装数据处理常用库
RUN cd /opt/runners/task-runner-python && \
    uv pip install numpy pandas requests

# --- 安全配置 ---
# 复制配置文件到系统目录
COPY n8n-task-runners.json /etc/n8n-task-runners.json
# 兼容性复制 (防止旧版本 n8n 查找路径不同)
COPY n8n-task-runners.json /etc/task-runners.json

USER runner