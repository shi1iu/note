## C++

## 一、1两数求和

```c++
class Solution {
public:
  vector<int> twoSum(vector<int>& nums, int target) {
  }
};
```

- `vector <int>` 是c++里面的动态数组，不需要手动分配内存

- `vector <int> &` 是引入的参数，避免拷贝

  **class**   在C++中，类里面可以封装函数，c语言中，是独立的函数
  
  
  
  1.1 哈希表
  
  哈希表就是用来快速判断一个元素是否出现集合里,*键为数值，值为索引*
  
  unordered_map为例，map[键]=值这个映射关系。
  
  
  
  1.2 `std::`是什么意思
  
  *using namespace std *作用是允许程序员在不指定命名空间的情况下使用标准库中的名称。(在程序的开头加上这句话)。
  
  eg：std:: map ，就说在标准库中，使用map功能。
  
  map.count ：来检查是否存在某个键,`count`直接返回布尔值。
  
  map.find：`find`需要与`end()`迭代器比较。
  
  
  
  1.3 `vector`是什么？
  
  ​	是一个序列容器，能自动分配内存的。
  
  1.4 vector <int > vec(2); 这里的(2)是数组吗？和C语言不一样吗？
  
  ​	这里的vec是向量，2代表向量的初始大小。
  
  1.5 it->second，这里的second代表it的第二个值，也就是值，也就是索引。
  
  1.6 answer2的逻辑
  
  
  
  answer1：哈希表
  
  ```c++
  class Solution {
  public:
    vector<int> twoSum(vector<int>& nums, int target) {
  	unordered_map <int,int > map;
  	vector <int > vec(2);
  	for(int i=0;i<nums.size();i++)
  	{
  		if(map.count(target-nums[i]))
  		{
   			vec[0]=i;
  			vec[1]=map[target-nums[i]];
  			break;
          }
  		map[nums[i]]=i;
  	}
  	return vec;
    }
  };
  ```
  
  answer2：哈希表
  
  ```c++
  class Solution {
  public:
      vector<int> twoSum(vector<int>& nums, int target) {
          unordered_map <int,int > map;
          for(int i=0;i<nums.size();++i)
          {
              auto it =map.find(target-nums[i]);
              if(it!=map.end())
              {
                  return {it->second,i};
              }
              map[nums[i]]=i; 
          }
          return {};
      }
  };
  ```
  
  answer3：暴力破解

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {};
    }
};

```

## 二、13.罗马数字转整数

```C++
class Solution {
public:
    int romanToInt(string s) {
        
    }
};
```

2.1如何插入数据到map中

## 2025_4_16学习

#### 1.C++中为什么赋值小数会加f

```c++
float 加f   = 8000000.7499999996f;
float 不加f = 8000000.7499999996;jia
```

加f可能更加准确一点。

#### 2.**::**的作用

> `::`是一个新符号，称为域解析操作符，在C++中用来指明要使用的命名空间。

```c++
Li::fp = fopen("one.txt", "r");  //使用小李定义的变量 fp
Han::fp = fopen("two.txt", "rb+");  //使用小韩定义的变量 fp
```

#### 3.using关键字

> 主要用于简化命名空间的使用、声明类型别名以及在派生类中引入基类成员。

using 声明不仅可以针对命名空间中的一个变量，也可以用于声明整个命名空间。

eg:`using namespace std`

#### 4.namespace 命名空间

> 是用于解决命名冲突和组织代码的一种机制。

C语言中两个函数相同，可能重定义了，在C++中，可以通过namespace解决冲突定义。

eg:`using namespace std`

```C++
namespace n1
{
	float	pai =  3.1415926; //这里的n1空间就被定义出来，如果需要就可以使用n1这个空间
}
```

````c++
namespace N1 // N1为命名空间的名称
{
	// 命名空间中的内容，既可以定义变量，也可以定义函数，也可以定义类型
	int a; //变量
	int Add(int left, int right) //函数 
	{
		return left + right;
	}
    struct ListNode //类型
    {
        int val;
        struct ListNode* next;
    }
 
}
````

## 2025_4_18学习

#### 1.vscode C++ 运行问题

运行出现Cout 参数错误

将环境变量放在Path的前面

#### 2.分配空间与释放空间

```c++
int *p =new int ;
delete p;
```

## 2025_4_21学习

#### 1.C++中struct中可以写函数

类体定义成员函数

```c++
struct student{//体内
	string name;
	double score;
	void print(){cout << name << " " << score << endl;}; //函数声明
}
```

```C++
struct student
{
    string name;
    double score;
    void print();
};

void student::print()
{
    cout << name << " " << score << endl;
}
```

C语言中struct不能定义函数成员，而C++中可以，不过为了区分建议用class，且class有封装特性，自行决定成员公开还是私有，struct默认所有成员公开!!!

class定义类，对象是类的实例。

```C++
class student{
private:
	string name;
	double score;
public: //接口
	void print() {cout << this->name << " " << this->score << endl;}
	string get_name() { return name; }
	double get_score() { return score; }
	void set_name(string n) { name = n; }
	void set_score(double s) { score = s; }
};
```

#### 2.const关键字

> `const` 指针是指针变量的值一经初始化，就不可以改变指向，初始化是必要的。

```C++
const int * pOne;    //指向整形常量的指针，它指向的值不能修改(可以修改地址)

int * const pTwo;    //指向整形的常量指针 ，它不能在指向别的变量，但指向（变量）的值可以修改。 (可以修改值，地址没法修改)

