# Leetcode
This repository inspired me to develop a good habit of writing questions


# 剑指 offer 记录

## 剑指offer 09
## 用栈实现队列
用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead, 分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )<br>
输入输出实例：
```
    输入：
        ["CQueue","appendTail","deleteHead","deleteHead"]
        [[],[3],[],[]]
    输出：[null,null,3,-1]
```
对于数列中各个项对应的是各个方法的使用的输入，第一个列表对于方法名，第二个列表对应放发需要输入的参数，最后是输出值。

 ``` c++
 class CQueue {
public:
    CQueue() {
    }
    
    void appendTail(int value) {
        s1.push(value);
        //s2.push(value);
    }
    
    int deleteHead() {
        int head;
        if(s1.size() <= 0)
            return -1;
        else{
            //将用于存储的栈内顺序进行倒置，用于删除栈底元素
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();   
            }
            //栈底弹出
            head = s2.top();
            s2.pop();
            //将栈进行还原
            while(!s2.empty()){
                s1.push(s2.top());
                s2.pop();
            }
            return head;
        }

    }
private:
    //建立需要进行使用的双栈，实现要求。
    stack<int>  s1;
    stack<int>  s2;
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```

## 剑指offer 30
## 包含min函数的栈
定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。
输入输出实例：
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```
本题目的重点是如何实现对于栈找最小元素的复杂度O(0)的实现。
解题思路：
![solve_way](https://github.com/SEU-PZH/Leetcode/blob/main/img/day1.png)
```c++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> s1;
    stack<int> s2;
    MinStack() {
    }
    
    void push(int x) {
        if(s1.empty()){
            s2.push(x);
        }
        else if(s2.top() >= x){
            //辅助栈的建立，不需严格递增
            s2.push(x);
        }
        s1.push(x);
    }
    
    void pop() {
        int head;
        head = s1.top();
        //保持元素一致性
        if(head == s2.top()){
            s2.pop();
        }
        s1.pop();
    }
    
    int top() {
        return s1.top();
    }
    
    int min() {
        return s2.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```


## 剑指 Offer 06
## 从尾到头打印链表
输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。
```
输入：head = [1,3,2]
输出：[2,3,1]
```
### 方法一：栈，通过栈的的方式，每次弹出栈中的顶层元素，实现反转。
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
 #include <iostream>
 using namespace std;
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        stack<int> s1;
        while(head != NULL){
            s1.push(head->val);
            head = head->next;
        }
        vector<int> v1;
        while(!s1.empty()){
            v1.push_back(s1.top());
            s1.pop();
        }
        return v1;
        
    }
};
```
### 方法二，递归方式，对于每次递归，在归的过程之对于最后的元素放入数组中。
```c++
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        if(!head)
            return {};
        vector<int> a=reversePrint(head->next);
        a.push_back(head->val);
        return a;
    }
};
```

## 剑指 Offer 24
## 反转链表 
定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。
```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```
```c++
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
    ListNode* reverseList(ListNode* head) {
        stack<int> s1;
        ListNode* node = head;
        while(node!=NULL){
            s1.push(node->val);
            node = node->next;
        }
        ListNode* temp = head;
        while(temp!=NULL){
            temp->val = s1.top();
            s1.pop();
            temp = temp->next;
        }
        return head;
        
    }
};
```
### 方法一：迭代
假设链表为1→2→3→∅，我们想要把它改成 3∅←1←2←3。

在遍历链表时，将当前节点的指针改为指向前一个节点。由于节点没有引用其前一个节点，因此必须事先存储其前一个节点。在更改引用之前，还需要存储后一个节点。最后返回新的头引用
```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
};
```
### 方法二：递归<br>
![solve_way](https://github.com/SEU-PZH/Leetcode/blob/main/img/day2.png)
```c++
//newHead指向的永远是反转后的头节点，而head可以看成是针对于每次递归过程中，归的过程中对于链表进行反转过程。
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        ListNode* newHead = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return newHead;
    }
};
```

## 剑指 Offer 35
## 复杂链表的复制
请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

![day2_2](https://user-images.githubusercontent.com/97490701/149089353-238efac1-cc26-41bc-90b3-a8b7deec3c33.png)
### 方法一：回溯 + 哈希表
这道题的难点，是建立一种你深拷贝的方式，对于每一个新的指向节点，都要对于其建立新的空间，方法一的方式是对于原本的节点使用MAP对应节点和新开辟节点之间的对应关系，最后进行相互指针的指引。
```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
//#include <iostream>
//using namespace std;
class Solution {
public:
    Node* copyRandomList(Node* head) {
        map<Node*, Node*> m1;
        Node* temp = head;
        while(head!=NULL){
            if(m1[head] == NULL){
                Node* copyhead = new Node(head->val);
                m1[head] = copyhead;
                head = head->next;
                //copyhead = copyhead->next;
            }
            
        }
        head = temp;
        Node* res = m1[head];
        while(head!=NULL){
            m1[head]->random = m1[head->random];
            m1[head]->next = m1[head->next];
            head = head->next;
        }
        return res;
    }
};
```
### 方法二：迭代 + 节点拆分
注意到方法一需要使用哈希表记录每一个节点对应新节点的创建情况，而我们可以使用一个小技巧来省去哈希表的空间。我们首先将该链表中每一个节点拆分为两个相连的节点，例如对于链表 A→B→C，我们可以将其拆分为A→A→B→B→C→C。对于任意一个原节点 SS，其拷贝节点 S'S′  即为其后继节点。这样，我们可以直接找到每一个拷贝节点 S'S 的随机指针应当指向的节点，即为其原节点 SS 的随机指针指向的节点 TT 的后继节点 T'T。需要注意原节点的随机指针可能为空，我们需要特别判断这种情况。

当我们完成了拷贝节点的随机指针的赋值，我们只需要将这个链表按照原节点与拷贝节点的种类进行拆分即可，只需要遍历一次。同样需要注意最后一个拷贝节点的后继节点为空，我们需要特别判断这种情况

![day2_3](https://user-images.githubusercontent.com/97490701/149090529-717ac349-f040-4d4c-9b70-c9ef3a3b9e4a.png)


```c++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == nullptr) {
            return nullptr;
        }
        for (Node* node = head; node != nullptr; node = node->next->next) {
            Node* nodeNew = new Node(node->val);
            nodeNew->next = node->next;
            node->next = nodeNew;
        }
        for (Node* node = head; node != nullptr; node = node->next->next) {
            Node* nodeNew = node->next;
            nodeNew->random = (node->random != nullptr) ? node->random->next : nullptr;
        }
        Node* headNew = head->next;
        for (Node* node = head; node != nullptr; node = node->next) {
            Node* nodeNew = node->next;
            node->next = node->next->next;
            nodeNew->next = (nodeNew->next != nullptr) ? nodeNew->next->next : nullptr;
        }
        return headNew;
    }
};
```


## 剑指 offer 05
## 替换空格
请实现一个函数，把字符串 s 中的每个空格替换成"%20"。
```
输入：s = "We are happy."
输出："We%20are%20happy."
```
```c++
class Solution {
public:
    string replaceSpace(string s) {
        for(int i=0;i<s.length();i++){
            if(s[i]==' '){
                s.erase(i,1);
                s.insert(i,"%20");
            } 
        }
        return s;
    }
};
```

 
## 剑指 offer 58 
## Ⅱ左旋字符串
字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。
示例 1：
```
输入: s = "abcdefg", k = 2
输出: "cdefgab"
```
示例 2：
```
输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"
```
```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        for(int i=0;i<n;i++){
            s.push_back(s[i]);
        }
        s.erase(0,n);
        return s;
    }
};
```

## 剑指 Offer 03
## 数组中重复的数字
找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```
```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        map<int, int> m1;
        for(int i=0; i<nums.size(); i++){
            m1[nums[i]]++;
            if(m1[nums[i]]>1){
                return nums[i];
            }
        }
        return 0;
    }
};
```

## 剑指 Offer 53  
## I. 在排序数组中查找数字 I
统计一个数字在排序数组中出现的次数。
```
输入: nums = [5,7,7,8,8,10], target = 8
输出: 2
```
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int a = 0;
        for(int i=0;i<nums.size();i++){
            if(nums[i] == target){
                a++;
            }
        }
        return a;
    }
};
```
二分查找
![day3](https://user-images.githubusercontent.com/97490701/149472948-eb27b74f-14bb-43e0-9c07-9c0b97470e2d.png)

```c++
class Solution {
public:
    int binarySearch(vector<int>& nums, int target, bool lower) {
        int left = 0, right = (int)nums.size() - 1, ans = (int)nums.size();
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
                ans = mid;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }

    int search(vector<int>& nums, int target) {
        int leftIdx = binarySearch(nums, target, true);
        int rightIdx = binarySearch(nums, target, false) - 1;
        if (leftIdx <= rightIdx && rightIdx < nums.size() && nums[leftIdx] == target && nums[rightIdx] == target) {
            return rightIdx - leftIdx + 1;
        }
        return 0;
    }
};
```

## 剑指 Offer 53 
## II. 0～n-1中缺失的数字
一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。
```
输入: [0,1,3]
输出: 2
```
二分查找
```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int left =0;
        int right = nums.size();
        int mid;
        while(left<=right){
            mid = (left+right)/2;
            if(nums[mid] != mid){
                right = mid;
            }
            if(nums[mid] == mid){
                left = mid;
            }
            if(right - left <= 1){
                if(nums[left] == left)
                    return left+1;
                else
                    return left;
            }
        }
        return -1;
    }
};
```


## 剑指 Offer 04
## 二维数组中的查找
在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
```
示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。
```
本来想得是通过两个二分查找，相夹找到范围，后来发现后几个实例过不去，发现对于二分查找找上下界，只能够用来确定他的上界，无法确定下界。
```c++
//#include <iostream>
//using namespace std;
class Solution {
public:
    int findcol(vector<vector<int>>& matrix, int target, bool flag) {
            int down, up=0;
            down = matrix.size()-1;//5
            //cout<<up;
            int mid_row;
            while((down-up)>1){
                mid_row = (down+up)/2;
                if(matrix[mid_row][0]>target || (flag == true && matrix[mid_row][0] == target)){
                    down = mid_row;
                }
                else{
                    up = mid_row;
                }
                if(abs(matrix[mid_row][0]-target)<=(matrix[0].size()-1))
                    break;
            }
            if(flag)
                return up;
            else
                return down;
    }

    int findrow(vector<vector<int>>& matrix, int target, bool flag) {
            int down, up=0;
            down = matrix[0].size()-1;
            int mid_row;
            while((down-up)>1){
                mid_row = (down+up)/2;
                if(matrix[0][mid_row]>target || (flag == true && matrix[0][mid_row] == target)){
                    down = mid_row;
                }
                else{
                    up = mid_row;
                }
                if((matrix[0][mid_row]-target)<=(matrix[0].size()-1))
                    break;
            }
            if(flag)
                return down;
            else
                return up;
    }

    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {

        int down_fin,up_fin;
        int left_fin,right_fin;
        if(matrix.size()==0){
            return false;
        }
        up_fin = findcol(matrix, target, true);
        down_fin = findcol(matrix, target, false);
        left_fin = findrow(matrix, target, false);
        right_fin = findrow(matrix, target, true);

        //cout<<down_fin<<up_fin<<left_fin<<right_fin;
        
        //if(matrix[][])

        for(int i=0;i<=down_fin;i++){
            for(int j = 0;j<matrix[0].size(); j++){
                if(matrix[i][j] == target){
                    return true;
                }
            }
        }
        for(int i=0;i<matrix.size();i++){
            for(int j = 0;j<=right_fin; j++){
                if(matrix[i][j] == target){
                    return true;
                }
            }
        }
        return false;
    }
};
```

### 方法一：暴力
```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int rows = matrix.length, columns = matrix[0].length;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                if (matrix[i][j] == target) {
                    return true;
                }
            }
        }
        return false;
    }
}

```
### 方法二：
先找行，再找列。<br>
若 flag > target ，则 target 一定在 flag 所在 行的上方 ，即 flag 所在行可被消去。<br>
若 flag < target ，则 target 一定在 flag 所在 列的右方 ，即 flag 所在列可被消去。<br>
![day4](https://user-images.githubusercontent.com/97490701/149607792-399dcbaa-aaed-4e6d-8b27-76b8af237522.png)

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        int i = matrix.size() - 1, j = 0;
        while(i >= 0 && j < matrix[0].size())
        {
            if(matrix[i][j] > target) i--;
            else if(matrix[i][j] < target) j++;
            else return true;
        }
        return false;
    }
};
```

## 剑指 Offer 11
## 旋转数组的最小数字
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。

给你一个可能存在 重复 元素值的数组 numbers ，它原来是一个升序排列的数组，并按上述情形进行了一次旋转。请返回旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一次旋转，该数组的最小值为1。  

```
输入：[3,4,5,1,2]
输出：1
```
### 方法一：暴力
```c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        if(numbers.size()==1)
            return numbers[0];
        for(int i=1;i<numbers.size();i++){
            if(numbers[i] - numbers[i-1]<0)
                return numbers[i];
        }
        return numbers[0];
    }
};
```
### 方法二：二分查找

![day4_2](https://user-images.githubusercontent.com/97490701/149612609-cdf89b30-280c-43b1-9c5f-42c47d7558dc.png)

```c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int low = 0;
        int high = numbers.size() - 1;
        while (low < high) {
            int pivot = low + (high - low) / 2;
            if (numbers[pivot] < numbers[high]) {
                high = pivot;
            }
            else if (numbers[pivot] > numbers[high]) {
                low = pivot + 1;
            }
            else {
                high -= 1;
            }
        }
        return numbers[low];
    }
};
```

## 剑指 Offer 50
## 第一个只出现一次的字符
在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
```
输入：s = "abaccdeff"
输出：'b'
```
### 方法一：使用哈希表存储频数
```c++
class Solution {
public:
    char firstUniqChar(string s) {
        map<char, int> m1;
        if(s.length() == 0){
            return ' ';
        }
        for(int i=0; i<s.length();i++){
            m1[s[i]]++;
        }
        for(int i=0; i<s.length();i++){
            if(m1[s[i]] == 1)
                return s[i];
        }
        return ' ';
    }
};
```
### 方法二：使用哈希表存储索引
```c++
class Solution {
public:
    char firstUniqChar(string s) {
        unordered_map<char, int> position;
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (position.count(s[i])) {
                position[s[i]] = -1;
            }
            else {
                position[s[i]] = i;
            }
        }
        int first = n;
        for (auto [_, pos]: position) {
            if (pos != -1 && pos < first) {
                first = pos;
            }
        }
        return first == n ? ' ' : s[first];
    }
};
```
### 方法三：队列
思路与算法<br>

我们也可以借助队列找到第一个不重复的字符。队列具有「先进先出」的性质，因此很适合用来找出第一个满足某个条件的元素。<br>

具体地，我们使用与方法二相同的哈希映射，并且使用一个额外的队列，按照顺序存储每一个字符以及它们第一次出现的位置。当我们对字符串进行遍历时，设当前遍历到的字符为 cc，如果 cc 不在哈希映射中，我们就将 cc 与它的索引作为一个二元组放入队尾，否则我们就需要检查队列中的元素是否都满足「只出现一次」的要求，即我们不断地根据哈希映射中存储的值（是否为 -1−1）选择弹出队首的元素，直到队首元素「真的」只出现了一次或者队列为空。<br>

在遍历完成后，如果队列为空，说明没有不重复的字符，返回空格，否则队首的元素即为第一个不重复的字符以及其索引的二元组。<br>

小贴士<br>

在维护队列时，我们使用了「延迟删除」这一技巧。也就是说，即使队列中有一些字符出现了超过一次，但它只要不位于队首，那么就不会对答案造成影响，我们也就可以不用去删除它。只有当它前面的所有字符被移出队列，它成为队首时，我们才需要将它移除。<br>

```c++
class Solution {
public:
    char firstUniqChar(string s) {
        unordered_map<char, int> position;
        queue<pair<char, int>> q;
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (!position.count(s[i])) {
                position[s[i]] = i;
                q.emplace(s[i], i);
            }
            else {
                position[s[i]] = -1;
                while (!q.empty() && position[q.front().first] == -1) {
                    q.pop();
                }
            }
        }
        return q.empty() ? ' ' : q.front().first;
    }
};
```

## 剑指 Offer 32 
## I. 从上到下打印二叉树
```
例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回：

