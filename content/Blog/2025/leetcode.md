---
title: leetcode hot100刷题笔记✍️
date: 2025-05-31  18:37:28 
taxonomies:
  tags:
    - leetcode
---

- 198.打家劫舍
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。  
给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。  
示例 1：  
输入：[1,2,3,1]  
输出：4  
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。  
     偷窃到的最高金额 = 1 + 3 = 4 。  
>思路:最经典的动规，把状态方程写出来。dp[i]=max(dp[i-2]+nums[i],dp[i-1])。考虑状态方程时只需要考虑当前的情况是由什么决定。

```
class Solution:
    def rob(self, nums: List[int]) -> int:
        dp=[0]*len(nums)
        if len(nums)==1:
            return nums[0]
        elif len(nums)==2:
            return max(nums[0],nums[1])
        dp[0]=nums[0]
        dp[1]=max(nums[0],nums[1])
        for i in range(2,len(nums)):
            dp[i]=max(dp[i-2]+nums[i],dp[i-1])
        return dp[len(nums)-1]

```

- 287.寻找重复数
给定一个包含 n + 1 个整数的数组 nums ，其数字都在 [1, n] 范围内（包括 1 和 n），可知至少存在一个重复的整数。  
假设 nums 只有 一个重复的整数 ，返回 这个重复的数 。  
你设计的解决方案必须 不修改 数组 nums 且只用常量级 O(1) 的额外空间。  
示例 1：  
输入：nums = [1,3,4,2,2]  
输出：2  
>思路：该题神似环形链表II,用快慢指针方法解决。在快慢指针第一次相遇后，用新指针从0开始走，新指针与慢指针重新开始走，当他们相遇时，新指针的值即是重复数。
、、、
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        slow,fast=0,0
        while True:
            slow,fast=nums[slow],nums[nums[fast]]
            if slow==fast:
                break
        ptr=0
        while ptr!=slow:
            slow,ptr=nums[slow],nums[ptr]
        return ptr
、、、
- 31.下一个排列
整数数组的一个 排列  就是将其所有成员以序列或线性顺序排列。  
例如，arr = [1,2,3] ，以下这些都可以视作 arr 的排列：[1,2,3]、[1,3,2]、[3,1,2]、[2,3,1]。  
示例 1：  
输入：nums = [1,2,3]  
输出：[1,3,2]  
>思路：1.找出num[i]< nums[i+1]。2.在右边找出j满足nums[i]< nums[j]。3.交换，并将(i+1，n)数进行调换。

```
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        left=len(nums)-2
        while left>=0 and nums[left]>=nums[left+1]:
            left-=1
        if left>=0:
            right=len(nums)-1
            while right>=0 and nums[left]>=nums[right]:
                right-=1
            nums[left],nums[right]=nums[right],nums[left]
        
        left,right=left+1,len(nums)-1
        while left<right:
            nums[left],nums[right]=nums[right],nums[left]
            left+=1
            right-=1
```

- 136.只出现一次的数字
给你一个 非空 整数数组 nums ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。  
你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。  
示例 1 ：  
输入：nums = [2,2,1]  
输出：1  
>思路：异或操作，所有出现两次的数字异或时都会为,把所有数字都进行异或操作，最后只留下出现一次的元素。

```
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        res=0
        for num in nums:
            res^=num
        return res

```

- **295.数据流的中位数**
中位数是有序整数列表中的中间值。如果列表的大小是偶数，则没有中间值，中位数是两个中间值的平均值。  
例如 arr = [2,3,4] 的中位数是 3 。  
例如 arr = [2,3] 的中位数是 (2 + 3) / 2 = 2.5 。  
实现 MedianFinder 类:  
MedianFinder() 初始化 MedianFinder 对象。  
void addNum(int num) 将数据流中的整数 num 添加到数据结构中。  
double findMedian() 返回到目前为止所有元素的中位数。与实际答案相差 10-5 以内的答案将被接受。  
示例 1：  
输入  
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]  
[[], [1], [2], [], [3], []]   
输出  
[null, null, null, 1.5, null, 2.0]  
解释  
MedianFinder medianFinder = new MedianFinder();  
medianFinder.addNum(1);    // arr = [1]  
medianFinder.addNum(2);    // arr = [1, 2]  
medianFinder.findMedian(); // 返回 1.5 ((1 + 2) / 2)  
medianFinder.addNum(3);    // arr[1, 2, 3]  
medianFinder.findMedian(); // return 2.0  
>思路：1.左边是最大堆 2.右边是最小堆 3.分割线一分为二 4.如果左堆大于右堆 直接返回max_stack[0],否则返回两个堆顶元素平均值
```
class MedianFinder:
    def __init__(self):
        self.maxx=[] #左大堆
        self.minn=[] #右小堆
        
    def addNum(self, num: int) -> None:
        if len(self.maxx)==len(self.minn):
            heappush(self.maxx,-heappushpop(self.minn,num))
        else:
            heappush(self.minn,-heappushpop(self.maxx,-num))
        
    def findMedian(self) -> float:
        if len(self.maxx)>len(self.minn):
            return -self.maxx[0]
        else:
            return (self.minn[0]-self.maxx[0])/2
```

