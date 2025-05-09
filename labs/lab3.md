# 实验三

## 实验目的

- 练习处理二维数组和字符数组
- 练习设计简单算法解决实际问题

## 基础编程

### P3.1 对称方阵判断

对称方阵是一类特殊的矩阵，在线性代数中具有广泛的应用。它的定义是，如果一个$N\times N$的矩阵$A$，有$A_{ij}=A_{ji}, \forall i,j\in[1,N]$，则矩阵$A$是对称方阵。例如，矩阵

$$
\left[\begin{matrix}
1 & 2 & 3 \newline
2 & 4 & 5 \newline
3 & 5 & 6
\end{matrix}\right]
$$

就是一个对称方阵。

现在，输入一个$N\times N$的矩阵，请你编写程序判断它是否是对称方阵。

#### 输入

输入有$(N+1)$行。

```
N
A11 A12 ... A1N
A21 A22 ... A2N
......
AN1 AN2 ... ANN
```

其中第一行只有一个整数$N$，为矩阵的行（列）数。接下来N行，每行有N个整数，每两个整数之间以一个空格隔开，为矩阵的每个元素的值。比如输入

```
3
1 2 3
4 5 6
7 8 9
```

代表矩阵

$$
\left[\begin{matrix}
1 & 2 & 3 \newline
4 & 5 & 6 \newline
7 & 8 & 9
\end{matrix}\right]
$$

输入数据的范围是，$2 \leq N\leq 20$，$-2147483647\leq A_{ij}\leq2147483647,\forall i,j\in[1,N]$

#### 输出

输出只有一行。如果输入的矩阵是对称方阵，输出

```
YES
```

否则，输出

```
NO
```

### P3.2 字符串拼接

给定两个字符串，请将它们首尾相接地拼接在一起，构成一个新的字符串。

#### 输入

输入只有一行：

```
str1 str2
```

其中，str1和str2是两个**不含空白字符的**字符串，每个字符串的长度不超过29个字符（不计末尾空字符）。str1和str2之间用1个空格隔开。

#### 输出

输出只有一行

```
str
```

其中str是str1和str2拼接构成的字符串，str1在前，str2在后。

#### 示例

##### 输入

```
hello world
```

##### 输出

```
helloworld
```

#### 特殊要求

- 不允许使用`<string.h>`库函数

- 请使用下面的代码模板。你只可以修改`/* BEGIN YOUR CODE */`和`/* END YOUR CODE */`之间的部分。在提交在线评测系统时，**仅提交`/* BEGIN YOUR CODE */`和`/* END YOUR CODE */`之间的代码**。
  
  ```c
  #include <stdio.h>
  
  int main() {
      char str1[30], str2[30], str[60];
      scanf("%s %s", str1, str2);
      /* BEGIN YOUR CODE */
  
      /* END YOUR CODE */
      printf("%s", str);
      return 0;
  }
  ```

## 进阶编程

### AP3.1 股票投资问题

给定一段时间内，一只股票的股价走势，请找出何时买入、何时卖出，所获得的收益最大。注意以下几点：

- 虽然实际股票的价格每时每刻都在变动，我们在这里假设它在一天内都是一个固定值
- 不需要考虑分红、印花税等乱七八糟的东西，所获得的收益就是卖出时的股价减去买入时的股价
- 只做多，不做空。即卖出必须在买入之后。
- 只进行一次买卖

#### 输入

输入共2行：

```
N
p1 p2 p3 ... pN
```

第一行仅有一个整数N，表示下面给出的股价个数。

第二行有N个浮点数，中间以一个空格隔开，每个浮点数仅有2位小数，表示一段时间内每个交易日的股价。

数据范围：$2\leq N \leq 50$，$0<p_i\leq 100.00, \forall i \in [1,N]$

#### 输出

输出仅有一行：

```
buyday sellday gain
```

`buyday`为收益最高的买入股票的交易日（从0开始计），`sellday`为收益最高的卖出股票的交易日（从0开始计），`gain`为最高收益（保留两位小数）。如果无论怎么买入和卖出都无法获得正收益，输出

```
0 0 0.00
```

#### 示例

