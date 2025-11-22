## C语言复习

### 大端小端

**什么是低地址，高地址？**
	为了便于管理存储地址，给地址进行编号，值较大的地址是高地址，值较小的地址是低地址<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20250929125858801.png" alt="image-20250929125858801" style="zoom:50%;" />

**什么是数据的高位和低位？**
	数据的高位是数据的左边位置的数，数据的低位是数据右边位置的数，又称高字节和低字节
<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20250929130043000.png" alt="image-20250929130043000" style="zoom:50%;" />

**什么是大小端存储？**
	大端存储是低位字节存放在高地址，小端存储是低字节存放在低地址

<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20250929130351139.png" alt="image-20250929130351139" style="zoom:50%;" />

​	注意：一个字节为一个存储单元，计算机读数据永远是从低地址开始的
​	大端存储注意：从低地址开始读取，读到的从高地址开始放

### ASCII码

![image-20250929132056021](C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20250929132056021.png)

### 优先级

​     <img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20250929135247895.png" alt="image-20250929135247895" style="zoom: 67%;" /><img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20250929135305054.png" alt="image-20250929135305054" style="zoom: 67%;" />

### const

​	作用：修饰一个变量使之成为常量。 要求：必须初始化。
const在指针上的用法有三种，如果在*和p前面都加上const，就相当于将指针锁住并且将它指向的地址也锁住，也就是指向不可改，内容也不可改。如果只在*前面加const，就是将指向的地址锁住，就是指向可改，内容不可改。要是只在p前面加上const，那么就是将指针锁住，但仍可以通过*p改变他指向的空间，也就是指向不可改，内容可改。

### 全局变量可以通过extern引用，不可二次赋值

### 进制判断和类型判断

**进制判断**：看前缀 —— 无前缀为十进制，前缀 “0” 为八进制，前缀 “0x/0X” 为十六进制；
**类型判断**：看后缀 —— 无后缀默认 int，“L/l” 为 long，“LL/ll” 为 long long，“U/u”（可与 L/LL 组合）为 unsigned 类（如 unsigned long long）。

--------------------

### 》》

​	》sizeof是个运算符并不是函数
​	》单字节编码通常指ASCII码。双字节编码一般指Unicode_码。
​	》常用字符的（编码十进制表示）：数字字符0是48, 字母a是97字母A是65, 空格是32, 回车\r 13 换行是’\n’10
​	》小数无后缀默认double类型
​	》指针加 1和*指针 偏移量等于所指类型大小
​	》const修饰变量，变量必须赋初值
​	》`#define POINT_A int*` 是宏定义，仅仅是简单的文本替换。  
​		typedef int POINT_B`是类型定义，会创建一个全新的类型别名 POINT_B（代表 int）。
​	》strlen计算字符串长度 不包含‘\0’
​	》\后跟跟八进制数表示一个ASCII字符，\x为十六进制
​	》左移：补0  m<<n=m乘2的n次方  右移： 正数补0 负数补1
​	》C 语言中，结构体的总大小并非简单的成员大小之和，而是要满足**内存对齐规则**—— 每个成员的起始地址必须是其自身大小的整数倍（特殊情况：若成员大小超过编译器默认对齐单位，则按默认对齐单位对齐）。
​	》字符串 文件系统

****

### 	C语言习题

**1、 sizeof(9u+8ull+0176-1578)的计算结果：__8_____。**
根据类型提升规则，运算时所有操作数都会被提升为表达式中最高等级的类型 `unsigned long long`（通常为 8 字节），因此整个表达式的结果类型是 `unsigned long long`，`sizeof` 计算结果为 `8`。
**2.(unsigned char)-1-100的计算结果：___155____。**
首先，`-1` 是 `int` 类型（通常 4 字节），其补码表示为全 1（`0xFFFFFFFF`）。
当 `(unsigned char)-1` 时，会截断为 1 字节（8 位），结果为 `0xFF`（即十进制 `255`，因为 `unsigned char` 范围是 `0~255`）。
最终计算 `255 - 100 = 155`，因此结果为 `155`。

**3**.<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251002202310106.png" alt="image-20251002202310106" style="zoom:25%;" />逗号表达式会按从左到右的顺序执行每个子表达式，整个表达式的结果是最后一个子表达式的值。
**4.int a[] = {1,2,3,4};**    a 代表：第0个元素的地址   &a 代表: int[4]类型的地址

**5.为能指向float[3] [5]数组的指针创建一个名为P_F35的数组指针类型:**typedef float (*P_F35)[3] [5]。

**6.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251006233339865.png" alt="image-20251006233339865" style="zoom:50%;" />

**7.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251013174813877.png" alt="image-20251013174813877" style="zoom:50%;" />

C/C++ 中，当不同类型的数值相加时，会先提升到**最大类型**：`float` + `long long`→ 先把 `float` 转成 `double`，再和 `long long` 相加结果是 `double`）然后 `double` + `char` → 把 `char` 转成 `double`，结果还是 `double`
**8.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251013191540960.png" alt="image-20251013191540960" style="zoom: 33%;" />

**9.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251013192048901.png" alt="image-20251013192048901" style="zoom:33%;" />

**10.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251013192653335.png" alt="image-20251013192653335" style="zoom: 50%;" />

**11.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251013193338152.png" alt="image-20251013193338152" style="zoom: 33%;" />

**12.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251014202432861.png" alt="image-20251014202432861" style="zoom:33%;" />

**13.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251014202553910.png" alt="image-20251014202553910" style="zoom:50%;" />

**14.**void fun(int b[100])  里的  sizeof(b) 

函数参数里的数组会退化为指针，所以  b  本质是  int*

**15.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251014202643127.png" alt="image-20251014202643127" style="zoom:50%;" />

**16.**<img src="C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251014202655553.png" alt="image-20251014202655553" style="zoom:50%;" />









----------

