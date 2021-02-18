011SHELL脚本编程

# 编程基础 

## 程序的组成  

* 程序=算法+数据结构  
* 数据:是程序的核心  
* 数据结构:数据在计算机中的类型和组织方式  
* 算法:处理数据的方式  

## 编程的风格  

* 面向过程语言 
	* 做做一件事,排个步骤,步骤一,步骤二,步骤三...  
	* 问题规模小,可以步骤化,按部就班处理  
	* 以指令为中心,数据服务于指令  
	* C,shell都是面向过程的语言 
* 面向对象语言
	* 一种认识世界,分析世界的方法论,将万事万物都抽象为对象.  
	* 类是抽象的概念,是一类事物的共同特征的集合.  
	* 对象是类的具象,是一个实体.  
	* 问题规模大,复杂系统  
	* 以数据为中心,指令服务于数据  
	* java,c#,python,golang都是面向对象语言

## 编程语言  

1. 计算机:运行二进制指令  
1. 编程语言:人与计算机之间交互的语言,分为两种:低级语言和高级语言.  

	* 低级语言:  
	机器:二进制0和1的序列,称为机器指令.难懂难写.  
	汇编:用一些单词符号代替了机器指令.
	* 高级语言:  
	编译:高级语言->编译器->机器代码文件->执行
	解释:高级语言->执行->解释器->机器代码.
	
1. 编译和解释型语言


![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-1.png)

## 编程逻辑处理方式 
三种处理逻辑  

* 顺序执行:程序从上到下按顺序执行  
* 选择执行:程序执行过程中,根据条件的不同,进行选择不同的分支继续执行  
* 循环执行:程序执行过程中需要重复执行多次某段语句

![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-2.png)
  
![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-3.png)




# SHELL脚本的基本用法

## 用途  

* 自动化执行,提高工作效率  
* 减少人工输入,避免人为错误  
* 标准化安装和配置  
* 用于日常重复性工作


## 基本结构  

SHELL脚本编程是基于过程的解释型语言.  
编程语言的基本结构:  

* 各种系统命令  
* 数据存储:变量,数组
* 表达式
* 控制语句  

shell脚本就是包含一些命令和声明,并符合一定格式的文本文件.  
格式要求:shebang机制.

```
#!/bin/bash
#!/usr/bin/python
#!/usr/bin/perl
```

## 创建过程  

1. 使用文本编辑器创建文本文件  
2. 文本第一行shebang机制
3. 编写代码
4. 为文件添加可执行权限
5. 运行脚本,由bash解释器解释执行  


## 多种执行方法

```
bash /data/hello.sh
bash < /data/hello.sh
/data/hello.sh
cd /data ; ./hello.sh
hello.sh	#需要先在环境变量中声明
cat /data/hello.sh | bash
```

## shell调试  

```
bash -n hello.sh	#只检测脚本中的语法错误,无法检查出命令错误,不会真正执行  
bash -x hello.sh	#调试并执行,显示具体细节
cat -A hello.sh		#检查是否由不可见的异常字符
```
总结:

* 语法错误,会导致后续的命令不继续执行,可以用bash -n检查错误,提示的行不一定准确.  
* 命令错误,默认后续的命令还会继续执行,bash -n无法检查处理,可以使用bash -x观察.  
* 逻辑错误,只能使用bash -x进行观察.

## 变量  

### 变量类型  

变量类型

* 内置变量/环境变量  
* 自定义变量

变量数据类型

* 字符
* 数值

静态语言和动态语言

* 态编译语言:使用变量前,先声明变量类型,之后类型不能改变,在编译时检査,如:java,C
* 动态编译语言:不用事先声明,可随时改变类型,如:bash, Python

强类型语言和弱类型语言

* 强类型语言:不同类型数据操作,必须经过强制转换成同一类型才能计算.例如:print('string'+str(1))
	
* 弱类型语言:语言的运行时会隐式做数据类型转换,无需指定类型,默认都是字符型,变量无需事先定义可直接调用.


### 变量命名  

1. 不能使用保留字和内置变量名.
2. 只能使用字母,数字和下划线组成,但不能以数字开头.注意不支持短横线-
3. 见名知义,避免用简写
4. 统一命名规则.


### 变量定义和引用
按变量的生效范围可以划分变量类型  

* 普通变量,生效范围为当前shell进程;对于当前shell之外的其他shell进程都无效(即便是当前shell的子shell)
* 环境变量生效范围为当前shell进程及其子进程
* 本地变量,生效范围为当前shell进程中某代码片段,通常指函数

```
####### 变量定义
name='value'	#value可以是字符串,可以是其他变量,也可以是个命令
####### 变量引用
$name
${name}
```
```
[root@zdx ~]# name='name'
[root@zdx ~]# echo ${name}
name
[root@zdx ~]# pass=${name}	#变量的间接赋值
[root@zdx ~]# echo ${pass}
name
[root@zdx ~]# name='name2'
[root@zdx ~]# echo ${name}
name2
[root@zdx ~]# echo ${pass}
name
[root@zdx ~]#

[root@zdx ~]# var=`seq 10`	#变量被赋值命令
[root@zdx ~]# echo ${var}
1 2 3 4 5 6 7 8 9 10
[root@zdx ~]#

[root@zdx ~]# var="haha"	#变量值追加
[root@zdx ~]# var+=hehe
[root@zdx ~]# echo ${var}
hahahehe
[root@zdx ~]#

[root@zdx ~]# cmd=hostname	#利用变量实现动态命令
[root@zdx ~]# ${cmd}
zdx.org
[root@zdx ~]#

```

