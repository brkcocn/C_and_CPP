读写文件与printf、scanf关联

```c
printf -- 屏幕 -- 标准输出

scanf -- 键盘 -- 标准输入

perror -- 屏幕 -- 标准错误
```

系统文件：

```c
标准输入 -- stdin -- 0

标准输出 -- stdout -- 1

标准错误 -- stderr -- 2

应用程序启动时，自动被打开，程序执行结束时，自动被关闭。 ---- 隐式回收。堆空间一起被回收。
```

文件指针和普通指针区别：

```c
FILE *fp = NULL;

借助文件操作函数来改变 fp 为空、野指针的状况。	fopen();  --> 相当于 fp = malloc();

操作文件， 使用文件读写函数来完成。 fputc、fgetc、 fputs、fgets、 fread、fwrite
```

文件分类：

```c
设备文件：

	屏幕、键盘、磁盘、网卡、声卡、显卡、扬声器...

磁盘文件：

	文本文件： 	ASCII

	二进制文件：	0101 二进制编码
```

文件操作一般步骤：

```c
1. 打开文件 fopen()  --> FILE *fp;

2. 读写文件 fputc、fgetc、fputs、fgets、fread、fwrite

3. 关闭文件 fclose()  
```


打开、关闭文件函数：

```c
FILE * fopen(const char * filename, const char * mode);

	参1：待打开文件的文件名（访问路径）

	参2：文件打开权限： r w w+ wb+

		"r": 只读方式打开文件， 文件不存在，报错。存在，以只读方式打开。

		"w": 只写方式打开文件， 文件不存在，创建一个空文件。文件如果存在，清空并打开。

		"w+":读、写方式打开文件，文件不存在，创建一个空文件。文件如果存在，清空并打开。

		"r+":读、写方式打开文件, 文件不存在，报错。存在，以读写方式打开。

		"a": 以追加的方式打开文件。

		"b": 操作的文件是一个二进制文件（Windows）

	返回值：
	    
	    成功：返回打开文件的文件指针

		失败：NULL
```


```c
int fclose(FILE * stream);

	参1：打开文件的fp（fopen的返回值）

	返回值：成功 ：0， 失败： -1;
```

文件访问路径：

```c
绝对路径：

	从系统磁盘的根盘符开始，找到待访问的文件路径

	Windows书写方法：

		1）C:\\Users\\afei\\Desktop\\06-文件分类.avi

		2）C:/Users/afei/Desktop/06-文件分类.avi  --- 也使用于Linux。

相对路径：

	1）如果在VS环境下，编译执行（Ctrl+F5），文件相对路径是指相对于  .vcxproj 所在目录位置。

	2）如果是双击 xxx.exe 文件执行，文件的相对路径是相对于 xxx.exe 所在目录位置。 
```

# 文件操作函数

##### fputc:

> 按字符写文件

```c
int fputc(int ch, FILE * stream);

	参1：待写入的字符

	参2：打开文件的fp（fopen的返回值）

	返回值： 
	     
	     成功： 写入文件中的字符对应的ASCII码

		 失败： -1
```

#####  fgetc:

> 按字符读文件

```c
int fgetc(FILE * stream);

	参1：待读取的文件fp（fopen的返回值）

	返回值： 
	
	     成功：读到的字符对应的ASCII码

		 失败： -1

文本文件的结束标记： EOF ---》 -1 
```

​	

##### feof()：

```c
int feof(FILE * stream);

	参1： fopen的返回值

	返回值： 
	
	     到达文件结尾--》非0 【真】
			
		 没到达文件结尾--》0 【假】

作用：	
	用来判断到达文件结尾。 既可以判断文本文件。也可以判断二进制文件。

特性：

 要想使用feof()检测文件结束标记，必须在该函数调用之前，调用读文件函数。（fget fscanf fread）
```



##### fgets()：

```c
获取一个字符串， 以\n作为结束标记。自动添加 \0. 空间足够大读\n, 空间不足舍弃\n, 必须有\0。

char * fgets(char * buffer , int size, FILE * stream);

	char buf[10];  	hello --> hello\n\0

	返回值： 
	
	     成功： 读到的字符串

		 失败： NULL
```

##### fputs()：

```c
写出一个字符串，如果字符串中有\n,会写\n。

int fputs(const char * buffer, FILE * stream);

	返回值： 
	     
	     成功： 0

		 失败： -1
```





**练习**

获取用户键盘输入，写入文件。

文件版四则运算。
