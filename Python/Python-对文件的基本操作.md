## python 对文件的操作
#### 涉及到的模块
##### os模块 — 操作系统的各种接口

##### shutil模块 — 提供了对文件和文件夹的一些高级操作

#### 创建文件
1.  os.mknod() 方法用于创建一个指定文件名的文件系统节点

```
os.mknod(filename[, mode=0600[, device=0]])
```



##### 参数

- filename -- 创建的文件系统节点

- mode -- mode指定创建或使用节点的权限, 组合 (或者bitwise) stat.S_IFREG, stat.S_IFCHR, stat.S_IFBLK, 和stat.S_IFIFO (这些常数在stat模块). 对于 stat.S_IFCHR和stat.S_IFBLK, 设备定义了 最新创建的设备特殊文件 (可能使用 os.makedev()),其它都将忽略。

- device -- 可选，指定创建文件的设备
 

##### 实例

```
#!/usr/bin/python3

import os
import stat

filename = '/tmp/tmpfile'
mode = 0600|stat.S_IRUSR

# 文件系统节点指定不同模式
os.mknod(filename, mode)
```

2. os.open() 方法用于打开一个文件，并且设置需要的打开选项，模式参数mode参数是可选的，默认为 0777。


```
os.open(file, flags[, mode]);
```
##### 参数
- file -- 要打开的文件
- flags -- 该参数有多种选项，多个使用 "|" 隔开：
- mode -- 参数是可选的，默认为 0777


##### 实例


```
#!/usr/bin/python3
  
import os, sys

# 打开文件
fd = os.open( "foo.txt", os.O_RDWR|os.O_CREAT )

# 写入字符串
os.write(fd, str.encode("This is test"))

# 关闭文件
os.close( fd )

print ("关闭文件成功!!")
```

3.  open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数，如果该文件无法被打开，会抛出 OSError。
 
**注意：** 
使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法。
open() 函数常用形式是接收两个参数：文件名(file)和模式(mode)。


```
open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```
##### 参数说明:

- file: 必需，文件路径（相对或者绝对路径）。
- mode: 可选，文件打开模式
- buffering: 设置缓冲
- encoding: 一般使用utf8
- errors: 报错级别
- newline: 区分换行符
- closefd: 传入的file参数类型
- opener:

模式`mode`指定文件的打开方式，默认为`r`，即文本只读。其他常见值中，`w`用于写入，`x`用于排他性创建，`a`用于追加到文件的末尾。在文本模式`t`下，如果未指定编码`encoding`，则使用的编码取决于系统。对于读取和写入字节，使用二进制模式`b`，无需指定编码。可用的模式有：
<table>
<tr>
<th>模式</th><th>用途</th>
</tr>
<tr><td>'r'</td><td>读（默认）</td></tr>
<tr><td>'w'</td><td>首先清空文件，然后写入</td></tr>
<tr><td>'x'</td><td>排他性创建，如文件已存在，抛出异常</td></tr>
<tr><td>'a'</td><td>在文件末尾追加</td></tr>
<tr><td>'b'</td><td>二进制模式</td></tr>
<tr><td>'t'</td><td>文本模式（默认）</td></tr>
<tr><td>'+'</td><td>打开磁盘文件进行更新（读写）</td></tr>
</table>
对于读写模式，有`r+`和`w+`两种，`r+`打开时不会清空文件，而`w+`打开时首先清空文件。对于二进制模式读写，`r+b`和`w+b`同理。


##### 实例


```
f = open('test.txt','r')
f.read()
f.close()
```

#### 文件关闭
1. `os.close(fd)`  
    关闭文件描述符`fd`
2. `file.close()`  
    将缓冲区中的内容写入文件，并关闭由`open()`返回的文件对象`file`。如果文件已经被关闭，则此方法不会产生任何影响。一旦文件关闭，对文件的任何操作都会引起`ValurError`  
    

#### 文件读取
1. os.read() 方法用于从文件描述符 fd 中读取最多 n 个字节，返回包含读取字节的字符串，文件描述符 fd对应文件已达到结尾, 返回一个空字符串。

```
os.read(fd,n)
```

##### 参数
- fd -- 文件描述符
- n -- 读取的字节

##### 实例

```
#!/usr/bin/python3

import os, sys
# 打开文件
fd = os.open("f1.txt",os.O_RDWR)
   
# 读取文本
ret = os.read(fd,12)
print (ret)

# 关闭文件
os.close(fd)
print ("关闭文件成功!!")
```

2. file.read() 方法用于从文件读取指定的字节数，如果未给定或为负则读取所有。

```
fileObject.read(); 
```
##### 参数
- size -- 从文件中读取的字节数。

##### 实例