```
set 	#命令可以查看系统中的变量
unset name		#可以删除变量
```

### 环境变量  

* 可以使子进程(包括孙子进程)集成父进程的变量(但是无法让父进程使用子进程的变量)  
* 一旦子进程修改了从父进程继承来的变量值,新的变量值会传递给孙子进程  
* 一般只在系统配置文件中使用,脚本中较少使用.  

```
[root@zdx ~]# pstree -p		#显示进程树
systemd(1)─┬─ModemManager(908)─┬─{ModemManager}(927)
           │                   └─{ModemManager}(930)
           ├─NetworkManager(993)─┬─{NetworkManager}(1000)
           │                     └─{NetworkManager}(1002)
           ├─VGAuthService(880)
           ├─agetty(1283)
           ├─alsactl(938)
           ├─atd(1156)
           ├─auditd(846)─┬─sedispatch(848)
           │             ├─{auditd}(847)
           │             └─{auditd}(849)
           ├─automount(1261)─┬─{automount}(1267)
           │                 ├─{automount}(1268)
           │                 ├─{automount}(1294)
           │                 └─{automount}(1298)
           ├─avahi-daemon(906)───avahi-daemon(919)
           ├─chronyd(912)
           ├─crond(1179)
           ├─cupsd(1003)
           ├─dbus-daemon(892)───{dbus-daemon}(916)
           ├─dnsmasq(1375)───dnsmasq(1376)
           ├─firewalld(978)───{firewalld}(1112)
           ├─gssproxy(887)─┬─{gssproxy}(893)
           │               ├─{gssproxy}(894)
           │               ├─{gssproxy}(895)
           │               ├─{gssproxy}(896)
           │               └─{gssproxy}(897)
           ├─ksmtuned(922)───sleep(923)
           ├─libvirtd(1137)─┬─{libvirtd}(1175)
           │                ├─{libvirtd}(1176)
           │                ├─{libvirtd}(1177)
           │                ├─{libvirtd}(1178)
           │                ├─{libvirtd}(1180)
           │                ├─{libvirtd}(1181)
           │                ├─{libvirtd}(1182)
           │                ├─{libvirtd}(1183)
           │                ├─{libvirtd}(1184)
           │                ├─{libvirtd}(1185)
           │                ├─{libvirtd}(1286)
           │                ├─{libvirtd}(1289)
           │                ├─{libvirtd}(1290)
           │                ├─{libvirtd}(1291)
           │                ├─{libvirtd}(1292)
           │                └─{libvirtd}(1379)
           ├─lsmd(888)
           ├─mcelog(885)
           ├─polkitd(876)─┬─{polkitd}(924)
           │              ├─{polkitd}(932)
           │              ├─{polkitd}(934)
           │              ├─{polkitd}(935)
           │              └─{polkitd}(976)
           ├─rhsmcertd(1016)
           ├─rngd(913)───{rngd}(925)
           ├─rpc.statd(1219)
           ├─rpcbind(842)
           ├─rsyslogd(1146)─┬─{rsyslogd}(1158)
           │                └─{rsyslogd}(1161)
           ├─smartd(902)
           ├─sshd(1004)───sshd(1199)───sshd(1405)───bash(1406)───pstree(1484)
           ├─sssd(891)─┬─sssd_be(931)
           │           └─sssd_nss(954)
           ├─systemd(1391)─┬─(sd-pam)(1395)
           │               ├─dbus-daemon(1417)───{dbus-daemon}(1433)
           │               └─pulseaudio(1404)─┬─{pulseaudio}(1434)
           │                                  └─{pulseaudio}(1435)
           ├─systemd-journal(702)
           ├─systemd-logind(985)
           ├─systemd-machine(877)
           ├─systemd-resolve(1131)
           ├─systemd-udevd(736)
           ├─tuned(1005)─┬─{tuned}(1297)
           │             ├─{tuned}(1302)
           │             └─{tuned}(1305)
           └─vmtoolsd(881)─┬─{vmtoolsd}(933)
                           └─{vmtoolsd}(943)
[root@zdx ~]# echo $BASHPID		#显示当前bash的PID
1406
[root@zdx ~]#
```

```
####### 变量的声明和赋值
export name=VALUE 	
declare -x name=VALUE	
####### 或者分两步实现
name=VALUE
export name

####### 显示所有的环境变量
env
printenv
export
declare -x

####### 删除变量
unset name

####### bash内建的环境变量
PATH
SHELL
USER
UID
HOME
PWD
SHLVL	#shell的嵌套层数,即深度
LANG
MAIL
HOSTNAME
HISTSIZE
_	#下划线,表示前一个命令的最后一个参数
```  

### 只读变量  
只读变量只能声明定义,后续不能修改和删除,即常量.  

```
####### 声明
readonly name
declare -r name

####### 查看只读常量  
readonly [-p]  
declare -r
```

范例:

