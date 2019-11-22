### python调用系统命令
#### 1. os.system
- `system`函数可以将字符串转化成命令执行。其原理是每一条system函数执行时，其会创建一个子进程在系统上执行命令行，子进程的执行结果无法影响主进程。  
- 上述原理会导致当需要执行多条命令行的时候可能得不到预期的结果，例如：
    ```
    >>> import os
    >>> os.system('cd /usr/local')
    >>> os.system('touch aaa.txt')
    ```
    会在当前目录下创建aaa.txt，而不会在/usr/local下创建。
- 想要连续执行多条命令需要在同一子进程中执行可以使用`&&`或`;`连接多个命令：
    ```
    >>> import os
    >>> os.system('cd /usr/local && touch aaa.txt')
    >>> os.system('cd /usr/local ; touch bbb.txt')
    ```
- `system`只返回子进程的执行结果（0-成功，非0-失败），不会返回命令的返回值。且会将命令输出在终端上的内容打印出来，例如：
    ```
    >>> import os
    >>> val = os.system('tou aaa.txt') # 没有tou命令
    sh: 1: tou: not found
    >>> print(val)
    32512
    ```
    返回了32512，而使用`echo $?`查看`tou aaa.txt`命令在终端的返回值，为127  
    
#### 2. os.popen
- `popen`函数创建一个管道，并创建一个子进程执行命令，该管道用于父子进程间通信。如不指定`popen`方法的`mode`参数，`mode`默认为`'r'`，即读取模式，管道中的内容为命令的输出。  
- 使用`read()`方法可以读取管道中的内容，例如：
    ```
    >>> import os
    >>> pipe = os.popen("ls")
    >>> print(pipe.read())
    aaa.txt
    bbb.txt
    ```
    打印了`ls`命令的输出
- 使用`popen`无法获取命令的返回值。

