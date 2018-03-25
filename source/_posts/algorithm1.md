---
title: 39.Combination Sum
date: 2016-8-13 12:02:36
tags:
  - 算法
  - 回溯法
  - Leetcode
categories:
  - 算法
---
## 题目
&emsp;&emsp;在leetcode上遇到一道题，详细如下：
> 39.Combination Sum
>
>  Given a set of candidate numbers C and a target number T, find all unique combinations in C where the candidate numbers sums to T.
>
>  The same repeated number may be chosen from C unlimited number of times.
>
>  Note:
>  - All numbers (including target) will be positive integers.
>
>  - The solution set must not contain duplicate combinations.
>
>  For example, given candidate set [2, 3, 6, 7] and target 7,
>  A solution set is:
> [ [2, 2, 3] , [7] ]

&emsp;&emsp;简单来说就是，已知一数组和一目标值，用数组中的数累加得到目标值。找出所有的可能组合，数字可以复用但必须是数组中出现的数字，结果不能出现重复的组合。
	<!-- more -->
## 思路
&emsp;&emsp;我的想法是先将数组从小到大排序，然后按回溯法检测。如果累加结果大于8，退回，累加列表中下一个值；如果累加结果等于8，保存结果，退回，累加列表中下一个值；如果累加结果小于8，累加列表中下一个值。实现递归计算所有可能结果，并且不出现重复项。

> &emsp;&emsp;回溯算法也叫试探法，它是一种系统地搜索问题的解的方法。回溯算法的基本思想是：从一条路往前走，能进则进，不能进则退回来，换一条路再试。
> &emsp;&emsp;用回溯算法解决问题的一般步骤为：
> 1. 定义一个解空间，它包含问题的解。
> 2. 利用适于搜索的方法组织解空间。
> 3. 利用深度优先法搜索解空间。
> 4. 利用限界函数避免移动到不可能产生解的子空间。

## 代码

我使用的是JavaScript，代码效率相比其他语言是真的低。。
```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function(candidates, target) {
    var index = 0,
        res = [],
        tempList = [];
    //从大到小排序列表
    candidates.sort(function(a,b){return (a-b)});
    //回溯计算
    backtrack(res,tempList,candidates,target,0);
    return res;
};

/**
 * 回溯算法
 * @param {number[][]} list     结果数组
 * @param {number[]} templist   保存当前路径结果
 * @param {number[]} candidates 整数列表
 * @param {number} remain       结果差值
 * @param {number} start        起始坐标
 * @return
 */
function backtrack(list,templist,candidates,remain,start){
    if(remain < 0){
        return;
    }else if(remain === 0){
        list.push(templist.slice(0));
    }else{
        for(var i = start; i < candidates.length; i++){
            templist.push(candidates[i]);
            arguments.callee(list,templist,candidates,remain - candidates[i],i);
            templist.pop();
        }
    }
}
```
运行结果如下：
![运行结果](http://7xk5u3.com1.z0.glb.clouddn.com/algorithm1-1.png)

&emsp;&emsp;在判断结果时，我选择了使用remain保存差值而不是累加值，这样可以减少需要传递的参数。

&emsp;&emsp;这里有一个小坑，就是当`remain === 0`时，需要此时的临时数组结果保存在最终结果时，我最初使用了`list.push(templist)`，最终结果是两个空子数组。子数组的个数没错，但为什么结果为空了？这是因为JavaScript的数组是引用类型的。在程序运行结束后，临时列表中的数据全部pop完，即空数组。所以最终结果也就剩空数组了，如果还不懂原因可以看看我的一篇 [文章](http://gentlemanczh.com/2016/08/10/transmit/)。现在我使用`list.push(templist.slice(0));`代替原程序，因为`templist.slice(0)`会返回一个新数组，且不会修改原数组。

## 思考
&emsp;&emsp;之后我觉得，这个代码还有优化的空间。因为我先将给定的数组，从小到大进行了排序。因此如果在计算时，结果已经超过了8，就没必要退回再累加之后的值了，因为之后一定都大于8。

&emsp;&emsp;例如当前的临时列表[2,2,2,2]，在判断结果后，remain明显是小于0的。因此退回，临时列表结果为[2,2,2]。但由于for循环，需要再累加之后的数，临时列表[2,2,2,3]。可之后的数一定大于2，那remain结果自然小于0。如果我在这种情况下直接跳出循环，直接退回到[2,2]，则会得到优化。

我将代码做了如下改动：
```javascript
/**
 * 回溯算法
 * @param {number[][]} list     结果数组
 * @param {number[]} templist   保存当前路径结果
 * @param {number[]} candidates 整数列表
 * @param {number} remain       结果差值
 * @param {number} start        起始坐标
 * @return
 */
function backtrack(list,templist,candidates,remain,start){
    if(remain < 0){
        return true;
    }else if(remain === 0){
        list.push(templist.slice(0));
        return true;
    }else{
        for(var i = start; i < candidates.length; i++){
            templist.push(candidates[i]);
            var flag = arguments.callee(list,templist,candidates,remain - candidates[i],i);
            templist.pop();
            if(flag){
                break;
            }
        }
    }
    return false;
}
```
&emsp;&emsp;此时，backtrack函数会返回一个布尔值。若果remain小于等于0，返回true，否则返回false。如果外层函数，检测到内层嵌套返回true，则说明上次的传入值已经使得累计值超出了目标值，不必再循环传入更大的参数了。因此立即break跳出当前for循环，进行之后的递归。

&emsp;&emsp;在运行测试后，虽然结果全部通过，但时间并没有得到很明显的优化。我认为由于每次测试所需时间有所浮动，而且可能由于数据长度的原因，没有明显体现出来性能优化。或者是我的思路有问题，如果您有想法可以再当且页面下留言。
