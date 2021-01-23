***想要理解Python中的装饰器，不得不先理解闭包（closure）这一概念***

### 闭包

闭包必须满足三个条件:

1. 必须有一个内嵌函数
2. 内嵌函数必须引用外部嵌套函数中的变量
3. 外部函数返回值必须是内嵌函数

```
# -*- coding:utf-8 -*-

#print_msg为外围函数
def print_msg():
    msg = "I'm closure"

    #printer是嵌套函数
    def printer():
        print(msg)

    return printer

#这里获得的就是一个闭包
closuer = print_msg()

#输出I'm closure
closuer()
```

###### msg 是一个局部变量，在print_msg函数执行之后就不会存在了。但是内部的嵌套函数引用了这个变量，将这个局部变量封闭在了嵌套函数中，这样就形成了闭包。++
###### 所以说，闭包就是引用了自有变量的函数，这个函数保存了执行的上下文，可以脱离原本的作用域独立存在。


### 装饰器


```
import functools

#定义一个打印出方法名及其参数的装饰器
def log(func):
    @functools.wraps(func)
    def wrapper(*args,**kwargs):
        print("call %s():"%func.__name__)
        print("args={}".format(*args))
        return func(*args,**kwargs)

    return wrapper

#调用它
@log
def test(p):
    print(test.__name__+"param:" + p)

test("I'm a param")
```

**输出结果为：**

```
call test():
args=I'm a param
testparam:I'm a param
```


**装饰器在使用时，用了@语法，让人有些困扰。其实，装饰器只是个方法，与下面的调用方式没有区别**：


```
def test(p):
    print(test.__name__ + " param: " + p)

wrapper = log(test)
wrapper("I'm a param")
```

**@语法只是将函数传入装饰器函数，并无神奇之处。**

**值得注意的是@functools.wraps(func)，这是python提供的装饰器。它能把原函数的元信息拷贝到装饰器里面的 func 函数中。函数的元信息包括docstring、name、参数列表等等。可以尝试去除@functools.wraps(func)，你会发现test.__name__的输出变成了wrapper。**


### 带参数的装饰器


```
# 带参数的装饰器，外面多套了一层，参数往进传，参数可以为多个

def log_with_param(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            print("call %s():" % func.__name__)
            print("args={}".format(*args))
            print("log_param = {}".format(text))
            return func(*args, **kwargs)
        return wrapper
    return decorator

# 给装饰器加了个参数logLevel
@log_with_param("logLevel")
def test_with_param(p):
    print(test_with_param.__name__ + "param:" + p)


test_with_param("I'm a param")
```

**输出结果**：

```
call test_with_param():
args=I'm a param
log_param = logLevel
test_with_paramparam:I'm a param
```

**内层的decorator函数的参数func传入方式的道理与上面的一样，将其@语法去除，恢复函数调用的形式一看就明白了：**


```
# 传入装饰器的参数，并接收返回的decorator函数
decorator = log_with_param("param")
# 传入test_with_param函数
wrapper = decorator(test_with_param)
# 调用装饰器函数
wrapper("I'm a param")
```

**装饰器这一语法体现了Python中函数是第一公民，函数是对象、是变量，可以作为参数、可以是返回值，非常的灵活与强大。**
