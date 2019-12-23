## 创建
1. `os.mkdir(path, mode=0o777, *, dir_fd=None)`  
    在`path`处创建一个文件夹，如`path`已存在，抛出` FileExistsError`  
    不能在不存在的父目录中创建子目录`path`
2. `os.mkdirs(name, mode=0o777, exist_ok=False)`  
    如递路径`name`的父目录不存在，归地创建文件夹，直到路径`name`所在的文件夹被创建。  
    如路径`name`已存在，且`exist_ok=False`，会抛出`OSError`。

## 删除
1. `os.rmdir(path, *, dir_fd=None)`
    只能删除空目录`path`
2. `os.removedirs(name)`  
    递归地删除目录。优先删除子目录，删除会在遇到非空目录时停止。
3. `shutil.rmtree(path, ignore_errors=False, onerror=None)`  
    递归地删除目录`path`中的全部内容。  
    如`ignore_errors=True`，忽略所有未能删除文件或文件夹的错误。  
    如`ignore_errors=False`，此类错误会传递给`onerror`处理。如`onerror`为`None`，抛出异常。

## 复制
1. `shutil.copytree(src, dst, symlinks=False, ignore=None, copy_function=copy2, ignore_dangling_symlinks=False)`  
    将`src`递归地复制到`dst`。  
    `dst`不能是已经存在的路径

## 移动
1. `shutil.move(src, dst, copy_function=copy2)`  
    将`src`移动至`dst`。   
    如果`dst`是一个已存在的文件夹，`src`将移动至`dst`下。  
    如果`dst`已存在，且不是文件夹，Unix系统中`dst`将被覆盖，Windows系统中会抛出异常。  
    如果`dst`不存在，会先创建`dst`不存在的父目录，再将`src`中的内容移动至`dst`下，删除`src`。  

## 重命名
1. `os.replace(src, dst, *, src_dir_fd=None, dst_dir_fd=None)`  
    将`src`重命名为`dst`。  
    如果`dst`已存在，是一个文件夹，抛出`OSError`，如果`dst`是一个文件，按照操作者的权限覆盖`dst`。
2. `os.rename(src, dst, *, src_dir_fd=None, dst_dir_fd=None)`  
    将`src`重命名为`dst`。  
    如果`dst`已存在，是一个文件夹，抛出`OSError`。  
    如果`dst`是一个文件，在Unix系统中覆盖`dst`，在Windows系统中抛出`OSError`。
3. `os.renames(old, new)`  
    递归地将`old`重命名为`new`，如果`new`中含有不存在的目录，创建该目录。将`old`下所有内容移动到`new`下后，删除`old`

## 获取当前目录
1. `os.getcwd()`  
    返回当前目录路径

## 更改工作目录
1. `os.chdir(path)`  
    将当前工作目录改为`path`

## 获取目录列表
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
    

## `os.path`模块
低级的字符串操作  
1. 获取绝对路径  
    `os.path.abspath(path)`返回路径`path`的绝对路径
