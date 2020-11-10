---
layout: post
title:  Leetcode_剑指 Offer 03. 数组中重复的数字
categories: leetcode
description: leetcode
keywords: leetcode
---

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

#### 1、使用Set

第一时间想到的就是使用set数据结构：

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> s = new HashSet();
        for (int i : nums) {
            if (s.contains(i)) {
                return i;
            } else {
                s.add(i);
            }
        }
        return 0;
    }
}
```

#### 2、使用数组进行记录

Set容器使用起来会有一定程度的时间消耗，所以开辟一个长度re为给定数组长度的数组，当re[i]值为0时，i元素没有出现过，当re[i]为1时，i元素曾经出现过。遍历给定的数组nums，如果遍历的元素i出现过(re[i]等于1)就立即返回，未出现将re[i]的值设为1.

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int[] re = new int[nums.length];
        for (int i : nums) {
            if (re[i] == 1) {
                return i;
            } else {
                re[i] = 1;
            }
        }
        return 0;
    }
}
```

#### 3、减少空间复杂度的方法

先排序，再检测是否有重复的元素。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Arrays.sort(nums);
        int old = -1;
        for (int i : nums) {
            if (i == old) {
                return i;
            }
            old = i;
        }
        return 0;
    }
}
```

#### 4、原地置换

遍历一遍，将每个元素i放在nums[i]格子里面，只要有重复元素就一定会重复放置。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int t = nums[i];
            if (i == t) {
                continue;
            }
            if (nums[t] == t) {
                return t;
            }
            
            nums[i] = nums[t];
            nums[t] = t;
            
        }
        return 0;
    }
}
```

