# syntax=docker/dockerfile:1

ARG N8N_VERSION=latest

# =========================================================
# Builder 阶段：用同版本 Alpine(3.22) 安装 git
# 目的：我们只在 builder 里用 apk，最终镜像里不需要 apk 存在
# =========================================================
FROM alpine:3.22 AS git_builder

RUN set -eux; \
    apk add --no-cache git ca-certificates

# =========================================================
# 最终阶段：基于官方 n8n 镜像（Docker Hardened Images / Alpine）
# =========================================================
FROM docker.n8n.io/n8nio/n8n:${N8N_VERSION}

USER root

# ---------------------------
# 1) 拷贝 git 可执行文件
# ---------------------------
COPY --from=git_builder /usr/bin/git /usr/bin/git

# ---------------------------
# 2) 拷贝 git 运行时依赖的共享库
#    注意：git 在 Alpine 下依赖不少库（libcurl/openssl/zlib 等）
#    这里直接把 builder 的 /usr/lib 相关内容拷过来是最省事也最稳的方式。
#    代价：镜像会大一点，但远比“回填 apk 并在线安装”可靠。
# ---------------------------
COPY --from=git_builder /usr/lib/ /usr/lib/

# ---------------------------
# 3) CA 证书（HTTPS 拉取必需）
# ---------------------------
COPY --from=git_builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt

# ---------------------------
# 4) 验证 git 可用（构建期就失败更好排查）
# ---------------------------
RUN set -eux; \
    git --version

# 保持官方安全模型：切回 node 用户，不改 entrypoint/CMD
USER node