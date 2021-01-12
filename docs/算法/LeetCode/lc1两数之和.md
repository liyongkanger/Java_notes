### lc1两数之和

### 题目描述
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
#### 示例
```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```



```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
       vector<int> res; //存放答案
       unordered_map<int, int> hash; //用一个hashmap来存
        //循环一遍nums数组 每步循环中我们做两件事
        //1. 判断target - nums[i] 是否在哈希表中
        //2. 将nums[i] 插入到哈希表中
        for(int i = 0; i < nums.size(); i ++){
        
            int another = target - nums[i];
            // 判断 hash表中key 是否等于 another
            if(hash.count(another)){
                res = vector<int>({hash[another], i});
                break;
            }
            //将数存入hash表中
            hash[nums[i]] = i;
        }
        return res;
    }
};
```