---
title: Linux下regex匹配高级用法
date: 2021-10-16 07:40:41
categories: shell
tags:
 - tech
 - shell
 - regex
---

- [背景](#背景)
- [常用方法](#常用方法)
- [xargs用法](#xargs用法)
	- [xargs参数](#xargs参数)
	- [xargs命令与管道符的区别：](#xargs命令与管道符的区别)
- [Find使用](#find使用)
	- [find 各参数performance比较](#find-各参数performance比较)
	- [区别find -exec and find | xargs](#区别find--exec-and-find--xargs)
		- [退出代码](#退出代码)
		- [案例](#案例)
			- [find -exec](#find--exec)
			- [find | xargs](#find--xargs)
- [Find 结论](#find-结论)
	- [参数的多少](#参数的多少)
	- [find -exec vs find | xargs](#find--exec-vs-find--xargs)
- [reference](#reference)
	- [find局限性](#find局限性)
- [ls + grep/egrep + xargs](#ls--grepegrep--xargs)
	- [grep and egrep用法](#grep-and-egrep用法)
		- [案例](#案例-1)


# 背景

最近由于需要大量处理csv数据文件，对于csv数据文件内容的处理当然优先选择python pandas module,但是对于服务器上海量文件的寻找,linux bash的原生态的find,grep比python显得优势更大。


# 常用方法

* find command
* ls + grep/egrep + xargs

# xargs用法 
先介绍xargs用法因为两者都需要用到 
## xargs参数
xargs的默认动作是echo，默认分隔符为空白字符（空格，TAB，换行符）。

```
-0，--null：以\0作为分隔符，接受到的特殊字符将当作文本符号处理；
-d：指定分段的分隔符，默认分隔字符为空白字符；
-a，--arg-file=file：指定命令标准输入的来源文件；
-e'FLAG' 或者-E 'FLAG'：指定一个终止符号，当xargs命令匹配到第一个FLAG后，停止传递，并退出命令；
-p：每当xargs执行一个分段时，询问一次用户是否执行；
-t：表示先打印执行的命令再输出；
-n NUM：表示一个分段包含的参数个数，参数之间以分隔符隔开，默认是将所有的参数当作一个分段输出；
-i：用于将分段分批传递给其后的{}进行输出，分段会替换{}所在的位置进行输出；
-I "FLAG"：可指定分段的替换符号，分段会分批替换到符号所在的位置进行输出执行；
-L：指定每次执行的最大的非空行的行数；
```
## xargs命令与管道符的区别：
管道符| 是将前一个命令的标准输出作为后一个命令的标准输入耳使用；而xargs命令是将前一个命令的标准输出作为后一个命令的参数而使用

  
# Find使用

对于常用的,比如我想寻找一些文件然后cp到特定路径下面

```
find path -name "yourname*.csv" -exec command {} /tmp/test/ \;
```

## find 各参数performance比较

```
du -hd0
 37G    .

# 'time' will print the total time taken for the command to finish

# time find -exec \;
time find . -name \*.php -type f -exec grep -Hn '$test' {} \;
real    1m24.433s
user    0m29.022s
sys     0m43.304s

# find -exec \+
time find . -name \*.php -type f -exec grep -Hn '$test' {} \+
real    0m13.050s
user    0m5.315s
sys     0m2.179s

# find | xargs -n1
time find . -name \*.php -type f -print0 | xargs -0 -n1 grep -Hn '$test'
real    0m55.159s
user    0m23.692s
sys     0m28.618s

# find | xargs
time find . -name \*.php -type f -print0 | xargs -0 -grep -Hn '$test'
real    0m12.047s
user    0m4.997s
sys     0m3.593s
```
得出结论find + and xargs 优于其他因为没有过多的fork和exec，减少了I/O的读写时间，可以看出大概快了6倍

## 区别find -exec and find | xargs
### 退出代码

- find -exec 返回退出代码在本身进程
- find | xargs 返回退出代码在子进程

### 案例
####  find -exec
```
find . -name \*.php -type f -exec php -l {} \;
...
PHP Parse error:  syntax error, unexpected '=' in ./test.php on line 3

Parse error: syntax error, unexpected '=' in ./test.php on line 3
Errors parsing ./test.php
...
No syntax errors detected in ./everythingcli/wordpress/wp-admin/network/user-new.php
No syntax errors detected in ./everythingcli/wordpress/wp-admin/network/users.php
No syntax errors detected in ./everythingcli/wordpress/wp-admin/network.php
No syntax errors detected in ./everythingcli/wordpress/wp-admin/options-discussion.php
No syntax errors detected in ./everythingcli/wordpress/wp-admin/options-general.php
No syntax errors detected in ./everythingcli/wordpress/wp-admin/options-head.php
No syntax errors detected in ./everythingcli/wordpress/wp-admin/options-media.php
...
```
可以看出尽管命令php -l报错 exit 1但是find -exec仍旧执行到最后且exit 0
#### find | xargs
```
find . -name \*.php -type f -print0 | xargs -0 -n1 php -l
...
PHP Parse error:  syntax error, unexpected '=' in ./test.php on line 3

Parse error: syntax error, unexpected '=' in ./test.php on line 3
Errors parsing ./test.php
```

你可以看出命令立即退出，这是因为子进程退出了这个是piping command设计的原理

# Find 结论
## 参数的多少
find和 xargs -n1 是否需要使用\; 如果子目录可以使用多个参数使用 +，如果子命令只支持一个参数使用\;和-n1

## find -exec vs find | xargs
find -exec 会继续执行没个文件尽管-exec报错，但find | xargs 会立即退出当子进程有错时，取决于你的需求

# reference 
https://www.everythingcli.org/find-exec-vs-find-xargs/

## find局限性
但是你会发现find也有其局限性
* 对于regex匹配很有限
* 输出不友好，比如./yourname.csv，你需要进行二次处理输出结果
有此我们引出第二种用法

# ls + grep/egrep + xargs

* ls 列出需要文件
* grep 运行regex正则匹配
* xargs 执行命令

## grep and egrep用法

匹配7位字母或者数字keyword

```
grep '^[a-zA-Z0-9]\{7\}$' /usr/share/wordlists/rockyou.txt
```

```
egrep '^.{7}$' /usr/share/wordlists/rockyou.txt
```


### 案例

找到一些特定文件名并复制到其他目录下，folder存在很多类似的文件
```
2019-12-01_db5xtqje90.csv
_2019-12-01_db5xtqje90.csv
_2019-12-01_db5xtqje90_processed.csv
2019-12-01_db5xtqje90.csv.dbfile.txt
2019-12-01_db5xtqje90_error.csv

ls | egrep '^.{40}$' | grep Revenue_2019-12 | xargs  -d "\n" cp -t /tmp/test/

# egrep '^.{40}$' 匹配40位任意字符长度的文件名
# xargs  -d "\n"  指定分隔符号为回车 
```

