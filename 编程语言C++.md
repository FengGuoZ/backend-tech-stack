## C语言🌼

### 1 变量类型🌼

#### 1.1 字符型 char	

- 占用**1B**存储空间，取值范围[0, 127]，如`'a'、'A'、'1'、'$'`等

- **char取值通过ASCII码表与字符对应**，`'a' = 97 = 0x61  `

- 字符输出控制格式控制**%c**



#### 1.2 整型 int

- 整数在计算机上**是准确表示的**，如`123、500、0、-10`等
- 整型包括int、short、long
- 取值范围[-2147483648, 2147483647]
  - 二进制表示下，最高位1表示负数，最高位0为正数
  - `0b1000 0000 0000 0000 0000 0000 0000 0000`表示INT_MIN
  - `0b1111 1111 1111 1111 1111 1111 1111 1111`表示INT_MAX

![image-20210320195212716](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210320195212716.png)



**C语言进制**

- C语言默认10进制
- 0b表2进制
- 0x表示16进制

100 = 0b0110 0100 = 0x64	进制转换



**数据类型的别名 typedef**

C语言使用 typedef 关键字**给数据类型起别名**

- 简化名称
- 提高可读性

```c++
typedef unsigned int size_t;	// 提高可读性
typedef pair<int, int> node;	// 简化名称
```



**随机数**

在C语言中，使用 <stdlib.h> 头文件中的 srand和rand 函数来**生成随机数**。

```c
#include <stdlib.h>
void srand(unsigned int seed);   	// 随机数生成器的初始化函数
int  rand();                    	// 获一个取随机数
```

srand函数初始化随机数发生器（俗称种子），通常使用time(0)作为种子

如果不播种，rand()生成的是伪随机数

```c
# <time.h>
srand(time(0));  	//	播下随机种子
int ii = rand();	//	获取随机数
```



在实际开发中，需求往往是**一定范围内的随机数**。

满足此需求的常用的方法是取模运算（取余数），再加上一个加法运算：

```c
int a = rand() % 50;   // 产生0~49的随机数
```

如果要规定上下限

```c
int a = rand() % 51 + 100;   // 产生100~150的随机数
```



#### 1.3 浮点型 double

- 浮点型在计算机上**是近似表示的**，有小数位，如`10.0、123.55、3459.98、-50.3`
- 8B双精度浮点型double
- 4B单精度浮点型float



#### 1.4 字符串

**C风格字符串是一个以空字符'\0'结束的字符数组**，这是约定，是规则

**空字符'\0'占用1B存储空间**，也可以直接写成0

![](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210320101004912.png)



###### 字符串结束标志

- 在C语言中，**字符串总是以'\0'作为结尾**，所以'\0'也被称为字符串结束标志，或者字符串结束符

- **由`" "`包围的字符串常量会自动在末尾添加`'\0'`**

- 例如，`"abc123"`从表面看起来只包含了 6 个字符，其实不然，C语言会在最后隐式地添加1个`'\0'`，这个过程是在后台默默地进行的，用户感受不到



###### '\0'详解

- '\0'是 ASCII 码表中的第 0 个字符
- 英文称为 NUL，中文称为“空字符”
- '\0'既不能显示，也没有控制功能，输出该字符不会有任何效果
- '\0'在C语言中唯一的作用就是作为字符串结束标志



###### 字符串长度

```c
#include <string.h>
size_t  strlen( const char*  str);
```

- 功能：**计算字符串的有效长度，不包含0**，字符串实际占用内存空间 = strlen + 1
  
- 返回值：返回字符串的字符数 
- strlen 函数计算的是字符串的实际长度，遇到第一个0结束
- 函数返回值类型size_t是无符号整数
- 如果只定义字符串没有初始化，求它的长度是没意义的，它会从首地址一直找下去，遇到0停止



###### strcpy() 字符串复制或赋值

```c
char *strcpy(char* dest, const char* src);
```

- 功 能：将参数src字符串拷贝至参数dest所指的地址

- 返回值：返回参数dest的字符串起始地址
- 复制完字符串后，在dest后追加0
- 如果参数dest所指的内存空间不够大，可能会造成缓冲溢出的错误情况



###### strcat() 字符串拼接

```c
char *strcat(char* dest,const char* src);
```

- 功能：将src字符串拼接到dest所指的字符串尾部

- 返回值：返回dest字符串起始地址

- dest最后原有的结尾字符0会被覆盖掉，并在连接后的字符串的尾部再增加一个0

- dest要有足够的空间来容纳要拼接的字符串，否则可能会造成缓冲溢出的错误情况



###### strcmp() 字符串比较

```c
int strcmp(const char *str1, const char *str2 );
```

- 功能：比较str1和str2的大小。

- **返回值：相等返回0**，str1大于str2返回1，str1小于str2返回-1。
- 两个字符串比较的方法是按顺序比较字符的ASCII码的大小。
- 在实际开发中，程序员一般只关心字符串是否相等，不关心哪个字符串更大或更小。



#### 1.5 指针类型

指针用于存放内存变量和常量地址，64位系统下占8B存储空间



### 2 常量、变量、关键字🌼

#### 2.1 常量

常量或常数，**表示固定不变的数据**，是具体的数据，**位于4G内存空间的常量区**

- 字符常量，如'6'，'a'，'F'


- 整型常量，如6，27，-299
- 浮点型常量，如5.43，-2.3，5.67，6.0
- 字符串常量，如"625"，"女"，"www.freecplus.net"，"西施"



#### 2.2 变量

变量用于存储数据，存储内容可变

**变量使用前必须先进行声明**，向操作系统申请一块内存空间，用于存放数据

**变量使用前建议初始化**，避免意料之外的错误

```c++
int   ii = 0;   		// 定义整数型变量，用于存放整数。
char  cc = 0;  			// 定义字符型变量，用于存放字符。
double money = 0.0;   	// 定义浮点型变量，用于存放浮点数。
char name[21];			// 定义一个可以存放20字符的字符串。
memset(name, 0, sizeof(name));
```

C语言**用字符数组来存放字符串**，并提供了丰富的库函数来操作C风格字符串

- 如果要**定义一个存放20个英文的字符串**，数组的**长度应该是20+1**，因为**字符串结尾标志\0也要占用1字节**

- 中文的汉字和标点符号需要两个字符宽度来存放（GBK编码）。例如name[21]可以存放20个英文字符，或10个中文字符
- 字符串不是C语言的基本数据类型，**不能用“=”赋值**，**不能用“>”和“<”比较大小**，**不能用“+”拼接**，**不能用==和!=判断两个字符串是否相同**，要用函数

| 操作类型  | 对应C字符串API |
| :-------: | :------------: |
|     =     |     strcpy     |
| > < == != |     strcmp     |
|     +     |     strcat     |



**变量的命名**

变量名属于标识符，需要符合标识符的命名规范，具体如下：

- 变量名的第一个字符必须是字母或下划线，不能是数字和其它字符。

- 变量名中的字母是区分大小写的。比如a和A是不同的变量名，num和Num也是不同的变量名

- 变量名不可以是C语言的关键字

- 关于变量的命名，为了便于理解，尽可能用英文单词或多个英文单词的简写



#### 2.3 C关键字

C语言**共32个关键字**

- 数据类型（12个）：char int short long float double void auto emun union struct typedef
- 修饰（6个）：signed unsigned const volatile static extern
- 流程控制（11个）：if else for do while break continue return switch case default
- 其余（3个）：typedef register goto



### 3 输入输出🌼

#### 3.1 数据输入

C语言获得用户键盘输入的API

- **getchar**：输入单个字符，保存到字符变量中，**可以接收`\n`**

- **gets**：输入一行数据，保存到字符串变量中，**不接收`\n`，但会从缓冲区中清除**

- **scanf：**格式化输入函数，一次可以输入多个数据，保存到多个变量中



#### 3.2 数据输出

C语言输出数据到控制终端的API

- **putchar**：输出单个字符。

- **puts**：输出字符串。

- **printf：**格式化输出函数，可输出常量、变量等。



#### 3.3 printf 格式化输出

```c
int printf ( const char * format, ... );
```

**格式控制符**

换行：`\n`	整数：`%d`	字符：`%c`	浮点数：`%lf`	字符串：`%s`	指针变量`%p`



#### 3.4 scanf 格式化输入

- 用户通过键盘**向输入缓冲区写入数据**

- 按回车键（Enter）清空输入缓冲区

- scanf格式化匹配、接收数据

```c
int scanf ( const char * format, ... );
```

格式控制符

换行：`\n`	整数：`%d`	字符：`%c`	浮点数：`%lf`	字符串：`%s`	指针变量`%p`



### 4 运算符🌼

#### 4.1 运算符分类

C语言有**6类运算符**

1）算术运算符

2）赋值运算符

3）sizeof运算符

4）关系运算符

5）逻辑运算符

6）位运算符



#### 4.2 算数运算符

![image-20210320000145348](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210320000145348.png)

**变量的自增或自减有两种写法**

- 变量名++，表示在本次使用变量后自增


- ++变量名，表示在本次使用变量前自增

- 变量名--，表示在本次使用变量后自增


- --变量名，表示在本次使用变量前自减



#### 4.2 赋值运算符

![image-20210320000455215](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210320000455215.png)

**赋值运算符优先级极低**，低于比较运算符



#### 4.3 sizeof 运算符

- sizeof用于计算变量/数据类型占用内存字节数

- sizeof是C语言的关键字，而非函数

**sizeof用于数据类型**

```c
sizeof(char);
sizeof(int);
```

数据类型必须用括号括住

**sizeof用于变量**

```c
sizeof(变量名);
sizeof 变量名;
```

变量名可以不用括号括住，带括号的用法更普遍，大多数程序员采用这种形式



### 5 分支/循环结构🌼

#### 5.1 分支结构 - if else

if 和 else 是C语言的关键字，用来对条件进行判断，并根据判断结果执行不同的语句

**C语言中非0即真**，**只有0表示假**

```c
if (判断条件) {
  语句块1
} else {
  语句块2
}
```

![image-20210320001423279](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210320001423279.png)



#### 5.2 多叉分支结构 - switch case

switch和 case 是C语言的关键字，用于实现多重分支结构，相较于多重 if else 形式更简洁

```c
switch (表达式) {
  case 整型数值1: 语句1;
  case 整型数值2: 语句2;
  ......
  case 整型数值n: 语句n;
  default: 语句n+1;
}
```

**执行过程**

1）计算表达式的值value，**表达式本身/计算结果必须是整数和字符**，不能包含任何变量。**case后的数值**要求相同

2）从第1个case开始，比较**value** 和**整型数值1**，如相等，**顺序执行冒号后面的所有语句**，而不管后面的case是否匹配成功