```root@zdx:~# readonly PI=3.1415
root@zdx:~# echo $PI
3.1415
root@zdx:~# PI=3.14
-bash: PI: readonly variable
root@zdx:~# unset PI
-bash: unset: PI: cannot unset: readonly variable
root@zdx:~#
```
logout之后再登录,该只读变量会被清除


### 位置变量  
位置变量：在bash shell中内置的变量, 在脚本代码中调用通过命令行传递给脚本的参数  

```
$1, $2, ... 	对应第1个、第2个等参数，shift [n]换位置,超过10个参数一定要加花括号防止歧义
$0 		命令本身,包括路径
$* 		传递给脚本的所有参数，全部参数合为一个字符串
$@ 		传递给脚本的所有参数，每个参数为独立字符串
$# 		传递给脚本的参数的个数

注意：$@ $* 只在被双引号包起来的时候才会有差异,$*会把所有的参数看成是一个参数整体,而$@会把所有参数还是看作一个个的个体.
```

清空所有位置变量

```
set --
```

范例

```

root@zdx:/data# cat arg.sh
#!/bin/bash
echo "1st arg is: " $1
echo "2st arg is: " $2
echo "3st arg is: " $3
echo "4st arg is: " $4
echo "10st arg is: " ${10}
echo "the number of arg is $#"
echo "the number of arg is $*"		#看不出区别
echo "the number of arg is $@"		#看不出区别
echo "the script name is $0"
set --		#清空了位置变量
echo "1st arg is: " $1
root@zdx:/data# bash arg.sh {a..z}
1st arg is:  a
2st arg is:  b
3st arg is:  c
4st arg is:  d
10st arg is:  j
the number of arg is 26
the number of arg is a b c d e f g h i j k l m n o p q r s t u v w x y z
the number of arg is a b c d e f g h i j k l m n o p q r s t u v w x y z
the script name is arg.sh		#注意此处显示的结果,和调用时的方法有点关系,建议可以搭配basename命令使用.
1st arg is:
root@zdx:/data#
```

```
root@zdx:/data# cat arg2.sh
#!/bin/bash
echo 'first arg is :' $1
echo 'second arg is :' $2
root@zdx:/data# bash arg2.sh a b c d
first arg is : a
second arg is : b		#看样子并没有异常,把b c d 都赋值给第二个位置参数,但是却丢失了2个参数,需要注意
root@zdx:/data#
```

**特殊用法,可以通过软连接的方式实现多态.**

```
[root@centos8 ~]#cat test.sh
#!/bin/bash
echo $0
[root@centos8 ~]#ln -s test.sh a.sh
[root@centos8 ~]#ln -s test.sh b.sh
[root@centos8 ~]#./a.sh
./a.sh
[root@centos8 ~]#./b.sh
./b.sh
```

### 退出状态代码  

进程执行后，将使用变量\$?保存状态码的相关数字，不同的值反应成功或失败,\$?取值范例 0-255  
注意,值是上一条指令执行结果的状态.,仅仅是最近的上一条指令.

```
$?的值为0 		#代表成功
$?的值是1到255		 #代表失败
```  

**范例:**

```
[root@zdx ~]# ping -c 1 -W 1 www.baidu.com &>/dev/null 
[root@zdx ~]# echo $?
0
[root@zdx ~]#
```

用户也可以在脚本中以下命令自定义退出状态码  

```
exit [n]
```  

**注意：**
* 脚本中一旦遇到exit命令，脚本会立即终止；终止退出状态取决于exit命令后面的数字
* 如果exit后面无数字,终止退出状态取决于exit命令前面命令执行结果
* 如果没有exit命令, 即未给脚本指定退出状态码，整个脚本的退出状态码取决于脚本中执行的最后一条命令的状态码  


### 展开命令行/执行优先级顺序  

**顺序如下:**

```
把命令行分成单个命令词
展开别名
展开大括号的声明{}
展开波浪符声明 ~
命令替换$() 和 ``
再次把命令行分成命令词
展开文件通配*、?、[abc]等等
准备I/0重导向 <、>
运行命令
```  

<font color="red" size=5>注意:脚本中使用别名是不能被正确解析的</font>  
***上面写的展开别名,其实是指在命令行的环境下,开发shell脚本是需要特别注意***


**防止扩展,可使用反斜线\来进行转义随后的字符.**

```
[root@zdx ~]# echo you cost: \$5.00
you cost: $5.00
[root@zdx ~]# 
```

**加引号来防止扩展**  

```
单引号（''）防止所有扩展
双引号（""）也可防止扩展，但是以下情况例外：$（美元符号）
```

**变量扩展**

```
`` 		:反引号，命令替换
\		:反斜线，禁止单个字符扩展
!		:叹号，历史命令替换
```

### 脚本安全和 set  

set 命令：可以用来定制 shell 环境

**$- 变量**

```
[root@zdx ~]# echo $-
himBHs
[root@zdx ~]# 
``` 

**含义:**

```
h：hashall，打开选项后，Shell 会将命令所在的路径hash下来，避免每次都要查询。通过set +h将h选项关闭
i：interactive-comments，包含这个选项说明当前的 shell 是一个交互式的 shell。所谓的交互式shell,在脚本中，i选项是关闭的
m：monitor，打开监控模式，就可以通过Job control来控制进程的停止、继续，后台或者前台执行等  
B：braceexpand，大括号扩展  
H：history，H选项打开，可以展开历史列表中的命令，可以通过!感叹号来完成，例如“!!”返回上最近的一个历史命令，“!n”返回第 n 个历史命令
```

