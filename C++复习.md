## C++复习

### C++基础

##### 面向对象三大特性：封装，继承，多态

​	**C语言面向过程的优点：**性能比面向对象高，比如单片机，嵌入式开发，Linux等一般面向过程开发，性能是最重要的因素
​								**缺点：**没有面向对象易维护，易复用，易扩展
​	**C++面向对象的   优点：**易维护，易复用，易扩展，由于面向对象有封装，继承，多态性的特征，可以设计出低耦合的系统，
​	使系统更加灵活，更加易维护
​								**缺点：**因为类调用时需要实例化，开销比较大，比较消耗资源，性能比面向过程低。

-------------------



##### 作用域：描述成员的归属，所属的范围，使用的范围

```C++
#include<iostream>
using namespace std;

int a = 1;
int main() {
	int a = 0;
	cout << a << endl;	//0

	//:: 作用域运算符   某个作用域 :: 成员 ,如果未指定任何的作用域 ，默认取全局作用域
	cout << ::a << endl; //1
	return 0;
}
```

```C++
//在全局自己开辟局部小的作用域
namespace 南岗区 {
	int a = 1;
}
namespace 道里区 {
	int b = 2;
}
```

-----------------



##### 动态申请空间

```C++
//------   C++中的动态申请空间，同malloc一样 都是在堆区申请空间  -------
int* p2= new int;	//new 关键字后放的是类型，返回当前类型的地址
*p2 = 200;
delete p2;  //手动释放空间
p2 = NULL;

int *p3= new int(300);	//申请空间 并指定初始化值
cout << *p3 << endl;	//300
delete p3;	//手动释放内存
p3 = NULL;

int* p4 = new int[3] {1,2,3}; //返回的是首元素地址，并不是整个数组的地址
delete [] p4;//在指针前 加上[]，方括号内不需要指定数字
p4 = NULL;
```

```C++
//1.new整形指针
int **p5= new int*;
delete p5;
p5 = NULL;

//2. new 整型指针的数组3
int** p6 = new int* [3];  //数组返回的是首元素地址，并不是整个数组的地址
delete[]p6;
p6 = NULL;

//3.整型数组的指针
int(**p7)[3] = new (int(*)[3]);  //new 关键字后放的是类型，返回当前类型的地址
delete p7;
p7 = NULL;

//4.二维数组
int (*p8)[2]= new int[3][2];
delete[]p8; //即使是多维数组仍然只用一个[]
p8 = NULL;
```

###### new和malloc的区别

1.new、delete是关键字，需要C++的编译器支持，malloc()、free()是函数，需要头文件支持。
2.new申请空间不需要指定申请大小，根据类型自动计算，new返回的是申请类型的地址，不需要强转，malloc()需要显式的指定申请空间的大小（字节），返回void*，需要强转成我们需要的类型。
3.new申请空间的同时可以设置初始化，而malloc需要手动赋值。
4.至关重要的一点：new申请类、结构体对象内存空间会自动调用构造函数，delete会自动调用类、结构体的析构函数，单独的malloc()和free()则不会调用构造、析构函数。

-------------



#### BOOl和bool

```C++
	BOOL B = TRUE; // typedef int BOOL;
	cout << sizeof(BOOL) << endl; //4
//----------------------------------------------------
	bool b = true;  //bool :C++ 中的关键字
	cout << sizeof(bool) << endl; //1
```

1.本质不同：BOOL整型的别名  bool关键字
2.占用空间大小不同： 4,1

-------



#### string字符串

C语言里操作字符串：

```C
	const char* p1 = "123";
	//p1[1] = 'b'; //error
	p1 = "456"; //正确

	char buf[5] = "abcd";
	buf[1] = '0';//正确
	//buf="defg" //error
	strcpy_s(buf, "defg");//正确

	//字符串的拼接
//char* p2 = p1 + buf;  //无法使用 +， +=做拼接
	char buf2[10] = "789";
	strcat_s(buf2, p1);
```

C++里操作字符串：

```C++
string str = "abcdef";
str[1] = '5';//正确
str = "123rtyhjhbvfdser";//正确

if (str != buf2) { //正确
	cout << "字符串不相等" << endl;
}

string str2 = str + p1; //正确
str += "ysl"; //正确

//28 28 这是计算这个类型本身的大小，并不是计算字符串的长度
cout << sizeof(string) << " " << sizeof(str2) << endl; 
//获取字符串的长度
cout << str2.size() << " " << str2.length() << endl;

str2.substr(3/*截取位置的下标*/, 3/*截取长度*/); //若长度超限，截取有效长度

cout << str.find("9d") << endl; //返回的是下标
```

-----------------

#### for循环

```C++
for(int i=0;i<sizeof(arr)/sizeof(arr[0]);==i) //两分三段
	
for(int v: arr)  //范围for循环
//可以遍历的：有范围的数组，字符串，支持begin ,end操作的容器(list,vector,map)
```

------

#### 函数参数默认值

```C++
void fun(int a = 10) { //当调用函数时，如果不传递实参，则使用的是这个默认值
	cout << "fun " << a << endl;
}

//规定 指定默认值的顺序：从右向左依次指定，中间不得间断
void fun2(int a, int b=100, int c = 300) {
	cout << "fun2 " << a << endl;
}

//函数的声明和定义如果分开如何指定？ 多数情况下，应当在声明处指定
//少数情况，声明和定义可以同时指定(声明局部化)
void fun4(int a=21){
    cout<<"fun4 "<<a<<endl;
}
int main(){
    fun4();//21
    void fun4(int a=22);//声明局部化
    fun4();//22
    return 0;
}
```

#### 函数重载

