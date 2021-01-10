### lc25k个一组反转链表

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。
### 题目描述
```
给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5。
```


#### 样例

```
blablabla
```

----------

#### 思路
1.k个一组反转链表，分为3步
2. 遍历判断是否足够k个,定义q指针遍历，不足够则break
3. 反转k个结点k个节点 k-1次操作， `a = p-next b = a-next`
4. p的下一个元素指向a,p的下个元素的下一个指向b(画图理解)

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
    /*
    1.内部反转
    2.前面的链接指针改变
    3.后面链接的指针改变
    */
    ListNode* reverseKGroup(ListNode* head, int k) {
        //反转相邻k个元素 画图5个点来表示关系
        auto dummy =  new ListNode(-1);
        dummy->next = head;
        for(auto p = dummy ; ;){
            //遍历是否足够k个
            auto q = p;
            for(int i = 0; i < k  && q; i ++) q = q->next;
            if(!q) break;
            //1.定义2个指针反转内部结点
            auto a = p->next, b = a -> next;
            // k个元素 k-1次操作
            for(int i = 0; i < k - 1; i ++){
                //将第二个的后一个存下来
                auto c = b->next;
                //第二个指针指向前一个
                b->next = a;
                //指针后移
                a = b;
                b = c;
            }
            //p的下一个元素指向a  p很后面那个元素指向b
            auto c = p->next;
            p->next = a;
            c ->next = b;
            p = c;
        }
        return dummy->next;
    }
};
```