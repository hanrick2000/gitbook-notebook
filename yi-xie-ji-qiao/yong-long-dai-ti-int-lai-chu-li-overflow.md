# 用long代替int来处理overflow

## [29. Divide Two Integers](https://leetcode.com/problems/divide-two-integers/description/)

不用乘除号去做除法，做法就是计算被除数的n次倍，看什么时候能到除数。

其实就是**Binary Search（注意这样一种Binary Search的变种）**

这题还有一个重点就是，在除的过程中可能出现溢出， Integer.MIN\_VALUE as dividend is really troublesome. 这个时候采用long型避免溢出，再最后的答案中再全部转换回int型。

```java
class Solution {
    public int divide(int dividend, int divisor) {
        int sign = 1;
        if ((dividend > 0 && divisor < 0) || (dividend < 0 && divisor > 0))
		    sign = -1;
        long ldividend = Math.abs((long) dividend);
	    long ldivisor = Math.abs((long) divisor);
        
        if ((ldividend == 0) || (ldividend < ldivisor))	return 0;
        
        long lans = ldivide(ldividend, ldivisor);
        
        int ans;
        if(lans> Integer.MAX_VALUE){ //Handle overflow.
		    ans = (sign == 1)? Integer.MAX_VALUE : Integer.MIN_VALUE;
	    } else {
		    ans = (int) (sign * lans);
	    }
	        return ans;
    }
    
    private long ldivide(long ldividend, long ldivisor) {
	// Recursion exit condition
	if (ldividend < ldivisor) return 0;
	
	//  Find the largest multiple so that (divisor * multiple <= dividend), 
	//  whereas we are moving with stride 1, 2, 4, 8, 16...2^n for performance reason.
	//  Think this as a binary search.
	long sum = ldivisor;
	long multiple = 1;
	while ((sum+sum) <= ldividend) {
		sum += sum;
		multiple += multiple;
	}
	//Look for additional value for the multiple from the reminder (dividend - sum) recursively.
	return multiple + ldivide(ldividend - sum, ldivisor);
}
```

这个[帖子](https://leetcode.com/problems/divide-two-integers/discuss/13417/No-Use-of-Long-Java-Solution)介绍了不用long去handle溢出的问题，但是其实我没有太看懂，但是还抽时间回来理解这种做法

```java
public class Solution {
    public int divide(int dividend, int divisor) {
		if(dividend==Integer.MIN_VALUE && divisor==-1) return Integer.MAX_VALUE;
        if(dividend > 0 && divisor > 0) return divideHelper(-dividend, -divisor);
        else if(dividend > 0) return -divideHelper(-dividend,divisor);
        else if(divisor > 0) return -divideHelper(dividend,-divisor);
        else return divideHelper(dividend, divisor);
    }
    
    private int divideHelper(int dividend, int divisor){
        // base case
        if(divisor < dividend) return 0;
        // get highest digit of divisor
        int cur = 0, res = 0;
        while((divisor << cur) >= dividend && divisor << cur < 0 && cur < 31) cur++;
        res = dividend - (divisor << cur-1);
        if(res > divisor) return 1 << cur-1;
        return (1 << cur-1)+divide(res, divisor);
    }
}

```

