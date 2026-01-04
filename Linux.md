Linux

### 1.FTP服务

**FTP（File Transfer Protocol）**是“文件传输协议”，主要作用是：

| 功能         | 说明                                                     |
| ------------ | -------------------------------------------------------- |
| 上传文件     | 将文件从客户端传到服务器                                 |
| 下载文件     | 从服务器获取文件到本地                                   |
| 浏览远程目录 | 支持目录操作（cd、ls、mkdir）                            |
| 权限控制     | 可设置用户名、密码、读写权限等                           |
| 跨平台       | Windows ↔ Linux，Linux ↔ Linux，Windows ↔ Windows 都支持 |

vsftpd

| 缩写部分 | 含义                                   |
| -------- | -------------------------------------- |
| **v**    | Very（非常）                           |
| **s**    | Secure（安全）                         |
| **ftp**  | File Transfer Protocol（文件传输协议） |
| **d**    | Daemon（守护进程，即后台运行的服务）   |

`systemctl`是一个在Linux系统中用于管理系统服务的命令

FTP文件目录不能加载出来

FileZilla   里设置

![image-20251017145200047](C:/Users/enoug/AppData/Roaming/Typora/draftsRecover/assets/image-20251017145200047.png)

### 2.linux下的GDB

[](https://blog.csdn.net/weixin_45031801/article/details/134399664)

### 3.虚拟机卡顿怎么办？

设置中将3D加速关闭，选项中将捕捉的默认，写成高。

### 3.[IPC](https://zhida.zhihu.com/search?content_id=224223692&content_type=Article&match_order=1&q=IPC&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjMwODQzODMsInEiOiJJUEMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMjQyMjM2OTIsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.73uxRcFaAvv_lqjvY0SZt4it0yuIsfHQZqeGBugSHT0&zhida_source=entity)进程间通信

`ipcs`命令查看系统中的消息队列、共享内存、信号量的命令：

<img src="C:/Users/enoug/AppData/Roaming/Typora/draftsRecover/assets/image-20251112100739540.png" alt="image-20251112100739540" style="zoom: 67%;" />

删除命令：

```shell
ipcrm -m "id"   //删除共享内存
ipcrm -s "id"   //删除信号量
ipcrm -q "id"   //删除消息队列
```

#### 3.1 管道

> 无名和有名管道都属于文件，管道通信实则都是在对文件进行读写操作。 
>
> 两者都具管道的特性：先进先出（追加写文件，从头读文件并清除已读内容）

##### 3.1.1 无名管道（pipe）

​	只能在具**有亲缘关系**的进程间使用；

​	读端和写端可以同时存在，所以可以双工通信；根据管道特性，虽然可以读写同时存在，但是通常读写都是相对的， 为了避免自写自读的情况发生，我们**只保留一组读写端进行单工通信**。

##### 3.1.2 有名管道（fifo）

​	**First In First Out**

​	可以在**无亲缘关系**的进程间使用；

#### 3.2 内存映射

#### 3.3 信号

#### 3.4 信号量

#### 3.5 消息队列

#### 3.6 socket通信（支持多机形式）

### 4.Bootloader和u-boot

Bootloader

是程序启动的总称

Bootloader的系统启动过程

1、第一阶段：

初始化最小硬件，始终，DDR，串口

为加载下一阶段做准备

2、第二阶段

初始化更多硬件（EMMC、NAND、ETH）

提供命令行功能

加载Linux内核

加载设备树、ramdisk

跳转到内核执行

5.sudo完指令后，会改变文件的属性

在使用sudo一个命令时，导致文件属性全部变成root，需要修改文件属性才行`sudo chown -R $USER:$USER u-boot`,解决问题。

### 5.`int system(const  )`

### 6. U-Boot 命令有哪些分类？

#### 6.1环境管理命令

- `printenv`
- `setenv`
- `saveenv`

#### 6.2存储器访问命令

- `md` 读内存
- `mw` 写内存
- `mm` 逐字节/32bit 修改
- `cmp` 比较内存区域

#### 6.3设备存储命令

MMC/SD：

- `mmc list`
- `mmc rescan`
- `fatload`
- `fatwrite`

NAND：

- `nand erase`
- `nand write`
- `nand read`

eMMC：

- `mmc write`
- `mmc erase`

#### 6.4网络命令

- `ping` 测试网络
- `tftp` 下载文件
- `dhcp` 自动获取 IP

#### 6.5启动命令

- `bootm`（启动 uImage）
- `bootz`（启动 zImage）
- `go`（跳转到裸机程序入口执行）

#### 6.6文件系统命令

- `ext4load`
- `fatls`
- `fatsize`

#### 6.7脚本执行类

- `run bootcmd`
- `source`

### 7.编写环境脚本时

`make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean `这里的arm和=不要有空格，否则会有问题。

### 8.设备号

分为**主设备号**和**次设备号**

| 项目              | 作用                               |
| ----------------- | ---------------------------------- |
| 主设备号（major） | 决定由哪个驱动处理改设备文件的操作 |
| 从设备号（minor） | 驱动内部用来区分不同的设备实例     |

一个驱动往往需要管理多个设备。

```c
//设备号dev_t
typedef unsigned int __u32;
typedef __u32 __kernel_dev_t;
typedef kernel_dev_t dev_t;
```

dev_t 中 高12位为主设备号，低20位为次设备号。 

`MINORMASK = (1 << 20) - 1`为0xFFFFF；

### 9.虚拟地址与物理地址的映射

#### 9.1 ioremap 函数

```c
#define ioremap(cookie,size) __arm_ioremap((cookie), (size),
										MT_DEVICE)

void __iomem * __arm_ioremap(  phys_addr, size_t size,
							unsigned int mtype)
{
return arch_ioremap_caller(phys_addr, size, mtype,
						__builtin_return_address(0));
}
```

#### 9.2 iounmap函数

```c
void iounmap(volatile void __iomem *addr)
```

### 10.字符设备开发流程

 

### 11.临时设置IPV4的地址

`ifconfig eth0 192.168.6.12 netmask 255.255.255.0 up`

### 12.NFS服务

**NFS（Network File System）**
 中文叫 **网络文件系统**。
 它允许一台电脑把自己的某个文件夹 “共享” 给另一台电脑使用。

🟨 客户端和服务器之间的关系（核心）

| 角色              | 功能                     | 举例                                       |
| ----------------- | ------------------------ | ------------------------------------------ |
| **服务器 Server** | 提供、保存、管理共享目录 | “我这里有个文件夹，你们可以来用。”         |
| **客户端 Client** | 挂载并访问服务器目录     | “把你的文件夹挂到我这来，我像本地一样用。” |

`mount -t nfs 192.168.6.100:/home/share /mnt/nfs`

其中/mnt/nfs是挂载点，即把远程的目录挂载到本地的哪个目录上？

### 13.Linux下各个文件系统（磁盘分区）的使用情况

```shell
Filesystem      Size  Used Avail Use% Mounted on

/dev/root       7.0G  584M  6.0G   9% /
devtmpfs        184M  4.0K  184M   1% /dev
tmpfs            40K     0   40K   0% /mnt/.psplash
tmpfs           248M  168K  248M   1% /run
tmpfs           248M  132K  248M   1% /var/volatile
/dev/mmcblk1p1  127M  6.8M  120M   6% /run/media/mmcblk1p1
```

| 文件系统              | 类型     | 作用               | 存储介质  |
| --------------------- | -------- | ------------------ | --------- |
| `/dev/root`           | 实体分区 | 根文件系统         | SD卡/eMMC |
| `devtmpfs`            | 内存     | 设备节点           | RAM       |
| `tmpfs /mnt/.psplash` | 内存     | 启动画临时目录     | RAM       |
| `tmpfs /run`          | 内存     | 运行时文件         | RAM       |
| `tmpfs /var/volatile` | 内存     | 日志/缓存（可写）  | RAM       |
| `/dev/mmcblk1p1`      | SD/eMMC  | 启动分区、文件存储 | SD卡/eMMC |

### 14./etc/fstab文件的作用

`/etc/fstab` 是 Linux 系统的文件系统挂载配置表，系统启动时会读取它来自动挂载文件系统。

### 15./etc/network/interfaces

开机自启运行`sudo ifdown eth0 && sudo ifup eth0`

### 16.Linux防火墙

查看防火墙状态：`sudo ufw status`

允许SSH防火墙：`sudo ufw ssh`

### 17.开机自启程序

1.找到服务的文件夹

```shell
sudo nano /etc/systemd/system/aout.service
```

2.修改此文件的内容

```shell
[Unit]
Description=A.out Startup Service
After=network.target

[Service]
Type=simple
ExecStart=/home/shi1iu/test/a.out
WorkingDirectory=/home/shi1iu/test
Restart=always
User=shi1iu
Environment=PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

[Install]
WantedBy=multi-user.target
```

然后

```shell
sudo systemctl daemon-reload
sudo systemctl enable aout.service
sudo systemctl start aout.service
```

检查状态

```shell
sudo systemctl status aout.service
```

注意，添加可执行文件的权限

```shell
chmod +x /home/shi1iu/test/a.out
```

### 18.开机自启程序2.0

1.**编辑rc.local文件**：在终端中输入以下命令打开rc.local文件（如果存在）:

```bash
sudo vi /etc/rc.local
```

2.**添加启动命令**：在文件的最后一行添加要执行程序的全路径，例如：

```bash
/usr/local/bin/your_script.sh
```

3.**保存并退出**：确保文件可执行，使用以下命令设置权限：

```bash
sudo chmod +x /etc/rc.local
```

### 19.开机自启程序3.0

将脚本添加到/etc/init.d目录

1. **创建脚本**：编写一个启动脚本并将其放入`/etc/init.d/`目录中，例如`your_script`。
2. **设置权限**：确保脚本具有可执行权限：

```bash
sudo chmod +x /etc/init.d/your_script
```

3. **使用update-rc.d命令**：将脚本添加到启动序列中：

```bash
sudo update-rc.d your_script defaults
```

这种方法适用于较旧的Linux发行版。
