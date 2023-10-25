---
title: Leetcode算法之链表
date: 2023-10-25 18:44:07
categories: 数据结构与算法
tags: Leetcode
---

## [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头

> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
> 输出：7 -> 0 -> 8
> 原因：342 + 465 = 807

思路：
1. 对于链表长度不一致的情况，补0
2. 只有当俩个链表均为null并且没有近位的情况下退出
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode(0);
        ListNode* res= head;
        int flag = 0;
        int sum = 0;
        while(l1 || l2 || flag)
        {
            int t1 = l1 == NULL ? 0 : l1->val;
            int t2 = l2 == NULL ? 0 : l2->val;
            sum = t1+t2+flag;

            flag = sum/10;
            sum = sum % 10;

            head->next = new ListNode(sum);
            head = head->next;
            
            l1 = l1 ? l1->next : l1;
            l2 = l2 ? l2->next : l2;
        }
        return res->next;
    }
};
```
## [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点

> 给定一个链表: 1->2->3->4->5, 和 n = 2.
> 当删除了倒数第二个节点后，链表变为 1->2->3->5.

思路：
1. 使用快慢指针，快指针先走N个节点，然后快慢指针一起走，知道快指针为NULL,此时慢指针即为倒数第N个节点
2. 由于要删除倒数第N个节点，需要保存其前节点，因此在遍历过程中，每次保存当前节点的前驱节点；或者让慢节点走到倒数第N+1个节点时，直接操作即可
3. 注意如果倒是第N个节点时头节点时，删除的方式需要注意，可以根据快节点走完N个节点的情况来判断
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* quick = head;
        ListNode* low = head;
        for(int i = 0; i < n; ++i)
            quick = quick->next;
        if(!quick)
        {
            head = head->next;
            return head;
        }
        while(quick->next)
        {
            quick = quick->next;
            low = low->next;
        }
        low->next = low->next->next;
        return head;
    }
};
```
## [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的

> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode(NULL);
        ListNode* res = head;
        while(l1 && l2)
        {
            if(l1->val < l2->val)
            {
                res->next = l1;
                l1 = l1->next;
            }             
            else
            {
                res->next = l2;
                l2 = l2->next;
            }
            res = res->next;
        }
        res->next = (l1 == NULL ? l2 : l1);
        return head->next;
    }
};
```

## [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)
给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表

```bash
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

```cpp
class Solution {
public:
    
    struct cmp{  
       bool operator()(ListNode *a,ListNode *b){
          return a->val > b->val;
       }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists) 
    {
        priority_queue<ListNode*, vector<ListNode*>, cmp> pt;
        ListNode* head = new ListNode(0);
        ListNode *p = head;

        for(auto node: lists)
        {
            if(node)
                pt.push(node);
        }
        while(!pt.empty())
        {
            ListNode* q = pt.top();
            pt.pop();
            p->next = q;
            p = q;
            if(q->next)
                pt.push(q->next);
        }
        return head->next;  
    }
};
```
## [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false
进阶：
你能用 O(1)（即，常量）内存解决此问题吗？
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919164611935.png#pic_center)

> 输入：head = [3,2,0,-4], pos = 1
> 输出：true
> 解释：链表中有一个环，其尾部连接到第二个节点。

思路：
1. 利用快慢指针，快指针每次走俩个节点，慢指针每次走一个节点
2. 如果存在环，那么快慢指针最终会相遇
3. 如果不存在，则快慢指针最后至少有一个先走到null节点
4. 对于只有单个节点和俩个节点的链表，可以直接判断

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(!head || !head->next) 
            return false;
        ListNode* quick = head;
        ListNode* slow = head;
        while(quick&&quick->next)
        {
            slow = slow->next;   
            quick = quick->next->next;

            if(quick == slow)
                return true;
        }
        return false;
    }
};
```

## [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表

> 输入：head = [3,2,0,-4], pos = 1
> 输出：tail connects to node index 1
> 解释：链表中有一个环，其尾部连接到第二个节点。

![](https://img-blog.csdnimg.cn/20200919191556946.png#pic_center)

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(!head || !head->next)
            return NULL;
        ListNode* slow = head;
        ListNode* quick = head;
        while(quick && quick->next)
        {
            slow = slow->next;
            quick = quick->next->next;
            
            if(slow == quick)
             {
                 slow = head;
                 while(slow != quick)
                 {
                     slow = slow->next;
                     quick = quick->next;
                 }
                 return slow;
             }
        }
        return NULL;
    }
};
```

## [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)
在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序

> 输入: 4->2->1->3
> 输出: 1->2->3->4

