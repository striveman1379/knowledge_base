### python中的压缩、解压
#### 压缩
1. tar、tgz、tar.gz、tar.bz2格式  
    使用`tarfile`模块，可以生成`.tar`, `.tgz`, `.tar.gz`, `.tar.bz2`等格式的压缩包  
    - 生成`test.tar`
        ```python
        import tarfile, os
        
        # 生成./test.tar包
        with tarfile.open("./test.tar", "w") as test_tar_file:
            # 压缩./test文件夹
            for f in os.listdir("./test"):
                f_path = os.path.join("./test", f)
                test_tar_file.add(f_path)
        ```
        `TarFile.add()`方法会递归地压缩所给的文件或文件夹。因此，想要压缩文件夹`./test`只需将`./test`内的文件路径作为参数传入即可。此处没有直接`test_tar_file.add("./test")`而是遍历添加`test`中的文件的原因是直接添加test文件夹所生成的压缩包会有2层test目录。
    - 生成`test.tgz`
        ```python
        import tarfile, os
        
        # 生成./test.tgz压缩包
        with tarfile.open("./test.tgz", "w:gz") as test_tgz_file:
            # 压缩./test文件夹
            for f in os.listdir("./test"):
                f_path = os.path.join("./test", f)
                test_tgz_file.add(f_path)
        ```
        与上面的方法唯一的区别在于打开tgz压缩包时使用了`mode="w:gz"`，而打开tar包时使用的是`mode="w"`。`mode`参数的格式为`filemode[:compression]`，即`:`前为打开文件的模式，`:`后为压缩/解压的格式，tar包仅打包不压缩，所以没有压缩格式，tgz包的压缩格式为`gz`。
    - 生成`test.tar.gz`
        ```python
        import tarfile, os
        
        # 生成./test.tar.gz压缩包
        with tarfile.open("./test.tar.gz", "w:gz") as test_tgz_file:
            # 压缩./test文件夹
            for f in os.listdir("./test"):
                f_path = os.path.join("./test", f)
                test_tgz_file.add(f_path)
        ```
        `tgz`是`tar.gz`的缩写，因此压缩tar.gz使用的方法与压缩tgz一致
    - 生成`test.tar.bz2`
        ```python
        import tarfile, os
        
        # 生成./test.tar.bz2压缩包
        with tarfile.open("./test.tar.bz2", "w:bz2") as test_tbz2_file:
            # 压缩./test文件夹
            for f in os.listdir("./test"):
                f_path = os.path.join("./test", f)
                test_tbz2_file.add(f_path)
        ```
        除`mode=w:bz2`以外，其余不变
2. zip格式
    使用`zipfile`模块，可以压缩和解压`.zip`压缩包，例如：  
    ```python
    import zipfile, os
    
    # 生成test.zip压缩包
    with zipfile.ZipFile("./test.zip", "w") as test_zip_file:
        # 压缩./test文件夹
        for f in os.listdir("./test"):
            f_path = os.path.join("./test", f)
            test_zip_file.write(f_path)
    ```
    `zipfile`的用法与`tarfile`类似，使用`zipfile.ZipFile`打开压缩包，并使用`ZipFile.write`为压缩包中添加文件。
3. rar格式  
    python目前没有支持生成rar压缩包的模块


#### 解压
1. tar、tgz、tar.gz、tar.bz2格式  
    使用`tarfile`模块，可以解压`.tar`, `.tgz`, `.tar.gz`, `.tar.bz2`等格式的压缩包  
    - 解压`test.tar`
        ```python
        import tarfile
        
        # 解压test.tar压缩包到当前目录
        with tarfile.open("./test.tar") as test_tar_file:
            test_tar_file.extractall("./")
        ```
        使用默认的`mode=r`打开压缩包并使用`TarFile.extractall(path)`提取其中所有内容到`path`目录，完成解压
    - 解压`test.tgz`
        ```python
        import tarfile
        
        # 解压test.tgz压缩包到当前目录
        with tarfile.open("./test.tgz", "r:gz") as test_tgz_file:
            test_tgz_file.extractall("./")
        ```
        使用`mode=r:gz`打开并解压.tgz压缩包
    - 解压`test.tar.gz`  
        使用的方法与解压`test.tgz`一致
    - 解压`test.tar.bz2`
        ```python
        import tarfile
        
        # 解压test.tar.bz2压缩包到当前目录
        with tarfile.open("./test.tar.bz2", "r:bz2") as test_bz2_file:
            test_bz2_file.extractall("./")
        ```
        使用`mode=r:bz2`打开并解压.bz2压缩包
2. zip格式
    解压`test.zip`  
    ```python
    import zipfile
    
    # 解压test.zip压缩包到当前目录
    with zipfile.ZipFile("./test.zip", "r") as test_zip_file:
        test_zip_file.extractall("./")
    ```
    `zipfile`的用法与`tarfile`类似，使用`mode=r`打开压缩包并使用`ZipFile.extractall(path)`提取其中所有内容到`path`目录，完成解压
3. rar格式  
    解压`test.rar`
    ```python
    from unrar import rarfile
    
    # 解压test.rar到当前目录
    rar_file = rarfile.RarFile("./test.rar")
    rar_file.extractall("./")
    ```
    `rarfile`的用法与`tarfile`类似，打开压缩包后使用`RarFile.extractall(path)`提取其中所有内容到`path`目录，完成解压