#### 3. subprocess模块 <font color=#ff0000>（推荐）</font>
- `subprocess`模块可以创建一个子进程，连接到它的输入/输出/错误管道，等待其执行完成，并获取它的返回值。<font color=#ff0000>该模块旨在取代旧有的模块和功能：os.system，os.popen 和 os.spawn*</font>
- 主要方法：
    1. `run(args, *, stdin=None, input=None, stdout=None, stderr=None, shell=False, cwd=None, timeout=None, check=False, encoding=None, errors=None, text=None, env=None)`<font color=#ff0000>（推荐）</font>
        - 适用于Python3.5及以上版本  
        - `args`: 必要参数，字符串或字符串列表形式的命令。
            - 如命令中不含空格可以将命令以字符串的方式传入
                ```
                >>> obj = subprocess.run("ls")
                aaa.txt  bbb.txt
                ```
            - 如命令中含有空格应以参数序列的方式传入
                ```
                >>> obj = subprocess.run(["ls", "-la"])
                total 20
                drwxr-xr-x  3 root root 12288 Aug  1 22:40 .
                drwxr-xr-x 27 root root  4096 Aug  1 21:00 ..
                -rw-rw-r--  1 root root     0 Aug  1 22:40 aaa.txt
                -rw-rw-r--  1 root root     0 Aug  1 22:40 bbb.txt
                ```
            - 或传入含有空格的字符串的同时使用`shell=True`  
                ```
                >>> obj = subprocess.run("ls -la", shell=True)
                total 20
                drwxr-xr-x  3 root root 12288 Aug  1 22:40 .
                drwxr-xr-x 27 root root  4096 Aug  1 21:00 ..
                -rw-rw-r--  1 root root     0 Aug  1 22:40 aaa.txt
                -rw-rw-r--  1 root root     0 Aug  1 22:40 bbb.txt
    
                ```
        - `stdin`: 标准输入的文件句柄，默认为None，可选`subprocess.PIPE`，`subprocess.DEVNULL`，已存在的文件描述符，或已存在的文件对象。注意不能与`input`参数同时使用。
        - `input`: 标准输入的内容，即传递给`args`的内容，默认为None。
            - 如`text=False`且`encoding=None`，`input`应以字节序列的形式传入。例如
                ```
                >>> obj = subprocess.run("python", input=b"print(1)\nprint(2)")
                1
                2
                ```
            - 如`text=True`，`input`可以为字符串。例如：
                ```
                >>> obj = subprocess.run("python", input="print(1)\nprint(2)", text=True)
                1
                2
                ```
            - 如指定了`encoding`，`input`可以为字符串。例如：
                ```
                >>> obj = subprocess.run("python", input="print(1)\nprint(2)", encoding="utf-8")
                1
                2
                ```
        - `stdout`: 标准输出的文件句柄，默认为None，可选`subprocess.PIPE`，`subprocess.DEVNULL`，已存在的文件描述符，或已存在的文件对象。
            - `stdout=None`会将命令的输出输出到屏幕
            ```
            >>> obj = subprocess.run("ls")
            aaa.txt  bbb.txt
            ```
            - `stdout=subprocess.PIPE`会将命令的输出写入`run`函数的返回实例的`stdout`属性中而不会输出到终端
            ```
            >>> obj = subprocess.run("ls", stdout=subprocess.PIPE)
            >>> obj
            CompletedProcess(args='ls', returncode=0, stdout=b'aaa.txt\nbbb.txt\n')
            >>> obj.stdout
            b'aaa.txt\nbbb.txt\n'
            ```
        - `stderr`: 标准错误的文件句柄，默认为None，可选`subprocess.PIPE`，`subprocess.DEVNULL`，`subprocess.STDOUT`，已存在的文件描述符，或已存在的文件对象。
            - `stderr`的用法与`stdout`类似
            - 值得注意的是`stderr=subprocess.STDOUT`会将错误输出到标注输出的文件句柄，返回实例的`stderr`属性为None
        - `shell`: 默认为`False`，python会解析`args`命令，如`shell=True`则不解析，直接将命令传入shell
        - `cwd`: 默认为None，即当前目录，否则会在指定的目录执行命令
        - `timeout`: 子进程运行指定时间后会被终止
        - `check`: 如果`check=True`且子进程的退出码不为零，会引发一个`CalledProcessError`异常，如`check=False`则不会。
        - `encoding`: 将`stdin`，`stdout`，`stderr`以指定编码打开，如未指定`encoding`，`errors`，`text`中的任意一个，文件句柄将以二进制打开
        - `errors`: 作用与`encoding`相同
        - `text`: 作用与`encoding`类似，如`text=True`则将`stdin`，`stdout`，`stderr`以字符串形式打开，否则以二进制打开
        - `env`: 子进程的环境变量。如不指定`env`，子进程将继承父进程的环境变量
        - 返回：`CompletedProcess`实例，例如：
            ```python
            >>> obj = subprocess.run("ls")
            aaa.txt  bbb.txt
            >>> obj
            CompletedProcess(args='ls', returncode=0)
            ```
            - `CompletedProcess`实例含有`args`, `returncode`, `stdout`和`stderr`四个属性
                - `args`与`run`方法中的`args`参数一致
                - `returncode`是执行命令的子进程的退出状态码（0-成功，非0-失败）
                - `stdout`：从子进程捕获的stdout。这通常是一个字节序列，如果`run()`函数被调用时指定`universal_newlines=True`，`text=True`或`encoding`，则该属性值是一个字符串。如果`run()`函数被调用时指定`stderr=subprocess.STDOUT`，那么`stdout`和`stderr`将会被整合到这一个属性中，且`stderr`将会为None
                - `stderr`：从子进程捕获的stderr。它的值与`stdout`一样，是一个字节序列或一个字符串。如果`stderr`没有被捕获的话，它的值就为None
    2. 类`Popen`  
        - `Popen`会创建一个子进程执行命令
        - `subprocess.run`函数是基于`Popen`的封装，`Popen`的用法与`run`函数类似。但是`Popen`不会等待命令执行结束。
        - `Popen`的构造函数：
        `Popen(args, bufsize=-1, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=True, shell=False, cwd=None, env=None, universal_newlines=False, startupinfo=None, creationflags=0, restore_signals=True, start_new_session=False, pass_fds=(), *, encoding=None, errors=None, text=None)`
            - `args`，`stdin`，`stdout`，`stderr`，`shell`，`cwd`，`env`，`universal_newlines`，`encoding`，`errors`，`text`参数的用法与`run`函数中的一致
            - `bufsize`：指定缓冲策略，0表示不缓冲，1表示行缓冲，其他大于1的数字表示缓冲区大小，负数表示使用系统默认缓冲策略
            - `executable`：指定执行命令的程序，`args`将作为该程序的参数
            - `preexec_fn`：如果将preexec_fn设置为可调用对象，则该对象将在子进程执行前被调用。（仅适用于Unix）
            - `close_fds`：在Unix上，如果`close_fds=True`，则除了0,1和2之外的所有文件描述符都将会在子进程执行之前被关闭。在Windows上，如果`close_fds=True`，那么子进程将不会继承任何句柄。注意，在Windows上，不能将`close_fds`设置为true的同时通过设置`stdin`，`stdout`或`stderr`来重定向标准句柄。
            - `startupinfo`和`creationflags`：仅适用于Windows，用于设置子进程的一些属性，如主窗口的外观，进程优先级等。
            - `restore_signals`：在Unix上，如果`restore_signals=True`，将python的`SIG_IGN`存储到`SIG_DFL`中
            - `start_new_session`：在Unix上，如果`start_new_session=Ture`，在子进程执行之前会先执行系统命令`setsid()`来建立一个对话期
            - `pass_fds`：在Unix上，为子进程提供与父进程共享的文件描述符。使用`pass_fds`的前提是`close_fds=True`
        - `Popen`实例可调用的方法:
            - `Popen.poll()`：用于检查子进程（命令）是否已经执行结束，没结束返回None，结束后返回状态码。
            - `Popen.wait(timeout=None)`：等待子进程结束，并返回状态码。如果在`timeout`指定的时间之后进程还没有结束，将会抛出一个`TimeoutExpired`异常。
            - `Popen.communicate(input=None, timeout=None)`：与子进程进行交互，将数据发送到`stdin`，从`stdout`和`stderr`中读取数据，直到达到文件结尾。
            - `Popen.send_signal(signal)`：给子进程发送信号
            - `Popen.terminate()`：停止子进程
            - `Popen.kill()`：杀死子进程
- 其他方法（适用于3.5以前的版本）
    1. `subprocess.call(args, *, stdin=None, stdout=None, stderr=None, shell=False, cwd=None, timeout=None)`
        - 执行`args`命令，等待命令执行完成，并返回命令的返回码。
        - 参数的使用方式与`run`函数中的类似
    2. `subprocess.check_call(args, *, stdin=None, stdout=None, stderr=None, shell=False, cwd=None, timeout=None)`
        - 执行`args`命令，等待命令执行完成，并返回命令的返回码。如返回码不为零，抛出一个`CalledProcessError`异常
        - 参数的使用方式与`run`函数中的类似
    3. `subprocess.check_output(args, *, stdin=None, stderr=None, shell=False, cwd=None, encoding=None, errors=None, universal_newlines=False, timeout=None)`
        - 执行`args`命令，等待命令执行完成，并返回命令的输出。如命令的返回码不为零，抛出一个`CalledProcessError`异常
        - 参数的使用方式与`run`函数中的类似


#### 参考
subprocess官方文档：https://docs.python.org/3.6/library/subprocess.html  
Python之系统交互：https://www.cnblogs.com/sunshine-1/p/7290468.html  
腾讯云社区subprocess文档：https://cloud.tencent.com/developer/section/1370097
