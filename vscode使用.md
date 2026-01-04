# vscode使用

### 1.如何选择多行一起输入？

> alt选择几个行再输入

### 2.远程连接ssh

首先测试终端cmd是否可以使用，`ssh user@ipaddr`，可以使用后，用vscode远程连接，如果没法连接，出现错误分析，连接不上，可能是连接错误，如果提示vscode服务错误，则可能是glibc的版本太低，升级乌班图即可。

一些命令：

```shell
$:ps -e | grep sshd 
$:sudo apt-get install openssh-server
$:sudo apt-get install net-tools
$:sudo systemctl enable --now ssh
$:sudo systemctl disable --now ssh
```



### 3.vs里面的终端乱码，如何解决？

> 在代码中main函数设置这样一句话就可以了。

```C
    // 设置控制台输出编码为 UTF-8
    SetConsoleOutputCP(CP_UTF8);
```

### 4.code模板

```c
/*********************************************************************
* @brief  : 简述
* @param  : char *a  ,   简述
* @retval : 返回值
*********************************************************************/
```

### 5.查看vs里面文件的类型

```shell
dumpbin /headers your_program.exe | findstr machine
```

### 6. makefile+gdb

1. 配置launch.json文件

2. 在cmakelists.txt中，必须添加

   `````makefile
   #debug版本才能调试
   set(CMAKE_BUILD_TYPE DEBUG)
   `````

   **添加最小配置文件：**

   ```json
   {
       // 使用 IntelliSense 了解相关属性。 
       // 悬停以查看现有属性的描述。
       // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
       "version": "0.2.0",
       "configurations": [
           {
               "name": "(gdb) Launch Program",
               "type": "cppdbg",
               "request": "launch",
               "cwd": "${workspaceFolder}",
               "program": "${workspaceFolder}/build7/app.exe"  // 你的可执行文件路径
           }
       ]
   }

简单搭配：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch Program",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/build/WinSockServer.exe",  // 你的可执行文件路径
            "args": [],
            "stopAtEntry": true,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "C:/msys64/mingw64/bin/gdb.exe",  // GDB 路径
            "setupCommands": [
                {
                    "description": "启用 GDB 的漂亮打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "make",
            "miDebuggerArgs": "-ex \"directory ${workspaceFolder}\""  // 指定源代码目录
        }
    ]
}
```

