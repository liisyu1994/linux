---
tartitle: Centos
date: 2018-02-16 00:00:01
tags: 
-  Centos
categories:
- Linux

---

![](<https://raw.githubusercontent.com/FameLsy/Images/master/Fullmetal%20Alchemist/wallhaven-335846.png>)

<!-- more -->





# 用户相关

## 添加用户

指令

```mysql
useradd [options] LOGIN
```


可选选项

| 选项 | 描述          |
| ---- | ------------- |
| u    | 指定uid       |
| g    | 指定主组      |
| d    | 指定家目录    |
| c    | 添加秒速信息  |
| s    | 指定shell信息 |
| G    | 指定附加组    |



示例

```
useradd -u 1024 -g root -d /test_user/ -c 'test' -s /bin/bash test_user
```



## 删除用户

指令

```
userdel [options] LOGIN
```



选项

| 选项 | 描述               |
| ---- | ------------------ |
| r    | 删除主目录和邮件池 |



示例

```
userdel -r famel
```



## 修改用户

指令

```
usermod [options] LOGIN
```



选项

| 选项 | 描述 |
| ---- | ---- |
|      |      |

## 修改用户密码

指令

```
  passwd  [-k]  [-l]  [-u  [-f]]  [-d] [-e] [-n mindays] [-x liisyudays] [-w warndays] [-i inactivedays] [-S] [--stdin] [username]
```



选项

| 选项  | 描述                                                         |
| ----- | ------------------------------------------------------------ |
| stdin | 允许接受参数作为密码(centos使用) 一次性修改密码方法<br /> `echo 123456 &#124; passwd --stdin famel` |



示例

```java
//修改root用户密码
sudo passwd
// 修改当前用户密码
passwd 
// 修改指定用户密码
passwd liisyu
```



## 查看用户信息

指令

```
id [OPTION]... [USER]
```



选项

| 选项 | 描述 |
| ---- | ---- |
|      |      |



示例

```java
//查看指定用户信息
id liisyu
```



## 查看所有用户登录情况

指令

```
who [OPTION]... [ FILE | ARG1 ARG2 ]
```



示例

```java
//查看所有用户登录信息
who
```



## 查看当前用户登录情况

指令

```
tty
```



示例

```java
//查看当前用户登录情况
tty
```



## 查看当前用户名

指令

```
whoami
```



示例

```java
//查看当前用户名
whoami
```



## 切换用户

指令

```
su [options...] [-] [user [args...]]
```



示例

```java
//切换用户
su - liisyu
```



## 退出用户

指令

```
exit
```



## 模拟用户的创建过程

第一步：添加新用户信息

```java
vim /etc/passwd

//加入以下信息
liisyu:x:1024:1024:liisyu,,,:/home/liisyu:/bin/bash
```



第二步：为新用户添加家目录

```
mkdir /home/liisyu
```



第三步：为新用户添加密码

```java
vim /etc/shadow

//加入以下信息
liisyu:*:17855:0:99999:7:::
```



第四步：为新用户添加主组

```java
vim /etc/group

//加入以下信息
liisyu:x:1024:
```



第五步：为组添加组密码

```java
vim /etc/gshadow

//加入以下信息
liisyu:!::
```



第六步：为用户创建邮箱

```java
touch /var/spool/mail/liisyu
```



第七步： 为创建的邮箱未见属主和属组改为liisyu

```java
chown -R liisyu.liisyu /var/spool/mail/liisyu 
```



第八步：把相关文件复制到家目录下

```
cp /etc/skel/.[!.]* /home/liisyu
```



第九步：将复制的文件属主和属组改为liisyu'

```
chown -R liisyu.liisyu /home/liisyu
```



第十步：为用户加个密码

```
passwd liisyu
```



# 组相关

## 添加组

指令

```java
groupadd [options] group
```



示例

```java
groupadd anewgroup
```



## 删除组

指令

```
groupdel [options] GROUP
```



示例

```
groupdel anewgroup
```



## 修改组

指令

```
groupmod [options] GROUP
```



可选选项

| 选项 | 描述     |
| ---- | -------- |
| n    | 修改组名 |
| g    | 修改组id |



示例

```java
// 修改组名
groupmod -n new_name old_name

// 修改组id
grioupmod -g 1032 goup_1
```





# 权限相关

## 权限相关概念



| 字母 | 描述                  | 对于文件 | 对于目录                             |
| ---- | --------------------- | -------- | ------------------------------------ |
| r    | 对应数字4，可读权限   | 能看内容 | 浏览目录下的子目录名，子文件名       |
| w    | 对应数字2，可写权限   | 修改内容 | 创建，重命名，删除子目录名、子文件名 |
| x    | 对应数字1，可执行权限 | 执行文件 | 可以cd切换进入                       |



此外：

1. 对于需要删除某一目录下的文件，需要`wx`权限
2. 想要执行文件，需要`rx`权限



## 查看权限

```java
// 查看当前目录下文件的权限
ll
// 查看当前目录的权限
ls -dl 
```



## 权限分析



```
root@famel-virtual-machine:~# ls -dl /home/famel
drwxr-xr-x 16 famel famel 4096 11月 21 13:45 /home/famel
```



第一段：文件类型
```d```，表示目录
```-```，普通文件
```l```，快捷方式
```p```，管道文件
```c```，字符设备
```b```，磁盘文件
第二段：```rwx```，属主的权限
第三段：```r-x```，属组的权限
第四段：```r-x```，除属主和属组之外的其他用户权限rw
第五段：```famel```，属主
第六段：```famel```，属组
第七段：```4096```，已经占用大小



## 修改权限



指令

```
 chmod [OPTION]
```



选项

| 选项 | 描述     |
| ---- | -------- |
| u    | 属主     |
| g    | 属组     |
| o    | 其他用户 |



示例

```java
//将a.txt的rw权限赋给属主
chmod u=rw a.txt
//删除属组的read权限
chmod g-r a.txt
//属主权限rwx,属组权限rwx,其他用户权限r
chmod 774 a.txt
//修改多个，o=表示无任何权限
chomd  u=rw,g=rwx,o= a.txt
//对当前目录下所有的文件夹权限开放
chmod 777 *
```



## 为LINUX用户添加sudo操作权限

第一步：进入root

```
su - root
```



第二步:修改```sudoers```文件

``````java
vim /etc/sudoers

//找到该行
## Allow root to run any commands anywhere 
root    ALL=(ALL)       ALL
``````



第三步：添加

```java
//为单个用户添加sudo操作，需要输入密码
liisyu    ALL=(ALL)   ALL
//为单个用户添加sudo操作，不需要输入密码
liisyu    ALL=(ALL)   NOPASSWD: ALL
//为用户组添加sudo操作，需要输入密码
%mygroup  ALL=(ALL)   ALL
//为用户组添加sudo操作，不需要输入密码
%mygroup  ALL=(ALL)   NOPASSWD: ALL
```



# 系统相关


## 查看网络信息

```
netstat -tunalp
```



## 实时系统信息

按`1`可以展开CPU

```java
top

//输出如下
top - 21:56:49 up(开机时间)  2:48（运行时间）,  3 users（使用人数）,  load average: 0.00, 0.00, 0.00（平均负载：1分钟，5分钟，15分钟）
Tasks（任务）: 157 total（当前任务）,   1 running（正在运行的任务）, 156 sleeping,   0 stopped（等待）,（暂停）   0 zombie（僵尸进程）
Cpu(s):  0.0%us,  0.3%sy,  0.0%ni, 99.7%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Mem（内存）:   1003020k total,   852632k used,   150388k free,    72608k buffers
Swap:  2097148k total,        0k used,  2097148k free,   472880k cached

   PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND                                                                                                  
     1 root      20   0 19344 1572 1244 S  0.0  0.2   0:01.60 init                                                                                                      
     2 root      20   0     0    0    0 S  0.0  0.0   0:00.00 kthreadd                                                                                                  
     3 root      RT   0     0    0    0 S  0.0  0.0   0:00.00 migration/0                                                                                               
     4 root      20   0     0    0    0 S  0.0  0.0   0:00.00 ksoftirqd/0                                                                                               
     5 root      RT   0     0    0    0 S  0.0  0.0   0:00.00 stopper/0                                                                                                 
     6 root      RT   0     0    0    0 S  0.0  0.0   0:00.01 watchdog/0                                                                                                
     7 root      20   0     0    0    0 S  0.0  0.0   0:05.11 events/0     
```

  

## 查看cpu信息

```java
// cpu信息保存在/proc/cpuinfo文件中
cat /proc/cpuinfo
```



## 查看指令路径

指令

```
which [options] [--] programname [...]
```



示例

```java
//查看ls指令的路径
which ls
```



### 命令的执行过程

以`ls`为例，解释命令是如何执行的
1. 首先需知道，执行```ls```命令，相当于执行`/bin/ls`,这两个操作时一样的
2. 可以通过`which ls`来查看命令的路径
```linux
[root@famel Packages]# which ls
alias ls='ls --color=auto'
	/bin/ls
```
3. 当执行`ls`命令时，该命令会传给`shell`
4. `shell`会从`PAHT`环境变量查找
5. 通过`echo $PATH`,可以查看环境变量的值
```linux
[root@famel Packages]# echo $PATH
/usr/lib64/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
```
6. 在桌面模式执行时，会发现`ls`查看的文件有颜色，而`/bin/ls`没有颜色。此时再看`which ls`的查询信息
`alias ls='ls --color=auto'`: 别名，也就说当ls执行时，其实执行的是`ls --color=auto`
也就是说`ls`真正等同于 `/bin/ls --color=auto`





## 别名

```java
//执行`test`命令时,就相当于执行了`ll /dev`
alias test='ll /dev'
```



## 关机

```
init 0
```

## 立即关机
```
shutdown -h now
```
## 重启
```
init 6
```

## 多用户终端切换
```
ctrl + alt + f1~f6(文本界面) \F7 (图形界面)
```

##  查看日期
```
date
```

## 查看日历
```
cal
```

+ 年: 查看某年的日历，如`cal 1990`
+ 月 年:查看某年的某月的日历,如`cal 3 1998`


## 帮助文档
```
指令 --help
man 指令
```
## 分页查看（上下）
```java
//通过键盘上下键翻阅
ls --help | less
```
## 百分比查看

```java
//百分比查看信息,通过回车翻页
ls --help | more
```



## 历史

查看历史

```
history
```
使用历史

```
! 历史编号
```



查看系统内核信息

```java
uname -a

//输出如下
Linux famel-virtual-machine  主机名
4.4.0-21-generic #37-Ubuntu 
SMP Mon Apr 18 18:33:37 UTC 2016 时间
 x86_64 x86_64 x86_64 GNU/Linux
```



# 目录相关

**事实上，LInux的思想是一切皆文件，所以应该把目录看出一个特殊的文件**

## 目录结构

| 目录       | 描述         |
| ---------- | ------------ |
| boot       | 启动相关     |
| bin        | 常用指令相关 |
| etc        | 配置文件相关 |
| sbin       | 系统命令相关 |
| media或mnt | 挂载相关     |
| home       | 家目录相关   |
| dev        | 设备文件相关 |



## 进入目录

```java
//需要x权限
cd /a/b
//进入上级目录
cd ..
//进入上次所在的目录
cd -
//进入用户家目录
cd ~
//进入根目录
cd /
```



## 添加目录

指令

```
mkdir
```



选项

| 选项 | 描述         |
| ---- | ------------ |
| p    | 递归添加目录 |



示例

```java
// 即使a，b目录不存在，也可以
mkdir -p /a/b/c
```





## 删除目录

需要wx权限

指令

```
rm
```

选项

| 选项 | 描述         |
| ---- | ------------ |
| r    | 递归方式删除 |
| f    | 无提示       |

示例

```java
//不推荐！！
rm -rf /a/b
```



### 删除目录推荐做法

```java
//将需要删除的目录移动到tmp文件夹，这样可以找回
mv /a/b/ /tmp
```



## 重命名目录

```java
// 同一目录下，即为重命名
mv new_name 文件/目录
```



## 查看当前所在目录

```
pwd
```

## 查看目录

指令

```
ls
```

选项

| 选项 | 描述                               |
| ---- | ---------------------------------- |
| d    | 列出目录本身，而不是查看它的内容   |
| l    | 查看目录详细信息,可简写成`ll`      |
| a    | 查看当前目录所有内容(包括隐藏文件) |



## 复制目录

```
cp from  to
```



## 更改目录属主和属组

```java
//将/home/famel目录的属主和属组改为famel
chown  famel.famel /home/famel
//-R：以递归方式操作文件和目录,将/home/famel 下所有的目录的属主和属组都改为famel
chown -R famel.famel /home/famel
```



## 计算目录大小

指令

```
du
```

| 选项 | 描述                   |
| ---- | ---------------------- |
| s    | 每个参数只显示一个总数 |
| h    | 输出大小               |

示例

```java
//计算/a文件大小
du -sh /a
```



# 文件相关

## 新建文件

```
touch 文件名
```

## 修改文件名

```java
//同目录下即为重命名
mv new_name 文件
```

## 删除文件

```
rm 文件
```

## 复制文件

```
cp from to
```



## 查看文件内容

### 查看文件内容

```java
cat 文件
```

### 分页查看

```java
less 文件
```

### 百分比查看

```java
more 文件
```

### 查看文件开头

```java
//默认10行
head 文件
// 指定行数
head -n 3 a.txt
```

### 查看文件尾部

```java
//默认10行
tail
// 指定行数
tail -n 3 a.txt
//文件内容追踪
tail -F test.log
```

### 修改文件内容

```java
//覆盖方式改写文件
echo 123456  a.txt
//追加方式
echo 123456 >> a.txt

```





## 系统文件详情介绍

### passwd文件

用户信息文件，位于 `/etc/passwd`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181121133848397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01BU09STA==,size_16,color_FFFFFF,t_70)
以```famel:x:1000:1000:Famel,,,:/home/famel:/bin/bash```为例
第一段：```famel```,用户名
第二段：```x```,密码的占位符
第三段：```1000```,uid
第四段：```1000```,gid
第五段：```Famel,,,```,用户描述信息
第六段：```/home/famel```,用户家目录
第七段：shell命令
```/bin/bash```: 用户登录shell（可登录）
```/sbin/nologin```: 用户不可登录（ftp可登录）
```/bin/false```: 一切服务均不可登录，最严格



