# docker-caddy

Web 服务器 Caddy 2 的 Docker 镜像。

感谢 [fjc0k/docker-caddy](https://github.com/fjc0k/docker-caddy)

-----

<!-- TOC depthFrom:2 -->

- [特性](#特性)
  - [支持设置时区](#支持设置时区)
  - [支持 TencentCloud 和 AliDns](#支持-tencentcloud-和-alidns)
  - [内置安全相关的 HTTP 响应头](#内置安全相关的-http-响应头)
  - [支持使用 host.docker.internal 访问宿主服务](#支持使用-hostdockerinternal-访问宿主服务)
- [许可](#许可)

<!-- /TOC -->

## 特性

与官方镜像 [caddyserver/caddy-docker](https://github.com/caddyserver/caddy-docker) 相比，做了一些符合中国用户使用习惯的增强，主要有以下几点：

### 支持设置时区

你可通过环境变量 `TZ` 设置时区，若不设置，默认是：`Asia/Shanghai`。

### 支持 TencentCloud 和 AliDns

支持使用 TencentCloud 和 AliDns 作为 DNS 提供者，并内置 [`tencentcloud`](https://github.com/zhuxiaole/docker-caddy/blob/master/src/snippets/tencentcloud) 和 [`alidns`](https://github.com/zhuxiaole/docker-caddy/blob/master/src/snippets/alidns) 和  片段方便导入：

```ini
# Caddyfile

www.foo.bar {
  import tencentcloud

  respond "hello caddy"
}
```

**注意：** 使用这项功能之前，你得先通过环境变量 `TENCENTCLOUD_SECRET_ID` 和 `TENCENTCLOUD_SECRET_KEY` 设置 TencentCloud 的鉴权信息。或者通过环境变量 `ALIYUN_ACCESS_KEY_ID` 和 `ALIYUN_ACCESS_KEY_SECRET` 设置 AliDns 的鉴权信息。

### 内置安全相关的 HTTP 响应头

全部安全相关的 HTTP 响应头皆提取自 [helmetjs/helmet](https://github.com/helmetjs/helmet#how-it-works) 的默认设置，你可通过内置的 [`security`](https://github.com/zhuxiaole/docker-caddy/blob/master/src/snippets/security) 片段导入：

```ini
# Caddyfile

www.foo.bar {
  import tencentcloud
  import security

  respond "hello caddy"
}
```

### 支持使用 host.docker.internal 访问宿主服务

```ini
# Caddyfile

api.foo.bar {
  import tencentcloud
  import security

  reverse_proxy host.docker.internal:4000
}
```

## 许可

Pigxiaole (c) MIT