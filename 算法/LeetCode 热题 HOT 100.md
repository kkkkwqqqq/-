# LeetCode 热题 HOT 100

## [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

> 题目描述：

![image-20220417102552162](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204171025413.png)

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204171025413.png)

![image-20220417102630465](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204171026610.png)

> 思路：双指针

难点在于，由于两条链表的长度可能不同，两条链表之间的节点无法对应：

[![img](https://labuladong.gitee.io/algo/images/链表技巧/5.jpeg)](https://labuladong.gitee.io/algo/images/链表技巧/5.jpeg)

如果用两个指针 `p1` 和 `p2` 分别在两条链表上前进，并不能**同时**走到公共节点，也就无法得到相交节点 `c1`。

**解决这个问题的关键是，通过某些方式，让 `p1` 和 `p2` 能够同时到达相交节点 `c1`**。

所以，我们可以让 `p1` 遍历完链表 `A` 之后开始遍历链表 `B`，让 `p2` 遍历完链表 `B` 之后开始遍历链表 `A`，这样相当于「逻辑上」两条链表接在了一起。

如果这样进行拼接，就可以让 `p1` 和 `p2` 同时进入公共部分，也就是同时到达相交节点 `c1`：

[![img](https://labuladong.gitee.io/algo/images/链表技巧/6.jpeg)](https://labuladong.gitee.io/algo/images/链表技巧/6.jpeg)

那你可能会问，如果说两个链表没有相交点，是否能够正确的返回 null 呢？

这个逻辑可以覆盖这种情况的，相当于 `c1` 节点是 null 空指针嘛，可以正确返回 null。

按照这个思路，可以写出如下代码：

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        // p1 指向 A 链表头结点，p2 指向 B 链表头结点
        ListNode p1 = headA, p2 = headB;
        while (p1 != p2) {
            // p1 走一步，如果走到 A 链表末尾，转到 B 链表
            if (p1 == null) p1 = headB;
            else            p1 = p1.next;
            // p2 走一步，如果走到 B 链表末尾，转到 A 链表
            if (p2 == null) p2 = headA;
            else            p2 = p2.next;
        }
        return p1;
    }
}
```



## [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

> 题目描述

![image-20220417104627855](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204171046927.png)

![image-20220417104644029](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204171046108.png)

> 思路:辅助栈  栈顶始终为已有元素的最小值

```java
import java.util.Stack;

class MinStack {
    Stack stack;
    Stack stack1;

    public MinStack() {
        stack=new Stack();
        stack1=new Stack();
        stack1.push(Integer.MAX_VALUE);

    }

    public void push(int val) {
        stack.push(val);
        stack1.push(Math.min((int)stack1.peek(),val));
    }

    public void pop() {
        stack.pop();
        stack1.pop();
    }

    public int top() {
        return (int)stack.peek();
    }

    public int getMin() {

        return (int)stack1.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```



## [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

> 题目描述：

![image-20220417111504135](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204171115211.png)

> 思路：

依次算出，每个节点的长度，最大的就是题目所求。

```java

class Solution {
    int max=0;
    public int diameterOfBinaryTree(TreeNode root) {
            maxDept(root);
            return max;
    }
   //求root作为跟结点的最大深度
    int maxDept(TreeNode root){
        if(root==null){
            return 0;
        }
        //左子树的高度
       int left= maxDept(root.left);
       // 右子树的高度
       int right=maxDept(root.right);
       max=Math.max(max,left+right);
       return 1+Math.max(left,right);
    }
}
```



## [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

> 题目描述：

![image-20220417111642961](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204171116014.png)

> 思路

回溯

> 具体代码：

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    List<String> res;

    public List<String> generateParenthesis(int n) {
        res = new ArrayList<>();
        backtrack(new StringBuilder(),n,n);
        return res;
    }

    private void backtrack(StringBuilder sb, int left, int right) {
        if (right<left){
            return;
        }
        if (left==0&&right==0){
            res.add(sb.toString());
        }
        if (left<0||right<0){
            return;
        }
        //添加左括号
        sb.append('(');
        backtrack(sb,left-1,right); //再次选择
        sb.deleteCharAt(sb.length()-1);

        //添加右括号
        sb.append(')');
        backtrack(sb,left,right-1); //再次选择
        sb.deleteCharAt(sb.length()-1);
    }


}

```

## [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)（ac）

> 题目描述：

![image-20220418094356189](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204180943401.png)

![image-20220418094410637](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204180944699.png)

> 思路：

dp味道太浓



> 编码：

```java
class Solution {
    public int minPathSum(int[][] grid) {
        //长
        int m= grid.length;
        //宽
        int n=grid[0].length;
        //dp[i][j] 表示从原点到i,j的最短路径
        int dp[][]=new int [m+1][n+1];
        dp[1][1]=grid[0][0];
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(i==1&&j==1){
                    continue;
                }
                if(j>1&&i>1&&j<=n){
                    dp[i][j]=Math.min(dp[i][j-1],dp[i-1][j])+grid[i-1][j-1];
                }else if(i==1){ //只能从左边来
                    dp[i][j]=dp[i][j-1]+grid[i-1][j-1];
                }else if(j==1){ //只能从上面来
                    dp[i][j]=dp[i-1][j]+grid[i-1][j-1];
                }
            }
        }
        return dp[m][n]; }
}
```



## [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

> 题目描述

![image-20220418193443810](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204181934940.png)

> 思路：

题目一看 就知道经典回溯了 模板是 添加--》再选择--》撤销选择 于是乎

```
import java.util.ArrayList;
import java.util.List;

//题目一看 就知道经典回溯了 模板是 添加--》再选择--》撤销选择
class Solution {
     // 每个数字到字母的映射
    String[] mapping = new String[] {
            "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"
    };

    //返回的集合
     List<String> list;
    public List<String> letterCombinations(String digits) {
        //初始化
          list= new ArrayList<String>();
          if(digits.length()==0){
            return list;
        }
         StringBuffer sb= new StringBuffer();
          int n=digits.length();
          back(sb,digits,0);
          return list;
    }
    public void back(StringBuffer sb,String digits,int start){
        if(sb.length()==digits.length()){
            list.add(sb.toString());
        }
        for(int i=start;i<digits.length();i++){
            int k = digits.charAt(i) - '0';
            for(char c: mapping[k].toCharArray()){
                 //选择
                    sb.append(c);
                   //进入下一次决策树
                back(sb,digits,i+1);
                    //取消选择
                   sb.deleteCharAt(sb.length() - 1);
            }
        }

    }
}

```

## [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

> 题目描述：

![image-20220419092157606](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204190921924.png)

> 方法一：对每个字符串进行排序 排序后值相等的即为字母异位词  思路想到了 落地写的太复杂了 于是乎看看官方题解，甚秒

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            char[] array = str.toCharArray();
            Arrays.sort(array);
            String key = new String(array);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}



```

> 方法二   利用每个字符的出现次数进行编码和巧妙利用hashmap的api

首先我们先来认识一个api

```java
map.putIfAbsent("b", 5)    //如果key不存在或者key已存在但是值为null，才put进去
```

> 编码：

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // 编码到分组的映射
        HashMap<String, List<String>> codeToGroup = new HashMap<>();
        for (String s : strs) {
            // 对字符串进行编码
            String code = encode(s);
            // 把编码相同的字符串放在一起
            codeToGroup.putIfAbsent(code, new LinkedList<>()); //如果key不存在或者key已存在但是值为null，才put进去  key是根据值相等来判断是否是同一个key 而不是通过地址
            codeToGroup.get(code).add(s);
        }

        // 获取结果
        List<List<String>> res = new LinkedList<>();
        for (List<String> group : codeToGroup.values()) {
            res.add(group);
        }

        return res;
    }

    // 利用每个字符的出现次数进行编码
    String encode(String s) {
        char[] code = new char[26];
        for (char c : s.toCharArray()) {
            int delta = c - 'a';
            code[delta]++;
        }
        return new String(code);
    }
}
```

## [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)（ac）

> 题目说明：

![image-20220419095919538](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204190959601.png)

![image-20220419100008216](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204191000279.png)

![image-20220419100031055](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204191000128.png)

> 动态规划痕迹太明显啦 ac了

```shell
class Solution {
    public int uniquePaths(int m, int n) {
        //dp[i][j]：表示从起始点到(i,j)的不同路径条数
            int dp[][]=new int [m+1][n+1];
            dp[1][1]=1;
            for(int i=1;i<=m;i++){
                for(int j=1;j<=n;j++){
                    if(i==1&&j==1){
                        continue;
                    }
                    if(i==1){
                        //只能从左边来
                        dp[i][j]=dp[i][j-1];
                    }else if(j==1){
                        //只能从上面来
                        dp[i][j]=dp[i-1][j];
                    }else{
                        //从左或者从上能取得的全部路径相加即为所求
                        dp[i][j]=dp[i][j-1]+dp[i-1][j];
                    }
                }
            }
            return dp[m][n];
    }
}
```



## [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

> 题目描述：

![image-20220420104119822](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204201041997.png)

> 思路： 区间问题 肯定是画图分析， 很容易发现（`如果前面看过东哥的区间系列`）要先对其第一个元素进行排序
>
> ==**提升点：对api不够熟悉**==

于是乎编码：

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        LinkedList<int[]> res = new LinkedList<>(); // List<int []> list= new ArrayList<>();也行下面对应改成这样就行：int last[] =list.get(list.size()-1);
        // 按区间的 start 升序排列
        Arrays.sort(intervals, (a, b) -> {
            return a[0] - b[0];                //按第一个从小到大进行升序排列
        });

        res.add(intervals[0]);
        for (int i = 1; i < intervals.length; i++) {
            int[] curr = intervals[i];
            // res 中最后一个元素的引用
            int[] last = res.getLast(); //记api
            if (curr[0] <= last[1]) {
                last[1] = Math.max(last[1], curr[1]);
            } else {
                // 处理下一个待合并区间
                res.add(curr);
            }
        }
        return res.toArray(new int[0][0]);  //记住api
    }
}
// 详细解析参见：
// https://labuladong.gitee.io/article/?qno=56

```



## [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

> 题目描述：

![image-20220420115329404](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204201153464.png)

![image-20220420115341846](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204201153916.png)

> 思路：讲真bfs挺薄弱的 多练吧哈哈    ==**记住api**==

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        if (root == null) {
            return res;
        }

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);  //将元素插入队列
        // while 循环控制从上向下一层层遍历
        while (!q.isEmpty()) {
            int sz = q.size();
            // 记录这一层的节点值
            List<Integer> level = new LinkedList<>();
            // for 循环控制每一层从左向右遍历
            for (int i = 0; i < sz; i++) {
                TreeNode cur = q.poll();  //将队首元素取出来
                level.add(cur.val);
                if (cur.left != null)
                    q.offer(cur.left);
                if (cur.right != null)
                    q.offer(cur.right);
            }
            res.add(level);
        }
        return res;
    }
}
```

## [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

> 题目描述：

![image-20220421102308287](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204211023563.png)

![image-20220421102318495](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204211023780.png)

> 思路： bfs

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int minDepth(TreeNode root) {
        if(root==null){
            return 0;
        }
        //计数
        int count=0;
        Queue<TreeNode> q=new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            count++;
           int sz= q.size(); 
           for(int i=0;i<sz;i++){
               TreeNode node= q.poll();
            
            if(node.left==null&&node.right==null){
                return count;
            }
            if(node.left!=null){
                q.offer(node.left);
            }
             if(node.right!=null){
                q.offer(node.right);
            }
           }       
        }
   return count; }
}
```

## [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)（二刷没写出来）

> 题目描述：

![image-20220421104605975](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204211046343.png)

> 思路

举个例子，比如给算法输入`n = 5`，也就是说用`{1,2,3,4,5}`这些数字去构造 BST。

首先，这棵 BST 的根节点总共有几种情况？

显然有 5 种情况对吧，因为每个数字都可以作为根节点。

比如说我们固定`3`作为根节点，这个前提下能有几种不同的 BST 呢？

根据 BST 的特性，根节点的左子树都比根节点的值小，右子树的值都比根节点的值大。

所以如果固定`3`作为根节点，左子树节点就是`{1,2}`的组合，右子树就是`{4,5}`的组合。

**左子树的组合数和右子树的组合数乘积**就是`3`作为根节点时的 BST 个数。

![图片](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204211146895.jpeg)

我们这是说了`3`为根节点这一种特殊情况，其实其他的节点也是一样的。

那你可能会问，我们可以一眼看出`{1,2}`和`{4,5}`有几种组合，但是怎么让算法进行计算呢？

其实很简单，只需要递归就行了，我们可以写这样一个函数：

```java
// 定义：闭区间 [lo, hi] 的数字能组成 count(lo, hi) 种 BST
int count(int lo, int hi);       
```

于是乎：

```java
class Solution {
    public int numTrees(int n) {
    //count(1,n)表示区间【1，n】能组成多少种不同的二叉排序树
     return count(1,n);
    }

