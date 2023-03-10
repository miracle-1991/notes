#LeetCode题解-链表篇

## 160 相交链表
编写一个程序，找到两个单链表相交的起始节点。
例如，下面的两个链表：
A: a1 → a2
↘
c1 → c2 → c3
↗
B: b1 → b2 → b3

在节点 c1 开始相交。
注意：
如果两个链表没有交点，返回 null。
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。

### 解法
使用双指针方法，a + c + b = b + c + a， 
使用指针1从a的头部开始遍历，当到达结尾时，从b的头部再开始，
指针2同理。如果存在相交的点，那么在两个指针分别走完a + c + b 和 b + c + a的长度后，就会交汇于此点。

```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* l1 = headA;
        ListNode* l2 = headB;
        while (l1 != l2) {
            l1 = l1 == nullptr ? headB : l1->next;
            l2 = l2 == nullptr ? headA : l2->next;
        }
        return l1;
    }
};
```

## 206 链表反转
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

### 解法1：迭代
在链表之前生成一个空节点，作为新链表的尾部，然后遍历这个链表，被遍历到的节点从原链表上摘链，然后加入到新链表上。因此使用双指针方法，指针1指向当前被遍历到的节点，指针2指向前一个节点，将当前节点的next指向前一个节点，⚠️记得保存next，遍历要用
```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* cur = head;
        while (cur != nullptr) {
            ListNode* next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
};
```

### 解法2: 递归
⚠️所有的递归都要要注意基线条件，也就是推出递归的条件，不能无限递归下去。
首先，生成基线条件，在递归到null或者尾部的那个节点时，直接返回当前递归到的节点
其次，递归的递归条件中，要分成2部分：
* 部分1: 除了当前节点，该节点之后的所有节点递归的结果，这应该是已经反转后的结果
* 部分2: 当前节点，该节点应该放到反转后链表的尾部，然后返回新链表的头部,⚠️要提前记录好新链表的尾部，方便放置当前节点
```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (head == nullptr || head->next == nullptr) { return head; }
        ListNode* newListEnd = head->next;
        ListNode* newList = reverseList(head->next);
        ListNode* curNode = head;
        newListEnd->next = curNode;
        curNode->next = nullptr;
        return newList;
    }
};
```

## 21 合并两个有序链表
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
### 解法
使用递归求解：
首先设置基线条件，当两个链表中有一个为空时，直接返回另外一个链表  
然后设置递归条件：
* 如果链表1的头部小于链表2的头部，那么链表1的头部作为新链表的头部，该头部的next应该是链表1除了头部的其他部分与链表2合并后的结果
* 反之亦然
```
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if (list1 == nullptr) { return list2; }
        if (list2 == nullptr) { return list1; }
        ListNode* newHead = nullptr;
        if (list1->val < list2->val) {
            newHead = list1;
            newHead->next = mergeTwoLists(list1->next, list2);
        }else {
            newHead = list2;
            newHead->next = mergeTwoLists(list1, list2->next);
        }
        return newHead;
    }
};
```

## 83 删除排序链表中的重复元素
给定一个已排序的链表的头 head ， 删除所有重复的元素，使每个元素只出现一次 。返回 已排序的链表 。
### 解法
递归
* 首先设置基线条件, 当前递归到的节点是空或者next为空时，直接返回当前节点。  
* 然后设置递归条件:
    * 部分1是对当前节点的next处理后的结果，是已经去除过相同元素的结果
    * 部分2中判断当前节点与next的关系，如果值重复，则应该跳过当前节点，next作为新的头部返回，否则不跳过
```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (head == nullptr || head->next == nullptr) { return head; }
        head->next = deleteDuplicates(head->next);
        return head->val == head->next->val ? head->next : head;
    }
};
```

