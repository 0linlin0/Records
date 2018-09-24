# 钓鱼wifi创建过程

只要有一个可以置入minitor模式的无线网卡就可以并不需要两个，首先先设置自己的web服务器 这里我选用了kail自带的apache

配置apache 其默认的web目录在 /var/www/html 下 修改index.html内容为
```php
<?php header(“Location:http://自定义的广告页面”)；exit；？>
```
修改index.html名称为index.php

>这里有个关于apache的小贴士 apache在kali上的配置文件在/etc/apache2/apache2.conf中反正网上说的httpd.conf我是没有找到 作为一个web服务器如果想将404都设置成一个默认页面的话 要设置.htaccess文件这个文件需要自己新建 在web根目录下新建输入要想让.htacess文件生效 要将配置文件中的AllowOverride None 改为AllowOverride All

>还有一点需要注意关于web服务器如果有多个网卡的话任何一个网卡的ip都可以访问到apache就像是apache工作在这些网卡之上

Web服务器端设置完毕后 要开启无线AP这里有一个工具 wifi-pumpkin 启动软件先设置iptables原来软件默认iptables 只有一个有用：
```
iptables -t nat -A POSTROUTING–out-interface $inet -j MASQUERADE
```
不过我们要将这个也要删除
加上另一个规则：
```
iptables -t nat -A PREROUTING -p tcp -mmultiport –dport 80,8080 -j DNAT –to 10.0.0.1
```
然后启动AP 这样不管在浏览器里面打入什么地址都自动显示````10.0.0.1````的首页
不过这样有些缺陷 这种方案对路由器可以上网没有问题，但是当路由器没有上网时，就会失效。彼时，用户在PC上敲击网址后，首先进行DNS查询，由于网络不通，DNS必然失败，从而导致PC压根就不会发送HTTP的报文出去。这样，就不存在数据的重定向。补充方案：一种可行的方案是，在没有上网时，通过DNS欺骗来达到重定向的目的。这时候就需要另一个工具 dnsspoof
在终端敲入命令：
```
dnsspoof –I wlan1 –f /etc/hosts
```
这里有两个需要注意的 一个是hosts文件的配置 ````10.0.0.1 * ````还有一个是```wlan1 ````这个参数 要是网关的网卡
大功告成

相关的理论解释：为什么在index.php中要写重定向的代码呢，这时我们就要提出一个问题，手机在连接一些公共wifi的时候上方会弹出登录到网络的提示，那么为什么会自动出现，知乎上有个这个问题“操作系统接驳网络连接设备后，都是怎样判断已经成功连入Interent？” 附上链接：[https://www.zhihu.com/question/21466991/answer/18365187](https://www.zhihu.com/question/21466991/answer/18365187)其中最主要的一点是在设备连接上一个网络后 会先进行对一个固定域名的http请求 域名请求之前要先有dns请求 这里也就解释了为什么还需要dnsspoof 如果遇到了重定向那么设备就判定需要登陆信息
Iptables的规则确保了所有http的请求都重定向到了网关的80端口 apache服务器返回重定向 其实这里还可以直接用DNS重定向的 但是考虑到每个设备都有dns的缓存 并不能很好的实现完全的重定向所以还是使用iptables 比较好
