# 914.卡牌分组

#### 题目

给定一副牌，每张牌上都写着一个整数。

此时，你需要选定一个数字 X，使我们可以将整副牌按下述规则分成 1 组或更多组：

&emsp;&emsp;每组都有 X 张牌。
&emsp;&emsp;组内所有的牌上都写着相同的整数。
仅当你可选的 X >= 2 时返回 true。(来源：leetcode)

#### 思路：最大公约数

&emsp;&emsp;根据题意，通过判断数组中每个数出现的次数，如[1,1,2,2,2,2]，每个数出现次数为2、4，则获取最小出现次数2，依次去对每个出现次数取余即可。

&emsp;&emsp;进一步思考，当[1,1,1,1,2,2,2,2]时，每个数出现次数为4、6，然而此样例为true，因为它们最大公约数为2，可以通过求取出现次数的最大公约数。

&emsp;&emsp;再进一步思考，当出现次数为14、21、22时，14和21的最大公约数为7，而14和22最大公约数为2，这个用例为false，由于需要对所有数进行均匀划分，所以，应该对于所有出现次数取最大公约数，即，让一个变量min_s来保存最大公约数，依次用min_s去和每个出现次数取最大公约数，min_s最后结果不为0，则返回true。

&emsp;&emsp;程序思路：

&emsp;&emsp;&emsp;&emsp;1、对数组进行排序

&emsp;&emsp;&emsp;&emsp;2、计数每个数出现的个数，塞进vector里面

&emsp;&emsp;&emsp;&emsp;3、用min_s等于第一个vector值，然后依次求min_s与vector中每个数的最大公约数

```java
import java.util.Vector;
class Solution {
    public int gcd(int m, int n){
        return n==0 ? m:gcd(n, m%n);
    }
    public boolean hasGroupsSizeX(int[] deck) {
        Vector<Integer> v = new Vector<Integer>();
        Arrays.sort(deck);			//排序
        int q = deck[0], sum = 0;
        for(int i:deck){			//对deck中每个出现次数计数，并存入v
            if(i == q)  sum++;
            else{
                v.addElement(sum);
                sum = 1;
                q = i;
            }
        }
        v.addElement(sum);	//添加最后一个数出现的次数
        int min_s = (int)v.get(0);	//初始化最大公约数
        for(int i = 0; i < v.size(); i++){
            min_s = gcd((int)v.get(i), min_s);	//计算min_s与vector中每个数的最大公约数
            if(min_s == 1)    return false;
        }
        return true;
    }
}
```

#### 优化

&emsp;&emsp;由于vector需要额外占用空间，并且调用数据时间过长，可以用已经遍历过的deck数组进行存储vector中的数据，只需要额外利用一个len变量即可。

```java
import java.util.Vector;
class Solution {
    public int gcd(int m, int n){
        return n==0 ? m:gcd(n, m%n);
    }
    public boolean hasGroupsSizeX(int[] deck) {
        Arrays.sort(deck);		//排序
        int q = deck[0], sum = 0, len = 0;
        for(int i:deck){			//对deck中每个出现次数计数，并存入v
            if(i == q)  sum++;
            else{
                deck[len++] = sum;
                sum = 1;
                q = i;
            }
        }
        deck[len++] = sum;	//添加最后一个数出现的次数
        int min_s = deck[0];	//初始化最大公约数
        for(int i = 0; i < len; i++){
            min_s = gcd(deck[i], min_s);
            if(min_s == 1)    return false;	//计算min_s与vector中每个数的最大公约数
        }
        return true;
    }
}
```

