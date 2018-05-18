# 删除链表倒数第n个节点
---
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
  
**示例：**  
>给定一个链表: 1->2->3->4->5, 和 n = 2.
>
>当删除了倒数第二个节点后，链表变为 1->2->3->5.

**说明：**  
给定的 n 保证是有效的。  

**进阶：**  
你能尝试使用一趟扫描实现吗？  

---
**Solution：**  
**解法一：** 为了得到倒数第k个节点，很自然的想法是先走到链表的尾端，再从尾端回溯k步。但是单向链表中，每个节点存储的是下一个节点的指针，只能从头到尾遍历，
无法从尾到头回溯。因此我们需要先遍历一遍链表，得到链表的长度len；再遍历一遍链表，删除第len - n + 1个节点。这种解法比较容易想到，但是需要遍历两次链表。  
**解法二：** 使用两个指针  
- 第一个指针从头开始遍历，向前走n-1步，第二个指针不动
- 从第n步起，第一个指针和第二个指针一起往前走
- 当第一个指针指向链表的末尾时，第二个指针便指向了我们要删除的倒数第n个节点 

**需要考虑的情况：**
- 链表的头指针为空指针
- 删除的节点为链表的头节点

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if (head == NULL) {
            return head;
        }
        ListNode* firstNode = head;
        ListNode* secondNode = head;
        ListNode* preNode = secondNode;
        for (int i = 0; i < n - 1; i++) {
            firstNode = firstNode->next;
        }
        while (firstNode->next != NULL) {
            firstNode = firstNode->next;
            preNode = secondNode;
            secondNode = secondNode->next;
        }
        if (secondNode == head) {
            head = head->next;
            delete secondNode;
            return head;
        }
        preNode->next = secondNode->next;
        delete secondNode;
        
        return head;
    }
};
```
