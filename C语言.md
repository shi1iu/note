### 1.makefile

> 构建工具。

```makefile
目标文件 : 依赖文件
	命令
```

这里的编译和链接是在一起的,没有生成xxx.o文件

```makefile
a : a.cpp m_point.cpp
	g++ a.cpp m_point.cpp -o a
```

这里生成a.o 和m_points.o是先进行编译产生，再链接a 这个可执行文件

```makefile
a : a.o m_point.o
	g++ a.o m_point.o -o a

a.o : a.cpp 
	g++ -c a.cpp 

m_point.o : m_point.cpp
	g++ -c m_point.cpp
```



#### 1.伪目标

> ​	不生产文件的目标，用来执行一些操作，只是一个标签。

- clean

```makefile
.PHONE:clean //显示的告诉Make这个clean就是一个伪目标
```

```makefile
.PHONY : clean all
all : a c 
	@echo "a & c"  #这里的@符号是在终端上不显示
# a : a.o m_point.o
#	g++ a.o m_point.o -o a  //下面和这里是一样的

a c : a.o m_point.o
	g++ a.o m_point.o -o $@  #这里的$@符号是代表目标文件(double)

a.o : a.cpp 
	g++ -c a.cpp 

m_point.o : m_point.cpp
	g++ -c m_point.cpp

clean :
	DEL *.o a.exe


```

#### 2.如何添加注释

```makefile
#这里可以加注释
main : main.c # 这里可以加注释
	g++ main.c -o main  #这里不要加注释！！！！
```

#### 3.定义变量

```makefile
CFLAGS = -Wall -g -02 #这些是编译选项变量
targets = main #这是目标文件
sources = main.cpp message.cpp
objects = main.o message.o

main : main.cpp
	g++ $(CFLAGS) main.cpp -o $@

```

#### 4.自动变量

- `&@`表示目标文件
- `&<`表示第一个依赖文件
- `&^`表示所有依赖文件
- `%`表示通配符

#### 5.如果使用makefile时改名

`make -f xxx.mk` 用于修改makefile名字的

`make -n`用于调试

### 2.Cmake

```Cmake
cmake -G "MSYS Makefiles" -DCMAKE_C_COMPILER="C:\msys64\ucrt64\bin\gcc.exe" -DCMAKE_CXX_COMPILER="C:\msys64\ucrt64\bin\g++.exe" ..
```

### 3.2025_5_6学习

#### 1.->和.的区别

A->a   其中A为指针

A.a  A为结构体或者对象
