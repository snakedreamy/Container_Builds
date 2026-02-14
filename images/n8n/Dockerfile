# syntax=docker/dockerfile:1

# n8n 版本由 workflow 传入
ARG N8N_VERSION=latest

# 关键：Alpine 版本也由 workflow 探测后传入（如 3.22）
# 默认 latest 只是兜底（正常不会用到）
ARG ALPINE_VERSION=latest

# =========================================================
# Builder 阶段：在“同版本 Alpine”里安装 git + ca 证书
# 最终镜像不需要 apk 存在，因为我们不在最终镜像执行 apk
# =========================================================
FROM alpine:${ALPINE_VERSION} AS git_builder

RUN set -eux; \
    apk add --no-cache git ca-certificates

# =========================================================
# 最终阶段：基于官方 n8n 镜像（Docker Hardened Images / Alpine）
# 不改 entrypoint / CMD
# =========================================================
FROM docker.n8n.io/n8nio/n8n:${N8N_VERSION}

USER root

# 拷贝 git 可执行文件
COPY --from=git_builder /usr/bin/git /usr/bin/git

# 拷贝 git 运行时依赖库（最稳妥：直接拷 /usr/lib）
# 这样不会出现 "Error loading shared library ..." 的问题
COPY --from=git_builder /usr/lib/ /usr/lib/

# 拷贝 CA 证书（HTTPS 拉取必需）
COPY --from=git_builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt

# 构建期验证：git 必须可用，否则直接失败
RUN set -eux; \
    git --version

# 保持官方安全模型：回到 node 用户
USER node