## 19. 删除链表的倒数第 N 个结点
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。
### 解法
很明显的双指针解法：  
链表 1->2->3,删除倒数第2个节点,也就是'2'
* 指针1先走2步，然后快慢指针同时向后遍历，如果指针1已经是末尾节点，那么指针2就是待删除节点的前一个节点
* 将指针1的next摘链

⚠️如果指针1走了n步后脱离了链条，说明要删除的就是头节点
```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* fast = head;
        ListNode* slow = head;
        int step = 0;
        while (step < n && fast != nullptr) {
            fast = fast->next;
            step++;
        }
        if (fast == nullptr) { return head->next; }
        while (fast->next != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next;
        return head;
    }
};
```

## 24. 两两交换链表中的节点
给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。
### 解法
双指针解法：
*指针1位于第一个节点之前，指向第一个节点
*指针2位于当前节点
*调换当前节点和下一个节点
```
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == nullptr || head->next == nullptr) { return head; }
        ListNode* p1 = new ListNode(-1, head);
        ListNode* p2 = head;
        ListNode* newHead = nullptr;
        while (p2 != nullptr && p2->next != nullptr) {
            ListNode* next = p2->next->next;
            p1->next = p2->next;
            p1->next->next = p2;
            p2->next = next;
            if (newHead == nullptr) { newHead = p1->next; }
            p1 = p2;
            p2 = next;
        }
        return newHead;
    }
};
```

## 445. 两数相加 II
给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
你可以假设除了数字 0 之外，这两个数字都不会以零开头。
### 解法
递归解法:
* 由于两个链表长度不一样，而相加都是从个位开始算，所以必须先把两个链表反转，分别为l1和l2
* 设置基线条件：当其中一个为null，返回另外一个与进位标志的和；两个都是null，返回0与进位标志的和
* 设置递归条件：当前节点的值是l1和l2的和，如果和大于10，设置进位标志，值-=10，当前节点的下一个节点是递归的结果
* 反转结果

```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* rl1 = reverse(l1);
        ListNode* rl2 = reverse(l2);
        ListNode* head = add(rl1, rl2, 0);
        return reverse(head);
    }

    ListNode* reverse(ListNode* head) {
        if (head == nullptr || head->next == nullptr) { return head; }
        ListNode* next = head->next;
        ListNode* newHead = reverse(head->next);
        next->next = head;
        head->next = nullptr;
        return newHead;
    }

    ListNode* add(ListNode* l1, ListNode* l2, int add_flag) {
        if (l1 == nullptr && l2 == nullptr) {
            if (add_flag == 1) {
                return new ListNode(1, nullptr);
            } else {
                return nullptr;
            }
        }
        int left  = l1 == nullptr ? 0 : l1->val;
        int right = l2 == nullptr ? 0 : l2->val;
        int sum = left + right + add_flag;
        add_flag = sum >= 10 ? 1 : 0;
        sum = sum >= 10 ? sum - 10 : sum;
        ListNode* node = new ListNode(sum);
        ListNode* l1next = l1 == nullptr ? l1 : l1->next;
        ListNode* l2next = l2 == nullptr ? l2 : l2->next;
        node->next = add(l1next, l2next, add_flag);
        return node;
    }
};
```
用栈求解和用反转求解思路是类似的，都是将个位数、十位数、百位数。。。依次放到最容易获得的地方。

