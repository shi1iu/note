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

### 3.tcp类

3.1获取本地ip地址函数

```C++
QString Widget::getLocalIP()
{
    QString hostName=QHostInfo::localHostName();  //本地主机名
    QHostInfo   hostInfo=QHostInfo::fromName(hostName);
    QString   localIP="";
    QList<QHostAddress> addList=hostInfo.addresses();
    if (addList.isEmpty())
        return localIP;
    foreach(QHostAddress aHost, addList)
        if (QAbstractSocket::IPv4Protocol==aHost.protocol())
        {
            localIP=aHost.toString();
            break;
        }
    return localIP;
}
```

### 4.foreach()函数

> 在 Qt 中，`foreach` 是一个宏，用于方便地遍历容器（如 `QList`、`QVector`、`QStringList` 等）中的元素。
