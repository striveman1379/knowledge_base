## 文件和目录 ##
###### cd /home 　进入 "/home 目录"
###### cd .. 	　　返回上一级目录
###### cd ../.. 　返回上两级目录
###### cd ~	　　进入个人的目录
###### cd -	　　返回上次所在的目录
###### pwd 　　显示当前路径
###### ls 　　查看目录中的文件
###### ls -f 　查看目录中的所有文件
###### ls -l 　显示文件和目录的详细资料
###### ls -a 　显示隐藏文件
###### ls \*[0-9]\* 　显示包含数字的文件名和目录名

###### ls -l |grep "^-"|wc -l  　　统计当前文件夹下文件的个数

###### ls -l |grep "^d"|wc -l  　　统计当前文件夹下目录的个数

###### ls -lR|grep "^-"|wc -l  　　统计当前文件夹下文件的个数，包括子文件夹里的

###### ls -lR|grep "^d"|wc -l  　　统计文件夹下目录的个数，包括子文件夹里的

###### tree 　　显示文件和目录由根目录开始的树形结构
###### mkdir dir1 　　创建一个叫做 'dir1' 的目录'
###### mkdir dir1 dir2 　　同时创建两个目录
###### mkdir -p /tmp/dir1/dir2 　　创建一个目录树
###### rm -f file1 　　删除一个叫做 'file1' 的文件'
###### rmdir dir1 　　删除一个叫做 'dir1' 的目录'
###### rm -rf dir1 　　删除一个叫做 'dir1' 的目录并同时删除其内容
###### rm -rf dir1 dir2 　　同时删除两个目录及它们的内容
###### mv dir1 new_dir 　　重命名/移动 一个目录
###### cp file1 file2 　　复制一个文件
###### cp dir/* . 复制一个目录下的所有文件到当前工作目录
###### cp -a /tmp/dir1 . 　　复制一个目录到当前工作目录
###### cp -a dir1 dir2 　　复制一个目录
###### ln -s file1 lnk1 　　创建一个指向文件或目录的软链接
###### ln file1 lnk1 　　创建一个指向文件或目录的物理链接

---

## 查看文件内容

###### cat file1 　　从第一个字节开始正向查看文件的内容

###### tac file1 　　从最后一行开始反向查看一个文件的内容

###### more file1 　　查看一个长文件的内容

###### less file1 　　类似于 'more' 命令，但是它允许在文件中和正向操作一样的反向操作

###### head -2 　　file1 查看一个文件的前两行

###### tail -2 file1 　　查看一个文件的最后两行

###### tail -f 　　/var/log/messages 实时查看被添加到一个文件中的内容

---

## 文件搜索

###### find / -name file1  从 '/' 　　开始进入根文件系统搜索文件和目录

###### find / -user user1  　　搜索属于用户 'user1' 的文件和目录

###### find /home/user1 -name *.bin  　　在目录 '/ home/user1' 中搜索带有'.bin' 结尾的文件

###### find /usr/bin -type f -atime +100  　　搜索在过去100天内未被使用过的执行文件

###### find /usr/bin -type f -mtime -10  　　搜索在10天内被创建或者修改过的文件

###### find / -name *.rpm -exec chmod 755 '{}'  　　搜索以 '.rpm' 结尾的文件并定义其权限

###### find / -xdev -name *.rpm  　　搜索以 '.rpm' 结尾的文件，忽略光驱、捷盘等可移动设备

###### find /data -size +1k|xargs du -hs

###### locate *.ps  　　寻找以 '.ps' 结尾的文件 - 先运行 'updatedb' 命令

###### whereis halt  　　显示一个二进制文件、源码或man的位置

###### which halt  　　显示一个二进制文件或可执行文件的完整路径

---

## 磁盘空间
###### df -h 　　显示已经挂载的分区列表

###### ls -lSr |more 　　以尺寸大小排列文件和目录

###### du -sh dir1 估算目录 'dir1' 　　已经使用的磁盘空间'

###### du -sk * | sort -rn 　　以容量大小为依据依次显示文件和目录的大小

---

## 打包和压缩文件

###### zip file1.zip file1 　　创建一个zip格式的压缩包

###### zip -r file1.zip file1 file2 dir1 　　将几个文件和目录同时压缩成一个zip格式的压缩包

###### unzip file1.zip 　　　解压一个zip格式压缩包

