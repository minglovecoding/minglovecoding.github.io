---
title: USACO刷题笔记
date: 2025-07-21  20:19:00 
taxonomies:
  tags:
    - usaco
---

> 2024-2025 December bronze
## 1.Roundabout Rounding ## 
- 1.strip是去除字符串两端的换行符、空格符、制表符
- 2.个数是min(4999,N)-4444

```
def diff(N):
    #找出N有多少个digits
    digits=0
    res=0
    while 10**digits<N:
        digits+=1
    #4999-4444
    for digit in range(1,digits+1):
        upper=int('5'+'0'*(digit-1))-1
        upper=min(N,upper)
        lower=int('4'*digit)
        res+=max(0,upper-lower)
    return res

T=int(input().strip())
for _ in range(T):
    N=int(input().strip())
    print(diff(N))

```
## 2.Farmer John's Cheese Block ##
- 三个轴面xy,xz,yz,一个`1*1*N`块能塞进原奶酪，说明原奶酪以任一个轴面为底的空间都被切完。所以可以初始化三个轴面为底的值为N，每切一个奶酪就把三个轴面底值-1。每切一次，累积轴面底为0的个数。最多的可能为3*N。

```
class cheese_block:
    def __init__(self,n):
        self.n=n 
        self.ans=0
        self.xy=[[n]*n for _ in range(n)]
        self.xz=[[n]*n for _ in range(n)]
        self.yz=[[n]*n for _ in range(n)]
    
    def carve(self,x,y,z):
        self.xy[x][y]-=1
        self.xz[x][z]-=1
        self.yz[y][z]-=1
        self.ans+=(self.xy[x][y]==0)+(self.xz[x][z]==0)+(self.yz[y][z]==0)
    
    def res(self):
        return self.ans
    
N,Q=map(int,input().split())
cb=cheese_block(N)
for _ in range(Q):
    x,y,z=map(int,input().split())
    cb.carve(x,y,z)
    print(cb.res())
    
```