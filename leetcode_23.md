### 优先队列
重点在于对**api**的熟悉，需要掌握**C++ priority_queue**的声明方式，**decltype**和**C++ lambda表达式**的运用以达到自定义容器的目的。还需要掌握**python setattr "monkey patch"**和**heapq**自定义的底层逻辑，通过修改**heapq中item的__lt__函数**来达到自定义容器的目的。

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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        auto head = ListNode(0);
        //C++ lambda
        auto comp = [](ListNode* const &a, ListNode* const &b){return a->val > b->val;};
        priority_queue<ListNode*, vector<ListNode*>, decltype(comp)> q(comp);
        //头结点入队
        for(auto &h: lists) if (h != nullptr) q.push(h);
        auto p = &head;
        while(!q.empty()){
            p->next = q.top();
            p = p->next;
            q.pop();
            //出队结点的下一个结点入队
            if(p->next != nullptr) q.push(p->next);
        }
        return head.next;
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
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        
        setattr(ListNode, "__lt__", lambda a,b: a.val <= b.val)

        pq = []
        for l in lists:
            if l:
                heapq.heappush(pq, l)
        out = ListNode(None)
        head = out
        while pq:
            l = heapq.heappop(pq)
            head.next = l
            head = head.next
            if l.next:
                heapq.heappush(pq, l.next)
        return out.next
```

**参考**
- [What do I use for a max-heap implementation in Python?](https://stackoverflow.com/questions/2501457/what-do-i-use-for-a-max-heap-implementation-in-python)
- [Python setattr()](https://www.programiz.com/python-programming/methods/built-in/setattr)
- [Simple python heapq with custom comparator function](https://leetcode.com/problems/merge-k-sorted-lists/solutions/368112/simple-python-heapq-with-custom-comparator-function/)
- [Type Inference in C++ (auto and decltype)](https://www.geeksforgeeks.org/type-inference-in-c-auto-and-decltype/)
- [C++优先队列的重载（最小堆、最大堆）](https://blog.csdn.net/sinat_37205608/article/details/82460301)
- [declaring a priority_queue in c++ with a custom comparator](https://stackoverflow.com/questions/16111337/declaring-a-priority-queue-in-c-with-a-custom-comparator)
- [C++ priority_queue api](https://cplusplus.com/reference/queue/priority_queue/)