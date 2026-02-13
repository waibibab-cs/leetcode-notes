# 25、K个一组翻转链表
hard

cpp
## 题解
1. 为了方便处理边界，首先可以定义一个虚拟节点
2. 计算出结点总数n之后，分别对每组进行反转
3. 难点在于每组反转之后的前后拼接,要维护每次的p指向当前组的反转后的最后一个结点、p->next指向下一组反转后的最后一个结点（下一组当前的第一个结点）
   * 先后拼接：直接该组最后一个结点的p->next的next指向下一组第一个结点cur
   * 再前拼接：上一组的最后一个结点p的next指向该组第一个结点pre
   * 更新p为该组反转后的最后一个结点，同时p->next就已经指向了下一组反转前的第一个结点也就是反转后的最后一个结点

C++
```cpp
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
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode dummy(0, head);
        ListNode* p = &dummy;
        int n = 0;
        ListNode* cur = head;
        while (cur) {
            cur = cur->next;
            n ++ ;
        }
        cur = head;
        ListNode* pre = NULL;
        for (int i = n; i >= k; i -= k) {
            for (int j = 0; j < k; j ++ ) {
                ListNode* nxt = cur->next;
                cur->next = pre;
                pre = cur;
                cur = nxt;
            }
            ListNode* tmp = p->next;
            p->next->next = cur;
            p->next = pre;
            p = tmp;
        }
        return dummy.next;
    }
};
```
