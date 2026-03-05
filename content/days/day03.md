---
title: "代码随想录打卡第3天 | 203. 移除链表元素、707. 设计链表、206. 反转链表"
date: 2026-03-06
tags: [链表]
---

## 203. 移除链表元素

- 链接：[力扣203](https://leetcode.cn/problems/remove-linked-list-elements/)
- 文章：[代码随想录](https://programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV18B4y1s7R9)
- 状态：✅

### 第一想法
因为要删除节点，可能头节点本身也是要删除的，因此引入dummy_head，这样之后的所有节点都是next节点也不用区分是不是头节点；

### 看完题解后的想法


### 实现中遇到的困难
实现的时候忽略了next的next仍然可能是要删除的值；

### 代码

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode dummy(INT_MAX);
        dummy.next = head;

        ListNode* cur = &dummy;
        while (cur->next != nullptr) {
            if (cur->next->val == val) {
                cur->next = cur->next->next;
                // 只是删掉了后继的第一个节点，可能有连续需要删除的
            } else {
                cur = cur->next;
                // 只有cur->next不是目标才能走
            }
        }

        return dummy.next;
    }
};
```

---

## 707. 设计链表

- 链接：[力扣707](https://leetcode.cn/problems/design-linked-list/)
- 文章：[代码随想录](https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1FU4y1X7WD)
- 状态：⚠️

### 第一想法
考察的是类的基本书写，需要注意各种代码规范；

因为有频繁的插入和删除，所以需要维护一个dummyHead来保证不用单独处理Head的情况；

因为是类的设计，所以要特别注意内存管理；

### 看完题解后的想法
插入头和插入尾没必要再写一遍，直接用写好了的插入函数；

### 实现中遇到的困难
单独写插入头和插入尾忘记进行size管理；

### 代码

```cpp
class MyLinkedList {
public:
    MyLinkedList() {
        dummyHead_ = new Node(INT_MAX);
        size_ = 0;
    }

    ~MyLinkedList() {
        Node* cur = dummyHead_;
        while (cur != nullptr) {
            Node* tmp = cur;
            cur = cur->next;
            delete tmp;
        }
    }
    
    int get(int index) {
        if (index >= size_) {
            return -1;
        }

        Node* cur = dummyHead_;
        for (int i = 0; i <= index; i++) {
            cur = cur->next;
        }

        return cur->val;
    }
    
    void addAtHead(int val) {
        addAtIndex(0, val);
    }
    
    void addAtTail(int val) {
        addAtIndex(size_, val);
    }
    
    void addAtIndex(int index, int val) {
        if (index > size_) {
            return;
        }

        Node* prev = dummyHead_;
        for (int i = 0; i < index; i++) {
            prev = prev->next;
        }

        Node* newNode = new Node(val);
        newNode->next = prev->next; // 1. 先连后继
        prev->next = newNode;       // 2. 再断前驱并连接新节点

        size_++;
    }
    
    void deleteAtIndex(int index) {
        if (index >= size_) {
            return;
        }

        Node* prev = dummyHead_;
        for (int i = 0; i < index; i++) {
            prev = prev->next;
        }

        Node* tmp = prev->next;
        prev->next = prev->next->next;
        delete tmp;
        size_--;
    }

private:
    struct Node {
        int val;
        Node* next;
        Node(int x) : val(x), next(nullptr) {}
    };

    Node *dummyHead_;
    int size_;
};
```

---

## 206. 反转链表

- 链接：[力扣206](https://leetcode.cn/problems/reverse-linked-list/)
- 文章：[代码随想录](https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1nB4y1i7eL)
- 状态：✅

### 第一想法
反转链表，注意移动的顺序：

首先记下next，不然直接反转会导致找不到next；

然后把cur的next指向前一个节点pre，实现反转；

再把pre移动到cur的位置，再把cur移动到next；

顺序不能错

### 看完题解后的想法
——

### 实现中遇到的困难
——

### 代码

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = nullptr;
        ListNode* cur = head;

        while (cur != nullptr) {
            ListNode* next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }

        return pre;
    }
};

```

---

## 今日收获
复习了链表的基本操作，复习了类的写法
