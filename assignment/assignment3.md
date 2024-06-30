# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：dp，重点是找到状态转移方程



##### 代码

```python
# 
n=int(input())
s=list(map(int,input().split()))
stack=[1]*n
for i in range(n):
        for j in range(i):
                if s[j]>=s[i]:
                        stack[i]=max(stack[j]+1,stack[i])
print(max(stack))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240310195457126](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310195457126.png)



**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：递归减少需要移动的塔



##### 代码

```python
# 
def moveone(From:str, To:str):
    print(From + "->" + To)
def henoi(nDisk:int, From:str, To:str, By:str): 
    if nDisk == 1:
            print('1:',end='')
            moveone(From, To)
    else: 
        henoi(nDisk - 1, From, By, To)
        print(str(nDisk)+':',end='')
        moveone(From, To)
        henoi(nDisk - 1, By, To, From)
n,a,b,c=input().split()
n=int(n)
henoi(n,a,c,b)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240310195701785](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310195701785.png)



**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：进行模拟



##### 代码

```python
# def josephus_A(n, k, m):
    people = [1] * n  
    
    i = k - 1           
    for j in range(n):  
        count = 0        
        while count < m: 
            if people[i] > 0: 
                count += 1
            if count == m:
                print(i + 1, end='') 
                people[i] = 0 
            i = (i + 1) % n 
            
        if j < n - 1:
            print(',', end='')
        else:
            print('')
            
    return
while True:
        n,k,m=input().split()
        n=int(n)
        k=int(k)
        m=int(m)
        if n+k+m!=0:
                josephus_A(n,k,m)
        else:
                break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310195910787](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310195910787.png)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：时间少的排前面就行



##### 代码

```python
# 
n=int(input())
s=list(map(int,input().split()))
u=[]
time=0
while min(s)!=99999999:
        t=min(s)
        k=0
        for i in range(len(s)):
                if s[i]==t:
                        k=i
                        break
        u.append(str(k+1))
        time+=s[k]*(len(s)-s.count(99999999)-1)
        s[k]=99999999
jun=time/n
print(' '.join(u))
print(f'{jun:.2f}')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310200115278](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310200115278.png)



**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：进行模拟



##### 代码

```python
# n=int(input())
dis=input().split()
ju=[]
zhidemai1=[]
zhidemai2=[]
for i in range(n):
        a,b=dis[i].split(',')
        a=a[1:]
        b=b[:-1]
        ju.append(int(a)+int(b))
price=list(map(int,input().split()))
y=[]
for i in range(n):
        y.append(price[i])
y.sort(key=lambda y:int(y))
xjb=[]
for i in range(n):
        xjb.append(ju[i]/price[i])
x=[]
for i in range(n):
        x.append(xjb[i])
x.sort(key=lambda x:float(x))
if n%2==0:
        zhong=(x[n//2]+x[n//2-1])/2
else:
        zhong=x[n//2]
for i in range(n):
        if xjb[i]>zhong:
                zhidemai1.append(i)
if n%2==0:
        zhongjia=(y[n//2]+y[n//2-1])/2
else:
        zhongjia=y[n//2]
for i in range(n):
        if price[i]<zhongjia:
                zhidemai2.append(i)
number=0
for i in range(n):
        if i in zhidemai1 and i in zhidemai2:
                number+=1
print(number)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310200215425](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310200215425.png)



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：写一个函数比较数据大小并以list返回一个以B，M表示的数据大小



##### 代码

```python
# 
def order(t:list):
        bi=[]
        mi=[]
        for i in range(len(t)):
                if t[i][-1]=='B':
                        if t[i][:-1].count('.')==1:
                                bi.append(float(t[i][:-1]))
                        else:
                                bi.append(int(t[i][:-1]))
                else:
                        if t[i][:-1].count('.')==1:
                                mi.append(float(t[i][:-1]))
                        else:
                                mi.append(int(t[i][:-1]))            
        bi.sort()
        mi.sort()
        p=[]
        for i in range(len(mi)):
                p.append(str(mi[i])+'M')
        for i in range(len(bi)):
                p.append(str(bi[i])+'B')
        return p
                

n=int(input())
p={}
for i in range(n):
        name,vol=input().split('-')
        if name not in p:
                p[name]=vol
        elif name in p:
                p[name]=p[name]+' '+vol
for k in p:
        t=p[k].split()
        s=order(t)
        p[k]=s
fin=sorted(p.items(), key = lambda p:p[0])
for e in range(len(fin)):
        print(fin[e][0],end=': ')
        print(', '.join(fin[e][1]))

                

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310205606561](C:\Users\张文杰\AppData\Roaming\Typora\typora-user-images\image-20240310205606561.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==



在考试时间里AC4，第一题和最后一题没有做出来，第一题没做出来主要是对于dp问题还不够熟悉（因为上学期对这方面的题做的很少，只做过1，2道题），最后一道题是没时间做了。而且我的代码都偏长，有些内置函数没有用到，所以现在靠群里的大佬的指点我也在慢慢学习，所以接下来要加强对dp问题的练习，加快做题速度，增强对内置函数的了解。

