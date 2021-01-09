
#### 1.方程: a2+b2+c2=1000这个方程有整数解吗？有：a,b,c=6,8,30 就是一组解。你能算出另一组合适的解吗？
> 请填写该解中最小的数字

#### 思路
1. 直接暴力打表找出最小值，因为题目中只说了正数所以可以为负数
```c++
#include<iostream>

using namespace std;

int main()
{
   for(int i = 0;i < 100;i++)
   {
      for(int j = 0;j < 100;j++)
      {
         for(int k = 0;k < 100;k++)
         {
            if(i * i + j * j + k * k == 1000)
            cout << i << "  " << j << " "<< k << endl; 
         }     
      }

   }
   return 0;
} 
```

答案： `-30`

#### 2.星系炸弹

在X星系的广袤空间中漂浮着许多X星人造“炸弹”，用来作为宇宙中的路标。
每个炸弹都可以设定多少天之后爆炸。

比如：阿尔法炸弹2015年1月1日放置，定时为15天，则它在2015年1月16日爆炸。
有一个贝塔炸弹，2014年11月9日放置，定时为1000天，请你计算它爆炸的准确日期。

请填写该日期，格式为 yyyy-mm-dd 即4位年份2位月份2位日期。比如：2015-02-19
请严格按照格式书写。不能出现其它文字或符号。

#### 思路1

直接用excel

#### 思路2

```c++

```



#### 3.奇妙的数字

小明发现了一个奇妙的数字。它的平方和立方正好把0~9的10个数字每个用且只用了一次。
你能猜出这个数字是多少吗？

请填写该数字，不要填写任何多余的内容。



暴力求解判断

```c++
#include<iostream>

using namespace std;

typedef long long LL;

bool check(int x){
    //防止溢出
    LL a = x * x, b = x * x * x;
    
    int nums[10] = {0};
    //用一个数组存放每一位的数字
    while(a){
        
       nums[a % 10] ++;
       //重复数字出现返回false
      if(nums[a % 10] > 1) return false;
        a /= 10;
    }
    
    while(b){
        nums[b % 10] ++;
        if(nums[b % 10] > 1) return false;
        b /= 10;
    }
    //检查是否用完9个数字
    for(int i = 0; i < 10; i ++){
        if(nums[i] == 0){
            return false;
        }
    }
    return true;
    
}
int main(){
    
    //直接暴力求解
    for(int i = 1; i < 1000; i ++){
        if(check(i)){
            cout << i << endl;
            cout << i * i << " " << i * i * i << endl;
        }
    }
    return 0;
}
```

#### 答案： 69

#### 4. 格子中输出

StringInGrid函数会在一个指定大小的格子中打印指定的字符串。
要求字符串在水平、垂直两个方向上都居中。
如果字符串太长，就截断。
如果不能恰好居中，可以稍稍偏左或者偏上一点。

下面的程序实现这个逻辑，请填写划线部分缺少的代码。

```c++
#include <stdio.h>
#include <string.h>
 
void StringInGrid(int width, int height, const char* s)
{
	int i,k;
	char buf[1000];
	strcpy(buf, s);
	if(strlen(s)>width-2) buf[width-2]=0;
	
	printf("+");
	for(i=0;i<width-2;i++) printf("-");
	printf("+\n");
	
	for(k=1; k<(height-1)/2;k++){
		printf("|");
		for(i=0;i<width-2;i++) printf(" ");
		printf("|\n");
	}
	
	printf("|");
	//%*s后面要传2个参数，一个是数字n一个数字符串s，表示输出的字符串
	//长度为n，如果s的长度不够， 则在前面用空格补充
	printf("%*s%s%*s",(width-2-strlen(buf))/2, "",buf,(width-2-strlen(buf)+1)/2,"");  //填空
	          
	printf("|\n");
	
	for(k=(height-1)/2+1; k<height-1; k++){
		printf("|");
		for(i=0;i<width-2;i++) printf(" ");
		printf("|\n");
	}	
	
	printf("+");
	for(i=0;i<width-2;i++) printf("-");
	printf("+\n");	
}
 
int main()
{
	StringInGrid(20,6,"abcd1234");
	return 0;
}
```

#### 答案： `width-2-strlen(buf))/2, "",buf,(width-2-strlen(buf)+1)/2,""`

#### 第五题 九数字分组

1,2,3...9 这九个数字组成一个分数，其值恰好为1/3，如何组法？

下面的程序实现了该功能，请填写划线部分缺失的代码。

```c++
#include <stdio.h>
 
void test(int x[])
{
	int a = x[0]*1000 + x[1]*100 + x[2]*10 + x[3];
	int b = x[4]*10000 + x[5]*1000 + x[6]*100 + x[7]*10 + x[8];
	
	if(a*3==b) printf("%d / %d\n", a, b);
}
 
void f(int x[], int k)
{
	int i,t;
	if(k>=9){
		test(x);
		return;
	}
	
	for(i=k; i<9; i++){
		{t=x[k]; x[k]=x[i]; x[i]=t;}
		f(x,k+1);
		_____________________________________________ // 填空处
	}
}
	
int main()
{
	int x[] = {1,2,3,4,5,6,7,8,9};
	f(x,0);	
	return 0;
}
```

深搜+回溯，每次交换两个元素的位置，遍历所有的情况，符合条件的情况则输出。

还原现场

#### 答案：t = x[i], x[i] = x[k], x[k] = t;

#### 第六题

牌型种数

小明被劫持到X赌城，被迫与其他3人玩牌。
一副扑克牌（去掉大小王牌，共52张），均匀发给4个人，每个人13张。
这时，小明脑子里突然冒出一个问题：
如果不考虑花色，只考虑点数，也不考虑自己得到的牌的先后顺序，自己手里能拿到的初始牌型组合一共有多少种呢？

请填写该整数，不要填写任何多余的内容或说明文字。

```c++
#include<iostream>
#include<algorithm>

using namespace std;

int cot = 0;
//每种类型的拍可以拿4张
//每个人13张牌
//sum当前拿了多少张 k是拿几种类型的牌最多有13种
void dfs(int sum, int k){

    if(sum == 13){
        //方案数+1
        cot ++;
        return ;
    }
    if(k == 14) return ;
    //每种类型的牌拿几张
    for(int i = 0; i <= 4; i ++){
        dfs(sum +i, k + 1);
    }
    
}

int main(){
    
    dfs(0, 1);
    cout << cot << endl;
}
```

#### 答案：3598180

#### 第七题

手链样式

小明有3颗红珊瑚，4颗白珊瑚，5颗黄玛瑙。
他想用它们串成一圈作为手链，送给女朋友。
现在小明想知道：如果考虑手链可以随意转动或翻转，一共可以有多少不同的组合样式呢？

请你提交该整数。不要填写任何多余的内容或说明性的文字。





