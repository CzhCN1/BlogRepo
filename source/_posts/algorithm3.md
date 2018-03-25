---
title: 169. Majority Element && 229. Majority Element II
date: 2016-8-16 15:29:39
tags:
  - 算法
  - 摩尔投票法
  - Leetcode
categories:
  - 算法
---
## 题目
>Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.
>You may assume that the array is non-empty and the majority element always exist in the array.

&emsp;&emsp;给第一个长度为n的数组，找到众数，该众数出现次数应超过n/2次。即该数出现的次数超过数组长度的一半。而且题目说明了，这个数组一定非空并且一定存在符合要求的众数。
<!-- more -->
## 思路
&emsp;&emsp;首先可以使用暴力解法，用一个数组统计每个值出现的次数，这就和求数组中出现次数最多的数是同一个问题了。然后遍历该数组，出现次数最多的那个当然就是。可是时间和空间复杂度都很高，不够优雅。

&emsp;&emsp;重读一遍题，如果要求该众数出现次数要超过一半，也就说明在数组中一定只有一个符合该要求的数(题目中说明一定存在该众数)。如果我们每次都删掉数组中两个数值不同的一组数，那么最后数组中剩下的数就一定是我们要求得的众数了。仔细想一想，是不是这个道理！

&emsp;&emsp;这里的思想就是**摩尔投票法**。摩尔投票法的基本思想很简单，在每一轮投票过程中，从数组中找出一对不同的元素，将其从数组中删除。这样不断的删除直到无法再进行投票，如果数组为空，则没有任何元素出现的次数超过该数组长度的一半。如果只存在一种元素，那么这个元素则可能为目标元素。

## 代码
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function(nums) {
    var a,
        cnt = 0;
    for(var i = 0,len = nums.length;i < len;i++){
        if(cnt === 0 || nums[i] == a){
            cnt++;
            a = nums[i];
        }else{
            cnt--;
        }
    }
    return a;
};
```
&emsp;&emsp;变量`a`保存上次出现的值，`cnt`保存变量a的值共出现几次。如果`cnt!=0`,而且`nums[i]!=a`，则说明本次和上次的值不同。我们并未真正删除掉该值，只是把cnt减1处理。如果`nums[i]==a`则`cnt++`。遍历整个数组后，变量a中则保存着我们需要的众数。

## 思考
&emsp;&emsp;上面的例子只是先了解一下这个算法，上题还有一个变形题是 229. Majority Element II。

### 题目
>Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times. The algorithm should run in linear time and in O(1) space.

&emsp;&emsp;给定一个长度为n的整数数组,找到数组中所有出现次数超过n/3的元素。要求线性时间复杂度和常数空间复杂度。
&emsp;&emsp;这道题和上题基本一样，只是众数的定义从超过数组的一半改为了超过三分之一。有了复杂度的要求，暴力解法就不再适用这道题了。而且少了<u>You may assume that the array is non-empty and the majority element always exist in the array.</u> 这个条件。

### 思路
&emsp;&emsp;看完题的要求，就应该猜到这题需要用到摩尔投票法。首先我们想想，这个众数最多能同时出现几个？两个。每个众数都会占大于三分之一的比例，两个就超过了三分之二。其余的数加起来也占不到三分之一，因此最多只可能存在两个众数。这与上道题只能存在一个众数的想法一样。

&emsp;&emsp;接下来就是套摩尔投票的模型了，如果我们每次投票去掉三个不同的数，最后剩下的就有可能是我们的众数了。这里为什么说是可能呢？因为题目没有说一定存在该众数呀！

### 代码
```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var majorityElement = function(nums) {
    var cnt1 = 0,
        cnt2 = 0,
        a,
        b;
    for(var i =0,len = nums.length;i<len;i++){
        if(cnt1 === 0 || nums[i] == a){
            cnt1++;
            a = nums[i];
        }else if(cnt2 === 0 || nums[i] == b){
            cnt2++;
            b = nums[i];
        }else{
            cnt1--;
            cnt2--;
        }
    }

    cnt1 = 0;
    cnt2 = 0;
    for(i =0,len = nums.length;i<len;i++){
        if(nums[i] == a){
            cnt1++;
        }else if(nums[i] == b){
            cnt2++
        }
    }

    var res = [];
    if(cnt1 > len/3){
        res.push(a);
    }
    if(cnt2 > len/3){
        res.push(b);
    }
    return res;
};
```
&emsp;&emsp;因为可能的众数为2,因此我们用了两组变量来分布记录。有了第一道题的经验，我们很快也就能搭起程序框架了。思路就是每有三个不一样的值时，投票"删掉"。即`nums!=a`而且`nums!=b`,cnt均大于1，那么就删掉这组的三个数。假设输入的数组为[1,3,2,3,4]我们来手动跑一遍这个程序：

nums[i]  |   | 1 | 3 | 2 | 3 | 4 
---------|---|---|---|---|---|---
a        |und| 1 | 1 | 1 | 3 | 3
cnt1     | 0 | 1 | 1 | 0 | 1 | 1
b        |und|und| 3 | 3 | 3 | 4
cnt2     | 0 | 0 | 1 | 0 | 0 | 1

&emsp;&emsp;从结果中看到，a最终结果为3,b最终结果为4。可我们一眼就可以看出来3确实是我们需要的众数，而4不是我们需要的众数。这也是我们为什么要加上众数检测的原因。

&emsp;&emsp;众数检测的思路很简单，遍历数组，把a变量和b变量出现的次数统计出来，计算是否出现次数超过三分之一。如果是则为我们求得的众数。

### 小BUG
&emsp;&emsp;上面的程序中有一个小坑，有某些情况会不能AC。例如[1,2,2,3,2,1,1,3]，结果应该为[2,1]，可程序跑出来的是[1]。为什么不能通过？再来跑一遍看看。

nums[i]  |   | 1 | 2 | 2 | 3 | 2
---------|---|---|---|---|---|---
a        |und| 1 | 1 | 1 | 1 | 2
cnt1     | 0 | 1 | 1 | 1 | 0 | 1
b        |und|und| 2 | 2 | 2 | 2
cnt2     | 0 | 0 | 1 | 2 | 1 | 1

&emsp;&emsp;不需要写完，到这一步我们就知道错在哪了。此时nums[i]为2，cnt1虽然等于0，可是b的值等于2，cnt2等于1。按照程序下一步进行，输入1，,会导致cnt1、cnt2都减1，即这组数被删除。可这组数并不是三个不同的数，其中有两个2，这并不符合我们的规则。所以在这步我们应该空出给a赋值2,而是给cnt2加1。

&emsp;&emsp;对第一个if的判断条件做如下修改即可：
将`if(cnt1 === 0 || nums[i] == a)`改为`if((cnt1 === 0 || nums[i] == a) && (nums[i] != b))`。即加上了判断当前值是否等于b,因此就解决了上述问题。你可能会问，如果cnt2等于0，此时nums[i] == b 也会使得判断跳转。其实这个并不会影响程序的流程和结果，不信的你可以纸上推一推。

## 总结
&emsp;&emsp;摩尔投票法在执行过程中，我们使用常量空间实时记录一个候选元素a以及其出现次数cnt，a即为当前阶段出现次数超过要求的元素。根据这样的定义，我们也可以将摩尔投票法看作是一种动态规划算法。

&emsp;&emsp;又是动态规划。。。到现在我还是没搞清楚DP，下一次应该会再试着讲一道DP题加深理解。
