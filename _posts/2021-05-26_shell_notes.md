

- [变量](#变量)
  - [局部变量](#局部变量)
  - [环境变量](#环境变量)
  - [位置参数](#位置参数)
- [特殊符号](#特殊符号)
  - [括弧](#括弧)
    - [小括号](#小括号)
    - [大括号](#大括号)
  - [命令执行()和{}区别](#命令执行和区别)
  - [单引号双引号反引号](#单引号双引号反引号)
  - [命令序列](#命令序列)
- [条件语句](#条件语句)
  - [基元和组合表达式](#基元和组合表达式)
  - [`if` statement](#if-statement)
  - [`case`](#case)
- [循环](#循环)
  - [`for` loop](#for-loop)
  - [`while` loop](#while-loop)
  - [`until`](#until)
  - [循环控制](#循环控制)
- [数组](#数组)
- [函数](#函数)
- [shell 扩展](#shell-扩展)
  - [bash 下的 split 取“数组”的首、尾：](#bash-下的-split-取数组的首尾)
  - [bash 下的 replace 与 replaceAll](#bash-下的-replace-与-replaceall)
  - [bash 下的变量空值检测与初始化](#bash-下的变量空值检测与初始化)
  - [bash 下的大小写变换](#bash-下的大小写变换)
- [Reference](#reference)


# 变量
## 局部变量
**局部变量** 是仅在某个脚本内部有效的变量。它们不能被其他的程序和脚本访问。

局部变量可以用`=`声明（作为一种约定，变量名、`=`、变量的值之间 **不应该** 有空格），其值可以用`$`访问到。举个例子：

```bash
username="denysdovhan"  # 声明变量
echo $username          # 输出变量的值
unset username          # 删除变量
```
我们可以用`local`关键字声明属于某个函数的局部变量。这样声明的变量会在函数结束时消失。

```bash
local local_var="I'm a local value"
```


## 环境变量

**环境变量** 是对当前shell会话内所有的程序或脚本都可见的变量。创建它们跟创建局部变量类似，但使用的是`export`关键字。

```bash
export GLOBAL_VAR="I'm a global variable"
```
bash中有 _非常多_ 的环境变量。你会非常频繁地遇到它们，这里有一张速查表，记录了在实践中最常见的环境变量。

| Variable     | Description                                                   |
| :----------- | :------------------------------------------------------------ |
| `$HOME`      | 当前用户的用户目录                                              |
| `$PATH`      | 用分号分隔的目录列表，shell会到这些目录中查找命令                 |
| `$PWD`       | 当前工作目录                                                   |
| `$RANDOM`    | 0到32767之间的整数                                             |
| `$UID`       | 数值类型，当前用户的用户ID                                      |
| `$PS1`       | 主要系统输入提示符                                              |
| `$PS2`       | 次要系统输入提示符 

## 位置参数

**位置参数** 是在调用一个函数并传给它参数时创建的变量。下表列出了在函数中，位置参数变量和一些其它的特殊变量以及它们的意义。

| Parameter      | Description                                                 |
| :------------- | :---------------------------------------------------------- |
| `$0`           | 脚本名称                                                     |
| `$1 … $9`      | 第1个到第9个参数列表                                          |
| `${10} … ${N}` | 第10个到N个参数列表                                           |
| `$*` or `$@`   | 除了`$0`外的所有位置参数                                      |
| `$#`           | 不包括`$0`在内的位置参数的个数                                 |
| `$FUNCNAME`    | 函数名称（仅在函数内部有值）                            |

在下面的例子中，位置参数为：`$0='./script.sh'`，`$1='foo'`，`$2='bar'`：

    ./script.sh foo bar

变量可以有 _默认_ 值。我们可以用如下语法来指定默认值：

```bash
 # 如果变量为空，赋给他们默认值
: ${VAR:='default'}
: ${$1:='first'}
# 或者
FOO=${FOO:-'default'}
```

# 特殊符号
## 括弧
```bash
[] #test命令的alias,里面不可以用比较符，比如=，所以有-ne -eq etc.

$[] #现在几乎不用了，忽略

[[ ]] #[]可以用的它可以用，还可以 && 和 ||，还可以搞正则表达式

() #( COMMAND [; ...] )把命令包起来，在subshell里面运行，你切换目录什么不会影响到这一层

$() #$( COMMAND [; ...] )执行命令的结果，比如$(ls)和ls一样

(( )) #整数运算， (( a > 0 ))、(( a = ((c = 5) / 2) << 3 ))、(( 5 * 6 % 2)) 之类的全都行。算完了之后给你返回一个值，也就是 0 是假，其余全部是真。

$(( )) #基本不用了，和(())差不多，区别是把运算结果输出，比如[ $(( a % 2)) -ne 0 ] 

{} # { COMMAND; [... COMMAND; ] } 一个不把命令放在 subshell 里面的包裹。这样你可以重定向一堆命令的输入或者输出，但是仍然可以修改变量

${} #常用语划分变量名字区间  比如 ${_name}
```

引用其他blog介绍
```bash
2.1 () 在子shell中运行
    (a=1);echo $a，结果是空，因为a=1不是在当前shell中运行的(a=1);(echo $a)也是空的。
    小技巧：(cd $path, do something) 可以让不切换当前目录而在其它目录干点别的事儿~
    () 还有个功能是数组的赋值：比如a=(1 3 5)，那么${a[0]}=1;${a[1]}=3;${a[2]}=5，需要注意的是，下标是从0开始的。
    
2.2 (()) 表达式计算
    a=1;((a++));echo $a，这时a就是2了。
    
2.3 <() 和 >() 进程代入，可以把命令的执行结果当成文件一样读入
    比如comm前一般需要sort，那就可以这样comm <(sort 1.lst) <(sort 2.lst)
    或者是paste <(cut -t2 file1) <(cut -t1 file1)，和管道差不多，但是支持多个输入。

2.4 $() $(cmd) 执行cmd的结果，
    比如cmd是echo ls，那么就是执行ls，比如file $(which bash)，which bash的结果是/bin/bash，
    所以file $(which bash)等于file /bin/bash。如果你$(ls)，而且你的当前目录下只有a b两个文件，
    那么就是执行a b，然后系统会提示，命令没找到。$() 基本和 `` 等价。
    
2.5 $(()) 表达式扩展，
    和(())很相似，但是这个是有点不同，$(())不能直接$((b++))，例如：b=1;echo $((++b))
    这时b等于2，显示的也是2，b=1; echo $((b++))这时b等于2，显示的是1.
    
2.6 [] 和 [[]]，[] 就是 test，[]和[[]]都是条件表达式，不过[[]]有比[]高的容错性，
    如果a为空，那么[ $a -eq 0 ]会报错，但是[[ $a -eq 0 ]]不会，所以一般都会使用[[]]或者是
    [ "$a" -eq 0 ]，[[]]支持的功能也比 [] 多，比如[[ aaa =~ a{3} ]]，[] 还有一种用途，
    如果你的当前目录下有a1-a9九个文件，你可以用a[1-9]来替代这九个文件。
    有点需要注意，你不能用a[1-20]来代替a1- a20，必须要a[1-9] a1[0-9] a20。
    但是需要注意的是 [[]] 数字进制转换的坑~
    
2.7 $[] 是 $(()) 的过去形式，现在已经不建议使用。

2.8 {n..m} {1..30} 就是1-30，或者是/{,s}bin/表示/bin/和/sbin/，ab{c,d,e}表示abc、abd、abe，
    小技巧：文件备份：cp a.sh{,.bak}
    而 { cmd1; cmd2; } 的作用是定义一个命令组，一般用在单行的条件表达式中：
    [[ 1 -eq 2 ]] && echo True || { echo False; echo "Program will exit！"; }
    其实 shell 函数的语法也是它的变体：
    a(){ i=$1; echo $((i++)); echo $((++i)); } && a 1

2.9 ${} 变量的Parameter Expansion，用法很多，最基本的 ${var}，防止变量扩展冲突，具体可以查看man bash。  
    && { COMMAND; [... COMMAND; ] } 一个不把命令放在 subshell 里面的包裹。这样你可以重定向一堆命令的输入或者输出，但是仍然可以修改变量
```

### 小括号
()：用于一串命令的执行时，()中的命令会在子shell中运行。
$():和反引号作用一样，用来引用系统命令。
```bash
now=`date +%T`
# or
now=$(date +%T)

echo $now # 11:08:00
```
算数扩展
```bash
result=$(( ((10 + 5*3) - 7) / 2 ))
echo $result # 9
```
### 大括号
生成字符串
```bash
echo beg{i,a,u}n # begin began begun
echo {0..5} #0 1 2 3 4 5
echo {00..8..2} # 00 02 04 06 08
```
{}变量命名，引用
```bash
#可以加{}也可以不加，{}有助于识别边界
your_name="hello"
echo ${your_name}
hello
echo $your_name
hello
```
[]:用于变量的测试。
[[]]:带特殊字符的测试
## 命令执行()和{}区别
()和{}都是对一串的命令进行执行,但有所区别：

* 相同点：
()和{}都是把一串的命令放在括号里面,并且命令之间用;号隔开
* 不同点
()只是对一串命令重新开一个子shell进行执行,{}对一串命令在当前shell执行
()最后一个命令可以不用分号,{}最后一个命令要用分号
()里的第一个命令和左边括号不必有空格,{}的第一个命令和左括号之间必须要有一个空格
()和{}中括号里面的某个命令的重定向只影响该命令,但括号外的重定向则影响到括号里的所有命令
```bash
[root@corp-centos-101 ~]# var=tes01
[root@corp-centos-101 ~]# echo $var
tes01
[root@corp-centos-101 ~]# (var=test2;echo $var)
test2
[root@corp-centos-101 ~]# echo $var
tes01
[root@corp-centos-101 ~]# { var=test2;echo $var; }
test2
[root@corp-centos-101 ~]# echo $var
test2
```
在{}中 第一个命令和{之间必须有空格,结束必须有;

{}中的修改了$var的值 说明在当前shell执行


## 单引号双引号反引号
单引号
* 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
* 单引号字串中不能出现单引号（对单引号使用转义符后也不行）

```bash
name="old boy"
echo "$name"
old boy
echo '#name'
#name
```

双引号
* 双引号里可以有变量
* 双引号里可以出现转义字符


反引号` `
* 用来执行系统命令
  
```bash
local_time=`date`
echo $local_time
Wed Mar 17 20:19:30 EDT 2021
bash
```

*区别*
单引号和双引号之间有很重要的区别。在双引号中，变量引用或者命令置换是会被展开的。在单引号中是不会的。举个例子：

```bash
echo "Your home: $HOME" # Your home: /Users/<username>
echo 'Your home: $HOME' # Your home: $HOME
```

当局部变量和环境变量包含空格时，它们在引号中的扩展要格外注意。随便举个例子，假如我们用`echo`来输出用户的输入：

```bash
INPUT="A string  with   strange    whitespace."
echo $INPUT   # A string with strange whitespace.
echo "$INPUT" # A string  with   strange    whitespace.
```

调用第一个`echo`时给了它5个单独的参数 —— $INPUT被分成了单独的词，`echo`在每个词之间打印了一个空格。第二种情况，调用`echo`时只给了它一个参数（整个$INPUT的值，包括其中的空格）。

来看一个更严肃的例子：

```bash
FILE="Favorite Things.txt"
cat $FILE   # 尝试输出两个文件: `Favorite` 和 `Things.txt`
cat "$FILE" # 输出一个文件: `Favorite Things.txt`
```

尽管这个问题可以通过把FILE重命名成`Favorite-Things.txt`来解决，但是，假如这个值来自某个环境变量，来自一个位置参数，或者来自其它命令（`find`, `cat`, 等等）呢。因此，如果输入 *可能* 包含空格，务必要用引号把表达式包起来。

## 命令序列

命令序列是由`;`，`&`，`&&`或者`||`运算符分隔的一个或多个管道序列。

如果一个命令以`&`结尾，shell将会在一个子shell中异步执行这个命令。换句话说，这个命令将会在后台执行。

以`;`分隔的命令将会依次执行：一个接着一个。shell会等待直到每个命令执行完。

```bash
# command2 会在 command1 之后执行
command1 ; command2

# 等同于这种写法
command1
command2
```

以`&&`和`||`分隔的命令分别叫做 _与_ 和 _或_ 序列。

_与序列_ 看起来是这样的：

```bash
# 当且仅当command1执行成功（返回0值）时，command2才会执行
command1 && command2
```

_或序列_ 是下面这种形式：

```bash
# 当且仅当command1执行失败（返回错误码）时，command2才会执行
command1 || command2
```

_与_ 或 _或_ 序列的返回值是序列中最后一个执行的命令的返回值。


# 条件语句

跟其它程序设计语言一样，Bash中的条件语句让我们可以决定一个操作是否被执行。结果取决于一个包在`[[ ]]`里的表达式。

条件表达式可以包含`&&`和`||`运算符，分别对应 _与_ 和 _或_ 。除此之外还有很多有用的[表达式](#基元和组合表达式)。

共有两个不同的条件表达式：`if`和`case`。
## 基元和组合表达式

由`[[ ]]`（`sh`中是`[ ]`）包起来的表达式被称作 **检测命令** 或 **基元**。这些表达式帮助我们检测一个条件的结果。在下面的表里，为了兼容`sh`，我们用的是`[ ]`。这里可以找到有关[bash中单双中括号区别](http://serverfault.com/a/52050)的答案。

**跟文件系统相关：**

| 基元       | 含义                                                      |
| :-----------: | :----------------------------------------------------------- |
| `[ -e FILE ]` | 如果`FILE`存在 (**e**xists)，为真                             |
| `[ -f FILE ]` | 如果`FILE`存在且为一个普通文件（**f**ile），为真                |
| `[ -d FILE ]` | 如果`FILE`存在且为一个目录（**d**irectory），为真               |
| `[ -s FILE ]` | 如果`FILE`存在且非空（**s**ize 大于0），为真                    |
| `[ -r FILE ]` | 如果`FILE`存在且有读权限（**r**eadable），为真                  |
| `[ -w FILE ]` | 如果`FILE`存在且有写权限（**w**ritable），为真                  |
| `[ -x FILE ]` | 如果`FILE`存在且有可执行权限（e**x**ecutable），为真            |
| `[ -L FILE ]` | 如果`FILE`存在且为一个符号链接（**l**ink），为真                |
| `[ FILE1 -nt FILE2 ]` | `FILE1`比`FILE2`新（**n**ewer **t**han）                  |
| `[ FILE1 -ot FILE2 ]` | `FILE1`比`FILE2`旧（**o**lder **t**han）                  |

**跟字符串相关：**

| 基元        | 含义                                                     |
| :------------: | :---------------------------------------------------------- |
| `[ -z STR ]`   | `STR`为空（长度为0，**z**ero）                               |
| `[ -n STR ]`   | `STR`非空（长度非0，**n**on-zero）                           |
| `[ STR1 == STR2 ]` | `STR1`和`STR2`相等                                      |
| `[ STR1 != STR2 ]` | `STR1`和`STR2`不等                                      |

**算数二元运算符：**

| 基元             | 含义                                                |
| :-----------------: | :----------------------------------------------------- |
| `[ ARG1 -eq ARG2 ]` | `ARG1`和`ARG2`相等（**eq**ual）                         |
| `[ ARG1 -ne ARG2 ]` | `ARG1`和`ARG2`不等（**n**ot **e**qual）                 |
| `[ ARG1 -lt ARG2 ]` | `ARG1`小于`ARG2`（**l**ess **t**han）                   |
| `[ ARG1 -le ARG2 ]` | `ARG1`小于等于`ARG2`（**l**ess than or **e**qual）      |
| `[ ARG1 -gt ARG2 ]` | `ARG1`大于`ARG2`（**g**reater **t**han）                |
| `[ ARG1 -ge ARG2 ]` | `ARG1`大于等于`ARG2`（**g**reater than or **e**qual）   |

条件语句可以跟 **组合表达式** 配合使用：

| Operation      | Effect                                                      |
| :------------: | :---------------------------------------------------------- |
| `[ ! EXPR ]`   | 如果`EXPR`为假，为真                                         |
| `[ (EXPR) ]`   | 返回`EXPR`的值                                               |
| `[ EXPR1 -a EXPR2 ]` | 逻辑 _与_， 如果`EXPR1`和（**a**nd）`EXPR2`都为真，为真  |
| `[ EXPR1 -o EXPR2 ]` | 逻辑 _或_， 如果`EXPR1`或（**o**r）`EXPR2`为真，为真     |

## `if` statement

`if`在使用上跟其它语言相同。如果中括号里的表达式为真，那么`then`和`fi`之间的代码会被执行。`fi`标志着条件代码块的结束。

```bash
单分支：
    if CONDITION; then
        if-true
    fi

双分支：
    if CONDITION; then
        if-true
    else
        if-false
    fi

多分支：
    if CONDITION1; then
        if-true
    elif CONDITION2; then
        if-ture
    elif CONDITION3; then
        if-ture
    ...
    esle
        all-false
    fi
```

```bash
#判断某路径下所有文件的类型
#!/bin/bash
#

for file in $(ls /var); do
    if [ -f /var/$file ]; then
    echo "Common file."
    elif [ -L /var/$file ]; then
    echo "Symbolic file."
    elif [ -d /var/$file ]; then
    echo "Directory."
    else
    echo "Other type."
    fi
done
```

## `case` 
```bash
case 变量引用 in
PAT1)
    分支1
    ;;
PAT2)
    分支2
    ;;
...
*)
    默认分支
    ;;
esac
```

```bash
#!/bin/bash
#
# chkconfig: - 88 12
# description: test service script
#
prog=$(basename $0)
lockfile=/var/lock/subsys/$prog

start() {
    if [ -e $lockfile ]; then
    echo "$prog is aleady running."
    return 0
    else
    touch $lockfile
    [ $? -eq 0 ] && echo "Starting $prog finished."
    fi
}

stop() {
    if [ -e $lockfile ]; then
    rm -f $lockfile && echo "Stop $prog ok."
    else
    echo "$prog is stopped yet."
    fi
}

status() {
    if [ -e $lockfile ]; then
    echo "$prog is running."
    else
    echo "$prog is stopped."
    fi
}

usage() {
    echo "Usage: $prog {start|stop|restart|status}"
}

if [ $# -lt 1 ]; then
    usage
    exit 1
fi

case $1 in
start)
    start
    ;;
stop)
    stop
    ;;
restart)
    stop
    start
    ;;
status)
    status
    ;;
*)
    usage
esac
```
# 循环

## `for` loop
```bash
for NAME in LIST; do
    循环体
done
```

```bash
#打印九九乘法表；(分别使用for和while循环实现)
#!/bin/bash
#

for j in {1..9}; do
    for i in $(seq 1 $j); do
        echo -e -n "${i}X${j}=$[$i*$j]\t"
    done
    echo
done
```
```bash
#!/bin/bash
#
declare -i i=1
declare -i j=1

while [ $j -le 9 ]; do
    while [ $i -le $j ]; do
        echo -e -n "${i}X${j}=$[$i*$j]\t"
        let i++
    done

    echo
    let i=1
    let j++
done
```
## `while` loop
```bash
while CONDITION; do
    循环体
done
```
 案例
 ```bash
#利用RANDOM生成10个随机数字，输出这个10数字，并显示其中的最大者和最小者；
#!/bin/bash
#
declare -i max=0
declare -i min=0
declare -i i=1


while [ $i -le 9 ]; do
    rand=$RANDOM
    echo $rand

    if [ $i -eq 1 ]; then
        max=$rand
        min=$rand
    fi

    if [ $rand -gt $max ]; then
        max=$rand
    fi
    if [ $rand -lt $min ]; then
        min=$rand
    fi
    let i++
done

echo "MAX: $max."
echo "MIN: $min."
```

## `until` 
```bash
until CONDITION; do
    循环体
done
```


## 循环控制

我们会遇到想提前结束一个循环或跳过某次循环执行的情况。这些可以使用shell内建的`break`和`continue`语句来实现。它们可以在任何循环中使用。

`break`语句用来提前结束当前循环。我们之前已经见过它了。

`continue`语句用来跳过某次迭代。我们可以这么来用它：

```bash
continue [N]：提前结束第N层的本轮循环，而直接进入下一轮判断；
    while CONDTIITON1; do
        CMD1
        ...
        if CONDITION2; then
            continue
        fi
        CMDn
        ...
    done

break [N]：提前结束循环；
    while CONDTIITON1; do
        CMD1
        ...
        if CONDITION2; then
            break
        fi
        CMDn
        ...
    done
```

```bash
for (( i = 0; i < 10; i++ )); do
  if [[ $(( i % 2 )) -eq 0 ]]; then continue; fi
  echo $i
done
```

运行上面的例子，会打印出所有0到9之间的奇数。
# 数组

**${ARRAY_NAME[INDEX]}**

声明数组：
  -  declare -a ARRAY_NAME
  -  declare -A ARRAY_NAME: 关联数组

数组元素的赋值:(元素之间空格分开)

    1. 一次只赋值一个元素；
        ARRAY_NAME[INDEX]=VALUE
            weekdays[0]="Sunday"
            weekdays[4]="Thursday"
    2. 一次赋值全部元素：
        ARRAY_NAME=("VAL1" "VAL2" "VAL3" ...)
    3. 只赋值特定元素：
        ARRAY_NAME=([0]="VAL1" [3]="VAL2" ...)
    4. read -a ARRAY
   
引用数组元素：${ARRAY_NAME[INDEX]}<br>
        注意：省略[INDEX]表示引用下标为0的元素；

_数组的添加删除_
```bash
#add element to the begining of an array use
arr=("new_element" "${arr[@]}")
arr=("new_element1" "new_element2" "..." "new_elementN" "${arr[@]}")
# add an element to the end of an array use.
arr=( "${arr[@]}" "new_element" )
or 
arr+=( "new_element" )

arr=( "${arr[@]}" "new_element1" "new_element2" "..." "new_elementN") #Or
arr+=( "new_element1" "new_element2" "..." "new_elementN" )

# add an element to the specific index of an array use
# unset array_name[index]
arr=(23 56 99 )
unset arr[1]
echo ${arr[@]}
unset arr
echo ${arr[*]} #没有结果因为整个组都被删除了

```

数组的长度(数组中元素的个数)：${#ARRAY_NAME[*]}, ${#ARRAY_NAME[@]}
```bash
[root@corp-centos-101 ~]# color=(red green blue)
[root@corp-centos-101 ~]# echo ${color[0]}
red
[root@corp-centos-101 ~]# echo ${color[1]}
green
[root@corp-centos-101 ~]# echo ${color[2]}
blue
[root@corp-centos-101 ~]# color2=(red green [6]=blue)
[root@corp-centos-101 ~]# echo ${color2[0]}
red
[root@corp-centos-101 ~]# echo ${color2[1]}
green
[root@corp-centos-101 ~]# echo ${color2[2]}

[root@corp-centos-101 ~]# echo ${color2[6]}
blue
[root@corp-centos-101 ~]# echo ${color2[*]}
red green blue
[root@corp-centos-101 ~]# echo ${#color2[*]}
3
```

```bash
#生成10个随机数保存于数组中，并找出其最大值和最小值；
#!/bin/bash
#
declare -a rand
declare -i max=0

for i in {0..9}; do
    rand[$i]=$RANDOM
    echo ${rand[$i]}
    [ ${rand[$i]} -gt $max ] && max=${rand[$i]}
done

echo "Max: $max"
```
# 函数

函数的作用
* 代码重用
* 模块化编程
* 结构化编程
```bash
语法一：
    function f_name {
        ...函数体...
    }

语法二：
    f_name() {
        ...函数体...
    }
```
函数返回值：
    函数的执行结果返回值：
        (1) 使用echo或print命令进行输出；
        (2) 函数体中调用命令的执行结果；
    函数的退出状态码：
        (1) 默认取决于函数体中执行的最后一条命令的退出状态码；
        (2) 自定义退出状态码：
            return
先声明变量            
函数可以接收参数并返回结果 —— 返回值。参数，在函数内部，跟[非交互式](#非交互模式)下的脚本参数处理方式相同 —— 使用[位置参数](#位置参数)。返回值可以使用`return`命令 _返回_ 。

下面这个函数接收一个名字参数，返回`0`，表示成功执行。
```bash
# 带参数的函数
greeting () {
  if [[ -n $1 ]]; then
    echo "Hello, $1!"
  else
    echo "Hello, unknown!"
  fi
  return 0
}

greeting Denys  # Hello, Denys!
greeting        # Hello, unknown!
```

# shell 扩展
## bash 下的 split 取“数组”的首、尾：
```bash
file=/dir1/dir2/dir3/my.file.txt
```
```bash
${file#*/}：拿掉第一条 / 及其左边的字符串：dir1/dir2/dir3/my.file.txt
${file##*/}：拿掉最后一条 / 及其左边的字符串：my.file.txt
${file#*.}：拿掉第一个 .  及其左边的字符串：file.txt
${file##*.}：拿掉最后一个 .  及其左边的字符串：txt
${file%/*}：拿掉最后条 / 及其右边的字符串：/dir1/dir2/dir3
${file%%/*}：拿掉第一条 / 及其右边的字符串：(空值)
${file%.*}：拿掉最后一个 .  及其右边的字符串：/dir1/dir2/dir3/my.file
${file%%.*}：拿掉第一个 .  及其右边的字符串：/dir1/dir2/dir3/my
```

Tips：  

记忆的方法为：  
\# 是去掉左边(在键盘上 # 在 $ 之左边)  
% 是去掉右边(在键盘上 % 在 $ 之右边)  
单一符号是最小匹配﹔两个符号是最大匹配（类似贪婪匹配）。  


## bash 下的 replace 与 replaceAll
```bash
${file/dir/path}：将第一个 dir 提换为 path：/path1/dir2/dir3/my.file.txt
${file//dir/path}：将全部 dir 提换为 path：/path1/path2/path3/my.file.tx
```
## bash 下的变量空值检测与初始化
利用 ${ } 还可针对不同的变量状态赋值(没设定、空值、非空值)：

```bash
${file-my.file.txt} ：假如 $file 没有设定，则使用 my.file.txt 作传回值。(空值及非空值时不作处理)
${file:-my.file.txt} ：假如 $file 没有设定或为空值，则使用 my.file.txt 作传回值。 (非空值时不作处理)
${file+my.file.txt} ：假如 $file 设为空值或非空值，均使用 my.file.txt 作传回值。(没设定时不作处理)
${file:+my.file.txt} ：若 $file 为非空值，则使用 my.file.txt 作传回值。 (没设定及空值时不作处理)
${file=my.file.txt} ：若 $file 没设定，则使用 my.file.txt 作传回值，同时将 $file 赋值为 my.file.txt 。 (空值及非空值时不作处理)
${file:=my.file.txt} ：若 $file 没设定或为空值，则使用 my.file.txt 作传回值，同时将 $file 赋值为 my.file.txt 。 (非空值时不作处理)
${file?my.file.txt} ：若 $file 没设定，则将 my.file.txt 输出至 STDERR。 (空值及非空值时不作处理)
${file:?my.file.txt} ：若 $file 没设定或为空值，则将 my.file.txt 输出至 STDERR。 (非空值时不作处理)
```
Tips:
以上的理解在于, 你一定要分清楚 unset 与 null 及 non-null 这三种赋值状态.  
一般而言, : 与 null 有关, 若不带 : 的话, null 不受影响, 若带 : 则连 null 也受影响。  
而 - 和 = 的区别在于是否把传回值赋给引用变量，例如：
```bash
${parameter:-word}     word is only substituted.
${parameter:=word}     word is substituted and assigned to parameter.
root@localhost ~ $ echo "$var"


root@localhost ~ $ echo "${var:-hello}"
hello
root@localhost ~ $ echo "$var"


root@localhost ~ $ echo "${var:=hello}"
hello
root@localhost ~ $ echo "$var"
```
##  bash 下的大小写变换
```bash
HI=HellO

echo "$HI" # HellO
echo ${HI^} # HellO
echo ${HI^^} # HELLO
echo ${HI,} # hellO
echo ${HI,,} # hello
echo ${HI~} # hellO
echo ${HI~~} #hELLo
^大写，,小写， ~大小写切换
重复一次只修改首字母，重复两次则应用于所有字母。

混着用会怎样？
echo ${HI^,^} # HellO
看来是不行的×_×
```
# Reference

https://mywiki.wooledge.org/BashGuide