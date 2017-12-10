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





