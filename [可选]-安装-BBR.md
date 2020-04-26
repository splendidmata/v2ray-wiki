来源：[Rat's](https://www.moerats.com/archives/387/)

---

支持系统：CentOS 6+、Debian 8+、Ubuntu 14+。

注意：该脚本在Vultr各个系统均测试通过，如果期间有出现任何问题，可向原作者反映帮助改善。

运行以下命令：

```
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```

---

Ubuntu 18.04 魔改 BBR 暂时有点问题，可使用以下命令安装：

```
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh"
apt install make gcc -y
sed -i 's#/usr/bin/gcc-4.9#/usr/bin/gcc#g' '/root/tcp.sh'
chmod +x tcp.sh && ./tcp.sh
```
