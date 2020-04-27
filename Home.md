# 需要呵护式技术支持请付费。
Welcome to the v2ray-sspanel-v3-mod_Uim-plugin wiki!
## Change log
4.22.1:


    4.22.1.13:

      1. 修复上传数据流量没统计的问题，影响版本 4.22.1.8-4.22.1.12， 鞠躬道歉
    4.22.1.12:
       
      1. useip改成useipv4默认

      2. 增加html_path 关键词，普通安装后自行修改config.json， docker的自己做image，自己挂载config.json和对应html文件。
   
      3. 增加CacheDurationSec自助配置，默认120，越小内存消耗小，但对时间要求高
    
      4. Aliyun DDNS 默认给了Aliyun信息就打开， 务必A记录

      5. 增加Aliyun 自动Tls
     
      6. 修复用户设备限制变化，而后端没有添加删除用户的问题
      
      7. 增加sock5的配置，写成socks;; 或者 socks;tls;, 考虑到udp部分，按照官网说法需要一个address，
         这里通过读取 额外参数nodeaddress=ip或者域名，（纯测试）

      8. 修复mysql对接问题，遗漏了dbname

      9. 发现自己的一个index对应map的更新傻逼问题，造成了index对应有偏差。
       
      10. 去掉了grpc依赖，写成feature形式
     
    4.22.1.11:
 
      1. 更新go1.13.4 to go1.14.2

    4.22.1.10:
     
      1. 更新依赖的库版本，希望能解决grpc的坑，如果不能，等我去掉grpc的版本
      2. 更换dns那的顺序，8.8.8.8为第一个，这样能够让其他的域名8.8.8.8解析，dns解锁的就解锁，

    4.22.1.9:
      
      1. 修改mux部分的逻辑判断，更好的在删除用户后直接断开连接

    4.22.1.8:

      1. 介入buf.copy部分，更早断掉失效用户io
      2. 上下对等限速

    4.22.1.7:

      1. 在ws，tcp,obfs-http上增加了 proxy protocol的支持，使用haproxy, 配置可以看(https://gitlab.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin/-/wikis/Haproxy-%E4%B8%AD%E8%BD%AC-Ha%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6) 可以获得客户端真实ip，不再是中转ip
      2. 尝试修复 plugin模式下 rule审计在ss那不生效，（因为我删掉了email，目前切成了两套email）

## 项目状态

支持 [ss-panel-v3-mod_Uim](https://github.com/NimaQu/ss-panel-v3-mod_Uim)，使用 WEBAPI 或 数据库连接。

亦支持 [SSRPanel](https://github.com/ssrpanel/SSRPanel)，使用数据库连接。

**作为 ss-panel-v3-mod 后端目前支持：**

- ss + ws (tls) 单端口
- ss + obfs-http 单端口 （测试ing）
- 限速
- 流量记录
- 在线人数
- 节点负载
- 流量中转
- 在线 IP 上报
- 服务器是否在线
- 后端根据前端的设定自动调用 API 增加用户。
- 域名审计，以及审计上报
- 限制ip访问
- 设置DNS

## 使用重点

1. ~后端端口设置为 0 或为空，才会监听本地，不再是 443。这个重写功能我还未提交pr到上游，请务必抄一下我的repo代码[ss-panel-v3-mod_Uim](https://github.com/v2rayv3/ss-panel-v3-mod_Uim)。~

## 安装使用

安装前请注意先行查看 [作为 SS 后端](https://gitlab.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin/-/wikis/%5B配置%5D-作为-SS-后端) 或 [作为 V2Ray 后端](https://gitlab.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin/-/wikis/%5B配置%5D-作为-V2Ray-后端) 使用，在前端面板添加好节点之后再进行后端的安装。

[docker image 制作](https://gitlab.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin/-/wikis/Docker-image%E5%88%B6%E4%BD%9C) 

[[推荐] docker 安装后端](https://gitlab.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin/-/wikis/docker-%E5%AE%89%E8%A3%85%E5%90%8E%E7%AB%AF)

[普通安装后端](https://gitlab.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin/-/wikis/普通安装后端)