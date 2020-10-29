# 面试题 17.16. 按摩师

#### 题目

&emsp;&emsp;一个有名的按摩师会收到源源不断的预约请求，每个预约都可以选择接或不接。在每次预约服务之间要有休息时间，因此她不能接受相邻的预约。给定一个预约请求序列，替按摩师找到最优的预约集合（总预约时间最长），返回总的分钟数。(转自leetcode)

#### 思路

&emsp;&emsp;为按摩师选择合适的按摩时间，首先，当预约数大于等于两个时，预约时间一定会选择最后两个的其中一个，而选择两个之中的其中一个i，则i的状态依赖于第i-2或者第i-3个预约时长。我们可以根据这种状态依赖反推，状态转移公式如下：
$$
s[i]=\begin{equation}  
\left\{ 
\begin{array}{**lr**}  
             nums[i] + s[i-2],&(i-3<0)&  \\
             nums[i] + max(s[i-2],s[i-3]),&(i-3>=0) &   
             \end{array}  
\right.  
\end{equation}  
$$
&emsp;&emsp;考虑到特殊情况：

&emsp;&emsp;&emsp;&emsp;预约为0，返回0；

&emsp;&emsp;&emsp;&emsp;预约为1，返回nums[0]；

&emsp;&emsp;为了节约空间利用，直接将nums的数组改写。最后只需要返回数组最后两个值中最大的一个即可。

代码如下：

```java
class Solution {
    public int massage(int[] nums) {
        if(nums.length == 0)    return 0;
        if(nums.length == 1){
            return nums[0];
        }

        for(int i = 2; i < nums.length; i++){
            if(i >= 3){
                nums[i] += Math.max(nums[i-2], nums[i-3]);
            }else{
                nums[i] += nums[0];
            }
        }
        return Math.max(nums[nums.length-1], nums[nums.length-2]);
    }
}
```

