# Assignment #F: All-Killed 满分

Updated 1844 GMT+8 May 20, 2024

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

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/



思路：



代码

```python
# 
n = int(input())
tree = [0]
for i in range(n):
    tree.append(list(map(int, input().split())))
stack = [1]
ans = []
while stack:
    ans.append(str(stack[-1]))
    temp = []
    for x in stack:
        if tree[x][0] != -1:
            temp.append(tree[x][0])
        if tree[x][1] != -1:
            temp.append(tree[x][1])
    stack = temp
print(" ".join(ans))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240528220832226](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240528220832226.png)



### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/



思路：



代码

```python
# 
n = int(input())
a = list(map(int, input().split()))
stack = []

for i in range(n):
    while stack and a[stack[-1]] < a[i]:
        a[stack.pop()] = i + 1


    stack.append(i)

while stack:
    a[stack[-1]] = 0
    stack.pop()

print(*a)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240528221104845](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240528221104845.png)



### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/



思路：



代码

```python
# 
from collections import defaultdict

def dfs(node, color):
    color[node] = 1
    for neighbour in graph[node]:
        if color[neighbour] == 1:
            return True
        if color[neighbour] == 0 and dfs(neighbour, color):
            return True
    color[node] = 2
    return False

T = int(input())
for _ in range(T):
    N, M = map(int, input().split())
    graph = defaultdict(list)
    for _ in range(M):
        x, y = map(int, input().split())
        graph[x].append(y)
    color = [0] * (N + 1)
    is_cyclic = False
    for node in range(1, N + 1):
        if color[node] == 0:
            if dfs(node, color):
                is_cyclic = True
                break
    print("Yes" if is_cyclic else "No")
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528221859435](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240528221859435.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/



思路：主要是二分思想的利用



代码

```python
# 
n, m = map(int, input().split())
L = list(int(input()) for x in range(n))

def check(x):
    num, cut = 1, 0
    for i in range(n):
        if cut + L[i] > x:
            num += 1
            cut = L[i]  
        else:
            cut += L[i]
    
    if num > m:
        return False
    else:
        return True

maxmax = sum(L)
minmax = max(L)
while minmax < maxmax:
    middle = (maxmax + minmax) // 2
    if check(middle):   
        maxmax = middle
    else:
        minmax = middle + 1
print(maxmax)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528225347388](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240528225347388.png)



### 07735: 道路

http://cs101.openjudge.cn/practice/07735/



思路：主要是Dijkstra算法，而且在Dijkstra算法的基础上实现剪枝

代码

```python
# 
import heapq
k = int(input())
n = int(input())
r = int(input())
graph = {i:[] for i in range(1, n+1)}
for _ in range(r):
    s, d, dl, dt = map(int, input().split())
    graph[s].append((dl,dt,d))
que = [(0,0,1)]
fee = [10000]*101
def dijkstra(g):
    while que:
        l, t, d = heapq.heappop(que)
        if d == n:
            return l
        if t>fee[d]:
            continue
        fee[d] = t
        for dl, dt, next_d in g[d]:
            if t+dt <= k:
                heapq.heappush(que,(l+dl, t+dt, next_d))
    return -1
print(dijkstra(graph))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528230035107](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240528230035107.png)



### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/



思路：这道题老师说是难题，然后自己看的时候才知道这题有多恶心，自己想半天没想到怎么做，所以借鉴了题解，发现题解中有三个解法，还是第二种比较简洁，第一种比较直白，最后选择第二种解法。



代码

```python
# 
def find(x):	
    if p[x] == x:
        return x
    else:
        p[x] = find(p[x])	
        return p[x]

n,k = map(int, input().split())

p = [0]*(3*n + 1)
for i in range(3*n+1):	
    p[i] = i

ans = 0
for _ in range(k):
    a,x,y = map(int, input().split())
    if x>n or y>n:
        ans += 1; continue
    
    if a==1:
        if find(x+n)==find(y) or find(y+n)==find(x):
            ans += 1; continue
        
        p[find(x)] = find(y)				
        p[find(x+n)] = find(y+n)
        p[find(x+2*n)] = find(y+2*n)
    else:
        if find(x)==find(y) or find(y+n)==find(x):
            ans += 1; continue
        p[find(x+n)] = find(y)
        p[find(y+2*n)] = find(x)
        p[find(x+2*n)] = find(y+n)

print(ans)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528231838506](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240528231838506.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

本周的题目主要还是dfs，Dijkstra算法，heapq等学过的知识点的运用，题目有难度，开始回顾作业和课件了。