- 347.前k个高频元素
给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。  
示例 1:  
输入: nums = [1,1,1,2,2,3], k = 2  
输出: [1,2]  
>思路：用Counter计算nums的计算个数，找出cnt.values()的最大值，即为buckets大小的边界。将出现次数相同的元素加入桶中，倒叙添加到res里，如果res个数等于k，返回即为res。

```
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        #先Count nums里面的元素出现次数
        #找出出现次数最大的值即为桶的大小
        #将值放到桶里
        #倒序将res加入桶
        cnt=Counter(nums)
        maxx=max(cnt.values())
        res=[]
        buckets=[[] for _ in range(maxx+1)]
        for value,count in cnt.items():
            buckets[count].append(value)
        for bucket in reversed(buckets):
            res+=bucket
            if len(res)==k:
                return res

```

- 215.数组中的第K个最大元素
给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。  
请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。  
你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。  
示例 1:  
输入: [3,2,1,5,6,4], k = 2  
输出: 5  
>思路:大顶堆,先建堆,再删除k-1个，最后返回max_heap[0]。

```
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        #大顶堆
        max_heap=[]
        for val in nums:
            heapq.heappush(max_heap,-val)
        for i in range(k-1):
            heapq.heappop(max_heap)
        return -max_heap[0]

```

- 84.柱状图中最大的矩形
给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。  
求在该柱状图中，能够勾勒出来的矩形的最大面积。  
输入：heights = [2,1,5,6,2,3]  
输出：10  
解释：最大的矩形为图中红色区域，面积为10 
>思路：左右单调栈，左栈默认边界为-1，右栈默认边界为n。计算每个index的最大边界面积，返回最大的面积。

```

```

- 739.每日温度
给定一个整数数组 temperatures ，表示每天的温度，返回一个数组 answer ，其中 answer[i] 是指对于第 i 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 0 来代替。  
示例 1:  
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
>思路： 单调栈从右向左遍历。如果cursor值大于或等于栈最后一个，将该值弹出。否则，res即为栈最后一个坐标减去当前坐标。

```
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        #从右向左 单调栈越来越小
        stack=[]
        n=len(temperatures)
        res=[0]*n
        for i in range(n-1,-1,-1):
            while stack and temperatures[i]>=temperatures[stack[-1]]:
                stack.pop()
            if stack:
                res[i]=stack[-1]-i
            stack.append(i)
        return res

```

- **4.寻找两个正序数组的中位数**
给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的 中位数 。  
算法的时间复杂度应该为 O(log (m+n)) 。  
示例 1：  
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
>思路: 二分查找，如果m+n是奇数,即查找第(m+n+1)//2的数，否则返回(m+n)//2+(m+n)//2+1的平均数，查找方法主要是比较两个数组第k//2-1值的大小,依次去除k值。

```
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        def getK(k):#k是第k大的值
            start1,start2=0,0
            while True:
                if start1==m:
                    return nums2[start2+k-1]
                if start2==n:
                    return nums1[start1+k-1]
                if k==1:
                    return min(nums1[start1],nums2[start2])
                _start1=min(m-1,start1+k//2-1)    
                _start2=min(n-1,start2+k//2-1)
                pivot1=nums1[_start1]
                pivot2=nums2[_start2]
                if pivot1<=pivot2:
                    k-=_start1-start1+1
                    start1=_start1+1
                else:
                    k-=_start2-start2+1
                    start2=_start2+1
                
        m,n=len(nums1),len(nums2)
        total=m+n
        if total%2==1:
            return getK((total+1)//2)
        else:
            return (getK(total//2)+getK(total//2+1))/2.0

```
- 34.在排序数组中查找元素的第一个和最后一个位置
给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。  
如果数组中不存在目标值 target，返回 [-1, -1]。  
你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题。  
示例 1：  
输入：nums = [5,7,7,8,8,10], target = 8  
输出：[3,4]  
>思路：用二分查找寻找某个值的首次出现时间，最后一个位置用low_bound(nums,target+1)-1表示。需要提前处理好找不到的情况。

