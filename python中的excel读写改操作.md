---
title: python中的excel读写改操作
tags: [Import-bef6]
created: '2019-11-06T06:51:51.318Z'
modified: '2019-11-27T06:42:17.447Z'
---

### python中的excel读写改操作
#### 写入  
使用`xlwt`第三方模块创建并写入excel表（工作簿）
1. 创建工作簿  
    `Workbook`类的构造函数为`Workbook(encoding='ascii', style_compression=0)`会创建一个工作簿  
    `:encoding`: 设置字符编码，默认为ASCII码  
    `:style_compression`: 表示是否压缩，不常用  
    例如：
    ```python
    book = xlwt.Workbook(encoding='utf-8', style_compression=0)
    ```
2. 添加工作表
    `Workbook`实例的`add_sheet(sheetname, cell_overwrite_ok=False)`方法可以向工作簿中添加`WorkSheet`工作表  
    `:sheetname`: 工作表的名字  
    `:cell_overwrite_ok`: 是否允许覆盖单元格中的内容，如为False，每个单元格只能写入一次  
    例如：
    ```python
    sheet = book.add_sheet('test', cell_overwrite_ok=True)
    ```
    向工作簿`book`中添加了一个名为`test`的表，并允许覆盖单元格
3. 向单元格中写入数据
    `WorkSheet`实例的`write(self, r, c, label='')`方法可以向一个单元格中写入数据  
    `:r`: 工作表中的行数（从0开始为第一行）  
    `:c`: 工作表中的列数（从0开始为第A列）  
    `:label`: 写入单元格的内容  
    例如：
    ```python
    sheet.write(0, 0, "一")
    sheet.write(0, 1, "二")
    sheet.write(1, 0, "三")
    sheet.write(1, 1, "四")
    ```
4. 保存  
    `Workbook`实例的`save(filename_or_stream)`方法将新建的工作簿保存至指定路径或文件中， 例如：
    ```python
    book.save("test.xlsx")
    ```
    将以上的工作簿存为test.xlsx，生成的excel如下图：  
    
    ![image](https://github.com/striveman1379/knowledge_base/blob/master/images/image1_xlwt.png)
    
#### 读取  
使用`xlrd`第三方模块读取excel表
1. 打开工作簿  
    使用`open_workbook(filename)`方法可以打开一个名为`filename`工作簿，例如:  
    ```python
    book = xlrd.open_workbook("test.xlsx")
    ```
    打开了上例中创建的工作簿，获取了`book`这一`xlrd.book.Book`实例
2. 获取工作表
    - 通过工作表的索引获取工作表  
        `Book`实例中的`sheet_by_index(sheetx)`可以通过工作表的索引`sheetx`获取工作表。这将返回一个`xlrd.sheet.Sheet`实例。  
        例如：
        ```python
        sheet = book.sheet_by_index(0)
        ```
    - 通过工作表的名字获取工作表
        `Book`实例中的`sheet_by_name(sheet_name)`可以通过工作表的名字`sheet_name`获取工作表。这将返回一个`xlrd.sheet.Sheet`实例。  
        例如：
        ```python
        sheet = book.sheet_by_name('test')
        ```
3. 获取总行数/总列数  
    `Sheet`实例中的`nrows`和`ncols`属性分别为工作表的总行数和总列数。  
    例如：
    ```python
    print(sheet.nrows)  # 2
    print(sheet.ncols)  # 2
    ```
4. 获取一行/一列数据  
    `Sheet`实例中的`row_values(rowx, start_colx=0, end_colx=None)`和`col_values(colx, start_rowx=0, end_rowx=None)`分别返回第`rowx`行/`colx`列的列表。  
    例如：
    ```python
    print(sheet.row_values(0))  # ['一', '二']
    print(sheet.col_values(0))  # ['一', '三']
    ```
5. 获取一个单元格中的数据
    `Sheet`实例中的`cell_value(rowx, colx)`方法返回工作表中第`rowx`行第`colx`列的单元格中的内容。  
    例如：
    ```python
    print(sheet.cell_value(1, 1))  # '四'
    ```
    
#### 修改
利用`xlrd`模块和`xlutils.copy`中的`copy`方法打开、复制并修改工作簿  
1. 打开工作簿  
    使用`open_workbook(filename)`方法可以打开一个名为`filename`工作簿，例如:  
    ```python
    book = xlrd.open_workbook("test.xlsx")
    ```
    打开了`xlwt`例子中创建的工作簿，获取了`book`这一`xlrd.book.Book`实例
2. 复制工作簿  
    导入`xlutils.copy`中的`copy`方法，`copy(wb)`方法复制`xlrd.Book`对象`wb`，并返回一个`xlwt.Workbook`对象。  
    例如：
    ```python
    from xlutils.copy import copy
    wb = copy(book)
    ```  
3. 获取工作表  
    `xlwt.Workbook`实例中的`get_sheet(sheet)`可以通过工作表的索引`sheet`获取工作表。这将返回一个`xlwt.Worksheet.Worksheet`实例。  
    例如：
    ```python
    sheet = wb.get_sheet(0)
    ```
4. 修改一个单元格中的数据
    `WorkSheet`实例的`write(self, r, c, label='')`方法可以向一个单元格中写入数据  
    `:r`: 工作表中的行数（从0开始为第一行）  
    `:c`: 工作表中的列数（从0开始为第A列）  
    `:label`: 写入单元格的内容  
    例如：
    ```python
	sheet.write(0, 0, "壹")  # 修改第一个单元格
    sheet.write(2, 0, "五")  # 增加单元格
    sheet.write(2, 1, "六")  # 增加单元格
    ```
5. 保存  
    `Workbook`实例的`save(filename_or_stream)`方法将修改后的工作簿保存至指定路径或文件中， 例如：
    ```python
    wb.save("test.xlsx")
    ```
    将以上的工作簿存为test.xlsx，修改后的excel如下图：  
    
    ![image](https://github.com/striveman1379/knowledge_base/blob/master/images/image2_xlwt.png)