3）如果**value** 和**整型数值1**不相等，就**跳过**冒号后面的语句1，继续比较第二个case、第三个case……

4）如果直到最后一个整型数值n都**没有找到相等的值**，那么就执行default后的语句 n+1

**匹配成功后的顺序执行可以被break关键字打断**



#### 5.3 循环结构

**while循环**

```c
while (表达式) {
  语句块
}
```



**for循环**

```c
for (语句1;表达式;语句2) {
  语句块
}
```

1）for循环开始时，会先执行语句1，而且在整个循环过程中**只执行一次**语句1。

2）**接着判断表达式**的条件，如果条件成立，就执行一次循环体中的语句块

3）语句块执行完后，接下来会执行语句2。

4）重复第2）步和第3），直到表达式的条件不成立才结束for循环。

在for循环中，语句1、表达式和语句2都可以为空，for (;;)等同于while (1)



**break与continue** -- 用于控制循环体代码的执行流程

- continue跳转到循环的首部，开启下一次循环


- break跳出循环



### 6 数组🌼

#### 6.1 数组的基本概念

数组(array)是一组数据类型相同的变量，**用于存放一组数据**

```c
// 数据类型 数组名[数组长度];
double money[20];
```

数组定义完成后，可以通过下标进行访问

money[0]，第2个元素是money[1]，...，直到第20个元素money[19]

数组在使用过程中**尤其注意不能越界**



#### 6.2 数组占用内存的大小

数组由多个变量组成，占用内存总空间 = 单个变量大小 × 变量个数

用sizeof（数组名）就可以得到整个数组占用内存的大小

```c
  int ii[10];    // 定义一个整型数组变量
  printf("sizeof(ii) = %d\n",sizeof(ii));     // 输出结果：sizeof(ii)=40
```



#### 6.3 数组的初始化

- 采用memset函数对数组进行初始化


```c
int no[10];
memset(no, 0, sizeof(no));	//将no数组全部初始化为0
```

- memset**按字节填充**

  - 对于非char类型，**memset只能置0**

  - 置其它数会发生逻辑错误



#### 6.4 二维数组

```c
datatype 数组名[第一维的长度][第二维的长度];
```

二维数组的初始化也是用memset

```c
int girl[4][5];
memset(girl,0,sizeof(girl));	// sizeof = 4*5*4 = 80
```



#### 6.5 字符串数组

- 一组字符组成字符串

- 一组字符串组成字符串数组
- 字符串数组的**本质为二维字符数组**

```c
char strname[10][21];   			// 10个字符串，每个字符串可以存放20个字符
memset(strname,0,sizeof(strname));	// 初始化
strcpy(strname[0],"妲己");
strcpy(strname[1],"褒似");
strcpy(strname[2],"西施");
strcpy(strname[3],"王昭君");
strcpy(strname[4],"貂婵");
……
strcpy(strname[9],"陈圆圆");
```



### 7 函数🌼

#### 7.1 函数

**函数是一组用于执行某个任务的语句集合**，可分为库函数、自定义函数

- **库函数由C语言提供**，实现了某些基本的功能，例如scanf、printf，在程序中可以直接使用

- **自定义函数是程序员为了完成某项任务而编写的函数**
  - 目的是为了实现某项的功能或让主程序更简洁
  - 自定义函数在使用之前，必须先声明和定义



#### 7.2 函数的声明

自定义函数的声明包括了**返回值**、**函数名**和**参数列表**。C语言中的声明函数的语法如下：

```c
return_type function_name(parameter list);
```

- **返回值的数据类型return_type：**函数执行完任务后的返回值
  - 可以是int、char、double或其它自定义的数据类型
  - 如果函数只执行任务而不返回值，return_type用关键字 void表示

- **函数名function_name：**函数名是标识符，命名规则与变量相同
- **参数列表parameter list：**当函数被调用时，调用者需要向函数传递参数
  - 参数列表包括参数的数据类型和书写顺序
  - 参数列表是可选的，可以有0至n个




#### 7.3 函数的定义

C语言中的函数定义的语法如下：

```c
return_type function_name(parameter list) {	// 注意，不要在函数定义的最后加分号
    // 实现函数功能的代码
}
```

函数定义的return_type、function_name和parameter list必须与函数声明一致



#### 7.4 通用功能函数

如果自定义函数是一个通用的功能模块，可以在公共的头文件中声明，在公共的程序文件中定义。

如果某程序需要调用公共的函数，在调用者程序中用#include指令包含公共的头文件，编译的时候把调用者程序和公共的程序文件一起编译

- **\#include <>** 用于包含**系统提供的头文件**
  - 编译的时候，**gcc在系统的头文件目录中寻找头文件**

- **\#include ""** 用于包含**程序员自定义的头文件**
  - 编译的时候，**gcc先在当前目录中寻找头文件**
  - 如果找不到，再到系统的头文件目录中寻找




#### 7.5 库函数

- C语言提供了数百个标准函数（C standard library），简称库函数

- 调用这些函数可以完成一些基本的功能，例如printf、scanf、memset、strcpy等

- C语言标准库函数的声明的头文件存放在/usr/include目录中


```c
<asset.h>  <ctype.h>  <errno.h>  <float.h>  <limits.h>
<locale.h>  <math.h>  <setjmp.h>  <signal.h>  <stdarg.h>
<stddef.h>  <stdlib.h>  <stdio.h>  <string.h>  <time.h>
```



#### 7.6 参数的const约束

在变量前加const约束，**主要用于定义函数的参数**，表示该参数在函数中是只读取，不允许改变

- 如果函数中试图改变它的值，编译的时候就会报错

- 可以防止程序员犯错，如果程序员犯了错误，编译器就能发现；

- 增加了源代码的可读性，标志只读变量。



### 8 C语言变量的作用域🌼

#### 8.1 作用域概念

**作用域是程序中定义的变量存在（或生效）的区域**，超过该区域变量就不能被访问。C 语言中有四种地方可以定义变量

- 在所有函数外部定义的是全局变量
- 在头文件中定义的是全局变量
- 在函数或语句块内部定义的是局部变量
- 函数的参数是该函数的局部变量



#### 8.2 全局变量

全局变量是定义在函数外部，通常是在程序的顶部（其它地方也可以）

全局变量在整个程序生命周期内都是有效的，在定义位置之后的任意函数中都能访问

全局变量在主程序退出时**由系统收回**内存空间



#### 8.3 局部变量

在某个函数或语句块的内部声明的变量称为局部变量，**局部变量只能在该函数或语句块内部的语句使用**

局部变量在函数或语句块外部是不可用的。

局部变量在函数返回或语句块结束时**由系统收回**内存空间。

局部变量和全局变量的名称可以相同，在某函数或语句块内部，如果局部变量名与全局变量名相同，就会**屏蔽全局变量**而使用局部变量



### 9 C语言指针🌼

#### 9.1 变量的地址

在C语言中，每定义一个变量，系统就会给变量分配一块内存，而内存是有地址的

如果把计算机的内存区域比喻成一个大宾馆，每块内存的地址就像宾馆房间的编号

C语言采用运算符**&**来**获取变量的地址**：

```c
int ii;
printf("address of ii：%p\n", &ii);
```

在printf函数中，输出内存地址的格式控制符是%p，地址采用十六进制的数字显示



#### 9.2 指针

**指针**是一种特别变量，**用于存放其它变量在内存中的地址编号**，指针在使用之前要先声明

```c
datatype *varname;
```

- datatype 是指针的基类型，它必须是一个有效的C数据类型（int、char、double或其它自定义的数据类型）


- varname 是指针的名称




#### 9.3 对指针赋值

不管是整型、浮点型、字符型，还是其他的数据类型的内存变量，它的地址**都是一个十六进制数**。

把指针指向具体的内存变量的地址，就是对指针赋值：

```c
double dd;
double *pdd = &dd;	// 电脑中pdd取值实例：0x7ffd79518e98
```



#### 9.4 通过指针操作内存变量

定义了指针变量，并指向了内存变量的地址，就可以通过指针来操作内存变量（在指针前加星号 *），效果与使用变量名相同



#### 9.5 空指针

空指针就是说指针没有指向任何内存变量，指针的值是空`0` `NULL`，所以不能操作内存，否则可能会引起程序的崩溃



#### 9.6 数组的地址

在C语言中，数组占用的内存空间是连续的，数组名是数组元素的首地址，也是数组的地址

**数组名、对数组取地址和数组元素的首地址是同一回事。**在应用开发中，程序员一般用数组名，书写最简单

```c++
int a[10];
cout << a << endl;
cout << &a << endl;
cout << &(a[0]) << endl;
/*	输出
		0x7ffcd6d19120
		0x7ffcd6d19120
		0x7ffcd6d19120
*/
```



#### 9.7 地址的运算

**地址可以用+-来运算**，运算单位为存储单元大小

加1表示**下一个存储单元的地址**，减1表示**上一个存储单元的地址**

一般情况下，地址的运算适用于数组，对单个变量的地址的运算没有意义



#### 9.8 指针占用内存情况

指针也是一种内存变量，是内存变量就要占用内存空间

在C语言中，任何类型的指针占用8字节的内存（32位操作系统4字节）



### 10 C语言数据类型转换🌼

- 计算机进行算术运算时，要求各操作数的类型具有**相同的存储位数及存储方式**
- 不能将 char 型（ 1 字节）数据与 int 型（4字节）数据直接参与运算
- 对于某些类型的转换编译器可隐式地自动进行，不需程序员干预，称这种转换为**自动类型转换**
- 而有些类型转换需要程序员显式指定，这种类型转换称为**强制类型转换**



#### 10.1 自动类型转换

一个表达式中出现不同类型间的混合运算，**较低类型将自动向较高类型转换**

- 不同数据类型之间的差别在于数据的取值范围和精度上

- 一般情况下，数据的取值范围越大、精度越高，其类型也越“高级”

赋值运算符两侧的类型不一致时，**左值优先**



**操作数中没有浮点型数据时**

`signed char->unsigned char->short->unsigned short->int->unsigned int->long->unsigned long`



**操作数中有浮点型数据时**

当操作数中含有浮点型数据时，**所有操作数都将转换为 double 型**

```c
int ii=100;
double dd=200.5;
ii + dd;
```

上述算术表达式中操作数 dd 为double，所以先把 ii转换为double浮点数后再参与运算，运算结果为双精度浮点数300.5



#### 10.2 强制类型转换

C 语言提供了可**显式指定类型转换的语法**，通常称之为强制类型转换

```c
// (目标类型)表达式;
int a=4, b=3;
double dd;
dd=(double)(a/b);  	// dd的结果是1.000000。
dd=((double)a)/b;   // dd的结果是1.333333
```

 

