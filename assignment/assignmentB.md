# Assignment #B: 图论和树算

Updated 1709 GMT+8 Apr 28, 2024

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

### 28170: 算鹰

dfs, http://cs101.openjudge.cn/practice/28170/



思路：dfs把一个点的“鹰”全去除，直至没有点



代码

```python
# 
def dfs(x,y):
    graph[x][y] = "-"
    for dx,dy in [(1,0),(-1,0),(0,1),(0,-1)]:
        if 0<=x+dx<10 and 0<=y+dy<10 and graph[x+dx][y+dy] == ".":
            dfs(x+dx,y+dy)
graph = []
result = 0
for i in range(10):
    graph.append(list(input()))
for i in range(10):
    for j in range(10):
        if graph[i][j] == ".":
            result += 1
            dfs(i,j)
print(result)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240507223531223](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240507223531223.png)



### 02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/



思路：



代码

```python
# 
def solve_n_queens(n):
    def is_valid(board, row, col):
        for i in range(row):
            if board[i] == col or abs(board[i] - col) == abs(i - row):
                return False
        return True

    def backtrack(row):
        if row == n:
            result.append([col+1 for col in board])
            return
        for col in range(n):
            if is_valid(board, row, col):
                board[row] = col 
                backtrack(row + 1) 
                board[row] = -1 

    result = []
    board = [-1] * n
    backtrack(0)
    return result


n = 8
queens = solve_n_queens(n)
k=int(input())
for i in range(k):
        t=int(input())
        print(''.join(map(str,(queens[t-1]))))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240508001600157](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240508001600157.png)



### 03151: Pots

bfs, http://cs101.openjudge.cn/practice/03151/



思路：bfs，每次6种操作，如果无法产生新状态而且不能到c则无解



代码

```python
# 
def bfs(A, B, C):
    start = (0, 0)
    visited = set()
    visited.add(start)
    queue = [(start, [])]

    while queue:
        (a, b), actions = queue.pop(0)

        if a == C or b == C:
            return actions

        next_states = [(A, b), (a, B), (0, b), (a, 0), (min(a + b, A),max(0, a + b - A)), (max(0, a + b - B), min(a + b, B))]

        for i in next_states:
            if i not in visited:
                visited.add(i)
                new_actions = actions + [get_action(a, b, i)]
                queue.append((i, new_actions))

    return ["impossible"]


def get_action(a, b, next_state):
    if next_state == (A, b):
        return "FILL(1)"
    elif next_state == (a, B):
        return "FILL(2)"
    elif next_state == (0, b):
        return "DROP(1)"
    elif next_state == (a, 0):
        return "DROP(2)"
    elif next_state == (min(a + b, A), max(0, a + b - A)):
        return "POUR(2,1)"
    else:
        return "POUR(1,2)"


A, B, C = map(int, input().split())
solution = bfs(A, B, C)

if solution == ["impossible"]:
    print(solution[0])
else:
    print(len(solution))
    for i in solution:
        print(i)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240507232309554](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240507232309554.png)



### 05907: 二叉树的操作

http://cs101.openjudge.cn/practice/05907/



思路：完全用树来解决



代码

```python
# 
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.parent = None

class BinaryTree:
    def __init__(self, n):
        self.root = TreeNode(0)
        self.node_dict = {0: self.root}
        self.build_tree(n)

    def build_tree(self, n):
        for _ in range(n):
            idx, left, right = map(int, input().split())
            if idx not in self.node_dict:
                self.node_dict[idx] = TreeNode(idx)
            node = self.node_dict[idx]
            if left != -1:
                if left not in self.node_dict:
                    self.node_dict[left] = TreeNode(left)
                left_node = self.node_dict[left]
                node.left = left_node
                left_node.parent = node
            if right != -1:
                if right not in self.node_dict:
                    self.node_dict[right] = TreeNode(right)
                right_node = self.node_dict[right]
                node.right = right_node
                right_node.parent = node

    def swap_nodes(self, x, y):
        node_x = self.node_dict[x]
        node_y = self.node_dict[y]
        px, py = node_x.parent, node_y.parent

        if px == py:
            px.left, px.right = px.right, px.left
            return
        
        if px.left == node_x:
            px.left = node_y
        else:
            px.right = node_y

        if py.left == node_y:
            py.left = node_x
        else:
            py.right = node_x

        node_x.parent, node_y.parent = py, px

    def find_leftmost_child(self, x):
        node = self.node_dict[x]
        while node.left:
            node = node.left
        return node.val


t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    tree = BinaryTree(n)
    for _ in range(m):
        op, *args = map(int, input().split())
        if op == 1:
            x, y = args
            tree.swap_nodes(x, y)
        elif op == 2:
            x, = args
            print(tree.find_leftmost_child(x))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240507225221985](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240507225221985.png)





### 18250: 冰阔落 I

Disjoint set, http://cs101.openjudge.cn/practice/18250/



思路：这道题不难。找父节点，如果不是一个父节点就合并，如果是就输出yes，最后用集合找出有可乐的杯子



代码

```python
#
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    root_x = find(x)
    root_y = find(y)
    if root_x != root_y:
        parent[root_y] = root_x

while True:
    try:
        n, m = map(int, input().split())
        parent = list(range(n + 1))

        for _ in range(m):
            a, b = map(int, input().split())
            if find(a) == find(b):
                print('Yes')
            else:
                print('No')
                union(a, b)

        unique_parents = set(find(x) for x in range(1, n + 1))  
        ans = sorted(unique_parents)
        print(len(ans))
        print(*ans)

    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240507231422920](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240507231422920.png)



### 05443: 兔子与樱花

http://cs101.openjudge.cn/practice/05443/



思路：类似于上次的兔子那道题，思路还是用堆



代码

```python
# 
import heapq
import math
def dijkstra(graph,start,end,P):
    if start == end: return []
    dist = {i:(math.inf,[]) for i in graph}
    dist[start] = (0,[start])
    pos = []
    heapq.heappush(pos,(0,start,[]))
    while pos:
        dist1,current,path = heapq.heappop(pos)
        for (next,dist2) in graph[current].items():
            if dist2+dist1 < dist[next][0]:
                dist[next] = (dist2+dist1,path+[next])
                heapq.heappush(pos,(dist1+dist2,next,path+[next]))
    return dist[end][1]

P = int(input())
graph = {input():{} for _ in range(P)}
for _ in range(int(input())):
    place1,place2,dist = input().split()
    graph[place1][place2] = graph[place2][place1] = int(dist)

for _ in range(int(input())):
    start,end = input().split()
    path = dijkstra(graph,start,end,P)
    s = start
    current = start
    for i in path:
        s += f'->({graph[current][i]})->{i}'
        current = i
    print(s)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240507230703376](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240507230703376.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

难度和上次的题差不多，加深了对dfs和bfs的理解。算鹰和兔子的题都类似于以前做过的题目，冰可乐类似并查集，二叉树的题就是翻译。



