### 扫雷小程序（C语言）

​	大家还记得学校上机课无聊时玩过的扫雷吗？扫雷是我们这些90年代的青年们玩过最经典的小游戏了吧？大家有木有常对他的工作原理疑惑，那么今天我们来用我们学过的C语言实现一个简单的扫雷小游戏。

​	还记的扫雷的规则吗？

​	当你第一次点击时，永远不会踩到雷（小伙伴们有木有发现呢）

​	以后点击的时候，如果以点击位置为中心的3*3的格子里有雷，则显示3\*3格子里的雷数；

​	雷盘(实在想不出点击的n\*n的格子叫啥，就叫雷盘吧 /手动滑稽 )

​	若没有雷则会递归的帮你把周围的也全部展开，如果设置的雷比较少而雷盘比较大时，是不是差不多会给你把大部分的都展开呢？

​	那么，如何实现呢？

1、雷盘的设计(分析)

​	比如我们要做一个雷盘大小为10 \* 10的一个扫雷游戏，理论上我们需要一个10 \* 10 的数组，那么问题来了，那么边缘的雷数怎么算呢？所以我们需要一个比雷盘大小要大2的一个数组，比如10\*10的扫雷就需要一个12*12的数组；这样一来，本来数组列是从 0~11 但是我们只用 1~10，这样用户在输入的时候好输入，我们在计算边缘雷数的时候也非常好判断，一举两得。大致的结构是这样的。

![](E:\MyCode\MyStudyNote\C\mine\leipan.png)					    		 			  

​	但是就仅仅一个雷盘够吗？答案是当然不够的，总不可能把雷盘直接显示给用户，这里我们还需要一个显示数组，大小和雷盘数组一样大。

​	我们先定义出棋盘大小和雷的个数

```c
#define ROW 12
#define COL 12
#define MINE_NUM 20
```

##### 2、主函数的实现

​	直接让主函数调用实现扫雷的函数 Mine（）；

```c
#include "mine.h"	

void menu(){
	printf("******************************\n");
	printf("*****      1、扫雷       *****\n");
	printf("*****      2、退出       *****\n");
	printf("******************************\n");
	printf("请输入选项<1/2>:");
}

int main(){
	int select = 0;
	menu();
	scanf("%d", &select);
	switch (select){
		case 1: Mine();
			break;
		case 2:exit(0);
			break;
		default:
			break;
	}
	system("pause");
	return 0;
}
```

##### 3、Mine() 扫雷函数

```c
void Mine(){

	char mine_board[ROW][COL];		//定义出雷盘数组和显示盘数组
	char show_board[ROW][COL];
	
	memset(mine_board, '0', sizeof(mine_board));	 //雷盘全部初始化为0
	memset(show_board, '*', sizeof(show_board))；	//显示盘全部初始化为*
	
    //打印棋盘
	//PrintBoard(mine_board, ROW, COL);			//打印出两个盘	
	PrintBoard(show_board, ROW, COL);			//雷盘在测试阶段可以用来测试

	//用户输入
	Click(mine_board,show_board);
}
```

##### 4、PrintBoard( )  打印棋盘函数

​	用来对雷盘或者显示盘进行显示	，用的是制表符，打印出来显得更加规整

```c
void PrintBoard(char board[][COL], int _row, int _col){
	printf("   ");
	for (int i = 1; i < 11; i++){
		printf("   %d    ", i);
	}
	printf("\n");
	printf("  ");
	for (int i = 0; i < 10; i++){
		if (i == 0){
			printf("┌───┬");
			continue;
		}
		if (i == 9){
			printf("───┐");
			continue;
		}
		printf("───┬", i);
	}
	printf("\n");
	for (int i = 1; i <= _row - 2; i++){
		for (int count = 0; count < 10; count++){
			if (count == 0){
				printf("  │");
			}
			printf("      │");
		}
		printf("\n");
		printf("%2d", i);
		for (int j = 1; j <= _col - 2; j++){
			if (j == 1){
				printf("│");
			}
			printf("   %c  │", board[i][j]);
		}
		printf("\n");
		for (int count = 0; count < 10; count++){
			if (count == 0){
				printf("  │");
			}
			printf("      │");
		}
		printf("\n");
		if (i != 10){
			for (int j = 0; j < 10; j++){
				if (j == 0){
					printf("  ├───┼");
					continue;
				}
				if (j == 9){
					printf("───┤");
					continue;
				}
				printf("───┼");
			}
			printf("\n");
		}
	}
	for (int i = 0; i < 10; i++){
		if (i == 0){
			printf("  └───┴");
			continue;
		}
		if (i == 9){
			printf("───┘");
			continue;
		}
		printf("───┴", i);
	}
	printf("\n");
	printf("请输入要点击的坐标<1~10>:\n");
}
```

