### lc28实现strStr()

### 题目描述
实现 strStr() 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

#### 样例

```
输入: haystack = "hello", needle = "ll"
输出: 2
```

----------

#### 算法思路
kmp算法
1. ne[i] 记录p[] 数组中 以i结尾 的 最大后缀 等于 最大前缀的末尾位置
2. 预处理出ne[]数组
3. 枚举s[]数组，利用ne[]数组找出最大前缀长度是m的对应的最大后缀的位置i  因此[i - m , m]是当前符合条件的子串
   


```c++
class Solution {
public:
 /*
    返回第一次出现的下标
    kmp算法
      1. ne[i] 记录p[] 数组中 以i结尾 的 最大后缀 等于 最大前缀的末尾位置
      2. 预处理出ne[]数组
      3. 枚举s[]数组，利用ne[]数组找出最大前缀长度是m的对应的最大后缀的位置i 
      因此[i - m , m]是当前符合条件的子串
    */
    int strStr(string s, string p) {
      if(p.empty()) return 0;

      int n = s.size(), m = p.size();
      s = ' ' + s, p = ' ' + p;

      vector<int> next(m +1);
        //预处理next数组， next[i]记录p数组中以i结尾的最大后缀等于最大前缀时的 最大前缀的末尾位置
      for(int i = 2, j = 0; i <= m; i ++){
          while(j && p[i] != p[j+1]) j = next[j];
          if(p[i] == p[j + 1]) j ++;
          next[i] = j;
      }

       //枚举s[] 找出最大前缀长度为m的最大后缀的位置i 
      for(int i = 1, j = 0; i <= n; i ++){
          while(j && s[i] != p[j + 1]) j = next[j];
          if(s[i] == p[j + 1]) j ++;
          if(j == m){
            j = next[j];
            return i - m;
          }
      }
      return -1;
    }
};
```