​	函数重载:在同一个作用域下，函数名相同，参数列表不同（形参的类型，顺序，数量），返回类型没有任何要求
​	函数重载是描述多个函数之间的关系，在调用函数时，可以根据传递的实参自动取匹配对应的函数。

```C++
#include<Windows.h>
#include<iostream>
using namespace std;
int Add(int a, int b)
{
	return a + b;
}
double Add(double a, double b)
{
	return a + b;
}
float Add(float a, float b)
{
	return a + b;
}
int main()
{
	cout<<Add(1,2)<<endl;
	cout<<Add(3.5, 4.5)<<endl;
	cout << Add(2.22, 3.33) << endl;
	return 0;
}
```

如遇区分不开情况可采用局部作用域+声明进行区分

```C++
int a=1;
{
    void fun3(int a);
    fun3(a);
}
fun3(a,20)
```

------------

#### nullptr关键字

​	C++中的关键字，描述真正的空指针  而NULL本质为宏替换：#define NULL 0,fun(0);
​	nullptr出现的原因：在函数重载的情况下，可能会出现NULL 0 含义不明确，（可以代表整型也可以代表空指针）

----------

#### 引用&

```C++
int a = 0;
int& b = a; //定义引用，对已存在的空间或变量 起别名   通过常量指针实现

void fun(int a){//值传递
    
}
void fun(int &a){//形参定义引用，对将来传递的实参起别名
    //&a;//取地址
}
```

​	函数传参的三种方式： 值传递，引用传递，地址传递
​	如果不涉及到修改实参，对于内置的基本类型来说，三种方式皆可。
​	对于复合类型来说，不推荐值传递，推荐引用传递，地址传递（从额外申请空间的角度考虑的）
​	如果要修改实参，推荐 引用传递，地址传递，根据自己的习惯来

​	指针和引用多数情况下可以通用
​		特点，区别：

```C++
int &c=b; //定义了引用就必须初始化，引用一个具体真实存在的空间；指针可以为空，可以不指向任何空间
int d=100; c=d;   //是赋值操作，而非是重新引用其它空间  一心一意的暖男
int* p=&d; //指向可以随时更改  三心二意的渣男

//指针有多级 引用没有多级
int && e=10; //右值的引用  int &c:左值的引用

//有指针的引用 没有引用的指针
int *& pp=p;

//int &p2[3]; //整型引用的数组错误没有这样的写法
```

--------------------------------

### 类基础

#### 	类封装

​	类：完成某一功能的数据和算法的集合，是一个抽象的概念。
​	对象：类的一个实例，集体的概念，是真正存在于内存中的。

```C++
//定义类的关键字
class CPeople {
public:
	string m_strName;	//姓名
	bool m_bSex;	//性别
	int m_nAge;		//年龄
	void eat() {
		cout << m_strName << "正在吃盒饭" << endl;
	}
	void show() {
		cout << "name:" << m_strName << "，sex:" << m_bSex << ",age:" << m_nAge << endl;
	}
};
int main() {
	CPeople peo; //类定义的变量 称之为对象
	peo.m_strName = "张三";
	peo.eat();
}
```

**public：**被 `public` 修饰的成员，在**任何地方都可以被访问**（类内部、类外部、派生类中均可见）。
**private：**被 `private` 修饰的成员，**只能在当前类的内部访问**，类外部（包括派生类）均无法直接访问。
**protected：**被 `protected` 修饰的成员，**只能在当前类内部或其派生类（子类）中访问**，类外部无法直接访问。

**构造函数：**函数名为当前类名，无返回类型，参数没有要求。
				 空类中，如果未定义任何的构造函数，编译器会默认添加无参的构造函数，函数体代码未执行任何内容
**作用：**为成员属性做初始化，在定义对象的时候，由编译器保证自动调用，一个类中，构造函数可以写多个，为函数重载的关系

```C++
CPeople(){	//构造函数
    m_bBox=true;
    m_nAge=20;
}

Cpeople(const string& name,int age){	//带参数的构造函数
    m_strName=name;
    m_bSex=true;
    m_nAge=age;
}

Cpeople* pPeo=new CPeople; //自动调用构造函数
Cpeople* pPeo2=new CPeople("李大埋汰"，50); //自动调用构造函数
Cpeople* pPeo3=(CPeople*)malloc(sizeof(CPeople)); //不会调用构造函数，也不会初始化成员
```

**析构函数：**函数名： ~Cpeople，参数为无参 函数体代码用来回收额外申请的空间（作用）
				在对象的生命周期结束的时候，编译器自动调用析构函数，用来回收额外的空间
栈区局部对象在函数返回的时候被销毁，在遇见大括号} 的时候，生命周期结束，被销毁，在执行delete的时候，生命周期结束被销毁
				先执行析构函数回收了额外的空间，再回收本身的空间

##### C++中结构体和类的区别

​		1.默认的访问修饰符不同，结构体public,类private
​		2.默认的继承方式不同，结构体public,类private

------------------------

### 类成员进阶

![image-20251109222159523](C:/Users/zhanr/AppData/Roaming/Typora/typora-user-images/image-20251109222159523.png)

​	空类定义对象大小为1，起到了一个占位作用，用来表明对象真实的存在于内存空间中
​	**一般的类成员属性：** 属于对象（跟对象走），定义对象的时候存在，每个对象都有属于自己的属性，彼此是独立的互不干扰
​	**类成员函数：**属于类的，编译期就存在，他的存在与否和是否定义对象无关。函数只会存在一份，多个对象共享这同一份函数
​	

```C++
CTest tst;
&tst.fun; //这并不是取函数的地址
&CTest::fun;//才是取函数地址
```

#### this指针

