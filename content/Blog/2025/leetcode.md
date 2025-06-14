---
title: leetcode hot100刷题笔记✍️
date: 2025-05-31  18:37:28 
taxonomies:
  tags:
    - leetcode
---
- 72.编辑距离
给你两个单词 word1 和 word2， 请返回将 word1 转换成 word2 所使用的最少操作数 。  
你可以对一个单词进行如下三种操作：  
插入一个字符  
删除一个字符  
替换一个字符  
示例 1：  
输入：word1 = "horse", word2 = "ros"  
输出：3  
解释：  
horse -> rorse (将 'h' 替换为 'r')  
rorse -> rose (删除 'r')  
rose -> ros (删除 'e')  
>思路：二维动态规划，需要注意当x!=y时，插入、删除、替换的状态方程是：f[i+1][j+1]=min(f[i+1][j]，f[i][j+1],f[i][j])+1。
```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m=len(word1)
        n=len(word2)
        f=[[0]*(n+1) for _ in range(m+1)]
        for i in range(n+1):
            f[0][i]=i
        for i,x in enumerate(word1):
            f[i+1][0]=i+1
            for j,y in enumerate(word2):
                if x==y:
                    f[i+1][j+1]=f[i][j]
                else:
                    f[i+1][j+1]=min(f[i][j+1],f[i+1][j],f[i][j])+1
        return f[m][n]

```
- 1143.最长公共子序列
给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。  
一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。  
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。  
两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。  
示例 1：  
输入：text1 = "abcde", text2 = "ace"   
输出：3    
解释：最长公共子序列是 "ace" ，它的长度为 3 。  
>思路：二维动态规划，dp第一行第一列都初始化为0，#dp[i][j]=dp[i-1][j-1]+1 text1[i]==text2[j]，dp[i][j]=max(dp[i-1][j],dp[i][j-1]) text1[i]!=text2[j]

```
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        #动态规划二维数组dp
        #dp第一行第一列都初始化为0
        #dp[i][j]=dp[i-1][j-1]+1 text1[i]==text2[j]
        #dp[i][j]=max(dp[i-1][j],dp[i][j-1]) text1[i]!=text2[j]
        m=len(text1)
        n=len(text2)
        dp=[[0]*(n+1) for _ in range(m+1)]
        for i in range(1,m+1):
            for j in range(1,n+1):
                if text1[i-1]==text2[j-1]:
                    dp[i][j]=dp[i-1][j-1]+1
                else:
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1])
        return dp[m][n]
```

- 5.最长回文子串
给你一个字符串 s，找到 s 中最长的 回文 子串。  
示例 1：  
输入：s = "babad"  
输出："bab"  
解释："aba" 同样是符合题意的答案。  
>思路：回文子串用暴力法。用双层循环取子串t,只需判断t是否等于t[::-1]。
```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n=len(s)
        res=[]
        for i in range(n):
            for j in range(i+1,n+1):
                t=s[i:j]
                if(t==t[::-1]):
                    res.append(t)
        cnt=0
        for i in range(len(res)):
            cnt=max(cnt,len(res[i]))
        for item in res:
            if(len(item)==cnt):
                return item

```

- 32.最长有效括号
给你一个只包含 '(' 和 ')' 的字符串，找出最长有效（格式正确且连续）括号子串的长度。  
示例 1：  
输入：s = "(()"  
输出：2  
解释：最长有效括号子串是 "()"  
>思路：对()下标标1用dp数组存储，遍历dp数组，找出连续的最长1。
```
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        #对()下标标记1
        #求最长连续1的值
        stack=[]
        res=0
        n=len(s)
        dp=[0]*n
        cnt=0
        for i in range(n):
            if s[i]=="(":
                stack.append(i)
            else:
                if stack:
                    j=stack.pop()
                    dp[i],dp[j]=1,1
        for item in dp:
            if item:
                cnt+=1
            else:
                res=max(res,cnt)
                cnt=0
        res=max(res,cnt)
        return res

```

- 139.单词拆分
给你一个字符串 s 和一个字符串列表 wordDict 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 s 则返回 true。  
注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。  
示例 1：  
输入: s = "leetcode", wordDict = ["leet", "code"]  
输出: true  
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。  
>思路：动态规划，初始化dp=[False]*(n+1),dp[0]等于True,用双层循环遍历字符s'是否在字符串s里。注意s[i:j]是指s[i]~s[j-1]。
```
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        n=len(s)
        dp=[False]*(n+1)
        dp[0]=True
        for i in range(n):
            for j in range(i+1,n+1):
                if dp[i]==True and s[i:j] in wordDict:
                    dp[j]=True
        return dp[n]

```