### 11 C语言结构体🌼

#### 11.1 结构体的概念

在C语言中，使用结构体（struct）来存放一组不同类型的数据

```c
struct 结构体名 {
  结构体成员变量一的声明;
  结构体成员变量二的声明;
  结构体成员变量三的声明;
  ......
  结构体成员变量四的声明;
};
```



结构体是一个集合，**是一种构造的数据类型**

用于描述一个数据集而自定义出来的数据类型

结构体的成员（member）可以是任意类型的变量，也可以是结构体变量

```c
struct st_girl {
  char name[51];   // 姓名
  int  age;        // 年龄
  int  height;     // 身高，单位：cm
  int  weight;     // 体重，单位：kg
  char sc[31];     // 身材，火辣；普通；飞机场
  char yz[31];     // 颜值，漂亮；一般；歪瓜裂枣
};
```



#### 11.2 结构体变量

结构体可以用它来定义变量

```c
struct st_girl queen, princess, waiters, workers;
```



#### 11.3 占用内存的情况

- 理论上讲结构体的各个成员在内存中是连续存放的，和数组非常类似
- 但由于内存对齐机制，**各个成员之间可能存在缝隙**
  - 例如struct st_girl全部成员变量占用的内存是50+4+4+30+30=118，但是结构体占用的内存是120
  - 内存对齐是为了提高寻址效率，典型的空间换时间

- 用sizeof可以得到结构体占用内存在总大小，sizeof(结构体名)或 sizeof(结构体变量名)都可以



#### 11.4 结构体初始化

采用memset函数初始化结构体，全部成员变量的值清零

```c
// 两种写法均可
memset(&queen,0,sizeof(struct st_girl));
memset(&queen,0,sizeof(queen));
```

注意如果把一个结构体的地址传给子函数，子函数用一个结构体指针（如struct st_girl *pst）来存放传入的结构体的地址，那么在**子函数中只能用以下方法来初始化结构体**

```c
memset(pst, 0, sizeof(struct st_girl));
```



#### 11.5 结构体成员变量的访问

采用**圆点`.`运算符**可以访问（使用）结构的成员

结构体成员变量的使用与普通变量的使用相同



#### 11.6 结构体指针

结构体是一种**自定义的数据类型**，结构体变量是内存变量，有内存地址，也有结构体指针

```c
struct st_girl queen;
struct st_girl *pst=&queen;
```

两种手动给结构体指针**申请内存**的方法：

```c
struct st_girl *queen = (struct st_girl *)malloc(sizeof(struct st_girl));	//需要用 free(queen)手动释放
struct st_girl *queen = new(struct st_girl);								//需要用delete(queen)手动释放
```

通过结构体指针可以使用结构体成员

```c
pointer->memberName;
```

->是1种C运算符，习惯称它为“箭头”，有了它，可以通过结构体指针直接使用结构体成员；这也是->在C语言中的**唯一用途**



#### 11.7 结构体的复制

要把结构体变量的值赋给另一个结构体变量，可用C语言提供的memcpy（memory copy的简写）实现内存拷贝功能

```c
void *memcpy(void *dest, const void *src, size_t n);
```

- **src** 源内存变量的起始地址。
- **dest** 目的内存变量的起始地址。
- **n** 需要复制内容的字节数。
- 函数返回指向dest的地址



#### 11.8 结构体作为函数的参数

结构体作为函数的参数时，为了减少开销，最好的办法就是**传递结构体变量的地址**

```c
void setvalue(struct st_girl *pst)
```



### 12 C语言格式化输出🌼

#### 12.1 printf()函数

 **printf() 格式化输出函数**

```c
int printf(const char *format, ...);
```

- printf函数是一个**可变参数函数**
- printf函数的参数的个数和类型都是可变的，每一个参数的输出格式都有对应的格式说明符与之对应.
- 从格式串的左端第 i 个格式说明符对应第 i 个输出参数。



**格式说明符**（ [] 中的为可选项）

```c
%[flags][width][.prec]type
```

- **类型符 type** -- 用于**表示输出数据的类型**

  - %hd、%d、%ld 以十进制、有符号的形式输出 short、int、long 类型的整数。

  - %hu、%u、%lu 以十进制、无符号的形式输出 short、int、long 类型的整数


  - %c 输出字符。


  - %lf 以普通方式输出double（float弃用，long doube无用）。


  - %e 以科学计数法输出double。


  - %s 输出字符串。


  - %p 输出内存的地址。


- **宽度 width** -- 用于**控制输出内容的宽度**

- **对齐标志 flags** -- 用于控制输出内容的对齐方式

  - 缺省或+，输出的内容右对齐（默认）


  - \-输出的内容左对齐。


- **精度 prec** -- 用于控制浮点数输出的精度，即小数点后面保留多少位

```c
// 输出宽度12 小数点后保留2位 输出类型浮点数 默认右对齐
// [      123.50]
printf("[%12.2lf]\n",123.5);   
```



#### 12.2 sprintf()函数

**格式化输出到字符串**

```c
int sprintf(char *str, const char *format, ...);
int snprintf(char *str, size_t size, const char *format, ...);
```

printf是把结果输出到屏幕，sprintf把格式化输出的内容保存到字符串str中

snprintf的n类似于strncpy中的n，意思是只获取输出结果的前n-1个字符，不是n个字符



C语言没有提供把整数和浮点数转换为字符串的库函数，而是采用sprintf和snprintf函数格式化输出到字符串。

```c
// 格式化输出到str中 
sprintf(str,"%d,%c,%f,%s",10,'A',25.97,"一共输入了三个数。");
printf("%s\n",str);
```

```c
// 格式化输出到str中，只截取前7个字符
snprintf(str,8,"%d,%c,%f,%s",10,'A',25.97,"一共输入了三个数。");
printf("%s\n",str);
```



### 13 C语言main函数的参数🌼

程序运行的时候，有些需要带参数，有些不带参数，**例如linux操作系统的命令，它们本质上就是C程序**

Linux命令中，没有参数的不多

```bash
pwd  	#显示当前目录
clear  	#清屏
```

大部Linux命令是带参数的

```bash
cp  book1.c book2.c
mkdir -r /tmp/dname
```

- 在实际开发中，main函数一般都需要参数，没有参数的情况极少。
- main函数的参数是从命令提示符下执行程序的时候传入。



#### 13.1 main函数的参数

main函数有三个参数，argc、argv和envp，它的标准写法如下：

```c
int main(int argc,char *argv[],char *envp[])
```

- int argc，存放了命令行参数的个数。

- char *argv[]，是个**字符串数组**，每个元素都是一个字符串指针，指向一个字符串，即命令行中的每一个参数

- char *envp[]，也是一个字符串的数组，这个数组的每一个元素是指向一个环境变量的字符指针，应用场景不多


注意几个事项

1）**argc的值是参数个数加1**，因为**程序名称是程序的第一个参数**，即argv[0]，eg：argv[0]是./app

2）main函数的参数，不管是书写的整数还是浮点数，全部被认为是字符串

3）参数的命名argc和argv是程序员的约定，也可以用argd或args，但是不建议这么做



#### 13.2 C语言的规范写法

先假设程序执行都是有参数的，也就是说main函数都有参数，那么优秀的程序员会在程序中**提供说明文字**

若执行程序时提供的参数与设计不符，显示程序的使用说明，说明文字应该包括程序的功能和全部参数的解释



### 14 C语言的动态内存管理🌼

动态内存管理指**在程序运行过程中动态的申请和释放堆内存空间**

- 需要时随时开辟，不需要时随时释放

- 通过手动调用库函数malloc和free实现




#### 14.1 malloc()

```c
void *malloc(unsigned int size)；
```

- malloc的作用是向系统申请一块大小为size的连续内存空间

- 申请失败，函数返回0，申请成功，返回成功分配内存块的起始地址

- malloc的返回值的地址的基类型为 void，需要强制类型转化

```c
int *pi=(int *)malloc(sizeof(int));
```



#### 14.2 free()

```c
void free(void *p);
```

- free的作用是释放指针p指向的动态内存空间

- p是调用malloc函数时返回的地址
- 调用前检查p，不可重复释放或释放nullptr



#### 14.3 内存耗尽

用动态分配内存技术的时候，分配出来的内存必须及时释放，否则会引起系统内存耗尽

内存问题是C程序员的主要问题之一，是初学者的恶梦



#### 14.4 野指针

野指针是指向无效内存地址的指针

与空指针不同，野指针无法通过简单地判断是否为 NULL避免，而只能通过养成良好的编程习惯来尽力减少



**内存指针变量未初始化**

指针变量刚被创建时不一定会自动初始化成为空指针（与编译器有关），它的缺省值是可能随机的即随便乱指

指针变量在创建的同时**应当被初始化**

- 要么将指针的值**设置为0**

- 要么让它**指向合法的内存**



**内存释放后之后指针未置nullptr**

调用free函数把指针所指的内存给释放掉，释放内存后应立即将指针置为0。

```c
free(pi);
pi = nullptr;
```



### 16 C语言的目录操作🌼

#### 16.1 获取当前工作目录

调用getcwd()函数可以获取当前的工作目录

```c
char *getcwd(char * buf,size_t size);
```

- 函数功能是把当前工作目录存入buf中
- 如果目录名超出了参数size长度，函数返回NULL
- 如果成功，返回buf



#### 16.2 切换工作目录

```c
int chdir(const char *path);
```

类似shell中使用cd命令切换目录一样，C程序中使用chdir函数来改变工作目录

- 返回0表示切换成功

- 返回非0表示失败

  



#### 16.3 目录的创建和删除

和shell相同，C程序中用mkdir/rmdir函数来创建/删除目录

mkdir/rmdir属于系统调用



**mkdir()创建目录**

```c
int mkdir(const char *pathname, mode_t mode);
```

mode固定填0755，注意，0不可省略，它表示八进制：

```c
mkdir("/tmp/aaa",0755);   // 创建/tmp/aaa目录
```



**rmdir()删除目录**

```c
int rmdir(const char *pathname);
```



### 17 C语言时间操作🌼

**unix时间纪元**

-  UNIX操作系统采用**1970年1月1日作为UNIX的纪元时间**
-  将从1970年1月1日开始经过的秒数**用一个整数存放**
-  1970年1月1日0点作为计算机表示时间的中间点，向左和向右偏移都可以得到更早或者更后的时间



#### 17.1 time_t别名

在C语言中，用time_t来表示时间数据类型，它是一个long（长整数）类型的别名，在time.h文件中定义

```c
typedef long time_t;
```



#### 17.2 time 获取当前second级精度时间

time函数返回从1970年1月1日0时0分0秒到现在的**秒数**

time函数是C语言标准库中的函数，在time.h文件中声明