​	this既是关键字又是指针，this指针参数是编译器 默认添加到类成员函数中，隐藏的第一个参数
​	**this作用：**连接对象和成员的桥梁，在成员函数中可以无感知的直接使用成员
​	类中的非静态成员函数，包括任何的构造和析构函数都会有this指针这个隐藏的参数

#### 静态成员

​	静态成员属性：属于类的，在编译期就存在了，只会存在一份，多个对象共享这同一个静态的成员属性
​	定义及初始化，在类外进行：格式（去掉static关键字） 类型 类名：：成员名=初始化值

```C++
class CTest{
public:
    int m_a;
    static int m_b; //在静态全局区
    CTest(){
        m_a=1;
    }
};
//定义及初始化
int CTest::m_b=2;

CTest* p=nullptr;
cout<<p->m_b<<endl;//2

CTest<<CTest::m_b<<endl; //类名作用域可以直接调用
```

**静态成员函数和普通成员函数的差异：**
	1.静态函数没有this指针，普通成员函数有this指针
	2.静态函数中只能使用静态成员，普通函数中都可以使用
	3.可以通过对象调用也可以使用类名作用域直接调用

#### 静态实例

​	

```C++
//统计 对象的数量
class CTest {
private:
	static int num; //统计对象的数量的变量
public:
	CTest() {
		++num;
	}
	~CTest() {
		--num;
	}
public:
	static int getNum() {
		return num;
	}
};
int CTest::num = 0;
```

#### 常量和初始化参数列表

​	构造函数中用于真正初始化成员的地方： 初始化参数列表
​	在参数列表后面，用一个冒号开头，后面写初始化的成员
​	常量必须在初始化参数列表中进行初始化
​	它是属于构造函数的一部分，先于函数体代码执行，析构函数和其他的函数无初始化参数列表

```C++
CTest():m_b2(2),m_a(1){
}
```

初始化的顺序：成员在类中声明的先后顺序，和写在初始化参数列表中的顺序无关
引用也应当在初始化参数列表进行初始化

#### 常函数：常量成员函数

```C++
void fun(/*const CTest* const this*/) const{
    cout<<"funConst"<<endl;
}
//在常函数中，this指针的类型比一般函数多一个const约束，要求了this指针指向的对象中的成员不能修改
//静态成员不受this指针的约束，可以查看，可以修改

//1.通过其他的指针间接修改
    int*p=(int*)&this->m_a;
    *p=30;

//2.通过mutable关键字修饰 可以修改
mutable int m_a;//修饰的一个变量，在常函数中可以修改

//3.不能调用一般的函数，this指针的安全级别是降低操作

//静态函数不能调用非静态的函数
static void funStatic(){
   // fun();	没有this指针 不能调用非静态的函数
}
-----------------------------------------------------------------------
同名，同参数的是否构成函数重载？ 一般函数和常函数是重载 和静态函数不构成重载（报错）
//不能重载具有相同参数的静态和非静态成员函数
//有一般的函数则优先匹配一般的函数，如果没有则匹配常函数

```

在常函数中即使是变量也不能改

### C++继承

继承：子类继承父类，可以使用父类成员，在定义对象时也会包含父类的成员，子类可以看做是父类的延续和扩展
子类对象的大小计算：不但包含子类，也要将父类的成员计算在内
									内存排布顺序：先父类成员再子类成员

```cpp
class CFather {//父类 基类
public:
	int m_fa;
	void funFather() {
		cout << "funFather" << endl;
	}
};

//继承写法：子类的类名后 冒号 继承方式 继承的父类
class CSon :public CFather {//子类 派生类
public:
	int m_son;
	CSon() {
		m_son = 1;
		m_fa = 2;
	}

	void funSon() {
		cout << "funSon" << endl;
	}
};
```

#### 继承下初始化顺序

在继承下定义子类对象 构造执行的顺序：父类->子类
对于父类和组合包含的类来说，无参的构造函数编译器可以帮助自动调用
如果想调用带参数的构造函数，需要手动显式的指定调用
如果不存在默认无参的构造，必须手动调用带参数的构造函数

析构顺序：子类->父类,与构造函数顺序相反

```cpp
class CFather {//父类
public:
	int m_fa;
	CFather():m_fa(2){}
};
class CSon :public CFather {//子类
public:
	int m_son;
	CSon() :m_son(1), CFather{}
};
```

#### 继承方式

​	继承方式：描述了父类的成员在子类中所表现的属性，和访问修饰符共同决定了父类成员在子类中的访问权限
​	public:公有继承
​	protected:保护继承
​	private:私有继承

继承方式：public：
父类属性   	子类属性
public:			public:
protected:		protected:
private:			不可访问

继承方式：protected:
父类属性   	子类属性
public:			protected:
protected:		protected:
private:			不可访问

继承方式：private:
父类属性   	子类属性
public:			private:
protected:		private:
private:			不可访问

默认的继承方式：私有继承			结构体默认的继承方式： 公有继承

#### 隐藏

隐藏：子类和父类中同名的成员的关系
			父类和子类中同名的函数不构成重载关系

#### 继承的优点

​	将多个类中的功能相似类中的公共成员提取到一个父类中，这些类继承父类各个子类就拥有了父类的功能，且只需要维护一份代码
提高了程序代码的复用性，扩展性，灵活性 维护方便

#### 父类指针指向子类对象

​	父类指针可以不通过强转 可以直接指向子类对象，保证了子类的对象可以成功的调用父类的函数
​	子类指针不可以指向父类对象
​	父类的引用也可以直接引用子类对象
​	
​	父类指针可以统一多个子类对象，提高代码复用性，扩展性，灵活性。

#### 类成员函数指针