### shadow文件

(用户密码文件)文件，位于 `/ect/shadow` 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181121134744413.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01BU09STA==,size_16,color_FFFFFF,t_70)
以如下行为例```famel:$6$7Xe...:17855:0:99999:7:::```
第一段：```famel```,用户名
第二段：```$6$7Xe...```,加密后的密码
第三段：```17855```
第四段：```0```
第五段：```99999```
第六段：```7```

### group文件

组文件，位于： `/etc/group`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181121141006499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01BU09STA==,size_16,color_FFFFFF,t_70)
以```famel:X:1000:```为例
第一段：```famel```,组名
第二段：```x```,组密码占位符
第三段：```1000```,组id
第四段：组员(famel 用户肯定在famel，所以这里是空的)，这里表示的是附加组

### gshadow文件

组密码文件,位于 `/etc/gshadow`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181121141809870.png)



# 归档/压缩

归档指的是将多个文件组合成一个文件，而压缩则会将文件大小压缩为更小的文件

## 归档

指令

```
tar [OPTION...] [FILE]
```



选项

| 选项 | 描述                     |
| ---- | ------------------------ |
| c    | 创建文件                 |
| C    | 指定解压位置             |
| f    | 指定归档文件             |
| v    | 显示信息                 |
| t    | 查看归档文件             |
| x    | 解压归档文件             |
| z    | 归档后使用`gzip`进行压缩 |
| j    | 归档后使用bzip2进行压缩  |