- 416.分割等和子集
给你一个只包含正整数的非空数组nums。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。  
示例 1：  
输入：nums = [1,5,11,5]  
输出：true  
解释：数组可以分割成 [1, 5, 5] 和 [11]。  
>思路：0-1背包问题，选与不选。累积所有数sum/2。如果为奇数，不可能，偶数则在nums里选数个和为sum/2。
```
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        cnt=sum(nums)
        if cnt%2:
            return False
        n=len(nums)
        cnt//=2
        f=[[False]*(cnt+1) for _ in range(n+1)]
        f[0][0]=True
        for i,x in enumerate(nums):
            for j in range(cnt+1):
                if j<x:
                    f[i+1][j]=f[i][j]
                else:
                    f[i+1][j]=f[i][j-x] or f[i][j]
        return f[n][cnt]

```
- 152.乘积最大子数组
给你一个整数数组 nums ，请你找出数组中乘积最大的非空连续 子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。  
测试用例的答案是一个 32-位 整数。  
示例 1:  
输入: nums = [2,3,-2,4]  
输出: 6  
解释: 子数组 [2,3] 有最大乘积 6。  
> 思路：动态规划，只需要考虑每个数当前的状态。只需在之前的状态更新即可，设maxx和minn。maxx=(pre_maxx`*`num,pre_minn`*`num,num)，每次更新res值。

```
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        maxx=minn=res=nums[0]
        for num in nums[1:]:
            maxx1=max(maxx*num,minn*num,num)
            minn1=min(maxx*num,minn*num,num)
            res=max(res,maxx1)
            maxx=maxx1
            minn=minn1
        return res
```

- 139.单词拆分
给你一个字符串 s 和一个字符串列表 wordDict 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 s 则返回 true。  
注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。  
示例 1：  
输入: s = "leetcode", wordDict = ["leet", "code"]  
输出: true  
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。  
>思路：从左向右遍历，遍历n-1次，初始化所有dp[i]=1,每一次遍历判断一次dp[i]是否小于dp[j],如是dp[j]=dp[i]+1。

```
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n=len(nums)
        dp=[1]*n
        for i in range(1,n):
            for j in range(i):
                if nums[j]<nums[i]:
                    dp[i]=max(dp[i],dp[j]+1)
        return max(dp)

```

- 322.零钱兑换  
给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。  
计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。  
你可以认为每种硬币的数量是无限的。  
示例 1：  
输入：coins = [1, 2, 5], amount = 11  
输出：3   
解释：11 = 5 + 5 + 1  
>思路：完全背包，返回结果f[n][amount],当数组中的值不确定时用enumerate遍历。

```
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        n=len(coins)
        f=[[inf]*(amount+1) for _ in range(n+1)]
        f[0][0]=0
        for i,j in enumerate(coins):
            for value in range(amount+1):
                if value<j:
                    f[i+1][value]=f[i][value]
                else:
                    f[i+1][value]=min(f[i][value],f[i+1][value-j]+1)
        ans=f[n][amount]
        return ans if ans<inf else -1

```

- 279.完全平方数
给你一个整数 n ，返回 和为 n 的完全平方数的最少数量 。  
完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。  
示例 1：  
输入：n = 12  
输出：3   
解释：12 = 4 + 4 + 4  
> 思路：完全背包，用二维数组f[i][j]计算，i表示乘数，j表示目标数。设立边界f[0][0]=0,f[0][N]=inf。

```
N=10000
f=[[0]*(N+1) for _ in range(isqrt(N)+1)]
f[0]=[0]+[inf]*N
for i in range(1,len(f)):
    for j in range(N+1):
        if j<i*i:
            f[i][j]=f[i-1][j]
        else:
            f[i][j]=min(f[i-1][j],f[i][j-i*i]+1)

class Solution:
    def numSquares(self, n: int) -> int:
        return f[isqrt(n)][n]

```

- 763.划分字母区间
给你一个字符串 s 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。例如，字符串 "ababcc" 能够被分为 ["abab", "cc"]，但类似 ["aba", "bcc"] 或 ["ab", "ab", "cc"] 的划分是非法的。  
注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 s 。  
返回一个表示每个字符串片段的长度的列表。  
示例 1：  
输入：s = "ababcbacadefegdehijhklij"  
输出：[9,7,8]  
解释：  
划分结果为 "ababcbaca"、"defegde"、"hijhklij" 。  
每个字母最多出现在一个片段中。  
像 "ababcbacadefegde", "hijhklij" 这样的划分是错误的，因为划分的片段数较少  
> 思路：1.记录每个字符在字符串里最后的位置 2.用end标记最远的距离 3.遍历字符串所有字符，当找到end，输出。需注意ord用法。