```c
time_t time(time_t *t);
```

time函数有两种调用方法：

```c
time_t tnow;
tnow = time(NULL); 	// 将空地址传递给time函数，并将time返回值赋给变量tnow
time(&tnow);       	// 将变量tnow的地址作为参数传递给time函数
```



#### 17.3 tm结构体

time_t只是一个长整型，不符合人类使用习惯，需要转换成可以方便表示时间的**tm结构体**

tm结构体在time.h中声明

```c
struct tm {
  int tm_sec;     // 秒：取值区间为[0,59]
  int tm_min;     // 分：取值区间为[0,59]
  int tm_hour;    // 时：取值区间为[0,23]
  int tm_mday;    // 日期：一个月中的日期：取值区间为[1,31]
  int tm_mon;     // 月份：（从一月开始，0代表一月），取值区间为[0,11]
  int tm_year;    // 年份：其值等于实际年份减去1900
  int tm_wday;    // 星期：取值区间为[0,6]，其中0代表星期天，1代表星期一，以此类推
  int tm_yday;    // 从每年的1月1日开始的天数：取值区间为[0,365]，其中0代表1月1日，1代表1月2日，以此类推
  int tm_isdst;   // 夏令时标识符，该字段意义不大，我们不用夏令时。
};
```

这个结构定义了年、月、日、时、分、秒、星期、当年中的某一天、夏令时。用这个结构体可以很方便的显示时间。



#### 17.4 localtime tm转time_t

localtime函数**用于把time_t表示的时间转换为struct tm结构体表示的时间**

```c
 struct tm * localtime(const time_t *);
```

- 函数返回struct tm结构体的地址

- struct tm结构体包含了时间的各要素，可以用格式化输出printf函数，把struct tm结构体输出为可读时间



#### 17.5 mktime time_t转tm

mktime 函数用于把struct tm表示的时间转换为time_t表示的时间

```c
time_t mktime(struct tm *tm);
```



#### 17.6 sleep/usleep 程序睡眠

sleep/usleep函数**用于实现程序秒/微秒级的挂起**

- 需要包含unistd.h头文件
- **程序sleep期间会交出cpu时间**

```c
unsigned int sleep(unsigned int seconds);
int usleep(useconds_t usec);
```



#### 17.7 gettimeofday 获取当前μs级精度时间

gettimeofday**精度高，可以用于程序的计时**

包含在<sys/time.h>头文件

```c
int gettimeofday(struct  timeval *tv, struct  timezone *tz )
```

- 当前的时间存放在tv 结构体中，tz一般置空（NULL）


- 函数执行成功后返回0，失败后返回-1




tv为timeval结构体，在sys/time.h文件中定义

```c
struct timeval {
  long  tv_sec;  // 1970年1月1日到现在的秒。
  long  tv_usec; // 当前秒的微妙，即百万分之一秒。
};

// 使用示例 -- 计算代码时间开销
timeval tstart, tend;

gettimeofday(tstart, NULL);
//To Do Something
gettimeofday(tend, NULL);

long tcost = tstart->tv_usec - tend->tv_usec;
```



## C++🌼

### 1 从C到C++🌼

C++**完全兼容所有C语法**

C++**引入面向对象特性**，是对C的扩展



#### 1.1 C++布尔类型（bool）

C语言并没有彻底从语法上支持“真”和“假”，只是用 0 和非 0 来代表

C++ 新增了 bool 类型（布尔类型），**占用1B空间**

bool 类型只有两个取值，true 和 false



#### 1.2 C++头文件

**C++丰富了标准库**，新增STL

C++同时也**对部分C头文件进行了标准化**



**C++头文件命名**

- **C++标准库的头文件不带副文档名(.h)**
  - #include <vector>


- **C++标准化后的C头文件带c字母前缀**

  - #include <stdio> -> <cstdio>

  - #include <math> -> <cmath>


- **C标准头文件保留，带副文档名(.h)**

  - 为了完全兼容C标准

  - #include <string.h>




**C++头文件引用**

- **<>**表示**优先从系统目录中查找该头文件**，一般为标准库文件
- **""**表示**优先从当前目录中查找该头文件** ，一般为自定义文件



### 2 C++函数重载🌼

**C语言中不允许函数同名**，为了实现几个相似功能的函数，程序员只能设计出多个不同名的函数

C++引入重载机制，解决这一问题



#### 2.1 函数重载的概念

**函数重载**(Function Overloading) -- **名称相同，参数列表不同的一组函数之间构成重载**

- 借助重载，一组功能相近的一函数可以使用同一函数名，方便调用
- 参数列表包括**参数的类型、参数的个数和参数的顺序**，其中一个不同就算作参数列表不同
- **参数名称和函数返回值不能作为重载的依据**
- **重载是一种静态多态**(编译时多态)



#### 2.2 C++ 函数重载实现方法

- 编译器在**编译C++程序时会根据参数列表对函数进行重命名**
- 程序被编译时，**编译器会根据参数列表逐个匹配**，选择对应的函数
- 如果匹配失败，编译器就会报错，这叫做重载决议（Overload Resolution）
- **函数重载仅仅是语法层面的，本质上它们还是不同的函数**，占用不同的内存，入口地址也不一样



### 3 C++的类和对象🌼

#### 3.1 C++ struct结构体新特征

**C语言中struct结构体不允许有函数**

C++中struct结构体的可以有函数函数



#### 3.2 面向对象编程 OOP

OOP -- Object Oriented Programming

- 类是一个通用的抽象概念， C++、Java、C#、PHP 等语言都支持类和对象，这些语言**被称为面向对象的编程语言**


- C语言不支持类和对象的概念，**被称为面向过程的编程语言**


- C++中在函数的基础上，多了一层封装，就是类（class）
- class的引入**使得C++具备了面向对象编程的3大特性 -- 封装、继承、多态**



### 4 C++类详解🌼

#### 4.1 类成员的访问权限

- C++通过 public、protected、private 三个关键字来控制成员变量和成员函数的访问权限，它们分别表示公有的、受保护的、私有的
- **在类的内部，即类的成员函数中**，无论成员被声明为 public、protected 还是 private，都是可以互相访问的，**没有访问权限的限制**
- 在类的外部（定义类的代码之外），只能通过对象访问public的成员，不能访问 private、protected 属性的成员。
- private 关键字的作用在于更好地隐藏类的内部实现
  - 该向外暴露的接口（能通过对象访问的成员）都声明为 public
  - 不希望外部知道、或者只在类内部使用的、或者对外部没有影响的成员，都建议声明为 private。


```c++
class Info {
public:
	string getName();	// 成员函数接口对外暴露
    void setName(const string& name);
private:
    string name;		// 成员变量对外隐藏
};
```



#### 4.2 成员变量的命名

成员变量大都以m开头，这是约定成俗的写法

- 以m开头既可以一眼看出这是成员变量

- 可以和成员函数中的参数名字区分开

```c++
void Info::setName(const string& name) {
    m_name = name;
}
```



#### 4.3 构造函数 constructor

**构造函数名和类名相同，且没有返回值**

```c
Classname(parameter list);	// 构造函数
```

- **构造函数没有返回值**，不管是声明还是定义，函数名前面都不能出现返回值类型，即使是 void 也不允许。

- **构造函数有参数l列表，允许重载**。一个类可以有多个重载的构造函数，创建对象时根据传递的参数来判断调用哪一个构造函数

- 构造函数在实际开发中会大量使用，用来做一些**初始化工作**，对成员变量进行初始化等，注意，不能用memset对整个类进行初始化




#### 4.4 析构函数

**析构函数字为`~类名`，也没有返回值**

```c
~Classname();	// 析构函数
```

- 析构函数**在对象销毁时自动执行**，用于进行清理工作，例如释放分配的内存、关闭打开的文件等，这个用途非常重要，可以防止程序员犯错
- **析构函数没有参数，所以不会重载**，一个类只能有一个析构函数
- 析构函数**没有返回值**，不管是声明还是定义，函数名前面都不能出现返回值类型，即使是 void 也不允许



### 5 C++引用🌼

**引用就是变量的别名**，对引用的操作与对变量直接操作完全一样

引用的声明**`datatype& 引用名 = 目标变量名;`**

**引用常用于传递函数参数**

- 本质是传递地址
- 数据量大时可以提高传参效率

```c++
void setName(const string& name);
```



### 7 C++字符串string🌼

**string为动态字符串**，大小可变

- string是STL(Standard Template Library 标准模板库)中的定义的类
- string会随存放字符的长度自动伸缩，程序员不必担心内存溢出的问题
- string类通过`string.c_str()`转化为C风格字符串



#### 7.1 string的声明

为了在程序中使用string类，必须包含头文件 <string>

```c++
#include <string>
std::string str1;
```



#### 7.2 string的重载的操作符

**string支持用运算符直观操作**，通过操作符重载实现

- 可以用 = 直接赋值

- 可以用 ==、>、<、>=、<=、和!= 比较字符串

- 可以用 + 或者 += 操作符连接两个字符串

- 可以用 [] 获取指定位置的字符，类似数组



#### 7.3 string转C风格字符串

```c++
const char *c_str();
```

string类采用**动态分配内存的方式来存放字符串**，c_str函数返回这个字符串的地址。

```c++
std::string str1 = "hello world";
char str2[31] = {0};
strcpy(str2,str1.c_str());
```



### 9 C++动态内存管理🌼

**C++用运算符new/delete实现堆内存空间的申请/释放**

对应C语言中的malloc/free

```c
datatype *pointer = new datatype;
delete pointer;
```

- datatype可以是C语言的**基本数据类型**，也可以是**类**


- pointer是一个指针，指向new返回的地址


- 如果new出来的是类，相当于创建对象，**所以会调用构造函数**，**delete的时候也会调用析构函数**




### 10 C++继承🌼

C++继承允许程序员根据一个类来定义另一个类

- 被继承的类称为基类或父类

- 新建的类称为派生类或子类
- 子类具有父类特性



#### 10.1 继承语法

定义一个派生类，需要指定它的基类

```c++
class <派生类名>:<继承方式> <基类名> {
  // 派生类类体
};

class goldfish : public fish {	// goldfish类继承fish类
	......
};
```

- 继承方式有 public、protected 、 private
- **默认为 private继承**，struct默认public继承，注意区分
- 通常使用public继承



#### 10.2 多继承

多继承即一个派生类可以有多个基类，它继承了多个基类的特性

```c
class <派生类名>:<继承方式1><基类名1>, <继承方式2><基类名2>, …{
  // 派生类类体
};
```



### 11 C++多态🌼

**C++多态指同一函数具有多种行为**

- **静态多态**，也称编译时多态，通过函数重载、Template实现
- **动态多态**，也称运行时多态，通过子类重写父类虚函数实现



