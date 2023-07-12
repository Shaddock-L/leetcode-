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


# 2023年7月7日

## 1 #2532. 过桥的时间 【模拟/优先队列】
https://leetcode.cn/problems/time-to-cross-a-bridge/description/
描述很长，题目很长，很复杂的模拟题，看完没用做的欲望。能想到是用priority queue但是条件好多好复杂。对照答案进行理解。  
参考ylb
```python3
class Solution:
    def findCrossingTime(self, n: int, k: int, time: List[List[int]]) -> int:
        #time[i] = [leftToRighti, pickOldi, rightToLefti, putNewi]

        #根据效率进行排序
        time.sort(key=lambda x : x[0] + x[2])
        cur = 0
        # 工人开始从左岸出发，0:过桥到达右岸，1：拿取箱子，2：从右边走回来, 3：放下箱子等待新一轮从左到右
        # wait中存放工人的下标索引
        wait_in_left, wait_in_right = [], []
        # work中存放(上桥前的时间, 工人下标) 这两个是小根堆，因为时间在前面的先上桥（出队）
        work_in_left, work_in_right = [], []
        #效率低的工人先走，因为工作速度相同时，编号越大效率越低，所以取负，优先出队（大根堆)
        for i in range(k):
            heapq.heappush(wait_in_left, -i)
        
        while 1:
            while work_in_left:
                t, i = work_in_left[0]
                if t > cur:
                    break   
                
                heapq.heappop(work_in_left)
                heapq.heappush(wait_in_left, -i)
            while work_in_right:
                t, i = work_in_right[0]
                if t > cur:
                    break   
                heapq.heappop(work_in_right)
                heapq.heappush(wait_in_right, -i)

            left_to_go = n > 0 and wait_in_left
            # right_to_go = bool(wait_in_right)
            right_to_go = len(wait_in_right) > 0

            if not left_to_go and not right_to_go:
                nxt = inf 
                if work_in_left:
                    nxt = min(nxt, work_in_left[0][0])
                if work_in_right:
                    nxt = min(nxt, work_in_right[0][0])
                cur = nxt 
                continue 
            if right_to_go:
                i = - heapq.heappop(wait_in_right)
                cur += time[i][2]
                if n == 0 and not wait_in_right and not work_in_right:
                    return cur
                heapq.heappush(work_in_left,(cur + time[i][3], i))
            else:
                i = -heapq.heappop(wait_in_left)
                cur += time[i][0]
                n -= 1
                heapq.heappush(work_in_right,(cur + time[i][1],i))
```

## 2 #723【矩阵/模拟/双指针】
https://leetcode.cn/problems/candy-crush/?envType=study-plan-v2&envId=premium-algo-100   
mark时可以标为相反数，这样既可以做到和普通数据不同，又可以利用abs来消除mark进行比较   
开心消消乐，硬模拟  
【1】先看行也没有能消除的，标记为相反数  
【2】再看列，也没有能消除的，标记为相反数  
【3】。因为题目规定数据为正数，所以再遍历一遍，看到负数的时候把它消掉，然后让剩余数据顺势掉下来补充即可。  遍历每一列，逐行下落，从最底下一行开始，随后把剩余的空间置零。  
```python3
class Solution:
    def candyCrush(self, board: List[List[int]]) -> List[List[int]]:
        m, n = len(board), len(board[0])
        changed = True

        while changed:
            changed = False 

            #横向
            for i in range(m):
                for j in range(n-2):
                    if abs(board[i][j]) == abs(board[i][j+1]) == abs(board[i][j+2]) != 0:
                        board[i][j] = board[i][j + 1] = board[i][j + 2] = -abs(board[i][j])
                        changed = True
            
            #纵向
            for i in range(m-2):
                for j in range(n):
                    if abs(board[i][j]) == abs(board[i+1][j]) == abs(board[i+2][j]) != 0:
                        board[i][j] = board[i+1][j] = board[i+2][j] = -abs(board[i][j])
                        changed = True
            
            for j in range(n):
                idx = m - 1
                for i in range(m-1,-1,-1):
                    if board[i][j] > 0:
                        board[idx][j] = board[i][j]
                        idx -= 1
                for i in range(idx, -1, -1):
                    board[i][j] = 0
        return board
```  
  
  
  
