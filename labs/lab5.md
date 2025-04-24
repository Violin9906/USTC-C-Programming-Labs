# 实验五

**请注意，本次实验所有题目必须编写自定义函数实现，不允许只有一个main函数。**

## 实验目的

- 练习简单的函数声明与使用
- 练习向函数中传递简单参数的方法
- 练习从函数中返回简单值的方法

## 基础编程

### P5.1 A+B Problem

输入两个整数$A$和$B$，请编写一个函数，返回它们的和，并编写一个完整的程序，利用你编写的函数计算多组输入数据的和。

#### 输入

输入有$(N+1)$行：

```
N
A1 B1
A2 B2
A3 B3
...
AN BN
```

第一行为一个整数$N$，$1\le N \le 100$。之后有$N$行，每行有两个整数$A_i$和$B_i$，中间用一个空格隔开，$-65535\le A_i, B_i \le 65535$。

#### 输出

输出有$N$行：

```
S1
S2
...
SN
```

其中每行有一个整数$S_i=A_i+B_i$。

#### 提示

请注意，本题必须自行编写函数计算两数的和。以下的函数原型可供参考：

```c
int add(int a, int b);
```



### P5.2 putcccchar

编写一个函数，将一个指定字符打印指定的行数和列数。并编写一个完整的程序，利用你编写的函数将输入的字符打印指定的行数和列数。

#### 输入

输入只有一行：

```
c A B
```

其中`c`是一个可打印的ASCII字符（字母、数字或标点符号），$A$和$B$是两个正整数，$0 < A, B \le 50$。

#### 输出

输出有$A$行，每行由$B$个字符`c`组成：

```
ccccccc...c
ccccccc...c
...
ccccccc...c
```

#### 示例

- 输入

  ```
  * 3 5
  ```

- 输出

  ```
  *****
  *****
  *****
  ```
  

#### 提示

请注意，本题必须自行编写函数输出指定内容，不得直接在main函数中输出。以下的函数原型可供参考：

```c
void putcccchar(char c, int a, int b);
```



## 进阶编程

### AP5.1 分数约分

编写一个程序，计算由两个正整数构成的分数进行约分后的最简分数，并输出。为了计算分数是否为最简，你需要将AP2.1所编写的辗转相除法程序**改编为函数**，用以计算分子分母的最大公因数。

#### 输入

输入只有一行：

```
A B
```

其中$1\le A\le 65535$，$2\le B \le 65535$是两个正整数，代表分数$\frac{A}{B}$。$A$和$B$的大小关系不定。

#### 输出

输出只有一行：

```
C D
```

其中$C$和$D$是两个正整数，由它们构成的分数$\frac{C}{D}$是$\frac{A}{B}$对应的最简分数。



### AP5.2 天数计算

编写一个函数，计算给定日期是对应年度的第几天。并编写完整程序，从输入读取日期，利用所编写的函数计算天数并输出。

#### 输入

输入只有一行：

```
Y M D
```

其中$1900\le Y \le 2100$是一个表示年份的整数，$1\le M \le 12$是一个表示月份的整数，$1\le D \le 31$是一个表示日期的整数。输入数据保证该日期存在（也就是不会有2022 2 31这样的输入）。

#### 输出

输出只有一行：

```
X
```

$X$为给定日期（Y-M-D）是年份Y当中的第几天。

#### 提示

注意闰年的处理：非整百年份能够被4整除、整百年份能够被400整除的是闰年。



## 持续设计

### CDP5.1 贪吃蛇

我们的贪吃蛇终于有点像样了！本次实验，你需要将CDP3.1和CDP4.1的实验内容用函数进行模块化封装，然后整合到一个程序中。你可以使用下面的代码模板：