## 234. 回文链表
给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。
### 解法
使用双指针解法：
* 首先用快慢指针，快指针遍历的速度是慢指针的2倍，快指针到达末尾时，慢指针到达中间，⚠️快指针要先走一步
* 然后利用中间节点，将一个链表切成两半，其中后半部分的链表需要反转
* 遍历两个链表，判断每个节点的值是否一致
```
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (head == nullptr || head->next == nullptr) { return true; }
        ListNode* fast = head->next;
        ListNode* slow = head;
        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
        }
        //1->2->3->4->5 slow:3 fast: nullptr
        //1->2->3->4 slow:2 fast: 4
        ListNode* l1 = head;
        ListNode* l2 = cut(slow);
        return diff(l1, l2);
    }

    ListNode* reverse(ListNode* head) {
        if (head == nullptr || head->next == nullptr) { return head; }
        ListNode* nextBackUp = head->next;
        ListNode* newHead = reverse(head->next);
        nextBackUp->next = head;
        head->next = nullptr;
        return newHead;
    }
    ListNode* cut(ListNode* mid) {
        ListNode* head = mid->next;
        mid->next = nullptr;
        return reverse(head);
    }
    bool diff(ListNode* l1, ListNode* l2) {
        while (l1 != nullptr && l2 != nullptr) {
            if (l1->val != l2->val) { return false; }
            l1 = l1->next;
            l2 = l2->next;
        }
        return true;
    }
};
```

## 725. 分隔链表
给你一个头结点为 head 的单链表和一个整数 k ，请你设计一个算法将链表分隔为 k 个连续的部分。
每部分的长度应该尽可能的相等：任意两部分的长度差距不能超过 1 。这可能会导致有些部分为 null 。
这 k 个部分应该按照在链表中出现的顺序排列，并且排在前面的部分的长度应该大于或等于排在后面的长度。
返回一个由上述 k 部分组成的数组。
### 解法
没有什么特别的技巧，也不需要特别的技巧，纯考验编程能力：
* 首先遍历一遍链表，知道总长度，分成k个部分后，每个部分的length是多少，余出来多少，余出来的这些值要均分到前几个节点中，这样就构成了一个由长度构成的数组
* 遍历该数组，每次输入该部分开始节点的指针和要拿到的长度，得到下一个部分的开始节点，循环往复
    * 得到开始节点和长度length后，向后走length-1步，断链，得到该部分，然后返回下一个部分的起始，比如：总体链表是1->2->3->4->5,开始节点是1，length是2，得到1->2，返回3
```
class Solution {
public:
    vector<ListNode*> splitListToParts(ListNode* head, int k) {
        int tLen = 0;
        ListNode* l = head;
        while (l != nullptr) {
            tLen++;
            l = l->next;
        }
        int pLen = tLen/k;
        int pMore = tLen%k;
        vector<int> lenArr = vector<int>(k, pLen);
        for (int i = 0; i < pMore; i++) {
            lenArr[i] += 1;
        }
        vector<ListNode*> ret;
        ListNode* pHead = head;
        for (int i = 0; i < k; i++) {
            ret.push_back(pHead);
            pHead = sub(pHead, lenArr[i]);
        }
        return ret;
    }

    ListNode* sub(ListNode* head, int len) {
        if (head == nullptr) { return head; }
        ListNode* l = head;
        for (int i = 0; i < len - 1; i++) {
            l = l->next;
        }
        ListNode* newPartHead = l->next;
        l->next = nullptr;
        return newPartHead;
    }
};
```

## 328. 奇偶链表
给定单链表的头节点 head ，将所有索引为奇数的节点和索引为偶数的节点分别组合在一起，然后返回重新排序的列表。
第一个节点的索引被认为是 奇数 ， 第二个节点的索引为 偶数 ，以此类推。
请注意，偶数组和奇数组内部的相对顺序应该与输入时保持一致。
你必须在 O(1) 的额外空间复杂度和 O(n) 的时间复杂度下解决这个问题。
### 解法
应该有很多种解法，此处用双指针：
* 生成odd指针用于拼接奇数索引的节点，even指针用于拼接偶数索引的节点
* 遍历原始链表，odd->next->next，成为odd的next，even->next->next成为even的next
* ⚠️要记录最开始的even的节点，用于拼接到odd链表的末尾
```
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* evenHead = even;
        while (even != nullptr && even->next != nullptr) {
            odd->next = odd->next->next;
            even->next = even->next->next;
            odd = odd->next;
            even = even->next;
        }
        odd->next = evenHead;
        return head;
    }
};
```