```cpp
//普通函数指针
void (*p_fun)()=&fun;//&可写可不写
(*p_fun)();

//类成员函数指针    ::*是C++中的整体操作符号
CTest tst;
void (CTest::*p_fun2)()=&CTest::fun;//&必须写
(tst .* p_fun2)(); //通过对象调用类成员函数指针指向的函数， .*编译器会自动的将对象的地址传递给this指针
((&tst) ->* p_fun2)(); //  ::* .* ->* C++中提供的整体操作符    &tst就变成了指针对象

//类中静态函数
p_fun=&CTest::funStatic;//类中静态的函数，通过指针调用 同函数一样
(*p_fun)();
```

**全局函数和类成员函数区别：**1.作用域不同     2.类成员函数有隐藏的this指针参数，全局函数没有

### C++多态

相同的行为方式，导致不同的行为结果，同一行语句展现了多种不同的表现形态，即是多态性
C++中多态：父类的指针可以指向任何继承于该类的子类对象，多种子类具有多种形态，由父类的指针统一管理
父类的指针则具有了多种形态 多态性

#### **多态形成的条件：**

​	1.在继承的条件下，用父类的指针指向子类对象
​	2.再父类中存在虚函数，并且子类重写了父类的这个虚函数

重写：在虚函数的前提下，子类定义了和父类虚函数一模一样的函数。
协变：要求父类的函数返回父类的引用，指针， 子类的函数 返回子类的指针，引用  其余情况要求返回类型必须一致。

```cpp
class CFather {
public:
	//定义虚函数的关键字: virtual
	virtual void fun(){
        /*对于实现没有要求*/
        	cout<<"CFather::fun"<<endl;
    }

};

class CSon :public CFather {
public:

	void fun() { //也是虚函数，只要这个函数重写了父类的虚函数，virtual可以省略 仍是虚函数
		cout << "CSon::fun" << endl;
	}

};

void fun(CFather fa, CFather& rfa,CFather* pfa){
    fa.fun();	//父类的对象 不能实现多态
    rfa.fun();	//可以实现多态
    pfa->fun();	//可以实现多态
}

int main() {
	CFather* pfa = new CSon;
	pfa->fun();		//CSon::fun ，编译期匹配父类函数 运行期调用子类函数
    
    CSon *pson=new CSon;
    pson->fun(); //CSon::fun
    
    pfa->CFather::fun();//CFather::fun	显式的指定父类的类名作用域 编译期绑定，破除了多态 
    
	return 0;
}
```

#### 绑定

​	编译期绑定（静态绑定）:通过名字调用全局，类中的一般函数成员 静态
​	运行期绑定（动态绑定）：多态调用函数一定是运行期绑定。 指针的形式运行期绑定

#### 虚函数

当一个类中存在虚函数的时候，定义对象会在对象内存空间的前面 开辟指针大小的空间 为虚函数列表指针，__vfptr
_vfptr:虚函数列表指针，由编译器增加的隐含的成员属性 属于对象的，在每个对象内存空间前面都会存在一份

vftable:虚函数列表，属于类的，只有一份，在编译期被构建出来，数组的每一个元素为虚函数的地址
多个对象中的虚函数列表指针指向的是同一个虚函数列表

作用：实现运行期绑定调用虚函数

虚函数的调用流程：首先找到对象中的虚函数列表指针，通过虚函数列表指针找到虚函数列表（函数指针数组），通过下标定位到虚函数真正的地址，取出地址后进入函数入口执行。

CTest* ptst=nullptr; ptst->fun();	//动态绑定 无法取到虚函数列表指

```cpp
//函数指针函数
typedef void (*P_FUN)();
P_FUN arr[2];
P_FUN* p_vftr = arr;

//对象中取 虚表地址
p_vftr=(P_FUN*)*(int*)&tst;
```

#### 	多态下的虚函数

​		CFather* pfa=new CSon;
​	虚函数列表指针：在继承下指向的是子类的虚函数列表 （实际的对象是哪个类的，就用哪个类的虚函数列表）
​	虚函数列表内容（子类）：
​			构建过程（编译期由编译器自动构建完成）：  
​								1.继承：将父类的虚函数列表继承过来到子类的虚表中
​								2.检查：如果有重写的，则原地覆盖，没有重写则仍然保留
​								3.顺序添加：如果子类有单独的虚函数，按照声明的先后顺序，依次添加到虚表的尾部。

常函数，析构函数可以为虚函数  		静态函数，构造函数不能为虚函数      

#### 虚析构

​	调用析构取决于指针的类型，在继承多态下，执行了父类的析构函数，子类析构没有执行，可能会造成内存泄漏
虚析构：目的也是实现多态 ，以后用到了多态 父类的析构应当为虚析构

```cpp
pfa->~CFather();	//由于发生了多态， ->~CSon(); 在回收父类子对象内存空间时，再去调用父类的析构
```

#### 纯虚函数

```cpp
class CAnimal{	//抽象类：包含纯虚函数的类，不能实例化对象
public:
    virtual void eat()=0;	//纯虚函数，在父类中不需要定义，后写=0
};

//类外定义纯虚函数，   通过静态绑定调用（加类名作用域）
void CAnimal::eat(){
    cout<<"CAnimal::eat()"<<endl;
}

//具体类：需要重写定义实现父类的纯虚函数
```

多态的缺点：
	1.时间问题：实现多态通过虚函数，调用流程复杂，效率低。
	2.空间问题：每个对象中，都会有一个vfptr, 占用空间，虚函数列表随着继承的层级越多其大小只增不减
	3.安全问题：如果一个函数为私有函数，就不要变为虚函数了，如果一个函数为虚函数，也不用加上private修饰符进行限制了

