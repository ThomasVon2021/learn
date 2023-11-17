# 简介：
需要一台具有公网ip的服务器，本文档已亚马逊云为例子。
在亚马逊上创建EC2 实例，在安全组上暴露出远程连接需要的端口，如下面文件中用到的6000和7000端口，都需要暴露出来。

2. 步骤一：下载和安装FRP
前往FRP的GitHub仓库：https://github.com/fatedier/frp。
下载适用于你的CentOS版本的FRP二进制文件。可以选择frp_x.x.x_linux_amd64.tar.gz文件，其中x.x.x是版本号。
解压下载的压缩文件：
bash
Copy code
tar -zxvf frp_x.x.x_linux_amd64.tar.gz
步骤二：配置FRP
进入FRP解压后的目录：

bash
Copy code
cd frp_x.x.x_linux_amd64
复制 frpc.ini（客户端配置） 和 frps.ini（服务器配置）的模板文件，并重命名为 frpc.ini 和 frps.ini。

bash
Copy code
cp frpc.ini.sample frpc.ini
cp frps.ini.sample frps.ini
编辑 frps.ini 文件，配置FRP服务器（内网中的一台计算机）：

ini
Copy code
[common]
bind_port = 7000
编辑 frpc.ini 文件，配置FRP客户端（外部网络的计算机）：

ini
Copy code
[common]
server_addr = your_server_ip
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 80
remote_port = 6000
server_addr 和 server_port 是FRP服务器的地址和端口。
[web] 部分是一个示例，表示将本地的HTTP服务（本地IP：127.0.0.1，本地端口：80）映射到FRP服务器的某个端口（这里是6000）。你可以根据需要添加其他映射。
步骤三：运行FRP
在FRP服务器上运行FRP服务器：

bash
Copy code
./frps -c frps.ini
在FRP客户端运行FRP客户端：

bash
Copy code
./frpc -c frpc.ini

查看对应的终端输出，可以看到frp是否连接上，确认呢连接上后，可以使用，ssh直接登陆，如
ssh -p 6000 xxx@xxx.xxx.xxx.xxx