#### 11.1 静态多态

- 静态多态通过函数重载、Template模板编程实现
- 静态多态是编译器在**编译期间**完成的
- 编译器会根据实参类型来**选择调用合适的函数**
- 如果没有适配的函数就会发出警告或者报错



#### 11.2 动态多态

- **基类指针指向不同派生类对象时**，调用同一个方法由不同的表现
- 派生类需重写基类虚函数



#### 11.3 虚函数

虚函数是在基类中使用关键字 virtual 声明的函数，在派生类中可以重写虚函数

父类和子类都要virtual关键字修饰

```c++
virtual returnTpye  funcname(parameters);	// 虚函数定义格式

class Foo {
public:
	virtual int Show();	// 虚函数实例    
}
```



#### 11.4 纯虚函数

纯虚函数是基类中只有声明，没有定义的函数，需要**在派生类中去实现函数的定义**

```c
virtual int Show() = 0; // 申明一个纯虚函数。
```



#### 11.5 C++ 接口(抽象类)

- 接口描述了类的行为和功能，**是标准和规范，不需要完成类的功能实现**
- C++ 接口是用抽象类来实现的，如果类中至少有一个函数被声明为**纯虚函数**，则这个类就是**抽象类**
- **抽象类不可实例化**，抽象类的派生类需要**实现所有纯虚函数后才能实例化**
- 可用于实例化对象的类被称为**具体类**



## STL标准模板库

### 1 C++标准库与STL

- C++标准库包含STL(Standard Template Library 标准模板库)，占标准库70%左右

- STL采用**泛型编程**(GP Generic Programming)实现，

- 泛型编程指用**Template(模板)为主要工具来编写程序**

![image-20220720165245306](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720165245306.png)



### 2 STL组成部件

**STL的6大部件Components**

- 容器 Containers，容器本身

- 分配器 Allocators，负责内存管理
- 算法 Algorithm，如查找算法、排序算法
- 迭代器 Iterators，迭代器是一种泛化的指针
- 仿函数 Functors，如less\<int>、greater\<int>
- 适配器 Adapters，起“粘结”作用，包含容器适配器、迭代器适配器、仿函数适配器

![image-20220720171341769](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720171341769.png)



**STL全部容器遵循前开后闭原则**

迭代器有效范围**[ .begin(), .end() )**

![image-20220720171519578](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720171519578.png)



### 3 容器分类

![image-20220720171740884](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720171740884.png)

|                            |   分类名   |                  对应STL容器                  |
| :------------------------: | :--------: | :-------------------------------------------: |
|  **Sequence Containers**   |  顺序容器  | array vector string list deque priority_queue |
| **Associative Containers** | 关联式容器 |                    map set                    |
|  **Unordered Containers**  |  散列容器  |          unordered_map unordered_set          |



### 4 OOP VS GP

- OOP(Object Oriented Programming ) -- **面向对象编程**

- GP(Generic Programming) -- **泛型函数编程**
  - Generic 通用的、泛化的
  - 通过Template实现

- **OOP将datas和method关联在一起**
- **GP将datas和method解耦**

```c++
using namespace std;

unordered_set.find()					// OOP思想，调用unordered_set类中的find()函数
::sort(vector.begin(), vector.end());	// GP思想，::sort()可以对不同数据类型进行排序
```



### 5 C++操作符重载

- **C++用operator关键字**实现**操作符重载(Operator Overloading)功能**
- operator和运算符一起使用，表示一个运算符重载函数
- 可将operator和运算符（如operator<）**视为类的一个成员函数名**，调用时省去operator关键字
- 运算符是左调用即**运算符左边的对象调用了运算符**
  - **左侧对象的`this`指针**作为隐藏参数传入函数
  - 对于单目运算符`++、--`，不需要传入其它参数
  - 对于双目运算符`+、-、==、<`，**右侧对象作为参数传入**


```c++
class FOO {
public:
    // 重载Foo类的<运算符，第二个const用于修饰隐含的this指针
    bool operator<(const Foo& obj) const {
        return this->val < obj->val;
    }
    int val;
};

int main(void) {
    Foo a, b;
    if(a < b) {	// 相当于a调用了operator<函数，b作为参数传入
        // ...
    } 
}
```



### 6 C++ Template模板编程

**C++模板编程通过template关键字实现**，分为**类模板、函数模板**

- 用**`template <typename T>`**作为占位符，**T代表一种抽象的数据类型**
  - **`template <class T>`**也可作占位符
  - T可以是C++标准数据类型，也可以是自定义的类
- 调用函数模板时，编译器自动做**实参推导**(argument deduction)，**无需手动指定类型**
- 类模板需要在使用时**手动指定模板类型**（即<>中的T）

![image-20220720231019470](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720231019470.png)

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720231458905.png" alt="image-20220720231458905" style="zoom:50%;" />



### 7 分配器allocators

**分配器用于申请/释放容器空间**

所有申请内存的操作都底层都是`malloc`和`free`

![image-20220720180603530](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720180603530.png)



### 8 迭代器 iterator

- iterator是聪明的指针，**内部有很多重载和实现**

- iterator指向的是容器节点，但**对*做了重载**，本来是取整个节点，变成取节点数据

- **algorithm使用iterator访问iterator**



**iteration分类**

|                   |      名称      | 支持的操作（向下兼容） |                  对应容器                   |
| :---------------: | :------------: | :--------------------: | :-----------------------------------------: |
| **Random Access** | 随机访问迭代器 |        it += n         |            vector、string、deque            |
| **Bidirectional** |   双向迭代器   |         ++ --          |               list、set、map                |
|    **Forward**    |   前向迭代器   |           ++           | forward_list、unordered_set、unorderded_map |
|      **Non**      |    无迭代器    |           \            |        stack、queue、priority_queue         |



algorithm使用iterator时，**要求iterator回答5种associated type**，该能力**通过traitor实现**

```c++
typedef std::bidirectional_iterator tag iterator_category;	// 迭代器类型
typedef T value_type;				// 值类型
typedef T* pointer;					// 指针类型
typedef T& reference;				// 引用类型
typedef ptrdiff_t difference_type;	// 差值类型
```

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720222218714.png" alt="image-20220720222218714" style="zoom: 67%;" />



### 9 STL容器结构

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720180903329.png" alt="image-20220720180903329" style="zoom:80%;" />

- **继承** -- C++子类继承基类的机制

- **复合** -- 在B类中实例化A类以调用A类方法的做法，即**B类中拥有一个A类**





### 10 vector容器

- vector是一个**封装了动态数组的顺序容器**（Sequence Container），**初始容量为0**

- vector对外表现为地址连续数组，**其底层元素也确实存储于连续数组中**

- vector类只拥有 start finish end_of_storage 这3个成员变量
- vector**尾添元素时间复杂度O(1)**，任意位置**insert元素时间复杂度O(n)**

![image-20220720233557739](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720233557739.png)



**vector扩容**

- vector.size()=vector.capacity()时，继续向vector中添加元素，会**触发vector扩容**

  - vector扩容时先**申请2 × capacity的大小的连续内存空间**

  - 然后将vector中旧元素全部拷贝到新空间中

  - 最后释放旧vector空间

- vector扩容的时间复杂度均为O(n)，代价较高

- 向空vector中连续尾添20个元素，vector.capacity()的数值变化

![image-20220720235832192](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720235832192.png)



### 11 list容器

- list是一个**封装了双向链表的顺序容器**

- 任意位置增删元素时间复杂度O(1)，但不支持随机访问

![image-20220720235345402](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220720235345402.png)









### 12 deque容器

deque双端队列是一种顺序容器，支持随机访问，**头/尾插、头/尾删的时间复杂度均为O(1)**

![image-20220721003931922](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220721003931922.png)

- **deque底层分段连续**，通过iterator的算法模拟，**对外表现为连续**
- dequeue底层**采用两级存储结构**
  - 表头(map)是一个vector，存储buffer指针
  - **1个buffer的大小为512B**


![image-20220721004250996](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220721004250996.png)

### 13 stack容器 & queue容器

stack栈和queue队列容器**底层均为deque**

**stack和queue不允许遍历，也不提供迭代器**

![image-20220721004646950](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220721004646950.png)



### 14 set容器 & map容器

- set和map**均为关联容器**，底层**基于rb_tree实现**，**内部有序**

- **增删改查时间复杂度O(logn)**，内置find()成员函数

- **map对 operator[] 进行了重载**
  - 如果key存在，则以引用的方式返回key对应的value值
  - 如果key不存在，**则插入该key，并赋值**

```c++
map <int,string> m;
m[1] = "hello";		// 如果键值1存在，则修改其data为"hello"
					// 如果不存在，则先插入键值与valuse默认值组成的kv对，再修改value为"hello"
```



### 15 unordered_set容器 & unordered_map容器

unordered_set和unordered_map**均为散列容器**，底层**基于hashtable实现**，内部无序

**增删改查时间复杂度O(1)**，内置find()成员函数



**hashtable中的数据最终存储于vector中**

- 调用哈希函数获取key对应哈希值
- 采用除留取余法获取value在vector中的下标
- 下标冲突时，采用链接地址解决



**rehash 打散**

- 当元素总数=数组大小时触发打散操作，**保证每个bucket平均存放的元素不及1个**

- 需要**将vector扩容至最接近原size()2倍的质数**，并重新计算存储位置

- GNU库初始bucket数目为53，扩充时查表

![image-20210707222705787](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210707222705787.png)

## C++库函数🌼

### A



### B



### C



### D



### E



### F

#### floor()	ceil()	round() 取整函数🌼

```c++
#include <cmath>
floor(4.3) = 5;	// 向下取整
ceil(4.8) = 4;	// 向上取整
round(4.4) = 4;	// 四舍五入
round(4.5) = 5；
```



#### find()

**string.find()**

```c++
size_t find (const string& str, size_t pos = 0) const;	// pos 用于指定查找位置

string s1 = "apple";
size_t idx = s1.find("pl");	// idx = 2
```

- 返回值为**首次找到 str 的字符下标**

- 查找失败，返回**string::npos**，`static const size_t npos = -1;`



### G

#### gcvt() 浮点数转C风格字符串🌼

```c++
char *gcvt(double value, int ndigit, char *buf);

char volValue[100] = {0};
float num = 80.56;
gcvt(num, 10, volValue);	// volValue = "80.55999756"
```

- value：浮点数
- ndigit：精度位数
- buf：输出字符串指针








### H



### I

####  INT_MAX和INT_MIN🌼

int类型上下界，-2147483648到2147483647 

```c
#include<limits.h>

#define INT_MAX 2147483647
#define INT_MIN (-INT_MAX - 1)
```



