---
title: 198.House Robber
date: 2016-8-14 14:57:20
tags:
  - 算法
  - 动态规划
  - Leetcode
categories:
  - 算法
---
## 题目：
>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
>Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

&emsp;&emsp;太拗口了，说白点就是找数组不相邻组合最大值。

&emsp;&emsp;例子:原数组为[1,6,2,3,7,4,9,8];
&emsp;&emsp;最大结果为：22 --> 6 + 7 + 9;
<!-- more -->
## 思路
&emsp;&emsp;讲道理，我第一时间没想到咋做....与求最大连续子数组和不同，这个题需要考虑不能相邻的问题，因此想用暴力解法都没写出来。刚接触各种算法，不是很了解。看到讨论里都在说DP，我查了下才知道这题用动态规划算法很容易。

### 动态规划
&emsp;&emsp;动态规划一般用来求解最优化问题，其适用的条件是要求待求解的最优化问题具备两个因素：最优子结构和子问题重叠。即当前子问题的解将由上一次子问题的解推出。动态规划解决最优化问题的关键在于以下几点。
- 状态的定义
- 方案的枚举
- 最优子问题和无后效性保证
- 子问题有重叠
- 编写状态转移方程

&emsp;&emsp;我的理解，动态规划就是一种递推，用前面得到的最优解去规划下一步的最优解，而且下一步的解不能影响到之前的结果。动态规划最重要的就是总结出状态的转移方程，从最小子问题开始解决。

&emsp;&emsp;回到我们的问题上，要想求出一个数组不相邻组合最大值。那我们可以求出数组前1个值不相邻组合最大值、求出数组前2个值不相邻组合最大值、求出数组前3个值不相邻组合最大值...一直求到最后一个元素，那么这些结果中最大的值就是我们最终的结果了。这样我们就把一个大问题的最优解问题，分解为一个个更小问题的解了。
## 代码
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var len = nums.length,
        maxArr = [];
    if(len === 0){
        return 0;
    }else if(len == 1){
        return nums[0]
    }else{
        maxArr[0] = nums[0];
        maxArr[1] = Math.max(nums[0],nums[1]);
        for(var i = 2;i<len;i++){
            maxArr[i] = Math.max(maxArr[i-2] + nums[i],maxArr[i-1]);
        }
        return maxArr[len-1];
    }
    return Math.max(best0,best1);
};
```
&emsp;&emsp;我们用数组[1,6,2,3,7,4,9,8]这例子结合代码继续分析。

&emsp;&emsp;首先，我们分析前一个值不相邻组合最大值，因为只有一个数，当然是1了。前两个值不相邻组合最大值呢？就是前两个中最大的一个值。前三个值不相邻组合最大值呢？我们理解想到的就是 (nums[0]+nums[1])和nums[2]中较大的一个。可我们已经求出了分别到数组索引一和二的两个最优解了，此时最优解的数组`maxArr`为[1,6]。我们能否用之前的最优解，推出当前的最优解呢？

&emsp;&emsp;如果最优解数组`maxArr`中，当前下标的前两个数不一样，则说明最最大值计算使用了前一个数。例如当前`maxArr`下标为2，可是`maxArr[0]`和`maxArr[1]`的值不一样，则说明最大值计算中一定用到了`nums[1]`即6这个数，那其相邻的数就不能使用。我们在递推计算时最重要的规划就是是否累加当前的这个值，能否累加则受到之前最优解的影响。

&emsp;&emsp;我脑子还是有点绕...先列出递推关系吧：`  maxArr[i] = Math.max(maxArr[i-2] + nums[i],maxArr[i-1]);`
在循环到数组中的某个值时只有两种情况，加上这个值还是不加这个值。如果maxArr[i-2] == maxArr[i-1]，则说明nums[i-1]这个值没有参与累加，那么nums[i]就可以参与累加,而且此时max[i-2]+nums[i]一定大于max[i-1]，所以max[i-1] = max[i-2]+nums[i];如果maxArr[i-2] < maxArr[i-1],则说明nums[i-1]这个值参与了累加，所有在该状态下nums[i]不能参与运算。可是，那是上一状态的最优解，并不是这一状态的最优解，因此我们需要比较max[i-2]+nums[i]和max[i-1]到底哪个大，大的那个就是这一状态的最优解。

&emsp;&emsp;我理解的还不是很深刻，解释起来不是很清楚，看来还是需要再做几道同样的题加深理解。

## 思考
&emsp;&emsp;在这里还有另一种动态规划的程序写法，它只用两个变量来保存状态，空间复杂度更低，速度也更快。
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var best0 = 0,
        best1 = 0,
        temp;
    for(var i =0,len = nums.length;i<len;i++){
        temp = best0;
        best0 = Math.max(best0,best1);
        best1 = temp + nums[i];
    }
    return Math.max(best0,best1);
};
```