    int count(int lo,int hi )
    {  int res=0;
        if(lo>=hi){
            return 1;
        }
        for(int mid=lo;mid<=hi;mid++){
            int left=count(lo,mid-1);
            int right=count(mid+1,hi);
            res+=left*right;
        }return res;
    }
}

```

==**但是存在一个问题:设计出来的代码会超时。发现是因为会产生重复子问题。**==

于是结合备忘录写出如下代码：

```java
class Solution {
    //备忘录来优化
    int[][] memo;
    public int numTrees(int n) {
      memo = new int[n + 1][n + 1];
    //count(1,n)表示区间【1，n】能组成多少种不同的二叉排序树
     return count(1,n);
    }

    int count(int lo,int hi )
    {  int res=0;
       if(lo>=hi){
            return 1;
        }
    // 查备忘录
        if (memo[lo][hi] != 0) {
            return memo[lo][hi];
        }
        for(int mid=lo;mid<=hi;mid++){
            int left=count(lo,mid-1);
            int right=count(mid+1,hi);
            res+=left*right;
        }// 将结果存入备忘录
        memo[lo][hi] = res;
        return memo[lo][hi];
    }
}

```

## [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

> 题目描述

![image-20220430113348246](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204301133532.png)

> 思路：

==明确递归函数的定义是什么，根据定义去演绎结合实际画图分析 即可轻松写出==

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
 //二叉树的题目基本离不开 递归
class Solution {
    public TreeNode invertTree(TreeNode root) {
          if (root == null) {
        return root;
    }
        invertNode(root);
        return root;
    }
    //反转两个root的两个子节点
  void  invertNode(TreeNode root){
      if(root==null){
          return ;
      }
        TreeNode temp=root.left;
        root.left=root.right;
        root.right=temp;
         // 让左右子节点继续翻转它们的子节点
                invertNode(root.left);
                invertNode(root.right); 
        
    }
}
```



