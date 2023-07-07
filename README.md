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
