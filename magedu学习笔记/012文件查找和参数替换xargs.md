012文件查找和参数替换xargs

# 文件查找  
在文件系统上查找符合条件的文件

* 非实时查找(数据库查找)：locate
* 实时查找：find  



## locate  

* locate 查询系统上预建的文件索引数据库 /var/lib/mlocate/mlocate.db  
* 索引的构建是在系统较为空闲时自动进行(周期性任务)，执行`updatedb`可以更新数据库  
* 索引构建过程需要遍历整个根文件系统，很消耗资源  
* `locate`和`updatedb`命令来自于mlocate包  


**工作特点:**  

* 查找速度快
* 模糊查找
* 非实时查找
* 搜索的是文件的全路径，不仅仅是文件名
* 可能只搜索用户具备读取和执行权限的目录  


格式:  

```
locate [OPTION]... [PATTERN]...
```  

选项:  

```
-i 			#不区分大小写的搜索
-n N 			#只列举前N个匹配项目
-r 			#使用基本正则表达式
```  


## find  

find 是实时查找工具，通过遍历指定路径完成文件查找. 

工作特点:  
* 查找速度略慢
* 精确查找
* 实时查找
* 查找条件丰富
* 可能只搜索用户具备读取和执行权限的目录  

格式:  

```
find [OPTION]... [查找路径] [查找条件] [处理动作]
```  

1. 查找路径：指定具体目标路径；默认为当前目录  
2. 查找条件：指定的查找标准，可以文件名、大小、类型、权限等标准进行；默认为找出指定路径下的所有文件  
3. 处理动作：对符合条件的文件做操作，默认输出至屏幕  

**注意,find使用通配符时候,一定要用双引号""引起来,如果使用单引号''会有异常**

### 指定搜索目录层级  

```
-maxdepth level 	#最大搜索目录深度,指定目录下的文件为第1级
-mindepth level 	#最小搜索目录深度
```  


### 对每个目录先处理目录内的文件，再处理目录本身  
按照默认的遍历顺序,是先显示目标本身,再显示目录下的文件.  
这个选项是改变了遍历顺序.  


```
-depth  
-d #warning: the -d option is deprecated; please use -depth instead, because the latter is a POSIX-compliant feature
```

### 根据文件名和inode查找  

**注意,不同分区的情况,有可能会有相同inode号的不同文件**

```
-name "文件名称" 		#支持使用glob，如：*, ?, [], [^],通配符要加双引号引起来
-iname "文件名称" 		#不区分字母大小写
-inum n 			#按inode号查找
-samefile name 			#相同inode号的文件
-links n 			#链接数为n的文件
-regex “PATTERN” 		#以PATTERN匹配整个文件路径，而非文件名称
```  

### 根据属主、属组查找  

```
-user USERNAME 		#查找属主为指定用户(UID)的文件
-group GRPNAME 		#查找属组为指定组(GID)的文件
-uid UserID 		#查找属主为指定的UID号的文件
-gid GroupID 		#查找属组为指定的GID号的文件
-nouser 		#查找没有属主的文件
-nogroup 		#查找没有属组的文件
```

### 根据文件类型查找  

```
-type TYPE

TYPE可以是以下形式：
f: 普通文件
d: 目录文件
l: 符号链接文件
s：套接字文件
b: 块设备文件
c: 字符设备文件
p: 管道文件
```

### 空文件或目录  

```
-empty
```

### 组合条件  

```
与：-a ，默认多个条件是与关系
或：-o
非：-not !
```  

德·摩根定律:  
* (非 A) 或 (非 B) = 非(A 且 B)  
* (非 A) 且 (非 B) = 非(A 或 B)  

范例:  

```
[root@zdx ~]# find ! \( -type d -a -empty \)		#注意转义  
[root@zdx ~]# find  /home -not \( -user zhaidx -o -user test \)

```

### 排除目录  

```
-path '要排除的目录或文件' -a -prune
```

范例:  

```
#查找/etc/下，除/etc/sane.d目录的其它所有.conf后缀的文件
find /etc -path '/etc/sane.d' -a -prune -o -name "*.conf"
#查找/etc/下，除/etc/sane.d和/etc/fonts两个目录的所有.conf后缀的文件
find /etc \( -path "/etc/sane.d" -o -path "/etc/fonts" \) -a -prune -o -name "*.conf"
#排除/proc和/sys目录
find / \( -path "/sys" -o -path "/proc" \) -a -prune -o -type f -a -mmin -1
```

注意,  
写法很严格,需要排除的目录后面不要加杠,否则失效  
-prune前面的-a好像可以不用写.

```
[root@zdx ~]# find /home/ -path /home/lucy -a -prune -o -name "*.bashrc" 
/home/zhaidx/.bashrc
/home/test/.bashrc
/home/lucy
[root@zdx ~]# find /home/ -path /home/lucy/ -a -prune -o -name "*.bashrc" 
find: warning: -path /home/lucy/ will not match anything because it ends with /.
/home/zhaidx/.bashrc
/home/test/.bashrc
/home/lucy/.bashrc
[root@zdx ~]# find /home/ -path /home/lucy -prune -o -name "*.bashrc" 
/home/zhaidx/.bashrc
/home/test/.bashrc
/home/lucy
[root@zdx ~]# 
```  

### 根据文件大小来查找  

