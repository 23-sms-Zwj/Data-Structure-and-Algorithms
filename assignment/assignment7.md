# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/



思路：



代码

```python
# 
s=input().split()
s=s[::-1]
print(' '.join(s))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240407202727080](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240407202727080.png)



### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/



思路：



代码

```python
# 
M,N=map(int,input().split())
s=[]
u=0
p=input().split()
for i in range(len(p)):
    if p[i] not in s and len(s)<=M-1:
        s.append(p[i])
        u+=1
    elif p[i] not in s and len(s)==M:
        del s[0]
        s.append(p[i])
        u+=1
print(u)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240407202835560](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240407202835560.png)



### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/



思路：



代码

```python
# 
n,k=map(int,input().split())
lst=list(map(int,input().split()))
lst.sort(key=lambda x:int(x))

if n==1 and k!=0:
    print(lst[0])
elif n==1 and k==0:
    if lst[0]==1:
        print(-1)
    else:
        print(1)

elif n!=1 and  k==0:
    if min(lst)==1:
        print(-1)
    else:
        print(1)
        
else:
    x=lst[k-1]
    if x<lst[k]:
        print(x)
    else:
        print(-1)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240407202900122](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240407202900122.png)



### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/



思路：



代码

```python
# 
class Node():
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def lei(s:str):
    if '1' not in s:
        return 'B'
    elif '0' not in s:
        return 'I'
    return 'F'

def buildTree(string):
    if len(string) == 0:
        return None
    if len(string)==1:
        return Node(lei(string))
    node = Node(lei(string))
    idx=len(string)
    p=idx//2
    node.left = buildTree(string[0:p])
    node.right = buildTree(string[p:])
    return node

def postorder(node):
    if node is None:
        return []
    output = []
    output.extend(postorder(node.left))
    output.extend(postorder(node.right))
    output.append(str(node.val))
    return output
n=input()
s=input()
print(''.join(postorder(buildTree(s))))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240407202948802](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240407202948802.png)



### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/



思路：考试的时候因为数据简单过了，现在发现没那么简单，如果有单独的也放在一个group中，最后遍历groups就行



代码

```python
# 
from collections import deque				

t = int(input())
groups = {}
member_to_group = {}

for _ in range(t):
    members = list(map(int, input().split()))
    group_id = members[0]  
    groups[group_id] = deque()
    for member in members:
        member_to_group[member] = group_id

queue = deque()
queue_set = set()

while True:
    command = input().split()
    if command[0] == 'STOP':
        break
    elif command[0] == 'ENQUEUE':
        x = int(command[1])
        group = member_to_group.get(x, None)
        if group is None:
            group = x
            groups[group] = deque([x])
            member_to_group[x] = group
        else:
            groups[group].append(x)
        if group not in queue_set:
            queue.append(group)
            queue_set.add(group)
    elif command[0] == 'DEQUEUE':
        if queue:
            group = queue[0]
            x = groups[group].popleft()
            print(x)
            if not groups[group]:
                queue.popleft()
                queue_set.remove(group)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240407210110538](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240407210110538.png)



### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/



思路：难点在于如何输出，考试的时候想到了找根节点和建树的方法，但是最后还是没有写出如何输出，参考了代码才想到用字典做



代码

```python
# 
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.children = []

def traverse_print(root, nodes):
    if root.children == []:
        print(root.value)
        return
    pac = {root.value: root}
    for child in root.children:
        pac[child] = nodes[child]
    for value in sorted(pac.keys()):
        if value in root.children:
            traverse_print(pac[value], nodes)
        else:
            print(root.value)

n = int(input())
nodes = {}
children_list = []
for i in range(n):
    info = list(map(int, input().split()))
    nodes[info[0]] = TreeNode(info[0])
    for child_value in info[1:]:
        nodes[info[0]].children.append(child_value)
        children_list.append(child_value)
root = nodes[[value for value in nodes.keys() if value not in children_list][0]]
traverse_print(root, nodes)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240407204254637](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240407204254637.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

考试时间内AC5，但是小组队列写的有问题（所以实际上AC4），现在已经改正确了。本次考试前四题比较简单，最后两题有难度，最后一题的traverse_print没有写出来，比较可惜。考试的时候有点魔怔了，第四题自己写了个mergesort浪费了一些时间，不然可能能做出最后一题。总的来说，本次模拟笔试题目让我找到了笔考的形式，也疏通了一些盲点，机考也反映出我的一些不足，还是要勤加练习，期末目标还是AC6吧。