#### isalnum() 判断字符是否为数字/字母🌼

数字/字母范围 **0-9|| a-z || A-Z**

```c++
int isalnum(int c);

char value = '9';
int res = isalnum(value);	// 实测返回值为8（非零值）
```

- c是数字/字母，则**返回非零值**，否则返回 0



### J



### K



### L



### M

#### memset() 内存填充指定字符🌼

memset()用于填充字符 c 到参数 str 所指向地址的前 n 个字符

```c++
#include <string.h>

void *memset(void *str, int c, size_t n)
```

- str -- 指向要填充的内存块
- c -- 要被设置的值。该值以 int 形式传递，**实际以char形式填充**
- n --  要被设置为该值的字符数

memset**以字节为单位填充**

通常用于**内存清零**（**各种类型的数组、结构体均适用**）

```c++
int nums[100];
memset(nums, 0, sizeof(nums));		// 将nums之后的400Byte全部置0，得到100个(int)0
```

除清零外**只能用于**char数组赋值，或int数组置-1（邪教用法）

```c++
char str[100];
memset(str, 'a', sizeof(str));		// 正确赋值100个(char)‘a’

int nums[100];
memset(nums, -1, sizeof(nums));		// 正确赋值100个(int)-1，实际上是赋值了400个1111111（-1补码）

memset(nums, 5, sizeof(nums));		// 错误用法，无法得到100个(int)5
```



#### memcpy() 拷贝缓冲区🌼

```c++
void *memcpy(void *dst, const void *src, size_t n)
```

- str1 -- 指向用于存储复制内容的目标数组，类型强制转换为 void* 指针。

- str2 --  指向要复制的数据源，类型强制转换为 void* 指针。

- n -- 要被复制的字节数。



### N



### O



### P

#### perror()打印错误信息 & exit()终止线程🌼

**perror()** -- **用于打印错误信息**，并输出到stderr(标准错误)

```c++
void perror ( const char * str );
    
pFile=fopen ("unexist.ent","rb");
if (pFile==NULL)
perror ("The following error occurred");

//输出结果
//The following error occurred: No such file or directory
```



**exit()** -- **终止当前线程**

```c++
void exit (int status);

exit(0);	//正常退出	EXIT_SUCCESS 0
exit(1);	//异常退出	EXIT_FAILURE 1
```







### Q



### R



### S

#### sprintf() 输出至格式化数据至Cstring🌼

```c++
int sprintf ( char * str, size, const char * format, ...);
sprintf(str, "My name is %s, I'm from %s.", "FengGuo", "China");	// str被赋值为："My name is FengGuo, I'm from China."
```

- str -- 目标字符串。

- format -- 格式化成字符串。

- **`...`** -- **可变参数**



#### ::stable_sort() 稳定排序🌼

```c++
void stable_sort ( RandomAccessIterator first, RandomAccessIterator last);					// 默认升序
void stable_sort ( RandomAccessIterator first, RandomAccessIterator last, Compare comp);	// 指定比较方式

vector<Restaurants> sortedRes;
::stable_sort(sortedRes.begin(), sortedRes.end(), idCmp);
```

内部基于归并排序实现



#### strcmp() & string::compare() 字符串比较

strcmp() -- **C风格字符串比较**

- 返回值 < 0，str1字典序小于str2
- 返回值 = 0，str1与str2相同（**核心用法**）
- 返回值 > 0，str1字典序大于str2

```c++
int strcmp(const char* str1，const char* str2);

char str1[15];
char str2[15];
strcpy(str1, "abcdef");			// can not use =
strcpy(str2, "ABCDEF");
int ret = strcmp(str1, str2);	// ret > 0
```



string::compare()：**STL string比较**

- 返回值 < 0，str1字典序小于str2
- 返回值 = 0，str1与str2相同（**核心用法**）
- 返回值 > 0，str1字典序大于str2

```c++
int compare (const string& str) const;

string str1 = "abcdef";
string str2 = "ABCDEF";
int ret = str1.compare(str2);		// ret > 0;
```





### T



### U



### V



### W



### X



### Y



### Z







### 数字/字符串互转API🌼

**a -- alphanumeric(字母数字)**



#### atof() Cstring转浮点数🌼

```c++
double atof(const char *nptr);
    
char szRecvBuff[100] = "12.2"; 
double current = atof(szRecvBuff);	// current = 12.2
```

- nptr -- **字符串输入**，支持`123.	12.3	0.12	1.23E4`等格式



#### atoi()  Cstring转整数🌼

```c++
int atoi(const char *nptr)
    
char szRecvBuff[100] = "12absd"; 
int current = atof(szRecvBuff);		// current被赋值为12（前2个有效数字）
```

- nptr -- **C风格字符串输入**



#### stof() std::string转浮点数🌼

```c++
float stof (const string&  str, size_t* idx = 0);
    
string s = "12.2asdf"; 
float current = stof(s);	// current被赋值为12.2
```

- **str -- 字符串输入**，支持`123.	12.3	0.12	1.23E4`等格式



#### stoi() std::string转整数🌼

```c++
int stoi (const string&  str, size_t* idx = 0, int base = 10);
    
string s = "67asdf"; 
int current = stoi(s);	// current被赋值为67

// 日期提取 "1998/8/24"
size_t index;					// 注意类型
int year = stoi(date, &index);	// index=4
date = date.substr(index+1);	// date="8/24"
int month = stoi(date, &index);	// index=1
date = date.substr(index+1);	// date="24"
int day = stoi(date);
```

- **str -- 字符串输入**
- idx 是 str 中有效数字转化后的**下一位**
- base -- 进制



#### to_string() 数字转std::string🌼

```c++
string to_string (int val);
string to_string (long val);
string to_string (long long val);
string to_string (unsigned val);
string to_string (unsigned long val);
string to_string (unsigned long long val);
string to_string (float val);
string to_string (double val);
string to_string (long double val);

int value = 12;
string str = to_string(value);		// str = "12";
```



### 字符串の骚操作🌼

#### strchr() Cstring字符查找🌼

```c++
char *strchr(const char *str, int c);
    
char s[] = "12+8i";
int real = atoi(s);			// 提取实部	real=12
char* p = strchr(s, '+');	// 找到 + 位置
int img = atoi(p+1);		// 利用查找到的p提取虚部	img=8
```

- str：要被检索的 C 字符串
- c：在 str 中要搜索的字符
- 如找到，返回在c在字符串中**第一次出现的地址**
- 如未找到，返回nullptr



#### find() std::string字符查找🌼

```c++
size_t string::find (char c, size_t pos = 0);         // 查找字符串[pos, end)范围内第一次出现c的位置

string s = "12+8i";
int real = stoi(s);					// 提取实部	real=12
int pos = s.find('+');				// 找到 + 位置
int img = stoi(s.substr(pos+1));	// 利用查找到的p提取虚部	img=8
```

- c -- 待查找字符
- pos -- 查找起始位置
- 如找到，返回在c在字符串中**第一次出现的下标**
- 如未找到，**返回-1**



#### substr() 获取子串🌼

```c++
string s = "I'm Zhu Wan Dong";
string name = s.substr(4);		// name = "Zhu Wan Dong"
```



## g++ & makefile & CMakeList

#### 1 VSCode详解

**VScode本质上只是一个文本编辑器**

- 其includePath默认包含
  - **`/usr/include/`** -- C/C++头文件位置
  - **`${workspaceFolder}/**`** -- 源文件当前所在位置
- 其余位置需要在**`.vscode/c_cpp_properties.json`**中配置
- 以上配置只是保证**在编写code的时候，能正确找到头文件**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220216020526808.png" alt="image-20220216020526808" style="zoom: 80%;" />





#### 2 g++详解

**g++本质是一个应用程序**，用户安装后

- 在**`/usr/bin/`**目录下出现了**`g++`**，这意味着可以在bash中用**`g++ main.cc -o app`**编译c文件
- 在**`/usr/include/`**目录下出现了stdio.h等C头文件，**这是g++暴露给用户开发的接口**

```bash
sudo apt update			# 获取最新软件更新 
sudo apt install g++	# 安装g++
						# 如果出现连接失败的错误，则需要修改nameserver
```



**开发应用的一般步骤**（以mysql为例）

- **安装mysql应用程序 mysql-server**

  - 完成此步后，在**`/usr/bin/`**目录下出现了**`mysql`**，这意味着可以在bash中用**`mysql -uroot -p`**进入mysql服务


  ```bash
  sudo apt update					# 获取最新软件更新 
  sudo apt install mysql-server	# 安装mysql-server
  ```

  ![image-20220216023052818](E:/ShareFile/C++后端技术栈/g++&makefile&CMakeList.assets/image-20220216023052818.png)

- 但安装好mysql-server后，并不能直接开发mysql，**因为没有/usr/include配套的头文件作为接口**

- **安装mysql开发包**

  - 开发包安装完成后，**`/usr/include/mysql`**被创建，**里面包含了开发mysql需要的接口**

  ```bash
  sudo apt install libmysqlclient-dev		# 程序编译时链接的库
  ```

  ![image-20220216024133374](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220216024133374.png)




#### 3 makefile & CMakeList

**g++指令用于编译源文件**

- 可直接在终端中执行
- 指令较多时，重复输入不方便
- 需要安装g++编译器



**Makefile文件是一系列g++指令的集合**

- 用make指令在终端调用，一次性执行多条g++指令
- 规则复杂
- 通常置于 projRootDir/bulid 文件夹内



**CMakeList.txt**

- 规则简单
- 用cmake dir 在终端调用，生成Makefile
- 所在目录为**顶层目录**（**变量PROJECT_SOURCE_DIR、CMAKE_SOURCE_DIR的值**）



**实际开发流程**

1. 在项目顶层目录（projRootDir）下编写好CMakeList.txt
2. 创建并cd至bulid目录下
3. 终端调用`cmake ..`
4. 终端调用`make`



#### 4 链接过程详解

**静态库的生成**

```c
//1.先写出相应的.h文件和对应的.c文件
//2.编译.c文件
//3.使用ar工具将.o文件归档生成.a静态库文件
[root@localhost linux]# ls
add.c add.h main.c sub.c sub.h
[root@localhost linux]# gcc -c add.c -o add.o
[root@localhost linux]# gcc -c sub.c -o sub.o
//生成静态库
//命名一般开头为lib
[root@localhost linux]# ar -rc libmymath.a add.o sub.o
//ar是gnu归档工具，rc表示(replace and create)
//测试目标文件生成后，静态库删掉，程序照样可以运行。
```



**动态库的生成**