### 探索程序

使用场景：
	头文件：变量声明，函数的声明，类的声明和定义，成员属性，函数的声明 …
	源文件：变量的定义，函数的定义，属性定义及初始化，成员函数的定义…

```cpp
//常函数：在源文件中const关键字需要保留
void CTest::funConst(/* const CTest* const this */)const{
    cout<<"funConst"<<endl;
}

//在源文件中 virtual 关键字 去掉
void CTest::funVirtual(){
    cout<<"funVirtual"<<endl;
}
```

#### 头文件重复包含

​	#pragma once 告诉编译器，当前这个头文件在其他的源文件中 只会包含一次
​	
​	基于宏的逻辑判断：
​		#ifndef  宏名字   		//如果没有定义这个宏
​		#define 宏名字		   //生效代码，就会定义了相同的宏
​	我需要去重的代码	
​		#endif						 //和 ifndef 配对使用

​	**#pragma once:**简单高效，直接和编译器沟通 效率高一些，以文件为单位进行去重
​	**基于宏的逻辑判断：**需要有宏判断的过程，效率略低，宏名字有重复的概率，可能会导致程序逻辑错误。优点：可以做到部分代码去重，是C++标准中的内容，适用范围广

#### 头文件重复包含

#### 编译期—运行期

​	class:编译期：成员函数，静态属性，静态函数，访问修饰符，作用域
​	对象：运行期的概念，一般的对象，引用对象，指针对象

```cpp
#include<iostream>
using namespace std;

int main() {

#ifdef  __cplusplus	//是否为C++的环境
#define A 10
#else
#define A 20
#endif

	int B = A;			//什么时候确定初始值
	cout << B << endl;	//10
	int a = 0;
	int b = 0;
	if (a > 10) {		//运行期判断
		b = 100;
	}
	else {
		b = 200;
	}
	int c = b;			//运行期确定
	cout << c << endl;


	return 0;
}
```

#### 宏

```c++
//预处理阶段进行替换
#define A 5

// 使用\替换多行： \用来连接当前行和下一行，一般最后一行不加
// 注意：\后面不能有任何的字符，包含 空格 注释 tab
//可以像函数一样传递参数，参数的作用也是一个替换
//宏参数 没有类型 只是一个替换的标识符 参数也不会自动计算表达式的求解，只是单纯的替换 
#define C(N) for(int i=0;i<N;i++){\
	cout<<i<<"   ";\
}\
cout<<endl;
//相对于函数，宏更高效

// #将宏参数转为字符串，相当于加上了“”
#define D(DD) #DD
// #@ 将参数转换为字符，相当于加上‘’
#define E(EE) #@EE
```

#### 内联函数

```cpp
//inline：定义内联函数的关键字
inline int Mul(int a, int b) {
	return a * b;
}
```

​	内联函数:在编译阶段进行替换，适用的场景：简单的函数，短小精悍，代码量少，没有复杂的逻辑。是一个建议性关键字，建议编译器将这个函数变为内联函数
​	递归函数，虚函数不能成为内联函数，虚函数动态绑定，静态绑定绑定调用虚函数可以成为内联函数
​	在类内声明且定义的函数，默认为内联函数；在类内声明，类外定义的函数，默认不是内联函数
​	

#### operator

​	 是用于**运算符重载**的关键字,作为函数名

```cpp
返回类型 operator运算符(参数列表) {
    // 操作逻辑
}
```

### 拷贝构造

#### 	转换构造

​	

```cpp
#include<iostream>
using namespace std;
class CTest {
public:
	int m_a;
	CTest():m_a(0){}

	//CTest(int a):m_a(a){} //可以发生隐式类型转换的构造函数 可以称之为 转换构造函数
	
	//也可以 转换构造函数， explicit:显式的，禁止构造函数发生隐式类型转换 显式的传参
	CTest(int a,int b=30):m_a(a){} //explicit CTest(int a,int b=30):m_a(a){} 

};


int main() {	
	CTest tst();	//无参的构造函数 CTest tst（）；默认解析为声明函数
	CTest tst2(10);	//匹配带参数的构造函数
	CTest tst3 = 20; //隐式类型转换	通过带参数的构造函数
	cout << tst3.m_a << endl; //20
	return 0;
}
```

#### 拷贝构造

```cpp
#include<iostream>
using namespace std;
class CTest {
public:
	int m_a;
	int* m_p;
	CTest(int a):m_a(a),m_p(new int(a)){} 
	~CTest() {
		if (m_p) {
			delete m_p;
			m_p = nullptr;
		}
	}
	//编译器会默认提供拷贝构造函数,必须加引用&
	//函数体内容：形参对象中成员属性依次给 this 对象中的属性做初始化
	//如果手动重构了拷贝构造函数 编译器默认 就不会再提供默认的了//CTest(CTest& tst):m_a(tst.m_a){}
	
	//浅拷贝问题:多个对象中的指针成员指向了同一个空间，导致在回收的时候，回收多次，使程序异常
	//默认的拷贝构造函数为浅拷贝，解决问题：手动重构拷贝构造函数，不但将指针拷贝一份，而且将指针指向的空间也要拷贝一份且初始化拷贝值
    //对于函数参数来说，尽量避免值传递，避免浅拷贝的问题，改用指针或接口
	CTest(const CTest& tst) :m_a(tst.m_a), m_p(tst.m_p) {
		if (tst.m_p) {
			m_p = new int(*tst.m_p);	//创建属于自己的空间并初始化拷贝值
		}
	}
};

int main() {	

	CTest tst1(10);	
	CTest tst2(tst1);
	cout << tst2.m_a << endl;//10
	cout << *tst2.m_p << endl; //10

	delete tst1.m_p;
	tst1.m_p = nullptr;
	CTest tst3(tst1);
	cout << tst3.m_a << endl;//10
	cout << tst3.m_p << endl; //000000000000000000000
	return 0;
}
```

