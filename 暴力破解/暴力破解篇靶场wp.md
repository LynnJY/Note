### 思维导图

![image-20240131203323244](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226111.png)

### 基于表单的暴力破解

1.抓包

![image-20240131201114361](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226112.png)

2.发送到intruder后把账户密码这两个地方标记，payloads中写入密码本

![image-20240131201318979](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226113.png)

![image-20240131201339309](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226114.png)

开始爆破

![image-20240131201407577](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226115.png)

### 验证码绕过（on server）

1.抓包

![image-20240131201521378](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226116.png)

2.发送后标记账户密码验证码改为网站图标的正确验证码

写入密码本

![image-20240131201718074](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226117.png)

开始爆破

![image-20240131201742613](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226118.png)

### 验证码绕过（on client）

1.输入正确验证码才能抓到包后，利用该包发送到模块

![image-20240131201933633](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226119.png)

照旧写入密码本，爆破

![image-20240131202104236](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226120.png)

这里也可以直接禁用JS

![image-20240131202246154](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226121.png)

就和第一个一样简单了

### token防爆破

经过几次抓包发现本次token值就是上次服务器发送回来的token值，所以用递归来重定向token（题目默认知道账户名，所以还是两个变量，使用鱼叉是因为第二个变量token是每次都重定向）

1.先抓包

2.写入密码，并定义线程为1，因为每一次循环后重定向，不是单线程token引用就乱了

3.设置重定向

![image-20240131202851694](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226122.png)

爆破

![image-20240131202942532](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250319162226123.png)

