### lc3无重复字符的最长子串

### 题目描述

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。


#### 样例

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

----------
1. 指针j向后移动一位， 哈希表中s[j]的计数加1
2. 假设j移动前的区间[i, j]照片中没有重复字符， 则j移动后，只有s[j]可能出现2次， 不断向后移动i，直到区间中s[j]的位数为1

```c++
class Solution {
public:

    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> hash; // 表示[i,j]中每个字符出现的次数
        int res = 0;
         //双指针算法
        for(int i = 0, j = 0; j < s.size(); j ++){
            // key s[j]  vaule 出现次数
            hash[s[j]] ++;
            //当出现次数 > 1       后端指针i++  字母s[i]与s[j]相等 次数减1;
            while(hash[s[j]] > 1) hash[s[i ++]] --;
            // 取指针长度的最大值
            res = max(res, j - i + 1);
        }
        return res;
    }
};
```