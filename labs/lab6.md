# 实验六

## 实验目的

- 练习向函数中传递复杂参数的方法
- 练习自顶向下模块化程序设计

## 基础编程

### P6.1 排序函数

编写一个函数`sort()`，对int类型的数组进行排序。函数应该接受数组名和数组有效长度作为参数，直接在原数组上进行排序，返回值类型为void。

请使用下面的模板：

```c
#include <stdio.h>

/* BEGIN YOUR CODE */

/* END YOUR CODE */

int main() {
    int array[100];
    int N,i;
    scanf("%d", &N);
    for(i=0;i<N;i++) {
        scanf("%d", &array[i]);
    }
    sort(array, N); // sort函数需要你自己编写
    for(i=0;i<N;i++) {
        printf("%d ", array[i]);
    }
    return 0;
}
```

#### 输入

输入有两行：

```
N
a1 a2 ... aN
```

第一行是一个整数N，$2\leq N \leq 100$。

第二行有N个整数，是需要排序的数组的内容。

#### 输出

输出只有一行：

```
b1 b2 ... bN
```

其中b~i~是数组中第i小的数（即升序排序）

#### 示例

输入

```
5
1 4 2 5 3
```

输出

```
1 2 3 4 5
```



### P6.2 实现strlen

在`<string.h>`库中有一个计算字符串长度的函数`strlen()`。它接受一个字符串作为参数，计算它的长度并返回。举个例子：

```c
#include <stdio.h>
#include <string.h>

int main() {
    char str[20]="Hello";
    int l = strlen(str);
    printf("%d", l); // 输出5
    return 0;
}
```

请你写一个自己的函数`my_strlen()`，实现与`strlen()`相同的功能。请使用下面的模板：

```c
#include <stdio.h>

/* BEGIN YOUR CODE */

/* END YOUR CODE */

int main() {
    char str[200];
    gets(str); // gets读取输入字符串直到遇到换行符，丢弃最后的换行符并将内容存放进数组
    printf("%d", my_strlen(str));
    return 0;
}
```

#### 输入

输入只有一行：

```
str
```

str是一个长度小于199个字符的字符串，以换行符结束（换行符不属于str的一部分）

#### 输出

输出只有一行：

```
len
```

len是str的长度（不包括换行符）。

#### 示例

输入

```
hello
```

输出

```
5
```



## 进阶编程

### AP6.1 天数计算plus

本题是AP5.2的进阶版本。请在AP5.2的基础上，运用自顶向下的程序设计思想，完成本题。

编写一个程序，输入两个日期，计算这两个日期之间相差多少天。

#### 输入

输入有两行：

```
Y1 M1 D1
Y2 M2 D2
```

其中，$1900\le Y_1,Y_2 \le2100$是两个表示年份的整数，$1\le M_1,M_2 \le12$是两个表示月份的整数，$1\le D_1,D_2 \le 31$是两个表示日期的整数。输入数据保证Y~1~-M~1~-D~1~和Y~2~-M~2~-D~2~这两个日期存在。

#### 输出

输出只有一行：

```
X
```

$X$为Y~2~-M~2~-D~2~与Y~1~-M~1~-D~1~之间相差的天数。若Y~2~-M~2~-D~2~更晚，则$X$为正值；反之为负值。若二者是同一天，$X$为0.

#### 示例

- 示例1

  - 输入

    ```
    2022 11 8
    2022 11 9
    ```

  - 输出

    ```
    1
    ```

- 示例2

  - 输入

    ```
    2022 11 9
    2022 11 8
    ```

  - 输出

    ```
    -1
    ```



## 持续设计

### CDP6.1 贪吃蛇

本次实验需要实现一个定长的贪吃蛇程序，即游戏规则与正常的贪吃蛇无异，但蛇的长度不会增加。你需要在CDP6.1的基础上完场下列工作：

1. 你需要能够在地图上随机生成食物。在游戏开始时你需要在地图上的随机位置生成一个食物（可以用@符号来表示，看起来很像某种食物对吧），并**确保食物没有生成在墙和蛇身上**。在随后的游戏中，每当蛇运动完毕，都需要检查食物是否被蛇吃掉了，如果吃掉了则需要生成一个新的食物。

2. 你需要将蛇的长度从1改为M。在CDP5.1中我们仅存储了蛇头的坐标，而本次实验你需要用一个长度为M的数组来存储从蛇头到蛇尾的每个坐标，并且每次蛇移动时，你需要：

   1. 读取蛇尾的坐标，并在蛇尾坐标处打印空格擦掉蛇尾
   2. 依次将数组的每个元素移动到下一个位置，蛇尾的坐标丢弃
   3. 根据前进方向，在数组的0号元素写入新的蛇头坐标，并在蛇头坐标处打印`o`绘制新的蛇头

   请将蛇的长度用`#define`进行定义，你可以定义M为5或你喜欢的值，这样方便我们日后将其修改为可变长度的版本。

   注意当蛇的长度增加后，你需要判断的不仅仅是蛇有没有撞墙，还需要判断**有没有咬到自己**。

   你可以修改之前的`struct Snake`的定义：

   ```c
   struct Snake {
       enum Direction dir; // 蛇的前进方向
       COORD pos[M]; // 蛇的每一节的坐标
   };
   ```

   **请注意：你需要充分利用模块化编程思想，将对蛇的各类操作封装成不同的模块。否则在后续的题目中，涉及到将蛇的存储方式由数组改为链表时，会很难改。**