范例:

```
[root@zdx ~]# echo $-
himBHs
[root@zdx ~]# set +h	#注意+号代表关闭
[root@zdx ~]# echo $-
imBHs
[root@zdx ~]# hash
-bash: hash: hashing disabled
[root@zdx ~]# set -h	#注意-号代表开启
[root@zdx ~]# hash
hits    command
   1    /usr/sbin/ping
[root@zdx ~]# 
```  

**set 命令实现脚本安全**  

```
-u	#在扩展一个没有设置的变量时，显示错误信息， 等同set -o nounset
-e 	#如果一个命令返回一个非0退出状态值(失败)就退出， 等同set -o errexit
-o option 	#显示，打开或者关闭选项  
			显示选项：set -o  
			打开选项：set -o 选项  
			关闭选项：set +o 选项  
-x 	#当执行命令时，打印命令及其参数,类似 bash -x
```

## 格式化输出printf  

格式  

```
printf "指定的格式" "文本1" "文本2"...
```  

![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-4.png)  

**常用格式替换符合**    

|替换符|功能|  
|:--|:--|  
|%s|字符串|
|%f|浮点格式|
|%b|相对应的参数中包含转义字符时，可以使用此替换符进行替换，对应的转义字符会被转义|
|%c|ASCII字符，即显示对应参数的第一个字符|
|%d,%i|十进制整数|
|%o|八进制值|
|%u|不带正负号的十进制值|
|%x|十六进制值（a-f）|
|%X|十六进制值（A-F）|
|%%|表示%本身|  

**说明：**  
%#s 中的#数字代表此替换符中的输出字符宽度，不足补空格，默认是右对齐，%-10s表示10个字符宽，- 表示左对齐  

**常用转义字符**

|转义符|功能|
|:--|:--|
|\a|警告字符，通常为ASCII的BEL字符|
|\b|后退|
|\f|换页|
|\n|换行|
|\r|回车|
|\t|水平制表符|
|\v|垂直制表符|
| \\ |表示\本身,左侧显示有问题,是2个斜杠,第一个斜杠转义了第二个斜杠,你懂的|  

范例:

```
root@zdx:~# printf "%s\n" 1 2 3 4
1
2
3
4
root@zdx:~# printf "%x\n" 111 112 113 114	#还可以转换成十六进制,输出时做转换
6f
70
71
72
root@zdx:~# printf "%f\n" 1 2 3 4		
1.000000
2.000000
3.000000
4.000000
root@zdx:~# printf "%.2f\n" 1 2 3 4		#小数点后2位
1.00
2.00
3.00
4.00
root@zdx:~# printf "(%s)" 1 2 3 4;echo		#最后换下行
(1)(2)(3)(4)
root@zdx:~# printf "(%s)" 1 2 3 4
(1)(2)(3)(4)root@zdx:~# printf "%s %s %s\n" 1 2 3 4			#注意不匹配的情况
1 2 3
4  
root@zdx:~# 
```  

```
root@zdx:~# VAR="welcome to study linux";printf "\033[1;32m%s\033[0m\n" $VAR	#还可以使用颜色
welcome
to
study
linux
root@zdx:~# VAR="welcome to study linux";printf "\033[1;32m%s\033[0m\n" "$VAR"		#注意,把整个参数作为一个整体了,区别于上面
welcome to study linux
root@zdx:~# 
```  

## 算数运算  

**注意:bash 只支持整数，不支持小数(如果一定需要,可以使用bc)**

```
* / % 		multiplication, division, remainder, %表示取模，即取余数，示例：9%4=1，5%3=2
+ - 		addition, subtraction
i++ i-- 	variable post-increment and post-decrement,先赋值给引用的变量,之后自增,注意区分
++i --i 	variable pre-increment and pre-decrement,先自增,然后赋值给引用的变量,注意区分
= *= /= %= += -= <<= >>= &= ^= |= 	assignment
- + 		unary minus and plus
! ~ 		logical and bitwise negation
** e		xponentiation 乘方,即指数运算
<< >> 		left and right bitwise shifts
<= >= < > 	comparison
== != 		equality and inequality
& 			bitwise AND
| 			bitwise OR
^ 			bitwise exclusive OR
&& 			logical AND
|| 			logical OR
expr?expr:expr 		conditional operator
expr1 , expr2 		comma
```

**注意,乘法符号在特定常见需要转义,expr**

```
root@zdx:~# expr 2 * 3
6
root@zdx:~# touch a.txt
root@zdx:~# expr 2 * 3		#此处报错,因为*会被认为是通配符,所以需要转义这个*号
expr: syntax error: unexpected argument ‘a.txt’
root@zdx:~# 
```  

**实现算术运算：**  

```
(1) let var=算术表达式	#格式:let varOPERvalue,支持i++
(2) ((var=算术表达式)) 		#和上面等价
(3) var=$[算术表达式]
(4) var=$((算术表达式))
(5) var=$(expr arg1 arg2 arg3 ...)
(6) declare -i var = 数值		#此时定义的var变量,已经声明是数字类型,后向可以直接进行计算,不会再被系统认为是字符串了
(7) echo '算术表达式' | bc	#也可以指定其他参数,比如ibase,obase,scale等等
```  