[3,9,20,15,7]
```
算法流程：<br>
特例处理： 当树的根节点为空，则直接返回空列表 [] ；<br>
初始化： 打印结果列表 res = [] ，包含根节点的队列 queue = [root] ；<br>
BFS 循环： 当队列 queue 为空时跳出；<br>
出队： 队首元素出队，记为 node；<br>
打印： 将 node.val 添加至列表 tmp 尾部；<br>
添加子节点： 若 node 的左（右）子节点不为空，则将左（右）子节点加入队列 queue ；<br>
返回值： 返回打印结果列表 res 即可。<br>

```c++
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        vector<int> res;
        if(!root)
            return res;
        queue<TreeNode*> q;
        q.push(root);
        while(q.size()){
            TreeNode* node=q.front();
            q.pop();
            res.push_back(node->val);
            if(node->left)
                q.push(node->left);
            if(node->right)
                q.push(node->right);
        }
        return res;

    }
};
```


## 剑指 Offer 32
## II. 从上到下打印二叉树 II
从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
```
例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]
```

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

 //#include <iostream>
 //using namespace std;
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        queue<int> q2;
        vector<int> temp;
        int height = 0;
        int num = 0;
        int flag;
        if(root==NULL){
            return res;
        }
        q.push(root);
        q2.push(height);
        while(!q.empty()){
            TreeNode* node = q.front();
            //cout<<q2.front();
            if(height != q2.front()){
                res.push_back(temp);    //判断当前节点与上一记录节点是否在同一层
                temp.clear();
            }
            num = q2.front();
            height = num;
            temp.push_back(node->val);
            q.pop();
            q2.pop();//每次我们都存储节点和节点的深度，因为是队列的形式，对于节点的存储也是同层的。
            flag = 1;
            if(node->left){
                q.push(node->left);
                q2.push(++num);
                flag = 0;
            }
            if(node->right){
                q.push(node->right);
                num = num + flag;//避免空节点造成的层数差异。
                q2.push(num);
            }
            flag = 1;
        }
        res.push_back(temp);//最后需要进行一次额外的存储过程。
        return res;
    }
};
```
大神的写法：
```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> q;
        vector<vector<int> > ans;
        if(root==NULL){
            return ans;
        }
        q.push(root);
        while(!q.empty()){
            vector<int> temp;
            for(int i=q.size();i>0;i--){//因为每次存储的次数都是在同一层的内容，每次将节点读出又会将下次层的节点分多次放入进去，便于下次循环存储，队列长度和每层节点数（lens）是对应的
                TreeNode* node = q.front();
                q.pop();
                temp.push_back(node->val);
                if(node->left!=NULL) q.push(node->left);
                if(node->right!=NULL) q.push(node->right);
            }
            ans.push_back(temp);
        }

        return ans;
    }
};
```


