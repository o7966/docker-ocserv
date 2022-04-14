# 说明
个人远程运维使用相较于原作者修改了一下参数：

放宽了链接数量限制、不允许DNS通过服务器转发、添加了静态路由，删除缺省路由。

```bash
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
