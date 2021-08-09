## 第十届蓝桥杯省赛C++B组

### 一. 组队

作为篮球队教练，你需要从以下名单中选出 1 号位至 5 号位各一名球员，组成球队的首发阵容。
每位球员担任 1 号位至 5 号位时的评分如下表所示。请你计算首发阵容 1 号位至 5 号位的评分之和最大可能是多少？

```txt
1 97 90 0 0 0
2 92 85 96 0 0
3 0 0 0 0 93
4 0 0 0 80 86
5 89 83 97 0 0
6 82 86 0 0 0
7 0 0 0 87 90
8 0 97 96 0 0
9 0 0 89 0 0
10 95 99 0 0 0
11 0 0 96 97 0
12 0 0 0 93 98
13 94 91 0 0 0
14 0 83 87 0 0
15 0 0 98 97 98
16 0 0 0 93 86
17 98 83 99 98 81
18 93 87 92 96 98
19 0 0 0 89 92
20 0 99 96 95 81
```

#### 答案 490

```c++
#include <stdio.h>
#include <stdbool.h> // C语言加上这个
#define N 20
#define M 6
int a[N][M];
int b[M], maxi;
void dfs(int cur, int sum) {
    if (cur == M) {
        if (maxi <= sum) {
            maxi = sum;
            printf("sum = %d, list = [", sum);
            for (int i = 1; i < M; ++i)
                printf("%d%c", b[i] + 1, (i < M - 1 ? ',' : ']'));
            printf("\n");
        }
    }
    else for (int i = 0; i < N; ++i) {
        bool ok = true;
        for (int j = 1; j < cur; ++j)
            if (b[j] == i) { ok = false; break; }
        if (ok) {
            b[cur] = i;
            dfs(cur + 1, sum + a[i][cur]);
        }
    }
}
int main() {
    for (int i = 0; i < N; ++i)
        for (int j = 0; j < M; ++j)
            scanf("%d", &a[i][j]);
    dfs(1, 0);
    return 0;
}

```

### 二. 年号字串

小明用字母A 对应数字1，B 对应2，以此类推，用Z 对应26。对于27以上的数字，小明用两位或更长位的字符串来对应，例如AA 对应27，AB 对应28，AZ 对应52，LQ 对应329。
请问2019 对应的字符串是什么？

```c++
#include<iostream>
using namespace std;

//702 --> ZZ
// 703 --> AAA
//18278 --> ZZZ
//18279 --> AAAA
void dfs(int N){
    if(N > 26) dfs((N - 1) / 26);
    putchar('A' + (N - 1) % 26);
}
int main(){
    int N;
    while(cin >> N){
        dfs(N);
        cout << endl;
    }
    return 0;
}


```

#### 答案： BYQ