内建的随机数生成器变量：  

```
$RANDOM 取值范围：0-32767
```  

## 逻辑运算  

true,false  

```
1,真
0,假
#注意,以上为二进制
```  

与：&：和0相与，结果为0，和1相与，结果保留原值  

```
0 与 0 = 0
0 与 1 = 0
1 与 0 = 0
1 与 1 = 1
```  

或：|：和1相或结果为1，和0相或，结果保留原值  

```
0 或 0 = 0
0 或 1 = 1
1 或 0 = 1
1 或 1 = 1
```
 
 非：！  
 
 ```
! 1 = 0 		! true
! 0 = 1 		! false
 ```  
 
 范例:
 
 ```
 [root@centos8 ~]#true
[root@centos8 ~]#echo $?	#注意此处不要混淆,$?返回的是上一步的状态码,十进制的.
0
[root@centos8 ~]#false
[root@centos8 ~]#echo $?
1
[root@centos8 ~]#! true
[root@centos8 ~]#echo $?
1
[root@centos8 ~]#! false
[root@centos8 ~]#echo $?
0
 ```
 
异或：^  
异或的两个值，相同为假，不同为真。  
特性:两个数字X,Y异或得到结果Z，Z再和任意两者之一X异或，将得出另一个值Y

```
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0
```

特性的应用,变量更换  

```
[root@centos8 ~]#x=10;y=20;temp=$x;x=$y;y=$temp;echo x=$x,y=$y	#容易理解
x=20,y=10
[root@centos8 ~]#x=10;y=20;x=$[x^y];y=$[x^y];x=$[x^y];echo x=$x,y=$y	#有个局限,只能用于数字
x=20,y=10
```  

**短路运算**  

短路与&&  

```
CMD1 短路与 CMD2
第一个CMD1结果为真 (1)，第二个CMD2必须要参与运算，才能得到最终的结果
第一个CMD1结果为假 (0)，总的结果必定为0，因此不需要执行CMD2
```
![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-5.png)


短路或|| 

```
CMD1 短路或 CMD2
第一个CMD1结果为真 (1)，总的结果必定为1，因此不需要执行CMD2
第一个CMD1结果为假 (0)，第二个CMD2 必须要参与运算,才能得到最终的结果
```  
![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-6.png)


应用:  

```
CONDITION && COMMAND1 || COMMAND2		#如果条件判断为真则执行命令1,如果条件判断为假则执行命令2,很有价值,注意顺序不能写反.
```  

## 条件测试命令  
条件测试：判断某需求是否满足，需要由测试机制来实现，专用的测试表达式需要由测试命令辅助完成测试过程，实现评估布尔声明，以便用在条件性环境下进行执行  

若真，则状态码变量 $? 返回0  
若假，则状态码变量 $? 返回1  

**条件测试命令**  

* test EXPRESSION  
* [ EXPRESSION ] #和test 等价，建议使用 [ ]  
* [[ EXPRESSION ]] 相对是于增强版的 [ ] ,支持通配符和正则表达式 

注意：EXPRESSION前后必须有空白字符  

**帮助**  

```
[root@zdx ~]# type [
[ is a shell builtin
[root@zdx ~]# help [
[: [ arg... ]
    Evaluate conditional expression.
    
    This is a synonym for the "test" builtin, but the last argument must
    be a literal `]', to match the opening `['.
[root@zdx ~]# help test
test: test [expr]
    Evaluate conditional expression.
    
    Exits with a status of 0 (true) or 1 (false) depending on
    the evaluation of EXPR.  Expressions may be unary or binary.  Unary
    expressions are often used to examine the status of a file.  There
    are string operators and numeric comparison operators as well.
    
    The behavior of test depends on the number of arguments.  Read the
    bash manual page for the complete specification.
    
    File operators:
    
      -a FILE        True if file exists.		#判断文件是否已存在,不建议使用,取反时的结果有BUG
      -b FILE        True if file is block special.		
      -c FILE        True if file is character special.
      -d FILE        True if file is a directory.
      -e FILE        True if file exists.		#判读文件是否已存在.
      -f FILE        True if file exists and is a regular file.		#判断是否存在并且是个普通文件
      -g FILE        True if file is set-group-id.		#SGID
      -h FILE        True if file is a symbolic link.
      -L FILE        True if file is a symbolic link.
      -k FILE        True if file has its `sticky' bit set.		#Sticky
      -p FILE        True if file is a named pipe.
      -r FILE        True if file is readable by you.
      -s FILE        True if file exists and is not empty.
      -S FILE        True if file is a socket.
      -t FD          True if FD is opened on a terminal.
      -u FILE        True if the file is set-user-id.		#SUID
      -w FILE        True if the file is writable by you.
      -x FILE        True if the file is executable by you.
      -O FILE        True if the file is effectively owned by you.
      -G FILE        True if the file is effectively owned by your group.
      -N FILE        True if the file has been modified since it was last read.
    
      FILE1 -nt FILE2  True if file1 is newer than file2 (according to
                       modification date).			#判断新旧
    
      FILE1 -ot FILE2  True if file1 is older than file2.		#判断新旧
    
      FILE1 -ef FILE2  True if file1 is a hard link to file2.	#判断硬链接
    
    String operators:		#字符串的比较
    
      -z STRING      True if string is empty.
    
      -n STRING
         STRING      True if string is not empty.
    
      STRING1 = STRING2
                     True if the strings are equal.
      STRING1 != STRING2
                     True if the strings are not equal.
      STRING1 < STRING2
                     True if STRING1 sorts before STRING2 lexicographically.
      STRING1 > STRING2
                     True if STRING1 sorts after STRING2 lexicographically.
    
    Other operators:
    
      -o OPTION      True if the shell option OPTION is enabled.
      -v VAR         True if the shell variable VAR is set.		#判断变量是否被定义
      -R VAR         True if the shell variable VAR is set and is a name
                     reference.
      ! EXPR         True if expr is false.
      EXPR1 -a EXPR2 True if both expr1 AND expr2 are true.		#逻辑与
      EXPR1 -o EXPR2 True if either expr1 OR expr2 is true.		#逻辑或
    
      arg1 OP arg2   Arithmetic tests.  OP is one of -eq, -ne,		#等于,不等于,小于,小于等于,大于,大于等于
                     -lt, -le, -gt, or -ge.
    
    Arithmetic binary operators return true if ARG1 is equal, not-equal,
    less-than, less-than-or-equal, greater-than, or greater-than-or-equal
    than ARG2.
    
    Exit Status:
    Returns success if EXPR evaluates to true; fails if EXPR evaluates to
    false or an invalid argument is given.
[root@zdx ~]# 
```  
算数表达式的比较还可以使用如下的操作符

