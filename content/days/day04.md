---
title: "代码随想录打卡第4天 | 24. 两两交换链表中的节点、19. 删除链表的倒数第N个节点、面试题02.07. 链表相交、142. 环形链表II"
date: 2026-03-07
tags: [链表, 双指针, 快慢指针]
---

## 24. 两两交换链表中的节点

- 链接：[力扣24](https://leetcode.cn/problems/swap-nodes-in-pairs/)
- 文章：[代码随想录](https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1YT411g7br)
- 状态：✅

### 第一想法
两两交换，头节点也要进入交换，要把这种特殊情况转变为一般情况，所以需要引入dummyHead；
让之后的交换都是某个节点的next和next->next；变成了一般性问题；

还有就是三指针的交换顺序，这里采用标记了最后需要指向的节点降低心智负担；

### 看完题解后的想法
——

### 实现中遇到的困难
dummy.next忘记指向head了；

### 代码

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode dummy(0);
        dummy.next = head;

        ListNode* cur = &dummy;

        while (cur->next != nullptr && cur->next->next != nullptr) {
            ListNode* finalFoward = cur->next->next->next;
            ListNode* firstMove = cur->next->next;
            ListNode* secondMove = cur->next;

            cur->next = firstMove;
            firstMove->next = secondMove;
            secondMove->next = finalFoward;

            cur = cur->next->next;
        }

        return dummy.next;
    }
};
```

---

## 19. 删除链表的倒数第N个节点

- 链接：[力扣19](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)
- 文章：[代码随想录](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1vW4y1U7Gf)
- 状态：⚠️

### 第一想法
第一想法是反转链表，然后从头找过去，然后删除节点再返回来；

但是dummy的处理就会变得复杂；

然后想到了快慢指针；

### 看完题解后的想法
——

### 实现中遇到的困难
是需要让慢指针停在要删除节点的前驱；

### 代码

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode dummy(0);
        dummy.next = head;

        ListNode* slow = &dummy;
        ListNode* fast = &dummy;

        // 快指针先走 n + 1 步，为了让 slow 停在被删节点的前驱
        for (int i = 0; i <= n; i++) {
            fast = fast->next;
        }

        while (fast != nullptr) {
            slow = slow->next;
            fast = fast->next;
        }

        ListNode* tmp = slow->next;
        slow->next = tmp->next;

        delete tmp;
        return dummy.next;
    }
};

```

---

## 面试题02.07. 链表相交

- 链接：[力扣面试题02.07](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)
- 文章：[代码随想录](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html)
- 状态：⚠️

### 第一想法
第一想法是使用set进行判重; 但是如何优化位空间 $O(1)$ 没有头绪；快慢指针判环之后咋做忘了；

### 看完题解后的想法
很巧妙,拆解了路程,从数学B的角度出发,让两个节点跑一次自己的链表再跑一次对面的链表;

$$ A路程：L_A = A+ C $$
$$ B路程：L_B = B + C $$
$$ A + C + B = B + C + A $$

即使 C = 0 也不妨碍两个指针会相遇在nullptr；

### 实现中遇到的困难
——

### 代码

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == nullptr || headB == nullptr) {
            return nullptr;
        }

        ListNode* pa = headA;
        ListNode* pb = headB;

        // 核心 a+c+b = b+c+a (c是可能存在的重合部分)
        while (pa != pb) {
            pa = (pa == nullptr) ? headB : pa->next;
            pb = (pb == nullptr) ? headA : pb->next;
        }

        return pa;
    }
};
```

---

## 142. 环形链表II

- 链接：[力扣142](https://leetcode.cn/problems/linked-list-cycle-ii/)
- 文章：[代码随想录](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1if4y1d7ob)
- 状态：⚠️

### 第一想法
哈希可解，但是如何优化？只记得快慢指针能判断相遇就是有环；

### 看完题解后的想法
#### 1. 龟兔赛跑为什么会相遇
快慢指针的相对速度是`1`， 在一个环内可以理解为慢指针在一个无限长的路劲上领先了快节点`K`步；

每次运动，快指针相对于慢指针都是在`-1`;

所以K步之后就重合了；

#### 2. 相遇之后，怎么找环的入口？

- $x$：从头节点到环入口的距离。

- $y$：从环入口到相遇点的距离。

- $z$：从相遇点继续往前走，回到环入口的距离。

慢指针走过的路径： $S = x + y$

快指针走过的路径： $F = x + y + n(y + z)$

因为快指针速度是慢指针的两倍，所以：
$ 2S = F$

$$2(x + y) = x + y + n(y + z)$$

$$x + y = n(y + z)$$

$$x = (n-1)(y + z) + z$$

这个公式 $x = (n-1) \times \text{环长} + z$ 告诉我们：

如果你从链表头派出一个兵（走 $x$ 步），同时从相遇点派出一个兵（走 $z$ 步，或者多转几圈后再走 $z$ 步），他们最终一定会在环入口相遇！

### 实现中遇到的困难


### 代码

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;

        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;

            if (slow == fast) {
                ListNode* p1 = slow;
                ListNode* p2 = head;
                while (p1 != p2) {
                    p1 = p1->next;
                    p2 = p2->next;
                }
                return p1;
            }
        }

        return nullptr;
    }
};
```

---

## 今日收获

对于链表的快慢指针的运用深化了理解；

复习了Floyd算法；
