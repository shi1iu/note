# C++类和对象

1. 封装
2. 继承
3. 多态

## 一、封装

### 1.属性和行为作为整体

```C++
#include <iostream>
#include <string>

using namespace std;

//设计一个圆类求周长

class cicle
{
    public:
    float R;
    float pi = 3.1415926;
    float calcute() {return 2*R*pi; }
};


int main()
{

    cicle yuan;
    yuan.R = 2;
    cout <<yuan.calcute()<<endl;
    system ("pause");
    return 0;
}

```

实例：学生姓名+ID

```C++
#include <iostream>
#include <string>

using namespace std;

class student 
{
    private:
        string name;
        string id ;
    public:
    void setname(string name_a)
    {
        name =name_a ;
    }

    void setid(string id_a)
    {
        id =id_a;
    }
    void displayname()
    {
        cout <<"name:"<<name<<endl;
    }
    void displayid()
    {
        cout <<"id:"<<id<<endl;
    }
};

int main()
{

    student std;
    std.setname("shiliu");
    std.setid("2020082128");
    std.displayname();
    std.displayid();

    system ("pause");
    return 0;
}
```

### 2.访问权限

1. public        公共权限  
2. protected 保护权限
3. private      私有权限

- protected 和private 有区别，private是完全私有，protected在继承中可能会被其他访问到。

### 3.struct和class区别

在C++中 struct和class唯一的**区别**就在于 **默认的访问权限不同**

区别：

* struct 默认权限为**公共**
* class   默认权限为**私有**

### 4.成员属性设置为私有

**优点1：**将所有成员属性设置为私有，可以自己控制读写权限

xxxxxxxxxx cmake -G "MSYS Makefiles" -DCMAKE_C_COMPILER="C:\msys64\ucrt64\bin\gcc.exe" -DCMAKE_CXX_COMPILER="C:\msys64\ucrt64\bin\g++.exe" ..Cmake

立方体类实例

```C++
#include <iostream>
#include <string>
using namespace std;
class Cube
{
    public :
        void setm_L(float L)
        {
            m_L = L;
        }

        void setm_W(float W)
        {
            m_W = W;
        }

        void setm_H(float H)
        {
            m_H = H;
        }

        float getm_L()
        {
            return m_L;
        }

        float getm_W()
        {
            return m_W;
        }
        float getm_H()
        {
            return m_H;
        }

        float mianji()
        {
            return 2*((m_L*m_H)+(m_L*m_W)+(m_W*m_H));
        }

        float tiji()
        {
            return m_L*m_W*m_H;
        }
        
        bool issame(Cube &pc)
        {
            if(getm_H()==pc.getm_H()&&getm_L()==pc.getm_L()&&getm_W()==pc.getm_W())
            {
                return true;
            }
            else
            {
                return false;
            }
        }

    private:
        float m_L;
        float m_W;
        float m_H;
};

bool isSame(Cube &p1,Cube &p2)
{
    if(p1.getm_H()==p2.getm_H()&&p1.getm_L()==p2.getm_L()&&p1.getm_W()&&p2.getm_W())
    {
        return true;
    }
    else
    {
        return false;
    }
}

int main()
{
    Cube cl;
    cl.setm_L(10);
    cl.setm_W(10);
    cl.setm_H(10);
    cout<<"mianji :"<<cl.mianji()<<endl;
    cout<<"tiji :"<<cl.tiji()<<endl;

    Cube cl2;
    cl2.setm_H(100);
    cl2.setm_L(10);
    cl2.setm_W(10);

    cout <<"the circle is same ?"<<"\t"<<(cl2.issame(cl)?"same":"not same")<<endl;

}
```

文件需要.h和.cpp的形式

`a.cpp`

```C++
#include <iostream> 
#include <string>
#include <math.h>
#include "m_point.h"
#include "circle.h"
using namespace std;


int main()
{
    m_circle cir1;
    m_point poi1;

    cir1.setm_xy(0,0,1);
    poi1.setm_xy(0,0);
     
    cir1.guanji(poi1);

}
```

`m_point.h`

```C++
#pragma once

#include<iostream>

using namespace std;

class m_point
{
    public :
        void setm_xy(int x,int y);


        int getx();


        int gety();

    private :
        int m_x;
        int m_y;
};

```

`m_point.cpp`

```C++
//#pragma once 
#include"m_point.h"


void m_point::setm_xy(int x,int y)
{
    m_x=x;
    m_y=y;
}

int m_point::getx()
{
    return m_x;
}

int m_point::gety()
{
    return m_y;
}
```

`circle.h`

```C++
#pragma once 

#include "m_point.h"

class m_circle
{
    public :
        void setm_xy(int x,int y,double R);
        double calculatejuli(m_point point);
        void guanji(m_point & points);
    private :
        int m_x;
        int m_y;
        double m_R;

};
```

circle.cpp

```C++
#include "circle.h"
#include <math.h>

        void m_circle::setm_xy(int x,int y,double R)
        {
            m_x=x;
            m_y=y;
            m_R= R;
        };

        double m_circle::calculatejuli(m_point point)
        {
            return sqrt(
                (m_x-point.getx())*(m_x-point.getx())
                +
                (m_y-point.gety())*(m_y-point.gety())
            );
        };

        void m_circle::guanji(m_point & points)
        {
                if(m_R==calculatejuli(points))
                {
                    cout<<"up"<<endl;
                }

                if(m_R<calculatejuli(points))
                {
                    cout<<"out"<<endl;
                }


                if(m_R>calculatejuli(points))
                {
                    cout<<"in"<<endl;
                }
                
        };

```

