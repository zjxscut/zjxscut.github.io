---
title: leetcode8-字符串转换整数
date: 2019-09-19 21:15:42
tags: leetcode
---

[题目原文链接](https://leetcode-cn.com/problems/string-to-integer-atoi/)

　　此题数据集设置的很严格，容易踩坑的数据有（"   +12"，"   -12"，" 2  -12"，" ##2  -12"，"-"，"-91283472332"，"3.114"，"+-3"），需严谨审题以及多次试错才可以完全写对。<!-- more -->

　　我们注意题目中的条件：

　　　- 空串不转换
　　　- 字符串开头可以有一串空格字符
　　　- 空格字符后的数字字符串才是我们需要转换的数据
　　　- 数字字符串首部可以有‘-’和‘+’字符，但仅能出现一个(仅有字符没有数字的这种情况需考虑)。
　　　- 如果数字字符串有其他非数字字符，则后面数字字符不计入转换范围（如："3.114"—>3）
　　　- 数字字符串首部如果出现任何非空格字符，则返回零

　　那我们可以采用这么一种思路去解决问题：

　　　1、空串判断

　　　2、去除掉字符串首部的连续空格

　　　3、判断剩下的字符串首部是否正负符号

　　　4、转换数字字符为long型，并检验是否超过表示范围

　　　5、返回正确的转换整数

代码如下：

```c++
class Solution {
public:
    int myAtoi(string str) {
        bool sign = 0;
        int i;
        long result = 0;
        if(str.length() == 0)  //非空字符串判断
            return 0;
        for(i = 0; str[i] == ' ' && i < str.length(); i++);//去除首部连续空格
        if((str[i] > '9' || str[i] < '0')){    //判断剩下字符串首部是否正负符号
            if(str[i] == '-' || str[i] == '+'){
                if(str[i++] == '-')
                    sign = 1;
            }
            else
                return 0;
        }
        for(; i < str.length(); i++){
            if(str[i] >= '0' && str[i] <= '9'){  //转换数字，通过乘10，使result已存在数字往高位移动
                result *= 10;
                result += str[i] - '0';
                if(result > INT_MAX && sign == 0)   //溢出判断
                    return INT_MAX;
                if(result > 2147483648 && sign == 1) //溢出判断，因为result没有添加符号，所以需要对符号进行判断
                    return INT_MIN;
            }
            else
                break;
        }
        if(sign == 1)
            result = -result;
        return result;
    }
};
```

