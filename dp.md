

### dp的基石

1. 最优子结构：小的最优解+其他条件-> 推出大结构的最优解

1. 重复子问题
2. 无后效性：推后面的状态的时候不需要知道前面的状态做了些什么导致现在这步什么事能做，什么事不能做

有后效性的解决方法：给dp数组升维

* 做dp问题的步骤

1. 设计状态，用于复用结果 (k->v)
2. 状态转移
3. 边界条件





### [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)

dp 的初值比较难想

### [Coin Change I](https://leetcode.com/problems/coin-change-ii/)

### [Coin Change II](https://leetcode.com/problems/coin-change-ii/)

0-1 完全背包入门题

### [Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/) vs [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

思考时错误的想法：为了通过动态规划逐步求解，$\text{dp}[i][j]$代表数组下标滑动到$i,j$时的最优解

正确的想法：每个$i,j$状态都存储试图将$\text{nums}[i]\text{与nums}[j]$匹配的最优结果。

dp的含义要慎重考虑，不能只考虑$\text{dp}[i][j]$代表滑动到$i,j$时的最优解。比如这道题：

由于动态规划需要满足**无后效性**，$\text{dp}[i][j]$应该定义为必须以$i,j$结尾的结果，也即必须满足$\text{nums}[i]==\text{nums}[j]$时匹配到的结果，这样方便之后的状态利用现在的状态进行转移。至于$j$这一层前面可能的更优结果，已经存到了前面的数组中，这也是花费空间开辟dp数组的原因，一定要利用好之前开辟的空间，不然思路容易出问题。

* 连续序列，子序列的dp方法比较

| 题目             | 连续子序列                                                   | 子序列                                                       |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 一维数字数组     | 最大子序和<br />$dp[i]=max(dp[i-1]+nums[i],nums[i])$<br />$dp$考虑结束子序**必须**为$i$时的情况 | 最长递增子序列<br />$dp[i]=max(dp[j])+1$<br />$dp$考虑以$i$结尾时的**最优**情况 |
| 两个一维数字数组 | 最长重复子数组<br />$dp[i][j]=dp[i-1][j-1]+1$<br />$dp$考虑以$i,j$结尾时的**最优**情况 | 最长公共子序列<br />$dp[i][j]=max(dp[i][j-1],dp[i-1][j])$<br />$dp$考虑结束子序**必须**为$i,j$时的情况 |
| 一维回文子串     | 最长回文子串<br />$dp[i][j]=dp[i+1][j-1]$<br />$dp$考虑以$i,j$结尾时的**最优**情况 | 最长回文子序列<br />$dp[i][j]=max(dp[i+1][j],dp[i][j-1])$<br />$dp$考虑结束子序**必须**为$i,j$时的情况 |





