# Assignment #1: 拉齐大家Python水平

Updated 0940 GMT+8 Feb 19, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）数算课程的先修课是计概，由于计概学习中可能使用了不同的编程语言，而数算课程要求Python语言，因此第一周作业练习Python编程。如果有同学坚持使用C/C++，也可以，但是建议也要会Python语言。

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

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/



思路：



##### 代码

```python
# 
def fib(n:int):
    n=int(n)
    f0=0
    f1=1
    f2=1
    k=0
    while k < n:
        f0,f1,f2=f1, f2, f0+f1+f2
        k+=1

    return f2
s=int(input())
print(fib(s-2))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240310210155623](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310210155623.png)



### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A



思路：



##### 代码

```python
# def chat(s:str):
        if s.count('h')==0:
                return 'NO'
        a=s.find('h')
        if a==len(s)-1:
                return 'NO'
        s=s[a+1:]
        if s.count('e')==0:
                return 'NO'
        b=s.find('e')
        if b==len(s)-1:
                return 'NO'
        s=s[b+1:]
        if s.count('l')==0:
                return 'NO'
        c=s.find('l')
        if c==len(s)-1:
                return 'NO'
        s=s[c+1:]                
        if s.count('l')==0:
                return 'NO'
        d=s.find('l')
        if d==len(s)-1:
                return 'NO'
        s=s[d+1:]
        if s.count('o')==0:
                return 'NO'
        return 'YES'
s=input()
print(chat(s))


```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240310212148883](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310212148883.png)



### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A



思路：



##### 代码

```python
# s='aoyeui'
t=input()
u=[]
t=t.lower()
for i in range(len(t)):
        if t[i] not in s:
                u.append(t[i])
if u:
        print('.',end='')
        print('.'.join(u))
                


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310213304857](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310213304857.png)



### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/



思路：



##### 代码

```python
# def prime(n:int):
    n=int(n)
    from math import sqrt
    for i in range(2,int(sqrt(n))+1):
        if n%i==0:
            return 0
    return 1
s=int(input())
i=1
while True:
    if prime(i)==1 and prime(s-i)==1:
        print(i,end=' ')
        print(s-i)
        break
    i+=1

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310212242250](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310212242250.png)



### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/



思路：



##### 代码

```python
# s=list(map(str,input().split('+')))
p={}

for i in range(len(s)):
    q=''
    for k in s[i]:
        if k!='n':
            q=q+k
        elif k=='n':
            break
    if q=='':
        q='1'
    t=s[i][::-1]
    y=''
    for m in t:
        if m!='^':
            y=y+m
        elif m=='^':
            break
    y=y[::-1]
    p[y]=q
n=sorted(p.items(), key = lambda p:(int(p[0]), p[1]),reverse=True)
i=0
if n:
    while n[i][1]=='0':
        i+=1
    print('n^',end='')
    print(n[i][0])
else:
    print('n^0')

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310212316503](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310212316503.png)



### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/



思路：



##### 代码

```python
# 
s=list(map(int,input().split()))
p={}
for i in s:
    if str(i) not in p:
        p[str(i)]=1
    else:
        p[str(i)]+=1
t=sorted(p.items(), key = lambda p:(int(p[1]), -1*int(p[0])),reverse=True)
u=t[0][1]
h=[]
for i in range(len(t)):
    if t[i][1]==u:
        h.append(str(t[i][0]))
print(' '.join(h))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310212351843](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310212351843.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“数算pre每日选做”、CF、LeetCode、洛谷等网站题目。==

主要是学会了如何对字典排序，而且知道了字典排序后返回list，但是其中的value返回的是tuple，作业比较简单，所以oj上的题每天在做，希望这学期机考能全AC吧。还有就是谢谢闫老师提供的题目和课上的讲解（极大地练习了英语）



