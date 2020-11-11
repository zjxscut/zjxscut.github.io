
```java
class Solution {
    public int findRotateSteps(String ring, String key) {
        char[] rings = new char[ring.length()*3];
        char[] ss = ring.toCharArray();
        for (int i = 0; i < ss.length; i++) {
            rings[i] = ss[i];
            rings[i + ss.length] = ss[i];
            rings[i + ss.length*2] = ss[i];
        }
        int point = ss.length;
        int step = 0;
        int ans = 0;
        for (char sk : key.toCharArray()) {
            int left = point, right = point;
            while (rings[left] != sk)   left--;
            while (rings[right] != sk)  right++;
            if (point - left < right -right) {
                step = point - left;
                point = left;
            } else {
                step = right - point;
                point = right;
            }
            step++;
            ans += step;
            point = point % ss.length + ss.length;
        }
        return ans;
    }
}

```
