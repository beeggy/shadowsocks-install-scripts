## shadowsocks-install-scripts
[![build status](https://travis-ci.org/jsycdut/shadowsocks-install-scripts.svg?branch=master)](https://travis-ci.org/jsycdut/shadowsocks-install-scripts)

本项目用于在Debian, Ubuntu, Cent OS, Arch等Linux操作系统上安装Python版本的Shadowsocks Server端。

## 项目进度
- [x] 安装Shadowsokcs
- [x] Debian 8 
- [x] Debian 9
- [x] Ubuntu 14.04  
- [x] Ubuntu 16.04
- [x] Cent OS 6 
- [x] Cent OS 7
- [ ] Arch
- [x] Systemd for Shadowsocks
- [x] SysVinit for shadowsocks
- [ ] iptables规则

## 使用方法
**只需要顺序执行如下命令即可完成Shadowsocks的安装**
```bash
git clone https://github.com/jsycdut/shadowsocks-install-scripts
cd shadowsocks-install-scripts
sudo ./install.sh
```
**注意脚本需要root权限来运行。**
脚本会自动在你的Linux上安装Shadowsocks的server端，安装完成后脚本提示server端的IP，端口，密码和加密方式等信息，显示效果如下，这里除了IP和你的Linux服务器有关，其余全都是预设的值。

```
Shadowsocks started! Enjoy yourself!
Your Shadowsocks Serverside Information as below
Server IP:   xxxx
Server Port: 8388
Password:    https://github.com/jsycdut
Method:      aes-256-cfb
```
## 安装原理说明
* `install.sh`

1. 判断Linux的类别和版本，即RHEL系列还是Debian系列，前者包括Redhat、CentOS，后者包括Debian、Ubuntu，这是为了弄明白工作环境，因为运行Python版本的shadowsocks需要一些依赖，安装这个依赖的一个办法是使用包管理器，包管理器	取决于Linux发行版，比如RHEL系列的使用yum，Debian系列的使用apt-get或者apt。

2. 下载必要的文件，包括shadowsocks Python版本的源代码，用于加解密的libsodium库，用于网络优化的bbr算法脚本等。

3. 安装Shadowsocks

4. 为Shadowsocks添加启动脚本（只支持使用systemd作为启动脚本的系统）

5. 清理系统安装文件

* `add_user.sh`

1. 读取port和password数组的内容
2. 利用脚本添加shadowsocks用户

## 关于启动脚本

启动脚本用于开机启动shadowsocks服务运行，另外一方面支持使用service命令来管理shadowsocks的启动，结束，重启以及状态查询
```
# 针对sysvinit启动的系统
service shadowsocksd {start|stop|restart|status}

# 针对systemd启动的系统
service shadowsocks {start|stop|restart|status} 或者 systemctl {start|stop|restart|status} shadowsocks
```
在安装脚本中，对当前Linux的启动系统进行了判断，同时支持了sysvinit和systemd两种，安装完成后使用上面对应的命令即可进行服务的管理，一般Ubuntu 15.04+， CentOS 7+， Debian 8+等系统都是systemd，之前的系统都可以使用sysvinit。

如何判断当前是sysvinit还是systemd？ 使用`ps -p 1 | grep systemd && grep "systemd"`此条命令即可，如果输出systemd，那么当前系统就是由systemd启动的，如果没有任何输出，那么就不是systemd，就可以认定为sysvinit（其实还有一种upstart技术，但是sysvinit更古老，也仍然受支持）
