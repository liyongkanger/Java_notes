### lc5最长回文子串

### 题目描述
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。


#### 样例

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案
```

----------

暴力写法 O(n^2)
```c++
class Solution {
public:
     //暴力 最长回文子串 时间复杂度O(n^2)
     /*
      枚举回文串中心i  然后分2中情况向离岸边扩展，直到遇到不同的字符为止
      回文串分奇数偶数
      奇数  s[i-k] == s[i + k]
      偶数  s[i-k] == s[i + k - 1]
     */
    string longestPalindrome(string s) {
        int res = 0;
        string str;
        for(int i = 0; i < s.size(); i ++){
                         // i
            for(int j = 0; i - j >= 0 && i + j < s.size(); j ++){
                //奇数  回文中心向离两边扩展
               if(s[i - j] == s[i + j]){
                   // 如果回文串 > res 更新 res
                   if(j * 2 + 1 > res){
                       res = j * 2 + 1;
                       str = s.substr(i - j, j * 2 + 1);
                   }
               }else
                    break;
            }
            //偶数
            for(int j = i, k = i + 1; j >= 0 && k < s.size(); j --, k ++){
                  if(s[j] == s[k]){
                      if(k -j + 1 > res){
                          res = k - j + 1;
                          str = s.substr(j, k - j + 1);
                      }
                  }
            }
        }
        return str;
    }
};
```