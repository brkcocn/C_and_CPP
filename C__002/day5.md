### 结构体嵌套二级指针练习

### 结构体偏移量

1. 获取属性偏移 

    ```c
    (1) offsetof(s,m) 
    (2) (int)&(p->b) - (int)p
    (3) (char *)&(p->b) - (char *)p    
    ```

2. 通过偏移量 获取内存

    ```c
    	printf("t1.b = %d\n", *(int *)((char *)&t1 + offsetof(struct Teacher, b)));
    
    	printf("t1.b = %d\n", *(int *)((int *)&t1 + 1 ));
    ```

3. 结构体嵌套结构体

### 内存对齐

内存对齐是   操作系统提高访问内存的策略，避免获取同一数据二次访问。

**查看对齐模数  #pragma pack(show)         默认为8**

结构体数据类型 对齐规则

1. 第一个属性开始  从0开始偏移

2. 第二个属性开始  要放在 该类型的大小与对齐模数  较小值的整数倍

3. 所有属性都计算完后，再整体做二次偏移，整体计算的结果 要放在结构体最大类型与对齐模数    较小值的整数倍上

4. 结构体嵌套结构体  结构体嵌套结构体时候，子结构体放在该子结构体中最大类型和对齐模数    较小值比的整数倍上

**简化版**

1. 第一个属性开始  从0开始偏移

2. 第二个属性开始  要放在 该类型的大小的整数倍

3. 所有属性都计算完后，再整体做二次偏移，整体计算的结果 要放在结构体最大类型整数倍上

4. 结构体嵌套结构体  结构体嵌套结构体时候，子结构体放在该子结构体中最大类型的整数倍上

**最终简化版**

用结构体中（包括子结构体）最大数据类型划分内存，填入数据即可。



### 文件读写回顾

#### **1.按照字符读写**

**写  fputc**

```c
int fputc(int ch, FILE * stream);
返回值：
	成功：成功写入文件的字符
	失败：返回-1
```
**写  fgetc**  

```c
int fgetc(FILE * stream);
返回值：
	成功：返回读取到的字符
	失败：-1
```

while ( (ch = fgetc(f_read)) != EOF  )  判断是否到文件尾

#### **2.按行读写**

**写  fputs**

  ```c
  int fputs(const char *str,FILE *stream);
  
  功能：将str所指定的字符串写入到stream指定的文件中， 字符串结束符 '\0' 不写入文件。 
  
  返回值：
  
    成功：0
  
    失败：-1
  ```
**读  fgets**

```c
char * fgets(char * str,int size,FILE * stream);

功能：从stream指定的文件内读入字符，保存到str所指定的内存空间，直到出现换行字符、读到文件结尾或是已读了size - 1个字符为止，最后
    
    会自动加上字符 '\0' 作为字符串结束。
    

参数：   size：指定最大读取字符串的长度（size - 1）

返回值：

  成功：成功读取的字符串

  读到文件尾或出错： NULL
```

```c
方法1
    	while ( !feof(f_read ))
	{
		char buf[1024] = { 0 };
		fgets(buf, 1024, f_read);
		printf("%s", buf);
	}

方法2
    while (fgets(tmp, 1024, f_read))
	{
		printf("%s", tmp);
	}
```



#### **3.按块读写**

1. **写  fwrite**
   4.3.1.1	参数1 数据地址  参数2  块大小   参数3  块个数   参数4  文件指针
   
   ```c
   size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
   功能：以数据块的方式给文件写入内容
   参数：
   	ptr：准备写入文件数据的地址
   	size： size_t 为 unsigned int类型，此参数指定写入文件内容的块数据大小
   	nmemb：写入文件的块数，写入文件数据总大小为：size * nmemb
   	stream：已经打开的文件指针
   返回值：
   	成功：nmemb
   	失败：0
   ```
   
2. 读  fread 

   ```c
   size_t fread(void *buf, size_t size, size_t nmemb, FILE *stream);
   功能：以数据块的方式从文件中读取内容
   参数：
   	buf：存放读取出来数据的内存空间
   	size： size_t 为 unsigned int类型，此参数指定读取文件内容的块数据大小
   	nmemb：读取文件的块数，读取文件数据总大小为：size * nmemb
   	stream：已经打开的文件指针
   返回值：
   	成功：实际成功读取到内容的块数，如果此值比nmemb小，但大于0，说明读到文件的结尾。
   	失败：0
   ```

   

#### 4.格式化读写

**1.写  fprintf**

```c
int fprintf(FILE * stream, const char * format, ...);
功能：根据参数format字符串来转换并格式化数据，然后将结果输出到stream指定的文件中，指定出现字符串结束符 '\0'  为止。

返回值：
	成功：实际写入文件的字符个数
	失败：-1
```

**2.读  fscanf**

```c
int fscanf(FILE * stream, const char * format, ...);
功能：从stream指定的文件读取字符串，并根据参数format字符串来转换并格式化数据。

返回值：
	成功：实际从文件中读取的字符个数
	失败： - 1
```

**注意**：**fscanf** 遇到空格和换行时结束。

#### 5.随机位置读写

**1.fseek**( 文件指针， 偏移， 起始位置 )  

```c
int fseek(FILE *stream, long offset, int whence);
功能：移动文件光标到指定位置。

参数：whence：其取值如下：
		SEEK_SET：从文件开头移动
		SEEK_CUR：从当前位置移动
		SEEK_END：从文件末尾移动
返回值：
	成功：0
	失败：-1
```

**2.ftell**

```c
long ftell(FILE *stream);
功能：获取文件流（文件光标）的读写位置。
参数：

返回值：
	成功：当前文件流（文件光标）的读写位置
	失败：-1
```



3. ```void rewind(FILE *stream); ```   将文件光标置首

#### 6.error宏  

**利用perror打印错误提示信息**



### 文件读写注意事项

**1.注意事项1：** 按照字符方式读文件有滞后性，会读出EOF**(-1)**
**2.注意事项2：**二进制读写结构体时，不要存指针到文件中，要将指针指向的内容存放到文件中

**练习：配置文件读写案例**

1. 文件中按照键值对方式 存放了有效的信息需要解析出来
2. 创建 config.h  和 config.c做配置文件读操作
3. 获取有效信息的行数  getFileLines
4. 判断字符串是否是有效行  int isValidLines(char *str)
5. 解析文件到配置信息数组中
 ```c 
 void parseFile(char * filePath, int lines , struct  ConfigInfo ** configinfo);
 ```
6. 通过key获取value值
```c
char * getInfoByKey(char * key, struct ConfigInfo * configinfo, int len);
```
7. 释放内存
```c
void freeConfigInfo(struct ConfigInfo * configinfo);
```



**configinfo.h**

```c
//获取有效行数
int getFileLines(char * filePath);

//检测当前行是否是有效信息
int isValidLines(char *str);

//解析文件
void parseFile(char * filePath, int lines , struct  ConfigInfo ** configinfo);

//根据key获取对应value
char * getInfoByKey(char * key, struct ConfigInfo * configinfo, int len);

//释放内存
void freeConfigInfo(struct ConfigInfo * configinfo);
```