`makefile`

```makefile
.PHONY : clean all

CFLAGS = -Wall -g 
TARGET = a 
SOURCES = a.cpp m_point.cpp circle.cpp
OBJECTS = a.o m_point.o circle.o

# a : a.o m_point.o
# 	g++ a.o m_point.o -o a

$(TARGET) : $(OBJECTS)
	g++ $(CFLAGS) $(OBJECTS) -o $@

%.o : %.cpp 
	g++ $(CFLAGS) -c $< -o $@ 

clean :
	DEL *.o *.exe
```

## 二、对象的特性

### 1.对象需要初始化和清理

**构造函数**和**析构函数**



c++利用了**构造函数**和**析构函数**解决上述问题，这两个函数将会被编译器自动调用，完成对象初始化和清理工作。

对象的初始化和清理工作是编译器强制要我们做的事情，因此如果**我们不提供构造和析构，编译器会提供**

**编译器提供的构造函数和析构函数是空实现。**



**构造函数语法：**`类名(){}`

1. 构造函数，没有返回值也不写void
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以发生重载
4. 程序在调用对象时候会自动调用构造，无须手动调用,而且只会调用一次



**析构函数语法：** `~类名(){}`

1. 析构函数，没有返回值也不写void
2. 函数名称与类名相同,在名称前加上符号  ~
3. 析构函数不可以有参数，因此不可以发生重载
4. 程序在对象销毁前会自动调用析构，无须手动调用,而且只会调用一次



```C++
#include<iostream>

using namespace std;

class Person 
{
    public:
    //构造函数  
    Person()
    {
        cout <<"creat"<<endl;
    }

    ~Person()
    {
        cout <<"xi gou"<<endl;
    }

};

void test1()
{
    Person p;//栈区
}


int main ()
{
    test1();
    
    //Person p1;
    system ("pause");
    return 0;
}
```



### 2.构造函数的分类和调用

分类：

- 有参构造和无参构造（参数分） 
- 普通构造和拷贝构造（类型分） 拷贝构造必须是引用的形式

调用方式：

- 括号法
- 显示法
- 隐式转换法

**括号法**：

```C++
Person(Person p2)//这里的构造函数，引入了Person 的类，与此同时，又调用了构造函数，与发生栈溢出的，如果一直调用的话。
{

}
```

```C++
Person p1;       //调用无参构造
Person p2(10);  //调用有参构造
Person p3(p2);  //调用拷贝构造

//注意1：调用无参构造函数不能加括号，如果加了编译器认为这是一个函数声明
//Person p2();
```

注：

```C++
Person(Person * p2)//这里的构造函数是有参构造，而不是拷贝构造
{
	...
}

最规范的方式
Person(const Person &p1) //加const是为了解决不影响另外一个类。
{
    
}
```

**显示法**：

```C++
Person p1;
Person p2 = Person(10);//有参
Person p3 = Person(p2);//拷贝
Person(10); //匿名对象 特点：当前行执行结束后，系统会立即回收掉匿名对象

//注意2：不能利用 拷贝构造函数 初始化匿名对象 编译器认为是对象声明
//Person(p3);
```

**隐式转换法**:

```C++
Person p4 = 10;  相当于Person p4 =  Person(10)
Person p5 = p4;
```

### 3.拷贝构造函数的调用时机

C++中拷贝构造函数调用时机通常有三种情况

* 使用一个已经创建完毕的对象来初始化一个新对象
* 值传递的方式给函数参数传值
* 以值方式返回局部对象

### 4.构造函数的调用时机

默认情况下，c++编译器至少给一个类添加3个函数

1．默认构造函数(无参，函数体为空)

2．默认析构函数(无参，函数体为空)

3．默认拷贝构造函数，对属性进行值拷贝

### 5.构造函数调用规则

构造函数调用规则如下：

* 如果用户定义有参构造函数，c++不在提供默认无参构造，但是会提供默认拷贝构造


* 如果用户定义拷贝构造函数，c++不会再提供其他构造函数

### 6.深浅拷贝

```C++
//浅拷贝，值传递
  Person(int age,int height)
  {
	 m_age = age;
     m_height= new int(height); //将指针给新的空间
  }

Person (const Person & p)
{
    m_age=age;
    m_height = new int (*p.m_height);//将值给新的地址空间
}
```

> 总结：如果属性有在堆区开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题

### 7.初始化列表

```c++
//初始化列表方式初始化
Person(int a, int b, int c) :m_A(a), m_B(b), m_C(c) {}
```

### 8.类对象作为类成员

```c++
//当类中成员是其他类对象时，我们称该成员为 对象成员
//构造的顺序是 ：先调用对象成员的构造，再调用本类构造
//析构顺序与构造相反
```

### 9.静态成员
静态成员变量特点：

1. 在编译阶段分配内存
2. 类内声明，类外初始化
3. 所有对象访问同一份数据

静态成员函数特点：

1. 程序共享一个函数
2. 静态成员函数只能访问静态成员变量（因为无法区分是哪个对象的非静态成员变量）静态成员函数不属于某个对象。

### 10.成员变量和成员函数分开存储
