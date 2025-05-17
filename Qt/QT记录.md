# QT记录

### 1.2025_5_15记录

#### 1.1connect函数使用

```C++
connect(ui->browseButton,SIGNAL(clicked()),this,SLOT(message()));
```

这里面中，connect函数的SIGNAL中，没有作用域，只有某个类型的信号就好了。

**信号签名无需加入类名**

### 2.2025_5_16记录

#### 2.1QObject定时器的使用

1. 创建信号与槽的模型，写定时器 starttimer()定时。
2. 构造虚函数，timerEvent()函数，写函数实现的代码。