```
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        start=end=0
        last=[0]*26
        res=list()
        for i,str1 in enumerate(s):
            last[ord(str1)-ord("a")]=i

        for i,str1 in enumerate(s):
            end=max(end,last[ord(str1)-ord("a")])
            if i==end:
                res.append(end-start+1)
                start=end+1
        return res

```

- 45.跳跃游戏II
给定一个长度为 n 的 0 索引整数数组 nums。初始位置为 nums[0]。  
每个元素 nums[i] 表示从索引 i 向后跳转的最大长度。换句话说，如果你在 nums[i] 处，你可以跳转到任意 nums[i + j] 处:  
0 <= j <= nums[i]   
i + j < n  
返回到达 nums[n - 1] 的最小跳跃次数。生成的测试用例可以到达 nums[n - 1]。  
示例 1:  
输入: nums = [2,3,1,1,4]  
输出: 2  
解释: 跳到最后一个位置的最小跳跃数是 2。  
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。  
>思路：设置坐标pos，pos初始化=末尾元素，依次寻找离pos最左的元素，并step+1，直到pos<0。

```
class Solution:
    def jump(self, nums: List[int]) -> int:
          pos=len(nums)-1
          steps=0
          while pos>0:
            for i in range(pos):
                if i+nums[i]>=pos:
                    pos=i
                    steps+=1
                    break
          return steps

```

- 347.前K个高频元素
给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。  
示例 1:  
输入: nums = [1,1,1,2,2,3], k = 2  
输出: [1,2]  
> 思路：先统计每个字符出现的次数，然后用heappq加入进来，注意heappq用法，heapq.push(res,num)是直接加入num，heapq.pushpop(res,num)是加入一个数再弹出最小值，遍历字典值是dict.items()。

```
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        #temp=Counter(nums).most_common(k)
        #res=[]
        #for item,_ in temp:
        #    res.append(item)
        #return res
         
        dic={}
        for num in nums:
            dic[num]=dic.setdefault(num,0)+1
        heapk=[]
        for num,cnt in dic.items():
            if len(heapk)<k:
                heapq.heappush(heapk,(cnt,num))
            elif cnt>heapk[0][0]:
                heapq.heappushpop(heapk,(cnt,num))
                print(1)
        res=[]
        for cnt,num in heapk:
            res.append(num)
        return res
        
```

- 208.实现Trie(前缀树)
Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。  
请你实现 Trie 类：  
Trie() 初始化前缀树对象。  
void insert(String word) 向前缀树中插入字符串 word 。  
boolean search(String word) 如果字符串 word 在前缀树中，返回 true（即，在检索之前已经插入）；否则，返回 false 。  
boolean startsWith(String prefix) 如果之前已经插入的字符串 word 的前缀之一为 prefix ，返回 true ；否则，返回 false 。  
示例：  
输入  
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]  
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]  
输出  
[null, null, true, false, true, null, true]  
解释  
Trie trie = new Trie();  
trie.insert("apple");  
trie.search("apple");   // 返回 True  
trie.search("app");     // 返回 False  
trie.startsWith("app"); // 返回 True  
trie.insert("app");  
trie.search("app");     // 返回 True  
>思路：前缀树经典题，初始化26字符数组[]和结尾bool值，需理解ord()和chr()用法。

```
class Trie:
    def __init__(self):
        self.children=[0]*26
        self.end=False
    
    def searchprefix(self,word:str)->"Trie":
        for cur in word:
            cur=ord(cur)-ord("a")
            if not self.children[cur]:
                return None
            self=self.children[cur]
        return self

    def insert(self, word: str) -> None:
        for cur in word:
            cur=ord(cur)-ord("a")
            if not self.children[cur]:
                self.children[cur]=Trie()
            self=self.children[cur]
        self.end=True

    def search(self, word: str) -> bool:
        node=self.searchprefix(word)
        return node is not None and node.end
       
    def startsWith(self, prefix: str) -> bool:
        return self.searchprefix(prefix) is not None

```

