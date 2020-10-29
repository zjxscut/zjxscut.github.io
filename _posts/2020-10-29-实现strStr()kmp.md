---
layout: post
title: Leetcode_实现strStr()kmp
categories: leetcode
description: leetcode
keywords: leetcode
---

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0 || needle == null){
            return 0;
        }
        if(haystack.length() < needle.length()){
            return -1;
        }
        int i, j;
        for(i = 0; i <= haystack.length() - needle.length(); i++){
            if(haystack.charAt(i) == needle.charAt(0)){
                for(j = 0; j < needle.length(); j++){
                    if(haystack.charAt(i + j) != needle.charAt(j)){
                        break;
                    }
                    if(j == needle.length() - 1){
                        return i;
                    }
                }
            }
        }
        return -1;
    }
}
```

kmp

```java
class Solution {
    public int[] getnext(String needle){
        int k = -1, j = 0;
        int[] next = new int[needle.length()];
        next[0] = -1;

        while(j < needle.length() - 1){
            if(k == -1 || needle.charAt(k) == needle.charAt(j)){
                k++;
                j++;
                next[j] = k;
            }
            else{
                k = next[k];
            }
        }
        return next;
    }
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0 || needle == null){
            return 0;
        }
        if(haystack.length() < needle.length()){
            return -1;
        }
        int[] next = getnext(needle);
        int i = 0, j = 0;
        while(i < haystack.length() && j < needle.length()){
            if(j == -1 || haystack.charAt(i) == needle.charAt(j)){
                j++;
                i++;
            }
            else{
                j = next[j];
            }
        }
        if(j == needle.length()){
            return i - j;
        }
        
        return -1;
    }
}
```

