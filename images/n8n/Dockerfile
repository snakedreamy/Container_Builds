# syntax=docker/dockerfile:1

ARG N8N_VERSION=latest

# 使用 GHCR 上的官方镜像作为底座 (与 workflow 检查的源保持一致)
FROM ghcr.io/n8n-io/n8n:${N8N_VERSION}

USER root

# 优化字体支持 (解决 PDF 生成和截图时的中文乱码)
# ttf-dejavu: 覆盖基础西文符号
# font-noto-cjk: 谷歌思源字体，完美支持简繁中日韩
RUN set -eux; \
    apk add --no-cache \
    ttf-dejavu \
    font-noto-cjk

# 如果未来需要安装其他系统级工具，写在这里
# RUN apk add --no-cache ffmpeg

USER node