## 3 #772 【分治/递归/栈】  
https://leetcode.cn/problems/basic-calculator-iii/description/   
括号内的内容，分解为子问题，通过调用自己，计算括号内的内容，然后继续。要注意的是，不是看到第一个右括号就开始递归，而是找到配套的：左括号数目与右括号数目相同时，进入递归（确保括号配套）。  
实现上，我们可以用一个stack来记录数据，加减法直接进栈正负数，乘法除法直接修改栈顶元素。
```python3
class Solution:
    def calculate(self, s: str) -> int:
        stack = []
        num = 0
        sign = '+'
        i = 0

        while i < len(s):
            if s[i].isdigit():
                num = num*10 + int(s[i])
            
            #如果是 ( ，则找到与之对应的右边括号进行递归，
            if s[i] == "(":
                j = i + 1
                l,r = 1,0
                while l > r:
                    if s[j] == "(":
                        l += 1
                    elif s[j] == ")":
                        r += 1
                    j += 1
                num = self.calculate(s[i+1:j-1])
                i = j-1 
            
            #i == len(s) - 1表示，已经到最后一位数字，在跳出前需要进行计算
            if (not s[i].isdigit() and s[i] != ' ') or i == len(s) - 1:
                if sign == '+':
                    stack.append(num)
                elif sign == '-':
                    stack.append(-num)
                elif sign == '*':
                    stack[-1] *= num  
                elif sign == '/':
                    stack[-1] = int(stack[-1] / num)
                sign = s[i]
                num = 0
            i += 1
        return sum(stack)
            
```

## 3 #224 【分治/递归/栈】  
https://leetcode.cn/problems/basic-calculator/description/  
方法1：和上一题一样，只是少了乘除法，代码部分略。  
方法2：用两个栈来模拟。一个stack放数字，一个stack放operator。本质上和方法1最后算sum是一样的，只是换了个形式。更倾向于方法1，好理解而且简单。  

**【小技巧1】由于第一个数可能是负数，为了减少边界判断。一个小技巧是先往 nums 添加一个 0**。  