- 207.课程表
你这个学期必须选修 numCourses 门课程，记为 0 到 numCourses - 1 。  
在选修某些课程之前需要一些先修课程。 先修课程按数组 prerequisites 给出，其中 prerequisites[i] = [ai, bi] ，表示如果要学习课程 ai 则 必须 先学习课程  bi 。  
例如，先修课程对 [0, 1] 表示：想要学习课程 0 ，你需要先完成课程 1 。  
请你判断是否可能完成所有课程的学习？如果可以，返回 true ；否则，返回 false 。  
示例 1：  
输入：numCourses = 2, prerequisites = [[1,0]]  
输出：true  
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。  
>思路：拓扑排序，判断是否是有向无环图即可。用defaultdict(list)当作邻接表。visited[u]表示是否访问，初始化为0，1表示访问过，2表示所有的后续结点入栈。
```
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        visited=[0]*numCourses
        edges=collections.defaultdict(list)
        res=True
        stack=[]
        #建邻接表
        for cur,pre in prerequisites:
            edges[pre].append(cur)
        #根据邻接表遍历是否DAG
        def dfs(u):
            nonlocal res
            visited[u]=1
            for v in edges[u]:
                if res and visited[v]==0:
                    dfs(v)
                    if not res:
                        return 
                elif visited[v]==1:
                    res=False
                    return
            visited[u]=2
            stack.append(u)

        #得出ans
        for i in range(numCourses):
            if res and visited[i]==0:
                dfs(i)
        return res

```

- 994.腐烂的橘子
在给定的 m x n 网格 grid 中，每个单元格可以有以下三个值之一：  
值 0 代表空单元格；  
值 1 代表新鲜橘子；  
值 2 代表腐烂的橘子。  
每分钟，腐烂的橘子 周围 4 个方向上相邻 的新鲜橘子都会腐烂。  
返回 直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1。  
示例 1：  
输入：grid = [[2,1,1],[1,1,0],[0,1,1]]  
输出：4  
> 思路：一眼BFS,首先把腐烂的橘子加入deque里，逐个腐烂周围新鲜的橘子，并加入到deque里。腐烂完全局判断是否还有新鲜的橘子。如有返回time=-1，如没有返回time。

```
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        deque=collections.deque()
        m=len(grid)
        n=len(grid[0])
        time=0
        for i in range(m):
            for j in range(n):
                if grid[i][j]==2:
                    deque.append((i,j,time))
        while deque:
            x,y,time=deque.popleft()
            for i,j in [[x-1,y],[x+1,y],[x,y-1],[x,y+1]]:
                if 0<=i<m and 0<=j<n and grid[i][j]==1:
                    grid[i][j]=2
                    deque.append((i,j,time+1))
        for i in range(m):
            for j in range(n):
                if grid[i][j]==1:
                    return -1
        return time

```
- 200.岛屿数量
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。
岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。
此外，你可以假设该网格的四条边均被水包围。
示例 1：    
输入：grid = [  
  ["1","1","1","1","0"],  
  ["1","1","0","1","0"],  
  ["1","1","0","0","0"],  
  ["0","0","0","0","0"]  
]  
输出：1
> 思路1：DFS深搜，dfs函数是先标记,再设立界限，逐个向上下左右延展判断是否是“1”，是的话继续调用dfs函数。思路2：BFS广搜，使用collection.deque函数，注意append时用deque.append(())，里面需加()。

```
class Solution:
    #标记
    #设立界限
    #向外延展

    #point=collections.deque()
    #point.popleft()
    #point.append()

    def numIslands(self, grid: List[List[str]]) -> int:
        m=len(grid)
        n=len(grid[0])
        res=0
        for i in range(m):
            for j in range(n):
                if grid[i][j]=="1":
                    grid[i][j]="0"
                    res+=1
                    deque=collections.deque([(i,j)])
                    while deque:
                        row,col=deque.popleft()
                        for x,y in [(row+1,col),(row-1,col),(row,col+1),(row,col-1)]:
                            if 0 <=x<m and 0<=y<n and grid[x][y]=="1":
                                deque.append((x,y))
                                grid[x][y]="0"
        return res

```

- 240.搜索二维矩阵II
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：  
每行的元素从左到右升序排列。  
每列的元素从上到下升序排列。  
示例 1：  
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5  
输出：true  
>思路：第一个查询元素在位置(0,n-1),当比target大向左移动，当比target小向下移动。

```
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m,n=len(matrix),len(matrix[0])
        x,y=0,n-1
        while x<m and y>=0:
            if matrix[x][y]==target:
                return True
            if matrix[x][y]>target:
                y-=1
            else:
                x+=1
        return False

```
- 48.旋转图像
给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。  
你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。  
示例 1：  
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]  
输出：[[7,4,1],[8,5,2],[9,6,3]]  
>思路：上下翻转，左右对角线反转。

