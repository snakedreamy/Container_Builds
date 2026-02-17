# Containerfile
FROM docker.io/ubuntu:24.04

# 避免交互式安装时的卡顿
ENV DEBIAN_FRONTEND=noninteractive

# 1. 替换国内源（可选，视网络情况而定）并安装基础包
# 如果你是 Rockchip 平台，建议保留官方源或使用专门的 ARM 镜像源
RUN apt-get update && apt-get install -y \
    openssh-server \
    sudo \
    curl \
    git \
    vim \
    nano \
    locales \
    udev \
    && rm -rf /var/lib/apt/lists/*

# 2. 配置 SSH 服务
# 创建 privilege separation 目录
RUN mkdir /run/sshd
# 允许 SSH 访问（后续我们会用密钥登录，这里先允许密码以便调试，或者直接配置密钥）
# 建议禁用密码登录，改用密钥，这里为了演示方便保留默认
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# 3. 创建与宿主机同 ID 的普通用户 (假设宿主机是 1000)
ARG USERNAME=abc
ARG USER_UID=1006
ARG USER_GID=1006

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s /bin/bash \
    && usermod -aG sudo $USERNAME \
    # 给 devuser 设置一个初始密码 (建议第一次登录后修改，或者只用 SSH Key)
    && echo "$USERNAME:rockchip" | chpasswd

# 4. 把用户加入 video/render 组，以便访问 /dev/dri (GPU)
# Rockchip 的 NPU/GPU 权限通常需要 render 或 video 组
RUN usermod -aG video,render,plugdev $USERNAME

# 5. 设置 Locale (解决终端乱码)
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# 6. 配置 SSH 密钥目录 (预先建立目录，防止权限问题)
USER $USERNAME
RUN mkdir -p /home/$USERNAME/.ssh && chmod 700 /home/$USERNAME/.ssh
USER root

# 7. 启动脚本
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]