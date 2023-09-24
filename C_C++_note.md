# 语法

## static 变量

```txt
静态变量赋初值的时候不允许使用变量。
```

## 宏定义和变量

```txt
命名数字不能放在开头
```

# C++语法

# 堆区开辟空间

```txt
单个变量类型		数据类型（数据值）   eg:  int* p = new int(10);
单个变量类型的释放	delete + 堆区空间地址 eg: delete p;

数组类型			int* arr = new int[10];
数组类型释放			deletep[] arr;
```

# 引用

## 引用的基本用法

```
语法：数据类型 &别名 = 原名
    int &b = a;

作用：给变量取别名
```

## 注意事项

```txt
注意事项：1必须初始化     2引用一旦初始化就不能再更改
```

##  引用做函数参数

```txt
作用：函数传参时，可以利用引用的技术让形参修饰实参
优点：可以简化指针修改实参
```



## 一、virtual虚函数的四种用法

### 1.虚函数

```txt
被修饰的函数称为虚函数, 是C++中多态的一种实现（多说一句，多态分编译时多态-通过重载实现和运行时多态-通过虚函数实现）。 也就是说用父类的指针或者引用指向其派生类的对象,当使用指针或引用调用函数的时候会根据具体的对象类型调用对应对象的函数（需要两个条件：父类的函数用virtual修饰和子类要重写父类的函数）。

使用virtual修饰的函数会根据实际对象的类型来调用，没有使用virtual修饰的根据指针的类型来调用。虚函数最关键的特点是“动态联编”，它可以在运行时判断指针指向的对象，并自动调用相应的函数。
```

```c++
#include <iostream>

class father {
public:
	void func1() {std::cout << "this is father func1" << std::endl;}
	virtual void func2() {std::cout << "this is father func2" << std::endl;
}

class son:public father {
public:
	void func1() {std::cout << "this is son func1" << std::endl;}
	void func2() {std::cout << "this is son func2" << std::endl;
}

int main() {
	father *f1 = new son();
	f1.func1();
	f1.func2();
	return 0;
}
```

**output**

```c++
this is father func1
this is son func2
```



```c++
#include<iostream> 
using namespace std;
class A{
public:
     virtual  void  display(){  cout<<"A"<<endl; }
     };
     
class B :  public A{
public:
            void  display(){ cout<<"B"<<endl; }
     };
     
void doDisplay(A *p)
{
    p->display();
    delete p;
}
 
int main(int argc,char* argv[])
{
    doDisplay(new B());
    return 0;
}
```

**output**

```c++
B
```

### 2.修饰析构函数

```txt
修饰析构函数与上面讲到的使用方法和原理相同，虚析构函数在销毁时会调用对象的析构函数，这样就不会出现像有的数据成员没有销毁导致内存泄露的问题或者程序直接崩溃。

下面也用两个例子说明：
```

```c++
class GrandFather {
public:
	GrandFather() {std::cout << "construct grandfather" << std::endl;}
	~GrandFather() {std::cout << "destruct grandfather" << std::endl;}
};
 
class Father：public GrandFather{
public:
	Father() {std::cout << "construct father" << std::endl;}
	~Father() {std::cout << "destruct father" << std::endl;}
};
 
class Son：public Father{
public:
	Son() {std::cout << "construct son" << std::endl;}
	~Son() {std::cout << "destruct son" << std::endl;}
};
 
int main() {
	Father *f = new Son();
	delete f;
	return 0;
}
```

**output**

```txt
没有son的析构函数，当将Father或者GrandFather其中一个的析构函数修改为virtual后输出就变为了
```

**output**

```c++
construct grandfather
construct father
construct son
destruct son
destruct father
destruct grandfather
```

```c++
#include<iostream>
using namespace std;
class Person{
 public:        Person()  {name = new char[16];cout<<"Person构造"<<endl;}
      virtual  ~Person()  {delete []name;cout<<"Person析构"<<endl;}
 private:
         char *name;
         };
         
class Teacher :virtual public Person{
public:         Teacher(){ cout<<"Teacher构造"<<endl; }
              ~Teacher(){ cout<<"Teacher析构"<<endl; }
};
 
class Student :virtual public Person{
public:         Student(){ cout<<"Student构造"<<endl; }
              ~Student(){ cout<<"Student析构"<<endl; }
};
 
class TS : public Teacher,public Student{
public:             TS(){ cout<<"TS构造"<<endl; }
                 ~TS(){ cout<<"TS析构"<<ENDL; }
};
 
int main(int argc,char* argv[])
{
Person *p = new TS();
delete p;
return 0;
}
```