```
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        n=len(matrix)
        for i in range(n//2):
            for j in range(n):
                matrix[i][j],matrix[n-i-1][j]=matrix[n-i-1][j],matrix[i][j]
        for i in range(n):
            for j in range(i):
                matrix[i][j],matrix[j][i]=matrix[j][i],matrix[i][j]

```

- 54.螺旋矩阵  
给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。
示例 1：  
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]  
输出：[1,2,3,6,9,8,7,4,5]  

>思路：模拟，找出四个角left,right,top,bottom。分为四层加入数组res里。最后输出res

```
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        res=[]
        left,right,top,bottom=0,len(matrix[0])-1,0,len(matrix)-1
        #define four corners
        while left<=right and top<=bottom:
            for col in range(left,right+1):
                res.append(matrix[top][col])
            for row in range(top+1,bottom+1):
                res.append(matrix[row][right])
            if left<right and top<bottom:
                for col in range(right-1,left,-1):
                    res.append(matrix[bottom][col])
                for row in range(bottom,top,-1):
                    res.append(matrix[row][left])
            left,right,top,bottom=left+1,right-1,top+1,bottom-1
        return res   

```
- 41.缺失的第一个正数
给你一个未排序的整数数组 nums ，请你找出其中没有出现的最小的正整数。  
请你实现时间复杂度为 O(n) 并且只使用常数级别额外空间的解决方案。  
示例 1：  
输入：nums = [1,2,0]  
输出：3  
解释：范围 [1,2] 中的数字都在数组中。  

>第一种是用哈希表存值，再逐个遍历元素判断是否在。第二种是原地生成哈希，让每个元素回到本来应该在的位置，需注意交换时要nums[nums[i]-1],nums[i]=nums[i],nums[nums[i]-1]。最后逐个判断元素是否符合要求。

```
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        n=len(nums)
        for i in range(n):
            while 1<=nums[i]<=n and nums[i]!=nums[nums[i]-1]:
                nums[nums[i]-1],nums[i]=nums[i],nums[nums[i]-1]
        
        for i in range(n):
            if nums[i]!=i+1:
                return i+1
        return n+1

```

- 189.轮换数组
给定一个整数数组 nums，将数组中的元素向右轮转 k 个位置，其中 k 是非负数。  
示例 1:  
输入: nums = [1,2,3,4,5,6,7], k = 3  
输出: [5,6,7,1,2,3,4]  
解释:  
向右轮转 1 步: [7,1,2,3,4,5,6]  
向右轮转 2 步: [6,7,1,2,3,4,5]  
向右轮转 3 步: [5,6,7,1,2,3,4]  
>思路：最优做法，反转数组，先reverse(0,n-1),再reverse(0,k-1),再reverse(k,n-1)

```
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        def reverse(i:int,j:int):
            while i<j:
                nums[i],nums[j]=nums[j],nums[i]
                i+=1
                j-=1
        n=len(nums)
        k=k%n
        reverse(0,n-1)
        reverse(0,k-1)
        reverse(k,n-1)

```

- 56.合并区间
以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。  
示例 1：  
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]  
输出：[[1,6],[8,10],[15,18]]  
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].  

>思路：首先是sort排序，intervals.sort(key=lambda x:x[0]),再逐个比较集合里的list,如果后一个列表里0位大于前一个列表的1位直接添加，否则更新前一个列表的1位，按此逻辑判断完所有元素。

```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x:x[0])
        res=[]
        for item in intervals:
            if not res or item[0]>res[-1][1]:
                res.append(item)
            else:
                res[-1][1]=max(res[-1][1],item[1])
        return res

```

- 53.最大子数和
给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。  
子数组是数组中的一个连续部分。  
示例 1：  
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]  
输出：6  
解释：连续子数组 [4,-1,2,1] 的和最大，为 6。  
>思路：动态规划，写出状态方程，dp[i]=dp[i-1]+nums[i],因为是求最大子数组和，所以需判断dp[i-1]是否大于0，如果小于，则dp[i]=nums[i]。

```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        n=len(nums)
        dp=[0]*n    
        res=dp[0]=nums[0]
        for i in range(1,n):
            if dp[i-1]>0:
                dp[i]=dp[i-1]+nums[i]
            else:
                dp[i]=nums[i]
            res=max(res,dp[i])
        return res    
 
```