#### operator =

```cpp
//类中重载等号操作符函数
//空类中也会默认提供 operator= 函数 参数为 const类名&对象名 返回类型为：当前类型对象的引用
//如果一旦手动重构了这个operator= ,编译器就不会提供默认的函数了，调用的就是自己重构的了
//operator= 默认是浅拷贝
CTest& operator=(/* CTest * const this */const CTest& test){
    this->m_a=tst.m_a;
    return *this;
}
```

#### 为何禁止隐式类型转换

```cpp
#include<iostream>
using namespace std;
class CTest {
public:
	int m_a;
	int* m_p;
	explicit CTest(int a):m_a(a),m_p(new int(a)){} 
	~CTest() {
		if (m_p) {
			delete m_p;
			m_p = nullptr;
		}
	}
};
int main() {	
	CTest tst(10);	
	//tst = 20;	// tst=CTest(20);//会隐式的创建对象 等号右边的20通过构造函数，创建出一个临时的匿名的对象,然后用这个匿名的对象给对象赋值(operator=)
	//会有潜在风险
	tst = CTest(20);	//explicit 禁止了隐式转换后，只能手动显示的创建对象，风险变得更明显了
	cout << tst.m_a << endl;
	cout << *tst.m_p << endl;
	return 0;
}
```

### 设计模式

#### 单例模式：

​	当前类最多只能创建一个实例
​	它必须自己创建这个实例（而不是调用者创建）
​	它必须自己向整个系统提供全局访问点访问这个实例
注意：构造私有化保证了对象不能在类外创建

##### 懒汉式：当前的这个唯一的实例，什么时候调用获取单例的接口，什么时候创建单例对象。

```cpp
#include<iostream>
using namespace std;
// 单例模式类：确保程序中仅有一个实例，并提供创建和销毁接口
class CSingleton {
private:
    // 确保实例的创建完全由类内部控制
    CSingleton(){}
    // 删除拷贝构造函数：显式禁止通过拷贝已有的实例创建新实例
    CSingleton(const CSingleton&) = delete;
    // 私有析构函数：禁止外部直接调用 delete 销毁实例
    ~CSingleton(){}
    // 静态成员指针：存储全局唯一的实例地址
    // 静态成员属于类本身，所有对象共享此指针，保证唯一性
    static CSingleton* m_psin;
public:
    // 首次调用时创建实例，后续调用直接返回已存在的实例
    static CSingleton* CreateSingleton() {
        if (!m_psin) {  // 判断实例是否已创建（为空则未创建）
            m_psin = new CSingleton;  // 首次调用时创建唯一实例
        }
        return m_psin;  // 返回实例指针
    }
    // 静态成员函数：销毁单例实例
    // 参数为引用指针，确保外部传入的指针也会被置空，避免野指针
    static void DestroySingleton(CSingleton*& psin) {
        if (m_psin) {   // 若实例存在
            delete m_psin;  // 销毁实例（会调用私有析构函数）
        }
        // 将静态指针和外部传入的指针都置空，防止悬空指针
        m_psin = psin = nullptr;
    }
};
// 初始化为 nullptr，表示初始状态下实例尚未创建
CSingleton* CSingleton::m_psin = nullptr;
int main() {	
    CSingleton* p1 = CSingleton::CreateSingleton();
    // 第二次调用：实例已存在，p2 指向同一实例（与 p1 地址相同）
    CSingleton* p2 = CSingleton::CreateSingleton();
    CSingleton* p3 = CSingleton::CreateSingleton();
    // 输出三个指针的地址，验证是否指向同一实例
    // 结果将显示三个地址完全相同，证明单例的唯一性
    cout << p1 << " " << p2 << " " << p3 << endl;
    // 调用销毁接口：通过 p1 传入指针，销毁实例并将所有相关指针置空
    CSingleton::DestroySingleton(p1);
    // 此时 p1、p2、p3 均为 nullptr（p1 被引用修改，p2、p3 需手动确认，此处仅演示销毁逻辑）
    return 0;
}
```

##### **懒汉式2：更简单**

```cpp
class CSingleton {
private:
	CSingleton():m_a(1){}
	CSingleton(const CSingleton&) = delete;	//删除拷贝构造
	~CSingleton(){}
public:
	// 静态成员函数：提供全局访问点，返回单例实例（懒汉式实现）
	static CSingleton* CreateSingleton() {
		// 局部静态变量：首次调用该函数时初始化，且仅初始化一次
		static CSingleton sin;
		return &sin;
	}
	int m_a;
};
```

##### 饿汉式：这个唯一的实例在程序创建之初就已经被创建了，通过接口直接获取

```cpp
class CSingleton {
private:
	CSingleton():m_a(1){}
	CSingleton(const CSingleton&) = delete;	//删除拷贝构造
	~CSingleton(){}
	static CSingleton sin ;
public:
	// 在多线程下，不会创建多个单例对象 return0；
	static CSingleton* CreateSingleton() {
		return &sin;
	}

	int m_a;
};

//类外定义
CSingleton CSingleton::sin;
```

#### 工厂模式

##### 简单工厂

