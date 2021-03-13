---
title: "在wsl中使用宿主机器的蓝灯代理"
date: 2021-03-13T22:01:46+08:00
---


首先遇到的问题是wsl升级到wsl2后，无法ping到宿主机器。
解决方法：在防火墙设置里添加inbound规则。
具体参考 [github/wsl](https://github.com/microsoft/WSL/issues/4171#issuecomment-559961027)

由于蓝灯本身不支持从私有网络的流量，所以在使用privoxy将流量指向蓝灯。

1， 在wsl中，设置 
export host_ip=`cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }'`
host_ip 为宿主机器的ip.

2, 配置privoxy。
在配置文件中，添加

```toml
listen_address 0.0.0.0:8118
forward / 127.0.0.1:50222
```

将`50222`替换为自己的蓝灯的代理端口。

3, 在wsl中设置代理

export https_proxy=http://`$host_ip`:8118

然后，就可以在wsl中访问受限制的网络了。


---
参考：
- [WSL2 中访问宿主机 Windows 的代理](https://zinglix.xyz/2020/04/18/wsl2-proxy/)
- [Mac OS 下共享蓝灯给局域网或iPhone(ios)使用](https://seakee.top/skill/47.html)
- [github/wsl](https://github.com/microsoft/WSL/issues/4171#issuecomment-559961027)