```
class Solution:
    def find_first(self,nums,target):
        left,right=0,len(nums)-1
        while(left<=right):
            mid=(left+right)//2
            if target<=nums[mid]:
                right=mid-1
            else:
                left=mid+1
        return left

    def searchRange(self, nums: List[int], target: int) -> List[int]:
        #二分查找
        start=self.find_first(nums,target)
        if start==len(nums) or nums[start]!=target:
            return [-1,-1]
        end=self.find_first(nums,target+1)-1
        return [start,end]

```

- 35.搜索插入位置
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。  
请必须使用时间复杂度为 O(log n) 的算法。  
示例 1:  
输入: nums = [1,3,5,6], target = 5  
输出: 2  

> 思路：二分查找最好使用`while(left<=right)`进行判断，最后返回left值。

```
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        #二分查找
        def find_index(left,right,target):
            while left<=right:
                mid=(left+right)//2
                if nums[mid]<target:
                    left=mid+1
                else:
                    right=mid-1
            return left
        return find_index(0,len(nums)-1,target)
```
- **39.组合总和**
给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 candidates 中可以使数字和为目标数 target 的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。  
candidates 中的 同一个 数字可以 无限制重复被选取 。如果至少一个数字的被选数量不同，则两种组合是不同的。   
对于给定的输入，保证和为 target 的不同组合数少于 150 个。  
示例 1：  
输入：candidates = [2,3,6,7], target = 7  
输出：[[2,2,3],[7]]  
解释：  
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。  
7 也是一个候选， 7 = 7 。  
仅有这两种组合。  
>思路：1.将candidate进行排序。2.dfs遍历时target表示目标数，当candidates[j]>target,break退出。3.path[:]表示路径所有值。4.返回结果dfs(0,target)
```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        #为了预防重复 需先sort
        candidates.sort()
        res=list()
        ans=list()
        n=len(candidates)
        def dfs(index,target):
            if target==0:
                res.append(ans[:])
            else:
                for index1 in range(index,n):
                    if candidates[index1]>target:
                        break
                    ans.append(candidates[index1])
                    dfs(index1,target-candidates[index1])
                    ans.pop()
        dfs(0,target)
        return res

```

- 17.电话号码的字母组合
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。  
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。  
示例 1：  
输入：digits = "23"  
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]  
>思路:深搜递归，用map映射电话号码。从0开始遍历到len(digits),返回结果res。

```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []
        res=list()
        _str=list()
        _map={
            "2":"abc",
            "3":"def",
            "4":"ghi",
            "5":"jkl",
            "6":"mno",
            "7":"pqrs",
            "8":"tuv",
            "9":"wxyz"
        }
        def dfs(index):
            if index==len(digits):
                res.append(''.join(_str))
            else:
                _str1=digits[index]
                for _str2 in _map[_str1]:
                    _str.append(_str2)
                    dfs(index+1)
                    _str.pop()
        dfs(0)
        return res
```

- **78.子集**
给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。  
解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。  
示例 1：  
输入：nums = [1,2,3]  
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]  
>思路：深搜，用index表示坐标，用tmp表示结果。
```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res=[]
        n=len(nums)
        def dfs(index,tmp):
            res.append(tmp)
            for i in range(index,n):
                dfs(i+1,tmp+[nums[i]])
        dfs(0,[])
        return res

```
- **46.全排列**  
给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。  
示例 1：  
输入：nums = [1,2,3]    
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]  
>思路：用nums和tmp,if nums为空，说明数取完了。
```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res=[]
        def dfs(nums,tmp):
            if not nums: 
                res.append(tmp)
            for i in range(len(nums)):
                dfs(nums[:i]+nums[i+1:],tmp+[nums[i]])
        dfs(nums,[])
        return res

```

- **207.课程表**
你这个学期必须选修 numCourses 门课程，记为 0 到 numCourses - 1 。
在选修某些课程之前需要一些先修课程。 先修课程按数组 prerequisites 给出，其中 prerequisites[i] = [ai, bi] ，表示如果要学习课程 ai 则 必须 先学习课程  bi 。
例如，先修课程对 [0, 1] 表示：想要学习课程 0 ，你需要先完成课程 1 。
请你判断是否可能完成所有课程的学习？如果可以，返回 true ；否则，返回 false 。
示例 1：
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。
>思路： 1.拓扑排序判断是否为环。2.用0，1，2表示三种状态。3.用哈希表建邻接表。4.用stack存结果
```
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        #拓扑排序判断是否为环
        #用0，1，2表示三种状态
        #用哈希表建邻接表
        #用stack存结果
        status=[0]*numCourses
        edges=defaultdict(list)
        stack=[]
        self.res=True
        #建边 
        for cur,pre in prerequisites:
            edges[pre].append(cur)
        #找环
        def dfs(u):
            status[u]=1
            for v in edges[u]:
                if self.res and status[v]==0:
                    dfs(v)
                elif status[v]==1:
                    self.res=False
            status[u]=2
            stack.append(u)

        for i in range(numCourses):
            if self.res and status[i]==0:
                dfs(i)
        return self.res 
```
- **124.二叉树中的最大路径和**
二叉树中的 路径 被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。  
路径和 是路径中各节点值的总和。  
给你一个二叉树的根节点 root ，返回其 最大路径和 。  
示例 1：  
输入：root = [1,2,3]   
输出：6  
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6  
>思路：用递归的方法,left计算左子树的值，right计算右子树的值，遍历每个结点，返回最大的self.res。
```
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        self.res=-inf
        def dfs(root):
            if not root: return 0
            left=max(dfs(root.left),0)
            right=max(dfs(root.right),0)
            self.res=max(self.res,root.val+left+right)
            return root.val+max(left,right)
        dfs(root)
        return self.res


```

