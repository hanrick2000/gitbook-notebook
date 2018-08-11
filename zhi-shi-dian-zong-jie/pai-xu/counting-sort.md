# counting sort

## [274. H-Index](https://leetcode.com/problems/h-index/description/)

This type of problems always throw me off, but it just takes some getting used to. The idea behind it is some ~~bucket sort~~  **\(counting sort\)**mechanisms. First, you may ask why bucket sort. Well, the h-index is defined as the number of papers with reference greater than the number. So assume `n` is the total number of papers, if we have `n+1` buckets, number from 0 to n, then for any paper with reference corresponding to the index of the bucket, we increment the count for that bucket. The only exception is that for any paper with larger number of reference than `n`, we put in the `n`-th bucket.

Then we iterate from the back to the front of the buckets, whenever the total count exceeds the index of the bucket, meaning that we have the index number of papers that have reference greater than or equal to the index. Which will be our h-index result. The reason to scan from the end of the array is that we are looking for the greatest h-index. For example, given array `[3,0,6,5,1]`, we have 6 buckets to contain how many papers have the corresponding index. Hope to image and explanation help.

![Buckets](http://i67.tinypic.com/2yvpfv5.jpg)

```java
class Solution {
    public int hIndex(int[] c) {
        int len = c.length;
        int[] bucket = new int[len+1];
        for(int n:c){
            if(n>len)
                bucket[len]++;
            else
                bucket[n]++;
        }
        
        int count = 0;
        for(int i=len;i>=0;i--){
            count+=bucket[i];
            if(count>=i)
                return i;
        }
        return -1;
    }
}
```

