# 扫描线

## [Range Addition](https://leetcode.com/problems/range-addition/)

数组长度为 n ，更新次数为 k 的话，比较 trivial 的做法就是 O\(nk\) 的暴力解。

然而最优解是 O\(n + k\)，利用的算法逻辑类似 meetings rooms 的扫描线算法。

仔细思考的时候用的是类似构造法： 先想只有一个 update 操作的时候，然后逐个叠加，寻找新的共同性质。

![](https://mnmunknown.gitbooks.io/algorithm-notes/content/range_addition.jpg)

不难看出我们可以用一个 "carry" 来表示我们目前 interval 的覆盖结果；比如 +4 和 -2 覆盖的地方，净增长一定是 +2，就像扫描线的时候带着当前的 interval / building 一样。同时我们需要定义一个 “起始” 和 “结束” ，代表着当前区间的覆盖结果，对 carry 值进行修正。

因此，只要在起始的位置 +val ，在终止的后一个位置 -val，两次操作就可以保证整个范围的正确更新。

```java
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
        if(length==0) return new int[0];
        int[] res = new int[length];
        for(int[] update : updates) {
            int start = update[0];
            int end = update[1];
            int inc = update[2];
            
            res[start] += inc;
            if(end+1<length) res[end+1] -= inc;
        }
        int carry = 0;
        for(int i=0;i<length;i++) {
            res[i] += carry;
            carry = res[i];
        }
        
        return res;
    }
}
```