const int *const pThree;  //指向整形常量 的常量指针 。它既不能再指向别的常量，指向的值也不能修改。
```

#### 3.类的指针和结构体指针

类的私有成员无法通过指针直接访问

```C++
class student 
{
	public :
	
	private:
};
```

#### 4.内联函数

> 为了消除函数调用的时空开销，C++ 提供一种提高效率的方法，即在编译时将函数调用处用函数体替换，类似于C语言中的宏展开。

```c++
inline void swap()
{
    
}  //这里的inline就是内联函数的标志，尽量不使用较多的代码段函数。
```

#### 5.函数重载

```C++
void swap(int *a, int *b);      //交换 int 变量的值
void swap(float *a, float *b);  //交换 float 变量的值
void swap(char *a, char *b);    //交换 char 变量的值
void swap(bool *a, bool *b);    //交换 bool 变量的值
```

使用一个函数名就可以使两数进行交换。

#### 6.类和对象

> 类是创建对象的模板，一个类可以创建多个对象，每个对象都是类类型的一个变量；创建对象的过程也叫类的实例化。每个对象都是类的一个具体实例（Instance），拥有类的成员变量和成员函数。

有些教程将类的成员变量称为类的**属性（Property）**，将类的成员函数称为类的**方法（Method）**。在面向对象的编程语言中，经常把**函数（Function）**称为**方法（Method）**。

## 2025_4_22学习

#### 1.引用

> 给变量起别名

语法：  数据类型 & 别名 = 原名

```C++
int main ()
{
	int a = 10;
    int &b = a;
    
    cout << "a="<< a << endl; 
    cout << "b="<< b << endl;
    
    b=100;
    cout << "a="<< a << endl; 
    cout << "b="<< b << endl;
    
    system("pause");
	return 0;
    
}
```

注意事项：

- 引用必须初始化  `int &b;`是错误的
- 引用在初始化后，不可以改变

#### 2.引用做函数参数

```C++
int myswap(int &a ,int &b) //通过引用建立形参和实参的关系
{
    int temp = a ; 
	a = b;
    b = temp; 
}
```

在做参数的时候，如果发送的具体的值，系统会分配一个`const`变量，传递给函数

```C++
void myswap1(int &a)
{

}

int main()
{
    myswap1(10);//这里传递的10，实际是分配了空间const int * a= 10; 这是合法的。
}
```

#### 3.引用做函数返回值

> 引用是可以作为函数的返回值存在的、

- 不要返回局部变量引用
- 函数调用作为左值

```C++
//返回局部变量引用
int & test01()
{
    int a = 100;
    return a;
}

//返回全局变量引用
int & test02()
{
    static int a = 100;
    return a;
}

int main()
{
	/* 这里会报错，因为a的值在test01会被释放，在栈内
    int &ref = test01(); //返回的a,与ref作为引用
    cout << "ref = " << ref <<endl;
    cout << "ref = " << ref <<endl;
    */
    
    int &ref = test02();//值存放在全局变量区
    cout << "ref = " << ref <<endl;
    cout << "ref = " << ref <<endl;   
    
    test02() = 1000; // 引用作为返回值时，函数可以作为左值
    
    cout << "ref = " << ref <<endl;//ref在前面就已经和a引用起来了
    cout << "ref = " << ref <<endl;  
    
    system ("pause");
    return 0;
}
```

#### 4.引用的本质

> 在C++内部实现是一个指针常量

#### 5.常量引用

> 常量引用主要用来修饰形参，防止误操作

```C++
void showValue(const int &v)
{
    // v += 10;
    cout << v << endl;
}
```

在main函数内，设置常量引用，不会将函数的值发生变化，可以在右，不能在左。

#### 6.函数默认参数

```C++
//自己传入数据，就用自己的数据，如果没有，就用默认值。
//语法：返回值类型 函数名 （形参 = 默认值） { }
int fun (int a ,int b=10,int c=20)
{
    return a + b + c;
}
```

- 第一个默认参数后面必须都是默认参数。
- 函数的声明如果有默认参数了，函数实现就不能有默认参数了。（两者只能有一个有默认参数）

```C++
//声明
int fun (int a,int b=10);
//实现
int fun (int a=10 ,int b)
{
    return a + b ;
}
//这种也是可行的
```

#### 7.函数占位参数

> C++中函数的形参列表里可以有占位函数，用来做占位，调用函数时必须填充该函数。

语法： `返回值类型 函数名 (数据类型) {}`

```C++
void func (int a ,int)
{
	cout << "this is func" <<endl;
}
```

#### 8.函数重载

> 函数名可以相同，提高复用性

满足条件：

- 同一作用域下
- 函数名相同
- 函数参数**类型不同**或者**个数不同**或者**顺序不同**

函数返回值不可以作为函数重载的条件

1. 引用作为重载的条件

```C++
void fun (int &a)
{
    cout << "1"<<endl;
}

void fun (double a)
{
    cout << "2"<<endl;
}

void fun (const int &a)
{
    cout <<"3"<<endl;
}

int main()
{
	int b=100;
    fun(b);//3
    
    const int c= 10;
    fun(c); //1
    
    fun(10); //2,如果没有void fun (fouble a),则是3,因为const int &a = 10;是合法的

    system ("pause");
    return 0;
}
```

2. 函数重载碰到默认参数

```C++
void fun (int a, int b =10)
{
    cout << "1"<<endl;
}

void fun (int a)
{
    cout << "2"<<endl;
}


int main()
{

    fun (20,10); //1
    fun (10);//出现二义性，报错，编译器找不到运行哪一个函数，尽量避免这种情况

    system ("pause");
    return 0;
}
```

#### 9.类和对象