## 压缩

指令

```java
gzip //gzip方式压缩,压缩时间短，压缩效率没bzip2好
bzip2//bzip方式压缩,压缩时间久，压缩率高
```

**注意：只能压缩单个文件，多个文件需进行归档**



选项

| 选项 | 描述   |
| ---- | ------ |
| d    | 解压缩 |
|      |        |
|      |        |
|      |        |
|      |        |
|      |        |



示例

```java
//压缩a.txt.得到a.txt.gz
gzip a.txt
//压缩a.txt.得到a.txt.bz2
bzip2 a.txt
```

# Vim编辑器

用于编辑文，指令

```
vim 文件
```



存在三个模式：`命令行模式`、`编辑模式`、`扩展模式`



## 命令行模式

| 键位     | 描述                                                         |
| -------- | :----------------------------------------------------------- |
| a        | 进入编辑模式,光标会相对原处后移一位                          |
| i        | 进入编辑模式，光标不动                                       |
| o        | 进入编辑模式，直接下一行，且新建成空行                       |
| O(大o)   | 光标跑到当前行头部                                           |
| :        | 进入扩展模式                                                 |
| $        | 光标跑到当前行尾部                                           |
| dd       | 剪切当前行                                                   |
| ndd      | n为数字,如```3dd```，表示从光标位置开始三行进行剪切操作（包括光标位置的行） |
| p        | 粘贴剪切板内容到光标的下一行（通过dd和yy操作的内容）         |
| P        | 粘贴剪切板内容到光标的上一行（通过dd和yy操作的内容）         |
| yy       | 复制当前行                                                   |
| nyy      | n为数字,如```3yy```，表示从光标位置开始三行进行复制操作（包括光标位置的行） |
| u        | 撤销操作                                                     |
| ctrl+r   | 重复操作                                                     |
| G        | 光标移动到最后一行                                           |
| nG       | 光标移动到第n行，从1开始算                                   |
| gg       | 光标移动到第一行，相当于1G                                   |
| H        | 跳到屏幕的开头                                               |
| M        | 跳到屏幕的中间                                               |
| L        | 跳到屏幕的结尾                                               |
| /+字符串 | 查找关键字                                                   |
| n        | 查找下一个，与查找关键字一起用                               |
| N        | 查找上一个，与查找关键字一起用                               |

