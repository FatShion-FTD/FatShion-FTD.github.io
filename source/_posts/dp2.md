---
title: "[算法分析] 动态规划 DP 之 1"
date: 2022-12-07 16:07:02
categories: 学习
tags:   [算法, 刷题, 找工] 
keywords: 动态规划, Dynamic Programming, DP, 
---

# 动态规划 Dynamic Programming
第二部分, 就从 打家劫舍 开始吧.
## 例题讲解
### 打家劫舍系列
隔一个抢一个,不能连着抢

子问题: 在当前时间 (**第 i 家**), 是 **抢 i-1 家** 还是 **抢 i 和 i-2家** 赚

转移方程: **dp[i] = max(dp[i-1], dp[i-2] + val);**

House Robber: https://leetcode.com/problems/house-robber/
```java
public int rob(int[] nums) {
    if(nums.length == 0)    return 0;
    if(nums.length == 1)    return nums[0];
    if(nums.length == 2)    return Math.max(nums[1], nums[0]);
    
    int[] dp = new int[nums.length];
    dp[0] = nums[0];
    dp[1] = Math.max(nums[1], nums[0]);
    
    for(int i = 2; i < nums.length; i++){
        dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);
    }
    
    return dp[nums.length-1];
}
```


