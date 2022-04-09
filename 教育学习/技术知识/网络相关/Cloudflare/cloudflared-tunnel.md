# CloudFlare Tunnel 简单教程

- Cloudflare Tunnel 为您提供了一种安全的方式来将您的资源连接到 Cloudflare，而无需公开可路由的 IP 地址。 使用 Tunnel，您无需将流量发送到外部 IP — 相反，您的基础架构 (cloudflared) 中的轻量级守护程序会创建与 Cloudflare 边缘的仅出站连接。 Cloudflare Tunnel 可以将 HTTP Web 服务器、SSH 服务器、远程桌面和其他协议安全地连接到 Cloudflare。 这样，您的源可以通过 Cloudflare 提供流量，而不会受到绕过 Cloudflare 的攻击。
- Cloudflare Tunnel提供一个安全便捷，跨网络的轻量级连接器，可以让我们在本机服务和Cloudflare之间创建安全的，仅出站连接。
- 类似一个内网穿透的工具，可以部署后访问家里或公司的个人服务；

![架构图](https://developers.cloudflare.com/cloudflare-one/static/documentation/connections/connect-apps/handshake.jpg)

## 下载：
[安装下载地址](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation/)

或者：

```
$ wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && dpkg -i cloudflared-linux-amd64.deb
or
$ wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-x86_64.rpm && rpm -ivh cloudflared-linux-x86_64.rpm

```

## 使用
1. 登陆
- 打开浏览器窗口并提示您登录 Cloudflare 帐户。
```
$ cloudflared tunnel login
```

2. 登录您的帐户后，选择您的主机名。选择一个域名，授权给Tunnel；

3. 常用命令

```

创建隧道：cloudflared tunnel create 隧道名
删除隧道：cloudflared tunnel delete 隧道名
列出隧道：cloudflared tunnel list
配置隧道：cloudflared tunnel route dns 隧道名 [CNAME 记录名称].[接入 CLoudflare 的域名]
运行隧道：cloudflared tunnel run (使用默认配置文件)
         cloudflared tunnel --config path/config.yaml run (指定配置文件)

```

4. 参考配置文件

- 默认配置文件位置： /root/.cloudflared/config.yml

```
tunnel: ad15adb1-a4cb-462a-92df-185de86e6666
credentials-file: /root/.cloudflared/ad15adb1-a4cb-462a-92df-185de86e6666.json
ingress:
  - hostname: bt-manager.xxx.com
    service: http://192.168.1.17:8888
  - hostname: xx.xxx.com
    service: http://192.168.1.17:80
  - service: http_status:404
```

5. 安装成服务
   
```
# 安装成服务
cloudflared service install
# 启动服务
systemctl start cloudflared
# 查看服务状态
systemctl status cloudflared
```