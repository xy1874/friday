## 实验目的

&emsp;&emsp;1. 熟悉RISC-V汇编程序的语法，掌握基本汇编程序的编写；

&emsp;&emsp;2. 理解子程序工作原理，掌握子程序的设计方法；

&emsp;&emsp;3. 熟悉汇编程序的开发过程及RARS的使用方法。


## 实验内容

&emsp;&emsp;数据段中存储着一个长度为10的字符串，请编写RISC-V汇编程序，找出其中的回文子串。

&emsp;&emsp;要求：

&emsp;&emsp;（1）若找到，输出回文子串的位置和长度；否则输出-1；

&emsp;&emsp;（2）若存在多个回文子串，只需找出其中任意一个即可；

&emsp;&emsp;（3）必须<u>**正确使用子程序**</u>，且<u>**不可使用伪指令**</u>；

!!! tip "小提示 :bulb:"
    &emsp;&emsp;可以使用RARS帮助你判断一条指令是否伪指令 —— 对程序进行汇编后，查看`Text Segment`窗口下的`Basic`指令和`Source`指令是否相同，如果不相同，则是伪指令，如图1-1所示。

    <center><img src="../assets/1-1.png" width = 600></center>
    <center>图1-1 借助RARS查看指令是否是伪指令</center>

    &emsp;&emsp;由图1-1可知，`li t0, 0x00002000`和`jal FUNC`都是伪指令。
