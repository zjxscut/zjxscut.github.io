---
title: leetcode5.最长回文子串
date: 2019-09-15 17:15:45
tags: leetcode
---

[题目原文链接](https://leetcode-cn.com/problems/longest-palindromic-substring/)

##### （1）dp法
　　最长回文字符串是一道非常典型的动态规划题，我们可以假定起点为start，回文串的长度为len。如果s\[start...(start\+len\-1)\]是回文串\(len大于2\)，那么s\[\(start+1\)\.\.\.\(start\+len\-2\)\]也是一个回文串，且s\[start\]==s\[start\+len\-1\]。于是，可以把最长回文子串问题分解为一系列子问题进行求解，首先构造状态转移方程<!-- more -->

<img src="http://latex.codecogs.com/gif.latex?dp[i,j]=
\begin{cases}
0,\qquad\qquad\qquad\qquad\quad(s[i]!=s[j])\\
dp[i+1][j-1]+2\qquad(dp[i+1][j-1]!=0\quad\&\&\quad s[i]==s[j])
\end{cases}" title="x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" />

　　根据这个方程，我们可以知道，判断s\[i...j\]是否是回文串，只依赖于dp\[i+1\]\[j-1\]的状态和s[i]、s[j]是否相等。

　　初始化一个空间dp\[len][len]，使	

        dp[i][i] = 1
        dp[i][i+1] = 2        (if s[i] == s[i+1])

代码如下：

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int len = s.length();
        int longest = 1, start = 0;
        int i;
        if(len == 0 || len == 1)         //对字符串长度为0和1的字符串进行特殊处理
            return s;
        vector<vector<int>> dp(len, vector<int>(len));     //建立一个len*len的dp数组
        for(i = 0; i < s.length(); i++){       //初始化dp数组的内容，标记长度为1和2的回文串
            dp[i][i] = 1;
            if((i < s.length()-1) && s[i] == s[i+1]){
                dp[i][i+1] = 2;
                longest = 2;
                start = i;
            }
        }
        for(int k = 3; k <= len; k++){     //假设字符串长为k
            for(i = 0; i < len - k + 1; i++){        //根据dp[i+1][j-1]的状态来确定dp[i][j]是否回文串
                if(s[i] == s[i + k - 1] && dp[i+1][i + k -2] != 0){
                    dp[i][i + k - 1] = k;
                    longest = k;
                    start = i;
                }
            }
        }
        return s.substr(start, longest);
    }
};
```

　　dp数组的作用在于记录子问题结果，避免了重复计算子问题，算法复杂度为O(n<sup>2</sup>)。



##### （2）Manacher法

　　其实可以想到，逐个字符，以字符为中心扩展，假定字符串半径为k，判断s[i+k]和s[i-k]是否相等，相等k+1，让s_len数组记录每个字符扩展的k，这样得出的字符串最大长度为k*2+1。如下所示：

|   s   |  a   |  a   |  a   |  b   |  a   |
| :---: | :--: | :--: | :--: | :--: | :--: |
| s_len |  0   |  1   |  0   |  0   |  0   |

　　但是这样做有个最大的问题：

　　　1、判断出来的回文串只能是奇数长度的串。

　　　2、会有重复计算。

　　让我们先解决第二个问题，在从左往右遍历，确定以每个字符为中心的最长回文串长度，假设从2P0-P指向的字符到P指向的字符是一个已经判断过的回文串，以P0处字符为中心。

　　现在该检测i指向字符为中心的字符串，可以发现，在以P0为对称轴，在这个回文串内(这个条件很关键)，j是i的对称位置，如果j的最长回文串在T[2P0-P...P]内，那i处的最长回文串跟j处的长度一样，如果j的最长回文串超过了T[2P0-P...P]的范围，那么i处的回文串长度暂定为p - i，然后再往两边搜索找到i为中心的最长回文串。

<img src="http://img.blog.csdn.net/20141221160212654" style="zoom:10%;" >

　　这样就解决了重复计算的问题，只有没有查找过的位置才进行查找，所以，能使它的时间复杂度为线性。

让我们解决第二个问题，其实这是一个很巧妙的想法，就是字符中间全插入一个特殊字符，如‘#’，见下图。

<img src="http://img.blog.csdn.net/20141221160159348" style="zoom:10%;">

　　因为插入后长度扩展了一倍，所以len[i]-1既为以原始串i处字符为中心的回文串长度，对于偶数长度回文串，它的中心字符是#，它的起始长度=(i - T_len[i] + 1)/2。因为只是把字符串长度扩展到2n+1，所以它的时间复杂度依旧是线性O(n)。

代码如下：

```c++
public:
    string longestPalindrome(string s) {
        int len = s.length();
        int longest = 1, start = 0;
        int i, p0 = 0, p = 0;
        string T;
        T.append(1, '#');
        for(i = 0; i < len ; i++){  //每个字符间插入字符#
            T.append(1, s[i]);
            T.append(1, '#');
        }
        vector<int> T_len(T.length());
        for(i = 0; i < T.length(); i++){   //以每个字符为中心进行回文串搜索
            T_len[i] = 1;
            if(i < p){        //判断是否搜索过
                T_len[i] = min(T_len[2 * p0 - i], p - i);  //已经判断的回文串过是否在以p指向字符为结束字符的回文串内
            }
            //在上述判断的基础上网两边再搜索一下，注意搜索越界问题
            while(i + T_len[i] < T.length() && i - T_len[i] >= 0 && T[i + T_len[i]] == T[i - T_len[i]])
                T_len[i]++;
            if(i + T_len[i] > p){//更新最右回文串的位置
                p0 = i;
                p = i + T_len[i];
            }
            if(T_len[i] - 1 > longest){//更新已经搜索到的最长回文串位置
                longest = T_len[i] - 1;
                start = (i - T_len[i] + 1)/2;
            }
        }
        return s.substr(start, longest);
    }
};
```