## 编辑模式

| 键位 | m描述          |
| ---- | -------------- |
| ESC  | 进入命令行模式 |



## 扩展模式

| 键位模式 | 描述     |
| -------- | -------- |
| w        | 保存     |
| q        | 退出     |
| ！       | 强制操作 |



# 磁盘相关

## 查看I/O信息

```
iostat
```



## 测试磁盘速度

指令

```
dd
```



选项

| 选项  | 描述         |
| ----- | ------------ |
| if=   | 读取文件     |
| of=   | 写入文件     |
| bs=   | 读写大小     |
| count | 执行读写次数 |



示例

```java
//从/dev/sbd1读数据写到a.txt,大小为500M每次，次数为2次
famel@famel-virtual-machine:~$ sudo dd if=/dev/sdb1 of=/a.txt bs=500M count=2
2+0 records in
2+0 records out
1048576000 bytes (1.0 GB, 1000 MiB) copied, 19.2122 s, 54.6 MB/s
```



## 查看磁盘信息

指令

```
df
```



选项

| 选项 | 描述                   |
| ---- | ---------------------- |
| T    | 查看磁盘系统信息       |
| h    | 查看磁盘大小和使用情况 |
| i    | 查看磁盘inode相关信息  |



## 磁盘分区

指令

```
fdisk
```