- **236.二叉树的最近公共祖先**
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。  
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”  
示例 1：  
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1  
输出：3  
解释：节点 5 和节点 1 的最近公共祖先是节点 3。  
>思路：1.递归判断p、q是否在左右子结点。2.如果p、q分布在左右子树,则公共结点是root,如果左结点没有，则返回右子树递归，如果右结点没有，则返回左子树递归。

```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':

        if not root or root==p or root==q:
            return root
        left=self.lowestCommonAncestor(root.left,p,q)
        right=self.lowestCommonAncestor(root.right,p,q)
        if not left:
            return right
        if not right:
            return left
        return root

```

- **437.路径总和III**
给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。  
路径 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。  
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8  
输出：3  
解释：和等于 8 的路径有 3 条，如图所示。  
>思路：典型的递归题，只需考虑以root为首的和不以root为首的例子即可。
```
class Solution:
    def pathSum(self, root: TreeNode, targetSum: int) -> int:
        def rootSum(root,targetSum):
            if not root: return 0
            res=0
            if root.val==targetSum:
                res+=1
            res+=rootSum(root.left,targetSum-root.val)
            res+=rootSum(root.right,targetSum-root.val)
            return res

        if not root: return 0
        res=rootSum(root,targetSum)
        res+=self.pathSum(root.left,targetSum)
        res+=self.pathSum(root.right,targetSum)
        return res

```

- **105.从前序与中序遍历序列构造二叉树**
给定两个整数数组 preorder 和 inorder ，其中 preorder 是二叉树的先序遍历， inorder 是同一棵树的中序遍历，请构造二叉树并返回其根节点。
示例 1:  
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]  
>思路：1.前序遍历的第一个node可把中序遍历分为两个part,根结点即为该node。2.用哈希表存放中序列表的坐标。

```
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        #Inorder很关键
        #前序遍历的第一个node可把中序遍历分为两个part,根结点即为该node
        n=len(inorder)
        dict={}
        for i in range(n):
            dict[inorder[i]]=i
        def dfs(index,left,right):
            if left>right: return
            mid=dict[preorder[index]]
            node=TreeNode(preorder[index])
            node.left=dfs(index+1,left,mid-1)
            node.right=dfs(index+mid-left+1,mid+1,right)
            return node
        return dfs(0,0,n-1)

```

- **108.将有序数组转换为二叉搜索树**
给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 平衡 二叉搜索树。  
示例 1：  
输入：nums = [-10,-3,0,5,9]  
输出：[0,-3,9,-10,null,5]  
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案  
>思路:将有序数组转换为平衡二叉树。递归建树结点,并建立连接。 

```
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        #根据数组建二叉树
        n=len(nums)
        def dfs(left,right):
            if left>right:
                return 
            mid=(left+right)//2
            root=TreeNode(nums[mid])
            root.left=dfs(left,mid-1)
            root.right=dfs(mid+1,right)
            return root
        return dfs(0,n-1)

```

- **543.二叉树的直径**
给你一棵二叉树的根节点，返回该树的 直径 。  
二叉树的 直径 是指树中任意两个节点之间最长路径的长度。这条路径可能经过也可能不经过根节点root。  
两节点之间路径的 长度 由它们之间边数表示。  
示例 1：  
输入：root = [1,2,3,4,5]  
输出：3  
解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。  
>思路：递归，注意用全局变量self.res表示结果。用dfs计算二叉树的深度。
```
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.res=0
        def dfs(node):
            if not node: return 0
            L=dfs(node.left)
            R=dfs(node.right)
            self.res=max(self.res,L+R)
            return max(L,R)+1
        dfs(root)
        return self.res

```