思路：
1. 归并的思想，先分割链表，局部排序，排序完后再合并排序
2. 
```cpp
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2)
    {
        ListNode* dummy= new ListNode(0);
        ListNode* head = dummy;
        while(l1 && l2)
        {
            if(l1->val < l2->val)
            {
                dummy->next = l1;
                l1 = l1->next;
            }
            else
            {
                dummy->next = l2;
                l2 = l2->next;
            }
            dummy = dummy->next;
        }
        dummy->next = l1 ? l1 : l2;
        return head->next;
    }

    ListNode* cut(ListNode* l1, int len)
    {
        ListNode* p = l1;
        while(--len && p)
        {
            p = p->next;
        }
        if(!p)
        {
            return NULL;
        }
        ListNode* next = p->next;
        p->next = NULL;
        return next;
    }

    ListNode* sortList(ListNode* head) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;

        int len = 0;
        ListNode* p = head;
        while(p != NULL)
        {
            p = p->next;
            len++;
        }
        for(int i = 1; i < len; i <<= 1)
        {
            ListNode* cur = dummy->next;
            ListNode* tail = dummy;
            while(cur)
            {
                ListNode* left = cur;
                ListNode* right = cut(left, i);
                cur = cut(right, i);
                tail->next = merge(left, right);
                while(tail->next)
                    tail = tail->next;
            }

        }
        return dummy->next;
    }
};
```

## [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
k 是一个正整数，它的值小于或等于链表的长度。
如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

> 示例：
> 给你这个链表：1->2->3->4->5
> 当 k = 2 时，应当返回: 2->1->4->3->5
> 当 k = 3 时，应当返回: 3->2->1->4->5
> 说明：
> 你的算法只能使用常数的额外空间。
> 你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

思路：
递归实现
1. 按k个元素分割链表，局部反转后连上；
2. 确定每次需要翻转的k个元素，递归翻转，对应链表尾端均为NULL；
3. 如果不足k个元素，则不反转，直接返回。
```cpp
class Solution {
public:
    ListNode* reverse(ListNode* head, int k)
    {
        ListNode* pre = NULL;
        while(head && k)
        {
            ListNode* cur = head->next;
            head->next = pre;
            pre = head;
            head = cur;
            k--;
        }
        return pre;
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        if(head == NULL || k == 1)
             return head;
        ListNode* tail = head;
        for(int i = 1; i < k && tail != NULL; ++i)
            tail = tail->next;
        if(tail == NULL)
            return head;
        ListNode *next = tail->next;
        reverse(head,k);
        head->next = reverseKGroup(next, k);
        return tail;
    }
};
```
## [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)
编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919204242538.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzMyMDUw,size_16,color_FFFFFF,t_70#pic_center)
在节点 c1 开始相交
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919204300272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzMyMDUw,size_16,color_FFFFFF,t_70#pic_center)
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* ha = headA;
        ListNode* hb = headB;
        while(ha != hb)
        {
            ha = ha ? ha->next:headB;
            hb = hb ? hb->next:headA;
        }
        return ha;
    }
};
```
## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
反转一个单链表

> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        return reverse(NULL, head);
    }
    ListNode* reverse(ListNode* pre, ListNode* cur)
    {
        if(cur == NULL) return pre;
        ListNode* next = cur->next;
        cur->next = pre;
        return reverse(cur, next);
    }
};

class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = NULL;
        while(head)
        {
            ListNode* next = head->next;
            head->next = pre;
            pre = head;
            head = next;      
        }
        return pre;
    }
};
```
## [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)
请判断一个链表是否为回文链表

> 输入: 1->2
> 输出: false
> 输入: 1->2->2->1
> 输出: true

进阶：
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题

思路：
1. 找到链表中点
2. 翻转后半部分链表，然后依次遍历俩部分链表即可
3. 恢复原链表
```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {

        ListNode* fast = head;
        ListNode* slow = head;
        while(fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }
        ListNode* pre = NULL;
        while(slow)
        {
            ListNode* next = slow->next;
            slow->next = pre;
            pre = slow;
            slow = next;
        }
        ListNode * cur = head;
        ListNode* pt = pre;
        while(cur && pt)
        {
            if(cur->val != pt->val)
                return false;
            cur = cur->next;
            pt = pt->next;
        }
        while(cur)
            cur = cur->next;
        while(pre)
        {
            ListNode* next = pre->next;
            pre->next = cur;
            cur = pre;
            pre = next;
        }        

        // while(head)
        // {
        //     std::cout << head->val << std::endl;
        //     head = head->next;
        // }
        return true;
    }
};
```