###### bunzip2 file1.bz2 　　解压一个叫做 'file1.bz2'的文件

###### bzip2 file1 　　压缩一个叫做 'file1' 的文件

###### gunzip file1.gz 　　解压一个叫做 'file1.gz'的文件

###### gzip file1 　　压缩一个叫做 'file1'的文件

###### gzip -9 file1 　　最大程度压缩

###### rar a file1.rar test_file 　　创建一个叫做 'file1.rar' 的包

###### rar a file1.rar file1 file2 dir1 　　同时压缩 'file1', 'file2' 以及目录 'dir1'

###### rar x file1.rar 　　解压rar包

###### unrar x file1.rar 　　解压rar包

###### tar -cvf archive.tar file1 　　创建一个非压缩的 tarball

###### tar -cvf archive.tar file1 file2 dir1 　　创建一个包含了 'file1', 'file2' 以及 'dir1'的档案文件

###### tar -tf archive.tar 　　显示一个包中的内容

###### tar -xvf archive.tar 　　释放一个包

###### tar -xvf archive.tar -C /tmp 　　将压缩包释放到 /tmp目录下

###### tar -cvfj archive.tar.bz2 dir1 　　创建一个bzip2格式的压缩包

###### tar -xvfj archive.tar.bz2 　　解压一个bzip2格式的压缩包

###### tar -cvfz archive.tar.gz dir1 　　创建一个gzip格式的压缩包

###### tar -xvfz archive.tar.gz 　　解压一个gzip格式的压缩包

---

## 用户和群组

###### groupadd group_name 　　创建一个新用户组

###### groupdel group_name 　　删除一个用户组

###### groupmod -n new_group_name old_group_name 　　重命名一个用户组

###### useradd -c "Name Surname " -g admin -d /home/user1 -s /bin/bash user1 　　创建一个属于 "admin" 用户组的用户

###### useradd user1 　　创建一个新用户

###### userdel -r user1 　　删除一个用户 ( '-r' 排除主目录)

###### usermod -c "User FTP" -g system -d /ftp/user1 -s /bin/nologin user1 　　修改用户属性

###### passwd 　　修改口令

###### passwd user1 　　修改一个用户的口令 (只允许root执行)

###### chage -E 2020-12-31 user1 　　设置用户口令的失效期限

###### pwck 　　检查 '/etc/passwd' 的文件格式和语法修正以及存在的用户

###### grpck 　　检查 '/etc/passwd' 的文件格式和语法修正以及存在的群组

###### newgrp group_name 　　登陆进一个新的群组以改变新创建文件的预设群组

---

## 文件的权限 - 使用 "+" 设置权限，使用 "-" 用于取消

###### ls -lh 　　显示权限

###### ls /tmp | pr -T5 -W$COLUMNS 　　将终端划分成5栏显示

###### chmod ugo+rwx directory1 　　设置目录的所有人(u)、群组(g)以及其他人(o)以读（r ）、写(w)和执行(x)的权限

###### chmod go-rwx directory1 　　删除群组(g)与其他人(o)对目录的读写执行权限

###### chown user1 file1 　　改变一个文件的所有人属性

###### chown -R user1 directory1 　　改变一个目录的所有人属性并同时改变改目录下所有文件的属性

###### chgrp group1 file1 　　改变文件的群组

###### chown user1:group1 file1 　　改变一个文件的所有人和群组属性

###### find / -perm -u+s 　　罗列一个系统中所有使用了SUID控制的文件

###### chmod u+s /bin/file1 　　设置一个二进制文件的 SUID 位 - 运行该文件的用户也被赋予和所有者同样的权限

###### chmod u-s /bin/file1 　　禁用一个二进制文件的 SUID位

###### chmod g+s /home/public 　　设置一个目录的SGID 位 - 类似SUID ，不过这是针对目录的

###### chmod g-s /home/public 　　禁用一个目录的 SGID 位

###### chmod o+t /home/public 　　设置一个文件的 STIKY 位 - 只允许合法所有人删除文件

###### chmod o-t /home/public 　　禁用一个目录的 STIKY 位

---

## 文本处理

###### cat file1 | command( sed, grep, awk, grep, etc...) > result.txt 　　合并一个文件的详细说明文本，并将简介写入一个新文件中

###### cat file1 | command( sed, grep, awk, grep, etc...) >> result.txt 　　合并一个文件的详细说明文本，并将简介写入一个已有的文件中

