# 简介：
需要一台具有公网ip的服务器，本文档已亚马逊云为例子。
在亚马逊上创建EC2 实例，在安全组上暴露出远程连接需要的端口，如下面文件中用到的6000和7000端口，都需要暴露出来。

2. 步骤一：下载和安装FRP
前往FRP的GitHub仓库：https://github.com/fatedier/frp/releases,如x86的linux服务器
```
wget https://github.com/fatedier/frp/releases/download/v0.53.0/frp_0.53.0_linux_amd64.tar.gz
```
下载适用于你的机器版本的FRP二进制文件。如选择frp_x.x.x_linux_amd64.tar.gz文件，其中x.x.x是版本号。
解压下载的压缩文件：
```
tar -zxvf frp_x.x.x_linux_amd64.tar.gz
```
步骤二：配置FRP
进入FRP解压后的目录：
```
cd frp_x.x.x_linux_amd64
```

编辑 frps.toml 文件，配置FRP服务器（内网中的一台计算机）：
```
bindPort = 10086
```

编辑 frpc.toml 文件，配置FRP客户端（外部网络的计算机）,其中serverPort要和服务端的bindPort保持一致：
```
serverAddr = "110.40.128.155"
serverPort = 10086

[[proxies]]
name = "test-tcp"
type = "tcp"
localIP = "127.0.0.1"
localPort = 80
remotePort = 6000

[[proxies]]
name = "vnc"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5900
remotePort = 6001
```
serverAddr 和 serverPort 是FRP服务器的地址和端口。
[web] 部分是一个示例，表示将本地的HTTP服务（本地IP：127.0.0.1，本地端口：80）映射到FRP服务器的某个端口（这里是6000）。你可以根据需要添加其他映射。
上述用到的10086，6000，6001端口都需要暴露出来。

步骤三：运行FRP
在FRP服务器上运行FRP服务器：
```
./frps -c frps.toml
```
在FRP客户端运行FRP客户端：
```
./frpc -c frpc.toml
```

查看对应的终端输出，可以看到frp是否连接上，确认呢连接上后，可以使用，ssh直接登陆，如
ssh -p 6000 xxx@xxx.xxx.xxx.xxx
