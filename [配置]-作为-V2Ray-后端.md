作为 V2Ray 后端支持 TCP、KCP、WS 等协议。

WS + TLS，TLS 可以自动配置 (由 acme.sh 提供)。

如果需要 Caddy 或者 Nginx 来反向代理，请将第一个端口设定为 0，让程序切成本地监听。

V2Ray 的 Grpc API 接口默认是 2333 (这 2333 不是连接端口)。

## 注意

**该 WIKI 的维护者并不是文科生或者专业的文案工作者，因此该 WIKI 可能晦涩难懂。**

**但是当你无法理解文档中的部分内容时，在向任何人提问之前，你应该先尝试以每秒 2 字的速度逐字大声朗读文档、这会帮助你耐心地阅读文档。**

**所有被提问到该后端安装使用问题的人，也应该先对提问者提出大声朗读该 WIKI 的要求。**

*—— 摘抄自 [KoolClash](https://koolclash.js.org/)*

## 节点配置

添加一个节点。

节点地址的填写格式请查看下方。

节点类型为 **V2Ray** 或 **V2Ray 中转**。

## 节点地址

**建议修改数据库节点地址的数据可容纳大小，防止太长无法加入**

**以下是节点地址的基本格式：**

> 没有CDN的域名或者IP;外部端口;AlterId;协议层;附加协议;额外参数

**目前逻辑：**

- 如果外部端口是 0 或者为空，则默认监听本地 [127.0.0.1:inside_port] (inside_port 下方有解释)
- 如果外部端口不是 0，则监听 [0.0.0.0:外部端口]，此时无需 inside_port
- 目前默认支持直接自动tls配置，和加载指定目录的tls证书(需要将证书放在CertFile: "/data/ssl/fullchain.cer",KeyFile: "/data/ssl/key.key", docker 请做好文件映射，不懂的可以谷歌)。 或者也可以使用 Caddy 来提供 TLS，控制代码不会生成 TLS 相关的配置。Caddyfile 可以在本项目的 [/Docker/Caddy_V2ray/Caddyfile](https://github.com/rico93/pay-v2ray-sspanel-v3-mod_Uim-plugin/blob/master/Docker/Caddy_V2ray/Caddyfile) 文件中看到
- 若 outside_port **存在**，则 PHP 端会在生成订阅时候使用 outside_port 覆盖 port


**额外参数：**

*额外参数使用 "|" 来分隔。*

- **inside_port**  为内部监听端口，当宿主机安装多个后端时，请确保每个不一样
- **outside_port** 用于重写外部端口，当使用 WS + TLS 协议或 NAT 类型节点试使用
- **path** 为访问路径，当协议为 WS 时使用。
- **host** 用于定义 headers，当协议为 WS 时使用。
- **server** 用于当节点藏在 CDN 后时覆盖第一个地址
- **中转** 请搭配relayserver和outside_port来复写订阅信息。


**中转配置逻辑**

```
没有CDN的域名或者IP;外部端口;AlterId;协议层;附加协议;额外参数|relayserver=中转地址|outside_port=中转端口
```

**配置示例：**

```
// TCP 示例
非CDN域名或者ip;非0;2;tcp

// TCP + TLS 示例
非CDN域名或者ip;非0;2;tcp;tls

// WS
非CDN域名或者ip;8080;2;ws;;path=/hls/cctv5phd.m3u8|host=这里可以用加了CDN的域名

// WS + CDN
非CDN域名或者ip;8080;2;ws;;path=/hls/cctv5phd.m3u8|server=这里用加了CDN的域名|host=这里用加了CDN的域名

// WS + TLS (自动配置）
非CDN且将要做TLS域名或者ip;非0;2;tls;ws;path=/hls/cctv5phd.m3u8|host=非CDN且要TLS域名


// WS + TLS (自动配置）+CDN
非CDN域名或者ip;非0;2;tls;ws;path=/hls/cctv5phd.m3u8|host=这里用加了CDN的域名|server=这里用加了CDN的域名

// WS + TLS (Caddy 提供)
非CDN域名或者ip;0;2;tls;ws;path=/hls/cctv5phd.m3u8|host=非CDN且要TLS域名|inside_port=10550|outside_port=443

// WS + TLS (Caddy 提供)+CDN
非CDN域名或者ip;0;2;tls;ws;path=/hls/cctv5phd.m3u8|host=这里用加了CDN的域名|inside_port=10550|outside_port=443|server=这里用加了CDN的域名


// nat🐔 ws
非CDN域名或者ip;非0;2;ws;;path=/hls/cctv5phd.m3u8|host=这里可以用加了CDN的域名

// nat🐔 ws + tls (自动配置)，因为部分商家并不提供 80 & 443 访问，所以请考虑手动申请 SSL 证书
非CDN域名且将要作tls的域名或者ip;非0;2;tls;ws;path=/hls/cctv5phd.m3u8|host=将要做tls的域名


// nat🐔 ws + tls (自动配置)+CDN，因为部分商家并不提供 80 & 443 访问，所以请考虑手动申请 SSL 证书
非CDN域名或者ip;非0;2;tls;ws;path=/hls/cctv5phd.m3u8|host=这里用加了CDN且将要tls的域名|server=这里用加了CDN且将要tls的域名


// nat🐔 ws + tls (Caddy 提供)，因为部分商家并不提供 80 & 443 访问，所以请考虑手动申请 SSL 证书
非CDN域名或者ip;0;2;tls;ws;path=/hls/cctv5phd.m3u8|host=将要tls的域名|inside_port=10550|outside_port=11120



// nat🐔 ws + tls (Caddy 提供)+CDN，因为部分商家并不提供 80 & 443 访问，所以请考虑手动申请 SSL 证书
非CDN域名或者ip;0;2;tls;ws;path=/hls/cctv5phd.m3u8|host=这里用加了CDN切将要tls的域名|inside_port=10550|outside_port=11120|server=这里用加了CDN切将要的域名


// 以下为 KCP 示例部分，支持所有 V2Ray 的 type：

// none: 默认值，不进行伪装，发送的数据是没有特征的数据包。
非CDN域名或者ip;非0;2;kcp;noop

// srtp: 伪装成 SRTP 数据包，会被识别为视频通话数据（如 FaceTime）。
非CDN域名或者ip;非0;2;kcp;srtp

// utp: 伪装成 uTP 数据包，会被识别为 BT 下载数据。
非CDN域名或者ip;非0;2;kcp;utp

// wechat-video: 伪装成微信视频通话的数据包。
非CDN域名或者ip;非0;2;kcp;wechat-video

// dtls: 伪装成 DTLS 1.2 数据包。
非CDN域名或者ip;非0;2;kcp;dtls

// wireguard: 伪装成 WireGuard 数据包(并不是真正的 WireGuard 协议) 。
非CDN域名或者ip;非0;2;kcp;wireguard
```

## 关于节点使用 CDN 的说明

首先准备两个域名，此处使用下方两个域名做示例：

- ip.gov.com
- cdn.gov.com

*ip.gov.com 仅作为节点 DDNS 域名，如您的节点无 IP 变化，可直接填写 IP*

将上面两个域名通过 A 记录指向节点服务器的 IP。

随后添加一个节点，地址示例如下：

```
ip.gov.com;0;2;tls;ws;path=/hls/cctv5phd.m3u8|host=cdn.gov.com|inside_port=10550|server=cdn.gov.com|outside_port=443
```

再通过 docker-compose 安装后端，使用 Caddy_V2ray。

此时测试连接该节点，如连接正常则表示证书申请成功且启动成功，此时可以继续下方的步骤。

登录你的域名解析服务商，将 cdn.gov.com 指向由 CDN 服务商提供的 CNAME 域名。