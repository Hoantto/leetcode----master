### 链表 + 递归
这题是`leetcode_82`的低阶版本，思路更加简洁一点。

**代码**
*C++ version*
```
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
    ListNode* deleteDuplicates(ListNode* head) {
        if(!head) return head;
        if(head->next && head->val == head->next->val){
            ListNode* cur_top = deleteDuplicates(head->next);
            head->next = cur_top->next;
        }else{
            head->next = deleteDuplicates(head->next);
        }
        return head;
    }
};
```

*python version*
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head == None:
            return head
        if head.next and head.val == head.next.val:
            cur_top = self.deleteDuplicates(head.next)
            head.next = cur_top.next
        else:
            head.next = self.deleteDuplicates(head.next)
        return head
```