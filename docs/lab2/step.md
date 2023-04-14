# 实验步骤

## 1. 存储配置及访问

### 1.1 配置存储模式

&emsp;&emsp;本实验首先需要配置RARS的存储模式，配置方法如图3-1所示。

<center><img src="../assets/3-1.png" width = 400></center>
<center>图3-1 从RARS菜单打开存储配置</center>

&emsp;&emsp;在存储配置对话框左侧，选择`Compact`，然后应用更改，如图3-2所示。

<center><img src="../assets/3-2.png" width = 400></center>
<center>图3-2 将存储配置更改为`Compact`模式</center>

&emsp;&emsp;在图3-2中，观察右侧的地址分配，可知数据段基地址为`0x00002000`

### 1.2 数据段访问

&emsp;&emsp;访问数据段的参考代码如下：

```asm
 1|.data
 2|    number: .word 0xa
 3|
 4|.text
 5|MAIN:
 6|    lui t0, 0x00002      # 将数据段基址赋值给t0
 7|    lw  a2, 0x0(t0)      # 将number变量的值加载到a2
 8|......
```

&emsp;&emsp;在RARS中运行上述代码，可查看到数据段中的数据，如图3-3所示。

<center><img src="../assets/3-3.png" width = 600></center>
<center>图3-3 在RARS中查看数据段</center>

&emsp;&emsp;执行程序后，可在RARS右侧查看`a2`寄存器的值，如图2-4所示。

<center><img src="../assets/3-4.png" width = 350></center>
<center>图3-4 查看程序执行结果</center>


## 2. 操作步骤

&emsp;&emsp;（1）在RARS中，新建`.asm`文件，按上文所述配置存储模式，并编写汇编程序；  

&emsp;&emsp;（2）点击菜单栏按钮，进行汇编，如图3-5所示；

<center><img src="../assets/3-5.png" width = 450></center>
<center>图3-5 点击按钮进行汇编</center>

&emsp;&emsp;（3）运行和调试程序：点击运行按钮直接运行程序；点击调试按钮可每次执行一条指令，如图3-6所示；

<center><img src="../assets/3-6.png" width = 460></center>
<center>图3-6 运行或调试程序</center>

!!! tip "调试技巧 :bulb:"
    &emsp;&emsp;为了提高调试效率，可在RARS中设置断点，如图3-7所示。

    <center><img src="../assets/3-7.png" width = 460></center>
    <center>图3-7 设置断点</center>

    &emsp;&emsp;首先在`Text Segment`窗口的`Bkpt`列下面，给相应的指令打断点。然后点击运行按钮，即可直接执行到断点处的指令。

&emsp;&emsp;（4）在运行结果、Data区和寄存器区观察结果。