```
-size [+|-]#UNIT 		#常用单位：k, M, G，c（byte）,注意大小写敏感  

#UNIT 				#表示(#-1, #],如：6k 表示(5k,6k]
-#UNIT 				#表示[0,#-1],如：-6k 表示[0,5k]
+#UNIT 				#表示(#,∞),如：+6k 表示(6k,∞)
```  

### 根据时间戳  

```
#以“天”为单位
-atime [+|-]#

# 		#表示[#,#+1)
+# 		#表示[#+1,∞]
-# 		#表示[0,#)

-mtime
-ctime


#以“分钟”为单位
-amin
-mmin
-cmin
```

![](https://raw.githubusercontent.com/ZhaiD0ngxue/pics/main/img/12-1.png)


### 根据权限查找  

```
-perm [/|-]MODE

MODE 		#精确权限匹配
/MODE 		#任何一类(u,g,o)对象的权限中只要能一位匹配即可，或关系，+ 从CentOS 7开始淘汰
-MODE 		#每一类对象都必须同时拥有指定权限，与关系

0 表示不关注
```  

说明：  
find -perm 755 会匹配权限模式恰好是755的文件  
只要当任意人有写权限时，find -perm /222就会匹配  
只有当每个人都有写权限时，find -perm -222才会匹配  
只有当其它人（other）有写权限时，find -perm -002才会匹配  
不关心所有组和其他人,只关心所有者,如果有读或者有写的权限时,find -perm /600才会匹配


###  正则表达式  

```
-regextype type
Changes the regular expression syntax understood by -regex and -iregex tests
which occur later on the command line. Currently-implemented types are
emacs (this is the default), posix-awk, posix-basic, posix-egrep and posix-
extended.
-regex pattern
File name matches regular expression pattern. This is a match on the whole
path, not a search. For example, to match a file named `./fubar3', you can
use the regular expression `.*bar.' or `.*b.*3', but not `f.*r3'. The regular
expressions understood by find are by default Emacs Regular Expressions, but
this can be changed with the -regextype option.
```  

**注意,使用正则时,描述的是应该整个路径而不仅仅是文件名,否则会找不到**

范例:  

```
[root@zdx ~]# find /etc -regextype posix-extended -regex ".*\.conf"
```  

### 处理动作  

```
-print		#默认的处理动作，显示至屏幕
-ls		#类似于对查找到的文件执行"ls -dils"命令格式输出
-fls file	#查找到的所有文件的长格式信息保存至指定文件中，相当于 -ls > file
-delete		#删除查找到的文件，慎用！
-ok COMMAND {} \; 		#对查找到的每个文件执行由COMMAND指定的命令，对于每个文件执行命令之前，都会交互式要求用户确认
-exec COMMAND {} \; 		#对查找到的每个文件执行由COMMAND指定的命令
{}				#用于引用查找到的文件名称自身
```  

范例:  

```
#备份配置文件，添加.orig这个扩展名
find -name ".conf" -exec cp {} {}.orig \;
#提示删除存在时间超过３天以上的joe的临时文件
find /tmp -ctime +3 -user joe -ok rm {} \;
#在主目录中寻找可被其它用户写入的文件
find ~ -perm -002 -exec chmod o-w {} \;
#查找/data下的权限为644，后缀为sh的普通文件，增加执行权限
find /data –type f -perm 644 -name "*.sh" –exec chmod 755 {} \;
```  


# 参数替换 xargs  

由于很多命令不支持管道|来传递参数，xargs用于产生某个命令的参数，xargs 可以读入 stdin 的数据，并且以空格符或回车符将 stdin 的数据分隔成为参数  
另外，许多命令不能接受过多参数，命令执行可能会失败，xargs 可以解决  
**注意：文件名或者是其他意义的名词内含有空格符的情况**  


find 经常和 xargs 命令进行组合,形式如下：  

```
find | xargs COMMAND
```
 范例:  
 
 ```
 [root@zdx ~]# seq 10
1
2
3
4
5
6
7
8
9
10
[root@zdx ~]# seq 10 |xargs		#xarg会把换行改成空格,在一行进行显示
1 2 3 4 5 6 7 8 9 10
[root@zdx ~]# 

[root@zdx ~]# seq 10 |xargs -n 2	#指定每一次传递几个参数
1 2
3 4
5 6
7 8
9 10
[root@zdx ~]# 

#文件名或者是其他意义的名词内含有空格符的情况,需要指定分隔符来解决.
find -type f -name "*.txt” -print0 | xargs -0 ls  

#-i是指可以使用{}来替代;-P指可以开启多进程并指定数进程数,第二个-P是wget的option,指定保存目录
seq 100 |xargs -i -P10 wget -P /data http://10.0.0.8/{}.html  


#从dir文件中获取字符串,传递给xargs,并通过-I来初始定义一个变量名%,后向启动一个子shell,可以多次调用这个%变量.
root@zdx:~# cat dirs | xargs -I % sh -c 'echo % ; mkdir %'
dir1
dir2
dir3
dir4
root@zdx:~# ls
dir1  dir2  dir3  dir4  dirs
root@zdx:~# 

#通过-p参数实现交互
root@zdx:~# echo haha | xargs -p touch
touch haha ?...n
root@zdx:~# ls
dir1  dir2  dir3  dir4  dirs
root@zdx:~# echo haha | xargs -p touch
touch haha ?...y
root@zdx:~# ls
dir1  dir2  dir3  dir4  dirs  haha
root@zdx:~# 

其他:
-d #		#指定分隔符
-L n		#每次读取的行数
 ```