```
== 相等
!= 不相等
<=
>=
<
>
```

### 变量测试  

```
#判断 NAME 变量是否定义
[ -v NAME ]
#判断 NAME 变量是否定义并且是名称引用,bash 4.4新特性
[ -R NAME ]
``` 

### 字符串测试  

**test和 [ ]用法**  

```
-z STRING 	字符串是否为空，没定义或空为真，不空为假，
-n STRING 	字符串是否不空，不空为真，空为假
STRING 		同上
STRING1 = STRING2 		是否等于，注意 = 前后有空格
STRING1 != STRING2 		是否不等于
> 		ascii码是否大于ascii码
< 		是否小于
```  
注意:
比较字符串时,建议变量放在双引号""中  
如果使用通配符的话,可以不用处理.  
如果只是使用字符的星号 * 或其他符号,可以进行转义.  


**[[]] 用法**  

```
== 		左侧字符串是否和右侧的PATTERN相同.注意:此表达式用于[[ ]]中，PATTERN为通配符
=~ 		左侧字符串是否能够被右侧的正则表达式的PATTERN所匹配.注意: 此表达式用于[[ ]]中；扩展的正则表达式
```  

建议：当使用正则表达式或通配符使用[[ ]]，其它情况一般使用 [ ],当然也可以一致使用[[ ]]避免歧义.  

范例:判断数字是否是100以内  

```
root@zdx:/data# num=101
root@zdx:/data# [[ $num =~ 100|[0-9]{1,2} ]]	#注意写法,此处逻辑错误
root@zdx:/data# echo $?
0
root@zdx:/data# [[ $num =~ ^(100|[0-9]{1,2})$ ]]
root@zdx:/data# echo $?
1
root@zdx:/data# 
```  

范例:判断IP是否合法

```
root@zdx:/data# IP=1.2.3.4
root@zdx:/data# [[ $IP =~ ^(([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$ ]]
root@zdx:/data# echo $?
0
root@zdx:/data# IP=1.2.3.444
root@zdx:/data# [[ $IP =~ ^(([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$ ]]
root@zdx:/data# echo $?
1
```  