选项

| 选项 | 描述         |
| ---- | ------------ |
| l    | 查看分区信息 |



示例

```java
//为sdb这个磁盘进行分区
fdisk /dev/sdb
//查看sda的分区信息
fdisk -l /dev/sda
```



### 磁盘分区信息解读

part1:

```java
//Disk /dev/sda：表明 /dev/sda是一个磁盘
//25 GiB：磁盘大小为25GB
//26843545600 bytes,磁盘大小字节为26843545600
//52428800 sectors：有52428800 个扇区

Disk /dev/sda: 25 GiB, 26843545600 bytes, 52428800 sectors
```



part2:

```java
//每一个扇区单位是512字节
Units: sectors of 1 * 512 = 512 bytes
```



part3:

```java
//扇区大小(逻辑/物理):512字节/512字节
Sector size (logical/physical): 512 bytes / 512 bytes
```



part4:

```java
//I/O大小(最小/最佳):512字节/512字节
I/O size (minimum/optimal): 512 bytes / 512 bytes
```



part5:

```java
//磁盘标签类型:dos,dos的分区方式磁盘必须< 2T
Disklabel type: dos
```



part6:

```java
//磁盘标识符:0x60356c74
Disk identifier: 0x60356c74
```



part7:

```java
//磁盘分区大小等
Device     Boot   Start      End  Sectors  Size Id Type
/dev/sda1  *       2048  3999743  3997696  1.9G 82 Linux swap / Solaris
/dev/sda2       4001790 52426751 48424962 23.1G  5 Extended
/dev/sda5       4001792  4976639   974848  476M 83 Linux
/dev/sda6       4978688 52426751 47448064 22.6G 83 Linux
```



## 格式化磁盘文件系统

```java
//格式化为ext4系统（日志文件系统(写数据，先缓存，再写入到硬盘)）
mkfs.ext4 /dev/sdb1
//格式化为xfs系统
mkfs.xfs /dev/sdb1
```



## 分区操作

磁盘主要分为

1. 主分区：只能创建4个，且一旦创建了4个主分区后无法创建扩展分区（即使磁盘有剩余）
2.  扩展分区:一般分完主分区后剩下的容量来创建扩展分区,创建完扩展分区后，可以创建数个逻辑分区（非无限制建，跟磁盘有关）
3. 逻辑分区：对扩展分区再次分区就是逻辑分区
    &emsp;&emsp;


### 磁盘分区操作

第一步：执行命令`fdisk /dev/sdb `，进行分区操作

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181122124139889.png)





第二步：输入命令,如果不知道命令，可以输入`m`查看命令



![在这里插入图片描述](https://img-blog.csdnimg.cn/20181122124428200.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01BU09STA==,size_16,color_FFFFFF,t_70)



可输入命令`p`,可查看分区表信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181122124706447.png)



可输入命令`n`,新建分区

`0 primary, 0 extended,4free`:表名该磁盘有0个主分区，0个扩展分区，可以创建4个主分区

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181122125016699.png)



第三步：输入`p`创建主分区,或输入`l`创建逻辑分区或输入`e`创建扩展分区

```linux
Partition number (1-4, default 1):            设置分区编号（默认1）
First sector (2048-41943039, default 2048):  设置起始扇区（默认从2048开始）
Last sector, +sectors or +size{K,M,G,T,P} (2048-41943039, default 41943039): +2G （设置扇区末尾，+2G表示2G大小的分区）
Created a new partition 1 of type 'Linux' and of size 2 GiB. 创建了一个2G大小的分区
```


第四步可选选项

1. 输入`q`,退出分区操作且不保存
2. 输入`d`.删除分区

```
 Partition number (1,2, default 2):  通过分区编号进行删除，输入p可以查看分区编号
```



3. 输入`w`,保存分区操作



第五步：为磁盘添加挂载点