```c
//fPIC:产生位置无关码(position independent code)
[root@localhost linux]# gcc - fPIC -c sub.c add.c
//shared:表示生成共享库格式
//生成静态库
//命名一般开头为lib
[root@localhost linux]# gcc -shared -o libmymath.so *.o
[root@localhost linux]# ls
add.c add.h add.o libmymath.so main.c sub.c sub.h sub.o
//原文链接：https://blog.csdn.net/weixin_42458272/article/details/106193786
```



**库文件的搜索**

- 库名在编译时用 **-l** 指定

- 库路径

  - **第一优先级**：从**-L** 指定库路径中找库名（从左往右，依次查找编译命令中指定的路径）

  - **第二优先级**：由**环境变量**指定的目录（**LIBRAY_PATH**）

  - **第三优先级**：由**系统指定的目录**
    - /usr/lib	
    - /usr/local/lib

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220223225848330.png" alt="image-20220223225848330" style="zoom: 80%;" />



**g++编译与CMake对应**

|      功能      |         g++指令         |                   CMake指令                   |
| :------------: | :---------------------: | :-------------------------------------------: |
| 添加头文件目录 |     -I（大写的 i ）     |              include_directories              |
| 添加库文件目录 |           -L            |               link_directories                |
|    链接库名    |     -l （小写的 L）     |             target_link_libraries             |
|  增加编译参数  | -Wall -std=c++11 -O2 -g | add_compile_options(-Wall -std=c++11 -O2 -g ) |
|   生成静态库   |     -c<br />ar -rs      |        add_library(name SHARED ${SRC})        |
|   生成动态库   |   -c<br />g++ -shared   |        add_library(name STATIC ${SRC})        |
| 生成可执行文件 |           g++           |          add_executable(main ${SRC})          |



**CMake变量取值 -- ${变量名}**

- 变量名可以是set自定义的：`set(SRC sayhello.cpp hello.cpp)`即`SRC = sayhello.cpp hello.cpp`
- 也可以是CMake常用变量：`PROJECT_SOURCE_DIR`



## C++知识点收集

#### 001 C++函数缺省值🌼

**函数声明时可以有**缺省(默认)值

**定义时不可以有**缺省值。

```c++
void Rand(int minvalue, int maxvalue, int totalcount, bool brep=true);	// 申明时可缺省合法
void Rand(int minvalue, int maxvalue, int totalcount, bool brep) {		// 定义时不可缺省
	//To Do SMT
}
```



#### 002 生存期 作用域 可见性🌼

**生存期 -- 变量在内存中存在的时间区间**

- 局部变量 -- [定义位置, 出作用域位置]，局部变量出作用域后被操作系统自动回收
- 全局变量 -- 与程序生存期同步
- 堆变量 -- [new位置, delete位置]，堆变量需手动申请/释放



**作用域 -- 变量的有效范围**

- **全局作用域** -- 全局可见，如全局变量
- **文件作用域** -- 仅当前文件可见，如static修饰的全局变量
- **函数作用域** -- 函数内部的局部变量
- **块作用域** -- {}内



**可见性 -- 变量在其作用域内可见**（可以被使用）



#### 003 static关键字🌼

static是C/C++中的关键字，**用于改变变量的存储方式和可见性**



##### 3.1 static修饰局部变量

- static 告知编译器，**将局部变量存储静态区而非栈上空间**
- static**延长了局部变量的生命周期**，直到主程序运行结束才释放
- static修饰的局部变量**只执行初始化一次**，**并具有“记忆”功能**
  - 静态局部变量**第一次调用时初始化并分配静态区内存**
  - 第i次调用子函数时的static变量值，继承第i-1次调用子函数结束时的static变量值

```c++
void selfCout(){
  // static int i = 0;
  int i = 0;
  cout << i << endl;
  i++;
}

int main(int argc, char *argv[]) {
  for (int ii = 0; ii < 10; ii++) {
    selfCout();
  }

  return 0;
}
```



##### 3.2 static 修饰全局变量

- 静态全局变量**具有文件作用域**，不能在其它文件中访问，即使extern外部声明也不可以

- 可用作**保护数据安全性的方法**



##### 3.3 在类中使用static

**类静态变量**

- static修饰的类成员变量称为**类静态变量**

- 类静态变量**脱离类实例对象存在**
  - 可以通过**类名::变量名**直接引用

  - **类静态变量为类所有**，不属于类实例对象

- 类静态变量**类内申明，类外定义**
  - 类内只是申明类静态变量
  - 类静态变量使用前**必须在类外初始化**，此过程**在编译阶段完成**



**类方法**

static修饰的类成员函数称为**类方法**

- 类方法可以通过**类名::方法名**直接调用
- **类方法无this指针，为所有对象共享**

- 类变量为类本身和所有类对象所**共有**。

```c++
class staticTest{
public:
  staticTest() {};

  static void ifoCout(){		// 类静态方法
      cout << "It's a static class funC" << endl;
  }	
  static int sValue;			// 类静态变量 类内申明
};

int staticTest::sValue = 824; 	// 类外定义、初始化

int main(int argc, char *argv[]){
  staticTest::ifoCout();	//直接用类名调用类方法
  cout << staticTest::sValue << endl;//直接用类名调用类变量

  return 0;
}
```



#### 004 C++格式控制符🌼

| 控制符 |             含义             |
| :----: | :--------------------------: |
|   %d   |             整数             |
|   %u   |          无符号整数          |
|  %lf   |          double输出          |
|   %s   |       C风格字符串输出        |
|   %p   | 十六进制显示地址（含0x前缀） |



#### 005 typedef & struct关键字🌼

**struct**关键字用于定义结构体

```c++
struct 结构体名{
    // 结构体所包含的变量或数组
};

struct BiTNode{
  char data;
  struct BiTNode *lchild;
  struct BiTNode *rchild;
};
```



**typedef**关键字用于为类型起别名

```c++
typedef struct BiTNode BiTNode;	// 使BiTNode和struct BitNode等价
typedef struct BiTNode *BiTree;	// 使BiTree和struct BitNode*等价
```



**typedef和struct组合使用，定义结构体的同时起别名**

```c++
typedef struct BiTNode{
  char data;              	// 存放结点的数据元素。
  struct BiTNode *lchild; 	// 指向左子结点地址的指针。
  struct BiTNode *rchild; 	// 指向右子结点地址的指针。
} BiTNode, *BiTree;
```



#### 006 数值常量INT_MIN DBL_MIN DBL_EPSILON🌼

**INT_MIN INT_MAX**

int类型上下界，**[-2147483648 = -2^31, 2147483647 = 2^31-1]**

```c
#include<limits.h>

#define INT_MAX 2147483647
#define INT_MIN (-INT_MAX - 1)	// -0
```



**DBL_MIN DBL_MAX**

int类型上下界，**[2.22507e-308, 1.79769e+308]**(-2^31 到 2^31-1)

```c++
#include <cfloat>

#define DBL_MAX		__DBL_MAX__
#define DBL_MIN		__DBL_MIN__
```





**DBL_EPSILON**

double类型精度，具体值为<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220722215153830.png" alt="image-20220722215153830" style="zoom:25%;" />

```c++
// 判断浮点数相等
abs(f1 - f2) < DBL_EPSILON	// 正确做法
f1 == f2;					// 错误做法,会引发逻辑错误
```




#### 007 指针的内存大小🌼

指针大小**由操作系统决定**，**与指向的数据类型无关**

- **32位系统下为4字节**
- **64位系统下为8字节**




#### 008 函数指针🌼

函数指针是**用于存放函数地址的指针**

- 函数名本质上就是一个函数指针常量

- **函数类型指返回值和参数列表**

  - 与函数名、形参名无关

  - 函数指针声明格式 -- **`returnType (*指针名)(参数列表);`**

- 函数指针**可作为输入/输出参数使用**，例如回调函数中的函数指针变量，用户可以传入同类型自定义函数

```c++
int func(int a, string s) {			// func是一个函数指针常量
    // ...
}

int main(void) {
    int (*p)(int, string) = NULL;	// 声明函数指针变量p
    p = func;						// 赋值
    p(a, s);						// 通过函数指针调用函数
    
    return 0；
}
```




#### 009 struct VS class🌼

在C语言中，struct 只能包含成员变量，不能包含成员函数（**C不是面向对象的1个体现**）

在C++中，struct 类似于 class，**既可以包含成员变量，又可以包含成员函数**

- class成员**默认private**属性；struct成员**默认public属性**
- class继承**默认private继承**；struct继承**默认public继承**
- **class 可以使用模板**（template，泛型编程）， struct 不能



#### 010 内存高低地址 & 变量的高低位 & 大小端法🌼

- **高地址**：地址编号值大；`0x10c9a88`
- **低地址**：地址编号值小；`0x10c9a78`
  - 动态分配内存时**堆空间向高地址增长**
  - **栈空间向低地址增长**
- **变量高低位**：左高右底

- **大端法**：**用高地址存储变量低位**、用低地址存储变量高位
- **小端法**：反之；弟（低地址）弟（变量低位）小（小端法）



#### 011 指针常量和常量指针🌼

**指针常量** -- 指针本身是常量，不可以改变指针指向的对象

```c++
int *const p = &a;	// const修饰p
p = &b;				// 不允许
*p = 10;			// 可以
```

**常量指针** -- 指针指向的是常量，不可以通过指针改变指向的值

```c++
const int *p = &a;	// const修饰int
p = &b;				// 可以
*p = 10;			// 不允许
```



#### 012 类成员变量的声明与定义🌼

**类内只是申明变量**

类实例化时才定义对象，正式分配内存空间



#### 013 C++自定义比较器🌼

**C++比较器详解**

- ::sort默认**less\<int>()**比较器，对应升序排列

  - 特别注意堆结构同样默认**less\<int>()** ，**但对应大根堆**

- 排序起到的效果是**把排序后的元素按位置先后输入到比较器中，输出都为true**

- sort需要的输入为**less\<int>()** or **StrCmp()**

- set等容器需要的输入为**less\<int>** or **StrCmp**



**3种C++比较器**

- **重载 < 运算符**

```c
struct Str{
    string s;
    bool operator < (const Str &str) const {
        return this->s.length() < str.s.length();
    }
};

vector<Str> strs;
::sort<strs.begin(), strs.end());	// 将Str按短->长排序
```

- **函数比较器**

```c
bool strShorterCmp(const Str str1, const Str str2){
    return str1.length() < str2.length();
}

vector<Str> strs;
::sort<strs.begin(), strs.end(), strShorterCmp);	// 将Str按短->长排序
```

- **类比较器**，需重载**operator()**运算符

```c++
class StrCmp{
public:
	bool operator()(Str s1, Str s2){
        return s1.s.length() < s2.s.length();
    }
};

vector<Str> strs;
::sort<strs.begin(), strs.end(), StrCmp());	// 将Str按短->长排序，注意这里给的类成员函数
set<Str, Strcmp>;							// 创建Str的set对象，注意这里给的是类本身
```




