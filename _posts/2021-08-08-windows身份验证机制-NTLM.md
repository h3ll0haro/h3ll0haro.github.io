---
layout: post
title: windows身份验证机制-NTLM
categories: 内网安全
tags: 协议分析
---



## windows身份验证机制

windows的身份认证机制主要有两种方式

+ NTLM主要被用于本地和工作组环境
+ Kerberos主要被用于域环境

NTLM（NT LAN Manager）是windows中最常见的身份认证方式，主要有本地认证和网络认证两种情况

### 0x01 本地认证

在计算机本地认证中，当用户进行注销、重启、开机等需要认证的操作时，首先windows会调用winlogon.exe进程（也就是我们平常见到的登录框）接受用户的密码。之后密码会被传送给进程lsass.exe，该进程会先把明文密码存储到内存中，然后将加密后的密码也就是NTLM hash与windows本地的SAM数据库（%SystemRoot%\system32\config\SAM，词组security account manager，意思是安全账号管理器，其作用是对windows账户安全管理，类似于linux系统中的/etc/passwd文件,删除该文件即可删除账号密码。这也是PE破解系统密码的原理。）中该用户的NTLM Hash对比，如果一致则通过认证

![image-20210808193112004](https://cdn.jsdelivr.net/gh/h3ll0haro/img@master/uPic/image-20210808193112004.png)

NTLM Hash，正常的明文密码加密为NTLM Hash的方法：password -> 十六进制编码 -> Uncode 转换 -> MD4 加密 ，最后得到NTLM Hash

如：`admin -> hex(16进制编码) 61646d696e -> unicode编码（在每个字节后面补00） 610064006d0069006e00 -> MD4 209c6174da490caeb422f3fa5a7ae634`

![image-20210808192902599](https://cdn.jsdelivr.net/gh/h3ll0haro/img@master/uPic/image-20210808192902599.png)

### 0x02 网络认证

网络认证需要使用NTLM协议，改协议基于挑战(challenge)/响应(response)机制

1. 首先客户端向服务端发送用户名以及本机的一些信息进行协商，主要用于确认双方协议版本

2. 服务端接收到客户端的用户名，先生成一个随机的16位的challenge（挑战随机数），然后本地存储后将challenge返回给客户端

3. 客户端接收到服务端发来的challenge后，使用用户输入的密码的NTLM Hash对challenge进行加密生成response（也叫Net NTLM Hash），将response发送给服务端。

4. 服务端接收到客户端发来的response，使用数据库中对应用户的NTLM Hash对之前存储的Challenge进行加密，得到的结果与接受的response进行对比，如果一致则通过认证

![image-20210808193917246](https://cdn.jsdelivr.net/gh/h3ll0haro/img@master/uPic/image-20210808193917246.png)

### 0x03 域环境中的网络认证

域环境虽然首选默认Kerberos认证，但是也可以使用NTLM来进行认证，其认证过程与工作组差异不大，区别主要是最终在域控DC中完成验证

1. 首先客户端向服务端发送用户名以及本机的一些信息

2. 服务端接收到客户端的用户名，先生成一个随机的16位的challenge（挑战随机数），然后本地存储后将challenge返回给客户端

3. 客户端接收到服务端发来的challenge后，使用用户输入的密码的NTLM Hash对challenge进行加密生成response，将response发送给服务端。

4. 此时由于服务端找不到存储用户的NTLM hash，此时他会将用户名、和之前返回给客户端的Challenge以及客户端发来的Response一起发送给域控制器DC。

5. 域控制器DC用本地数据库(NTDS.dit)里面对应用户的NTLM Hash对challenge进行加密，并将结果与Response对比，然后将验证结果返回给服务端，并由服务端将结果消息返回给客户端

6. 如果验证结果正确，则Client可以成功访问到Server的服务

![image-20210808194119733](https://cdn.jsdelivr.net/gh/h3ll0haro/img@master/uPic/image-20210808194119733.png)

### 0x04 NTLM 的缺陷

用户的明文密码并没有在验证过程中出现，而是使用了密码的NTLM Hash，那么攻击者只要拿到了用户密码的NTLM Hash就可以冒充用户，使用复用凭证Hash进行攻击的方式，不需要解密明文密码就可以通过验证，这就是hash传递攻击(Pass the Hash)。