# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/practice/27653/



思路：分数的加法应该也写在类里，但是这里偷了个懒，直接在类外面进行了



##### 代码

```python
# 
class Fraction:
    def __init__(self, top, bottom):
        self.num = top
        self.den = bottom
    def __str__(self):
        return str(self.num)+"/"+str(self.den)
def func2(num1,num2):
    m = max(num1, num2)
    n = min(num1, num2)
    r = m % n
    while r != 0:
        m = n
        n = r
        r = m % n
    return num1//n, num2//n

s=input().split()
num=int(s[0])*int(s[3])+int(s[2])*int(s[1])
den=int(s[1])*int(s[3])
a,b=func2(num,den)
myf=Fraction(a,b)
print(myf)
```



代码运行截图 ==（至少包含有"Accepted"）==



![image-20240310213955807](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310213955807.png)

### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110



思路：单价越高越往前



##### 代码

```python
# n,w=input().split()
n=int(n)
w=int(w)
s=[]
t=0
for i in range(n):
    pc,wt=map(int,input().split())
    va=pc/wt
    s.append([va,wt])
s.sort(key=lambda x:x[0],reverse=True)
i=0
while w>0 and i<len(s):
    if w>s[i][1]:
        t+=s[i][0]*s[i][1]
        w-=s[i][1]
    else:
        t+=w*s[i][0]
        w=0
    i+=1
print(f'{t:.1f}')

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240310214055413](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310214055413.png)



### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



思路：



##### 代码

```python
# nc=int(input())
for i in range(nc):
    n,m,b=map(int,input().split())
    s=[]
    for k in range(n):
        s.append(list(map(int,input().split())))
    s.sort(key=lambda x:(x[0],-1*x[1]))
    u=s[0][0]
    while b>0 and s!=[]:
        for k in range(m):
            if s[0][0]==u:
                b-=s[0][1]
                del s[0]
                if s==[]:
                    break
        if s:
            while s[0][0]<=u:
                del s[0]
                if s==[]:
                    break
        pku=u
        if s:
            u=s[0][0]
        if s==[]:
            break
    if b>0:
        print('alive')
    else:
        print(pku)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310214228414](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310214228414.png)



### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B



思路：用筛法筛出素数，先判断这个数是不是平方数，再看他的二次根是不是质数（刚开始没有用筛法，写了很多都超时了，然后剪枝还超时，写了2个小时最后才想到筛）



##### 代码

```python
# from math import(sqrt)
fan = 10**6+15
r = [True for i in range(fan)] 
for i in range(2,fan):
    if r[i]: 
        for j in range(i+i,fan,i):   
            r[j] = False
r[1]=False
        
n=int(input())
s=list(map(int,input().split()))
for i in range(n):
        a=int(sqrt(s[i]))
        if a*a==s[i]:
                if r[a]:
                        print('YES')
                else:
                        print('NO')
        else:
                print('NO')
                
                
#下面是另一种
import math
 
def sieve_of_eratosthenes(n):
    primes = [True] * (n+1)
    primes[0] = primes[1] = False
    p = 2
    while p * p <= n:
        if primes[p]:
            for i in range(p * p, n+1, p):
                primes[i] = False
        p += 1
    return [i for i in range(n+1) if primes[i]]
 
def is_t_prime(num, primes):
    root = int(math.sqrt(num))
    if root * root != num:
        return False
    return root in primes
 
def main():
    n = int(input())
    nums = list(map(int, input().split()))
 
    max_num = max(nums)
    primes = set(sieve_of_eratosthenes(int(math.sqrt(max_num)) + 1))
 
    for num in nums:
        if is_t_prime(num, primes):
            print("YES")
        else:
            print("NO")
 
if __name__ == "__main__":
    main()
                

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240311000213850](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240311000213850.png)

![image-20240311000155223](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240311000155223.png)



### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A



思路：



##### 代码

```python
# 
from itertools import accumulate

def prefix_sum(nums):
    return list(accumulate(nums))

def suffix_sum(nums):
    return list(accumulate(reversed(nums)))[::-1]

t = int(input())
for _ in range(t):
    N, x = map(int, input().split())
    a = [int(i) for i in input().split()]
    aprefix_sum = prefix_sum(a)
    asuffix_sum = suffix_sum(a)

    left = 0
    right = N - 1
    if right == 0:
        if a[0] % x != 0:
            print(1)
        else:
            print(-1)
        continue

    leftmax = 0
    rightmax = 0
    while left != right:
        total = asuffix_sum[left]
        if total % x != 0:
            leftmax = right - left + 1
            break
        else:
            left += 1

    left = 0
    right = N - 1
    while left != right:
        total = aprefix_sum[right]
        if total % x != 0:
            rightmax = right - left + 1
            break
        else:
            right -= 1

    if leftmax == 0 and rightmax == 0:
        print(-1)
    else:
        print(max(leftmax, rightmax))
        
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240312003256602](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240312003256602.png)



### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/



思路：就是t-prime的一个拓展，思路没什么新奇的



##### 代码

```python
# from math import(sqrt)
fan = 10**4+15
r = [True for i in range(fan)] 
for i in range(2,fan):
    if r[i]: 
        for j in range(i+i,fan,i):   
            r[j] = False
r[1]=False
        
m,n=map(int,input().split())
for i in range(m):
        s=list(map(int,input().split()))
        u=0
        for y in range(len(s)):
                a=int(sqrt(s[y]))
                if a*a==s[y]:
                        if r[a]:
                                u+=s[y]
        q=u/len(s)
        if u==0:
                print(0)
        else:
                print(f'{q:.2f}')


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![image-20240311001032004](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240311001032004.png)

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

本次作业对我有一点难度，第4题主要是忘了上学期的筛法，一直在剪枝，做题做了很久，但是最后AC的时候超级开心，而且我还用set写了一个耗时更短的程序，很有成就感，第五题是真的很难，写了很久最后看胡佬的代码才做出来。总结一下就是在刷新题的时候还是要多复习复习学过的知识，这样才能更高效的写代码。