#### 014 前向声明🌼

**前向声明**(forward declare) -- **在定义类之前先声明**

- 适用于**类之间相互包含**的情况


- 例如TinyWebServer中，util_timer类和client_data类为一一对应关系，各自包含1个对方类型的成员变量

```c++
class Edge; //前向申明Edge

class Node{
public:
	/* */
    vector<Edge> edges; // 用到了Edge类
    /* */
};

class Edge{
public:
    /* */
    Node from;  // 用到了Node类
    Node to;    // 终点
    /* */
};
```



#### 015 线程锁竞争🌼

**锁机制用于在多线程程序中保护临界区资源**

- A线程加锁1后，**B线程申请同一把锁1则会被阻塞**

- 是否阻塞**以是否申请同一把锁为标准**，不以是否存在相同的临界区资源为标准
  - B线程不申请锁1则不会阻塞
  - B线程申请锁2也不会阻塞



#### 016 vector手动分配内存🌼

**初始化时分配**

```c++
vector<int> foo(10);		// 指定初始大小，不指定值（编译器默认填充0）
vector<int> foo1(20, 4); 	// 指定初始大小和值
```

**初始化后分配** -- resize()

```c++
foo.resize(10);		// 重设大小，扩大的部分默认填充0
foo.resize(10, 6);	// 重设大小，扩大部分填充为指定值
```



#### 017 STL operator[] & operator<

**operator[] 成员访问**

- 支持的容器：**vector、string、dequeue、map、unordered_map**
  - map、unordered_map有则返回second值，无则插入键，同时可以赋值



**operator= 严格深拷贝容器**

- 支持的容器：**vector**、**deque**、**forward_list**、**list**、**set**、**map**、**unordered_set**、**unorderded_map**
  - **有迭代器的都支持**
- 不支持的容器：**stack**、**queue**、**priority_queue**
  - **没有迭代器的不支持**





#### 018 STL容器内存占用🌼

**STL容器本身占用一定的内存**

- 内存大小**与操作系统位数有关**，与容器内**存放数据的数量\类型无关**

  - vector**本身拥有**三个迭代器 **start**、**finish**、**end_of_storage**

  - **迭代器是特化的指针**，所以 vector 本身大小为 **24Byte**


![image-20211202155905992](E:\ShareFile\C++后端技术栈\编程语言.assets\image-20211202155905992.png)

- STL容器本身大小统计（**64位环境实测**）
  - 所有容器本身大小**均为 8B 的整数倍**
  - string 本体 32Byte（竟然这么大）
  - deque、queue、stack 本体大小**同为 80**，进一步验证了queue、stack不过是**功能阉割了的 deque**
  - forward_list本体怕不是就1个裸迭代器（2333）

|      |      容器      | 本身大小 |
| :--: | :------------: | :------: |
|  1   |     string     |  【32】  |
|  2   |     vector     |  【24】  |
|  3   |      list      |    24    |
|  4   |  forward_list  |    8     |
|  5   |     deque      |  【80】  |
|  6   |     queue      |  【80】  |
|  7   |     stack      |  【80】  |
|  8   | priority_queue |    32    |
|  9   |      set       |    48    |
|  10  |      map       |    48    |
|  11  | unordered_set  |    56    |
|  12  | unordered_map  |    56    |



#### 019 同步和异步🌼

**线程中的同步与异步**

- **同步线程**：当程序1调用程序2时，**程序1停下不动**，直到程序2完成回到程序1来，程序1才继续执行下去
  - 好兄弟，我等你!
- **异步线程**：当程序1调用程序2时，**程序1径自继续自己的下一个动作**，不受程序2的的影响
  - 好兄弟，我先溜了！

**IO中的同步与异步**

- **同步IO**：发送方发出数据后，**等接收方发回响应**以后才发下一个数据包的通讯方式 
- **异步IO**：发送方发出数据后，**不等接收方发回响应**，接着发送下个数据包的通讯方式
- 举例：socket属性中，如果屏蔽naggle算法，则为异步IO

**linux下的信号是异步的**，因为**基于中断实现**，无法确定回调型号处理函数时，主线程的上下文位置



#### 020 运算符优先级🌼

|                          |   优先级别   |                   实例                    |
| :----------------------: | :----------: | :---------------------------------------: |
|      **算数运算符**      |    3-4 级    |               **+ - * / %**               |
|      **移位运算符**      |     5 级     |                 **<< >>**                 |
| **比较运算符【分水岭】** |    6-7 级    |           **> = < >= <= == !=**           |
|       **位运算符**       |   8-10 级    |                **& \| ^**                 |
|      **逻辑运算符**      |   11-12级    | **&& \|\|**<br />**特例 ~** 单目运算，2级 |
|      **赋值运算符**      | 14级（极低） |                   **=**                   |



**易错点在于容易被连用的判断运算符**，连用时才有优先级的问题

尤其注意**位运算符**、**赋值运算符**优先级**小于比较运算符**

```c++
// & 与 <
// 正确写法
if((bitMap[ii] & bitMap[jj]) == 0){ // 位图没有同一个位为 1 的情况出现
    ans = max((int)words[ii].size() * (int)words[jj].size(), ans);
}

// = 与 <
// 正确写法
if ((listen_sock = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP)) < 0) 
        ERR_EXIT("socket()");
// 错误写法，会导致listen_sock没有正确赋值
if (listen_sock = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) < 0) 
        ERR_EXIT("socket()");
```



#### 021 extern关键字🌼

**extern关键字用外部变量**

- 在a.c文件中**全局区定义变量foo**，具备全局可见性
- b.c可以通过 **extern** 声明，直接访问 **foo**

```c++
// b.c
int main(void){
    extern int foo;			// 外部声明
    cout << foo <<endl;
    
    return 0;
}
```



#### 022 STL哈希表支持的数据类型🌼

哈希表**直接支持的类型**

- char、int、double等内置基础类型
- **指针型**
- string



哈希表**不直接支持**

- **vector\<int>**

- **pair<int, int>**

- 直接使用会报错：`错误    C2338    The C++ Standard doesn't provide a hash for this type.`  

- 需要**额外配置hash方法**

  ```c++
  // vector<int> 所需的哈希方法
  struct VectorHash {
      size_t operator()(const std::vector<int>& v) const {
          std::hash<int> hasher;
          size_t seed = 0;
          for (int i : v) {
              seed ^= hasher(i) + 0x9e3779b9 + (seed<<6) + (seed>>2);
          }
          return seed;
      }
  };
  ```





#### 023 C++在A.cpp中调用B.cpp定义的函数🌼

**创建头文件并包含**

- 创建B.hpp，声明函数foo
- 在B.cpp中定义foo
- A.cpp中inlcude<B.hpp>，则foo可以在A.cpp中调用



**extern申明外部函数**（全局函数也具有**全局作用域**）

- 在B.cpp中定义foo
- 在A.cpp中 extern foo() 申明后即可调用



#### 024 派生类的访问权限

**派生类访问权限**

- 派生类可以**访问基类中所有非private成员**
- 派生类可以**访问基类中所有非private函数**
- 派生类访问权限**与继承方式无关**

```c++
class A {
public:
	int n;
private:
	int m;
};

class B : public A {			// public继承
public:
	void f1() { 
		cout << n << endl; 		// 可以访问
		// cout << m << endl; 	// 不可访问：note: declared private here
		return ;
	}
};
```



#### 025 C++异常处理 throw try catch

C++程序运行时可能出现异常情况，如内存溢出、文件打开失败，如果不及时处理异常，可能会导致程序崩溃，所以**引入C++异常处理机制**



- **C++通过throw + try...catch实现异常处理**

  - throw为关键字
  - **catch块个数 ≥ 1**

  - 异常类型可以是基本/自定义数据类型

  - 能够**捕获任何异常**的catch(...)语句（类比switch-default）

```c++
try {
	// ...
    throw 表达式;	// 抛出异常 throw为C++关键字
} catch(异常类型) {
	// ...
}

catch(...) {	// 能够捕获任意异常 一般是最后一个catch块
    
}
```

- try...catch**语句执行过程**

  - try块中未抛出异常，则**跳过catch语句块继续执行**

  - try块中抛出异常类型，**转到第一个匹配异常类型i的catch块中执行**

- 异常**若在本函数中未被处理**，会**层层上抛给调用者**

  - 若抛到main函数中仍未被处理，则**程序立即异常终止**

```c++
// divid函数异常处理实例
int divid(vector<int>& vec, size_t i, size_t j) {
    try {
        if(i<0 || i>=vec.size() || j<0 || j>=vec.size())	// 内存越界
            throw 1;	// 抛出异常
        if(vev[j] == 0)	// 除数为0
            throw 2;	// 抛出异常
    } catch(int e) {	// 异常类型匹配，异常值传入e
        if(e == 1) {
            cout << "out-of-range" << endl;
        }
        if(e == 2) {
            cout << "can't divid 0" << endl;
        }
        
        return -1;
    } catch(...) {	// default处理
        cout << "unknow error" << endl;
        return -1;
    }
    
    return vec[i] / vec[j];	// 无异常时执行
}
```



**C++标准异常类** -- 由exception基类派生，用于规范异常

```c++
class exception {
public:
  exception() _GLIBCXX_USE_NOEXCEPT { }
  virtual ~exception() _GLIBCXX_TXN_SAFE_DYN _GLIBCXX_USE_NOEXCEPT;
  /** Returns a C-style character string describing the general cause
   *  of the current error.  */
  virtual const char*
  what() const _GLIBCXX_TXN_SAFE_DYN _GLIBCXX_USE_NOEXCEPT;
}
```

**派生关系**

- bad_cast：强制类型转化异常
- bad_alloc：new操作异常（没有足够堆空间）
- out_of_range：数组下标越界

![image-20220723000051135](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220723000051135.png)



#### 026 errno变量

**errno是一个全局整数变量**

- 其值描述了调用库函数时产生的**错误条件或诊断信息**（提示信息）
- **库函数都可以对其改写**，用于标识错误类型，以read()为例
  - 非阻塞read()发现缓存区中无数据时，返回-1，并将errno**置为EAGAIN**常量

- 使用时需包含errno.h头文件，因为此头文件中有error的extern声明
- perror()函数用于输出errno对应的错误描述

![image-20220729012903399](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220729012903399.png)



#### 027 STL对pair<int, int>的支持

- vector支持 **vector<pair<int, int>>**
- 堆结构支持 **priority_queue<pair<int, int>>**，默认大根堆

- **STL内置pair<int, int>类型的比较器**
  - 默认升序：**less<pair<int, int>>()**
  - 降序：greater<pair<int, int>>()







