# SSH

- sshd - The SSH server on Unix/Linux
- sshd_config - Server configuration file on Unix/Linux
- ssh_config - Client configuration file on Unix/Linux
- SSH port, and how it got that number

## sshd. - The SSH server

sshd is the OpenSSH server process. It listens to incoming connections using the SSH protocol and acts as the server for the protocol. It handles user authentication, encryption, terminal connections, file transfers, and tunneling.

**VPS 安装OS(linux)时，自动安装了；**
**macOS 用 brew 安装。**

## configuration file

全局配置
* /etc/ssh/sshd_config  (server)
* /etc/ssh/ssh_config   (client)

当前用户配置
* ~/.ssh/authorized_keys (server)
* ~/.ssh/config    
* ~/.ssh/known_hosts


---

# SSH端口转发

SSH有三种端口转发模式，**本地端口转发(Local Port Forwarding)，远程端口转发(Remote Port Forwarding)以及动态端口转发(Dynamic Port Forwarding)** 。对于本地/远程端口转发，两者的方向恰好相反。
**动态端口转发**则可以用于科学上网。

SSH端口转发也被称作SSH隧道(SSH Tunnel)，因为它们都是通过SSH登陆之后，在SSH客户端与SSH服务端之间建立了一个隧道，从而进行通信。SSH隧道是非常安全的，因为SSH是通过加密传输数据的(SSH全称为Secure Shell)。

在本文所有示例中，本地主机A1为SSH客户端，远程云主机B1为SSH服务端。从A1主机通过SSH登陆B1主机，指定不同的端口转发选项(-L、-R和-D)，即可在A1与B1之间建立SSH隧道，从而进行不同的端口转发。

## 本地端口转发

**应用场景:**
>远程云主机B1运行了一个服务，端口为3000，本地主机A1需要访问这个服务。

示例为一个简单的Node.js服务:
```
var http = require('http');
var server = http.createServer(function(request, response)
{
    response.writeHead(200,
    {
        "Content-Type": "text/plain"
    });
    response.end("Hello Fundebug\n");
});
server.listen(3000);
```
假设云主机B1的IP为**103.59.22.17**，则该服务的访问地址为:**http://103.59.22.17:3000**

### 为啥需要本地端口转发呢？
>一般来讲，云主机的防火墙默认只打开了22端口，如果需要访问3000端口的话，需要修改防火墙。为了保证安全，防火墙需要配置允许访问的IP地址。但是，本地公忘IP通常是网络提供商动态分配的，是不断变化的。这样的话，防火墙配置需要经常修改，就会很麻烦。

### 什么是本地端口转发？
>所谓本地端口转发，就是将发送到本地端口的请求，转发到目标端口。这样，就可以通过访问本地端口，来访问目标端口的服务。使用-L属性，就可以指定需要转发的端口，语法是这样的:
`-L 本地网卡地址:本地端口:目标地址:目标端口`
>通过本地端口转发，可以将发送到本地主机A1端口2000的请求，转发到远程云主机B1的3000端口。
```
# 在本地主机A1登陆远程云主机B1，并进行本地端口转发
ssh -L localhost:2000:localhost:3000 root@103.59.22.17
```
>这样，在本地主机A1上可以通过访问http://localhost:2000来访问远程云主机B1上的Node.js服务。