- 示例1
  
  - 输入
    
    ```
    5
    1.10 0.99 1.05 1.09 1.08
    ```
  
  - 输出
    
    ```
    1 3 0.10
    ```

- 示例2
  
  - 输入
    
    ```
    4
    2.08 2.06 2.01 1.96
    ```
  
  - 输出
    
    ```
    0 0 0.00
    ```

## 持续设计

### CDP3.1 贪吃蛇

本次持续设计题的难度会比之前的大一些了。我们这一次的任务是，将一个用二维数组表示的地图显示出来（用上一次的代码），然后从键盘输入一个坐标，判断这个坐标所在的位置。如果坐标所在的地方是空的，就在这个位置输出一个字符`o`，反之，如果坐标所在的地方是墙壁，则在最下面输出`GAME OVER`。

#### 定位光标

首先，我们来了解一下如何在任意位置输出字符。为了能在任意位置输出字符，需要额外包含`windows.h`头文件。（只对windows系统有效，因此使用其它操作系统的同学请在实验室的电脑上完成这一实验）

```c
#include <windows.h>
```

接下来我们需要一个结构体变量，用来存储坐标信息。这个结构体类型叫做`COORD`，是定义在`windows.h`头文件中的。它有两个成员`X`和`Y`，分别存储横、纵坐标。这里，我们给这个结构体变量起名叫`pos`，也就是position，位置的意思。当然你也可以起其它的名字，这个不重要。

```c
COORD pos;
pos.X = 2;
pos.Y = 3;
// 作为结构体变量，你也可以直接初始化它，写作：
// COORD pos = {2,3}
```

为了将这个坐标显示在控制台（就是我们运行程序的时候弹出的黑色窗口）中，首先需要获取操作系统对标准输出进行操作的句柄。你可以理解为，我们需要通过这个“句柄”（英文是handler比较容易理解，句柄这个翻译可以入选史上最差的10个专有名词翻译之一）来操作控制台。不理解也没关系，总之复制粘贴下面的这一行：

```c
HANDLE output = GetStdHandle(STD_OUTPUT_HANDLE); // output是句柄的变量名
```

然后通过`windows.h`中提供的函数来设置光标的位置：

```c
SetConsoleCursorPosition(output, pos);
```

这里的`output`和上面的句柄变量名一致，而`pos`和存储坐标信息的结构体变量名一致。

如果要将光标移动到新的位置，只需要修改`pos`的内容，然后再次调用上面的`SetConsoleCursorPosition`函数：

```c
pos.X = 5;
pos.Y = 1; // 新的位置。由于不是初始化，不能再用COORD pos = {2,3}的写法
SetConsoleCursorPosition(output, pos);
```

请尝试运行下面的代码，仔细体会上面调整光标位置的方法是如何工作的。**最上面一行的Y坐标是多少？最左边一列的X坐标呢？**

```c
#include <stdio.h>
#include <windows.h>

int main() {
    COORD pos;

    pos.X = 2;
    pos.Y = 3;
    HANDLE output = GetStdHandle(STD_OUTPUT_HANDLE); // 这一行只需要一次
    SetConsoleCursorPosition(output, pos);
    putchar('#');

    pos.X = 5;
    pos.Y = 1;
    SetConsoleCursorPosition(output, pos);
    putchar('*');

    // 下面几行的作用是将光标移到最下面
    // 否则在程序结束时输出的"请按任意键继续"会把前面我们输出的东西给覆盖掉。
    pos.X = 0;
    pos.Y = 4;
    SetConsoleCursorPosition(output, pos);
}
```

**注意：如果你使用的是Windows 11系统，上述定位光标的方法在Windows 11默认的Windows终端应用下可能不起作用。请按下组合键Win+r，输入wt并回车，打开Windows终端程序，然后按下组合键Ctrl+逗号打开设置，在启动-默认终端应用程序中选中Windows控制台主机，保存设置并关闭终端。**

#### 显示地图

这次显示地图使用的是上一次持续设计题CDP2.1的代码。所不同的是，这次的二维数组是$50\times 50$的数组，也就是说，是一个真正的贪吃蛇地图了。不过，原理都是一样的。二维数组的定义可以从下面的网址拷贝：