## [95. 不同的二叉搜索树 II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)

> 题目描述：

![image-20220504110010601](../../../AppData/Roaming/Typora/typora-user-images/image-20220504110010601.png)

> 思路三部曲：

定义一个函数（`只需明确你对他的定义比如build(1, n)代表构造闭区间 [1, n] 组成的 BST,然后按照这个函数一定能实现这个效果的逻辑继续往后写。`）

1、穷举`root`节点的所有可能。

2、递归构造出左右子树的所有合法 BST。

3、给`root`节点穷举所有左右子树的组合。

编码：

```java
class Solution {
    /* 主函数 */
    public List<TreeNode> generateTrees(int n) {
        if (n == 0) return new LinkedList<>();
        // 构造闭区间 [1, n] 组成的 BST
        return build(1, n);
    }

    /* 构造闭区间 [lo, hi] 组成的 BST */
    List<TreeNode> build(int lo, int hi) {
        List<TreeNode> res = new LinkedList<>();
        // base case
        if (lo > hi) {
            res.add(null);
            return res;
        }

        // 1、穷举 root 节点的所有可能。
        for (int i = lo; i <= hi; i++) {
            // 2、递归构造出左右子树的所有合法 BST。
            List<TreeNode> leftTree = build(lo, i - 1);
            List<TreeNode> rightTree = build(i + 1, hi);
            // 3、给 root 节点穷举所有左右子树的组合。
            for (TreeNode left : leftTree) {
                for (TreeNode right : rightTree) {
                    // i 作为根节点 root 的值
                    TreeNode root = new TreeNode(i);
                    root.left = left;
                    root.right = right;
                    res.add(root);
                }
            }
        }
        return res;
    }
}
// 详细解析参见：
// https://labuladong.github.io/article/?qno=95

```

