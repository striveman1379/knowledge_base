### Python对目录的操作

#### 创建目录
1、os.mkdir() 方法用于以数字权限模式创建目录。默认的模式为 0777 (八进制)。

```
os.mkdir(path[, mode])
```
##### 参数
- path -- 要创建的目录
- mode -- 要为目录设置的权限数字模式

##### 实例

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-

import os

# 创建的目录
path = "/tmp/home/monthly/daily/hourly"

os.mkdir( path, 0755 )

print ("目录已创建")
```

2、os.makedirs() 方法用于递归创建目录。像 mkdir(), 但创建的所有intermediate-level文件夹需要包含子目录。

```
os.makedirs(path, mode=0o777)
```
##### 参数
- path -- 需要递归创建的目录。
- mode -- 权限模式。

##### 实例

```
#!/usr/bin/python3

import os, sys

# 创建的目录
path = "/tmp/home/monthly/daily"

os.makedirs( path, 0o777 );

print ("路径被创建")
```

#### 复制
1. `shutil.copytree(src, dst, symlinks=False, ignore=None, copy_function=copy2, ignore_dangling_symlinks=False)`  
    将`src`递归地复制到`dst`。  
    `dst`不能是已经存在的路径

#### 移动
1. `shutil.move(src, dst, copy_function=copy2)`  
    将`src`移动至`dst`。   
    如果`dst`是一个已存在的文件夹，`src`将移动至`dst`下。  
    如果`dst`已存在，且不是文件夹，Unix系统中`dst`将被覆盖，Windows系统中会抛出异常。  
    如果`dst`不存在，会先创建`dst`不存在的父目录，再将`src`中的内容移动至`dst`下，删除`src`。 

#### 重命名
1、os.rename() 方法用于命名文件或目录，从 src 到 dst,如果dst是一个存在的目录, 将抛出OSError。

```
os.rename(src, dst)
```
##### 参数
- src -- 要修改的目录名
- dst -- 修改后的目录名

##### 实例

```
#!/usr/bin/python3

import os

# 列出目录
print ("目录为: %s"%os.listdir(os.getcwd()))

# 重命名
os.rename("test","test2")

print ("重命名成功。")

# 列出重命名后的目录
print ("目录为: %s" %os.listdir(os.getcwd()))
```
2、os.renames() 方法用于递归重命名目录或文件。类似rename()。

```
os.renames(old, new)
```
##### 参数
- old -- 要重命名的目录
- new --文件或目录的新名字。甚至可以是包含在目录中的文件，或者完整的目录树。

##### 实例
```
#!/usr/bin/python3

import os, sys
print ("当前目录为: %s" %os.getcwd())

# 列出目录
print ("目录为: %s"%os.listdir(os.getcwd()))

# 重命名 "aa1.txt"
os.renames("aa1.txt","aanew.txt")

print ("重命名成功。")

# 列出重命名的文件 "aa1.txt"
print ("目录为: %s" %os.listdir(os.getcwd()))
```

3、replace() 方法把字符串中的 old（旧字符串） 替换成 new(新字符串)，如果指定第三个参数max，则替换不超过 max 次。

```
str.replace(old, new[, max])
```
##### 参数
- old -- 将被替换的子字符串
- new -- 新字符串，用于替换old子字符串
- max -- 可选字符串, 替换不超过 max 次


```
#!/usr/bin/python

str = "this is string example....wow!!! this is really string";
print str.replace("is", "was");
print str.replace("is", "was", 3);

以上实例输出结果如下：
thwas was string example....wow!!! thwas was really string
thwas was string example....wow!!! thwas is really string
```

#### 删除

1、os.rmdir() 方法用于删除指定路径的目录。仅当这文件夹是空的才可以, 否则, 抛出OSError。

```
os.rmdir(path)      #path -- 要删除的目录路径
```

##### 实例

```
#!/usr/bin/python3

import os

# 列出目录
print ("目录为: %s"%os.listdir(os.getcwd()))

# 删除路径
os.rmdir("mydir")

# 列出重命名后的目录
print ("目录为: %s" %os.listdir(os.getcwd()))
```

2、os.removedirs() 方法用于递归删除目录。像rmdir(), 如果子文件夹成功删除, removedirs()才尝试它们的父文件夹,直到抛出一个error(它基本上被忽略,因为它一般意味着你文件夹不为空)。

```
os.removedirs(path)
```
##### 实例

```
#!/usr/bin/python3

import os

# 列出目录
print ("目录为: %s" %os.listdir(os.getcwd()))

# 移除
os.removedirs("/test")

# 列出移除后的目录
print ("移除后目录为:" %os.listdir(os.getcwd()))
```

3、shutil.rmtree(path, ignore_errors=False, onerror=None)  
    递归地删除目录`path`中的全部内容。  
    如`ignore_errors=True`，忽略所有未能删除文件或文件夹的错误。  
    如`ignore_errors=False`，此类错误会传递给`onerror`处理。如`onerror`为`None`，抛出异常。

#### 获取当前目录
os.getcwd() 方法用于返回当前工作目录。


##### 实例

```
#!/usr/bin/python3

import os

# 打印当前目录
print ("当前工作目录 : %s" % os.getcwd())
```

#### 更改工作目录
os.chdir() 方法用于改变当前工作目录到指定的路径。

```
os.chdir(path)      #path -- 要切换到的新路径
```
#### 获取目录列表
1. `os.listdir(path='.')`  
    返回一个`path`中所有文件和文件夹名字的列表（顺序随机，不包括`.`和`..`）  
    注意：列表中的元素之包括文件的名字，不包括路径

2. `os.scandir(path='.')`  
    返回一个`os.DirEntry`对象的迭代器（顺序随机，不包括`.`和`..`），每个`os.DirEntry`对象都是`path`中的一个文件或文件夹。  
    如果需要知道列表中每个元素是否是文件/文件夹，使用`scandir()`比使用`listdir()`会有更好的表现。  
    例如：
    ```python
    with os.scandir(path) as it:
        for entry in it:
            if not entry.name.startswith('.') and entry.is_file():
                print(entry.name)
    ```

3. `os.walk(top, topdown=True, onerror=None, followlinks=False)`  
    通过遍历目录`top`，递归地生成目录下的文件名。对于`top`中的每一个文件夹，它都会生成一个3元组`(dirpath, dirnames, filenames)`。

    `dirpath`是一个字符串形式的目录路径，`dirnames`是`dirpath`中子目录的名称列表（不包括`.`和`..`)，`filenames`为`dirpath`下非目录文件的名称列表。注意，列表中的名称不包含路径部分。要获得`dirpath`中的文件或目录的完整路径，请使用os.path.join(dirpath, name)。
    
    如果参数`topdown=True`，自上而下遍历。如果`topdown=False`，自下而上遍历。
