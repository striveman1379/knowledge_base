## 简介
Click 是用 Python 写的一个第三方模块，用于快速创建命令行。

## 安装
`pip install click`

## 使用
1. 使用 `@click.command()`装饰一个函数，使之成为命令行接口
2. 使用 `@click.option()`装饰函数，为其添加命令行选项
3. 使用 `@click.argument()`装饰函数，为其添加固定参数，支持设置不定长度的参数值  

其中，`@click.command()`必须存在，`@click.option()`和`@click.argument()`可根据情况选用至少一种

例如:
```
# hello.py
import click

@click.command()
@click.option('--count', default=1, help='number of greetings')
@click.argument('name')
def hello(count, name):
    for x in range(count):
        print('Hello %s!' % name)

if __name__ == '__main__':
    hello()
```
函数`hello()`有2个参数，`name`和`count`。其中`name`使用argument进行装饰， `count`使用option进行装饰，规定它的默认值为1，帮助信息为'number of greetings'。主函数中调用`hello()`时没有提供给它任何参数，因为参数已经通过命令行进行传递。

执行 `hello.py`示例：
+ 直接执行`python3 hello.py`会报错：缺少参数`name`

    ```python
    > python3 hello.py
    Usage: hello.py [OPTIONS] NAME
    
    Error: Missing argument "name".
    ```
+ 执行`python3 hello.py -v`会报错：不存在选项`-v`
    ```python
    > python3 hello.py -v
    Error: no such option: -v
    ```
+ 执行`python3 hello.py name`，不输入`--count`参数，会打印默认的`count=1`次`Hello name!`
    ```python
    > python3 hello.py name
    Hello name!
    ```
+ 执行`python3 hello.py --help`会打印帮助信息
    ```python
    > python3 hello.py --help
    Usage: hello.py [OPTIONS] NAME
    
    Options:
      --count INTEGER  number of greetings
      --help           Show this message and exit.
    ```
+ 执行`python3 hello.py --count 3 name`会将`Hello name!`打印`count=3`次
    ```python
    > python3 hello.py --count 3 name
    Hello name!
    Hello name!
    Hello name!
    ```
    
### 帮助信息
click会自动生成帮助信息，命令行选项为`--help`

### 使用option进行装饰

1. 参数  
    + 每个参数都有长参数和短参数2种写法
        + 长参数`--argname`中的参数名`argname`应与函数中的参数名一致
        + 如存在长参数，短参数`-a`中的参数名`a`可以为任意值，如不存在长参数，短参数`-a`中的参数名`a`应与函数中的参数名一致
        + 指定参数时可以同时使用这2种方法，也可以只使用其中一种
        + 例如  
            ```python
            @click.command()  
            @click.option('-v', '--verbose')  
            def log(verbose):  
                print('Verbosity: %s' % verbose)
            ```
            同时指定了长参数`--verbose`和短参数`-v`
2. 必填参数
    + 每一个使用option装饰的参数都必须是函数已有的参数，且每个函数已有的参数都必须被装饰。因此，函数的每个未设置默认值的参数都是必填参数。
    + 如函数使用了可变参数，该可变参数不是必填参数，详见可选参数中的例子。
3. 可选参数
    + 设置参数的默认值`default`即可使该参数成为可选参数
    + 例如
        ```python
        @click.command()
        @click.option('--count', default=1, help='number of greetings')
        @click.option('--name', default='World')
        def hello(count, name):
            for x in range(count):
                print('Hello %s!' % name)
        ```
        将`count`和`name`两个参数设为了可选参数，直接执行`python hello.py`时不会报错，而是打印一次"Hello World!"，因为`count`的默认值为1，`name`的默认值为"World"
    + 或在函数中使用可变参数
    + 例如
        ```python
        @click.command()
        @click.option('--count', help='number of greetings', type=int)
        @click.option('--name')
        def hello(**args):
            count = 1
            if args['count']:
                count = args['count']
            name = 'World'
            if args['name']:
                name = args['name']
            for x in range(count):
                print('Hello %s!' % name)
        ```
        可以达到上个例子一样的效果