1. 创建一个`/sdb1`文件夹
2.  对`/dev/sdb1`进行格式化系统
3. 挂载：`mount /dev/sdb1 /sdb1`



## 挂载点

挂载点：磁盘的入口，对磁盘进行存储删除等操作的通道



指令

```java
//挂载
mount 磁盘 挂载文件夹

//卸载挂载点
umount 磁盘

//强制卸载挂载点
umount -l  磁盘
```





# 内存
## 查看内存信息

指令

```
free
```

选项

| 选项 | 描述                                                  |
| ---- | ----------------------------------------------------- |
| m    | 以兆为单位输出信息                                    |
| k    | 以kb为单位输出信息                                    |
| w    | 以宽屏的方式显示数据，此模式下buffer和cache会独立显示 |



### 信息解析

`tota`:总内存
`used`:已使用内存
`free`: 剩余内存
`shared`：共享内存
`buffer`:内核缓冲区使用的内存(Buffers in /proc/meminfo),用于存放要写入disk(块设备)的数据
`cache`:页面缓存和slabs使用的内存(Cached and Slab in /proc/meminfo)，用于存放从disk读出的数据
`avaliable`:预计还可以使用的内存

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181123151247772.png)



## swap分区

### 启动swap分区

指令

```
swapon
```

选项

| 选项 | 描述                 |
| ---- | -------------------- |
| s    | 显示交换设备相关信息 |
| a    | 激活swap分区         |



示例

```java
//假设sdb7已经制作成swap分区
swapon -a /dev/sdb7 
```



### 禁用swap分区

指令

```
swapoff
```



示例

```java
//禁用sdb7分区
swapoff /dev/sdb7
```



### 制作swap分区

指令

```
mkswap
```



示例

```java
//将磁盘sdb7制作成swap分区
mkswap /dev/sdb7
```

# 进程管理

## 查找进程

```java
//查找与mysql相关的进程
ps aux | grep mysql
```

## 杀死进程

```java
kill -9 pid
```

## 后台运行

```java
//后台运行firefox
firefox &
```

## 查询后台运行进程 

```
jobs
```



# 包管理相关


## 安装二进制软件包

指令

```
rpm
```

选项

| 选项 | 描述               |
| ---- | ------------------ |
| i    | 安装rpm包          |
| v    | 显示详细信息       |
| h    | 显示安装进度条     |
| q    | 显示安装软件的信息 |
| e    | 卸载安装的软件     |



## YUM

yum源位置：`/etc/yum.repos.d/`

指令

```
yum
```

选项

| 选项         | 描述                                    |
| ------------ | --------------------------------------- |
| clean all    | 清除缓存，`yum clean all`               |
| -y install   | 安装软件包                              |
| -y earse     | 卸载软件包                              |
| -y makecache | 建立缓存,提高装软件包速度（不用检索源） |
| -y reinstall | 重新覆盖安装                            |
| -y update    | 更新源                                  |
| list         | 查看安装的软件                          |



### YUM回滚操作

比如,安装了软件`gitlab-ee`



第一步：查看操作

```java
// 查看软件的yum操作
sudo yum history list gitlab-ee

//显示如下
ID     | Login user               | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
5 | famelee <famel>          | 2018-11-30 22:27 | Install        |    1 **
```



第二步：进行回滚(**通过id**)

```
sudo yum history undo 5
```



注意：回滚操作会删除依赖





### yum源文件

位置：`/etc/yum.repos.d/xxx.repo`

```linux
name=   :源名
baseurl=：源URl
本地：file:// + url,如file:///dev表示本地的/dev为源路径
enable=1; 1表示开启yum源
gpgcheck=0；不检测

```
### yum 配置文件

位置：`/etc/yum.conf`

```linux
[main]
cachedir=/var/cache/yum/$basearch/$releasever 缓存路径
keepcache=0 1表示保存缓存
debuglevel=2
logfile=/var/log/yum.log
exactarch=1
obsoletes=1
gpgcheck=1
plugins=1
installonly_limit=5
bugtracker_url=http://bugs.centos.org/set_project.php?project_id=19&ref=http://bugs.centos.org/bug_report_page.php?category=yum
distroverpkg=centos-release
```





<!-- centos安装 -->

# CentOS最小化安装后联网

选择最小化安装Centos后，Linux是没有联网的,可以使用以下命令查看是否分配了ip地址

```
ip addr
```

成功分配ip地址显示如下

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:74:a2:d2 brd ff:ff:ff:ff:ff:ff
    inet 192.168.171.128/24 brd 192.168.171.255 scope global noprefixroute dynamic ens33
       valid_lft 1522sec preferred_lft 1522sec
    inet6 fe80::a3d7:62c5:cd2e:7350/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

如果没有成功的话，就需要输入以下命令来寻找网卡

