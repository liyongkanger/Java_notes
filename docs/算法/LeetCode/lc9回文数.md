### lc9回文数

[//]: # "推荐题解模板，请替换blablabla等内容 ^^"

### 题目描述
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
#### 样例

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

----------
#### 转换为字符串求解
```
class Solution {
public:
    bool isPalindrome(int x) {
       if(x < 0) return 0;

       string s = to_string(x);
       return s == string(s.rbegin(), s.rend());
    }
};

```
#### 反转对比
```
class Solution {
public:
    bool isPalindrome(int x) {
       if(x < 0)  return false;
       int t = x;
       // long long 存储防止溢出
       //反转后对比
       long long res = 0;
       while(x){
           res = res * 10 + x % 10;
           x /= 10;
       }
       return res == t;
    }
};


```
#### int型存储
```c++
class Solution {
public:
    /*
    判断一个数是否是回文数
    1.负数不是回文数
    2.如果是回文数 逆序和正序是相等的 （防止最大溢出）

    改进： 先算出后一半的逆序值 在判断和前一半是否相等即可
    */
    bool isPalindrome(int x) {
       // 负数 , 0 , 首位不为 0 
      if(x < 0 || x && x % 10 == 0)  return false;
      //记录尾数
      int s = 0;
      //边生成边判断
      while(s <= x){
          //逆序x 生成s  
          s = s * 10 + x % 10;
          //特判 分奇数偶数情况
          if(s == x || s == x / 10) return true;
          x /= 10;
      }
      return false;

    }
};
```