[http://home.ustc.edu.cn/~violinwang/C/exp/map.c](http://home.ustc.edu.cn/~violinwang/C/exp/map.c)

这个地图如果直接输出出来，由于字符的长宽并不相等，视觉效果可能并不好。在实验课上，助教会讲解如何修改字体，让地图的显示更加美观。

总之，本次持续设计题你需要：

1. 在控制台的前50行，输出上面提到的二维数组定义的地图。
2. 将光标移动到第51行，然后等待用户输入两个0~49之间的整数，构成一个坐标。
3. 判断地图上这个坐标是否有墙壁。如果没有墙壁，将光标移动到这个坐标并输出一个`o`，否则将光标移动到第51行的开头，输出`GAME OVER`。

### CDP3.2 极简教务系统

本次持续设计需要你在上一次的基础上实现多用户登录，并根据登录用户的身份不同显示不同的菜单页面。

用户的基本信息使用结构体来存储，可参考教材P143的内容。

```c
struct Info {
    int ID;
    char name[NSTRLEN];
    char pwd[PSTRLEN];
    char type;
};
```

其中的ID为用户名，具有唯一性，采用整数来表示。用户的姓名和密码采用字符数组存储，其最大长度由常量`NSTRLEN`和`PSTRLEN`指定。这两个常量可以使用`#define`在程序开头处定义，方便以后修改。`type`标识用户的类型，可以是教学秘书、教师或学生。简单起见，我们本次实验只设置一个教学秘书、一个教师、一个学生，他们的ID、姓名、密码等可以直接编写在程序中。

程序运行后，首先显示登录页面，提示用户输入ID和密码。如果ID和密码匹配，则显示菜单页面。对于教学秘书，显示如下的菜单页面：

```
========极简教务系统========
欢迎您，xxx教学秘书

    1. 新增用户
    2. 删除用户
    3. 新增课程
    4. 教师排课
    0. 登出当前用户

==========================
请选择：
```

对于教师用户，显示：

```
========极简教务系统========
欢迎您，xxx老师

    1. 录入成绩
    2. 修改成绩
    3. 成绩排序
    0. 登出当前用户

==========================
请选择：
```

对于学生用户，显示：

```
========极简教务系统========
欢迎您，xxx同学

    1. 学生选课
    2. 查询成绩
    0. 登出当前用户

==========================
请选择：
```

其中的`xxx`使用用户的姓名代替。你也可以自行设计更好看的菜单排版。和上次一样，除了登出当前用户之外，其他的功能都在显示“当前功能未开放”后退回到菜单页面。

### CDP3.3 Alpha Tic-Tac-Toe

让我们来完善井字棋程序，让它能够实现人与人之间对弈。

首先，为了更好地管理数据，我们应该定义一个结构体类型来代表当前盘面的状态，包括棋盘棋子本身——我们之前已经采用一个二维数组来表示它们——以及当前执子的玩家——我们可以用一个`int`类型变量来表示它。

```c
struct State {      // 表示盘面状态
    int player;     // 与棋盘一样，1代表圆方执子，-1代表叉方执子
    int grid[3][3]; // 棋盘
};
```

你可以像这样定义状态变量并初始化，假设叉方先行：

```c
struct State state = {-1, {0}};
```

然后，你需要编写代码，判断一个棋盘是否已是胜局，如果是的话胜方是谁。井字棋的胜利规则很简单，当有一方的三枚棋子连成一条直线时就获胜了，棋盘上总共有三横三纵两对角共8条直线，你可以想一想怎样判断。

最后，将所有代码放入一个循环里，并以胜局的出现作为循环退出条件。在每次循环中，首先打印当前执子方，然后读取两个数作为落子的坐标，更新棋盘并打印，判断是否出现胜局，然后翻转执子方。为了方便你编写程序，提供如下的伪代码供参考：

```
初始化state变量；
初始化winner变量为0；
While(winner==0)：
    打印执子方；
    读取落子坐标；
    更新棋盘并打印；
    判断是否为胜局，如果是，存入winner；
    翻转执子方state.player = -1 * state.player；
End While；
打印获胜者；
```