- **94.二叉树的中序遍历**
给定一个二叉树的根节点root,返回它的中序遍历。  
示例 1：  
输入：root = [1,null,2,3]  
输出：[1,3,2]  
>思路：1.dfs 2.栈中序遍历 3.标记法

```
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res=[]
        white,gray=0,1
        stack=[(white,root)]
        while stack:
            color,node=stack.pop()
            if not node: continue
            if color==white:
                stack.append((white,node.right))
                stack.append((gray,node))
                stack.append((white,node.left))
            else:
                res.append(node.val)
        return res


```

- **148.排序链表**
给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。  
示例 1：  
输入：head = [4,2,1,3]  
输出：[1,2,3,4]  
>思路：递归+快慢指针+归并排序，需注意定义两段指针起始结点，mid,slow.next=slow.next,None。
```
class Solution:
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre=res=ListNode(-1)
        #递归+快慢指针
        if not head or not head.next: return head
        slow,fast=head,head.next
        while fast and fast.next:
            slow=slow.next
            fast=fast.next.next
        mid,slow.next=slow.next,None
        left,right=self.sortList(head),self.sortList(mid)
        while left and right:
            if left.val<right.val:
                res.next=left
                left=left.next
            else:
                res.next=right
                right=right.next
            res=res.next
        res.next=left if left else right
        return pre.next

```
- 25.K个一组翻转链表
给你链表的头节点 head ，每 k 个节点一组进行翻转，请你返回修改后的链表。  
k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。  
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。  
输入：head = [1,2,3,4,5], k = 2  
输出：[2,1,4,3,5]  
>思路：1.用stack先进后出的特性进行存储。2.

```
#1.用stack先进后出的特性
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        if not head or not head.next:
            return head
        #用stack先进后出特性做
        pre=res=ListNode(-1)
        stack=[]
        while True:
            cnt=0
            tmp=head
            #将链表结点推入栈
            while tmp and cnt!=k:
                stack.append(tmp)
                tmp=tmp.next
                cnt+=1         
            #剩余的达不到翻转链表
            if cnt!=k:
                res.next=head
                break  
            #开始翻转
            while cnt:
                res.next=stack.pop()
                res=res.next
                cnt-=1
            #此时head是未翻转链表的头结点
            head=tmp
        return pre.next

#2.递归法


```

- 24.两两交换链表中的节点
给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。  
示例 1：  
输入：head = [1,2,3,4]  
输出：[2,1,4,3]  
>思路：用栈处理链表节点，需注意在移动链表节点时，需在节点入栈前移动，若在节点入栈后移动会引发错误。
```
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        #用栈存放链表结点
        if not head or not head.next:
            return head
        res=p=ListNode(-1)
        stack=[]
        while head and head.next:
            stack.append(head)
            stack.append(head.next)
            head=head.next.next
            p.next=stack.pop()
            p.next.next=stack.pop()
            p=p.next.next
            
        if head:
            p.next=head
        else:
            p.next=None
        return res.next

```
- **2.两数相加**
给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。  
请你将两个数相加，并以相同形式返回一个表示和的链表。  
你可以假设除了数字 0 之外，这两个数都不会以 0 开头。  
输入：l1 = [2,4,3], l2 = [5,6,4]  
输出：[7,0,8]  
解释：342 + 465 = 807.  
>思路：新建链表节点，用temp存储链表相加值，更新temp大小。

```
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        #生成一个新链表用于存储结果
        #用temp存储value值
        pre=res=ListNode()
        temp=0
        while l1 or l2 or temp:
            if l1:
                temp+=l1.val
                l1=l1.next
            if l2:
                temp+=l2.val
                l2=l2.next
            res.next=ListNode(temp%10)
            temp//=10
            res=res.next
        return pre.next

```

- 21.合并两个有序链表
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。   
示例 1：  
输入：l1 = [1,2,4], l2 = [1,3,4]  
输出：[1,1,2,3,4,4]  
>思路：递归法，注意递归的顺序，if l1.val < l2.val: l1.next=self.mergeTwolist(l1.next,l2)。
```
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        #递归做法
        if not list1:
            return list2
        if not list2:
            return list1
        if list1.val<list2.val:
            list1.next=self.mergeTwoLists(list1.next,list2)
            return list1
        else:
            list2.next=self.mergeTwoLists(list1,list2.next)
            return list2

```

- 438.找到字符串中所有字母异味词
给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。  
示例 1:  
输入: s = "cbaebabacd", p = "abc"  
输出: [0,6]  
解释:  
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。  
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。  
>思路：滑动窗口经典例题，双指针left、right。找异位词首先是cnt=Counter(p)计算p的字符个数。迭代s时依次减少cnt的个数。
```
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        res=[]
        n=len(s)
        left=0
        cnt=Counter(p)
        for right in range(n):
            cnt[s[right]]-=1
            while cnt[s[right]]<0:
                 cnt[s[left]]+=1
                 left+=1
            if right-left+1==len(p):
                res.append(left)
        return res
```

