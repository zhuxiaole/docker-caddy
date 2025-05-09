###### 构建 ######

ARG CADDY_VERSION=2.10.0

FROM caddy:$CADDY_VERSION-builder-alpine AS builder

RUN xcaddy build \
  --with github.com/caddy-dns/tencentcloud \
  --with github.com/caddy-dns/alidns \
  --with github.com/mholt/caddy-l4

###### 镜像 ######
FROM caddy:$CADDY_VERSION-alpine

# 默认中国时区
ENV TZ=Asia/Shanghai
ENV CONFIG_FILE=/etc/caddy/Caddyfile
ENV TENCENTCLOUD_SECRET_ID=
ENV TENCENTCLOUD_SECRET_KEY=
ENV ALIYUN_ACCESS_KEY_ID=
ENV ALIYUN_ACCESS_KEY_SECRET=

RUN \
  # 在/etc/caddy/Caddyfile生成默认配置文件
  echo "" > /etc/caddy/Caddyfile && \
  # 安装 tzdata 以使用 TZ 环境变量设置时区
  apk add --no-cache --update tzdata

WORKDIR /

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
COPY ./snippets/tencentcloud /etc/caddy/tencentcloud
COPY ./snippets/alidns /etc/caddy/alidns
COPY ./snippets/security /etc/caddy/security
COPY ./entrypoint.sh /entrypoint.sh

RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
