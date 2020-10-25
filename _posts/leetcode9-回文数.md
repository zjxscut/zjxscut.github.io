---
title: leetcode9_回文数
date: 2019-09-20 20:16:51
tags: leetcode
---

[题目原文链接](https://leetcode-cn.com/problems/palindrome-number/submissions/)

　　因使用string进行判断过于简单，故此处不详述。

　　在不将int型转换为字符串的情况下，考虑到无法确定int值的长度，所以使用前后数字对比会比较麻烦，那就考虑直接构造一个数字x_new<!-- more -->，这个数字刚好将传入值x的低位和高位颠倒（如：x=3245，那x_new=5423）。

　　此题还需要考虑以下题目特征：

　　　当x为负数时，不为回文数

　　　当0 <= x < 10时，x为回文数

　　　接下来剩下的只剩下大于等于10的正整数了（要考虑溢出，所以用long类型去构造x_new）

代码如下：

```c++
class Solution {
public:
    bool isPalindrome(int x) {
        int temporary = x;
        long x_new = 0;
        if(x < 0)
            return false;
        if(x < 10)
            return true;
        while(temporary != 0){  //将x的高位和低位翻转，赋值给x_new
            x_new *= 10;
            x_new += temporary%10;
            temporary /= 10;
        }
        if(x_new == x)
            return true;
        return false;
    }
};
```

