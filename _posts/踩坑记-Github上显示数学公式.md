---
title: 踩坑记-Github上显示数学公式
date: 2019-09-15 21:40:17
tags: 踩坑记系列
---

数学公式在Markdown本地端看起来是没有问题的，可上传上去就会显示出源码<!-- more -->如：

```
$$
dp[i,j]=
\begin{cases}
0,\qquad\qquad\qquad\qquad\quad(s[i]!=s[j])\\
dp[i+1][j-1]+2\qquad(dp[i+1][j-1]!=0\quad\&\&\quad s[i]==s[j]
$$
```

解决方法，将上述代码替换为：

```
<img src="http://latex.codecogs.com/gif.latex?dp[i,j]=
\begin{cases}
0,\qquad\qquad\qquad\qquad\quad(s[i]!=s[j])\\
dp[i+1][j-1]+2\qquad(dp[i+1][j-1]!=0\quad\&\&\quad s[i]==s[j])
\end{cases}" title="x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" />
```

显示效果如下：

<img src="http://latex.codecogs.com/gif.latex?dp[i,j]=
\begin{cases}
0,\qquad\qquad\qquad\qquad\quad(s[i]!=s[j])\\
dp[i+1][j-1]+2\qquad(dp[i+1][j-1]!=0\quad\&\&\quad s[i]==s[j])
\end{cases}" title="x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" />

没看懂没关系，模板如下：

```
<img src="http://latex.codecogs.com/gif.latex?插入你的公式" title="x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" />
```

将“插入你的公式”替换为你的公式就可以了，数学公式语法为TeX语法。