- 42.接雨水
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。  
示例 1：  
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]  
输出：6  
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。  
>思路：对每一个柱子判断leftmax和rightmax值大小，取最小值减去该柱子高度即为能接多少雨水，最后依次迭代每个柱子的雨水并累积。返回结果值。
```
class Solution:
    def trap(self, height: List[int]) -> int:
        #接雨水
        sum=0
        n=len(height)
        leftmax=[height[0]]+[0]*(n-1)
        rightmax=[0]*(n-1)+[height[n-1]]
        for i in range(1,n):
            leftmax[i]=max(height[i],leftmax[i-1])
        for j in range(n-2,-1,-1):
            rightmax[j]=max(rightmax[j+1],height[j])
        for i in range(n):
            sum+=(min(leftmax[i],rightmax[i])-height[i])
        return sum

```

- 15.三数之和
给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请你返回所有和为 0 且不重复的三元组。  
注意：答案中不可以包含重复的三元组。  
示例 1：  
输入：nums = [-1,0,1,2,-1,-4]  
输出：[[-1,-1,2],[-1,0,1]]  
>思路：先sort数组排序，用for循环逐个遍历left值。mid值往右遍历，同时right值向左遍历，当相遇即停止。
```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res=list()
        nums.sort()
        n=len(nums)
        for left in range(n):
            if left==0 or nums[left]!=nums[left-1]:
                target=-nums[left]
                right=n-1
                for mid in range(left+1,n):
                    if mid==left+1 or nums[mid]!=nums[mid-1]:
                        while mid<right and nums[mid]+nums[right]>target:
                            right-=1
                        if mid==right:
                            break
                        if nums[mid]+nums[right]==target:
                            res.append([nums[left],nums[mid],nums[right]])
        return res

```

- 128.最长连续序列
给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。  
请你设计并实现时间复杂度为 O(n) 的算法解决此问题。  
示例 1：  
输入：nums = [100,4,200,1,3,2]  
输出：4  
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。  
>思路：用set去掉重复值，每次判断x时需判断x-1在不在序列里。
```
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        if not nums:
            return 0
        res=1
        set1=set(nums)
        for x in set1:
            if x-1 in set1:
                continue
            y=x+1
            while y in set1:
                res=max(res,y-x+1)
                y=y+1
        return res

```
- 49.字母异位词分组  
给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。  
示例 1:    
输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]  
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]  
解释：  
在 strs 中没有字符串可以通过重新排列来形成 "bat"。  
字符串 "nat" 和 "tan" 是字母异位词，因为它们可以重新排列以形成彼此。  
字符串 "ate" ，"eat" 和 "tea" 是字母异位词，因为它们可以重新排列以形成彼此。  
>思路：用defaultdict字典表示相同字符的列表，A=''.join(sorted(str))表示元字符。

```
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res=defaultdict(list)
        for str in strs:
            A=''.join(sorted(str))
            res[A].append(str)
        return list(res.values())

```


- 153.寻找旋转排序数组中的最小值
已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,2,4,5,6,7] 在变化后可能得到：  
若旋转 4 次，则可以得到 [4,5,6,7,0,1,2]  
若旋转 7 次，则可以得到 [0,1,2,4,5,6,7]  
注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]]。  
给你一个元素值 互不相同 的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素。  
你必须设计一个时间复杂度为 O(log n) 的算法解决此问题。  
示例 1：  
输入：nums = [3,4,5,1,2]  
输出：1  
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。  
>思路：判断mid与右边界的关系，如果nums[mid]小于右边界，right=mid。如果nums[mid]大于右边界，left=mid+1。
```
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left,right=0,len(nums)-1
        while left<right:
            mid=(left+right)//2
            if nums[mid]<nums[right]:
                right=mid
            else:
                left=mid+1
        return nums[left]
```

- **33.搜索旋转排序数组**
整数数组 nums 按升序排列，数组中的值 互不相同 。  
在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。  
给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。  
你必须设计一个时间复杂度为 O(log n) 的算法解决此问题。  
示例 1：  
输入：nums = [4,5,6,7,0,1,2], target = 0  
输出：4  
>思路：首先判断mid是在左有序数组中还是在右有序数中。再判断target是在有序数组中还是在无序数组中，移动left或right。
```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left,right=0,len(nums)-1
        while (left<=right):
            mid=(left+right)//2
            if nums[mid]==target:
                return mid
            if nums[0]<=nums[mid]:
                if nums[0]<=target<nums[mid]:
                    right=mid-1
                else:
                    left=mid+1
            else:
                if nums[mid]<target<=nums[len(nums)-1]:
                    left=mid+1
                else:
                    right=mid-1
        return -1

```

