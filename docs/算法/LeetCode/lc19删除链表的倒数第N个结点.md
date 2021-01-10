### lc19删除链表的倒数第N个结点

### 题目描述
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。


#### 样例

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

----------
#### 思路
1. 删除倒数第k个就是删除 正数第 n - k个
2. 删除p的下一个 `p->next = p->next->next`, 所以遍历移动到要删除结点的上一个`n - k + 1`
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
public:
    //删除倒数第n个 就是删除正数sum - n 个
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        //定义虚拟头结点 因为头结点可能被删除
        auto dummy = new ListNode(-1);
        dummy->next = head;
       
        int sum = 0;
        for(auto p = dummy; p ; p = p->next) sum ++; //求出多少个结点

        auto p = dummy;
        // p的下一个就是删除的结点
        for(int i = 0; i < sum - n - 1; i ++) p = p->next;
        // p下一个指向p的下一个的下个 等于删除p的下一个
        p->next = p->next->next;

        return dummy ->next;

       

    }
};
```