- 76.最小覆盖子串
给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。  
注意：  
对于 t 中重复字符，我们寻找的子字符串中该字符数量必须不少于 t 中该字符数量。  
如果 s 中存在这样的子串，我们保证它是唯一的答案。  
示例 1：  
输入：s = "ADOBECODEBANC", t = "ABC"  
输出："BANC"  
解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。  

>思路：左右双指针，用哈希表记录t的键和值，右指针依次迭代，当t值在s里，进行t值减1操作，当dict[key]==0，len减1操作，当len等于0时，移动左指针，找出最小的res

```
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        res=""
        dic=defaultdict(int)
        left=0
        for i,val in enumerate(t):
            dic[val]+=1
        n=len(dic)
        for right,val in enumerate(s):
            if val in t:
                dic[val]-=1
                if dic[val]==0:
                    n-=1
                while n==0: #找到子串了
                    if not res or right-left+1<len(res):
                        res=s[left:right+1]
                    if s[left] in dic:
                        dic[s[left]]+=1
                        if dic[s[left]]>0:
                             n+=1
                    left+=1    
        return res

```

- 239.滑动窗口最大值
给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。返回滑动窗口中的最大值 。
示例 1：  
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3  
输出：[3,3,5,5,6,7]  
解释：  
滑动窗口的位置                最大值    
[1  3  -1] -3  5  3  6  7       3  
 1 [3  -1  -3] 5  3  6  7       3  
 1  3 [-1  -3  5] 3  6  7       5  
 1  3  -1 [-3  5  3] 6  7       5  
 1  3  -1  -3 [5  3  6] 7       6  
 1  3  -1  -3  5 [3  6  7]      7  

>思路：deque双端队列递减存放窗口元素值。分两部分，首先是窗口未形成，这时一直在添加元素，deque只需对新加入的元素进行判断。如果小于deque最后的元素，直接添加，如果大于，deque逐个弹出尾部元素。当窗口形成后，还需考虑移走的元素是不是deque[0],如果是，需要deque.popleft()。

```
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        deque=collections.deque()
        res=[]
        n=len(nums)
        for i in range(k):
            while deque and deque[-1]<nums[i]:
                deque.pop()
            deque.append(nums[i])
        res.append(deque[0])

        for j in range(k,n):
            if deque[0]==nums[j-k]:
                deque.popleft()
            while deque and deque[-1]<nums[j]:
                deque.pop()
            deque.append(nums[j])
            res.append(deque[0])
        return res

```
- 560.和为K的子数组  
给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的子数组的个数 。
子数组是数组中元素的连续非空序列。  
示例 1：  
输入：nums = [1,1,1], k = 2  
输出：2  

> 思路：前缀和+哈希表，python哈希表是dic=defaultdict([数据类型])，逐个遍历每个前缀和，关键是在字典里找key为pre[i]-k的值。

```
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        res=0
        pre=[0]*(len(nums)+1)
        dic=defaultdict(int)
        n=len(nums)
        for i in range(n):
            pre[i+1]=pre[i]+nums[i]
        for j in pre:
            res+=dic[j-k]
            dic[j]+=1
        return res

```

- 3. 无重复字符的最长子串
给定一个字符串 s ，请你找出其中不含有重复字符的 最长 子串 的长度。  
示例 1:  
输入: s = "abcabcbb"  
输出: 3   
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。  
> 思路：左右双指针，注意set用法。

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        res=0
        n=len(s)
        set1=set()
        left=0
        for right in range(n):
            while s[right] in set1:
                set1.remove(s[left])
                left+=1
            set1.add(s[right])
            res=max(res,len(set1))
        return res

```

- 438. 找到字符串中所有字母异位词  
 给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。  
 示例 1:  
 输入: s = "cbaebabacd", p = "abc"  
 输出: [0,6]  
 解释:  
 起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。  
 起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。  

>思路：该题的解题思路是首先用Counter计数器计算每个字符的出现次数,用left,right双指针依次在母串里迭代，每迭代一次对减速器进行-1操作，如果cnt_p[s[right]]<0,则移动左指针，并对cnt_p[s[left]]进行+1操作。当right-left+1等于len(p)时候，说明找到了该子串的起始索引。  

```
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        res=[]
        n=len(s)
        cnt_p=Counter(p)
        left=0
        for right in range(n):
            cnt_p[s[right]]-=1
            while cnt_p[s[right]]<0:
                cnt_p[s[left]]+=1
                left+=1
            if right-left+1==len(p):
                res.append(left)
        return res
        
```


