###### grep Aug /var/log/messages 　　在文件 '/var/log/messages'中查找关键词"Aug"

###### grep ^Aug /var/log/messages 　　在文件 '/var/log/messages'中查找以"Aug"开始的词汇

###### grep [0-9] /var/log/messages 　　选择 '/var/log/messages' 文件中所有包含数字的行

###### grep Aug -R /var/log/* 　　在目录 '/var/log' 及随后的目录中搜索字符串"Aug"

###### sed 's/stringa1/stringa2/g' example.txt 　　将example.txt文件中的 "string1" 替换成 "string2"

###### sed '/^$/d' example.txt 　　从example.txt文件中删除所有空白行

###### sed '/ *#/d; /^$/d' example.txt 　　从example.txt文件中删除所有注释和空白行

###### echo 'esempio' | tr '[:lower:]' '[:upper:]' 　　合并上下单元格内容

###### sed -e '1d' result.txt 　　从文件example.txt 中排除第一行

###### sed -n '/stringa1/p' 　　查看只包含词汇 "string1"的行

###### sed -e 's/ *$//' example.txt 　　删除每一行最后的空白字符

###### sed -e 's/stringa1//g' example.txt 　　从文档中只删除词汇 "string1" 并保留剩余全部

###### sed -n '1,5p;5q' example.txt 　　查看从第一行到第5行内容

###### sed -n '5p;5q' example.txt 　　查看第5行

###### sed -e 's/00*/0/g' example.txt 　　用单个零替换多个零

###### cat -n file1 　　标示文件的行数

###### cat example.txt | awk 'NR%2==1' 　　删除example.txt文件中的所有偶数行

###### echo a b c | awk '{print $1}' 　　查看一行第一栏

###### echo a b c | awk '{print $1,$3}' 　　查看一行的第一和第三栏

###### paste file1 file2 　　合并两个文件或两栏的内容

###### paste -d '+' file1 file2 　　合并两个文件或两栏的内容，中间用"+"区分

###### sort file1 file2 　　排序两个文件的内容

###### sort file1 file2 | uniq 　　取出两个文件的并集(重复的行只保留一份)

###### sort file1 file2 | uniq -u 　　删除交集，留下其他的行

###### sort file1 file2 | uniq -d 　　取出两个文件的交集(只留下同时存在于两个文件中的文件)

###### comm -1 file1 file2 　　比较两个文件的内容只删除 'file1' 所包含的内容

###### comm -2 file1 file2 　　比较两个文件的内容只删除 'file2' 所包含的内容

###### comm -3 file1 file2 　　比较两个文件的内容只删除两个文件共有的部分

---

## 其他常用命令

###### pwd： 　　显示当前所在位置

###### grep 要搜索的字符串 要搜索的文件 --color： 　　搜索命令，--color代表高亮显示
###### ps -ef/ps aux： 　　这两个命令都是查看当前系统正在运行进程，两者的区别是展示格式不同。如果想要查看特定的进程可以使用这样的格式：ps aux|grep redis （查看包括redis字符串的进程）
**==注意==**：如果直接用ps（（Process Status））命令，会显示所有进程的状态，通常结合grep命令查看某进程的状态。

###### kill -9 进程的pid： 杀死进程（-9 表示强制终止。）
==先用ps查找进程，然后用kill杀掉==

##### 网络通信命令：
###### 查看当前系统的网卡信息：ifconfig
###### 查看与某台机器的连接情况：ping
###### 查看当前系统的端口使用：netstat -an

###### shutdown： shutdown -h now： 　　指定现在立即关机；
###### shutdown +5 "System will shutdown after 5 minutes":　　指定5分钟后关机，同时送出警告信息给登入用户。
###### reboot： reboot： 重开机
###### reboot -w： 做个重开机的模拟（只有纪录并不会真的重开机）。



#### scp命令详解

先说下常用的情况：

两台机器IP分别为：A.104.238.161.75，B.43.224.34.73。

1、在A服务器上操作，将B服务器上/home/lk/目录下所有的文件全部复制到本地的/root目录下，

命令为：scp -r root@43.224.34.73:/home/lk /root。

2、在A服务器上将/root/lk目录下所有的文件传输到B的/home/lk/cpfile目录下，
命令为：scp -r /root/lk root@43.224.34.73:/home/lk/cpfile
