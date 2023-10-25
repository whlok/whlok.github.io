---
title: Leetcode算法之设计
date: 2023-10-25 18:46:31
categories: 数据结构与算法
tags: Leetcode
---

## [707. 设计链表](https://leetcode-cn.com/problems/design-linked-list/)
设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：val 和 next。val 是当前节点的值，next 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 prev 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：

get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点

> MyLinkedList linkedList = new MyLinkedList();
> linkedList.addAtHead(1);
> linkedList.addAtTail(3);
> linkedList.addAtIndex(1,2);   //链表变为1-> 2-> 3
> linkedList.get(1);            //返回2
> linkedList.deleteAtIndex(1);  //现在链表是1-> 3
> linkedList.get(1);            //返回3

## [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)
运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

进阶:

你是否可以在 O(1) 时间复杂度内完成这两种操作？

> LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );
> cache.put(1, 1);
> cache.put(2, 2);
> cache.get(1);       // 返回  1
> cache.put(3, 3);    // 该操作会使得关键字 2 作废
> cache.get(2);       // 返回 -1 (未找到)
> cache.put(4, 4);    // 该操作会使得关键字 1 作废
> cache.get(1);       // 返回 -1 (未找到)
> cache.get(3);       // 返回  3
> cache.get(4);       // 返回  4

思路：
1. 采用链表+哈希表实现，利用哈希查找的时间复杂度为O(1),链表插入删除元素的时间复杂度为O(1),这里使用双向链表，如果使用单链表的话，删除元素需要保存其前向节点，所以直接使用双向链表；
2. ltcached的每一个节点是（key,val）对，mp的关键字是key，值为key对应的链表节点的迭代器；
3. 根据LRU规则，判断是否存在key,存在则返回对应的val（由于mp[key]存放的时链表节点的迭代器，所以val = mp[key]->second），删除该元素，并将元素重新插入链表头，若超出容量指责删除链表尾部元素；不存在则返回-1；
4. 插入元素时，若存在，更新值即可，并删除该元素，插入链表头部；若不存在，在容量达到最大值的情况下，删除链表尾部元素；每次更新时，同样需要更新mp中的值。

```cpp
class LRUCache {
public:
    LRUCache(int capacity) 
        :cap(capacity){}

    int get(int key) 
    {
        if(map.count(key)!=0)
        {
            int val = map[key]->second;
            list_cache.erase(map[key]);
            list_cache.push_front(make_pair(key,val));
            map[key]=list_cache.begin();
            return val;
        }
        return -1;
    }
    
    void put(int key, int value) {
        if(get(key) != -1)
        {
            list_cache.front() = make_pair(key,value);
            return;
        }
        if(list_cache.size() == cap)
        {
            auto p = list_cache.back();
            map.erase(p.first);
            list_cache.pop_back();
        }
        list_cache.push_front(make_pair(key,value));
        map[key]=list_cache.begin();
    }
private:
    int cap;
    list<pair<int,int>> list_cache;
    unordered_map<int,list<pair<int,int>>::iterator> map;
};

struct DlinkNode {
    int key, value;
    DlinkNode* prev;
    DlinkNode* next;
    DlinkNode(): key(0), value(0), prev(nullptr), next(nullptr) {}
    DlinkNode(int _key, int _value): key(_key), value(_value), prev(nullptr), next(nullptr) {}
};

class LRUCache {
private:
    unordered_map<int, DlinkNode*> cache;
    DlinkNode* head;
    DlinkNode* tail;
    int size;
    int capacity;

public:
    LRUCache(int _capacity): capacity(_capacity), size(0) {
        // 使用伪头部和伪尾部节点
        head = new DlinkNode();
        tail = new DlinkNode();
        head->next = tail;
        tail->prev = head;
    }
    
    int get(int key) {
        if (!cache.count(key)) {
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        DlinkNode* node = cache[key];
        moveToHead(node);
        return node->value;
    }
    
    void put(int key, int value) {
        if (!cache.count(key)) {
            // 如果 key 不存在，创建一个新的节点
            DlinkNode* node = new DlinkNode(key, value);
            // 添加进哈希表
            cache[key] = node;
            // 添加至双向链表的头部
            addToHead(node);
            ++size;
            if (size > capacity) {
                // 如果超出容量，删除双向链表的尾部节点
                DlinkNode* removed = removeTail();
                // 删除哈希表中对应的项
                cache.erase(removed->key);
                // 防止内存泄漏
                delete removed;
                --size;
            }
        }
        else 
        {
            // 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            DlinkNode* node = cache[key];
            node->value = value;
            moveToHead(node);
        }
    }

    void addToHead(DlinkNode* node)
    {
        node->prev = head;
        node->next = head->next;
        head->next->prev = node;
        head->next = node;
    }

    void removeNode(DlinkNode* node)
    {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void moveToHead(DlinkNode* node)
    {
        removeNode(node);
        addToHead(node);
    }

    DlinkNode* removeTail()
    {
        DlinkNode* node = tail->prev;
        removeNode(node);
        return node;
    }
};
```
