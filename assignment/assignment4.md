# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/



思路：1、鉴于oj测试量很小，可以直接用list来构建双端队列

2、class 双端队列进行处理



代码

```python
# 
t=int(input())
for i in range(t):
        s=[]
        n=int(input())
        for k in range(n):
                ty,zhi=input().split()
                if ty=='1':
                        s.append(zhi)
                if ty=='2':
                        if zhi=='0':
                                del s[0]
                        elif zhi=='1':
                                s.pop()
        if s:
                print(' '.join(s))
        else:
                print('NULL')





class Node:
    def __init__(self, value=None):
        self.value = value
        self.next = None
        self.prev = None

class MyDeque:
    def __init__(self):
        self.head = None
        self.tail = None

    def isEmpty(self):
        return self.head is None

    def append(self, value):
        new_node = Node(value)
        if self.isEmpty():
            self.head = self.tail = new_node
        else:
            self.tail.next = new_node
            new_node.prev = self.tail
            self.tail = new_node

    def appendleft(self, value):
        new_node = Node(value)
        if self.isEmpty():
            self.head = self.tail = new_node
        else:
            new_node.next = self.head
            self.head.prev = new_node
            self.head = new_node

    def pop(self):
        if self.isEmpty():
            return None
        ret_value = self.tail.value
        if self.head == self.tail:
            self.head = self.tail = None
        else:
            self.tail = self.tail.prev
            self.tail.next = None
        return ret_value

    def popleft(self):
        if self.isEmpty():
            return None
        ret_value = self.head.value
        if self.head == self.tail:
            self.head = self.tail = None
        else:
            self.head = self.head.next
            self.head.prev = None
        return ret_value

    def printDeque(self):
        elements = []
        current = self.head
        while current:
            elements.append(current.value)
            current = current.next
        return elements

t = int(input()) 
for _ in range(t):
    n = int(input())  
    my_deque = MyDeque()
    for _ in range(n):
        parts = list(map(int, input().split()))
        if parts[0] == 1: 
            my_deque.append(parts[1])
        elif parts[0] == 2:
            if parts[1] == 0:
                my_deque.popleft()
            else:
                my_deque.pop()
    if my_deque.isEmpty():
        print("NULL")
    else:
        print(' '.join(map(str, my_deque.printDeque())))

        

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240317212944288](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240317212944288.png)

![image-20240317213004239](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240317213004239.png)

### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/



思路：将波兰表达式逆序后用处理逆波兰表达式的方法进行操作



代码

```python
# 
s=input().split()
s=s[::-1]
stack=[]
for i in range(len(s)):
        if s[i] in '+-*/':
                a,b=stack.pop(),stack.pop()
                stack.append(str(eval(a+s[i]+b)))
        else:
                stack.append(s[i])
v=float(stack[0])
print(f'{v:.6f}')
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240317213304738](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240317213304738.png)



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/



思路：先用字典存储运算符的优先级，然后用stack存运算符和括号，用number存表达式中每一个进入的数字。遍历表达式，如果是数字就直接加到后续表达式中；如果不是数但是number非空就先把number加到后续表达式中；再看是否为运算符，如果是并且stack中运算符优先级更高就把这个运算符加入到后续表达式中，再把这个运算符存到stack中；如果是左括号就加到stack中；如果是右括号，就把stack中非左括号的运算符全加到后序表达式中，直到遇到左括号就删去左括号；最后如果number非空就加到后序表达式中，stack非空也加到后序表达式中就完成了。



代码

```python
# 
def infix_to_postfix(expression):
    precedence={'+':1,'-':1,'*':2,'/':2}
    stack=[]
    postfix=[]
    number=''

    for char in expression:
        if char.isnumeric() or char == '.':
            number += char
        else:
            if number:
                num=float(number)
                postfix.append(int(num) if num.is_integer() else num)
                number=''
            if char in '+-*/':
                while stack and stack[-1] in '+-*/' and precedence[char]<=precedence[stack[-1]]:
                    postfix.append(stack.pop())
                stack.append(char)
            elif char =='(':
                stack.append(char)
            elif char==')':
                while stack and stack[-1]!='(':
                    postfix.append(stack.pop())
                stack.pop()

    if number:
        num=float(number)
        postfix.append(int(num) if num.is_integer() else num)

    while stack:
        postfix.append(stack.pop())

    return ' '.join(str(character) for character in postfix)

n=int(input())
for _ in range(n):
    expression = input()
    print(infix_to_postfix(expression))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240317225049355](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240317225049355.png)



### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/



思路：



代码

```python
# 
def is_valid_pop_sequence(origin, output):
    if len(origin) != len(output):
        return False 
    stack = []
    renew_origin= list(origin)    
    for char in output:
        while (not stack or stack[-1] != char) and renew_origin:
            stack.append(renew_origin.pop(0))
        if not stack or stack[-1] != char:
            return False      
        stack.pop() 
    return True

origin = input().strip()
while True:
    try:
        output = input().strip()
        if is_valid_pop_sequence(origin, output):
            print('YES')
        else:
            print('NO')
    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240317234449682](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240317234449682.png)



### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/



思路：分类左树和右树，最后取最高的一边



代码

```python
# 
class TreeNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

def get_height(root):
    if root is None:
        return 0
    lh = get_height(root.left)
    rh = get_height(root.right)
    return max(lh, rh) + 1

if __name__ == "__main__":
    T = [TreeNode(0) for _ in range(20)]
    n = int(input())
    for i in range(1, n + 1):
        templ, tempr = map(int, input().split())
        if templ != -1:
            T[i].left = T[templ]
        if tempr != -1:
            T[i].right = T[tempr]
    print(get_height(T[1]))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240317213459534](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240317213459534.png)



### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/



思路：利用mergesort计算出逆序数



代码

```python
# 
def merge_sort(lst):
    if len(lst) <= 1:
        return lst, 0
    middle = len(lst) // 2
    left, inv_left = merge_sort(lst[:middle])
    right, inv_right = merge_sort(lst[middle:])
    merged, inv_merge = merge(left, right)
    return merged, inv_left + inv_right + inv_merge

def merge(left, right):
    merged = []
    inv_count = 0
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            merged.append(left[i])
            i += 1
        else:
            merged.append(right[j])
            j += 1
            inv_count += len(left) - i 
    merged += left[i:]
    merged += right[j:]
    return merged, inv_count

while True:
    n = int(input())
    if n == 0:
        break

    lst = []
    for _ in range(n):
        lst.append(int(input()))

    _, inversions = merge_sort(lst)
    print(inversions)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240318002300084](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240318002300084.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

本周作业难度明显增加，每道题都要认真考虑才能做出来。我觉得在做题之前应该把闫老师的课件先全部看懂，这样不仅对做题有所启发，也可以知道如何写出一个规范、可读性高的代码。闫老师的课件十分全面，读完后对与树这个新概念有一个全面的了解。（解析树等代码我觉得完全可以直接作为一个cheatingsheet）本周因为其他科任务较重没有做额外的练习，spring选做也落下了一点，但是会慢慢跟上的。



