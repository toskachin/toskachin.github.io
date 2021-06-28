 [shell变量的高级应用](#shell变量的高级应用)
  - [变量内容替换和删除](#变量内容替换和删除)
  - [字符串处理](#字符串处理)
    - [计算字符串长度](#计算字符串长度)
    - [获取子串在字符串中索引的位置](#获取子串在字符串中索引的位置)
    - [提取字符串](#提取字符串)
  - [数组的操作](#数组的操作)
    - [删除元素](#删除元素)
    - [分片访问](#分片访问)
    - [内容替换](#内容替换)
  - [数组遍历](#数组遍历)
    - [PS](#ps)


   

# shell变量的高级应用
## 变量内容替换和删除

|语法	|说明|
| :------------: | :--------------------- | 
|${变量名#匹配规则}}|	从变量开头进行规则匹配，将符合最短的数据删除|
|${变量名##匹配规则}}|	从变量开头进行规则匹配，将符合最长的数据删除|
|${变量名%匹配规则}}|	从变量尾部进行规则匹配，将符合最短的数据删除|
|${变量名%%匹配规则}}	|从变量尾部进行规则匹配，将符合最长的数据删除|
|${变量名/旧字符串/新字符串}}|	变量内容符合旧字符串，则第一个旧字符串会被新字符串取代|
|${变量名//旧字符串/新字符串}}|	变量内容符合旧字符串，则全部的旧字符串会被新字符串取代|

```bash
[shdevops@localhost ~]$ variable_1="I love you,do you love me?"
[shdevops@localhost ~]$ echo $variable_1
I love you,do you love me?
[shdevops@localhost ~]$ echo ${variable_1#*ov} //从头开始将最短数据删除
e you,do you love me?
[shdevops@localhost ~]$ echo ${variable_1##*ov} //从头开始将最长数据删除
e me?
[shdevops@localhost ~]$ echo ${variable_1%ov*} //从尾开始将最短数据删除
I love you,do you l
[shdevops@localhost ~]$ echo ${variable_1%%ov} //从尾开始将最长数据删除
I l
[shdevops@localhost ~]$ echo $PATH
/sbin:/bin:/usr/sbin:/usr/bin
[shdevops@localhost ~]$ echo ${PATH/bin/BIN} //替换一个
/sBIN:/bin:/usr/sbin:/usr/bin
[shdevops@localhost ~]$ echo ${PATH//bin/BIN} //替换所有
/sBIN:/BIN:/usr/sBIN:/usr/BIN
```

## 字符串处理
### 计算字符串长度

|      | 语法 | 说明 |
| :-------- | :------- | :---- |
| 方法一    | `${#string}`      | NA |
| 方法二   | `expr length "$string"`        |string如有空格，必须加双引号  |


```bash
[shdevops@localhost ~]$ echo ${#var1}
21
[shdevops@localhost ~]$ echo `expr length "$var1"`
21
```

### 获取子串在字符串中索引的位置

| 语法 | 说明|
| :--- | :--- |
| `expr index $string $substring` | 查找子串中任一出现的字符，找到第一个出现字符的位置就返回 |

```bash
[shdevops@localhost ~]$ variable_1="I love you,do you love me?"

[shdevops@localhost ~]$ echo `expr index "$variable_1" oe`
4
```
### 提取字符串

| | 语法 | 说明  |
|:---|:---|:---|
|方法一| `${string:position}` | 从 string 中的 position 位置开始，索引下标从 0 开始 |
|方法二 |`${string:position:length}`  | 从 position 开始，匹配长度为 length|
|方法三|`${string: -position}`|从右边开始匹配，注意冒号后面的空格|
|方法四|`${string:(position}`|从左边开始匹配|
|方法五|`expr substr $string $position $length`|从 position 开始，匹配长度为 length，索引下标从 1 开始|

```bash
[shdevops@localhost ~]$ var_str="k8s docker cloud aws"
[shdevops@localhost ~]$ echo ${var_str: 3} //方法一
docker cloud aws
[shdevops@localhost ~]$ echo ${var_str:3:5} //方法二
dock
[shdevops@localhost ~]$ echo ${var_str: -5}  //方法三，注意有空格
d aws
[shdevops@localhost ~]$ echo ${var_str: -5:3} //方法三 ，带长度
d a
[shdevops@localhost ~]$ echo ${var_str:-5} //方法三，没有空格无效
k8s docker cloud aws
[shdevops@localhost ~]$ echo ${var_str:(-5)} //方法三，可以用括号代替空格
d aws
[shdevops@localhost ~]$ echo `expr substr "$var_str" 1 5` //方法五，从索引1开始计数
k8s d

```

## 数组的操作
> 我们知道在强类型语言中定义变量的时候必须为这个变量定义类型，比如定义整形可以定义为 int 型，字符串 string 类型，日期类型 Date 等等，在使用之前必须定义类型，这是强类型编程语言的风格。Shell 编程中，由于 Shell 是弱类型语言，在使用变量前不需要为变量声明类型， 但其实 Shell 本身也是支持提前声明有类型变量的，只是说使用方式和一般的强类型编程语言有些不同， 使用 declare 和 typeset 命令来进行声明。
> * declare 命令和 typeset 命令两者等价
> * declare、typeset 命令都是用来定义变量类型的
> 
> 既然它们是等价的 那我们用 declare 进行演示

declare 常用命令参数表


| 参数 | 含义  |
| :------------: | :--------------------- | 
| -i |	将变量设为整数 |
|-a|	将变量定义为数组|
|-f|	显示此脚本定义过的所有函数及内容|
|-F|	仅显示此脚本定义过的函数名|
|-x|	将变量声明为环境变量|

`declare -a array_name` 声明数组， 然后 `array_name=(value1 ... valuen)`， 这里不声明也可以

### 删除元素
* `unset my_arr[2]` 清楚元素 `unset my_arr` 清空整个数组

```bash
[shdevops@localhost ~]$
[shdevops@localhost ~]$ my_arr=(1 "abc")
[shdevops@localhost ~]$ echo ${my_arr[*]}
1 abc
[shdevops@localhost ~]$ unset my_arr[1]
[shdevops@localhost ~]$ echo ${my_arr[*]}
1
[shdevops@localhost ~]$ unset my_arr
[shdevops@localhost ~]$ echo ${my_arr[*]}
```

###  分片访问
*  ${array[@]:1:3} 数组下标从 1 到 3 的三个元素
*  
```bash
[shdevops@localhost ~]$ my_arr=("apple" "juice" "cat" "dog" "ox")
[shdevops@localhost ~]$ echo ${my_arr[@]:1:3}
juice cat dog
```
### 内容替换
*  `${my_array[@]/an/AN}` 将数组中所有元素内包含 an 的子串替换为 AN
```bash
[shdevops@localhost ~]$ my_arr=("an1" "an2" "an3" "an4")
[shdevops@localhost ~]$ echo ${my_arr[@]/an/AN}
AN1 AN2 AN3 AN4
[shdevops@localhost ~]$
```
## 数组遍历

```bash
my_arr=(an1 an2 an3 an4)
for v in ${my_arr[@]}
do
echo $v
done

[shdevops@localhost ~]$ for v in ${my_arr[@]};do echo $v;done
an1
an2
an3
an4
[shdevops@localhost ~]$
```
### PS
```
取消声明的变量：

declare +r
declare +i
declare +a
declare +x
```