3. 你需要进行计分，蛇每吃掉一个食物计1分，你可以将分数打印在地图下方，或者你喜欢的地图外面的任何地方。



### CDP6.2 极简教务系统

本次实验需要你完善用户系统，即编写教学秘书的新增用户、删除用户这两个模块。我们在CDP4.2中已经设计了一个数组来存储所有用户的信息，并且设置了一个全局变量来记录数组中实际记录的用户数量。在此基础上：

- 新增用户的流程为：
  - 检查用户容量是否超过上限，如果超过则新增用户失败，退回主菜单
  - 读取新用户的id，并确保与已存在的用户没有重复
  - 读取新用户的用户名、密码和用户类型，并写入到AllUser数组中新的单元中
  - 将全局变量UsrNum加1
- 删除用户的流程为：
  - 读取要删除用户的id
  - 检查是否存在该用户，如果不存在要求重新输入；如果存在则打印该用户的信息以供确认
  - 如果确认信息无误，则删除该用户。你需要将AllUser数组中后面的所有元素往前挪一个位置，并将UsrNum减1。如果信息有误，允许重新输入id。



### CDP6.3 Alpha Tic-Tac-Toe

本次实验让我们来实现一个具有一定智能的AI来下棋。我们将使用一种被称为蒙特卡洛树搜索（Monte Carlo Tree Search, MCTS）的方法，利用我们在CDP5.3中编写的随机走子函数来实现它。

让我们来思考一下如何让下棋的AI具有智能。很容易想到的是，AI总是选择那些能够让自己的胜率更高的下法，从而让自己获胜的可能最大化。也就是说，对于任意的状态$s$和在此状态下可以选择的动作$\{a_0, a_1,\dots,a_N\}$，最优的动作就是：
$$
a^*=\arg\max_{a}q(s,a)
$$
这里，$a^*$代表最优的动作，上标$*$经常被用于表达“最优”的含义。函数$q(s,a)$表示在状态$s$下、采取动作$a$后，新的局面下AI的胜率——更广义地说，这是对状态$s$下采取动作$a$的“价值”的衡量，它有一个专有名词叫做“动作价值函数”。运算符$\arg\max$你可能是第一次见到，它表示在它后面的函数取得最大值时，自变量的取值。例如，我们知道$\max_{x}\{1-x^2\}=1$，而$\arg\max_{x}\{1-x^2\}=0$，即$1-x^2$取到最大值1时$x$的值。

好的！那么我们只需要让我们的AI按照这个规则选择最优的动作就好了，除了——我们并不知道函数$q(s,a)$的值。对于井字棋这种简单的游戏，或许你可以把这个函数手算出来，但是对于更复杂的游戏来说可能就不是那么容易的了（比如围棋？不知道你是否还记得Alpha Go）。那么我们该怎么办呢？

在人工智能领域，有一个几乎万能的工具——蒙特卡洛采样。如果有一个东西，你很难计算出它的准确值，但是你可以通过随机抽样的方式来估计它，那么就这样做！这就是蒙特卡洛采样。具体到井字棋，我们只需要让双方都随机走子直到终盘，反复进行这个过程直到对胜率有一个比较准确的估计。对每个可选的动作都进行这样的模拟，然后选择胜率最高的动作即可。不过对于井字棋，由于平局是非常常见的事情（并且先手存在一个不会输的策略，只会胜利或平局），所以这里选择负率最低的动作或许比胜率最高的动作更合理——你可以自己尝试一下再决定。

下面的伪代码或许能够帮到你：

```text
蒙特卡洛树搜索:
	输入：状态s
	获取当前状态下的合法动作集合{a[1], a[2], ..., a[N]}
	For 每个动作a[i]:
		获取执行动作a[i]后的新状态s'
		从s'开始，双方随机走子直到终盘
		重复随机走子M次，统计非负的局数，得到q(s, a[i])的估计值
	End For
	选择q(s, a[i])最高的动作a*
	输出: a*
```

其实，蒙特卡洛树搜索对胜率的估计并不是这么简单的，实际上为了获得更准确的估计，人们常常采用一种被称为UCB（上限置信区间）的统计方法。如果有兴趣的话，可以自行了解相关知识，尝试实现基于UCB统计方法的蒙特卡洛树搜索（可选，不计分）。
