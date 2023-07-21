###### 构建 ######

ARG CADDY_VERSION=2.6.4

FROM caddy:$CADDY_VERSION-builder-alpine AS builder

RUN xcaddy build \
  --with github.com/caddy-dns/dnspod \
  --with github.com/mholt/caddy-l4

###### 镜像 ######
FROM caddy:$CADDY_VERSION-alpine

# 默认中国时区
ENV TZ=Asia/Shanghai
ENV CONFIG_FILE=/etc/caddy/Caddyfile

# 安装 tzdata 以使用 TZ 环境变量设置时区
RUN apk add --no-cache --update tzdata

WORKDIR /

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
COPY ./Caddyfile /etc/caddy/Caddyfile
COPY ./snippets/dnspod /etc/caddy/dnspod
COPY ./snippets/security /etc/caddy/security
COPY ./entrypoint.sh /entrypoint.sh

RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
