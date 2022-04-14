# 说明

`docker-entrypoint.sh`文件的字符集需要注意，例如回车换行符别出问题，否则脚本无法运行。

宿主机我测试centos8需要用iptables-nft命令，因为宿主机的Iptables版本不同，导致alpine需要使用新版本nftable。如果宿主机是ubuntu2004则不用修改。

```bash
# Enable NAT forwarding
iptables -t nat -A POSTROUTING -j MASQUERADE
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
iptables -A INPUT -p udp --dport 443 -j ACCEPT

iptables -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
```

```bash
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

