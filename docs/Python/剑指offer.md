## 题目描述2020/2/11

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

思路：

1. 矩阵是有序的，从左下角看，往右递增，往上递减
2. 从左下角开始查找，比目标大则上移，比目标小则右移
3. 与目标一样大，返回

```python
# -*- coding:utf-8 -*-
class Solution:
    # array 二维列表
    def Find(self, target, array):
        # write code here
        row_size = len(array) - 1
        col_size = len(array[0]) - 1
        row = 0
        col = col_size
        while row <= row_size and col >= 0:
            if array[row][col] == target:
                return True
            elif array[row][col] > target:
                col -= 1
            elif array[row][col] < target:
                row += 1
        return False
```



## 题目描述2020/2/12

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

思路：

1. 先计算出替换后的总长度
2. 从最后面的字符开始不断往后移动，每个字符只需移动一次

```python
# -*- coding:utf-8 -*-
class Solution:
    # s 源字符串
    def replaceSpace(self, s):
        # write code here
        return s.replace(' ', '%20')
```



```c++
class Solution {
public:
	void replaceSpace(char *str,int length) {
        if(str==NULL||length<0) return;
        int oldnum = length;

        int count = 0;
        for(int i=0; i<length; i++){
            if(str[i] == ' ') count++;
        } 
        int newnum = oldnum + 2*count;

        int pold = oldnum - 1;
        for(int i=newnum-1; i>=0; i--){
            if(str[pold] == ' '){
                str[i] = '0';
                str[--i] = '2';
                str[--i] = '%';
                pold--;
            }
            else{
                str[i] = str[pold];
                pold--;
            }
        }
	}
};
```



## 题目描述2020/2/13

输入一个链表，按链表从尾到头的顺序返回一个ArrayList。

思路：

递归

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, listNode):
        # write code here
        if listNode is None:
            return []
        return self.printListFromTailToHead(listNode.next) + [listNode.val]

```



## 题目描述2012/2/14

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

思路：

- 不断拆分前序序列和中序序列，使之成为左右子树的序列
- 递归

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回构造的TreeNode根节点
    def reConstructBinaryTree(self, pre, tin):
        # write code here
        if not pre or not tin:
            return None
        root = pre[0]	//根节点的值
        left = tin[0:tin.index(root)]	# 左子树的中序序列
        right = tin[tin.index(root)+1:]	# 右子树的中序序列
        rootnode = TreeNode(root)	//创建根节点
        # 左子树的前序序列为：当前前序序列减去第一个（即为根节点的值），长度与中序序列一致
        rootnode.left = self.reConstructBinaryTree(pre[1:1+len(left)], left)
        # 右子树的前序序列为：当前前序序列后面数起，长度与中序序列一致
        rootnode.right = self.reConstructBinaryTree(pre[-len(right):], right)
        return rootnode

```



## 题目描述2020/2/16

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

思路：

- stack1实现push，stack2实现pop
- stack2空的时候，将stack1全部元素pop出并push到stack1

```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.stack1 = []
        self.stack2 = []
    def push(self, node):
        # write code here
        self.stack1.append(node)
    def pop(self):
        # return xx
        if self.stack2:
            return self.stack2.pop();
        while self.stack1:
            self.stack2.append(self.stack1.pop())
        return self.stack2.pop()
```

```c++
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        if(!stack2.empty()){
            int temp = stack2.top();
            stack2.pop();
            return temp;
        }

        while(!stack1.empty()){
            stack2.push(stack1.top());
            stack1.pop();
        }
        
         int temp = stack2.top();
         stack2.pop();
         return temp;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```



## 题目描述2019/2/19

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。
例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

![](https://uploadfiles.nowcoder.com/images/20190819/807319133_1566217781994_E0D8DA4D79E73A35EB4365215E2F13BB)

![](https://uploadfiles.nowcoder.com/images/20190822/807319133_1566442034216_88777DA092B0D79C405886B923CA4344)

[参考链接](https://www.nowcoder.com/questionTerminal/9f3231a991af4f55b95579b44b7a01ba?)

```c++
链接：https://www.nowcoder.com/questionTerminal/9f3231a991af4f55b95579b44b7a01ba?answerType=1&f=discussion
来源：牛客网

int minNumberInRotateArray(vector<int> rotateArray) {
        if(rotateArray.empty())
            return 0;
 
        int low = 0;
        int high = rotateArray.size() - 1;
        int mid = 0;
 
        while(low < high){
            // 子数组是非递减的数组，10111
            if (rotateArray[low] < rotateArray[high])
                return rotateArray[low];
            mid = low + (high - low) / 2;
            if(rotateArray[mid] > rotateArray[low])
                low = mid + 1;
            else if(rotateArray[mid] < rotateArray[high])
                high = mid;
            else low++;
        }
        return rotateArray[low];
    }
```

## 题目描述2020/2/21

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。

思路：

使用循环避免栈消耗

```c++
class Solution {
public:
    int Fibonacci(int n) {
        int a = 0;
        int b = 1;
        while(n--){
            b = a + b;
            a = b - a;
        };
        return a;
    };
};
```

## 题目描述2020/2/24

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

思路：

- 本质是Fibonacci数列
  - case1：第一次走的是1级，剩下n-1级
  - case2：第一次走的是2级，剩下n-2级
- case1和case2次数和就是全部情况

```c++
class Solution {
public:
    int jumpFloor(int number) {
        int i = 1, j = 2;
        for(int x = 1; x<number; ++x){
            j = j + i;
            i = j - i;
        }
        return i;
        
    }
};
```



## 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

思路：

https://www.nowcoder.com/questionTerminal/22243d016f6b47f2a6928b4313c85387?f=discussion

f(1) = 1 

  f(2) = f(2-1) + f(2-2)         //f(2-2) 表示2阶一次跳2阶的次数。 

  f(3) = f(3-1) + f(3-2) + f(3-3)  

  ... 

  f(n) = f(n-1) + f(n-2) + f(n-3) + ... + f(n-(n-1)) + f(n-n)  

​    

  说明：  

  1）这里的f(n) 代表的是n个台阶有一次1,2,...n阶的 跳法数。 

  2）n = 1时，只有1种跳法，f(1) = 1 

  3) n = 2时，会有两个跳得方式，一次1阶或者2阶，这回归到了问题（1） ，f(2) = f(2-1) + f(2-2)  

  4) n = 3时，会有三种跳得方式，1阶、2阶、3阶， 

​      那么就是第一次跳出1阶后面剩下：f(3-1);第一次跳出2阶，剩下f(3-2)；第一次3阶，那么剩下f(3-3) 

​      因此结论是f(3) = f(3-1)+f(3-2)+f(3-3) 

  5) n = n时，会有n中跳的方式，1阶、2阶...n阶，得出结论： 

​      f(n) = f(n-1)+f(n-2)+...+f(n-(n-1)) + f(n-n) =>  f(0) + f(1) + f(2) + f(3) + ... + f(n-1) 

​      

  6) 由以上已经是一种结论，但是为了简单，我们可以继续简化： 

​      f(n-1) = f(0) + f(1)+f(2)+f(3) + ... + f((n-1)-1) =  f(0) + f(1) + f(2) + f(3) + ... + f(n-2) 

​      f(n) = f(0) + f(1) + f(2) + f(3) + ... + f(n-2) +  f(n-1) = f(n-1) + f(n-1) 

​      可以得出： 

​      f(n) = 2*f(n-1)

```c++
class Solution {
public:
    int jumpFloorII(int number) {
        int a = 1;
        int i = 1;
        while(i < number){
            a = a * 2;
            ++i;
        }
        return a;
    }
};
```



