### python中ftp的基本操作
使用ftplib模块中的FTP对象连接远程服务器并执行相关操作
1. **FTP的构造函数**  
    `ftplib.FTP(host='', user='', passwd='', timeout=None)`
    - 参数
        1. `host`：ftp主机地址
        2. `user`：用户名
        3. `passwd`：密码
        4. `timeout`：可选，指定时间后断开与ftp服务器的连接
    - 返回：一个FTP对象的实例
    - 例如：
        ```python
        from ftplib import FTP
        # 连接FTP服务器
        ftp = FTP(host="103.38.225.154", user="fromPhone", passwd="fromPhone")
        ftp.quit()
        ```
        值得注意的是，在使用完ftp服务器后应断开连接，即`ftp.quit()`。因此，FTP也支持`with ... as ... :`的用法，无需手动断开连接。例如：
        ```python
        from ftplib import FTP
        # 连接FTP服务器
        with FTP(host="103.38.225.154", user="fromPhone", passwd="fromPhone") as ftp:
            pass
        ```
2. **上传文件**  
    使用`FTP.storbinary(cmd, fp, blocksize=8192, callback=None, rest=None)`函数，将本地文件以二进制打开并上传。
    - 参数
        1. `cmd`：储存指令，格式为`"STOR "+filepath`，`filepath`应为ftp中文件的目标路径。`STOR`是`FTP`模块使用的储存指令的关键字。注意：`STOR`后面必须加空格
        2. `fp`：一个以二进制打开的只读文件对象，该文件为需要上传的文件。例如`open("filepath", "rb")`
        3. `blocksize`：缓冲区大小，默认为8192
        4. `callback`：上传完成后对ftp中的文件执行的函数句柄
    - 例如：
        ```python
        from ftplib import FTP
        with FTP(host="103.38.225.154", user="fromPhone", passwd="fromPhone") as ftp:
            ftp.storbinary("STOR"+"/ftpupload/demo.txt", open("/home/root/Desktop/demo.txt", "rb"))
        ```
        将本地桌面上的`demo.txt`上传到ftp服务器的`/ftpupload`下。  
        上传时，目标文件所在目录必须存在，如不存在请先创建，否则会报错（`ftplib.error_perm: 550 Filename invalid`）。
3. **下载文件**  
    使用`retrbinary(cmd, callback, blocksize=8192, rest=None)`函数，获取ftp中的文件并利用`callback`将文件写入本地文件中，完成下载。
    - 参数
        1. `cmd`：获取指令，格式为`"RETR "+filepath`，`filepath`应为ftp中文件的路径。`RETR`是`FTP`模块使用的获取指令的关键字。注意：`RETR`后面必须加空格
        2. `callback`：获取ftp中的文件后对其执行的函数句柄，想要下载文件，应将`callback`设置为一个二进制可写入文件对象的`write`方法。例如`open("filepath", "wb").write`
        3. `blocksize`：缓冲区大小，默认为8192
        4. `rest`：对获取的文件的字节偏移量，从第`rest+1`个开始获取
    - 例如
        ```python
        from ftplib import FTP
        with FTP(host="103.38.225.154", user="fromPhone", passwd="fromPhone") as ftp:
            ftp.retrbinary("RETR "+"/ftpupload/demo.txt", open("/home/root/Desktop/demo.txt", "wb").write)
        ```
        将ftp服务器中`/ftpupload`下的`demo.txt`下载到桌面上
4. **创建目录**  
    使用`FTP.mkd(pathname)`创建指定路径的目录
5. **设置当前路径**  
    使用`FTP.cwd(pathname)`设置FTP当前操作的路径
6. **删除文件**  
    使用`FTP.delete(filepath)`删除指定文件，如成功执行，返回删除结果
7. **删除目录**  
    使用`FTP.rmd(dirpath)`删除指定目录  
    注意：删除目录时应保证目录为空，否则会报错（`ftplib.error_perm: 550 Directory not empty.`）
8. **编码**  
    `ftplib`的默认编码为`latin-1`，根据ftp服务器的编码，应在打开FTP后设置其编码。例如：`ftp.encoding='UTF-8'`
