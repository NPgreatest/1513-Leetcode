

### 可能用到的方法

1. 对于数组的问题

根据数据量思考是否可以DFS, 进一步考虑能分治、dp或者greedy

对于$O(n \text{log}n)$: 排序，堆

对于$O(n)$: 双指针，滑动窗口，前缀和，(原地)哈希，计数排序，快速选择，摩尔投票，

对于$O(\text{log}n)$: 二分



### 原地哈希 O(n) O(1)

https://leetcode.com/problems/first-missing-positive/

https://leetcode.com/problems/maximum-element-after-decreasing-and-rearranging/description/

将数组中每一个位置看作大小为1的桶：不断地交换数组中的数字到对应的下标位置

```c++
while(nums[i]!=i){
	swap(nums[i],nums[nums[i]]);
	if(nums[nums[i]]==nums[i]) return nums[i];
}
```

### 快慢指针 in array O(n) O(1)

https://leetcode.cn/problems/find-the-duplicate-number/

对数组构建图，每个位置$i$连一条$i\rightarrow \text{nums}[i]$的边。

> **数学推导:** 入环前走了a, 环长L, 环内走了b相遇, 相遇后再走c到达入环点 (b+c==L)
>
> 第一次相遇时: 2*step(slow)==step(fast)   
>
>  2(a+b)=a+kL+b 化简后
>
> a=(k-1)L + c
>
> 此时将慢指针重置，两指针正常速度走，相遇后即为入环点

### 摩尔投票 O(n) O(1)

https://leetcode.com/problems/majority-element-ii/

超过 n/k 个相同的元素，可以在与其它元素抵消的情况下剩余。



### 前缀和

https://leetcode.com/problems/split-array-largest-sum/ (dp)

[Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) 通过哈希表从O(n^2)降到O(n) 由于可能有负数，不能用滑动窗口

以O(1)的速度获得数组中从$i$一直加到$j$的结果



### 滑动窗口 lp++ rp++

前提：窗口内总和的单调性：当rp所在位置不符合条件时，rp再往右滑也不符合条件，只能移动lp

### 双指针 lp++ rp--

核心思想：当一个指针确定在某个位置，另外一个指针能够落在某个明确的分割点，使得左半部分满足，右半部分不满足。





### 二分

[Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/) Hard

核心思想：确定中点后，可以直接淘汰掉一半的搜索空间。 不止能二分排序数组，还可以二分答案



1. 

