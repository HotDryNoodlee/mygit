- [基础进阶](#基础进阶)
	- [const的作用](#const的作用)
	- [指针](#指针)
	- [枚举类](#枚举类)
	- [内联函数](#内联函数)
	- [构造函数](#构造函数)
	- [类的继承](#类的继承)
	- [2023 7 14](#2023-7-14)

## 基础进阶

### const的作用
* 类型检查:
const常量与#define宏定义常量的区别：__const常量具有类型，编译器可以进行安全检查；#define宏定义没有数据类型，只是简单的字符串替换，不能进行安全检查。__

* 
### 指针

* 如果不想修改实参，就用值传递，如果想修改实参，就用地址传递

* 当数组名传入到函数作为参数时，被退化为指向首元素的指针

### 枚举类
* 限定作用域的枚举类
在enum后面加关键字class或者struct
```c++
    enum class color
    {
    	RED,
    	GREEN,
    	BLUE
    };
```
* 解决枚举值重名的问题，保持代码的可读性
```c++

//定义两种枚举
	enum class color_inner
	{
		RED,
		GREEN,
		BLUE
	};
 
	enum color_out
	{
		RED,
		GREEN,
		BLUE
	};
    //声明并赋值
    color_out backColor = RED; //正确
    color_inner forntColor = RED; //错误，默认使用了out中的RED，没有指定作用域
    color_out backColor = color_out::RED; //正确，out也可以显示指定作用域
    color_inner forntColor = color_inner::RED;  //正确，inner必须指定作用域
```
* 枚举类赋值
可以部分指定，未被初始化的枚举值的值默认将比其前面的枚举值大1
```c++
	enum color
	{
		RED=2,  
		GREEN,  //默认值是3，比前一个多1
		BLUE=7  
	};
```
可以部分指定，未被初始化的枚举值的值默认将比其前面的枚举值大1
```c++
	enum color
	{
		RED=2,  
		GREEN,  //默认值是3，比前一个多1
		BLUE=7  
	};
```
* 指定枚举类型
C++11中，还可以指定给枚举类型赋值的整数类型，在enum的名字后面加上冒号以及指定的类型，限定作用域枚举默认为32位整形，在某些情况下，甚至没必要用到32位，为了节省开销，甚至可以用8位整形unsigned char，将类型指定成后，枚举变量变成了8位整型，减少了内存使用。不限定作用域的枚举类型，其成员不存在默认类型，只需要知道潜在类型是足够大的，肯定能容纳枚举值就行。
需要注意的是，不能指定为float或者double等类型，因为枚举量必须是一个整数，float和double都不是整数。
```c++
	enum color:unsigned long
	{
		RED=1,  
		GREEN=5, 
		BLUE=7  
	};
```







***
### 内联函数

* inline（小心，不是online），翻译成“内联”或“内嵌”。意指：当编译器发现某段代码在调用一个内联函数时，它不是去调用该函数，而是将该函数的代码，整段插入到当前位置。这样做的好处是省去了调用的过程，加快程序运行速度。（函数的调用过程，由于有前面所说的参数入栈等操作，所以总要多占用一些时间）。这样做的不好处：由于每当代码调用到内联函数，就需要在调用处直接插入一段该函数的代码，所以程序的体积将增大。内联函数的本质是，节省时间但是消耗空间。

* inline函数的规则：
1. 含有递归调用的函数不能设置为inline;
2. 使用了复杂流程控制语句：循环语句和switch语句，无法设置为inline；
3. 由于inline增加体积的特性，所以建议inline函数内的代码应很短小。最好不超过５行;
4. inline仅做为一种“请求”，特定的情况下，编译器将不理会inline关键字，而强制让函数成为普通函数。出现这种情况，编译器会给出警告消息;
5. 在你调用一个内联函数之前，这个函数一定要在之前有声明或已定义为inline,如果在前面声明为普通函数，而在调用代码后面才定义为一个inline函数，程序可以通过编译，但该函数没有实现inline。比如下面代码片段：
```c++
	//函数一开始没有被声明为inline: 
	void foo(); 
	//然后就有代码调用它： 
	foo(); 
	//在调用后才有定义函数为inline: 
	inline void foo() 
	{ 
	   ...... 
	} 
	//代码是的foo()函数最终没有实现inline
```
6. 为了调试方便，在程序处于调试阶段时，所有内联函数都不被实现。

* 使用内联函数时应注意以下几个问题：

1. 在一个文件中定义的内联函数不能在另一个文件中使用。它们通常放在头文件中共享。()
2. 内联函数应该简洁，只有几个语句，如果语句较多，不适合于定义为内联函数。
3. 内联函数体中，不能有循环语句、if语句或switch语句，否则，函数定义时即使有inline关键字，编译器也会把该函数作为非内联函数处理。
4. 内联函数要在函数被调用之前声明。关键字inline 必须与函数定义体放在一起才能使函数成为内联，仅将inline 放在函数声明前面不起任何作用。
5. 内联函数必须是和函数体申明在一起，才有效。

* 下面申明的三个函数都是内联函数。在C++中，在类的内部定义了函数体的函数，被默认为是内
联函数。而不管你是否有inline关键字。我们也可以将定义在类的外部的函数定义为内联函数.
```c++
	class tableClass{
		private:
			int i,j;
		public:
			int add() { return iI+j;};
			inline int dec() { return i-j;}
			int GetNum();
	}
	inline int tableclass::GetNum(){
	    return i;
	}
```

* 内联函数在C++类中，应用最广的，应该是用来定义存取函数。我们定义的类中一般会把数据成员定义成私有的或者保护的，这样，外界就不能直接读写我们类成员的数据了。
```c++
	class sample{
		private:
			int nTest;
		public:
			int readtest(){ return nTest; }
			void settest(int i) { nTest=i; }
	}
```

***** 

### 构造函数
* 在C++类的构造函数中经常会看到如下格式的写法：
```c++
	MyWindow::MyWindow(QWidget* parent , Qt::WindowFlags flag) : QMainWindow(parent,flag)
```
上述语句中单冒号(:)的作用是表示后面是初始化列表，一般有三种使用场景。
  1. 对父类进行初始化
```C++
	MyWindow::MyWindow(QWidget* parent , Qt::WindowFlags flag) : QMainWindow(parent,flag)
```
  2. 对类成员进行初始化
```C++
	class rectangle //头文件中类定义
	{
	public:
	    rectangle( int pointX, int pointY, int Width, int Length );
	private:
	    CPoint m_point;
	    int m_Width;
	    int m_Length;
	};
	
	rectangle::rectangle(int pointX, int pointY, int Width, int Length) : m_point(pointX,pointY),m_Width(Width),m_Length(Length)//源文件中构造函数实现
	{
	    todo......
	}
```
  3. 对类的const成员变量进行初始化<br>
   		由于const成员变量的值无法在构造函数内部初始化，因此只能在变量定义时赋值或使用初始化列表赋值。


*******
### 类的继承
* 派生类不可直接访问基类的private成员，可通过基类的共有成员函数访问
- 基类中protected的成员
  - 类内部：可以访问
  - 类的使用者：不能访问
  - 类的派生类成员：可以访问
- 基类中private的成员
  - 类内部：可以访问
  - 类的使用者：不能访问
  - 类的派生类成员：不能访问
* 派生方式为protected的继承称为保护继承，在这种继承方式下，基类的public成员在派生类中会变成protected成员，基类的protected和private成员在派生类中保持原来的访问权限

### 2023 7 14