​	打印出来的样例：有木有很漂(xian)亮(qi)​	 ![ ](E:\MyCode\MyStudyNote\C\mine\mine.png)

##### 5、Click( )    用户输入

​	当我们玩这个游戏时，第一步时肯定不会踩到雷的，所以要单独处理，从第二次往后就是用户点击，检查是否有雷，若该没有雷则判断周围是否有雷，若有雷则显示雷数，不然显示空格进行展开。

```c
void Click(char mine[][COL], char show[][COL]){
	int is_win = 0;		//记录没有踩到雷的个数
	int x = 0;
	int y = 0;
	while (1){
		scanf("%d %d", &x, &y);		//让用户进行输入
		if (x < 1 || x > 10 || y < 1 || y > 10){
			printf("你的输入有误,请重新输入：\n");
			fflush(stdin);
			continue;
		}
		fflush(stdin);
		if (is_win == 0){			//如果是第一次输入
			mine[x][y] = 'f';		//把雷盘相应的项设置成f
			CreateMine(mine, ROW, COL);		//随机生成雷
		}
		if (show[x][y] != '*'){			//检测该位置是否已经展开或者点击
			printf("该位置已经点击或者展开过了！\n");
			continue;
		}
		if (mine[x][y] == '1'){			//踩到雷
			printf("很遗憾，你点到雷了!\n");
			printf("游戏结束！\n");
			break;
		}
		int mine_num = AroundMine(mine, x ,y);	//用户输入正确，判断周围雷数
		if (mine_num == 0){
			show[x][y] = ' ';	//没有雷存入空格  
			CheckAround(mine, show, x, y, &is_win);		//递归展开
		}else{
			show[x][y] = mine_num + '0';	//存入雷数
		}
		is_win++; 
        
		if ((100 - is_win) == MINE_NUM){		//如果条件成立，说明玩家已经赢了
			printf("你赢了\n");
			break;
		}
		system("cls");
		PrintBoard(mine, ROW, COL);
		PrintBoard(show, ROW, COL);
	}
}
```

##### 6、CreateMine( )  随机生成雷

```c
int GetRandomNum(int _start, int _end){		
	return rand() % (_end - _start + 1) + 1;	//获得一个1到10的随机数
}

void CreateMine(char mine[][COL], int _row, int _col){
	int row = 0;
	int col = 0;
	srand((unsigned int)time(NULL));		//设置随机种子
	int number = MINE_NUM;					//设置雷数
	while (number){
		row = GetRandomNum(1, _col - 2);
		col = GetRandomNum(1, _col - 2);
		if (mine[row][col] == '0'){		//如果雷盘该处为0，则放雷
			mine[row][col] = '1';		//用户第一次输入时雷盘内是f，第一次点击处不放雷
			number--;
		}
	}
}
```

##### 7、AroundMine( )  检测当前位置周围的雷数

```c
int AroundMine(char mine[][COL],int x,int y){
	int count = 0;
	for (int i = x - 1; i <= x + 1; i++){
		for (int j = y - 1; j <= y + 1; j++){
			if (mine[i][j] == '1'){
				count++;
			}
		}
	}
	return count;
}
```

##### 8、CheckAround( ) 递归检测

```c
int CheckAround(char mine[][ROW], char show[][COL], int x, int y, int *is_win){
	int mine_num = -1;
	if (x < 1 || x>10 || y < 1 || y > 10){		//保证传入参数的合法性
		return 0;
	}
	for (int i = x - 1; i <= x + 1; i++){
		for (int j = y - 1; j <= y + 1; j++){
			//不检测超过棋盘范围和其本身
			if ((i == x && j == y) || i > 10 || i < 1 || j > 10 || j < 1){
				continue;
			}
			//不检测已经被点击过或者展开过的
			if (show[i][j] != '*'){
				continue;
			}
			//剩下的本身周围是没有被展开过的
			if (AroundMine(mine, i, j) == 0){
				show[i][j] = ' ';
				(*is_win)++;
				CheckAround(mine, show ,i, j, is_win);
			}
 		}
	}
	return 0;
}
```

​	到这里我们就差不多完成了这个扫雷小程序，让我们看看结果把，这里我只设置了两个雷，看看递归展开之后的结果。

​	![](E:\MyCode\MyStudyNote\C\mine\dihuizhankai.png)

​	有木有很NICE呢？





​	如果需要源代码请移步我的GitHub

​	[GitHub - YeLing0119 - Mine](https://github.com/YeLing0119/MyCode/tree/master/Game/mine)

​	由于本人才疏学浅，若有疏忽还望不吝赐教。

​	@YeLing0119