### 文件测试  

 存在性测试  
 
 ```
-a FILE		：同 -e,不推荐使用,取反是有异常
-e FILE		: 文件存在性测试，存在为真，否则为假
-b FILE		：是否存在且为块设备文件
-c FILE		：是否存在且为字符设备文件
-d FILE		：是否存在且为目录文件
-f FILE		：是否存在且为普通文件
-h FILE 或 -L FILE	：存在且为符号链接文件
-p FILE		：是否存在且为命名管道文件
-S FILE		：是否存在且为套接字文件
 ```  
 
 文件权限测试  
 **注意：最终结果由用户对文件的实际权限决定，而非文件属性决定**
 
 ```
-r FILE		：是否存在且可读
-w FILE		: 是否存在且可写
-x FILE		: 是否存在且可执行
-u FILE		：是否存在且拥有suid权限
-g FILE		：是否存在且拥有sgid权限
-k FILE		：是否存在且拥有sticky权限
 ```  
 
 文件属性测试 
 
 ```
-s FILE 		#是否存在且非空
-t fd 			#fd 文件描述符是否在某终端已经打开
-N FILE 		#文件自从上一次被读取之后是否被修改过
-O FILE			#当前有效用户是否为文件属主
-G FILE			#当前有效用户是否为文件属组
FILE1 -ef FILE2		#FILE1是否是FILE2的硬链接
FILE1 -nt FILE2 	#FILE1是否新于FILE2（mtime）
FILE1 -ot FILE2		#FILE1是否旧于FILE2
 ```  
 
 ## 关于()和{}  
 
 (CMD1;CMD2;...）和 { CMD1;CMD2;...; } 都可以将多个命令组合在一起，批量执行
 
 区别:  
 
 ```
 [root@centos8 ~]#man bash  
 ( list ) 会开启子shell,并且list中变量赋值及内部命令执行后,将不再影响后续的环境帮助参看:man bash 搜索(list)
{ list; } 不会启子shell, 在当前shell中运行,会影响当前shell环境帮助参看:man bash 搜索{ list; }
 ```
 
 ```
root@zdx:/data# umask;( umask 123;umask;( umask;umask 234;umask ));umask
0022
0123
0123
0234
0022
root@zdx:/data# umask;{ umask 123;umask;{ umask;umask 234;umask; } };umask	#书写时注意空格
0022
0123
0123
0234
0234
root@zdx:/data#  
```  

## 组合测试条件  

### 方式一[]

```
[ EXPRESSION1 -a EXPRESSION2 ] 并且，EXPRESSION1和EXPRESSION2都是真，结果才为真
[ EXPRESSION1 -o EXPRESSION2 ] 或者，EXPRESSION1和EXPRESSION2只要有一个真，结果就为真
[ ! EXPRESSION ] 取反
```  
**说明： -a 和 -o 需要使用测试命令进行，[[ ]] 不支持**

范例:

```
root@zdx:/data# [[ -e arg.sh -a -e arg2.sh ]]
-bash: syntax error in conditional expression
-bash: syntax error near `-a'
root@zdx:/data# [ -e arg.sh -a -e arg2.sh ]
root@zdx:/data# echo $?
0
root@zdx:/data# 


```

### 方式二[[]]  

```
COMMAND1 && COMMAND2 	#并且，短路与，代表条件性的AND THEN
如果COMMAND1 成功,将执行COMMAND2,否则,将不执行COMMAND2

COMMAND1 || COMMAND2 	#或者，短路或，代表条件性的OR ELSE
如果COMMAND1 成功,将不执行COMMAND2,否则,将执行COMMAND2

! COMMAND #非,取反
```

范例:

```
root@zdx:/data# [[ -e arg.sh && -e arg2.sh ]]
root@zdx:/data# echo $?
0
root@zdx:/data# [[ -e arg.sh && -e arg3.sh ]]
root@zdx:/data# echo $?
1
root@zdx:/data# 
```

范例:  
**注意:当&& 和 || 混合使用，&& 要在前，|| 放在后**

```
root@zdx:/data# ping -c1 -W1 172.16.0.1 &> /dev/null && echo '172.16.0.1 isup' || (echo '172.16.0.1 is unreachable'; exit 1)
172.16.0.1 is unreachable
root@zdx:/data# ping -c1 -W1 10.0.0.5 &> /dev/null && echo '172.16.0.1 isup' || (echo '172.16.0.1 is unreachable'; exit 1)
172.16.0.1 isup
root@zdx:/data# 
``` 


## 使用read命令来接收输入实现交互  

使用read来把输入值分配给一个或多个shell变量，read从标准输入中读取值，**给每个单词分配一个变量，所有剩余单词都被分配给最后一个变量.**  
如果变量名没有指定，默认标准输入的值赋值给系统内置变量REPLY  

格式:

```
read [options] [name ...]
```  

常见选项：  

```
-p	 #指定要显示的提示
-s	 #静默输入，一般用于密码
-n N	#指定输入的字符长度N
-d '字符'		#输入结束符
-t N			#TIMEOUT为N秒
```  

```
root@zdx:/data# read
zhaidx
root@zdx:/data# echo $REPLY
zhaidx

root@zdx:/data# read name age
zhaidx 18
root@zdx:/data# echo $name
zhaidx
root@zdx:/data# echo $age
18
root@zdx:/data# 

root@zdx:/data# read -p "plz input your name: "  yourname
plz input your name: zdx
root@zdx:/data# echo $yourname
zdx
root@zdx:/data# 
```
read的特殊写法,<<<

```
[root@zdx ~]# read W I T B <<< "I am the boss"
[root@zdx ~]# echo $W
I
[root@zdx ~]# echo $I
am
[root@zdx ~]# echo $T
the
[root@zdx ~]# echo $B
boss
[root@zdx ~]# 
```


**注意此时管道符的用法:**  
我们的目的是使用echo,通过管道,将参数传递给read.  
但是实际上管道符会把整体指令分为前后两部分执行.  
管道前后的命令都是在子进程中执行,父进程无法获取到子进程中的变量.  
所以书写时需要注意添加括号进行调整.

```
#Pipelines:A pipeline is a sequence of one or more commands separated by one of the control operators | or |&  
[root@centos8 ~]#echo zhaidx | read NAME	#这里是开了2个子进程进行执行,实际是成功了
[root@centos8 ~]#echo $NAME	#此时,上一步已经执行完成,退出到当前的进程,这时就无法再访问刚刚子进程中的变量了

[root@centos8 ~]#echo zhaidx | { read NAME; echo $NAME; }	#解决方法,访问变量和子进程在一起,就可以正确访问
zhaidx
```


# bash shell配置文件  

## 按生效范围划分  

* 全局配置:针对所有用户有效.  

```
/etc/profile
/etc/profile.d/*.sh
/etc/bashrc
```  

* 用户配置:仅针对特定用户生效  

```
~/.bash_profile
~/.bashrc
```  

## 按照登录方式分类  

* 交互式登录  
	1. 直接通过终端输入账号密码登录  
	2. 使用 `su - UserName` 切换的用户  

配置文件生效和执行顺序:  

```
#放在每个文件最前
/etc/profile
/etc/profile.d/*. sh
/etc/bashrc
~/ .bash_ profile
~/ .bashrc
/etc/bashrc

#放在每个文件最后
/etc/profile.d/*.sh
/etc/bashrc
/etc/profile
/etc/bashrc #此文件执行两次
~/.bashrc
~/.bash_profile
```

**注意：文件之间的调用关系，写在同一个文件的不同位置，将影响文件的执行顺序**  
**注意:后生效的配置会将之前生效的配置覆盖,所以建议提前做好规划**

* 非交互式登录  
	1. su UserName  
	1. 图形界面下打开的终端
	1. 执行脚本
	1. 任何其它的bash实例  


配置文件执行顺序:  

```
/etc/profile.d/*.sh
/etc/bashrc
~/.bashrc
```  

## 按功能划分  
### profile类
profile类为交互式登录的shell提供配置  

```
全局：/etc/profile, /etc/profile.d/*.sh
个人：~/.bash_profile
```
功用：
(1) 用于定义环境变量
(2) 运行命令或脚本

### bashrc类  
bashrc类：为非交互式和交互式登录的shell提供配置  

```
全局：/etc/bashrc
个人：~/.bashrc
```  

功用:  
(1) 定义命令别名和函数
(2) 定义本地变量  


## 配置文件生效  
修改profile和bashrc文件后需生效两种方法:  
1. 重新启动shell进程  
2. source或者. 配置文件  

注意:  
source 会在当前shell中执行脚本,所有一般只用于执行置文件,或在脚本中调用另一个脚本的场景
 
 
 ## bash退出时执行任务  
 
保存在~/.bash_logout文件中（用户）,在退出登录shell时运行  
功能：  
* 创建自动备份
* 清除临时文件  




# 流程控制  

## 条件选择  
### 条件判断介绍  
#### 单分支条件  

![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-7.png)

#### 多分支条件  

![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-8.png)

![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-9.png)
  
  
 ### 选择执行 if 语句  
 
 格式:  
 
 ```
 if COMMANDS; then COMMANDS; [ elif COMMANDS; then COMMANDS; ]... [ else COMMANDS; ] fi
 ```  
 
 单分支  
 
 ```
if 判断条件; then
	条件为真的分支代码
fi
 ```  
 
 双分支  
 
 ```
if 判断条件; then
	条件为真的分支代码
else
	条件为假的分支代码
fi
 ```  
 
 多分支  
 
 ```
if 判断条件1; then
	条件1为真的分支代码
elif 判断条件2; then
	条件2为真的分支代码
elif 判断条件3; then
	条件3为真的分支代码
...
else
	以上条件都为假的分支代码
fi
 ```  
 
说明:  
 
* 多个条件时，逐个条件进行判断，第一次遇为“真”条件时，执行其分支，而后结束整个if语句,所以**判断条件的顺序时应该注意从严谨到宽泛.**  
* if 语句可嵌套  

### 条件判断 case 语句  

格式:  

```
case WORD in [PATTERN [| PATTERN]...) COMMANDS ;;]... esac
```  

```
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
````

case支持glob风格的通配符：  

```
* 		#任意长度任意字符
? 		#任意单个字符
[] 		#指定范围内的任意单个字符
| 或者，	#如: a|b
```  

## 循环  
### 循环执行介绍  
将某代码段重复运行多次，通常有进入循环的条件和退出循环的条件

重复运行次数
* 循环次数事先已知
* 循环次数事先未知

常见的循环的命令：for, while, until  

![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-10.png) 

### for循环  

#### 格式一

```
for NAME [in WORDS ... ] ; do COMMANDS; done

#方式1
for 变量名 in 列表;do
	循环体
done

#方式2
for 变量名 in 列表
do
	循环体
done
```  

**执行机制：**
* 依次将列表中的元素赋值给“变量名”; 每次赋值后即执行一次循环体; 直到列表中的元素耗尽，循环结束
* 如果省略 [in WORDS ... ] ，此时使用位置参量  

**for 循环列表生成方式：**
* 直接给出列表
* 整数列表：

```
{start..end}
$(seq [start [step]] end)
```  

* 返回列表的命令:  

```
$(COMMAND)
```

* 使用glob，如：*.sh
* 变量引用，如：$@，$*，$#


#### 格式二  

双小括号方法，即((…))格式，也可以用于算术运算，双小括号方法也可以使bash Shell实现C语言风格的变量操作  

I=10;((I++))  

![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/11-11.png) 

```
for ((: for (( exp1; exp2; exp3 )); do COMMANDS; done

for ((控制变量初始化;条件判断表达式;控制变量的修正表达式))
do
	循环体
done
```  

说明:  
* 控制变量初始化：仅在运行到循环代码段时执行一次
* 控制变量的修正表达式：每轮循环结束会先进行控制变量修正运算，而后再做条件判断  



### while循环