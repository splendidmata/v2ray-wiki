# 注意

**该 WIKI 的维护者并不是文科生或者专业的文案工作者，因此该 WIKI 可能晦涩难懂。**

**但是当你无法理解文档中的部分内容时，在向任何人提问之前，你应该先尝试以每秒 2 字的速度逐字大声朗读文档、这会帮助你耐心地阅读文档。**

**所有被提问到该后端安装使用问题的人，也应该先对提问者提出大声朗读该 WIKI 的要求。**

*—— 摘抄自 [KoolClash](https://koolclash.js.org/)*

# 作为 普通 SS 配置

## 节点配置

添加一个节点

节点类型为 **Shadowsocks**。

单端口多用户启用 -> 修改为 -> **只启用普通端口**。

## 支持的加密方式

**用户的加密方式必需在支持的加密方式之内才可使用**

- aes-256-cfb
- aes-128-cfb
- chacha20
- chacha20-ietf
- aes-256-gcm
- aes-128-gcm
- chacha20-poly1305 或称 chacha20-ietf-poly1305
- xchacha20-ietf-poly1305

## 安装节点

推荐使用 docker run 命令直接跑或普通安装

如您使用 docker-compose 安装，请选择 **Docker** 即可

---

# 作为 SS + WS(tls) 配置，单端口

## 节点配置

添加一个节点

节点类型为 **Shadowsocks - V2Ray-Plugin**

### 节点地址写法

**以下是节点地址的基本格式：**

> 没有CDN的域名或者IP;外部端口;;协议层;附加协议;额外参数

**额外参数：**

*额外参数使用 "|" 来分隔。*

- **path** 为访问路径
- **server** 为 TLS 域名和用于当节点藏在 CDN 后时覆盖第一个地址
- **host** 用于定义 headers

**配置示例：**

```
// ws
没有CDN的域名或IP;端口;;ws;;path=/hls/cctv5phd.m3u8

// ws + tls
没有CDN的域名或IP;443;;ws;tls;path=/hls/cctv5phd.m3u8|server=TLS域名

// ws+tls+中转

没有CDN的域名或IP;443;;ws;tls;path=/hls/cctv5phd.m3u8|server=TLS域名|host=TLS域名|relayserver=中转地址|outside_port=中转端口

// ws + tls+CDN
没有CDN的域名或IP;443;;ws;tls;path=/hls/cctv5phd.m3u8|server=这里写CDN的域名

// obfs-http
没有CDN的域名或IP;端口;;obfs;http;


```

#### 关于节点使用 CDN 的说明

如您节点无需使用 CDN，则跳过这段进入节点安装

首先准备两个域名，此处使用下方两个域名做示例：

- ip.gov.com
- cdn.gov.com

*ip.gov.com 仅作为节点 DDNS 域名，如您的节点无 IP 变化，可直接填写 IP*

将上面两个域名通过 A 记录指向节点服务器的 IP。

随后添加一个节点，地址示例如下：

```
ip.gov.com;443;;tls;ws;path=/hls/cctv5phd.m3u8|server=cdn.gov.com
```

再安装后端，后端会使用 cdn.gov.com 申请证书。

此时测试连接该节点，如连接正常则表示证书申请成功且启动成功，此时可以继续下方的步骤。

登录你的域名解析服务商，将 cdn.gov.com 指向由 CDN 服务商提供的 CNAME 域名。

## 支持的加密方式

**用户的加密方式必需在支持的加密方式之内才可使用**

- aes-256-gcm
- aes-128-gcm
- chacha20-poly1305 或称 chacha20-ietf-poly1305
- xchacha20-ietf-poly1305

## 安装节点

推荐使用 docker run 命令直接跑或普通安装

如您使用 docker-compose 安装，请选择 **Docker** 即可

Shadowsocks-V2Ray-Plugin 节点不需要使用 Caddy 或 Nginx 分流