2. 获取文件名  
    `os.path.basename(path)`返回路径`path`的文件名。  
    无论路径中最后一个`\`或`/`后是什么，都会作为basename返回  
    注意：`os.path.basename('/foo/bar/')`返回`''`
3. 获取父目录路径  
    `os.path.dirname(path)`返回路径`path`的父目录。  
    无论路径中最后一个`\`或`/`前是什么，都会作为basename返回
4. 判断路径是否存在  
    `os.path.exists(path)`判断路径`path`是否存在
5. 判断路径是否是绝对路径  
    `os.path.isabs(path)`判断路径`path`是否是绝对路径
6. 判断路径是否对应已存在的文件  
    `os.path.isfile(path)`判断路径`path`是否对应已存在的文件
7. 判断路径是否对应已存在的文件夹  
    `os.path.isdir(path)`判断路径`path`是否对应已存在的文件夹
8. 拼接多个路径  
    `os.path.join(path, *paths)`会使用`os.sep`拼接`path`与`*paths`
9. 切割路径  
    `os.path.split(path)`将路径分为`(head, tail)`，`tail`为文件名，即最后一个`\`或`/`后的内容，`head`为`tail`之前的部分。
10. 切割扩展名  
    `os.path.splitext(path)`将路径分为`(root, ext)`，`ext`为文件扩展名，即最后一个`.`及其后面的部分，`root`为`ext`之前的部分。
	
## `pathlib`模块
高级的路径对象  
Python3中的新特性  
一般使用`pathlib.Path`实例的方法对路径实例进行操作  
1. 创建`Path`实例  
    使用`Path(*pathsegments)`方法创建实例，`*pathsegments`可以是一个或多个字符串，`Path`实例。根据所在系统不同，创建的`Path`实例有不同的类：Windows系统下为`WindowsPath`，Unix系统下为`PosixPath`。例如：
    ```python
    >>> from pathlib import Path
    >>> Path('a/b', 'c')  # Windows下
    WindowsPath('a/b/c')
    >>> Path('a/b/c')  # Unix 下
    PosixPath('a/b/c')
    ```

2. 从`Path`对象中获取路径  
    ```python
    >>> from pathlib import Path
    >>> p = Path('.')
    >>> str(p)
    '.'
    ```
    对于一个`Path`对象`p`，使用`str(p)`方法就可以获取字符串形式的路径
    
3. 拼接多个路径  
    - 使用`/`
        ```python
        >>> from pathlib import Path
        >>> p = Path('a') / Path('b')
        >>> str(p)
        'a/b'
        >>> q = p / 'c/d'
        >>> str(q)
        'a/b/c/d'
        ```
        使用`/`拼接多个路径，得到一个新的`Path`对象。路径可以是字符串也可以是`Path`对象，但不能都是字符串。
    - `Path.joinpath(*other)`  
        ```python
        >>> from pathlib import Path
        >>> p = Path('a').joinpath(Path('b'))
        >>> str(p)
        'a/b'
        >>> q = p.joinpath('c', 'd')
        >>> str(q)
        'a/b/c/d'
        ```
        对于一个`Path`对象`p`，调用它的`joinpath(*other)`方法可以将`*other`参数中的路径拼接到`p`后。`*other`可以是一个或多个字符串也或`Path`对象。
    
4. 获取路径的组成部分
    ```python
    >>> from pathlib import Path
    >>> p = Path('a/b/c/d')
    >>> p.parts
    ('a', 'b', 'c', 'd')
    ```
    对于一个`Path`对象`p`，它的`parts`属性是一个含有路径组成部分的元组
    
5. 获取路径的父目录
    - 获取上一级父目录
        ```python
        >>> from pathlib import Path
        >>> p = Path('a/b/c/d')
        >>> q = p.parent
        >>> str(q)
        'a/b/c'
        ```
        对于一个`Path`对象`p`，它的`parent`属性是它的逻辑父目录，父目录也是一个`Path`对象。根目录`/`和当前目录`.`的父目录是它们自身。
    - 获取父目录序列
        ```python
        >>> from pathlib import Path
        >>> p = Path('a/b/c/d')
        >>> str(p.parents[0])
        'a/b/c'
        >>> str(p.parents[3])
        '.'
        ```
        对于一个`Path`对象`p`，它的`parents`属性是它的逻辑父目录序列，序列的每个值都是一个`Path`对象，索引0是直接的父目录，索引越大越接近根。根目录`/`和当前目录`.`的父目录序列长度为0。

6. 获取文件名
    - 获取完整文件名
        ```python
        >>> from pathlib import Path
        >>> p = Path('a/b/c/d.txt')
        >>> p.name
        'd.txt'
        ```
        对于一个`Path`对象`p`，它的`name`属性是路径的最后一个组成部分。如果`p`是一个目录的路径，则`p.name`是最末文件夹的名字。
    - 获取文件扩展名
        ```python
        >>> from pathlib import Path
        >>> p = Path('a/b/c/d.txt')
        >>> p.suffix
        '.txt'
        ```
        对于一个`Path`对象`p`，它的`suffix`属性是文件的扩展名。如果`p`是一个目录的路径，则`p.suffix`为空字符串。
    - 获取文件多个扩展名
        ```python
        >>> from pathlib import Path
        >>> p = Path('a/b/c/d.wav.enc')
        >>> p.suffixes
        ['.wav', '.enc']
        ```
        对于一个`Path`对象`p`，它的`suffixes`属性是文件的扩展名的列表。
    - 获取文件名，不带扩展名
        ```python
        >>> from pathlib import Path
        >>> p = Path('a/b/c/d.txt')
        >>> p.stem
        'd'
        >>> q = Path('a/b/c/d.wav.enc')
        >>> q.stem
        'd.wav'
        ```
        对于一个`Path`对象`p`，它的`stem`属性是文件名去掉最后一个扩展名的部分。
7. 获取当前路径
    ```python
    >>> from pathlib import Path
    >>> p = Path('./c/d')
    >>> q = p.cwd()
    >>> str(q)
    'a/b'
    ```
    若当前目录为`a/b`，无论`Path`对象`p`的路径是什么，使用`p.cwd()`都会返回当前目录路径
8. 获取绝对路径
    ```python
    >>> from pathlib import Path
    >>> p = Path('./c/d')
    >>> q = p.resolve()
    >>> str(q)
    'a/b/c/d'
    ```
    若当前目录为`a/b`，对于路径为`./c/d`的`Path`对象`p`来说，使用`p.resolve()`即可获取`p`的绝对路径`a/b/c/d`
    
9. 获取目录下所有文件或文件夹
    ```python
    >>> from pathlib import Path
    >>> p = Path('docs')
    >>> for child in p.iterdir(): child
    ...
    PosixPath('docs/conf.py')
    PosixPath('docs/_templates')
    PosixPath('docs/make.bat')
    PosixPath('docs/index.rst')
    PosixPath('docs/_build')
    PosixPath('docs/_static')
    PosixPath('docs/Makefile')
    ```
    对于一个`Path`对象`p`，如果它是一个目录，`p.iterdir()`会返回一个含有目录下所有文件或文件夹名字的列表，使用`for ... in ...`或`next(...)`均可获取其中的内容。注意，与`os.listdir()`和`os.walk()`不同的是，`p.iterdir()`返回的所有文件或文件夹均为`Path`实例，实例中已包含文件或文件夹的路径，无需手动拼接路径。
10. 判断路径...
    1. 是否存在  
        对于一个`Path`对象`p`，`p.exists()`返回`p`中的路径是否存在
    2. 是否是文件夹  
        对于一个`Path`对象`p`，`p.is_dir()`返回`p`中的路径是否是已存在的文件夹
    3. 是否是文件  
        对于一个`Path`对象`p`，`p.is_file()`返回`p`中的路径是否是已存在的文件
    4. 是否是软连接  
        对于一个`Path`对象`p`，`p.is_symlink()`返回`p`中的路径是否是软连接
    5. 是否是绝对路径  
        对于一个`Path`对象`p`，`p.is_absolute()`返回`p`中的路径是否是绝对路径
    6. 是否符合命名规律  
        对于一个`Path`对象`p`，`p.match(pattern)`返回`p`中的路径是否符合命名规律`pattern`
11. 更改路径末尾的文件名，获得新路径
    - 更改文件名
        ```python
        >>> from pathlib import Path
        >>> p = Path('a/b/c/d.txt')
        >>> q = p.with_name('e.txt')
        >>> str(q)
        'a/b/c/e.txt'
        ```
        注意与重命名区分
        
<html>
<!--在这里插入内容-->
</html>


<html>
<!--在这里插入内容-->
</html>


    - 更改扩展名
        ```python
        >>> from pathlib import Path
        >>> p = Path('a/b/c/d.txt')
        >>> q = p.with_suffix('.wav')
        >>> str(q)
        'a/b/c/d.wav'
        ```
        注意与重命名区分
12. 文件/目录操作
    - 创建文件  
        对于一个`Path`对象`p`，`p.touch(mode=0o666, exist_ok=True)`按参数`mode`在`p`所在的路径创建一个文件。如文件已存在，且`exist_ok=False`，抛出`FileExistsError`异常。
    - 创建文件夹  
        对于一个`Path`对象`p`，`p.mkdir(mode=0o777, parents=False, exist_ok=False)`按参数`mode`在`p`所在的路径创建一个文件夹。
        - 如文件夹的父目录不存在，且`parents=False`，抛出`FileNotFoundError`，如`parents=True`，创建父目录。
        - 如文件夹已存在，且`exist_ok=False`，抛出`FileExistsError`异常。
    - 重命名  
        对于一个`Path`对象`p`，`p.rename(target)`会将`p`代表的文件或文件夹重命名为`target`。
        - `target`可以是字符串或`Path`对象
        - 如`target`已存在，Unix系统中会自动覆盖原`target`，Windows系统会抛出异常
    - 删除文件  
        对于一个`Path`对象`p`，`p.unlink()`会删除文件`p`
    - 删除文件夹  
        对于一个`Path`对象`p`，`p.rmdir()`会删除文件夹`p`，文件夹必须为空。
    - 打开文件  
        对于一个`Path`对象`p`，`p.open(mode='r', buffering=-1, encoding=None, errors=None, newline=None)`的使用方式与`io.open()`一致
    - 读文件  
        对于一个`Path`对象`p`，`p.read_text(encoding=None, errors=None)`会返回文件`p`的全部内容
    - 写文件  
        对于一个`Path`对象`p`，`p.write_text(data, encoding=None, errors=None)`会先清空文件`p`，再写入`data`

## `os.path` 与 `pathlib` 方法对比
|`os` and `os.path`|	`pathlib`|
|----|----|
|os.path.abspath()|	Path.resolve()|
|os.getcwd()	|Path.cwd()|
|os.path.exists()|	Path.exists()|
|os.path.expanduser()	|Path.expanduser() and Path.home()|
|os.path.isdir()|	Path.is_dir()|
|os.path.isfile()|	Path.is_file()|
|os.path.islink()|	Path.is_symlink()|
|os.stat()	|Path.stat(), Path.owner(), Path.group()|
|os.path.isabs()|	Path.is_absolute()|
|os.path.join()|	Path.joinpath()|
|os.path.basename()	|Path.name|
|os.path.dirname()|	Path.parent|
|os.path.splitext()	|Path.suffix|