```c
#include <stdio.h>
#include <windows.h>
// 其它你需要的头文件

// map的定义

// 方向的枚举类型定义
enum Direction {
	up,down,left,right
};

struct Snake {
    enum Direction dir; // 蛇的前进方向
    COORD pos; // 存储蛇头坐标，COORD类型在windows.h中定义
    // 之后的实验会在这里添加更多的信息
};

struct Snake snake; // 蛇

void setPos(int x, int y); // 设置光标位置的函数
void showMap(void); // 显示地图
void getDir(void); // 根据用户输入更新蛇的前进方向
int moveSnake(void); // 移动蛇，如果撞墙返回1，否则返回0

int main() {
    snake.dir = right;
    snake.pos.X = 25;
    snake.pos.Y = 25; // 初始化蛇的移动方向和位置
    showMap();
    while(1) {
        getDir();
        if(moveSnake() == 1) {
            // 把光标移动到合适位置，打印Game Over的提示
            return 0;
        } else {
            Sleep(500);
        }
    }
    return 0;
}

// 函数setPos, showMap, getDir, moveSnake等的实现代码

```

完成本次持续设计后，你应该能看到一个只有“蛇头”，不会生长的贪吃蛇版本了。请注意蛇撞墙的判断和绘制蛇的先后顺序，避免“穿模”的情况出现。



### CDP5.2 极简教务系统

本次实验请你对CDP4.2的程序进行模块化封装。目前，我们已经完成的功能有：

- 登录
- 根据用户类型显示菜单
-  登出

根据模块化的思想，可以将以上三个功能封装为下面的函数：

```c
/****************************************************
 * login    :用户登录模块
 *           功能：打印登录界面，读取用户名和密码并匹配已有用户
 *           参数：无
 *           返回值：返回已登录用户在AllUser数组中的下标
 ***************************************************/
int login();
```

```c
/****************************************************
 * showMenu :菜单显示模块
 *           功能：根据登录用户类型打印菜单
 *           参数：用户类型(char)，用户名(char[])
 *           返回值：无
 ***************************************************/
void showMenu(char utype, char name[]);
```

```c
/****************************************************
 * logout   :用户登出模块
 *           功能：登出当前用户
 *           参数：无
 *           返回值：无
 ***************************************************/
void logout();
```

请将程序补充完整。



### CDP5.3 Alpha Tic-Tac-Toe

本次实验我们将对已经完成的程序进行模块化封装。你可以使用下面的代码模板。如果你的代码不是按照这样的方式组织的，也可自行修改。

```c
#include <stdio.h>
#include <stdlib.h>
// 其他你需要的头文件

struct State {      // 表示盘面状态
    int player;     // 与棋盘一样，1代表圆方执子，-1代表叉方执子
    int grid[3][3]; // 棋盘
};

struct Action {		// 表示采取的动作
    int rank;		// 落子的行
    int col;		// 落子的列
};

struct State state = {-1, {0}}; // 初始状态：×方执子，棋盘为空

/* 自定义函数原型 */
char chessPieceChar(int chessPieceValue);	// 根据棋子的值返回'X','O'或' '，用于打印棋子
void showGrid(void); 						// 打印棋盘棋子
struct Action randomAction(void); 			// 检查当前棋盘状态，返回一个合法的随机动作
void step(struct Action action); 			// 执行动作，更新状态
int isValidAction(struct Action action);	// 检查动作是否合法
int isFull(void);							// 检查棋盘是否已满
int winner(void); 							// 检查是否有人胜出
struct Action humanAction(void);			// 从键盘读取输入，返回人类的动作
// 其他你需要定义的函数

int main() {
    do {
        struct Action action;
        switch(state.player) {
            case -1: 
                action = randomAction();
                break;
            case 1:
                action = humanAction();
                break;
            default:
                // 出错了
        }
        if(!isValidAction(action)) {
            continue; // 非法动作，重新获取
        }
        step(action);
    } while(!isFull() && winner()==0);
    // 打印终局信息：胜方/平局
    return 0;
}

/* 自定义函数实现 */
char chessPieceChar(int chessPieceValue) {
    // ...
}
// ......
```