```
#!/usr/bin/python3

# 打开文件
fo = open("test.txt", "r+")
print ("文件名为: ", fo.name)

line = fo.read(10)
print ("读取的字符串: %s" % (line))

# 关闭文件
fo.close()
```

3. file.readline() 方法用于从文件读取整行，包括 "\n" 字符。如果指定了一个非负数的参数，则返回指定大小的字节数，包括 "\n" 字符。


```
fileObject.readline();
```

##### 参数

- size -- 从文件中读取的字节数

##### 实例


```
#!/usr/bin/python
# -*- coding: UTF-8 -*-

# 打开文件
fo = open("test.txt", "r+")
print ("文件名为: ", fo.name)

line = fo.readline()
print ("读取第一行 %s" % (line))

line = fo.readline(5)
print ("读取的字符串为: %s" % (line))

# 关闭文件
fo.close()
```

4. file.readlines()  方法用于读取所有行(直到结束符 EOF)并返回列表，该列表可以由 Python 的 for... in ... 结构进行处理。 如果碰到结束符 EOF 则返回空字符串。

```
fileObject.readlines( );
```
##### 实例


```
#!/usr/bin/python3
 
# 打开文件
fo = open("test.txt", "r")
print ("文件名为: ", fo.name)
 
for line in fo.readlines():                          #依次读取每行  
    line = line.strip()                             #去掉每行头尾空白  
    print ("读取的数据为: %s" % (line))
 
# 关闭文件
fo.close()
```

#### 文件写入

1. os.write() 方法用于写入字符串到文件描述符 fd 中. 返回实际写入的字符串长度
    
```
os.write(fd, str)
```
##### 参数
- fd -- 文件描述符
- str -- 写入的字符串

##### 实例


```
#!/usr/bin/python3

import os

# 打开文件
fd = os.open("f1.txt",os.O_RDWR|os.O_CREAT)

# 写入字符串
str = "This is test file"
ret = os.write(fd,bytes(str, 'UTF-8'))

# 输入返回值
print ("写入的位数为: ")
print (ret)

print ("写入成功")

# 关闭文件
os.close(fd)
print ("关闭文件成功!!")
```
2. write() 方法用于向文件中写入指定字符串。
在文件关闭前或缓冲区刷新前，字符串内容存储在缓冲区中，这时你在文件中是看不到写入的内容的。
如果文件打开模式带 b，那写入文件内容时，str (参数)要用 encode 方法转为 bytes 形式，否则报错：TypeError: a bytes-like object is required, not 'str'。

```
fileObject.write( [ str ])
```

##### 实例

```
#!/usr/bin/python3

# 打开文件
fo = open("haha.txt", "r+")
print ("文件名: ", fo.name)

str = "6:www.haha.com"
# 在文件末尾写入一行
fo.seek(0, 2)
line = fo.write( str )

# 读取文件所有内容
fo.seek(0,0)
for index in range(6):
    line = next(fo)
    print ("文件行号 %d - %s" % (index, line))

# 关闭文件
fo.close()
```

3. writelines() 方法用于向文件中写入一序列的字符串。
这一序列字符串可以是由迭代对象产生的，如一个字符串列表。
换行需要制定换行符 \n。


```
fileObject.writelines( [ str ])
```

##### 参数
- str -- 要写入文件的字符串序列。

##### 实例


```
#!/usr/bin/python3

# 打开文件
fo = open("test.txt", "w")
print ("文件名为: ", fo.name)
seq = ["菜鸟 1\n", "菜鸟 2"]
fo.writelines( seq )

# 关闭文件
fo.close()
```

#### 裁剪

1. os.ftruncate() 裁剪文件描述符fd对应的文件, 它最大不能超过文件大小。

```
os.ftruncate(fd, length)
```

##### 参数
- fd -- 文件的描述符。
- length -- 要裁剪文件大小。

##### 实例

```
#!/usr/bin/python3

import os

# 打开文件
fd = os.open( "foo.txt", os.O_RDWR|os.O_CREAT )

# 写入字符串
os.write(fd, "This is test - This is test")

# 使用 ftruncate() 方法
os.ftruncate(fd, 10)

# 读取内容
os.lseek(fd, 0, 0)
str = os.read(fd, 100)
print ("读取的字符串是 : ", str)

# 关闭文件
os.close( fd)

print ("关闭文件成功!!")
```

2. truncate() 方法用于从文件的首行首字符开始截断，截断文件为 size 个字符，无 size 表示从当前位置截断；截断之后 V 后面的所有字符被删除，其中 Widnows 系统下的换行代表2个字符大小。 


```
fileObject.truncate( [ size ])
```

##### 参数
- size -- 可选，如果存在则文件截断为 size 字节。

##### 实例

```
#!/usr/bin/python3

fo = open("test.txt", "r+")
print ("文件名: ", fo.name)

line = fo.readline()
print ("读取行: %s" % (line))

fo.truncate()
line = fo.readlines()
print ("读取行: %s" % (line))

# 关闭文件
fo.close()
```


