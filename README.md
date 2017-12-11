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




