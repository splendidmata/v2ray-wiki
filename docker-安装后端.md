## 为了保证image的相对稳定，请大家务必自己维护，

怎么制作 请看 [Docker Image 制作](https://gitlab.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin/-/wikis/Docker-image%E5%88%B6%E4%BD%9C)

## 分流的docker image地址 请从群置顶获取

## 两种安装方式，请任选其一
## 第一种
#### docker 方式安装
首先安装docker
```
curl -fsSL https://get.docker.com -o get-docker.sh  && \
bash get-docker.sh
```
#### docker run 命令运行
```
docker run -d --name=昵称 \
-e speedtest=0  -e api_port=2333 -e usemysql=0 -e downWithPanel=0 \
-e node_id=73 -e sspanel_url=网站WebAPI地址 -e key=Sspanel_Mu_Key -e CacheDurationSec=120 \
--log-opt max-size=10m --log-opt max-file=5 \
--network=host --restart=always \
a3v8meq8wcqn2twa/a3v8meq:4.22.1.7
```
链接配置可选变量组 变量解释， 按需求删减。
```
webapi: -e usemysql=0  -e sspanel_url=网站WebAPI地址 -e key=Sspanel_Mu_Key
mysql: -e usemysql=1  -e MYSQLHOST=数据库ip地址 -e MYSQLDBNAME="demo_dbname" -e MYSQLUSR="demo_user" -e MYSQLPASSWD="demo_dbpassword" -e MYSQLPORT=3306
限制内存使用:--memory="300m"  --memory-swap="1g"
DDNS和自动TLS会用到的，需要cf域名： -e CF_Key=bbbbbbbbbbbbbbbbbb -e CF_Email=v2rayV3@test.com 或者 -e ALi_Key=sdfsdfsdfljlbjkljlkjsdfoiwje -e Ali_Secret=jlsdflanljkljlfdsaklkjflsa

流媒体DNS配置，填写解锁dns：-e LDNS=1.1.1.1
plugin需要与前端匹配环境变量， -e MUREGEX=%5m%id.%suffix -e MUSUFFIX=microsoft.com
haproxy 中转需要的 配置 -e ProxyTCP=1
```
### 一些命令
# 查看 logs
docker logs 昵称 --tail 100
## 第二种


### 获取access token

1. 跟着官方教程获取[access_token](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html)

#### docker-compose 方式安装

```
mkdir v2ray-agent  &&  \
cd v2ray-agent && \
curl --request GET --header 'PRIVATE-TOKEN: <your_access_token>' https://gitlab.com/api/v4/projects/16926132/repository/files/install.sh/raw?ref=master -o install.sh && \
chmod +x install.sh && \
bash install.sh

```

### 一些命令

请在 docker-compose.yml 同目录下执行。

```
# 更新、拉取 image
docker-compose pull

# 创建并启动容器，加上 -d 后台运行
docker-compose up

# 重启容器
docker-compose restart

# 停止容器
docker-compose stop

# 停止并删除容器
docker-compose down

# 查看 logs
docker-compose logs
```