- **51.N皇后**
按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。  
n 皇后问题 研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。  
给你一个整数 n ，返回所有不同的 n 皇后问题 的解决方案。  
每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。  
示例 1：  
输入：n = 4  
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]  
解释：如上图所示，4 皇后问题存在两个不同的解法。  
>思路：N皇后问题主要考虑判别原理，纵轴、左斜对角、右斜对角都无任何皇后，用True或False进行标记。深搜用dfs(row=0)进行，用mark进行标记。
```
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        #n表示表盘有多大
        #res输出表示输出结果 
        #用mark=[0]*n可以对表进行标记
        #思路：放置的棋子不能有同行同列或正斜负斜，处理正斜是x+y，负斜是x-y
        #正斜1+3 2+2 3+1 diag1[x+y]=1
        #负斜2-1 3-2 4-3 diag2[x-y]=1
        #用col=[False]*n对纵轴进行标记
        #用深搜的思路 从dfs(0)开始遍历横轴开始 当遍历到n即停止 输出结果res
        res=[]
        mark=[0]*n
        col=[False]*n
        diag1=[False]*(2*n-1) #x+y
        diag2=[False]*(2*n-1) #x-y
        def dfs(row):
            if row==n:
                res.append(['.'*c+'Q'+'.'*(n-c-1) for c in mark])
            for index,tag in enumerate(col):
                if not tag and not diag1[row+index] and not diag2[row-index+(n-1)]:
                    mark[row]=index
                    col[index]=diag1[row+index]=diag2[row-index+(n-1)]=True
                    dfs(row+1)
                    col[index]=diag1[row+index]=diag2[row-index+(n-1)]=False
        dfs(0)
        return res

```

- **131.分割回文串**
给你一个字符串 s，请你将 s 分割成一些 子串，使每个子串都是 回文串 。返回 s 所有可能的分割方案。  
示例 1：  
输入：s = "aab"  
输出：[["a","a","b"],["aa","b"]]  
>#经典递归
```
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        
        n=len(s)
        res=[]
        path=[]
        def dfs(i):
            if i==n:
                res.append(path.copy())
                return
            for j in range(i,n):
                t=s[i:j+1]
                if t==t[::-1]:
                    path.append(t)
                    dfs(j+1)
                    path.pop()
        dfs(0)
        return res
```

- **79.单词搜索**
给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。  
单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。  
示例 1：  
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"  
输出：true  
>思路： #可能在任一一点dfs
        #dfs需设置三个参数，x,y坐标和数组k值。
        #dfs时有上下左右四个方向。
        #设置边界 继续递归。
        #当visit时需标记，访问完需恢复标记。
        #优化，首部->尾部和尾部->首部一样
        #如果尾部元素个数明显小于首部元素，则优先访问尾部元素。
```
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        #深搜从0到len(word)
        m=len(board)
        n=len(board[0])
        #找到起点可以上下左右移动，经过的点进行标记，用dfs(i+1,j)
        #遍历表盘所有单元格
        def dfs(i,j,k):
            if not 0<=i<m or not 0<=j<n or not word[k]==board[i][j]: return False
            if k==len(word)-1: return True
            board[i][j]=""
            res=dfs(i+1,j,k+1) or dfs(i,j+1,k+1) or dfs(i-1,j,k+1) or dfs(i,j-1,k+1)
            board[i][j]=word[k]
            return res
        
        first,last=0,0
        #优化 选第一个或最后一个最小的数开始找
        for i in range(m):
            for j in range(n):
                if board[i][j]==word[0]:
                    first+=1
                elif board[i][j]==word[-1]:
                    last+=1
        if last<first:
            word=word[::-1]

        for i in range(m):
            for j in range(n):
                if dfs(i,j,0): return True
        return False

```

- 22.括号生成
数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。  
示例 1：  
输入：n = 3  
输出：["((()))","(()())","(())()","()(())","()()()"]  
>思路：dfs时，添加left和right参数
```
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        res=[]
        def dfs(path,left,right):
            if left>n or right>left: return 
            if len(path)==n*2:
                res.append(path)
                return
            dfs(path+'(',left+1,right)
            dfs(path+')',left,right+1)
        dfs('',0,0)
        return res

```

