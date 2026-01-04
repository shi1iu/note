# FPGA

### 1.设计按键控制led，通过同步信号进行缓存。

```verilog
module key_led_control (
    input  wire clk,       // 时钟
    input  wire rst_n,     // 异步复位，低电平有效
    input  wire key,       // 按键输入（低电平有效）
    output reg  led        // LED 输出
);

    // 内部信号
    reg key_sync_0, key_sync_1;
    reg key_prev;
    wire key_negedge;

    // 同步按键信号到时钟域（防止亚稳态）
    always @(posedge clk or negedge rst_n) begin
        if (!rst_n) begin
            key_sync_0 <= 1'b1;
            key_sync_1 <= 1'b1;
        end else begin
            key_sync_0 <= key;
            key_sync_1 <= key_sync_0;
        end
    end

    // 检测下降沿（按键按下）
    always @(posedge clk or negedge rst_n) begin
        if (!rst_n)
            key_prev <= 1'b1;
        else
            key_prev <= key_sync_1;
    end

    assign key_negedge = (key_prev == 1'b1 && key_sync_1 == 1'b0);

    // 状态翻转逻辑（按下按键时翻转LED）
    always @(posedge clk or negedge rst_n) begin
        if (!rst_n)
            led <= 1'b0;
        else if (key_negedge)
            led <= ~led;
    end

endmodule

```

tb文件

```verilog
`timescale 1ns / 1ps

module tb_key_led_control;

    // 测试信号定义
    reg clk;        // 时钟信号
    reg rst_n;      // 复位信号，低电平有效
    reg key;        // 按键输入
    wire led;       // LED 输出
    wire key_sync_0; // 用于观察 key_sync_0 的值
    wire key_sync_1; // 用于观察 key_sync_1 的值

    // 实例化被测试模块
    key_led_control uut (
        .clk(clk),
        .rst_n(rst_n),
        .key(key),
        .led(led)
    );

    // 时钟生成：每 10 ns 一个周期
    always begin
        #5 clk = ~clk;  // 时钟周期 10 ns
    end

    // 仿真过程
    initial begin
        // 初始化信号
        clk = 0;
        rst_n = 0;       // 复位信号
        key = 1;         // 初始时按键没有按下

        // 复位
        #10 rst_n = 1;  // 释放复位
        #10 rst_n = 0;  // 再次加复位
        #10 rst_n = 1;  // 释放复位

        // 按键模拟：模拟按键按下与松开
        #20 key = 0;  // 按下按键
        #20 key = 1;  // 松开按键

        #20 key = 0;  // 再次按下按键
        #20 key = 1;  // 松开按键

        #50 key = 0;  // 再次按下按键
        #20 key = 1;  // 松开按键

        // 仿真结束
        #50 $finish;
    end

    // 监视 LED 状态和同步信号
    initial begin
        $monitor("At time %t, LED = %b, key_sync_0 = %b, key_sync_1 = %b", $time, led, uut.key_sync_0, uut.key_sync_1);
    end

endmodule

```

可以看到在key上升沿的时刻，led才会翻转。

### 2.将字节序反转

```verilog
AaaaaaaaBbbbbbbbCcccccccDddddddd => DdddddddCcccccccBbbbbbbbAaaaaaaa
```

使用拼接字去处理

```verilog
module top_module(
    input [31:0] in,  // 32位输入
    output [31:0] out // 32位输出
);

    // 通过拼接来反转字节顺序：
    assign out = {in[7:0], in[15:8], in[23:16], in[31:24]};

endmodule
```

### 3.always语句和assign语句

always里面不能有assign语句，本身两个概念就是并列的，所以不能同时使用。

4.{}拼接字符的错误使用

```verilog
module top_module (
    input [7:0] in,
    output [31:0] out );//

    // assign out = { replicate-sign-bit , the-input };

    assign out[31:0] = {{24{in[7]}},in[7:0]};//这里不能写成assign out[31:0] = {24{in[7]},in[7:0]};没法识别24。
endmodule
```

### 4.数组的声明

​    wire [7:0] temp1,temp2,temp3;这样的声明是对的，    

```verilog
wire temp1[7:0],temp2[7:0],temp3[7:0];
```

是不对的。

### 5.状态机



### 6.verilog 在VSCODE的插件

> Digital IDE

### 7.led左移

```verilog
reg [3:0]
led <= led<<1;//这种情况不能循环移
led <= {led[2:0],led[3]}; //将后三位数移动到前面，最高的一位放在最低位
```

### 8.如何将仿真时间加长？

在Tcl console下run x 单位   eg：`run 100 us`

### 9.在两个always语句同时驱动两个信号

> 可能会导致 **竞争条件（race conditions）** 和 **不确定的行为**

### 10.vscode中仿真Verilog

下载

1. 下载**Iverilog**

2. 安装插件 **Digital IDE**

3. **WareTrace**

4.  ``` verilog
    $dumpfile("tb_led.vcd");//你的文件名
        $dumpvars;
    ```

### 11.**Synthesis Options**

1. Global Synthesis（全局综合）
2. Out of Context per IP（按 IP 核独立综合）

### 12.自定义IP核