#### 删除
1. os.remove() 方法用于删除指定路径的文件。如果指定的路径是一个目录，将抛出OSError。

```
os.remove(path)             #path -- 要移除的文件路径
```
##### 实例


```
#!/usr/bin/python3

import os, sys

# 列出目录
print ("目录为: %s" %os.listdir(os.getcwd()))

# 移除
os.remove("aa.txt")

# 移除后列出目录
print ("移除后 : %s" %os.listdir(os.getcwd()))
```


#### 复制

1. `shutil.copyfile(src, dst, *, follow_symlinks=True)`  
    复制路径为`src`的文件至路径`dst`，仅复制文件内容。`dst`必须为一个文件路径。  
    如果`src`和`dst`指向同一个路径，抛出`SameFileError`。  
    如`dst`路径没有写入权限，抛出`OSError`。  
    如`dst`是已存在文件的路径，覆盖该文件。  
    返回`dst`
2. `shutil.copy(src, dst, *, follow_symlinks=True)`  
    复制路径为`src`的文件至路径`dst`，复制文件内容和权限。  
    如`dst`是一个文件夹，文件`src`会复制到文件夹内，文件名与`src`的文件名一致。  
    返回`dst`
    
3. `shutil.copy2(src, dst, *, follow_symlinks=True)`  
    复制路径为`src`的文件至路径`dst`，复制文件内容、权限及修改时间等信息。  
    如`dst`是一个文件夹，文件`src`会复制到文件夹内，文件名与`src`的文件名一致。  
    返回`dst`
4.  `shutil.copymode(src, dst)`

    仅拷贝权限。内容、组、用户均不变
5.  `shutil.copystat(src, dst)`

    仅拷贝状态的信息，即文件属性，包括：mode bits, atime, mtime, flags
    

#### 移动
1. `shutil.move(src, dst, copy_function=copy2)`  
    移动路径为`src`的文件至路径`dst`并返回`dst`。  
    如果`dst`是一个文件夹，文件`src`会移动到文件夹内，文件名与`src`的文件名一致。  
    如果`dst`是一个已存在的文件，在Windows系统中，会抛出`OSError`，在Unix系统中，会覆盖原有的文件。  
    对于同一分区中的`src`和`dst`，`shutil.move`会调用`os.rename()`，否则，`src`会复制到`dst`中，然后删除`src`。

#### 重命名
1. `os.rename(src, dst, *, src_dir_fd=None, dst_dir_fd=None)`  
    重命名路径为`src`的文件为`dst`。  
    如`dst`是一个文件夹，抛出`OSError`  
    如果`dst`是一个已存在的文件，在Windows系统中，会抛出`OSError`，在Unix系统中，会覆盖原有的文件。

##### 参数
- src -- 要修改的目录名
- dst -- 修改后的目录名

##### 实例

```
#!/usr/bin/python3

import os, sys

# 列出目录
print ("目录为: %s"%os.listdir(os.getcwd()))

# 重命名
os.rename("test","test2")

print ("重命名成功。")

# 列出重命名后的目录
print ("目录为: %s" %os.listdir(os.getcwd()))
```


### python文件读写可用with open语句
Python引入了with语句来自动帮我们调用close()方法。

在文本模式下，如果未指定编码encoding，则使用的编码取决于系统。

##### 读文件
	
	with open('/path/to/file', 'r',encoding="utf-8") as f:
    print(f.read())
	
调用read()会一次性读取文件的全部内容，如果文件有10G，内存就爆了，所以，要保险起见，可以反复调用read(size)方法，每次最多读取size个字节的内容。另外，调用readline()可以每次读取一行内容，调用readlines()一次读取所有内容并按行返回list。因此，要根据需要决定怎么调用。
	
如果文件很小，read()一次性读取最方便；如果不能确定文件大小，反复调用read(size)比较保险；如果是配置文件，调用readlines()最方便：

	for line in f.readlines():
    print(line.strip()) # 把末尾的'\n'删掉

##### 写文件
	
写文件和读文件是一样的，唯一区别是调用open()函数时，传入标识符'w'或者'wb'表示写文本文件或写二进制文件：

	with open('/Users/michael/test.txt', 'w') as f:
    	f.write('Hello, world!')

要写入特定编码的文本文件，请给open()函数传入encoding参数，将字符串自动转换成指定编码。


对于多个文件的读写，可以写成以下两种方式：

	with open('/home/Desktop/output_measures.txt','r') as f:
    	with open('/home/Desktop/output_measures2.txt','r') as f1:
        	with open('/home/Desktop/output_output_bk.txt','r') as f2:
	　　　　　　　........
	　　　　　　　........
	　　　　　　　........
