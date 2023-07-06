# 2023年7月6日
## 1. #2178 【贪心/数学】
https://leetcode.cn/problems/maximum-split-of-positive-even-integers/  
贪心模拟  
## 2. #270  【树】
https://leetcode.cn/problems/closest-binary-search-tree-value/?envType=study-plan-v2&envId=premium-algo-100  
bfs层序遍历BST  
## 3. #272  【树/二分】
https://leetcode.cn/problems/closest-binary-search-tree-value-ii/?envType=study-plan-v2&envId=premium-algo-100  
利用中序遍历性质（左根右），一次遍历记录排序好的bst节点，再利用二分法找出最接近target的索引，然后用双指针找左右边界    
## 4. #255 【树/单调栈】 
https://leetcode.cn/problems/verify-preorder-sequence-in-binary-search-tree/?envType=study-plan-v2&envId=premium-algo-100  
 判断是否为BST。前序遍历（根左右）记录的数组有如下特点：局部递减（左子树），整体递增（左子树结束了 开始右子树。 本题使用单调（递减）栈  维护一个递减的stack，每当有新元素new要进stack时和top元素比较，如果top比new小，top出栈，更新【出栈最小值】为top，直到top大于new或者stack为空，由于这样递减stack，所以每次出栈更新的【出栈最小值】会大于之前的。遇到new小于【出栈最小值】情况时，即可判断BT不是BST，因为已经出栈的元素一定在当前new的左边，根据BST性质，左边的要小于右边的，所以可以做出判断。  
举例： [5,2,1,3,6], 3 开始是右子树，大于左子树的1,2,利用单调栈将1,2,出栈。 随后的6是更上一层的右子树， 大于左边的3，5，利用单调栈将3，5出栈。   
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  preorder=[5,2,6,1,3]  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; stk1 = [5,2] minv=-inf  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; stk1 = [6] | 5,2出栈 minv = 5  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   遍历到元素 1，小于minv 5，做出判断，不是BST

## 5. #496 【单调栈】  
https://leetcode.cn/problems/next-greater-element-i/  
**【单调栈小技巧】单调栈中存放的元素最好是下标而不是值，因为有的题目需要根据下标计算，这样泛化性更好**  


## 6. #503 【单调栈】  
https://leetcode.cn/problems/next-greater-element-ii/  
**【单调栈小技巧】遇到循环数组，将原始数组“翻倍”，就是在后面再接一个原始数组，每个元素不仅可以比较自己右边的元素，而且也可以和左边的元素比较了。真实情况中不需要重新构造数组，而是利用遍历 下标取模len(arr)来模拟[0, 2xlen(arr)]** 

## 7. #853 【数组/排序/单调栈】  
https://leetcode.cn/problems/car-fleet/  
根据题目意思，需要先进行排序，抽象为单调栈算法。  
某些问题，先进行排序后，可以有更好的解决方式。本身不难，如果想到排序，很容易解。  

## 8 #1214 【树/二分/dfs/bfs/stack】  
https://leetcode.cn/problems/two-sum-bsts/?envType=study-plan-v2&envId=premium-algo-100  
方法1： 死做，遍历2棵树，记录数据到两个数组，然后查找。没用上BST性质。  
方法2： 中序遍历记录值到数组，因为是中序，所以已经排好序了。找一个值，双指针夹逼定理。

## 9 #277 【图/贪心】
https://leetcode.cn/problems/find-the-celebrity/?envType=study-plan-v2&envId=premium-algo-100  
方法1：直接利用题目中描述的性质，A知道B, A->B. 名人定义为别人认识她，她不认识别人，即找出度为0，且入度为n的节点。但是复杂度太高超时。  
方法2：利用名人的定义，A知道B， B 不知道A, B才是名人。所以两个节点互相比较，总能去掉一个人。如果互相不认识，直接返回-1。  

## 10 #323 【并查集/ bfs/ dfs】
https://leetcode.cn/problems/number-of-connected-components-in-an-undirected-graph/?envType=study-plan-v2&envId=premium-algo-100  
方法1：一眼并查集！并查集代码默写：
```python3
class UnionFind():
    def __init__(self, n):
        self.n = n
        self.parent = list(range(n))
        self.rank = [1 for _ in range(n)]
        self.cnt = n
    
    def find(self, x):
        # 找到最上面的根
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(x,y):
        rootX, rootY = self.find(x), self.find(y)
        if rootX == rootY:
            #同根，不需要合并了
            return False
        # 保持rootX的rank 比 rootY 低，因为想把rootX 并到rootY上
        if self.rank[rootX] > self.rank[rootY]:
            rootX, rootY = rootY, rootX
        self.parent[rootX] = rootY
        self.rank[rootY] += self.rank[rootX]
        # 2合1， group数量少一个了
        self.cnt -= 1
        # 并！
        return True 
    
    # 辅助方程/非必要
    def have_same_parent(x,y):
        return self.parent[x] == self.parent[y]
    
    # 辅助方程/非必要
    def get_group_size(x):
        rootX = self.find(x)
        return self.rank[rootX]
```
方法2：DFS. 用接邻表建图，设置visit数组。  
&emsp;&emsp;&emsp;&nbsp;&nbsp;从头开始dfs，如果没visit过，dfs一下，连接所有能连接到的节点，计数+1。  
&emsp;&emsp;&emsp;&nbsp;&nbsp;全部过一遍之后，即可知道有几个分组。

方法3：BFS. 和DFS一模一样，只是方法换成了BFS