```cpp
#include<iostream>
using namespace std;
class CEngine {
public:
	virtual void working() = 0;
};
class CEngine2L: public CEngine{
public:
	void working(){
		cout << "2.0L的发动机正在工作" << endl;
	}
};
class CEngine2T : public CEngine {
public:
	void working(){
		cout << "2.0T的发动机正在工作" << endl;
	}
};
//简单工厂
class CEngineCFac {
public:
	CEngine*  CreateEngine(const string& type) {
		CEngine* pEn=nullptr;
		if (type == "2L"){
			pEn=new CEngine2L;
		}
		else if (type == "2T"){
			pEn = new CEngine2T;
		}
		return pEn;
	}
};
class CCar {
public:
	CEngine* m_pEn;//发动机
/*  CCar(const string &type) {
		if (type == "2L"){
			m_pEn = new CEngine2L;
		}
		else if (type == "2T"){
			m_pEn = new CEngine2T;
		}
		else {
			m_pEn = nullptr;
		}      
	}			*/
	CCar(CEngineCFac* pfac, const string& type)
      :m_pEn(pfac ? pfac->CreateEngine(type) : nullptr){
	}
	~CCar(){
		if (m_pEn){
			delete m_pEn;
			m_pEn = nullptr;
		}
	}
	void drive(){
		if (m_pEn){
			m_pEn->working();
			cout << "小车开始跑" << endl;
		}
		else {
			cout << "没有发动机" << endl;
		}
	}
};
int main()
{
	//CCar c1("2L");
	//c1.drive();
	CEngineCFac fac;
	CCar c2(&fac, "2T");
	c2.drive();
	return 0;
}
```

采用工厂模式的优点：
	解耦：汽车类无需自己创建发动机，依赖工厂获取，职责单一
	简化使用：客户端只需指定类型，无需关注具体发动机的创建细节
	易维护扩展：新增发动机类型时，仅需修改工厂，客户端代码不变

##### 工厂方法

```cpp
class CEngineCFac {
    virtual CEngine* CreateEngine() = 0;
};

class CEngineFac2L : public CEngineCFac {
public:
    CEngine* CreateEngine() {
        return new CEngine2L;
    }
};

class CEngineFac2T : public CEngineCFac {
public:
    CEngine* CreateEngine() {
        return new CEngine2T;
    }
};
```

##### 抽象工厂

抽象工厂和工厂方法的模式基本一样，区别在于工厂方法是生产一个具体的产品，而抽象工厂可以用来生产一组相同，有相对关系的产品，重点在于一组，一批，一系列

```cpp
// 抽象产品：发动机
class CEngine {
public:
    virtual ~CEngine() = default;
    virtual void Run() = 0;
};
// 具体产品：2L发动机
class CEngine2L : public CEngine {
public:
    void Run() override { /* 2L发动机运行逻辑 */ }
};
// 具体产品：2T发动机
class CEngine2T : public CEngine {
public:
    void Run() override { /* 2T发动机运行逻辑 */ }
};
// 抽象产品：变速箱
class CTransmission {
public:
    virtual ~CTransmission() = default;
    virtual void Shift() = 0;
};
// 具体产品：手动变速箱
class CManualTrans : public CTransmission {
public:
    void Shift() override { /* 手动换挡逻辑 */ }
};
// 具体产品：自动变速箱
class CAutoTrans : public CTransmission {
public:
    void Shift() override { /* 自动换挡逻辑 */ }
};
// 抽象工厂：定义产品族的创建接口（发动机+变速箱）
class CCarFactory {
public:
    virtual ~CCarFactory() = default;
    virtual CEngine* CreateEngine() = 0;       // 创建发动机
    virtual CTransmission* CreateTransmission() = 0; // 创建变速箱
};
// 具体工厂1：生产2L发动机+手动变速箱的产品族
class CStandardCarFactory : public CCarFactory {
public:
    CEngine* CreateEngine() override {
        return new CEngine2L();
    }
    CTransmission* CreateTransmission() override {
        return new CManualTrans();
    }
};
// 具体工厂2：生产2T发动机+自动变速箱的产品族
class CPerformanceCarFactory : public CCarFactory {
public:
    CEngine* CreateEngine() override {
        return new CEngine2T();
    }
    CTransmission* CreateTransmission() override {
        return new CAutoTrans();
    }
};
```

三种工厂方式总结：
	对于简单工厂和工厂方法来说，两者的使用方式实际上是一样的，如果对于产品的分类和名称是确定的，数量是相对固定的，推荐使用简单工厂模式
	抽象工厂用来解决先对复杂的问题，适用于一系列，大批量的对象生产

### STL：标准模版库

#### 容器：

序列性容器：容器保持了元素的原始位置，每个元素都有固定的位置，位置取决于插入的时间，地点，list vector deque
关联性容器：元素的位置取决于容器特定的排序规则，一般是与元素的值有关 map,set,hash_map

##### list链表

```c++
list<int> lst{1,2,3,4};
list<int> lst(3);	//创建链表指定长度
list<int> lst(3,6);	//创建链表指定长度 指定初始值

lst.push_back(10);
lst.push_front(4);
lst.remove(6);	//将所有值为6的节点都删除掉
lst.unique();	//连续且相同，保留一个
lst.sort(greater<int>());	//括号里指定排序规则，greater<int>():排序规则中的降序
lst.reverse();	//翻转链表

//原链表 10 10 6 4
list<int> lst2{1,9,5,0};
::advance(ite,2);	//指定迭代器偏移量
//剪切，把 lst2 里所有元素，直接挪到 lst 中 ite 指着的位置前面，挪完后 lst2 就空了
lst.splice(ite,lst2);	//10 10 1 9 5 0 6 4
lst.splice(ite,lst2,++lst2.begin());//10 10 9 6 4 	++lst2.begin()为剪切lst2的第几个元素
lst.splice(ite,lst2,++lst2.begin(),-lst2.end())//10 10 9 5 6 4 	范围：左闭右开

lst.merge(lst2); 	//把lst2合并到lst1
/*两链表必须升序（默认），否则结果乱
被合并链表会清空，元素直接移动（无拷贝）
支持传比较函数自定义排序规则。*/

lst.swap(lst2); //交换
```