**【小技巧2】为防止 () 内出现的首个字符为运算符，将所有的空格去掉，并将 (- 替换为 (0-，(+ 替换为 (0+（当然也可以不进行这样的预处理，将这个处理逻辑放到循环里去做。**

```python3
class Solution:
    def calculate(self, s):
        nums = []
        #先导一个0，避免第一位是-的情况
        nums.append(0)
        #去掉s中的空格
        s = re.sub(r" *", "", s)
        ops = []
        def calc():
            nonlocal nums
            nonlocal ops 
            if len(nums) < 2 or len(ops) == 0:
                return 
            b = nums.pop()
            a = nums.pop()
            op = ops.pop()
            if op == '+':
                result = a + b
            else:
                result = a - b
            nums.append(result)
        
        n =  len(s)
        i = 0
        while i < n:
            c = s[i]
            if c == '(':
                ops.append(c)
            elif c == ')':
                while ops:
                    op = ops[-1]
                    if op != '(':
                        #括号内先做计算
                        calc()
                    else:
                        #最近的括号结束
                        ops.pop()
                        break 
            else:
                #数字
                if c.isdigit():
                    #找到连续数字字符，组成num
                    num = 0
                    j = i 
                    while j < n and s[j].isdigit():
                        num = num * 10 + int(s[j])
                        j += 1
                    nums.append(num)
                    i = j-1
                else:
                    # 加或者减
                    if i > 0 and (s[i-1] == "(" or s[i-1] == '+' or s[i-1] == '-'):
                        nums.append(0)
                    while ops and ops[-1] != '(':
                        calc()
                    ops.append(c)
            i += 1
        
        while ops:
            calc()
        return nums[-1]
```

## 4 #1490. 【树/hash Table/BFS/DFS】
https://leetcode.cn/problems/clone-n-ary-tree/description/?envType=study-plan-v2&envId=premium-algo-100  
方法1：比较典型的树递归。操作根节点，然后子节点返回调用原方程。
```python3
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []
"""

class Solution:
    def cloneTree(self, root: 'Node') -> 'Node':
        if not root:
            return root 
        newRoot = Node(root.val)
        newRoot.children = [self.cloneTree(n) for n in root.children]
        return newRoot
```

## 5 #1506 【树/BFS/DFS/位运算】
https://leetcode.cn/problems/find-root-of-n-ary-tree/description/?envType=study-plan-v2&envId=premium-algo-100  

  
方法1：死做，对每个节点进行bfs，看当前节点能不能遍历到所有节点（size == cnt)。复杂度 O(n^2) 超时
```python3
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []
"""

class Solution:
    def findRoot(self, tree: List['Node']) -> 'Node': 
        size = len(tree)
        def bfs(node):
            if node == None:
                return 0  
            ret = 1
            q = [node]
            while q:
                n = q.pop(0)
                for kid in n.children:
                    q.append(kid)
                    ret += 1
            return ret 
        for n in tree:
            if bfs(n) == size:
                return n   
        return 
```

方法2：利用树的性质，root不会出现在children里，所以遍历所有节点的children，加入一个set，再和原set取差集，剩下的就是root
```python3
class Solution:
    def findRoot(self, tree: List['Node']) -> 'Node': 
        s1 = set(tree)
        s2 = set()
        for node in tree:
            for kid in node.children:
                s2.add(kid)
        return list(s1-s2)[0]
```

方法3：除了根节点，其余节点都会在children里被遍历到，再结合原数组，可以消除原数组里的元素，最后就剩下root。用到位运算实现。此方法为方法2改良版。  
【前置知识】：a XOR a = 0  

```python3
class Solution:
    def findRoot(self, tree: List['Node']) -> 'Node': 
        xorSum = 0
        for n in tree:
            xorSum ^= n.val   
            for kid in n.children:
                xorSum ^= kid.val   
        for n in tree:
            if n.val == xorSum:
                return n
        return 
```

## 6 #1522   【树/BFS/DFS】
记录高度，最长路径是子节点中最max的2条高度的和。由于是N叉树，所以计算高度时取所有子节点中高度最高的+1。最后过该节点的最长2条（或只有1条）子节点高度的和就是最大路径长度。  
利用字典记录节点高度。  
```python3
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []
"""

class Solution:
    def diameter(self, root: 'Node') -> int:
        """
        :type root: 'Node'
        :rtype: int
        """
        #记录高度，最长路径是子节点中最max的2条高度的和
        #即通过该节点的，最大路径长度
        depthdic = defaultdict(int)

        def dfs(node):
            nonlocal depthdic
            if node == None:
                return 0 
            ret = [dfs(kid) for kid in node.children]
            if  len(ret) == 0:
                depth = 1 
            else:
                print(ret)
                depth = max(ret) + 1  
            depthdic[node] = depth   
            return depth
        dfs(root)
        ans = 0 
        q = [root]
        while q:
            node = q.pop(0)
            temp = []
            for kid in node.children:
                temp.append(depthdic[kid])
                q.append(kid)
            
            if len(temp) > 0:
                if len(temp) < 2:
                    ans = max(ans, temp[0])
                else:
                    ans = max(sum(heapq.nlargest(2, temp)), ans)
        return ans
```

# 2023年7月8日 \<Video Games Day\>


<table><tr><td bgcolor=yellow><font face="黑体" color=green size=5>周末摆烂偷懒😶</font></td></tr></table>

## 1 #167 【双指针/哈希表】
方法1：有序数组，直接双指针左右找  
方法2：哈希表记录，遇到target-current在表内时直接返回  
两种方法差不多，复杂度都是O(n)  
```python3
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l = 0
        r = len(numbers) - 1
        while l < r:
            if numbers[l] + numbers[r] == target:
                return [l+1,r+1]
            elif numbers[l] + numbers[r] < target:
                l += 1
            else:
                r -= 1
        return [0,0]

        """
        #方法2
        dic = {}
        for i, n in enumerate(numbers):
            find = target - n  
            if find in dic:
                return [dic[find], i+1]
            dic[n] = i+1
        """
```
  
## 2 #582
  https://leetcode.cn/submissions/detail/445207244/  
比较明显的并查集题目。  
心得：可以用哈希表模拟并查集，少些一点代码。  
<font face="黑体" color=orange size=2>常规做法</font>
```python3
class UnionFind:
    def __init__(self, n):
        self.n = n    
        self.parent = list(range(n))
        
    def find(self,x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
        
    #本题中 y 已经是 x 父节点，只要把X并入Y即可
    def union(self,x,y):
        rootX = self.find(x)    
        self.parent[rootX] = y 
        return True 

class Solution:
    def killProcess(self, pid: List[int], ppid: List[int], kill: int) -> List[int]:
        n = max(pid) + 1
        UF = UnionFind(n)
        for x, y in zip(pid, ppid):
            UF.union(x, y)
        UF.parent[kill] = -1
        ans = []
        for k in pid:
            r = UF.parent[k]   
            while r != 0 and r != -1:
                r = UF.parent[r]
            #从k 可以联通到 kill
            if r == -1:
                ans.append(k)
        return ans 
```  
<font face="黑体" color=orange size=2>hash table做法</font>
```python3
class Solution:
    def killProcess(self, pid: List[int], ppid: List[int], kill: int) -> List[int]:
        parent = defaultdict(int)    
        for x, y in zip(pid, ppid):
            parent[x] = y 
        parent[kill] = -1
        ans = []
        for k in parent:
            r = k  
            while r != 0 and r != -1:
                r = parent[r]
            if r == -1:
                ans.append(k)
        return ans 
```

## 3 #1059. 【图/DFS/拓扑】  
https://leetcode.cn/problems/all-paths-from-source-lead-to-destination/  
方法1：拓扑排序。由接邻表建图。同时用一个数组记录所有节点的入度。从终点反向搜索，搜索的同时减少入度。如果搜到起点后，起点的入度也为零，则成功，否则返回False。

```python3
class Solution:
    def leadsToDestination(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        g = defaultdict(list)
        degree = [0] * n 
        for x, y in edges:
            g[y].append(x)
            degree[x] += 1

        if degree[destination]:
            # 题目问的是 最终结束于， 所以不能有出度
            return False 
        # q存放的是 出度为 0 的节点，只有当SOURCE的出度也更新为0时，才算满足题目条件
        q = deque([destination])
        # q = [destination]
        while q:
            # node = q.pop(0)
            node = q.pop()
            if node == source:
                return True 
            for newNode in g[node]:
                degree[newNode] -= 1
                if degree[newNode] == 0:
                    # q.append(newNode)
                    q.appendleft(newNode)
        return False 
```
方法2： 回溯
这个方法要注意 图中有没有环，不能单纯dfs，不然会陷入死循环  【得加记忆化 不然超时，加了就超快, 因为很多节点在之前的循环中已经被访问过，而访问过后之后的内容是不会变的，完全一样的】
```python3
class Solution:
    def leadsToDestination(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        g = defaultdict(list)
        for x, y in edges:
            g[x].append(y)
        if destination in g:
            return False 
        visit = [False for _ in range(n)]
        visit[source] = True 
        @cache
        def backtrace(x):
            if len(g[x]) == 0:
                return x == destination  
            for y in g[x]:
                if visit[y]:
                    return False 
                visit[y] = True 
                if not backtrace(y):
                    return False 
                visit[y] = False 
            return True 
        return backtrace(source)
```
## 4 #604.  【设计/数组/字符串】
https://leetcode.cn/problems/design-compressed-string-iterator/description/  
常规做法：把字符串解码了，变成普通字符串

```python3 
class StringIterator:
    def __init__(self, cs: str):
        self.sa = []
        self.cur = 0
        num = 0
        c = cs[0]
        for i in range(len(cs)):  
            if cs[i].isdigit():
                num = num * 10 + int(cs[i])
            else:
                if num != 0:
                    self.sa.append(c*num)
                num = 0
                c = cs[i]
        self.sa.append(c*num)
        self.s = ''.join(self.sa)
        self.size = len(self.s)
    略....
```

## 5 #1236. 【字符串/BFS】
 https://leetcode.cn/problems/web-crawler/description/  
 没啥好说的，就是简单的BFS。只不过数据结构变成了给你一个接口，达到返回类似接邻表的目的。

```python3 
#class HtmlParser(object):
#    def getUrls(self, url):
#        """
#        :type url: str
#        :rtype List[str]
#        """

class Solution:
    def crawl(self, startUrl: str, htmlParser: 'HtmlParser') -> List[str]:
        domain = "http://" + startUrl.split('/')[2]
        q = deque([startUrl])
        res = set()

        while q:
            link = q.popleft()
            res.add(link)
            for url in htmlParser.getUrls(link):
                if url.startswith(domain) and url not in res:
                    q.append(url)
        return list(res)
        
```

# 2023年7月9日 
## 1
## 2
## 3
周赛3题，内容未放出暂略。  
心得：别复制黏贴！！！大段的类似的代码，很容易改错！！！题目3因为一个nums2没改成nums1导致debug很久没发现问题！！！  

## 4 #15.   【排序/双指针】
https://leetcode.cn/problems/3sum/description/  
因为题目要求不能有重复的三元组，所以先排序，然后双指针左右两端找，比较传统的思路。  
双指针遇到连续重复的数字直接跳过，加速遍历。
```python3
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        ans = []
        n = len(nums)
        if n < 3:
            return []
        nums.sort()
        for i in range(n):
            if nums[i] > 0:
                return ans   
            if i > 0 and nums[i] == nums[i-1]:
                continue   
            l = i + 1
            r = n - 1
            while l < r:
                if nums[i] + nums[l] + nums[r] == 0:
                    
                    ans.append([ nums[i], nums[l] , nums[r]])
                    while l < r and nums[l] == nums[l+1]:
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1
                    l += 1
                    r -= 1
                elif nums[i] + nums[l] + nums[r] > 0:
                    r -= 1
                else:
                    l += 1
        return ans 
```

## 5 #305 【图/并查集】
https://leetcode.cn/problems/number-of-islands-ii/description/?envType=study-plan-v2&envId=premium-algo-100

并查集乱杀。逐步并查就可以，在遇到一个新陆地的时候，看看四周是否是陆地，如果是陆地的话，看看他们的根是不是同一个，然后再把岛屿数量减去相应的数值。


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
    
    def union(self,x,y):
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
        
        # 并！
        return True 

class Solution:
    def numIslands2(self, m: int, n: int, positions: List[List[int]]) -> List[int]:
        def valid(x,y):
            return 0 <= x < m and 0 <= y < n
        
        UF = UnionFind(m*n)
        g = [[0 for _ in range(n)] for _ in range(m)]
        cnt = 0
        ans = []
        dirs = [(1,0),(0,1),(-1,0),(0,-1)]
        for x,y in positions:
            if g[x][y] == 1:
                ans.append(cnt)
                continue
            g[x][y] = 1
            tempParent = set()
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy   
                if valid(nx,ny) and g[nx][ny] == 1:
                    tempParent.add(UF.find(nx * n + ny))
            cnt += 1 - len(tempParent)
            ans.append(cnt)
            for p in tempParent:
                UF.union(x*n+y, p)
        return ans 
```

## 6 694. 【DFS/BFS】
https://leetcode.cn/problems/number-of-distinct-islands/description/  
方法一： 尝试使用并查集，并用元组(1,0),(0,1)代表方向，记录方向，达到定义岛屿的目的， 失败！！问题暂时没找到，先贴出代码，回头看。
```python3 
class Solution:
    def numDistinctIslands(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        def valid(x,y):
            return 0 <= x < m and 0 <= y < n
        UF = UnionFind(m*n)
        dirs = [(0,1),(1,0)]
        path = defaultdict(list)
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    p = UF.find(i * n + j)
                    if path[p] == []:
                        path[p].append((0,0))
                    for dx, dy in dirs:
                        nx = i + dx
                        ny = j + dy   
                        if valid(nx,ny) and grid[nx][ny] == 1:
                            UF.union(nx*n + ny, i * n + j)
                            
                            path[p].append((dx,dy))
        ans = []
        for v in path.values():
            if v not in ans:
                ans.append(v)
        return len(ans)
```

方法2：DFS记录方向，搜过的区域置零，防止反复被搜，用set去重，看最终set大小。
```python3
class Solution:
    def numDistinctIslands(self, grid: List[List[int]]) -> int:
        dirs = ((1,0),(0,1),(-1,0),(0,-1))
        m,n = len(grid), len(grid[0])
        def valid(x,y):
            return 0 <= x < m and 0 <= y < n
        def dfs(x,y):
            grid[x][y] = 0
            for i, [dx,dy] in enumerate(dirs):
                nx,ny = x + dx, y + dy   
                nonlocal path
                path += str(i)  
                if valid(nx,ny) and grid[nx][ny] == 1:
                    dfs(nx,ny)
        
        ans = set()
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    path = ""
                    dfs(i,j)
                    ans.add(path)
        return len(ans)
```

## 7 169 【数学/排序】
https://leetcode.cn/problems/majority-element/description/   
求数量大于 n//2的数， 直接排序，中间的必然是。
```python3
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        n = len(nums)
        nums.sort()
        return nums[n//2]
```

# 2023年7月10日   

## 1 #16. 【双指针/排序】
https://leetcode.cn/problems/3sum-closest/description/  
和昨天每日一题一个套路。确定一个位置， 从它往后开始双指针找符合条件的值。  
进行几处优化  
1, 因为原数组经过排序，所以 连续3个数之和大于target之后，即可判断答案，直接跳出循环，后面和只会更大。  
2，因为原数组经过排序，所以当前数字+最后两个数字要是小于target，没有别的能更接近target了，直接判断，然后continue。
```python3
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        n = len(nums)
        nums.sort()
        if n < 4:
            return sum(nums)
        cur_min = float("inf")
        ans = 0
        for i in range(n-2):
            x = nums[i]
            if i and nums[i] == nums[i-1]:
                continue 
            s = x + nums[i+1] + nums[i+2]
            if s > target:
                if s - target < cur_min:
                    return s
                break   
            s = x + nums[-1] + nums[-2]
            if s < target:
                if target - s < cur_min:
                    cur_min = target - s
                    ans = s     
                continue   
            l, r = i+1, n - 1
            while l < r:
                s = x + nums[l] + nums[r]
                if s == target:
                    return s   
                if s > target:
                    if s - target < cur_min:
                        cur_min = s - target   
                        ans = s  
                    r -= 1
                elif s < target:
                    if target - s < cur_min:
                        cur_min = target - s    
                        ans = s 
                    l += 1
        return ans 

```

## 2 #1136 【拓扑序/图/BFS】
https://leetcode.cn/problems/parallel-courses/description/?envType=study-plan-v2&envId=premium-algo-100  
有previous限制条件，直接想到拓扑排序。利用bfs计算次数。建图时直接判断有无互为前置课程。  
最后计算degree的sum来判断是否存在环，如果存在环，则有些节点不会被访问过，所以degree不会为0.

```python3 
class Solution:
    def minimumSemesters(self, n: int, relations: List[List[int]]) -> int:
        #利用一个数组记录出度，出度为0的可以上，
        #用字典建图，记录 当前节点的来源，如果该节点被修完，它的来源节点的degree -= 1
        #如果有环，则无法完成
        degree = [0] * n 
        g = defaultdict(list)
        for x,y in relations:
            g[y-1].append(x-1)
            if y-1 in g[x-1]:
                #有环
                return -1 
            degree[x-1] += 1    
        cnt = 0
        print(degree)
        q = [i for i in range(n) if degree[i] == 0]
        while q:
            size = len(q)
            cnt += 1
            while size > 0:
                size -= 1
                cur = q.pop(0)
                for p in g[cur]:
                    degree[p] -= 1
                    if degree[p] == 0:
                        q.append(p)
        if sum(degree) != 0:
            return -1 
        return cnt 
```

## 3 #1494 【状态压缩/DFS/递归】
https://leetcode.cn/problems/parallel-courses-ii/description/   

前置知识：关于集合和位运算的代码实现，参考灵神笔记。https://leetcode.cn/circle/discuss/CaOJ45/  
头好痒，好像有什么东西要长出来了😀  
```python3
class Solution:
    def minNumberOfSemesters(self, n: int, relations: List[List[int]], k: int) -> int:
        pre1 = [0] * n 
        for x,y in relations:
            pre1[y-1] |= 1 << (x-1)
        
        u = (1<<n) - 1

        @cache    
        def dfs(i):
            if i == 0:
                return 0
            ci = u ^ i
            i1 = 0
            for j,p in enumerate(pre1):
                if i >> j &1 and p | ci == ci:
                    i1 |= 1 << j    
            if i1.bit_count() <= k:
                return dfs(i ^ i1) + 1
            
            res = inf  
            j = i1 
            while j:
                if j.bit_count() == k:
                    res = min(res, dfs(i ^j) + 1)
                j = (j-1) & i1   
            return res   
        return dfs(u)
```

# 2023年7月11日  

## 1 #505 【BFS/DFS/最短路径】  
https://leetcode.cn/problems/the-maze-ii/?envType=study-plan-v2&envId=premium-algo-100

方法1：
bfs硬搜会出现问题，因为碰墙次数少的走法不一定路径是最短的。借鉴dijkstra算法，每次走到一个点，要和它之前离起点的距离（初始为inf）作比较，如果比之前的小，则更新距离，入队继续bfs，否则不入队。  
进一步优化可以用优先队列来实现dijkstra算法，这样每次出队的都是目前离起点最近的一个点。

```python3
class Solution:
    def shortestDistance(self, maze: List[List[int]], start: List[int], destination: List[int]) -> int:
        m, n = len(maze), len(maze[0])
        if maze[start[0]][start[1]] == 1 or maze[destination[0]][destination[1]] == 1:
            return False
        q1 = [(start[0], start[1])]
        dirs = [(1,0),(-1,0),(0,1),(0,-1)]
        dist = [[float('inf') for _ in range(n)] for _ in range(m)]
        dist[start[0]][start[1]] =0
        while q1:
            size = len(q1)
            while size:
                size -= 1
                x,y = q1.pop(0)
                for dx, dy in dirs:
                    nx, ny = x, y
                    cnt = 0
                    while 0 <= nx + dx < m and 0 <= ny + dy < n and maze[nx+dx][ny+dy] == 0 :
                        cnt += 1
                        nx += dx   
                        ny += dy  
                    if dist[x][y] + cnt < dist[nx][ny]:
                        dist[nx][ny] = dist[x][y] + cnt
                        q1.append((nx,ny))
        if dist[destination[0]][destination[1]] == float("inf"):
            return -1
        return dist[destination[0]][destination[1]]
```

<font face="黑体" color=#00FFFF size=8>做了好几天图，今天强化一下DP</font>  
  
## 2 #70. 【DP/数学/记忆化】  
https://leetcode.cn/problems/climbing-stairs/  

入门级dp，不多说，走一步时看前一步，走两步时看前两步。
```python3
class Solution:
    def climbStairs(self, n: int) -> int:
        #dp[i][0]:走1阶梯，  dp[i][1]：走2阶梯
        if n == 1:
            return 1
        dp = [[0,0] for _ in range(n)]
        dp[0][0] = 1
        dp[1][0] = 1
        dp[1][1] = 1
        for i in range(2,n):
            dp[i][0] = dp[i-1][0] + dp[i-1][1]
            dp[i][1] = dp[i-2][0] + dp[i-2][1]
            #可以定义成dp[i], 那样就整合成dp[i] = dp[i-1] + dp[i-1],我这边为了明显所以拆开了
        return dp[-1][0] + dp[-1][1]
```

## 3 #198 【dp】
思路一样的，就是看上一项，上两项。一维数组，二维数组都行。
#https://leetcode.cn/problems/house-robber/description/

```python3
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        if n < 3:
            return max(nums)
        # # DP[I][x]: x == 0:拿   x== 1： 不拿
        # dp = [[0,0] for _ in range(n)]
        
        # dp[0][0] = nums[0]
        # dp[1][0] = nums[1]
        # dp[1][1] = nums[0]
        # for i in range(2,n):
        #     dp[i][0] = dp[i-1][1] + nums[i]
        #     dp[i][1] = max(dp[i-1][0],dp[i-1][1])
        # return max(dp[-1])

        # f = [0 for _ in range(n)]
        # # f[i]: 在i家取得的最大财富
        # f[0] = nums[0]
        # f[1] = max(nums[0], nums[1])
        # for i in range(2,n):
        #     f[i] = max(f[i-1], f[i-2] + nums[i])

        # return f[-1]
        
        #再简化，维护2个值而不是数组
        prev = nums[0]
        cur = max(prev,nums[1])
        for i in range(2,n):
            temp = cur
            cur = max(cur, prev+nums[i])
            prev = temp  
        return cur
```

## 4 #1911 【dp】  
https://leetcode.cn/problems/maximum-alternating-subsequence-sum/description/  

选择当前数作为最后一位，如果下标是偶数，那么就是max(前一项是奇数 + 当前值 【选择】， 前一项是偶数【不选】)。 如果下标是奇数，那么就是max（前一项是偶数-当前值【选择】，前一项是奇数【不选】）。
还有一种方法是看波峰波谷。加上波峰，减去波谷，这种方法要注意边界条件【头和尾】
```python3
class Solution:
    def maxAlternatingSum(self, nums: List[int]) -> int:
        even, odd = nums[0], 0 
        for i in range(1,len(nums)):
            even, odd = max(even, odd + nums[i]), max(odd, even - nums[i])
        return even
```
## 5 #213. 【dp】
https://leetcode.cn/problems/house-robber/  
选当前一家，或不选，两种情况。维护两个int来优化空间。和198一样。只不过头尾不能同时取，所以分成有头无尾情况 和有尾无头情况讨论
```python3
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
        def help(arr):
            n = len(arr)
            if n < 3:
                return max(arr)
            prev = arr[0]
            cur = max(prev,arr[1])
            for i in range(2,n):
                temp = cur        
                #如果要
                cur = max(prev+arr[i], cur)   
                prev = temp  
            return cur 

        return max(help(nums[:-1]), help(nums[1:]))
```

## 6 #337 【树形dp/dfs】
https://leetcode.cn/problems/house-robber-iii/description/  
打家劫舍再升级，这次变成树形DP，树形dp就是基于dfs后续遍历，左右根，来取dp值  
```python3
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        def dfs(node):
            if not node:
                return (0,0)
            l1, l2 = dfs(node.left)
            r1,r2 = dfs(node.right)
            rob = l2 + r2 + node.val 
            not_rob = max(l1,l2) + max(r1,r2)
            return (rob, not_rob)
        return max(dfs(root))
```
## 7 #2560 【dp/二分/最小化最大值】
https://leetcode.cn/problems/house-robber-iv/description/  
打家劫舍第四版，除了dp外，结合了二分。  
遇到了最小化最大值，或者最大化最小值的问题时，可以用二分去快速逼近答案。  
dp套路不变，看是否满足 小于当前规定的mx值，如果满足可以选，然后依然分为 选/不选 当前位置两种情况。  
```python3
class Solution:
    def minCapability(self, nums: List[int], k: int) -> int:
        n = len(nums)
        if n == 1:
            return nums[0]
        def check(mx):
            dp = [0 for _ in range(n)]
            dp[0] = 1 if nums[0] <= mx else 0
            dp[1] = 1 if min(nums[0],nums[1]) <= mx else 0
            for i in range(2,n):
                if nums[i] <= mx:
                    dp[i] = max(dp[i-2] + 1, dp[i-1])
                else:
                    dp[i] = dp[i-1]
            return dp[-1]
        l, r = 0, max(nums)
        while l < r:
            mid = l + (r-l) // 2
            if check(mid) >= k:
                r = mid 
            else:
                l = mid + 1
        return l
```          

## 8 #1167 【优先队列/堆】
https://leetcode.cn/problems/minimum-cost-to-connect-sticks/description/?envType=study-plan-v2&envId=premium-algo-100
没啥说的，题目描述都就差直接告诉你要用优先队列了  
```python3
class Solution:
    def connectSticks(self, sticks: List[int]) -> int:
        ans = 0
        heapq.heapify(sticks)
        while len(sticks) > 1:
            x = heapq.heappop(sticks)
            y = heapq.heappop(sticks)
            ans += x + y  
            heapq.heappush(sticks,x+y)
        return ans 
```
## 9 #1057 【优先队列/模拟】
https://leetcode.cn/problems/campus-bikes/description/?envType=study-plan-v2&envId=premium-algo-100  

把距离入队，优先距离小的两者出栈，赋值，如果自行车已经被分配，则继续下面的队列头出队。
```python3
        def cal(a,b,x,y):
            return abs(a - x) + abs(b - y)
        n = len(workers)
        m = len(bikes)
        dis = []
        for i in range(n):
            a,b = workers[i]
            for j in range(m):
                x,y = bikes[j]
                heapq.heappush(dis,(cal(a, b, x, y), i, j))
        ans = [-1 for _ in range(n)]
        used = [0 for _ in range(m)]
        cnt = 0
        while dis and cnt < n:
            _,i,j = heapq.heappop(dis)
            if ans[i] == -1 and used[j] == 0:
                used[j] = 1
                ans[i] = j
                cnt += 1
        return ans 
```
# 7月12日
## 1 #2544 【数组/数学】
https://leetcode.cn/problems/alternating-digit-sum/description/  
没啥说的。
```python3
class Solution:
    def alternateDigitSum(self, n: int) -> int:
        arr = []
        num = n 
        while num:
            arr.append(num%10)
            num //= 10
        arr = arr[::-1]
        return sum(arr[::2]) - sum(arr[1::2])
```              
## 2 #499
https://leetcode.cn/problems/the-maze-iii/description/?envType=study-plan-v2&envId=premium-algo-100  
&emsp;&emsp;迷宫Ⅲ。同样是要碰壁才能改变方向，但是第一个方法没有用到最短路径算法，没有记录每个点距离起点的最近路径长度。而是用BFS一层一层搜。  
&emsp;&emsp;并且同时用一个visit数组记录，每个点在四个方向上是否到达过。使用set记录，如果某个带着某个方向之前到达过这个点，那么就不重新入队，因为已经不是最短路径了，但是如果是从别的方向过来的，则入队，因为不确定之后的路径会不会更短。  
如果不碰壁,就不改变方向地入队，如果碰壁，则更换方向，并更新path入队。

```python3
class Solution:
    def findShortestWay(self, maze: List[List[int]], ball: List[int], hole: List[int]) -> str:
        visit = defaultdict(set)
        m,n = len(maze), len(maze[0])
        dirs = {'u':(-1,0), 'd':(1,0), 'l':(0,-1), 'r':(0,1)}
        ans = 'z'
        def valid(x,y):
            return 0 <= x < m and 0 <= y < n and not maze[x][y]
        q = [[ball[0] + dirs[d][0], ball[1] + dirs[d][1], d] for d in dirs if valid(ball[0] + dirs[d][0],ball[1] + dirs[d][1])]
        while q and ans == 'z':
            size = len(q)
            for _ in range(size):
                x,y,path = q.pop(0)
                visit[(x,y)].add(path[-1])
                if [x,y] == hole:
                    ans = min(ans,path)  
                    continue   
                a, b = x + dirs[path[-1]][0], y + dirs[path[-1]][1]
                if valid(a,b):
                    if path[-1] not in visit[(a,b)]:
                        q.append([a,b,path])
                else:
                    for d in dirs:
                        if d == path[-1]:
                            continue  
                        a,b =  x + dirs[d][0], y + dirs[d][1]
                        if valid(a,b):
                            if d not in visit[(a,b)]:
                                q.append([a,b,path + d])
        return ans if ans != 'z' else 'impossible'
```

## 3 #276 【DP】
https://leetcode.cn/problems/paint-fence/description/?envType=study-plan-v2&envId=premium-algo-100  
两种情况，异色，同色。分类讨论即可
```python3 
class Solution:
    def numWays(self, n: int, k: int) -> int:
        if n < 2:
            return k
        dp = [[0,0] for _ in range(n+1)]
        dp[0][1] = k   
        dp[1][0] = k
        dp[1][1] = k*(k-1)
        for i in range(2,n+1):
            dp[i][0] = dp[i-1][1]
            dp[i][1] = (k-1)*dp[i-1][0] + (k-1) * dp[i-1][1]
        return sum(dp[n-1])
```

## 4 #256 【DP】
https://leetcode.cn/problems/paint-house/description/?envType=study-plan-v2&envId=premium-algo-100  
三种情况，不能连续同色，当前看 前一家另外两种颜色的情况。简单。
```python3
class Solution:
    def minCost(self, costs: List[List[int]]) -> int:
        n = len(costs)
        if n == 1:
            return min(costs[0])
        dp = [[float('inf'),float('inf'),float('inf')] for _ in range(n)]
        dp[0] = costs[0]
        for i in range(1,n):
            dp[i][0] = min(dp[i-1][1],dp[i-1][2]) + costs[i][0]
            dp[i][1] = min(dp[i-1][0],dp[i-1][2]) + costs[i][1]
            dp[i][2] = min(dp[i-1][1],dp[i-1][0]) + costs[i][2]
        return min(dp[-1])

```