## 剑指 Offer 32
## III. 从上到下打印二叉树 III
请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。
```
例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [20,9],
  [15,7]
]

```
只需要根据上一题对于节点的插入顺序采用循环插入的方式进行插入即可。
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> q;
        vector<vector<int> > ans;
        if(root==NULL){
            return ans;
        }
        int flag = 1;
        q.push(root);
        while(!q.empty()){
            vector<int> temp;
            flag = -flag;
            for(int i=q.size();i>0;i--){
                TreeNode* node = q.front();
                q.pop();
                if(flag == 1)
                    temp.insert(temp.begin(),node->val);
                else
                    temp.push_back(node->val);
                if(node->left!=NULL) q.push(node->left);
                if(node->right!=NULL) q.push(node->right);
            }
            ans.push_back(temp);
        }

        return ans;
    }
};
```


## 剑指 Offer 26
## 树的子结构
输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。
```
例如:
给定的树 A:

     3
    / \
   4   5
  / \
 1   2
给定的树 B：

   4 
  /
 1
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。
```
```
示例 1：

输入：A = [1,2,3], B = [3,1]
输出：false
示例 2：

输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```
解题思路：
若树 BB 是树 AA 的子结构，则子结构的根节点可能为树 AA 的任意一个节点。因此，判断树 BB 是否是树 AA 的子结构，需完成以下两步工作：

先序遍历树 AA 中的每个节点 n_An 
A
​
  ；（对应函数 isSubStructure(A, B)）
判断树 AA 中 以 n_An 
A
​
  为根节点的子树 是否包含树 BB 。（对应函数 recur(A, B)）


算法流程：
```
名词规定：树 AA 的根节点记作 节点 AA ，树 BB 的根节点称为 节点 BB 。

