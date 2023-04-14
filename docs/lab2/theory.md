# 实验原理

## 1. 子程序

&emsp;&emsp;子程序是一段具有特定功能的、能被重复调用的代码。在程序设计中使用子程序，有利于增加代码可读性和可维护性。

&emsp;&emsp;RISC-V的子程序并不像X86那样需要使用专门的关键字来定义。从形式上看，RISC-V汇编的子程序可以看作是一段带有标签的、可重复执行的代码。

&emsp;&emsp;下列代码是一个在主程序中使用子程序的例子。

```asm
 1|.data
 2|    ......
 3|
 4|.macro push %a
 5|	   addi	sp, sp, -4
 6|	   sw   %a, 0(sp) 
 7|.end_macro
 8|
 9|.macro pop %a
10|	   lw   %a, 0(sp) 
11|	   addi	sp, sp, 4
12|.end_macro
13|
14|.text
15|MAIN:
16|	   ......
17|	   jal  ra, SOME_FUNC       # Call sub-routine.
18|	   ......
19|	   ori  a7, zero, 10        # Set system call number(10 for termination).
20|    ecall                    # This program terminates here.
21|
22|SOME_FUNC:
23|	   push	t0
24|    push t1
25|	   ......                   # Assume that t0 and t1 are modified here.
26|    pop  t1
27|	   pop 	t0
28|	   jalr zero, 0(ra)         # Sub-routine returns.
```

- 第2-5行：`push`宏定义将一个寄存器压入栈中。

- 第7-10行：`pop`宏定义将栈顶的数据弹出到指定寄存器。

- 第15行：主程序通过`jal  ra, FUNC`指令进入子程序，并将返回地址保存至`ra`寄存器。

- 第17行：停机指令。

- 第21-22行：**保护现场** —— 除去用于传递返回值的寄存器之外，子程序在修改任意寄存器之前，都需要先将其压入栈中，防止子程序返回后造成主程序执行出错。

- 第24-25行：**恢复现场** —— 子程序返回之前，从栈中恢复那些被子程序修改过的寄存器。

- 第26行：子程序通过`jalr zero, 0(ra)`指令返回到主程序第16行继续执行。



## 2. 输入与打印

&emsp;&emsp;RARS通过系统调用语句提供了输入输出功能。用户需要按照约定，设置参数寄存器，然后执行`ecall`指令即可。

&emsp;&emsp;在RARS中，`a7`寄存器用于传递系统调用的编号，具体参数则通过`a0`、`a1`等寄存器设置。详见表2-1。

<center>表2-1 RARS常用系统调用</center>
<center>

| 用途 | `a7`的值 | 参数设置 | 返回值 |
| :-: | :-: | :- | :- |
| 打印整数 | 1 | `a0`存放待打印数据 | 无 |
| 打印字符串 | 4 | `a0`存放待打印字符串（字符串需以空字符结尾） | 无 |
| 输入整数 | 5 | 无 | 读取的整数将存放在`a0` |
| 输入字符串 | 8 | `a0`存放字符串缓存的地址，`a1`存放最大允许输入的字符数 | 无 |
| 退出程序 | 10 | 无 | 无 |
| 退出程序 | 93 | `a0`存放退出参数 | 无 |

</center>

!!! example "举个栗子 :chestnut:"
    &emsp;&emsp;若想打印寄存器`t0`，则可通过以下语句实现：

    ```asm
     1|    ori   a0, t0, 0
     2|    ori   a7, zero, 1
     3|    ecall
    ```

!!! tip "小提示 :bulb:"
    &emsp;&emsp;若有多个信息需要打印，可通过自定义宏来实现：

    ```asm
     1|.macro print %reg, %mode
     2|    ori   a0, %reg, 0
     2|    ori   a7, zero, %mode
     3|    ecall
     4|.end_macro
     5|
     6|.text
     7|    ......
     8|    print t0, 1      # 打印t0寄存器的值
     9|    print t1, 4      # 打印字符串（字符串地址存储在t1寄存器）
    10|    ......
    ```
