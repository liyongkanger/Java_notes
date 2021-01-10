### ac851spfa求最短路

### 题目描述
给定一个n个点m条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你求出1号点到n号点的最短距离，如果无法从1号点走到n号点，则输出impossible。

数据保证不存在负权回路

#### 样例
####输入

```
3 3
1 2 5
2 3 -3
1 3 4
```
#### 输出
```
2
```

----------

#### 思路
1. spfa算法解决稀疏图最短路的问题算法类似bfs
2. 开一个bool数组表示点是否存在于队列
```c++
#include<iostream>
#include<algorithm>
#include<cstring>
#include<queue>

using namespace std;

const int N = 100010;
//稀疏图
int n, m;
int h[N], w[N], e[N], ne[N], idx;
int dist[N];
bool st[N];//这里的bool数组与dijstra的bool数组不一样， 这里bool数组是确认入队，dijkstra是标记到源点的最短路

void add(int a, int b, int c){
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++;
}
/*
spfa 
1.初始化 dist 和 dist[1] = 0;
2.用一个队列存结点， 如果当前结点进入队列则st[t] = true;
3.当队列不为空的时候  用更新过的值去更新其他的点
*/
int spfa(){
 memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    queue<int> q;
    q.push(1);
    st[1] = true;
   
   while(q.size()){
    
     int t = q.front();
        q.pop();

        st[t] = false;
       
       for(int i = h[t]; i != -1; i = ne[i]){
           int j = e[i];
           if(dist[j] > dist[t] + w[i]){
               dist[j] = dist[t] + w[i];
               if(!st[j]){
                   q.push(j);
                   st[j] = true;
               }
           }
       }
   }
   return dist[n];
}


int main(){
    
   scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);

    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }

    int t = spfa();

    if (t == 0x3f3f3f3f) puts("impossible");
    else printf("%d\n", t);

    return 0;
}
```