- 39.组合总和
给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 candidates 中可以使数字和为目标数 target 的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。  
candidates 中的 同一个 数字可以 无限制重复被选取 。如果至少一个数字的被选数量不同，则两种组合是不同的。   
对于给定的输入，保证和为 target 的不同组合数少于 150 个。  
示例 1：  
输入：candidates = [2,3,6,7], target = 7  
输出：[[2,2,3],[7]]  
解释：  
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。  
7 也是一个候选， 7 = 7 。  
仅有这两种组合。  
>思路：1.将candidate进行排序。2.dfs遍历时target表示目标数，当candidates[j]>target,break退出。3.path[:]表示路径所有值。4.返回结果dfs(0,target)
```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        #为了预防重复 需先sort
        candidates.sort()
        res=list()
        ans=list()
        n=len(candidates)
        def dfs(index,target):
            if target==0:
                res.append(ans[:])
            else:
                for index1 in range(index,n):
                    if candidates[index1]>target:
                        break
                    ans.append(candidates[index1])
                    dfs(index1,target-candidates[index1])
                    ans.pop()
        dfs(0,target)
        return res

```

- 78.子集
给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。  
解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。  
示例 1：  
输入：nums = [1,2,3]  
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]  
>思路：递归深搜，从index=0开始向后扫描，每次将扫描值加入到res里。
```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res=[]
        n=len(nums)
        def dfs(index,tmp):
            res.append(tmp)
            for j in range(index,n):
                dfs(j+1,tmp+[nums[j]])
        dfs(0,[])
        return res

```
- 46.全排列
给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。  
示例 1：  
输入：nums = [1,2,3]  
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]  
>思路:很明显的深搜，用tmp逐个存放值，最后用res添加。
```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res=[]
        def dfs(nums,tmp):
            if not nums:
                res.append(tmp)
            for i in range(len(nums)):
                dfs(nums[:i]+nums[i+1:],tmp+[nums[i]])
        dfs(nums,[])
        return res
        
```

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
        #f[0][0]=0
        n=len(coins)
        coins.sort()
        f=[[inf]*(amount+1) for _ in range(n+1)]
        f[0][0]=0

        for i in range(1,len(coins)+1):
            for j in range(amount+1):
                if coins[i-1]>j:
                    f[i][j]=f[i-1][j]
                else:
                    f[i][j]=min(f[i-1][j],f[i][j-coins[i-1]]+1)
        return f[n][amount] if f[n][amount]<inf else -1


```

- **279.完全平方数**
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

- **45.跳跃游戏II**
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

- **240.搜索二维矩阵II**
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
#1.各回各家
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

#2.哈希表
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        n=len(nums)
        dicc=defaultdict(int)
        for i in range(n):
            dicc[nums[i]]=1
        for i in range(1,n+1):
            if dicc[i]!=1:
                return i
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
#1.利用reverse函数
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

#2.利用数组特性
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        n=len(nums)
        k=k%n
        nums[0:n]=nums[n-k:n]+nums[0:n-k]

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

- **53.最大子数和**
给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。  
子数组是数组中的一个连续部分。  
示例 1：  
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]  
输出：6  
解释：连续子数组 [4,-1,2,1] 的和最大，为 6。  
>思路：动态规划，写出状态方程，dp[i]=dp[i-1]+nums[i],因为是求最大子数组和，所以需判断dp[i-1]是否大于0，如果小于，则dp[i]=nums[i]。

```
#1.动态方程
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

#2.前缀和差分，找出特例
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        res=-inf
        minn=0
        maxx=-inf
        n=len(nums)
        if n==1:
            return nums[0]
        dp=[0]*(n+1)
        for i in range(n):
            dp[i+1]=dp[i]+nums[i]
            minn=min(minn,dp[i+1])
            res=max(res,dp[i+1]-minn)
        for i in range(n):
            maxx=max(maxx,nums[i])
        return res if maxx>0 else maxx

```

- **76.最小覆盖子串**
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
        n=len(s)
        left=0
        dicc=defaultdict(int)

        for i in range(len(t)):
            dicc[t[i]]+=1
        n1=len(dicc)

        for right,value in enumerate(s):
            if value in t:
                dicc[value]-=1
                if dicc[value]==0:
                    n1-=1
                    while n1==0:
                        while not res or right-left+1<len(res):
                            res=s[left:right+1]
                        if s[left] in t:
                            dicc[s[left]]+=1
                            if dicc[s[left]]>0:
                                n1+=1
                        left+=1
        return res

```

- **239.滑动窗口最大值**
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
- **560.和为K的子数组**  
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

- **3. 无重复字符的最长子串**  
给定一个字符串 s ，请你找出其中不含有重复字符的 最长 子串 的长度。  
示例 1:  
输入: s = "abcabcbb"  
输出: 3   
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。  
> 思路：左右双指针，注意set用法，sset.add()和sset.remove()。

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


