recur(A, B) 函数：

终止条件：
当节点 BB 为空：说明树 BB 已匹配完成（越过叶子节点），因此返回 truetrue ；
当节点 AA 为空：说明已经越过树 AA 叶子节点，即匹配失败，返回 falsefalse ；
当节点 AA 和 BB 的值不同：说明匹配失败，返回 falsefalse ；
返回值：
判断 AA 和 BB 的左子节点是否相等，即 recur(A.left, B.left) ；
判断 AA 和 BB 的右子节点是否相等，即 recur(A.right, B.right) ；
isSubStructure(A, B) 函数：

特例处理： 当 树 AA 为空 或 树 BB 为空 时，直接返回 falsefalse ；
返回值： 若树 BB 是树 AA 的子结构，则必满足以下三种情况之一，因此用或 || 连接；
以 节点 AA 为根节点的子树 包含树 BB ，对应 recur(A, B)；
树 BB 是 树 AA 左子树 的子结构，对应 isSubStructure(A.left, B)；
树 BB 是 树 AA 右子树 的子结构，对应 isSubStructure(A.right, B)；
以上 2. 3. 实质上是在对树 AA 做 先序遍历 。
```

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
 class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (A == nullptr || B == nullptr) {
            return false;
        }
        return hasSubStructure(A, B) || isSubStructure(A->left, B) || isSubStructure(A->right, B);
        
    }
    bool hasSubStructure(TreeNode* A, TreeNode* B) {
        if (B == nullptr) {
            return true;
        }
        if (A == nullptr) {
            return false;//在不满足b为nullptr但是A变成了空
        }
        if (A->val != B->val) {
            return false;
        }
        return hasSubStructure(A->left, B->left) && hasSubStructure(A->right, B->right);
    }
};
```


