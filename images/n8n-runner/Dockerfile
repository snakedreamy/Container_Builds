# syntax=docker/dockerfile:1

ARG RUNNERS_VERSION=latest
FROM n8nio/runners:${RUNNERS_VERSION}

USER root

# JS runner：安装常用包（按需增删）
RUN cd /opt/runners/task-runner-javascript && pnpm add moment uuid

# Python runner：安装常用包（按需增删）
RUN cd /opt/runners/task-runner-python && uv pip install numpy pandas

# allowlist 配置（非常重要：不在 allowlist 的包，Code 节点不能用）
COPY n8n-task-runners.json /etc/n8n-task-runners.json
COPY n8n-task-runners.json /etc/task-runners.json

USER runner