## [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)(二刷ac)

> 题目描述：

![image-20220504114631942](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205041146131.png)

> 思路：定义函数，明确这个函数能实现的功能。根据这个函数的定义去编写剩下的代码

==**比如：定义flatten(TreeNode root)为能将root为根的树展开为链表，那么我们再写剩下逻辑如果子问题需要将以某节点根展开为链表，直接套这个函数即可。**==

有了这个函数定义，如何按题目要求把一棵树拉平成一条链表？

对于一个节点 `x`，可以执行以下流程：

1、先利用 `flatten(x.left)` 和 `flatten(x.right)` 将 `x` 的左右子树拉平。

2、将 `x` 的右子树接到左子树下方，然后将整个左子树作为右子树。

[![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205041201125.jpeg)](https://labuladong.github.io/algo/images/二叉树系列/2.jpeg)

```java
class Solution {
    // 定义：将以 root 为根的树拉平为链表
    public void flatten(TreeNode root) {
        // base case
        if (root == null) return;
        // 先递归拉平左右子树
        flatten(root.left);
        flatten(root.right);

        /****后序遍历位置****/
        // 1、左右子树已经被拉平成一条链表
        TreeNode left = root.left;
        TreeNode right = root.right;

        // 2、将左子树作为右子树
        root.left = null;
        root.right = left;

        // 3、将原先的右子树接到当前右子树的末端
        TreeNode p = root;
        while (p.right != null) {
            p = p.right;
        }
        p.right = right;
    }
}
// 详细解析参见：
// https://labuladong.github.io/article/?qno=114

```

你看，这就是递归的魅力，你说 `flatten` 函数是怎么把左右子树拉平的？

不容易说清楚，但是只要知道 `flatten` 的定义如此并利用这个定义，让每一个节点做它该做的事情，然后 `flatten` 函数就会按照定义工作。
