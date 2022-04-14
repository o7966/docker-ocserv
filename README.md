# 说明
## 感谢：
我fork了：https://github.com/Pezhvak/docker-ocserv  感谢作者。

原使用说明写的很好了，请阅读这里：https://github.com/Pezhvak/docker-ocserv/blob/master/README.md 

根据自己的需求做了一些修改：

```bash
# Dockerfile:增加了国内的源可以让下载快点🙈
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories \
    && apk update && apk add musl-dev iptables gnutls-dev gnutls-utils readline-dev libnl3-dev lz4-dev libseccomp-dev libev-dev
```

```bash
# config/ocserv.conf:放宽了链接数量限制、不允许DNS通过服务器转发、添加了静态路由，删除缺省路由。
max-clients = 100
max-same-clients = 10
tunnel-all-dns = false
#dns = 8.8.8.8
#dns = 4.2.2.4
#dns = 2001:4860:4860::8888
#dns = 2001:4860:4860::8844
route = 10.0.24.0/255.255.255.0
route = 172.16.0.0/255.240.0.0
```

```bash
# scripts/docker-entrypoint.sh 可能需要改一下：
# 宿主机我测试centos8需要用iptables-nft命令，因为宿主机的Iptables版本不同，导致alpine需要使用新版本nftable。如果宿主机是ubuntu2004则不用修改。
iptables -L -n
modprobe: can't change directory to '/lib/modules': No such file or directory
iptables v1.8.7 (legacy): can't initialize iptables table `filter': Table does not exist (do you need to insmod?)
Perhaps iptables or your kernel needs to be upgraded.
# 如果提示上面错误，需要修改为iptables-nft
# Enable NAT forwarding
iptables-nft -t nat -A POSTROUTING -j MASQUERADE
iptables-nft -A INPUT -p tcp --dport 443 -j ACCEPT
iptables-nft -A INPUT -p udp --dport 443 -j ACCEPT

iptables-nft -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
```

