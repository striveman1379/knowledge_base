## 涉及模块
- `os`模块 — 操作系统的各种接口<!--16.1. os — Miscellaneous operating system interfaces  -->  
    源代码: [Lib/os.py](https://github.com/python/cpython/tree/3.7/Lib/os.py)
    
    这个模快提供了一个便携的方式去使用操作系统的相关功能。如果你只是想要读取或写入文件请参阅[`open()`](https://docs.python.org/3.7/library/functions.html#open)，如果你想要操作路径，请参阅[`os.path`](https://docs.python.org/3.7/library/os.path.html#module-os.path)模块，如果你想要在命令行上读取所有文件中的所有行，请参阅[`fileinput`](https://docs.python.org/3.7/library/fileinput.html#module-fileinput)模块。需要创建临时文件和目录，请参阅[`tempfile`](https://docs.python.org/3.7/library/tempfile.html#module-tempfile)模块。需要高级的文件和目录处理请参见[`shutil`](https://docs.python.org/3.7/library/shutil.html#module-shutil)模块。
    <!--This module provides a portable way of using operating system dependent functionality. If you just want to read or write a file see open(), if you want to manipulate paths, see the os.path module, and if you want to read all the lines in all the files on the command line see the fileinput module. For creating temporary files and directories see the tempfile module, and for high-level file and directory handling see the shutil module.-->
    
    使用这些功能的注意事项：<!--Notes on the availability of these functions:-->
    
    - Python中内建的操作系统相关的模块，只要有相同的功能，就会使用相同的接口；例如，函数`os.stat(path)`以相同的格式返回关于路径的统计信息。<!--The design of all built-in operating system dependent modules of Python is such that as long as the same functionality is available, it uses the same interface; for example, the function os.stat(path) returns stat information about path in the same format (which happens to have originated with the POSIX interface).-->
    - 通过`os`模块可以获得特定操作系统特有的扩展，但使用它们会影响可移植性。<!--Extensions peculiar to a particular operating system are also available through the os module, but using them is of course a threat to portability.
      -->
    - 接受字节格式的路径或文件名的函数返回字节对象，接受字符串格式的路径或文件名的函数返回字符串对象。<!--All functions accepting path or file names accept both bytes and string objects, and result in an object of the same type, if a path or file name is returned.-->
- `io`模块 — 使用流的核心工具<!--16.2. io — Core tools for working with streams  -->  
    源代码: [Lib/io.py](https://github.com/python/cpython/tree/3.7/Lib/io.py)

    `io`模块提供了Python处理各种类型I/O的主要工具。有三种主要类型的I/O：文本I/O`text I/O`，二进制I/O`binary I/O`和原始I/O`raw I/O`。这些是通用类别，并且它们中的每一个可以使用各种后备存储。属于这些类别中的任何一个的具体对象称为文件对象`file object`。其他常见术语是流`stream`和文件状对象`file-like object`。<!--The io module provides Python’s main facilities for dealing with various types of I/O. There are three main types of I/O: text I/O, binary I/O and raw I/O. These are generic categories, and various backing stores can be used for each of them. A concrete object belonging to any of these categories is called a file object. Other common terms are stream and file-like object.-->
    
    独立于其类别，每个流的具体对象也将具有各种能力：它可以是只读的，只写的或读写的。它还可以允许任意随机存取（寻找向前或向后到任何位置），或只允许顺序存取（例如在套接字或管道的情况下）。<!--Independently of its category, each concrete stream object will also have various capabilities: it can be read-only, write-only, or read-write. It can also allow arbitrary random access (seeking forwards or backwards to any location), or only sequential access (for example in the case of a socket or pipe).-->
    
    所有流都对传入的数据类型敏感。例如，给二进制流的`write()`方法一个`str`对象将引起`TypeError`。同理，给文本流的`write()`方法一个`bytes`对象也会引起`TypeError`。<!--All streams are careful about the type of data you give to them. For example giving a str object to the write() method of a binary stream will raise a TypeError. So will giving a bytes object to the write() method of a text stream.-->

- `shutil`模块 — 高级文件操作<!--11.10. shutil — High-level file operations  -->  
    源代码: [Lib/shutil.py](https://github.com/python/cpython/tree/3.7/Lib/shutil.py)
    
    `shutil`模块提供了对文件和文件夹的一些高级操作。特别地，提供了支持文件复制和移除的功能。有关单个文件的操作，另请参见`os`模块。
## 创建文件
1. `os.mknod(path, mode=0o600, device=0, *, dir_fd=None)`  
    创建路径为`path`的文件系统节点（文件，设备专用文件或命名管道）。模式`mode`指定了节点的权限和类型，类型包括`stat`中的`stat.S_IFREG`，`stat.S_IFCHR`，`stat.S_IFBLK`，和`stat.S_IFIFO`。
    <!--Create a filesystem node (file, device special file or named pipe) named path. mode specifies both the permissions to use and the type of node to be created, being combined (bitwise OR) with one of `stat.S_IFREG`, `stat.S_IFCHR`, `stat.S_IFBLK`, and `stat.S_IFIFO` (those constants are available in [stat](https://docs.python.org/3.7/library/stat.html#module-stat)). For `stat.S_IFCHR` and `stat.S_IFBLK`, device defines the newly created device special file (probably using [os.makedev()](https://docs.python.org/3.7/library/os.html#os.makedev)), otherwise it is ignored.    This function can also support paths relative to directory descriptors.-->
    
    仅限Unix系统使用
2. `os.open(path, flags, mode=0o777, *, dir_fd=None)`  
    此方法主要用于打开一个文件，使用`flag=os.O_CREAT`即可创建文件。  
    如文件已存在，且`flag=os.O_CREAT|os.O_EXCL`，抛出异常。  
    如只需创建文件，不需要编辑，创建后应使用`os.close(fd)`方法关闭文件，`fd`为`os.open()`返回的文件描述符。  
    详见下方**打开**中的`os.open()`

3. `open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)`  
    此方法主要用于打开一个文件`file`，如文件不存在，创建该文件。  
    如只需创建文件，不需要编辑，创建后应使用`FileObject.close()`方法关闭文件，`FileObject`为`open()`返回的文件对象。  
    详见下方**打开**中的`open()`

## 打开
1. `os.open(path, flags, mode=0o777, *, dir_fd=None)`  
    打开文件`path`并根据标志设置各种特征值`flags`，并根据模式`mode`设置其模式。  
    返回新打开的文件的文件描述符。
    <!--Open the file path and set various flags according to flags and possibly its mode according to mode. When computing mode, the current umask value is first masked out. Return the file descriptor for the newly opened file. The new file descriptor is non-inheritable.-->

    <!--For a description of the flag and mode values, see the C run-time documentation; flag constants (like O_RDONLY and O_WRONLY) are defined in the os module. In particular, on Windows adding O_BINARY is needed to open files in binary mode.-->
    
    <!--This function can support paths relative to directory descriptors with the dir_fd parameter.-->
    
    注意，这个方法适用于低级别的 I / O。通常，打开文件会使用Python内置的方法`open()`（见下一个方法）。  
    <!--Note This function is intended for low-level I/O. For normal usage, use the built-in function open(), which returns a file object with read() and write() methods (and many more). To wrap a file descriptor in a file object, use fdopen().-->
    
    以下为`flags`参数的可选项，如需选用多个参数，可将参数以位或进行组合<!--The following constants are options for the flags parameter to the open() function. They can be combined using the bitwise OR operator |. Some of them are not available on all platforms. For descriptions of their availability and use, consult the open(2) manual page on Unix or the MSDN on Windows.-->

    os.O_RDONLY  只读  
    os.O_WRONLY  只写   
    os.O_RDWR  读写  
    os.O_APPEND  追加  
    os.O_CREAT  不存在则创建  
    os.O_EXCL  如文件已存在，报错  
    os.O_TRUNC   写前清空文件


2. `open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)`  
    打开路径`file`处的文件，并返回一个文件对象，如不能打开文件，抛出`OSError`异常。
    <!--Open file and return a corresponding file object. If the file cannot be opened, an OSError is raised.-->
    
    路径`file`可以是一个字符串，也可以是一个有效的文件描述符。如果提供的是文件描述符，在默认情况下，文件对象关闭后，文件描述符也将关闭。而如果`closefd=False`，文件对象关闭后，文件描述符不关闭。  
    
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
    
	缓冲区`buffering`为一个可选的整型参数。0为关闭缓冲（只对二进制模式生效，文本模式会抛出`ValueError`），1为行缓冲（只对文本模式生效），大于1的数字为缓冲区的字节数，不指定`buffering`时，`buffering=-1`，使用默认缓冲策略，缓冲区大小为`io.DEFAULT_BUFFER_SIZE`，一般为4096或8192字节。  
    
    编码`encoding`为解码，编码文件时使用的编码。只有文件在文本模式打开时才可以使用此参数。默认使用的编码为系统编码，`locale.getpreferredencoding()`。
        
    换行符`newline`控制文件的换行模式（只对文本模式生效）。可选值为`None`，`''`，`''\n'`，`''\r'`，和`''\r\n'`（其他值会引起`ValueError`）。它的工作方式如下：  
    - 读取时，如果`newline=None`，文本中所有`\n`，`\r`，和`\r\n`将被自动转化为`\n`。如果`newline=''`，所有`\n`，`\r`，和`\r\n`保持原样，这三个都作为换行符。如果`newline`是其他可选值，比如`\n`，则只用`\n`作为换行符。
    - 写入时，如果`newline=None`，则写入的任何`\n`字符都将转换为系统默认换行符 `os.linesep`。如果`newline`是`''`或`\n`，则不会进行转换。如果`newline`是其他可选值，写入的`\n`字符都将转换为给定换行符。
        
    `open()`返回一个文件对象(file object)，是`io`模块中继承了`IOBase`类的实例，这个文件对象有用于读写的方法，且为一个上下文管理器（Context Manager），因此支持使用`with`语句打开文件。
    
    <!--The type of file object returned by the open() function depends on the mode. When open() is used to open a file in a text mode ('w', 'r', 'wt', 'rt', etc.), it returns a subclass of io.TextIOBase (specifically io.TextIOWrapper). When used to open a file in a binary mode with buffering, the returned class is a subclass of io.BufferedIOBase. The exact class varies: in read binary mode, it returns an io.BufferedReader; in write binary and append binary modes, it returns an io.BufferedWriter, and in read/write mode, it returns an io.BufferedRandom. When buffering is disabled, the raw stream, a subclass of io.RawIOBase, io.FileIO, is returned.
    
    See also the file handling modules, such as, fileinput, io (where open() is declared), os, os.path, tempfile, and shutil.
    
    io模块中的IOBase (and its subclasses) supports the iterator protocol, meaning that an IOBase object can be iterated over yielding the lines in a stream(用更少地中间变量写流式代码，因此更节省内存和CPU). Lines are defined slightly differently depending on whether the stream is a binary stream (yielding bytes), or a text stream (yielding character strings). See readline() below.
    
    IOBase is also a context manager and therefore supports the with statement. In this example, file is closed after the with statement’s suite is finished—even if an exception occurs:-->

3. `os.fdopen(fd, *args, **kwargs)`  
    根据`fd`打开并返回一个文件对象


## 关闭
1. `os.close(fd)`  
    关闭文件描述符`fd`
2. `file.close()`  
    将缓冲区中的内容写入文件，并关闭由`open()`返回的文件对象`file`。如果文件已经被关闭，则此方法不会产生任何影响。一旦文件关闭，对文件的任何操作都会引起`ValurError`  
    <!--Flush and close this stream. This method has no effect if the file is already closed. Once the file is closed, any operation on the file (e.g. reading or writing) will raise a ValueError.
    As a convenience, it is allowed to call this method more than once; only the first call, however, will have an effect.-->

## 写入
1. `os.write(fd, str)`  
    将字节字符串`str`写入文件描述符`fd`，并返回写入的字节数。
2. `file.write(s)`  
    将字符串`s`写入通过`open()`打开的文件对象`file`中，并返回写入的字符数。

3. `file.writelines(lines)`  
    将列表`lines`中的所有项写入通过`open()`打开的文件对象`file`中。注意：写入时，不会在每一项后额外添加换行符，因此，列表`lines`中的每一项的末尾通常有一个换行符。

## 读取
1. `os.read(fd, n)`  
    从文件描述符`fd`中读取`n`个字节，或读到EOF（如可读取字节数小于`n`）。返回读取的字节字符串。如果`fd`已读到文件末尾，继续读取会返回空字符串。

2. `file.read(size=-1)`  
    从由`open()`返回的文件对象`file`中读取最多`size`个字符，并将读取内容作为一个字符串返回。如果`size`为负或`None`，读取到EOF为止。

3. `file.readline(size=-1)`  
    从由`open()`返回的文件对象`file`中读取一行。并将读取内容作为一个字符串返回。如果已读到EOF，继续读取会返回空字符串。  
    如指定了`size`，最多读取`size`个字符。
    
    换行符见`open()`的参数`newline`，按照`newline`指定的换行符，一行的末尾包含一个换行符

4. `file.readlines(hint=-1)`  
    从由`open()`返回的文件对象`file`中读取所有行。并返回由行组成的列表。  
    如指定了`hint`，先按行读取，在读取的字符数超过`hint`后，停止读取，返回列表。
    
    __注意__：想要读取文件`file`的全部行，可以使用`for line in file:`语法，而无需使用`readlines()`方法。`for line in file:`使用了迭代器，因此更加节省CPU和内存。
5. `for line in file:`  
    对于一个由`open()`返回的文件对象`file`，读取其中的所有行并打印可以使用：
    ```python
    with open(file_path) as file:
        for line in file:
            print(line)
    ```
    鉴于每行末尾有一个换行符，`print`也会换行，以上代码会造成如下效果：
    ```
    line 1
    
    line 2
    
    line 3
    
    ...
    ```
    

## 设置光标位置
1. `os.lseek(fd, pos, how)`  
    根据`how`设置`fd`中的光标位置`pos`。如`how=0`或`how=SEEK_SET`，`pos`是相对于文件开头的位置；如`how=1`或`how=SEEK_CUR`，`pos`是相对于当前位置的位置；如`how=2`或`how=SEEK_END`，`pos`是相对于文件末尾的位置。返回光标相对于文件开头的新位置。
2. `file.seek(offset[, whence])`  
    重置由`open()`返回的文件对象`file`的光标位置。光标位置由`offset`和`whence`共同决定，模式同上。`whence`默认为`SEEK_SET`。返回光标相对于文件开头的新位置。

## 清空
1. `os.truncate(path, length)`  
    截取路径为`path`的文件，使它的长度最多为`length`（从头截取，剩余部分删除）。`path`可以是一个文件描述符。
    
    使用此方法无需打开文件
2. `file.truncate(size=None)`  
    截取由`open()`返回的文件对象`file`，`file`必须可写。如`size=None`，截取至当前光标位置，如`size`为正整数，将文件对象`file`截断或延长至`size`大小。如需延长，一般使用`\00`字节填充文件。返回文件的新大小。  
    此方法不会改变光标的位置。

## 复制
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


## 删除
1. `os.remove(path, *, dir_fd=None)`  
    删除路径为`path`的文件。  
    如果`path`是一个文件夹，抛出`OSError`  
    在Windows中，删除一个正在使用的文件会引起异常，而Unix会在文件使用结束后将其删除。

## 移动
1. `shutil.move(src, dst, copy_function=copy2)`  
    移动路径为`src`的文件至路径`dst`并返回`dst`。  
    如果`dst`是一个文件夹，文件`src`会移动到文件夹内，文件名与`src`的文件名一致。  
    如果`dst`是一个已存在的文件，在Windows系统中，会抛出`OSError`，在Unix系统中，会覆盖原有的文件。  
    对于同一分区中的`src`和`dst`，`shutil.move`会调用`os.rename()`，否则，`src`会复制到`dst`中，然后删除`src`。


## 重命名
1. `os.rename(src, dst, *, src_dir_fd=None, dst_dir_fd=None)`  
    重命名路径为`src`的文件为`dst`。  
    如`dst`是一个文件夹，抛出`OSError`  
    如果`dst`是一个已存在的文件，在Windows系统中，会抛出`OSError`，在Unix系统中，会覆盖原有的文件。

## 补充
### python文件读写可用with open语句
Python引入了with语句来自动帮我们调用close()方法。

在文本模式下，如果未指定编码encoding，则使用的编码取决于系统。

#####	读文件
	
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







### 疑问点
什么是文件描述符?


在linux下一切皆文件,文件描述符是内核为了高效的管理已经被打开的文件所创建的索引,它是一个非负整数,用于指代被打开的文件,所有执行I/O操作的系统调用都是通过文件描述符完成的。


在linux中,进程是通过文件描述符(file descriptors 简称fd)来访问文件的,文件描述符实际上是一个整数。在程序刚启动的时候,默认有三个文件描述符,分别是:0(代表标准输入),1(代表标准输出),2(代表标准错误)。再打开一个新的文件的话,它的文件描述符就是3。


POSIX标准规定,每次打开的文件时(含socket)必须使用当前进程中最小可用的文件描述符号码。



Linux中的文件描述符与打开文件之间的关系
[https://blog.csdn.net/cywosp/article/details/38965239](https://blog.csdn.net/cywosp/article/details/38965239)