4. 参数默认值
    + 设置option中的默认值`default`，赋予该参数一个默认值
    + 如没有设置option中的类型`type`，则此参数的类型为默认值的类型
    + 如没有设置option中的类型`type`和默认值`default`，则此参数的类型默认为字符串
    + 例如`@click.option('--shout/--no-shout', default=False)`  
        会将`shout`和`no-shout`参数设为`False`，如使用了`--shout`选项，将`shout`参数设为`True`
5. 控制台提示输入
    + 设置option中的提示`prompt`，如在控制台输入参数时没有输入该参数，会提示输入该参数而不会直接报错
    + `prompt`的值可以为`True`，也可以为一个字符串。
        + 如`prompt=True`，提示`"参数名（首字母大写）: "`（如参数名为`count`，提示`Count: `）
        + 如`prompt`的值为字符串，提示时显示该字符串
    + 例如`@click.option('--name', prompt=True)`，    如没有输入`--name=...`，会提示`Name: `
6. 多个参数
    + 设置option中的参数个数`nargs`，会将多个参数作为一个元组传递给函数
    + 如输入的参数个数不足或过多，会报错`Error: Missing argument`或`Error: Got unexpected extra argument`
    + 用户也可以元组的方式提供参数，元组中参数的个数必须与`nargs`一致，且需要设置option中的类型`type`为`click.Tuple([..., ...])`(`...`部分可以为unicode，int等)
    + 与argument不同，option中的`nargs`值必须为非负整数，而argument中的`nargs`可以为-1
    + 例如`@click.option('--pos', nargs=2, type=float)`，会将输入时`--pos a b`中的a, b两个浮点数作为元组`(a, b)`传递给函数
    + `@click.option('--pos', nargs=2, type=click.Tuple(float, float))`会将输入时`--pos (a, b)`中的元组`(a, b)`传递给函数

### 使用argument进行装饰
`@click.argument`可以用来添加必填参数，它的用法与option类似，但支持的功能比option少。  

1. 帮助信息
    + 与option不同，argument的帮助信息只显示每个参数的位置，没有详细说明
    + 例如
        ```python
        @click.command()
        @click.option('--count', default=1, help='number of greetings')
        @click.argument('firstname')
        @click.argument('lastname')
        def hello(count, firstname, lastname):
            for x in range(count):
                print('Hello %s %s!' % (firstname, lastname))
        ```
        的帮助信息为：
        ```python
        Usage: hello.py [OPTIONS] FIRSTNAME LASTNAME
    
        Options:
        --count INTEGER  number of greetings
        --help           Show this message and exit.
        ```
2. 参数
    + 使用argument装饰的参数应注意输入时的顺序，而使用option装饰的参数可以以任意顺序输入
3. 不定长参数
    + 设置argument中的参数个数`nargs`，会将多个参数作为一个元组传递给函数
    + 如`nargs=-1`则参数长度不固定
    + 例如
        ```
        # copy.py
        import click
        
        @click.command()
        @click.argument('src', nargs=-1)
        @click.argument('dst', nargs=1)
        def copy(src, dst):
            for fn in src:
                print('move %s to folder %s' % (fn, dst))
        
        if __name__=="__main__": 
            copy()
        ```
        中的`src`参数为不定长参数，`dst`的长度为1  
        执行`copy.py`示例：
        + 直接执行`python3 copy.py`会提示缺少`dst`参数
            ```python
            > python3 copy.py
            Usage: copy.py [OPTIONS] [SRC]... DST
            
            Error: Missing argument "dst".
            ```
        + 执行`python3 copy.py dst`会将提供的唯一一个参数传递给`dst`参数，默认`src`参数的长度为0。因此不做输出。
            ```python
            > python3 copy.py dst
            >
            ```
        + 执行`python3 copy.py src1 src2 src3 dst`会分别打印对应3个`src`参数的句子
            ```python
            > python3 copy.py src1 src2 src3 dst
            move src1 to folder dst
            move src2 to folder dst
            move src3 to folder dst
            ```

### 参考
+ click官网 http://click.pocoo.org/5
+ 命令行神器 Click 简明笔记 http://python.jobbole.com/87111/
