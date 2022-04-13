# 网络分析工具

## ping

package internet groper 因特网包探索器，ping 属于一个通信协议，是 TCP/IP 协议的一部分，利用
ping 命令可以检查网络是否连通

应用格式，ping 空格 ip 地址

ping 发送一个 ICMP(因特网信报控制协议)，它利用的原理是，利用网络上机器 IP 地址的唯一性，给目标 IP 地址
发送一个数据包，再要求对方返回一个同样大小的数据包来确定两台网络机器是否连接相通以及时延是多少

ping 指的是端到端的连通，通常用来作为可用性的检查，但是某些病毒木马会强行大量远程执行 ping 命令来抢占
你的网络资源，导致系统、网速变慢。严禁 ping 入侵作为大多数防火墙的一个基本功能提供给用户进行选择。通常的
情况下你如果不用作服务器或者进行网络测试，可以放心的选中它，以保护你的计算机

- ping 127.0.0.1 可以检查本地的 TCP/IP 协议有没有设置好

- ping 本机 ip 地址，可以检查本机的 IP 地址是否设置有误

- ping 本网网关或本网 IP 地址，这样可以检查硬件设备是否有问题

- ping 本地 DNS 地址，这样可以检查本地 DNS 服务器是否工作正常

- ping 远程 ip 地址，可以检查本网或本机与外部的连接是否正常

## tcpdump

tcpdump 可以过滤报文。如果没有给出任何条件，则网络上所有的信息包将会被截获

- 关于类型的关键字，主要包括 host、net、port 等，例如`host 210.27.48.2`即指明`210.27.48.2`是
  一台主机，`net 202.0.0.0`指明`202.0.0.0`是一个网络地址，`port 23`指明端口号是 23。如果没有
  指明类型，默认的类型是 host

- 确定传输方向的关键字，主要包括`src, dst, dst or src`等，这些关键字指明了传输的方向，例如`src 210.27.48.2`
  指明 IP 包中源地址为`210.27.48.2`，`dst net 202.0.0.0` 指明目的网络地址是 202.0.0.0

- 协议的关键字，主要包括`fddi, ip, arp, rarp, tcp, upd`等类型，如果没有指定任何协议，则 tcpdump 将会监听
  所有协议的信息包

**截取某主机相关的包**

```
tcpdump host 210.27.48.1
```

截取所有 210.27.18.1 的主机收到的和发出的所有的数据包

截取多主机，在命令行中使用括号时，要添加`\`

```
tcpdump host 210.27.48.1 and \( 210.27.48.2 or 210.27.48.3 \)
```

如果想要获取主机 210.27.48.1 接收或发出的 telnet 包

```
tcpdump tcp port 23 host 210.27.48.1
```

截取某端口相关的包

```
tcpdump port 6666
```

截取某网卡的包

```
tcpdump -ieth1
```

<!-- 110.242.68.4 -->

## netstat

netstat 命令用于显示于 IP、TCP、UDP、ICMP 协议相关的统计数据

- 列出所有的端口，包括监听的和未监听的`netstat -a`

- 列出所有 TCP 端口`netstat -at`

- 列出所有 UDP 端口`netstat -au`

- 列出所有处于监听状态的 socket`netstat -l`

- 列出所有监听 TCP 端口的 socket `netstat -lt`

- 列出所有监听 UNIX 端口的 socket `netstat -lx`

- 在 netstat 输出中显示 PID 和进程名称 `netstat -p`

- 找出程序运行的端口`netstat -ap | grep ssh`

- 找出运行的指定端口的进程`netstat -an | grep '.80'`

## lsof

lsof(list open file) 是一个列出当前系统打开的文件的工具。在 Linux 环境下，任何事物都是以文件的形式存在

查看 6666 端口现在运行情况

```
lsof -i :6666
```

查看 root 用户所有程序打开的文件，文件类型为`.txt`

```
lsof -a -u root -d txt
```

监控打开的文件和设备

<!-- /Users/chengqunzhong/Desktop/开发区/性能/Snip20210702_3.png -->

```
lsof /path/to/Snip20210702_3.png
```

监控程序，如查看指定程序 server 打开的文件

```
lsof -c server
```

监控用户，查看指定用户打开的文件

```
lsof -u root
```