```
cd /etc/sysconfig/network-scripts/
```

该目录下有两个 `ifcfg` 开头的文件

```
-rw-r--r--. 1 root root    52 Feb 11 10:03 ifcfg-eth0
-rw-r--r--. 1 root root   254 Mar 22  2017 ifcfg-lo
```

修改除`ifcfg-lo`的另一个文件

```
vi ifcfg-eth0

# 将 ONBOOT 改为 yes

```

保存后重启网卡服务

```
service network restart
```

再次执行 `ip addr`  查看是否分配了ip

# 常用工具的安装

安装 ifconfig

```
yum install net-tools
```

安装vim

```
yum install vim
```

# JDK 安装

[JDK 8官网下载](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

选择linux版本下载(这里我使用的是IDM提取的链接),然后使用 `wget` 工具进行下载

```
wget https://download.oracle.com/otn-pub/java/jdk/8u201-b09/42970487e3af4f5aa5bca3f542482c60/jdk-8u201-linux-x64.tar.gz?AuthParam=1554275167_27b6bf4971db78028f7ae08fac86c9ee
```

下载完成后，进行解压(有可能文件名不对，手动改为.tar.gz结尾)

```
sudo tar -zxvf jdk-8u201-linux-x64.tar.gz -C /usr/local/
```

配置环境变量

```
sudo vim /etc/profile

# 在末尾输入
export JAVA_HOME=/usr/local/jdk1.8.0_201
export PATH=$JAVA_HOME/bin:$PATH
```

立即更新环境变量

```
source /etc/profile
```

# MySQL安装

第一步，添加 [MySQL官方库](https://dev.mysql.com/downloads/repo/yum/),选择合适的rpm包

```
# 替换platform-and-version-specific-package-name.rpm
sudo yum local install platform-and-version-specific-package-name.rpm
```

第二步，选择合适的库

```
# 查看哪些库已启用
yum repolist all | grep mysql

# 可以看到当前是8.0 版本被启用
mysql55-community/x86_64           MySQL 5.5 Community Server    disabled
mysql55-community-source           MySQL 5.5 Community Server -  disabled
mysql56-community/x86_64           MySQL 5.6 Community Server    disabled
mysql56-community-source           MySQL 5.6 Community Server -  disabled
mysql57-community/x86_64           MySQL 5.7 Community Server    disabled
mysql57-community-source           MySQL 5.7 Community Server -  disabled
mysql80-community/x86_64           MySQL 8.0 Community Server    enabled:     82

```

禁用版本和选择版本,打开 `mysql-community.repo`文件

```
sudo vim /etc/yum.repos.d/mysql-community.repo

# 如要选择5.7版本，则将enabled 改为1，并将其他改为0表示禁用
# Enable to use MySQL 5.7
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

```

通过以下指令验证是否正确禁用启用某些库

```
yum repolist enabled | grep mysql
```

第三步，安装

```
sudo yum install mysql-community-server
```

第四步，启动服务

```
sudo service mysqld start
```

检查服务器启动状态

```
sudo service mysqld status
```

服务器初次启动时，MySQL已经将root用户和密码保存在错误日志中,通过以下指令来查找密码

```
sudo grep 'temporary password' /var/log/mysqld.log
```

修改密码

```
# 登录
mysql -uroot -p
# 修改密码
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword!';
```



# Centos 7防火墙

CentOS 7.0默认使用的是firewall作为防火墙

开启/关闭

```
systemctl stop firewalld.service && systemctl disable firewalld.service

systemctl start firewalld.service && systemctl enable firewalld.service
```

查看指定端口是否已经打开

```
firewall-cmd --query-port=666/tcp
```

开放端口(开放完后需重载)

```
firewall-cmd --add-port=8080/tcp --permanent
```

防火墙重载

```
firewall-cmd --reload
```

查看防火墙运行状态

```
systemctl status firewalld
```

# 修改启动顺序

```
sudo vim /ect/default/grub
```

修改

```
GRUB_DEFALUT= 2 //修改启动顺序

GRUB_TIMOUT = 3 //修改等待时间
```

更新

```
sudo update-grub
```



# 搜狗输入法

**第一步**   
打开ubuntu自带的商店，搜索*fcitx*,安装搜索出来的3个软件  
**第二步**  
下载[suogou输入法linux版本](https://pinyin.sogou.com/linux/?r=pinyin)   
**第三步**    
->打开系统设置*Region&Language*     
->*Manage Installed Languages* ，跳出弹框，忽略即可  
->*Keyboard input method system*,选择fcitx  
-> 打开软件*fcitx configutrayion*,添加*souguo pinyin*放第一个就好了

# JDK

[JDK下载官网](https://www.oracle.com/technetwork/java/javase/downloads/index.html)  
解压：  

```linux
sudo tar -zxvf jdk-8u191-linux-x64.tar.gz -C /usr/local
```

编辑环境变量

```linux
 vim ~/.bashrc
```

 输入以下内容（注意jdk文件夹的名字）

```linux
 export JAVA_HOME=/usr/local/jdk1.8.0_181
 export PATH=${JAVA_HOME}/bin:$PATH
```

使配置生效

```linux
source ~/.bashrc 
```

JeQ3MHvmKhp2T%JhMd

# 安装VM虚拟机

下载14.x的版本  ，有免费版本
[VM下载官网](https://my.vmware.com/cn/web/vmware/info/slug/desktop_end_user_computing/vmware_workstation_pro/14_0)  
下载完后添加权限

```linux
sudo chmod +x VMware-Workstation-Full-14.1.2-8497320.x86_64.bundle
```

安装

```linux
sudo ./VMware-Workstation-Full-14.1.2-8497320.x86_64.bundle
```



如果没有安装*gcc*,还不能打开，通过以下命令安装*gcc*

```linux
sudo apt-get  install  build-essential
```

卸载

```
sudo vmware-installer -u vmware-workstation
```

可能提示需要输入密钥，直接跳过，个人使用免费

在安装系统过程中，如果出现could not open /dev/vmmon:????????.最简单的方法是关闭电脑的安全启动。

或者更加详细看https://blog.sunriseydy.top/technology/server-blog/server/vmware_cannot_open_vmmon/。

# Idea

[idea下载官网](https://www.jetbrains.com/idea/download/#section=linux)

解压  

```linux
sudo tar -zxvf ideaIC-2018.3.1-no-jdk.tar.gz -C /usr/local/
```

通过命令打开

```linux
/usr/local/idea-IC-183.4588.61/bin/idea.sh
```

配置,打开后，不要跳过配置，根据自动配置，可以在lunix系统上创建启动器等，省得自己配了！！！  
如果出现:

```linux
 Failed to load module "canberra-gtk-module"
```

那么就执行命令

```linux
sudo apt-get install libcanberra-gtk-module
```

如果无法以图标的方式打开，则

```linux
sudo vim /etc/environment 
# 加入这句话
JAVA_HOME="/usr/local/jdk1.8.0_191"
```

注销用户重新登录即可，原因与linux不同的环境变量有关。



**关闭vim风格**

**菜单栏:tools->vim emulator**

# 其他软件

**Google 浏览器**：[谷歌浏览器官网](https://www.google.cn/chrome/index.html)  
**WPS**：[wps官网](http://www.wps.cn/product/wpslinux#)  
**vscode**： [vscode下载官网](https://code.visualstudio.com/)  

# 美化 

**第一步，安装美化工具**

```linux
sudo apt install gnome-tweak-tool
```

**第二步，去下载插件或扩展**  
[扩展下载地址](https://extensions.gnome.org/ )  
注意：不要用谷歌浏览器，因为新版的谷歌开始不支持插件了。  
打开之后，先安装一个浏览器插件。否则会有相应的警告，这样是无法安装扩展的。  
**第三步，安装常用扩展**  
*User THemes*：用来设置主题的  
*Hide top Bar*：用来隐藏lunix上边栏
*Dock to bash*：用来设置启动器的

**美化资源下载网站**    
[wallhaven,壁纸下载](https://alpha.wallhaven.cc/)  
[各种主题下载](https://www.opendesktop.org/s/Gnome)

# 更换锁屏登录壁纸

输入以下命令

```linux
sudo vim /usr/share/gnome-shell/theme/ubuntu.css
```

搜索关键字*lockDialogGroup*,修改如下示例

```linux
 background: #2c001e url(file:///home/famel/Pictures/wallhaven-32182.jpg);
```

# 安装xmind8

[xmind官网](https://www.xmind.cn/download/xmind8/)  
下载软件后，解压后进根目录, 输入

```linux
sudo ./setup.sh --fix-missing
```

添加快捷方式  
先添加图标

```linux
sudo vim ~/.local/share/applications/xmind.desktop
```

输入以下内容

```
[Desktop Entry]
Type=Application
Path=<path to xmind unzip dir>/XMind_amd64
Exec=<path to xmind unzip dir>/XMind_amd64/XMind
Name=XMind
Comment=Create mind maps
GenericName=Planning Tool
Icon=<path to xmind unzip dir>/XMind_amd64/configuration/org.eclipse.osgi/983/0/.cp/icons/xmind.256.png
Categories=Office;
```

*< path to xmind unzip dir >*对应xmind目录

# 安装gcc编译器

```shell
# c编译器
yum -y install gcc
# c++ 编译器
yum -y install gcc-c++
```

