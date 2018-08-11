---
description: 之前看到文章说其实很少会用贪心的方法去解决问题，但是我最近遇到了很多问题，他们感觉都是利用了贪心的思想，我把他们整理到这里
---

# Greedy

## [871. Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/description/)

解法1：动态规划

这里的DP感觉上很像是一个背包问题 ，甚至有点0/1背包问题的感觉，但是我还是有一点吃不准，等复习过背包问题以后再回过头来看看

解法2: heap 

这里说是PQ，但是我感觉有贪心的思想在里面——当耗尽的时候，上溯去找最大的stations

```java
class Solution {
    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
        
        int prev = 0, res = 0;
        for(int[] station : stations){
            int loc = station[0];
            int cap = station[1];
            startFuel -= loc - prev;
            while(!pq.isEmpty()&&startFuel<0){
                startFuel += pq.poll();
                res++;
            }
            if(startFuel<0)
                return -1;
            prev = loc;
            pq.offer(cap);
        }
        
        startFuel -= target - prev;
        while(!pq.isEmpty()&&startFuel<0){
            startFuel += pq.poll();
            res++;
        }
        if(startFuel<0)
            return -1;
        return res;
    }
}
```

## [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/description/)

Greedy的意思就是寻找当前位置的最优解，在本题中，从pos=0开始，找到第一个正确的位置，然后从此位置开始，递归查找剩下的字符串。

```java
public class Solution {
    public String removeDuplicateLetters(String s) {
        int[] cnt = new int[26];
        int pos = 0; // the position for the smallest s[i]
        for (int i = 0; i < s.length(); i++) cnt[s.charAt(i) - 'a']++;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) < s.charAt(pos)) pos = i;
            if (--cnt[s.charAt(i) - 'a'] == 0) break;
        }
        return s.length() == 0 ? "" : s.charAt(pos) + removeDuplicateLetters(s.substring(pos + 1).replaceAll("" + s.charAt(pos), ""));
    }
}
```

利用stack的做法

```java
class Solution {
    public String removeDuplicateLetters(String s) {
        char[] arr = s.toCharArray();
        int[] count = new int[256];
        for(char c:arr){
            count[c]++;
        }
        
        Stack<Character> stack = new Stack<Character>();
        for(int i=0;i<arr.length;i++){
            
            char tmp = arr[i];
            count[tmp]--;
            if(stack.contains(tmp)) continue;
            while(!stack.isEmpty()&&tmp<stack.peek()&&count[stack.peek()]>0){
                stack.pop();
            }
            stack.push(tmp);
        }
        
        StringBuilder sb = new StringBuilder();
        while(!stack.isEmpty()){
            sb.append(stack.pop());
        }
        sb.reverse();
        
        return sb.toString();
            
    }
}
```