**output**

```c++
Person构造
Teacher构造
Student构造
TS构造
TS析构
Student析构
Teacher析构
Person析构
```

但是当我们把Person类中析构前面的virtual去掉之后的运行结果为：

```txt
Person构造
Teacher构造
Student构造
TS构造
Person析构
程序崩溃
```

###  3.修饰继承性

假如有这种场景，一个类继承两个或者更多的父类，但是这些父类里又有一些有共同的父类，会出现什么情况呢？

下面给出两个例子：

```c++
#include<iostream> 
using namespace std;
class Person{
   public:    Person(){ cout<<"Person构造"<<endl; }
           ~Person(){ cout<<"Person析构"<<endl; }
};
 
class Teacher : virtual public Person{
   public:    Teacher(){ cout<<"Teacher构造"<<endl; }
            ~Teacher(){ out<<"Teacher析构"<<endl; }
};
 
class Student : virtual public Person{
  public:      Student(){ cout<<"Student构造"<<endl; }
             ~Student(){ cout<<"Student析构"<<endl; }
};
 
class TS : public Teacher,  public Student{
public:   TS(){ cout<<"TS构造"<<endl; }
          ~TS(){ cout<<"TS析构"<<endl; }
};
int main(int argc,char* argv[])
{
    TS ts;
    return 0;
}
```

**output:**

```c++
Person构造 
Teacher构造 
Student构造 
TS构造 
TS析构 
Student析构 
Teacher析构 
Person析构 
```

当Teacher类和Student类没有虚继承Person类的时候，也就是把virtual去掉时候终端输出的结果为：

我们在构造TS的时候需要先构造他的基类，也就是Teacher类和Student类。而Teacher类和Student类由都继承于Person类。这样就导致了构造TS的时候实例化了两个Person类。

```c++
Person构造
Teacher构造
Person构造
Student构造
TS构造
TS析构
Student析构
Person析构
Teacher析构
Person析构
```

```c++
class GrandFather {
public:
	GrandFather() {std::cout << "construct grandfather" << std::endl;}
	~GrandFather() {std::cout << "destruct grandfather" << std::endl;}
};
 
class Father1：public GrandFather{
public:
	Father1() {std::cout << "construct father1" << std::endl;}
	~Father1() {std::cout << "destruct father1" << std::endl;}
};
 
class Father2：public GrandFather{
public:
	Father2() {std::cout << "construct father2" << std::endl;}
	~Father2() {std::cout << "destruct father2" << std::endl;}
};
 
class Son：public Father1, Father2{
public:
	Son() {std::cout << "construct son" << std::endl;}
	~Son() {std::cout << "destruct son" << std::endl;}
};
 
int main() {
	Father *f = new Son();
	delete f;
	return 0;
}
```

**output:**

```c++
construct grandfather
construct father1
construct grandfather
construct father2
construct son
destruct son
destruct father2
destruct grandfather
destruct father1
destruct grandfather
```

通过这个例子我们看到创建一个son会创建两个grandfather，不符合我们的预期啊，而且还可能会导致程序挂掉。这里就请virtual出场了，当把father1和father2继承grandfather修改为virtual继承(也就是在public前面加一个virtual)的时候输出会变成这样：
**output:**

```c++
construct grandfather
construct father1
construct father2
construct son
destruct son
destruct father2
destruct father1
destruct grandfather
```



### 4.纯虚函数

纯虚函数的定义是在虚函数的后面加一个=0。定义了纯虚函数的类是一个抽象类。

纯虚函数需要注意这几点：
1.定义了纯虚函数的类不能够实例化，也就是不能够创建对象
2.继承了含有纯虚函数的父类的子类如果没有实现纯虚函数也不能够实例化

```c++
virtual void func() = 0;
```



## 二、虚基类