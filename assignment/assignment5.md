# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024

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

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/



思路：建树，找到根节点，然后求高度，利用没有子节点为叶节点求出叶子数目（既可以用函数，也可以全局变量计数）



代码

```python
# class TreeNode:
    def __init__(self):
        self.left = None
        self.right = None

def tree_height(node):
    if node is None:
        return -1 
    return max(tree_height(node.left), tree_height(node.right)) + 1

n = int(input())  
nodes = [TreeNode() for _ in range(n)]
has_parent = [False] * n  

global countleaves
countleaves=0
for i in range(n):
    left_index, right_index = map(int, input().split())
    if left_index == -1 and right_index == -1:
        countleaves+=1
    if left_index != -1:
        nodes[i].left = nodes[left_index]
        has_parent[left_index] = True
    if right_index != -1:
        nodes[i].right = nodes[right_index]
        has_parent[right_index] = True

root_index = has_parent.index(False)
root = nodes[root_index]

height = tree_height(root)
leaves = countleaves

print(f"{height} {leaves}")


######################################################


class TreeNode:
    def __init__(self):
        self.left = None
        self.right = None

def tree_height(node):
    if node is None:
        return -1 
    return max(tree_height(node.left), tree_height(node.right)) + 1

def count_leaves(node):
    if node is None:
        return 0
    if node.left is None and node.right is None:
        return 1
    return count_leaves(node.left) + count_leaves(node.right)

n = int(input())  
nodes = [TreeNode() for _ in range(n)]
has_parent = [False] * n  

for i in range(n):
    left_index, right_index = map(int, input().split())
    if left_index != -1:
        nodes[i].left = nodes[left_index]
        has_parent[left_index] = True
    if right_index != -1:
        nodes[i].right = nodes[right_index]
        has_parent[right_index] = True

root_index = has_parent.index(False)
root = nodes[root_index]

height = tree_height(root)
leaves = count_leaves(root)

print(f"{height} {leaves}")


```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240324120453175](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240324120453175.png)

![image-20240324120601053](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240324120601053.png)

### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/



思路：建树，遇到左括号就加子节点，右括号就弹出，然后写函数进行前序遍历和后序遍历



代码

```python
# 
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.children = []

def parse_tree(s):
    stack = []
    node = None
    for char in s:
        if char.isalpha(): 
            node = TreeNode(char)
            if stack: 
                stack[-1].children.append(node)
        elif char == '(': 
            if node:
                stack.append(node) 
                node = None
        elif char == ')':  
            if stack:
                node = stack.pop() 
    return node  


def preorder(node):
    output = [node.value]
    for child in node.children:
        output.extend(preorder(child))
    return ''.join(output)

def postorder(node):
    output = []
    for child in node.children:
        output.extend(postorder(child))
    output.append(node.value)
    return ''.join(output)

s = input().strip()
root = parse_tree(s) 
print(preorder(root)) 
print(postorder(root))  

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240324234604024](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240324234604024.png)



### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/



思路：建树，把子目录和子文件作为子节点



代码

```python
# 
class Node:
    def __init__(self,name):
        self.name=name
        self.dirs=[]
        self.files=[]

def print_(root,m):
    pre='|     '*m
    print(pre+root.name)
    for Dir in root.dirs:
        print_(Dir,m+1)
    for file in sorted(root.files):
        print(pre+file)
        
tests,test=[],[]
while True:
    s=input()
    if s=='#':
        break
    elif s=='*':
        tests.append(test)
        test=[]
    else:
        test.append(s)
for n,test in enumerate(tests,1):
    root=Node('ROOT')
    stack=[root]
    print(f'DATA SET {n}:')
    for i in test:
        if i[0]=='d':
            Dir=Node(i)
            stack[-1].dirs.append(Dir)
            stack.append(Dir)
        elif i[0]=='f':
            stack[-1].files.append(i)
        else:
            stack.pop()
    print_(root,0)
    print()


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240325000402700](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240325000402700.png)



### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/



思路：大写字母看作为运算符，有大写字母就建子节点，左右儿子为小写字母，最后按按层次遍历表达式树的结果前后颠倒就得到队列表达式



代码

```python
# 
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def build_tree(postfix):
    stack = []
    for char in postfix:
        node = TreeNode(char)
        if char.isupper():
            node.right = stack.pop()
            node.left = stack.pop()
        stack.append(node)
    return stack[0]

def level_order_traversal(root):
    queue = [root]
    traversal = []
    while queue:
        node = queue.pop(0)
        traversal.append(node.value)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return traversal

n = int(input().strip())
for _ in range(n):
    postfix = input().strip()
    root = build_tree(postfix)
    queue_expression = level_order_traversal(root)[::-1]
    print(''.join(queue_expression))


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240325003413898](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240325003413898.png)



### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/



思路：利用递归建树，要点在于后序表达式中最后一个是树的root，这样可以分为左树和右树处理



代码

```python
# 
def build_tree(inorder, postorder):
    if not inorder or not postorder:
        return []

    root_val = postorder[-1]
    root_index = inorder.index(root_val)

    left_inorder = inorder[:root_index]
    right_inorder = inorder[root_index + 1:]

    left_postorder = postorder[:len(left_inorder)]
    right_postorder = postorder[len(left_inorder):-1]

    root = [root_val]
    root.extend(build_tree(left_inorder, left_postorder))
    root.extend(build_tree(right_inorder, right_postorder))

    return root

inorder = input().strip()
postorder = input().strip()
preorder = build_tree(inorder, postorder)
print(''.join(preorder))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240325005830813](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240325005830813.png)



### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/



思路：与前一题类似



代码

```python
# 
def build_tree(preorder, inorder):
    if not preorder or not inorder:
        return ''

    root_val = preorder[0]
    root_index = inorder.index(root_val)

    left_inorder = inorder[:root_index]
    right_inorder = inorder[root_index + 1:]

    left_preorder = preorder[1:len(left_inorder)+1]
    right_preorder = preorder[len(left_inorder)+1:]

    root = root_val
    root=build_tree(right_preorder, right_inorder)+root
    root=build_tree(left_preorder, left_inorder)+root

    return root

while True:
    try:
        preorder = input().strip()
        inorder = input().strip()
        postorder = build_tree(preorder, inorder)
        print(''.join(postorder))
    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240325012449854](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240325012449854.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

本次作业较难，是关于树的一些通用问题，如深度、叶节点、前中后序遍历等等，涉及的解法比较通用，尤其最后两题，有明显的相似点，这也体现出数算的精髓在于复用。而且本次作业明显有别与其他次作业，做本次作业过程中感觉融汇贯通了很多以前的知识，比如括号匹配等等。解决树问题的方法我目前掌握的就是情景还原和递归，还需要不断深入。本次学习中通过看GitHub上的题解学到了不少内置函数，也学习了一道题的多种解法。每天看看群里大佬的讨论，感觉需要学习的东西还很多，加油加油！



