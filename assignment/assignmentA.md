# Assignment #A: 图论：遍历，树算及栈

Updated 2018 GMT+8 Apr 21, 2024

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

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/



思路：简单的用栈



代码

```python
# 
def reverse(s):
    stack = []
    for char in s:
        if char == ')':
            temp = []
            while stack and stack[-1] != '(':
                temp.append(stack.pop())
            if stack:
                stack.pop()
            stack.extend(temp)
        else:
            stack.append(char)
    return ''.join(stack)

s = input().strip()
print(reverse(s))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240505212737907](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240505212737907.png)



### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/



思路：根据前中序写后序



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
        preorder,inorder = input().split()
        postorder = build_tree(preorder, inorder)
        print(''.join(postorder))
    except EOFError:
        break
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240505213016831](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240505213016831.png)



### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现



思路：利用队列，每次加0或1，直至找到倍数



代码

```python
# 
from collections import deque

def find_multiple(n):
    q = deque()
    q.append((1 % n, "1"))
    visited = set([1 % n])
    while q:
        mod, num_str = q.popleft()
        if mod == 0:
            return num_str
        for digit in ["0", "1"]:
            new_num_str = num_str + digit
            new_mod = (mod * 10 + int(digit)) % n
            if new_mod not in visited:
                q.append((new_mod, new_num_str))
                visited.add(new_mod)
while True:
    n = int(input())
    if n == 0:
        break
    print(find_multiple(n))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240505213834939](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240505213834939.png)



### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/



思路：



代码

```python
# 
from collections import deque
m, n, chakra = map(int, input().split())
graph = []
pos = set()
ans = []
naruto = sasuke = None
for i in range(m):
    arr = list(input())
    if '@' in arr:
        naruto = (i, arr.index('@'))
        pos.add((naruto[0], naruto[1], chakra))
    if '+' in arr:
        sasuke = (i, arr.index('+'))

    graph.append(arr)

graph[sasuke[0]][sasuke[1]] = '*'

stepx = [-1, 1, 0, 0]
stepy = [0, 0, -1, 1]

def bfs():
    global ans
    queue = deque()
    queue.append((naruto[0], naruto[1], 0, chakra))

    while queue:
        x, y, depth, chak = queue.popleft()
        if (x, y) == sasuke:
            ans.append(depth)
            return
        
        for i in range(4):
            x1, y1 = x + stepx[i], y + stepy[i]
            if 0 <= x1 < m and 0 <= y1 < n :
                if graph[x1][y1] == '*' and (x1, y1, chak) not in pos:
                    queue.append((x1, y1, depth + 1, chak))
                    pos.add((x1, y1, chak))
                elif chak > 0 and graph[x1][y1] == '#' and (x1, y1, chak-1) not in pos:
                    queue.append((x1, y1, depth + 1, chak-1))
                    pos.add((x1, y1, chak-1))
bfs()
if ans:
    print(min(ans))
else:
    print(-1)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240505215057949](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240505215057949.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/



思路：参考了群里的做法，利用堆结构实现bfs



代码

```python
# 
import heapq
m,n,p = map(int,input().split())
info = []
for _ in range(m):
    row = [int(x) if x!='#' else '#' for x in input().split()]
    info.append(row)
dire = [(-1,0),(1,0),(0,1),(0,-1)]

def bfs(sr,sc,er,ec):
    if info[sr][sc]=='#' or info[er][ec]=='#':
        return 'NO'
    dist = [[float('inf')]*n for _ in range(m)]
    dist[sr][sc] = 0
    points = [(0,sr,sc)]
    while points:
        d,nr,nc = heapq.heappop(points)
        h = info[nr][nc]
        if nr==er and nc==ec:
            return d
        for dr,dc in dire:
            nnr = nr + dr
            nnc = nc + dc
            if 0<=nnr<m and 0<=nnc<n and info[nnr][nnc]!='#' and dist[nnr][nnc]>d+abs(info[nnr][nnc]-h):
                dist[nnr][nnc] = d+abs(info[nnr][nnc]-h)
                heapq.heappush(points,(dist[nnr][nnc],nnr,nnc))
    return 'NO'

for _ in range(p):
    sr,sc,er,ec = map(int,input().split())
    print(bfs(sr,sc,er,ec))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240505221735881](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240505221735881.png)



### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/



思路：



代码

```python
# 
class DisjSet:
    def __init__(self, n):
        self.parent = [i for i in range(n)]
        self.rank = [0]*n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        xset, yset = self.find(x), self.find(y)
        if self.rank[xset] > self.rank[yset]:
            self.parent[yset] = xset
        else:
            self.parent[xset] = yset
            if self.rank[xset] == self.rank[yset]:
                self.rank[yset] += 1

def kruskal(n, edges):
    dset = DisjSet(n)
    edges.sort(key = lambda x:x[2])
    sol = 0
    for u, v, w in edges:
        u, v = ord(u)-65, ord(v)-65
        if dset.find(u) != dset.find(v):
            dset.union(u, v)
            sol += w
    if len(set(dset.find(i) for i in range(n))) > 1:
        return -1
    return sol

n = int(input())
edges = []
for _ in range(n-1):
    arr = input().split()
    root, m = arr[0], int(arr[1])
    for i in range(m):
        edges.append((root, arr[2+2*i], int(arr[3+2*i])))
print(kruskal(n, edges))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240505224623496](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240505224623496.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

前三道题简单，后三道题有难度，第五题巧妙地用到堆，第六题参考了题解，感受到了kruskal的美丽，美美放假一周，做题的时候有点生疏了，也没多做其他的题，接下来收心学习！



