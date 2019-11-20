## python os.listdir和os.walk的区别

### 1、os.listdir
##### 使用情况：在一个目录下面只有文件，没有文件夹，这个时候可以使用os.listdir；

例如：d:\listdir文件夹下有三个文件（text1.txt、test2.txt、test3.txt），获得文件的绝对路径

    import os
	path = r'd:\listdir'
	for filename in os.listdir(path):
    #目录的路径和文件名拼接起来，得到了文件的绝对路径
    	print(os.path.join(path,filename))

输出结果：

	 d:\listdir\test1.txt
	 d:\listdir\test2.txt
	 d:\listdir\test3.txt


### 2、os.walk
使用情况：递归的情况，一个目录下面既有目录（目录下面还可能有目录和文件）也有文件，如何读取里面所有文件，使用os.walk；

例如：d:\listdir文件夹下有三个文件（text1.txt、test2.txt、test3.txt）和两个文件夹filedir1（包含文件text1_1.txt、text1_2.txt）和filedir2（包含文件text2_1.txt、text2_2.txt）：

    import os
	path = r'd:\listdir'
	for dirpath,dirnames,filenames in os.walk(path):
    	print(dirpath,dirnames,filenames)
输出结果：
	
    d:\listdir ['filedir1', 'filedir2'] ['test1.txt', 'test2 .txt']
	d:\listdir\filedir1[] ['test1_1.txt', 'test1_2.txt']
	d:\listdir\filedir2[] ['test2_1.txt','test2_2.txt']

##### 说明：os.walk输入一个路径名称，以yield的方式（其实是一个生成器）返回一个三元组 dirpath, dirnames, filenames；

dirpath为目录的路径，为一个字符串。比如上面的d:\listdir、d:\listdir\filedir1、d:\listdir\filedir2等。

dirnames列出了目录路径下面所有存在的目录的名称。比如在d:\listdir下面有两个目录：filedir1和filedir2。

filenames列出了目录路径下面所有文件的名称。同样在 d:\listdir下面有两个文件test1.txt和test2 .txt，那么将会列出这两个文件名。

获取路径下面的所有文件的绝对路径：
	
	import os
	path = r'd:\listdir'
	for dirpath,dirnames,filenames in os.walk(path):
    	for filename in filenames:
        	print(os.path.join(dirpath,filename))

输出结果：
	
	d:\listdir\test1.txt
	d:\listdir\test2.txt
	d:\listdir\filedir1\test1_1.txt
	d:\listdir\filedir1\test1_2.txt
	d:\listdir\filedir2\test2_1.txt
	d:\listdir\filedir2\test2_2.txt


#### 两者执行效率情况

使用os.listdir查找多层目录里面的文件时，需要递归调用它自身来查找到每一个文件。

使用os.walk也是通过遍历每一个目录来查找到每一个文件。

我通过查找自己pycharm安装目录里面的每一个文件来测试效率：

os.walk的运行时间为：0.05307364463806152 S

os.listdir的运行时间为：0.05684828758239746 S

二者的效率为同一级别的，但是os.walk使用起来更加方便一些。
