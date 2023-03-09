### 链表模拟
利用快慢指针找到链表倒数`k % len`位置以及其前一个位置即可。

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
    ListNode* rotateRight(ListNode* head, int k) {
        ListNode* l = head;
        int len = 0;
        while(l){
            len++;
            l = l->next;
        }
        if(len == 0){
            return head;
        }
        k = k % len;
        if(k == 0){
            return head;
        }else{
            k--;
        }
        ListNode* prev = new ListNode(0, head);
        ListNode* tmp = prev;
        l = head;
        ListNode* m = head;
        while(k--){
            m = m->next;
        }
        while(m->next){
            prev = prev->next;
            l = l->next;
            m = m->next;
        }
        tmp->next = nullptr;
        prev->next = nullptr;
        m->next = head;
        return l;
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
    def rotateRight(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        l = head
        length = 0
        while l:
            length += 1
            l = l.next
        if length == 0:
            return head
        k = k % length
        if k == 0:
            return head
        else:
            k -= 1
        prev = ListNode(0, head)
        tmp = prev
        l = head
        m = head
        while k:
            m = m.next
            k -= 1
        while m.next:
            prev = prev.next
            l = l.next
            m = m.next
        tmp.next = None
        prev.next = None
        m.next = head
        return l
```