## 剑指 Offer 27 
## 二叉树的镜像

请完成一个函数，输入一个二叉树，该函数输出它的镜像。
```
例如输入：

     4
   /   \
  2     7
 / \   / \
1   3 6   9
镜像输出：

     4
   /   \
  7     2
 / \   / \
9   6 3   1

 

示例 1：

输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

方法一：递归<br>
思路与算法<br>
这是一道很经典的二叉树问题。显然，我们从根节点开始，递归地对树进行遍历，并从叶子节点先开始翻转得到镜像。如果当前遍历到的节点 \textit{root}root 的左右两棵子树都已经翻转得到镜像，那么我们只需要交换两棵子树的位置，即可得到以 \textit{root}root 为根节点的整棵子树的镜像。<br>
```c++
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if (root == nullptr) {
            return nullptr;
        }
        TreeNode* left = mirrorTree(root->left);
        TreeNode* right = mirrorTree(root->right);//避免直接覆盖，需要中间值过度。
        root->left = right;
        root->right = left;
        return root;
    }
};
```

## 剑指 Offer 28
## 对称的二叉树
请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。
```
例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3

 

示例 1：

输入：root = [1,2,2,3,4,4,3]
输出：true
示例 2：

输入：root = [1,2,2,null,3,null,3]
输出：false
```
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == NULL){
            return true;
        }
        else
            return isSy(root->left, root->right);
    }

    bool isSy(TreeNode* root1, TreeNode* root2){
        if(root1==NULL && root2 == NULL)
            return true;
        if(root1==NULL || root2 == NULL)
            return false;


        if(root1->val != root2->val){
            return false;
        }
        else{
            return isSy(root1->left, root2->right)&&isSy(root1->right,root2->left);
        }

    }
};
```


