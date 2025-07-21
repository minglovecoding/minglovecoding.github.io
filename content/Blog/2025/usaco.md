---
title: usaco刷题笔记
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