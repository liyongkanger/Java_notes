###  ac849Dijkstra求最短路I

### 题目描述
给定一个n个点m条边的有向图，图中可能存在重边和自环，所有边权均为正值。

请你求出1号点到n号点的最短距离，如果无法从1号点走到n号点，则输出-1。


#### 样例
#### 输入
```
3 3
1 2 2
2 3 1
1 3 4
```
#### 输出
```
3
```

----------

#### 思路
dijkstra()最短路   稠密图找最短路径
初始化   dist数组全部初始化最大值 更新dist[1] = 0
1. 遍历 剩余 n - 1个结点
2. 找到一个点距离源点最近的点
3. 用这个最近的点来更新其他点到源点的最近的距离

```c++
#include<cstring>
#include<iostream>
#include<algorithm>

/*
dijkstra()最短路   稠密图找最短路径
初始化   dist数组全部初始化最大值 更新dist[1] = 0
1.遍历 剩余 n - 1个结点
2.找到一个点距离源点最近的点
3.用这个最近的点来更新其他点到源点的最近的距离

*/
using namespace std;

const int N = 510;
int n, m;
int g[N][N];
int dist[N];
bool st[N]; //确定是否是最短路

int dijkstra(){
    //初始化dist数组
    memset(dist, 0x3f,  sizeof dist);
    dist[1] = 0;
    
    for(int i = 0; i < n - 1; i ++){
    
        int t = -1; 
        //遍历找出距离源点最短的点
        for(int j = 1; j <= n; j ++){
            if(!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
        }
        //点t是最短路
        st[t] = true;
        
        //用t隔着点更新其他点到源点的距离
        for(int j = 1; j <= n; j ++)
           dist[j] = min(dist[j], dist[t] + g[t][j]);

    }
    
       if(dist[n] == 0x3f3f3f3f) return -1;
      return dist[n];
}


int main(){
    
    scanf("%d%d", &n, &m);
    
    memset(g, 0x3f, sizeof g);
    
    while(m --){
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        //解决重边问题
        g[a][b] = min(g[a][b], c);
    }
    printf("%d\n", dijkstra());
    
    return 0;
}
```