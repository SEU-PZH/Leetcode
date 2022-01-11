# Leetcode
This repository inspired me to develop a good habit of writing questions


# 剑指 offer 记录

## 剑指offer 09
## 232. 用栈实现队列
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
            head = s2.top();
            s2.pop();
<!--        //将栈进行还原
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

#
