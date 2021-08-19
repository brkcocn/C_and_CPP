```warning: conflicting types for 'fun'`` 未声明函数fun

fopen 没创建文件 权限错用单引号 ‘w’

putchar 用单引号

宏定义无分号

 

 

把 snake.size 错当成 size 并定义 混淆

 

Nullpr 一般为那一步置空指针 如```if( p=NULL)```

访问冲突conflict 一般为内存泄漏，分配的内存不够。



strcpy 拷贝字符串到'\0'结束 
memcpy 拷贝大小固定内存 无‘\0'
用 memset初始化就不会有乱码

一级指针 储存一个地址（可以）对应 多个数据  （线）
二级指针  储存多个地址  对应多个数据  （面）





字符串数组打印函数

```c
void show(char *str) {
	for (int i = 0; i < 5; i++)
	{
		printf("%s", str[i]);  error!!
		printf("%c\n", str[i]);  right!!
	}
}
```

```c
		if (maxORmin!=i)
		{
			char buf[32] = {0};
			//char *buf=str[i];
		    strcpy(buf, str[i]);
			str[i] = str[maxORmin];
			 str[maxORmin]=buf;
		}
```

memcpy 、strcpy 处理指针所指的内存，相当于解引用一次再处理。