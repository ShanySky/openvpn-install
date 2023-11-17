# 本分支说明

安装脚本的主要用法参见原项目[angristan/openvpn-install: Set up your own OpenVPN server on Debian, Ubuntu, Fedora, CentOS or Arch Linux. (github.com)](https://github.com/angristan/openvpn-install)  

本分支主要在CentOS7环境中增加了双因子验证的功能，主要参考了[OpenVPN with Google 2-factor authentication on CentOS 7 – Velenux Home Page (wordpress.com)](https://velenux.wordpress.com/2019/03/12/openvpn-with-google-2-factor-authentication-on-centos-7/)这篇帖子。另外修复了一些在自己的测试环境中发现的问题，以及针对自己的使用场景做了一些微调。

## 1 新增功能

1. 提供了开启双因子验证的功能；
   
   *说明：在安装前的询问阶段，最后一个询问项即为是否开启双因子验证，默认开启。开启后在新建用户时会自动在客户端的配置文件中添加必要配置项*

2. 增加了控制客户端访问是否全经过VPN的功能；
   
   *说明：在原版脚本中，客户端的所有网络访问全经过VPN，在本分支版本中可以选择关闭该功能，默认选项是关闭。关闭时，只有在访问服务端网段的IP时客户端会走VPN，适用只希望通过VPN让客户端能访问服务端侧资源的场景。*

## 2 修复问题

1. 解决在CentOS环境中无法正常添加防火墙信息的问题。不再通过启动系统服务的方式添加，改为通过开机执行脚本的方式；

2. 解决移除用户后无法再添加同名用户的问题；

## 3 调整内容

1. 将加密设置中控制通道的安全层默认选项由 tls-crypt 改为 tls-auth；
   
   *说明：在OpenVPN社区资源的[强化VPN安全 ](https://openvpn.net/community-resources/hardening-openvpn-security/)的帖子中建议启用 tls-auth 功能*

2. 在CentOS环境下，如果在安装时检测到已安装openssl，则跳过openssl的安装；
   
   *说明：主要是为了避免覆盖掉系统中已安装的高版本的openssl*

3. 升级easy-rsa，由3.1.2升级为3.1.5；

4. 在服务端配置中增加重新协商密钥的间隔时间参数“reneg-sec”，间隔时间调整为3小时；

5. 增加一个不设置DNS的安装选项，同时将其设置为默认选项。

## 4 离线安装

脚本中有两个组件需要从github下载或clone，在某些网络环境下访问github可能会出现连接不上或是不稳定的问题，因此在本脚本中允许提前下载这两个组件的压缩包，提供了离线安装的功能。  

1. 提供 easy-rsa 离线安装的功能。允许提前下载 easy-rsa.tgz 压缩包，保存在脚本所在目录下，脚本检测到后会优先使用。需要注意的是，压缩包名称必须为 easy-rsa.tgz；

2. 提供 google-authenticator-libpam 离线安装的功能。  
   
   *说明：google-authenticator-libpam 是开启双因子验证的必备组件，脚本中通过 clone [GitHub - google/google-authenticator-libpam](https://github.com/google/google-authenticator-libpam) 编译安装。压缩包的名称只要以 google-authenticator-libpam 开头，.tar.gz 结尾即可，例如：google-authenticator-libpam.tar.gz、google-authenticator-libpam-1.09.tar.gz*

## 5 常用命令

1. 启动openvpn-server服务：systemctl start openvpn-server@server

2. 停止openvpn-server服务：systemctl stop openvpn-server@server

3. 重启openvpn-server服务：systemctl restart openvpn-server@server
