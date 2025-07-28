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
## 2.Farmer John's Cheese Block 
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
## 3. It's Mooin' Time 
- 修改每一个字符为26个字母,
用哈希表累积s[i:i+3]的数量,
当累计数>=F,将其加入到set里,
输出set个数以及set里的value值#修改每一个字符为26个字母,
用哈希表累积s[i:i+3]的数量,
当累计数>=F,将其加入到set里,
输出set个数以及set里的value值,

```
from collections import defaultdict

N,F=map(int,input().split())
S=input()
moos=set() #防止moo重复

def find_moos(s):
    cnt=defaultdict(int)
    for i in range(0,N-2):
        if s[i]!=s[i+1] and s[i+1]==s[i+2]:
            cnt[s[i:i+3]]+=1
            if cnt[s[i:i+3]]>=F:
                moos.add(s[i:i+3])

#遍历修改后的S
for i in range(N):
    for j in range(26):
        _S=S[:i]+chr(j+ord('a'))+S[i+1:]
        find_moos(_S)
moos=sorted(moos)
print(len(moos))
for moo in moos:
    print(moo)

```

> 2024-2025 January bronze
## 1.Astral Superposition
- 1.注意生成原map时要用`map1=[[0 for _ in range(N)] for _ in range(N)]`
- 2.处理原字符需将字符转化为数字
- 3.先处理map2为2的
- 4.再处理map2为1的
```
#将字符特征化
WGB={
    'W':0,
    'G':1,
    'B':2,
}

def _case():
    N,A,B=[int(x) for x in input().split()]
    #str to num
    map2=[[WGB[x] for x in input()] for _ in range(N)]
    #res is map1
    map1=[[0 for _ in range(N)] for _ in range(N)]
    #先处理map2的bold
    for i in range(N):
        for j in range(N):
            if map2[i][j]==2:
                map1[i][j]=1
                if i<B or j<A:
                    print(-1)
                    return
                if map2[i-B][j-A]==0:
                    print(-1)
                    return
                map1[i-B][j-A]=1
    #再处理map1的gray
    for i in range(N):
        for j in range(N):
            if map2[i][j]==1:
                if map1[i][j]==1:
                    continue
                if i<B or j<A:
                    map1[i][j]=1
                    continue
                if map1[i-B][j-A]==1:
                    continue
                map1[i][j]=1
    print(sum(sum(x) for x in map1))
    #print(map1)
    #return res

T=int(input())
while T:
    T-=1
    _case()

```

## 2. It's Mooin' Time II
- 用left和right记录左右字符出现的次数
- 1 2 2 [left:right]
```
N=int(input())
nums=[int(x) for x in input().split()]

left=[0]*(N+1)
right=[0]*(N+1)
_left=0
ans=0

for i in range(N):
    right[nums[i]]+=1

for i in range(N):
    if right[nums[i]]==2:
        ans+=_left-(left[nums[i]]>0)
    right[nums[i]]-=1
    _left+=left[nums[i]]==0
    left[nums[i]]+=1

print(ans)

```

## 3.Cow Checkups
- 逐个遍历index向外拓展
```
def main():
    import sys
    input = sys.stdin.read
    data = input().split()

    n = int(data[0])
    A = list(map(int, data[1:n+1]))
    B = list(map(int, data[n+1:2*n+1]))

    already_same = sum(1 for i in range(n) if A[i] == B[i])

    ans = [0] * (n + 1)

    def expand(l, r):
        nonlocal already_same
        match = already_same
        while l >= 0 and r < n:
            # 减去之前的匹配值
            match -= (A[l] == B[l]) + (A[r] == B[r])
            # 加上交换后的匹配值
            match += (A[l] == B[r]) + (A[r] == B[l])
            ans[match] += 1
            l -= 1
            r += 1

    for mid in range(n):
        expand(mid, mid)      # 奇数长度的中心扩展
        expand(mid, mid + 1)  # 偶数长度的中心扩展

    for count in ans:
        print(count)

if __name__ == "__main__":
    main()

```