##### DynamicArray:动态数组

##### vector向量:底层是动态数组

```cpp
vector<int> vec(5); //创建容量为5的动态数组
vector<int>::iterator ite=vec.begin();
vector<int> vec(5,3);//指定初始化的值

int arr[5]={2,3,4,5,6};
vector<int> vec(arr,arr+4);	//2 3 4 5 左闭右开
vec.pop_back();//2 3 4

vec.insert(vec.begin(),1);
ite=vec.begin()+3;	//ite[3]  vector中的迭代器支持随机访问
//插入或删除某个位置，之后的元素的迭代器默认会失效

vec.clear();	//清空使用量，容量不变
vec.resize(5);	//设定使用量 5个0
vec.resize(5,1); //5个1
//VS 中 容量不够用时1.5倍扩容

//容量缩容
vec.shrink_to_fit(); //缩减到和使用量一致
vector<int>().swap(vec);	//不但交换了使用量，也会交换容量。
vector<int>{1,2,3}.swap(vec);
```

##### deque双端队列：分段连续内存块 + 中控数组

```cpp
deque<int> de{1,2,3,4};
deque.push_back(5);
deque.push_front(0);

deque.pop_front();
deque.pop_back();

int size=de.size();
for(int i=0;i<size;++i){
    cout<<de[i]<<" ";
}
```

##### map:映射表,关联性容器

键值，实值，特定的排序规则 可以按照键值自动排序（升序） 键值唯一，不允许重复
map的每一个元素 键值对->键值，实值

```cpp
map<char,int> m{{'d',1},{'a',2},{'c',3}};
map<char,int>::iterator ite=m.begin();
while(ite!=m.end()){
    //键值：first    实值：second
    cout<<ite->first<<"-"<<ite->second<<" ";	// a-2  c-3  d-1
    ++ite;
}
cout<<endl;

//m[键值]=实值;
m['b']=4;// 如果键值不存在，则是新增元素  如果键值已存在，则是通过键值修改实值

for(pair<char,int>pr:m){
    cout<<pr.first<<"-"<<pr.second<<" ";	// a-2 b-4 c-3  d-1
}

m.insert(pair<char,int>('f',6));
m.insert(pair<char,int>('f',7));//如果键值存在，则插入失败，键值中迭代器：键值已存在的那个元素，实值bool描述是否插入成功
//map中键值不存在，则迭代器返回的是新插入的元素

ite=m.erase(ite);	//返回的是删除元素的下一个

ite=m.find('f');	//根据键值查找，返回对应的迭代器，如果没有找到则返回end
if(ite!=m.end()){//找到了
    m.erase(ite);
}

m.count('a');	//按键值统计元素的数量 不是0就是1
m.upper_bound('b');	//返回大于该键值的元素的迭代器
m.lower_bound('b');//返回小于等于该键值的元素的迭代器

//不允许通过迭代器修改键值 但是可以通过迭代器修改实值

```

##### set：集合，键值即是实值,自动排序且元素唯一

```c++
set<int> st{4,1,9,3,4,0,4}; 	//0 1 3 4 9
st.insert(6);
pair<set<int>::iterator,bool> pr=st.insert(6); //返回值是 pair 类型（包含 “迭代器 + 插入成功标识”）
ite=st.find(4);
```

##### hash_map哈希表

```cpp
unordered_map<char,int> m{{'c',1},{'a',2},{'y',3},{'x',5}};
//无序map
```

#### 算法<algorithm>

```cpp
void show(int v){
    cout<< v <<" ";
}
---------------------------------------------------------------------------------------------------------------
list<int> lst{1,2,3,4};
::for_each(lst.begin(),lst.end(),&show);   //遍历,范围:左闭右开  1,2,3,4
::for_each(vec.begin(),vec.begin()+2,&show);
void add1(int& v){
    if(v%2==1) ++v;
}
::for_each(vec.begin(),vec.end(),&add1);
--------------------------------------------------------------------------------------------------------------

::count(vec.begin(),vec.end(),8); 	//统计8的个数
-------------------------------------------------------------------------------------------------------------

//对比 vec2 从第二个元素开始的所有元素，与 vec 从第一个元素开始的对应元素，是否全部相等（逐元素用 == 对比）。
::equal(++vec2.begin(),vec2.end(),vec.begin());
//被比较的范围元素的数量如果小于比较元素数量，则报错
-------------------------------------------------------------------------------------------------------------------
   
::find(lst.begin(),lst.end(),3);	//find,按范围查找 
```

####  迭代器

```cpp
list<int>::iterator ite=lst.begin();	//正向迭代器
list<int>::reverse_iterator iterev=lst.rbegin();	//反向迭代器 指向反向的头
ite=iterev.base();	//反向迭代器转为正向迭代器
lst.erase(--ite); 	//反向转正向 转换有偏移
```

#### 容器适配器

主要包括：stack栈适配器，queue队列适配器

```cpp
stack<int> st;	//空栈 底层默认用双端队列实现
st.push(1);
st.push(2);
st.pop();
st.top();

queue<int> que;	//空队列	底层默认用双端队列实现
que.push(1);
que.push(2);
que.back();	//队尾
que.front();//队头
que.pop();	//在队头出
```

#### 仿函数

```cpp
int add(int a,int b){
    return a+b;
}

//仿函数  重载operator()来实现的模拟的效果
int operator() (int a,int b){
    return a+b;
}

```























































































































