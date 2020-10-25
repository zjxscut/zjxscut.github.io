---
title: leetcode6-Z字形变换
date: 2019-09-17 20:21:34
tags: leetcode
---

[题目原文链接](https://leetcode-cn.com/problems/zigzag-conversion/)

由于它有numRows行，所以我们就定义每行对应一个string，把逻辑分为两步<!-- more -->，第一步，把全有字母的一列，从上往下依次输出到output的每个string里面，第二步，将Z字形中间部分的字符从下往上输出到output[numRows-2]到output[1]里面。

代码如下：

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        int i = 0, j;
        string result;
        vector<string> output(numRows);
        while(i < s.length()){
            for(j = 0; j < numRows && i < s.length(); j++){//输出全是字母的列到output
                output[j] += s[i++];
            }
            for(j = numRows - 2; j > 0 && i < s.length(); j--){//输出Z中间的字符
                output[j] += s[i++];
            }
        }
        for(i = 0; i < numRows; i++)  //合并numRows个数组
            result += output[i];
        return result;
    }
};
```

