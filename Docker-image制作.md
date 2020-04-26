此教程面向所有层级的开发者和初学者，请严格仔细核对安装流程，依次执行操作，感谢rico大佬，如有错误欢迎tg群内反馈，<br>ps：我语文一般

P.前提准备
A.在 [hub.docker.com](https://hub.docker.com/) 注册一个帐号，接下来我们要用到这个帐号的username等东西<br>
B.(最低)一台性能配置在1GB RAM  1核 的KVM框架的VPS上，位置建议在美国，速度会快点，避免卡代码<br>
C.系统建议Centos 7.0 64bit(本文教程环境)<br>
D.不怕麻烦<br>

0. 安装 docker <br>
 `
curl -fsSL https://get.docker.com -o get-docker.sh  && 
bash get-docker.sh
 `

1. 重启docker防止出错<br>
`service docker restart`

2. git clone 本项目  <br>
`git clone https://gitlab.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin.git`  
<br>

0. **注意:若提示git错误请执行 git安装(无错误请忽略)**<br>
`yum -y install git`<br>
更新后执行git clone<br>
`git clone https://gitlab.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin.git`
<br>

3. 从TG群“存档”内下载二进制文件（比如v2ray-4.22.1.2.zip），重命名成v2ray-linux-64.zip  <br>
具体可以看一看clone下来的项目的Docker/alpine_fixed/Dockerfile，第七行：  <br>
`COPY v2ray-linux-64.zip /tmp/v2ray-linux-64.zip`  <br>
所以要改名<br>
CD到项目pay-v2ray-sspanel-v3-mod_Uim-plugin目录下，执行 <br>
`cd pay-v2ray-sspanel-v3-mod_Uim-plugin`<br>
CD到文件上传目录Docker/alpine_fixed，执行<br>
`cd Docker/alpine_fixed`<br>
把改完名的压缩包放入 Docker/alpine_fixed 目录下<br>
**注意:这里为了避免错误，推荐xftp或者finalshell进行二进制传输**<br>
<br>

4. 执行docker build  生成镜像<br>
`docker build . -t username/repo:tag` <br>
这里的username就是[hub.docker.com](https://hub.docker.com/)注册的username, repo和tag分别自行命名<br>
举例:  docker build . -t rico/image:v2ray           //rico为docker hub注册用的用户名<br>
**注意:这里可能会卡代码，卡住就是垃圾机子，换机子从来，去看前提**<br>
<br>

5. 登录docker，执行<br>
`docker login`<br>
用户名 username，    //举例:rico<br>
密码，注册docker hub时的password<br>
登录成功后继续以下操作<br>
<br>

6. docker push  执行<br>
`docker push username/repo:tag` <br>
username/repo:tag同上一步, 推送你制作的image到你刚才命名的repo:tag仓库  <br>
举例:  docker push rico/image:v2ray           //rico为docker hub注册用的用户名<br>
<br>

7. 打开你的[docker hub用户中心看镜像]   https://hub.docker.com/u/用户名  <br>
镜像存在即可，完成制作，整个镜像在33.75 MB


8. 之后所有docker相关 项目地址都改成上述 username/repo:tag  <br>
比如clone的项目下 Docker/V2ray中的 docker-compose.yml 第五行<br>
这个不细讲，有问题群内提问，附上截图<br>
```
version: '2'

services:
  v2ray:
    image: xxx/xxx:xxx //这里的xxx/xxx:xxx改成上述的username/repo:tag
    restart: always
    network_mode: "host"
    environment:
```


教程完结，晚安宝贝们！
