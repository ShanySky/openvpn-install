# openvpn-install

安装脚本的主要用法参见原项目[angristan/openvpn-install: Set up your own OpenVPN server on Debian, Ubuntu, Fedora, CentOS or Arch Linux. (github.com)](https://github.com/angristan/openvpn-install)

## 本分支补充的功能

本分支主要在CentOS7环境中增加了双因子验证的功能，另外修复了一些在自己的测试环境中发现的问题，以及针对自己的使用场景做了一些微调。

安装前询问中，最后一项即为询问是否需要启用双因子验证的功能，默认是开启。开启双因子验证后生成的客户端配置文件中会自动加入需要的配置项。

这个脚本中需要下从github上面下载easy-rsa的压缩包，以及 clone [GitHub - google/google-authenticator-libpam](https://github.com/google/google-authenticator-libpam)，考虑到在“某些”网络环境下大家直接下载或clone github的资料可能会出问题，因此提供了离线安装的功能。针对easy-rsa，只要提前下载好，并放在脚本同目录下，脚本会优先使用，压缩包的名称必须为 easy-rsa.tgz 。针对 google-authenticator-libpam ，需要下载 google-authenticator-libpam.tar.gz 的压缩包，名称上可以灵活些，只要是以 google-authenticator-libpam开头，tar.gz结尾即可，比如：google-authenticator-libpam.tar.gz，google-authenticator-libpam-1.09.tar.gz等。

## 背景说明

为了能在openvpn中启用双因子验证我在网上找了很多资料，也做了很多测试，不过大多数可能因为环境的差异都无法成功，后面从[OpenVPN with Google 2-factor authentication on CentOS 7 – Velenux Home Page (wordpress.com)](https://velenux.wordpress.com/2019/03/12/openvpn-with-google-2-factor-authentication-on-centos-7/)这篇帖子中找到方向，经过测试和调整最终成功实现。为了方便自己的使用，以及回馈从网上得到的帮助，特将此方法整合至此脚本中。

有啥问题可以交流，不过本人能力和时间有限，不一定能解决，请大家理解。最后祝大家好运！