## 剑指 Offer 10
## I. 斐波那契数列
写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项（即 F(N)）。斐波那契数列的定义如下：
```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

示例 1：

输入：n = 2
输出：1
示例 2：

输入：n = 5
输出：5
```

```c++
class Solution {
public:
    int fib(int n) {
        int a[3];
        a[0] = 0;
        a[1] = 1;
        a[2] = 0;
        if(n == 0 || n ==1)
            return (a[n]%1000000007);
        for(int i = 0; i < n-1; i++){
            a[2] = (a[0] + a[1])%1000000007;
            a[0] = a[1];
            a[1] = a[2];
        }
        return (a[2]%1000000007);
    }
};
```
![fit](https://user-images.githubusercontent.com/97490701/152756100-fb27e0a0-f9f0-4f29-8d1c-571cbd4bf5ac.png)
复杂度logn

```c++

class Solution {
public:
    const int MOD = 1000000007;

    int fib(int n) {
        if (n < 2) {
            return n;
        }
        vector<vector<long>> q{{1, 1}, {1, 0}};
        vector<vector<long>> res = pow(q, n - 1);
        return res[0][0];
    }

    vector<vector<long>> pow(vector<vector<long>>& a, int n) {
        vector<vector<long>> ret{{1, 0}, {0, 1}};
        while (n > 0) {
            if (n & 1) {
                ret = multiply(ret, a);
            }
            n >>= 1;
            a = multiply(a, a);
        }
        return ret;
    }

    vector<vector<long>> multiply(vector<vector<long>>& a, vector<vector<long>>& b) {
        vector<vector<long>> c{{0, 0}, {0, 0}};
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                c[i][j] = (a[i][0] * b[0][j] + a[i][1] * b[1][j]) % MOD;
            }
        }
        return c;
    }
};
```


## 剑指 Offer 10
## II. 青蛙跳台阶问题
一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
```
示例 1：

输入：n = 2
输出：2
示例 2：

输入：n = 7
输出：21
示例 3：

输入：n = 0
输出：1
```

```c++
class Solution {
public:
    int numWays(int n) {
        if(n == 0)
            return 1;
        if(n== 1 || n == 2)
            return n;
        int a[n+1];
        a[0] = 1;
        a[1] = 1;
        a[2] = 2;
        for(int i = 3; i<n+1; i++){
            a[i] = (a[i-2] + a[i-1])%1000000007 ;
        }
        return a[n]%1000000007;
    }
};
```


## 剑指 Offer 63
## 股票的最大利润
假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？
```
示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

```


```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buy = 100000;
        int sale = 0;
        int j;
        int pro = 0;
        int k;
        for(int i=0; i<prices.size();i++){
            if(buy>prices[i]){
                buy = prices[i];
                j = i;
            }
            if(sale<prices[i] || j>k){
                sale = prices[i];
                k = i;
            }
            if(pro<sale-buy){
                pro = sale-buy;
            }

        }
        return pro;
    }
};
```
