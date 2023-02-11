### 链表模拟
由于链表中都是**逆序排列**的，同时遍历两表，逐位计算它们的和，并与当前位置的进位相加，如果当前位置两个链表处相应的位置的数字为$n1,n2$，进位值为$carry$，则它们的和为$(n1+n2+carry)\ mod\ 10$，而新的进位值为$\lfloor \frac{n1+n2+carry}{10} \rfloor$。

如果两个链表的长度不同，则可以认为在长度短的链表后面有若干个0。

此外，如果链表遍历结束后，有$carry>0$，还需要在答案链表后面附加一个节点，节点的值为$carry$。

**代码**
*C++ version*
```
class Solution{
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2){
        ListNode *head = nullptr, *tail = nullptr;
        int carry = 0;
        while(l1 || l2){
            int n1 = l1? l1->val: 0;
            int n2 = l2? l2->val: 0;
            int sum = n1 + n2 + carry;
            if(!head){
                head = tail = new ListNode(sum % 10);
            }else{
                tail->next = new ListNode(sum % 10);
                tail = tail->next;
            }
            carry = sum / 10;
            if(l1){
                l1 = l1->next;
            }
            if(l2){
                l2 = l2->next;
            }
        }
        if(carry > 10){
            tail->next = new ListNode(carry);
        }
        return head;
    }
};
```
**复杂度分析**
> - 时间复杂度：$O(max(m,n))$，其中m和n分别为两个链表的长度。我们要遍历两个链表的全部位置，而处理每个位置只需要$O(1)$的时间。
> - 空间复杂度： $O(1)$。注意返回值不计入空间复杂度。