---
layout: post
title:  Leetcode 剑指 Offer 14- I. 剪绳子
categories: leetcode
description: leetcode
keywords: leetcode
---

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

### 推理过程：

如果将绳子分为长度为x和y两段，且x+y=n，则两段绳子的乘积为x*y：

<img src="http://latex.codecogs.com/gif.latex?n=x+y\ge 2*\sqrt{x*y}\Rightarrow n \ge 2*\sqrt{x*y}" title="x" />

对上式推导可以表述为：<img src="http://latex.codecogs.com/gif.latex?x*y \le \frac {n^{2}}{4}" title="x" />，其内在含义为当且仅当x==y时，取到最大值。

可以推广此概念到多段分段，也就是分段尽可能地均匀才可以使乘积最大（看官方题解才知道分段值最好是2和3，但做题时不太容易发现这个推论），于是将n的前几个值写出来，从2开始分别为：1、2、4(2x2)、6(2x3)、9(3x3)、12(2x2x3)、18(2x3x3)、24(3x3x3)、36(2x2x3x3)、54(2x3x3x3)。可以发现，n的值是依赖之前曾经计算过的值，且是一个峰形的值，可以得到计算公式val = dp[loc]\*(i - loc)，loc是i之前的序号。

由于是峰形的值分布，当峰形开始下降即停止即可获得最大值。但可以观察到n在较小的数值部分不符合这个规律，这种计算方法是计算分成3份或更多的情况，而分成两份则只需要折半分配，并将值进行比较即可。

### 代码如下：

```java
class Solution {
    public int cuttingRope(int n) {
        if (n <= 3) return n-1;
        int[] dp = new int[n + 1];
        dp[2] = 1; dp[3] = 2;

        for (int i = 4; i <= n; i++) {
            int val = 0, oldVal = 0;
            for (int loc = i-1; loc >= 2; loc--) {
                val = dp[loc]*(i - loc);
                if (val <= oldVal) {	// 有减小的趋势就停止计算
                    break;
                }
                oldVal = val;
            }
            val = Math.max(oldVal, (i/2) * (i - i/2)); // 与折半分的值进行比较
            dp[i] = val;
        }

        return dp[n];
    }
}
```

