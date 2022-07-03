---
title: 高盛投行运维岗位面试
date: 2021-09-09 16:49:49
categories: 面试
tags:
 - 面试
 - 技术
---

# Background

打算写一个面试专题，主要目的是一是分享，另一个是记录技术细节
以下是我转入产品运维后面试过的还记得的公司


Optiver
goldman sachs
jump trading
morgan stanley
bloomberg
shopee singapore
apple
paypal
Mastercard
Microsoft
tiktok singapore
IBM
支付宝
宜家
迪卡侬
特斯拉
NVIDIA




# 面试流程

一共1.5小时全英文，分为三个部分

* behave questions
* Linux questions
* coding skill


# 面试题

## behave questions

我在这边就不分享了，纯属于情商题，不难，没有唯一答案，个人观念不同，可以都对，根据你的工作经验总结出来。

## Linux questions

> 如何查询一个服务器跑了哪些应用

```
ps command, 

ps aux
```

> ps command 输出了哪些信息

```
username,

```


> 如何检查一个服务器网络连接情况

```
netstat -altp 

```

> 解释下 netstat 命令输出 state 种类和各个的含义

```
listen
time_wait
eslaticed 
```

> PPID是什么，如果kill PPID 会有啥影响


> pid存在于系统哪个目录下，目录内都有什么内容


> 文件重定向

cat.sh > text.txt

/dev/stdin 标准输入设备（键盘） 0

/dev/stdout 标准输出设备（显示器） 1

/dev/stderr 标准错误输出设备（显示器） 2


> linux系统如何删除文件名为-f的文件

```
rm -- -foo
rm ./-foo
```

# coding question

```
41.42.97.104 - - [26/Feb/2015:03:35:40 -0500] "GET /root/ HTTP/1.1" 301 20 "http://baibai.123.com/09" "Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.115 Safari/537.36" - 0.562 
41.42.97.104 - - [26/Feb/2015:03:35:41 -0500] "GET /crossadkla.xml HTTP/1.1" 304 0 "https://baibai.123.com/" "Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.115 Safari/537.36" - 0.000 
99.122.189.203 - - [26/Feb/2015:03:35:42 -0500] "GET /root/ HTTP/1.1" 301 20 "http://baibai.123.com/11" "Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.115 Safari/537.36" - 0.562 
99.122.189.203  - - [26/Feb/2015:03:35:44 -0500] "GET /crossadkla.xml HTTP/1.1" 304 0 "https://baibai.123.com/" "Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.115 Safari/537.36" - 0.000
99.122.189.203  - - [26/Feb/2015:03:35:44 -0500] "GET /crossadkla.xml HTTP/1.1" 304 0 "https://baibai.123.com/" "Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.115 Safari/537.36" - 0.000
```

问题 统计访问最多的2个IP地址并打印出来

