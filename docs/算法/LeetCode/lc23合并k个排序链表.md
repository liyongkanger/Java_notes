### lc23合并k个排序链表

### 题目描述
给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表

#### 样例

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6

```

----------

#### 思路
1. 合并多个链表组采用并归排序的思想 多个指着指向最小的然后比较，每次取最小的因此我们可以优化用小根堆来存这个lists, 优先队列是大根堆我们在重写operator的时候写成`>`每次取出最小的加入结果链表中
2.遍历整个堆 每次弹出最小的加入链表，记得查看一下最小的next是否在堆中

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
    /*
    合并K个有序链表


    */
public:

    struct Cmp{
        //优先队列本来是大根堆这里用 >  变成小根堆
        bool operator()(ListNode* a, ListNode* b){
            return a->val > b->val;
        }
    };

    ListNode* mergeKLists(vector<ListNode*>& lists) {
    //并归排序的思路每次找出一个最小值， 我们这里优化用一个堆来取最小值 时间复杂度nlogn
      priority_queue<ListNode*, vector<ListNode*>, Cmp> heap;

      auto dummy = new ListNode(-1), tail = dummy;
      // 将值放入堆中
      for(auto l : lists) if(l) heap.push(l);
 
     //
      while(heap.size()){
          //每次访问堆顶元素
          auto t = heap.top();
          heap.pop();
         // 将小的放入新的链表中
          tail->next = t;
          tail = tail->next;
          //查看对顶元素是否有下一个结点， 如果有放入堆中
          if(t->next) heap.push(t->next);
      }
      return dummy->next;
    }
};
```