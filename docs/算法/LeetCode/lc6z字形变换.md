### lc6z字形变换

### 题目描述
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

L   C   I   R
E T O E S I I G
E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。


#### 样例

```
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:

L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

----------

#### 思路
```
0     6       12
1   5 7    11 ..
2 4   8 10
3     9
```
1. 对于第一行和最后一行，是公差为2(n-1)的等差数列， 首项是0 和 n - 1 
2. 对于第 i 行 是两个公差为2(n-1)的等差数列交替排列， 首项分别是 i和2n - i - 2
3. 时间复杂度O(n)

```c++
class Solution {
public:
    string convert(string s, int numRows) {
      string res;

      if(numRows == 1) return s;
     
      for(int j = 0; j < numRows; j ++){
           //如果是第一行或者是最后一行 公差是 2(n -1) 首项是 0 或者 n - 1
           if(j == 0 || j == numRows - 1){
               //遍历 将等差公式下标放入
              for(int i = j; i < s.size(); i += (numRows - 1) * 2){
                  res += s[i];
              }
              // 其他行
          }else{
                //公差为2(n -1 )等差数列 首项是 i  和  2n - i - 2
              for(int k = j,i = numRows * 2 - j - 2;
                i < s.size() || k < s.size() ;
                i += (numRows - 1) * 2 , k += (numRows - 1) * 2){
                    //交替出现
                    if(k < s.size()) res += s[k];
                    if(i < s.size()) res += s[i];
                }
          }
      }
      return res;
    } 
};
```