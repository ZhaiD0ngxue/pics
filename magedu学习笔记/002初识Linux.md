002初识Linux

# 初识Linux

## 用户类型

- root用户

> 1.  UID为0
> 2.  一个特殊管理账户,被称为超级用户,其权限接近完整的系统控制,对系统几乎有无限的能力,所以除非必要,不要使用root用户进行登录.

- 普通用户

> 1.  UID不为0
> 2.  权限有限,造成的影响和管控能力有限

## 终端

1.  终端设备通常指的是KVM(keyboard,video monitor,mouse),即输入输出的设备.
2.  我们日常工作中也会使用终端模拟软件,如xshell,secureCRT,putty等.
3.  终端类型包括控制台终端(/dev/console),串行终端(/dev/ttyS#),虚拟终端(tty,/dev/tty#),图形终端(startx,xwindows),伪终端(/dev/pts#)等等.
4.  查看当前用户所在终端

```
[root@zdx ~]# tty
/dev/pts/1
[root@zdx ~]# 
```

## 交互式接口

1.  交互式接口类型:
    
    - GUI,图形化交互接口
    - CLI,命令行交互接口
2.  SHELL
    
    - 简单理解,SHELL就是Linux的解释器,它本身就是一个程序,可以将用户输入的命令"翻译"为计算机可以执行的命令.
    - SHELL也是一种高级程序设计语言.
    - 有多种SHELL.  
        ![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/2-1.png)
    - 目前Linux系统主要使用的SHELL是bash.
    - 查看对当前使用的shell类型
    

\[root@zdx ~\]# echo $SHELL /bin/bash \[root@zdx ~\]#

```
    * 显示当前系统支持的SHELL版本 

    ```
[root@zdx ~]# cat /etc/shells 
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
[root@zdx ~]# 
```

## 简单命令

**设置主机名**

```
#######临时生效
[root@zdx ~]# hostname NAME

#######持久生效
[root@zdx ~]# hostnamectl set-hostname NAME

```

**注意:**

- 主机名不支持使用下划线.
- 有些软件对主机名有特殊要求.

## 命令提示符

- 一般会在提示符中体现当前用户,当前目录等信息.
- "#"代表管理员,"$"代表普通用户.
- 命令提示符格式可通过如下命令查询和修改,提示符相关说明可自行学习

```
#######查询
[root@zdx ~]# echo $PS1
[\u@\h \W]\$
#######临时修改
[root@zdx ~]# PS1="[\t \u@\h \W]\$"		#此处发生了错误,因为双引号的原因,最后的\$的\被转义了
[11:41:09 root@zdx ~]$
#######持久配置
[11:43:13 root@zdx ~]$echo 'PS1="[\t \u@\h \W]\$"'>> /etc/bashrc
```

## 执行命令

### 执行过程

> 输入命令并回车执行,由shell程序找到键入的命令所对应的可执行程序或代码,并由其分析后提交给内核分配资源将其运行起来.

### 命令的类型

1.  内部命令
2.  外部命令

通过如下命令进行区分

```
type COMMAND  
type -a COMMAND	#打印所有	
```

### 内部命令

help 内部命令列表  
enable 管理内部命令

- enable cmd 启用内部命令
- enable -n cmd 禁用内部命令
- enable 查看所有可用的内部命令
- enable -n 查看所有禁用的内部命令

### 外部命令

**查看外部命令路径**

```
[root@zdx ~]# which ls
alias ls='ls --color=auto'
        /usr/bin/ls
[root@zdx ~]# which cat
/usr/bin/cat
[root@zdx ~]# whereis cat
cat: /usr/bin/cat /usr/share/man/man1/cat.1.gz /usr/share/man/man1p/cat.1p.gz
[root@zdx ~]# 
```

**hash**  
用户登录时,hash表为空.当外部命令被执行时,系统默认会从PATH路径下寻找命令,并将该命令的路径记录到hash表中,当再次使用该命令时,shell有限从hash表中寻找以提高调用效率. ***此处需要注意,no such file or directory的问题***

```
hash 			#显示hash缓存
hash -d cmd 	#删除某cmd的hash记录
hash -r 		#情况hash记录
```

### 命令别名

目的是对于经常使用的较长命令,可以将其定义一个简短的别名以方便用户使用.

```
alias #显示当前的所有别名  
alias NAME='CMD' # 给CMD命令定义一个NAME别名  
unalias -a #取消所有别名  
unalias NAME #取消某NAME的别名  
```

如果需要持久保持,需要将定义写入到配置文件`/etc/bashrc`中.  
写入到配置文件的定义不会立即生效,需要使用`.`命令或`source`命令来使其生效.  
如果别名同原命令同名,如果需要调用执行原命令,可使用如下方法

```
\ALIASNAME
"ALIASNAME"
'ALIASNAME'
commond ALIASNAME
/PATH/commond #只是用于外部命令
```

### 命令格式

```
COMMAND [OPTIONS...] [ARGUMENTS...]
COMMAND [COMMAND] [COMMAND] ...

```

选项,用于启用或关闭命令的某个或某些功能.

> - 短选项:UNIX风格
> - 长选项: GNU风格
> - BSD风格选项

范例

```
[root@zdx home]# id -u zhaidx
1000
[root@zdx home]# ls -a
.  ..  zhaidx
[root@zdx home]# ls --all
.  ..  zhaidx
[root@zdx home]# 
```

**注意**

- 多个选项以及多个参数和命令之间使用空格分隔.
- 取消和结束命令执行:ctrl+C(强制结束),ctrl+d(正常结束)
- 多个命令可以使用";"隔开,写在同一行进行执行
- 一个命令可以用"\\"来代表仍未结束,下一行继续书写

### 命令执行顺序

1.  别名
2.  内部命令
3.  已被hash的外部命令
4.  外部命令
5.  command not found.

## 常见命令

### 硬件信息

```
#######查看CPU信息
[root@zdx home]# lscpu  
[root@zdx home]# cat /proc/cpuinfo 
#######查看内存信息  
[root@zdx home]# free  
[root@zdx home]# cat /proc/meminfo   
#######查看硬盘信息  
[root@zdx home]# lsblk  
[root@zdx home]# cat /proc/partitions 

```

### 系统版本信息

```
#######查看内核版本
[root@zdx home]# uname -r
#######查看操作系统发行版本
[root@zdx home]# cat /etc/redhat-release   
[root@zdx home]# cat /etc/os-release  
[root@zdx home]# lsb_release -a
```

### 日期和时间

Linux有2个时钟:

- 系统时钟,由Linux内核通过CPU工作频率进行的
- 硬件时钟,由主板BIOS时钟获得

```
#######显示系统时间  
[root@zdx home]# date
Thu Dec  3 14:02:38 CST 2020  

[root@zdx home]# date +%s
1606975405  

[root@zdx home]# date "+%F %T"
2020-12-03 14:03:53  

[root@zdx home]# date -d @1606975405
Thu Dec  3 06:03:25 GMT 2020

[root@zdx home]# date -u
Thu Dec  3 09:05:48 UTC 2020
``````
#######显示硬件时钟  
[root@zdx home]# clock
2020-12-03 14:04:31.741444+08:00  
[root@zdx home]# clock -s #以硬件时钟为准,校正系统时钟  
[root@zdx home]# clock -w #以系统时钟为准,校正硬件时钟 
######clock和hwclock是一回事
[root@zdx home]# ll /usr/sbin/clock
lrwxrwxrwx. 1 root root 7 Apr 24  2020 /usr/sbin/clock -> hwclock
``````
####### 列出所有时区  
[root@zdx home]# timedatectl list-timezones
####### 设置指定时区  
[root@zdx home]# timedatectl set-timezone Africa/Bamako  
####### 查看当前时区  
[root@zdx home]# ll /etc/localtime  
root@zdx home # cat /etc/timezone  

[root@zdx home]# timedatectl status
               Local time: Thu 2020-12-03 06:14:31 GMT
           Universal time: Thu 2020-12-03 06:14:31 UTC
                 RTC time: Thu 2020-12-03 06:14:31
                Time zone: Africa/Bamako (GMT, +0000)
System clock synchronized: no
              NTP service: active
          RTC in local TZ: no
``````
####### 显示当月日历  
[root@zdx home]# cal 
####### 显示当年日历  
[root@zdx home]# cal -y  
######## 显示指定年的日历  
[root@zdx home]# cal 2021    
####### 显示指定年月的日历  
[root@zdx home]# cal 9 1752
   September 1752   
Su Mo Tu We Th Fr Sa
       1  2 14 15 16
17 18 19 20 21 22 23
24 25 26 27 28 29 30
                    
                    
                    
[root@zdx home]# 
```

### 关机和重启

1.  关机
    
    - halt
    - poweroff
    - init 0
2.  重启
    
    - reboot  
        a. -f:强制,不调用shutdown b. -p:切断电源
    - init 6
3.  关机或重启
    
    - shutdown  
        a. -r:reboot  
        b. -h:halt  
        c. -c:cancle  
        d. TIME,无指定,默认相当于+1 * now,立刻 * +#,相对时间法,表示几分钟后执行  
        \* hh:mm,决定时间法

### 用户登录信息查看命令

- whoami,显示当前登录的有效用户
- who,系统当前所有登录的会话
- w,系统当前所有登录的会话和操作

### 文本编辑

- nano,简单
- gedit,图形化文本工具
- vi/vim,最常用

### 会话管理

- screen
    - 场景一:长时间运行的情况
    - 场景二:多人共享窗口的情况,远程协助
- tmux
    - 单个窗口中可以再做划分小窗口

### 输出信息

echo命令用于将后面所跟的字符进行屏幕输出.

```
echo [-neE] [字符串]
```

选项:

- -E (默认)不支持\\解释功能
- -n 不自动换行
- -e 启用\\解释功能
    - \\a,发出警告声
    - \\b,退格
    - \\c,最后不加上换行符号
    - \\e,escape,相当于\\033
    - \\n,换行且光标移至行首
    - \\r,回车,光标移至行首但不换行
    - \\t,制表符
    - \\,插入\\字符
    - \\0nnn,插入八进制nnn所代表ASCII字符
    - \\xHH,插入十六进制HH所代表的ASCII字符

### 字符集和编码

**字符集和编码是两个容易混淆的概念,他们是不同层面的  
charset,字符集,指的是二进制和字符的对应关系,不关注最终的存储形式  
encoding,字符集编码,实现如何将字符转化为实际的二进制进行存储或翻译,编码决定了空间的使用大小**

*个人理解  
字符集就是一个字符对应一个二进制,会有不同的字符集,如UTF-8,ASCII等等  
编码就是一串字符按照某一字符集进行转化为二进制的行为,当然解码是相反的操作行为*

常用的字符集:

1.  ASCII字符集,常用
2.  UTF字符集
    - UTF-8,常用,变长存储,最省空间
    - UTF-16
    - UTF-32

```
#######查看当前系统语言和字符集
[root@zdx home]# echo $LANG
en_US.UTF-8
[root@zdx home]# LANG=zh_CN.UTF-8
[root@zdx home]# echo $LANG
zh_CN.UTF-8 
######## 字符集转换
[root@zdx home]# unix2dos file  
[root@zdx home]# dos2unix file
```

### 命令符号

#### ``和$()

```
#######可以用来执行命令
$(COMMOND) 或`COMMOND`  
[root@zdx home]# echo `echo $LANG`
en_US.UTF-8
[root@zdx home]# echo $(echo $LANG)
en_US.UTF-8
[root@zdx home]# 
```

#### ''

```
#######强引用
[root@zdx home]# echo 'echo $LANG'
echo $LANG
[root@zdx home]# 
```

#### ""

```
#######弱引用  
[root@zdx home]# echo "echo $LANG"
echo en_US.UTF-8
[root@zdx home]#   

```

#### $和${}

```
#######获取变量的值  
[root@zdx home]# echo $LANG
en_US.UTF-8
[root@zdx home]# echo ${LANG}
en_US.UTF-8
[root@zdx home]# 
```

#### {}

```
[root@zdx home]# echo {0..5}.log
0.log 1.log 2.log 3.log 4.log 5.log
[root@zdx home]# echo {a..f..2}.log
a.log c.log e.log
[root@zdx home]# 
```

### TAB键使用

1.  命令补全功能
2.  路径补全功能

### 历史命令功能

> 用户执行命令后,系统会在内存中记录下执行过的命令  
> 当用户正常退出后,会将内存中的历史信息存放到文件中,默认是`~/.bash_history`中.  
> 当用户再次登录shell后,系统会从文件中读取历史命令再加载到内存中.  
> **注意,登录进shell后新执行的命令只会记录在内存的缓存中,只有用户正常退出时,才会追加到文件中**  
> 利用历史命令,可以用它来重复执行命令,提高输入效率

命令:history

```
####### 格式
history [-c] [-d offset] [n]  
history -anrw [filename]  
history -ps arg [arg..] 
####### 参数
-c:清空历史命令  
-d offset:删除历史中指定的第offset个命令  
n:显示最近的n条历史  
-a:追加本次会话新执行的命令到历史文件中  
-r:读取文件到历史缓存 
-w:保存历史缓存到文件中  
-n:读取历史文件中未读过的行到历史列表  
-p:展开历史参数成多行,但不存在历史列表中  
-s:展开历史参数成一行,附加在历史列表后  

```

命令历史相关环境变量

环境变量可以`export 变量名=值`的方式生效  
或存放在`/etc/profile`或`~/.bash_profile`文件中持久保持

```
HISTSIZE:命令历史记录的HISTFILE:指定历史文件,默认为~/. bash_history
HISTFILESIZE:命令历史文件记录历史的条数
HISTTIMEFORMAT="%F %T whoami " 显示时间和用户
HISTIGNORE="str1:str2*:..." 忽略str1命令,Str2开头的历史
HISTCONTROL:控制命令历史的记录方式

ignoredups是默认值,可忽略重复的命令,连续且相同为“重复”
ignorespace忽略所有以空白开头的命令
ignoreboth相当于 ignoredups, ignorespace的组合
erasedups删除重复命令
```

调用历史命令

```
!!	#再次执行上一条命令  
!:0	#执行上一条的命令,去除参数  
!:n	#执行序号为n的命令  
!-n	#执行倒数第n条命令  

ctrl+r	#搜索历史命令  
ctrl+g	#从历史搜索模式退出  

!$	#代表上一条命令的最后一个参数  
!^	#代表上一条命令的第一个参数  
!*	#代表上一条命令的所有参数  
!:n	#代表上一条命令的第n个参数(第一个参数序号是0)  
!n:m	#代表第n条命令的第m个参数(第一个参数序号是0)
ESC,.	#输入上一条命令的最后一个参数  

```

### bash快捷键

```
ctrl+c		#强制退出,取消
ctrl+z		#挂起当前进程

ctrl+s		#锁定屏幕
ctrl+q		#取消屏幕锁定
ctrl+l		#清屏

ctrl+a		#光标到行开头  
ctrl+e		#光标移动到行末尾
ctrl+f		#光标右移一个字符  
alt+f		#光标右移到单词词尾  
ctrl+b		#光标左移一个字符  
alt+b		#光标向左移动到单词词首

ctrl+d		#删除当前光标所在的一个字符
ctrl+u		#从光标处删除到行首
ctrl+k		#从光标出删除到行尾  
ctrl+w		#从光标出向左删除至单词词首  
alt+r		#删除整行  
ctrl+y		#将删除的字符粘贴到光标处

alt+u		#将光标所在字符至词尾的字符变为大写
alt+l		#将光标所在字符至词尾的字符变为小写

```

注意

![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/2-2.png)

## 帮助

### whatis

```
####### 可以通过whatis命令获取命令的简短描述信息
[root@zdx home]# whatis cal
cal (1)              - display a calendar
cal (1p)             - print a calendar
[root@zdx home]# 
#######需要相关数据库支持  
mandb	#centos7及以后
makewhatis	#centos6
```

### help

#### 内部命令的帮助

```
help COMMOND
man COMMOND
```

#### 外部命令的帮助

```
COMMOND --help或COMMOND -h  
man COMMOND  
info COMMOND  
自身的帮助文档,README,INSTALL,CHANGELOG  
官方文档  
```

### man

1.  不同类型的帮助称为不同的章节,统称为Linux手册.可通过`whatis`命令查询到.
    1.  :用户命令
    2.  :系统调用
    3.  :C库调用
    4.  :设备文件及特殊文件
    5.  :配置文件格式
    6.  :游戏
    7.  :杂项
    8.  :管理类的命令
    9.  : Linux内核AP
2.  查看man手册

```
man [option] [SECTION] page...  
man [SECTION] keyword
```

3.  说明

```
NAME名称及简要说明
SYNOPSIS用法格式说明
[]可选内容
<>必选内容
a|b二选一
(}分组
...同一内容可出现多次
DESCRIPTON详细说明
OPTIONS选项说明
EXAMPLES示例
FLES相关文件
AUTHOR作者
COPYRIGHT版本信息
REPORTING BUGS bug信息
SEE ALSO其它帮助参考
``````
man -a keyword		#列出keyword的所有帮助  
man -k keywork		#搜索man手册,列出所有匹配的页面,使用whatis数据库  
man -f keyword		#获取简短的说明信息,相当于whatis  
man -w [section] keyword		#man帮助文件的路径  

```