---
title: "[算法分析] 动态规划 DP 之 1"
date: 2022-12-04 02:01:39
categories: 学习
keywords: 动态规划, Dynamic Programming, DP, 
tags:   [算法, 刷题, 找工] 
---

# 动态规划 Dynamic Programming

算法题里面最操蛋的, 想得出来就是想得出来, 想不出来拿铜头皮带把你抽的陀螺转你也想不出来的        
**动态规划**        
这是第一篇, 所以更像是 DP 从何而来, 要干什么, (去到哪里, 什么人生三问).       
拙见, 拙见. 我连工作都没有!! 我是个傻逼!!!

## 分析
动态规划来自于 DFS, 深度优先搜索. 在 DFS 中, 我们经常遇到一个问题, 就是重复搜索了. 明明这些东西都见过了, 但未来要跑的路, 还要再跑一遍, 这就导致了时间复杂度, 哧儿一下上去了. 所以就有了 记忆化 memorization, 来记忆一种名叫 **状态** 的东西. 状态是啥, 这不重要, 因为这个因问题而异. 但是有一点很重要, 状态记录了哪些东西见过了 或者 哪些东西他没见过, 这很重要, 因为有了这些信息, 跟据香农的信息熵理论, 我们就不再做无用功了, 因为可能性为 1 的事件, 不产生信息. 回到问题, 利用状态, 我们就能快速判断是否重复, 并在O(1)内给出当前状态下再往后走的解.

那么上述过程, 是一种 top-down 的寻找, 是我们定义好状态后, 去规避去躲开重复的路. 这其实已经是 DP 了, 但是这太被动了, 也不够 elegant (会不能和阿尼亚当同学的).

### 数学part, 跳过跳过!!
现在带一点数学, Markov Process 马尔可夫过程. 

```
設為一概率空間，另設集合為一指標集合。如果對於所有，均有一隨機變量定義於概率空間，則集合為一隨機過程。—Wikipedia
```

wiki我也看不懂, 只是贴一下. 我的拙见是: 首先这是个概率模型, 变量们互相变来变去是跟据一个概率来的, 这个过程就是 Markov Process. 下面的那个好理解.

A stochastic process is said to be Markov if   
$prob(s_{t+1}|s^t)=prob(s_{t+1}|s_{t})$      
where $s^{t}$ is the history up to period t and $s_{t}$ is the realization of the state at period t.


那这个玩意儿重要在哪里: 就是在随机过程中，明天的状态只取决于今天的状态. 和之前的状态无关啦。也就是不管你今儿是怎样达到 的，只要你今天的状态是 现在这个数值, 你明天状态的可能性就决定好了.

你看这个马尔可夫过程中确定明天状态的部分, 是不是和 DFS + memo 去躲避重复计算很像? 当然熟悉状态机的朋友, 一下也能看出来, 马尔可夫过程就是个概率分布的状态机. 

那跟据马尔可夫过程是不是也能做和 DFS + memo 相同的事了呢? 是的, 但是需要一个概率模型.

在 Markov Process 中, 这个概率模型 也被称之为 转移矩阵 (Transition Mtrix) 而最终结果也被叫做稳态 (stable state). 初始的概率分布, 在经过无数次矩阵乘法过后, 状态会逐渐收敛到稳态.

示例: 如果状态空间只包含了两个状态A和B。如果今天是A,那么明天还是A的概率为0.8。如果今日是B,那么明天还是B的概率为0.7。一开始状态在A, 求稳态方程如下:
```Matlab
%initial pi%
pi=[1,0];       % 表示初始概率分布, 随便换

%transistion matrix%
P=[0.8,0.2;0.3,0.7];

%critical value%
cv=1;           % 当前状态和上一状态的状态差

while cv>0.0000000001;      % 状态稳定, 即状态差收敛到够小
    pi2=pi*P;               % 当前状态 = 上一状态 * 转移矩阵
    cv=abs(min(pi2-pi));    % 状态差
    pi=pi2;
end
```

这个 pi 结果最后是 0.6 和 0.4. 不管换什么初始都会稳定在这里. 

此时问题来了, 面对问题, 我们从哪儿来这个转移矩阵? 答案是 对问题抽象, 寻找子问题. 我们看下上述代码, 无数次的矩阵乘法 就对应了 子问题的重复进行, 而初始状态就是我们问题开始时候的状态, 这个是固定的. 稳定态自然也不是咱们追求的, 咱们要的只是 问题所需的某一状态作为结束. 

再简化一下: 在有限状态转移下, 从固定初始状态到固定终结状态的有限状态转移问题. 

## 简而言之
最优子问题, 找 状态转移方程 进行子问题扩大 直到 原问题

思路: 1. 发现递归 -> 2. 从上到下的递归 -> 3. 加入 记忆 的递归 -> 4. 递归 转 迭代 + 记忆-> 5. 优化记忆空间

一般来说, 还是 直接 找 状态转移方程 直接写实在

例题: 打家劫舍1

## 例题
### 1. 发现递归: 是 **抢 i-1 家** 还是 **抢 i 和 i-2家** 赚
### 2. 从终末状态开始, 从上到下递归:
```java
public int rob(int[] nums) {
    return rob(nums, nums.length-1);
}
private int rob(int[] nums, int i){
    if(i < 0)   return 0;
    return Math.max(rob(nums, i-1), rob(nums, i-2) + nums[i]);
}
```
### 3. 加入 记忆 的递归
发现这家抢过了, 以后抢多抢少也确定了(算过了), 直接 return
```java
int[] memo;
public int rob(int[] nums) {
    memo = new int[nums.length + 1];
    return rob(nums, nums.length-1);
}
private int rob(int[] nums, int i){
    if(i < 0)   return 0;
    if(memo[i] != 0)    return memo[i];
    int res = Math.max(rob(nums, i-1), rob(nums, i-2) + nums[i]);
    memo[i] = res;
    return res;
}
```
### 4. 递归 转 迭代 + 记忆
状态转移从初始开始, 到终末状态结束. Markov Process 开始应用, 我不管你之前的状态了, 我就看后面. 从第一家开始抢, 到每家都看看, 是抢现在这家 + 上上一家 钱多 还是 抢上一家 钱多.
```java
public int rob(int[] nums) {
    if (nums.length == 0) return 0;
    int[] memo = new int[nums.length + 1];
    memo[0] = 0;
    memo[1] = nums[0];
    for (int i = 1; i < nums.length; i++) {
        int val = nums[i];
        memo[i+1] = Math.max(memo[i], memo[i-1] + val);
    }
    return memo[nums.length];
}
```
### 5. 优化记忆空间
熟悉编译里面状态机的朋友看到这个带入状态机就好了, 3状态: 上一家prev2, 上上一家prev1, 现在.
```java
public int rob(int[] nums) {
    if (nums.length == 0) return 0;
    int prev1 = 0;
    int prev2 = 0;
    for (int num : nums) {
        int tmp = prev1;
        prev1 = Math.max(prev2 + num, prev1);
        prev2 = tmp;
    }
    return prev1;
}
```