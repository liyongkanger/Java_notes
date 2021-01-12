### lc2两数相加

### 题目描述
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
#### 样例

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

----------

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    
       ListNode *res = new ListNode(-1); // 添加虚拟头结点，因为头结点可能有操作
       ListNode *cur = res;
       int carry = 0; //进位

     //从最低位到最高位， 每一位相加
       while(l1 || l2){
           int n1  = l1 ? l1->val : 0; //如果l1不为空则赋值
           int n2  = l2 ? l2->val : 0;
           int sum = n1 + n2 + carry;

           carry = sum / 10;
           cur->next = new ListNode(sum % 10);
           cur = cur -> next;
           if(l1) l1 = l1->next;
           if(l2) l2 = l2->next;
       }
        //判断最后的进位是否为0
        if(carry) cur -> next = new ListNode(1);
        return res -> next;

    }
};
```

