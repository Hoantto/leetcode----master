### 链表 + 递归
本题的递归思想为：
- `head`后面有值而且和`head`的值相同，那么就找到不相等为止，然后对后面一个节点去递归，这样就把前面重复的给删除掉了。
- `head`后面有值但和`head`的值不等，那么就递归后面一个节点，接在`head`的后面。
- 最后返回`head`。

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
            while(head->next && head->val == head->next->val){
                head = head->next;
            }
            return deleteDuplicates(head->next);
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
            while head.next and head.val == head.next.val:
                head = head.next
            return self.deleteDuplicates(head.next)
        else:
            head.next = self.deleteDuplicates(head.next)
        return head
```