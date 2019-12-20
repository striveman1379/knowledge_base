## 1.python顶部要指定路径和编码

    #!/usr/bin/python 
    # -*- encoding: "UTF-8" -*-


## 2.出现如下的错误
### UnicodeDecodeError: 'ascii' codec can't decode byte 0xe8 in position 0: ordinal not in range(128)

### 解决方案：

#### python2中用的下面这些
    import sys
    reload(sys)
    sys.setdefaultencoding('utf-8')
---    
    import sys
    defaultencoding = 'utf-8'
    if sys.getdefaultencoding() != defaultencoding:
        reload(sys)
        sys.setdefaultencoding(defaultencoding)
    
---
上面两种其实一样的，写法不同而已

