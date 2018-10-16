# Evolve from recursion to dp, 尾递归

{% embed url="https://leetcode.com/problems/decode-ways/discuss/30451/Evolve-from-recursion-to-dp" %}

上面这个帖子介绍了从递归到DP的思路流程，写的非常好：

## [91. Decode Ways](https://leetcode.com/problems/decode-ways/discuss/30451/Evolve-from-recursion-to-dp)

### 1. Recursion O\(2^n\)

```java
 int numDecodings(string s) {
        return s.empty() ? 0: numDecodings(0,s);    
    }
    int numDecodings(int p, string& s) {
        int n = s.size();
        if(p == n) return 1;
        if(s[p] == '0') return 0;
        int res = numDecodings(p+1,s);
        if( p < n-1 && (s[p]=='1'|| (s[p]=='2'&& s[p+1]<'7'))) res += numDecodings(p+2,s);
        return res;
```

### 2. Memoization O\(n\)

可以看到从普通递归到记忆化搜索，最主要的变化就是新增了一个Mem数组，用于保存已有的数据，当再次访问的时候，可以直接返回值。其时间复杂度与DP一致

```java
 int numDecodings(string s) {
        int n = s.size();
        vector<int> mem(n+1,-1);
        mem[n]=1;
        return s.empty()? 0 : num(0,s,mem);   
    }
    int num(int i, string &s, vector<int> &mem) {
        if(mem[i]>-1) return mem[i];
        if(s[i]=='0') return mem[i] = 0;
        int res = num(i+1,s,mem);
        if(i<s.size()-1 && (s[i]=='1'||s[i]=='2'&&s[i+1]<'7')) res+=num(i+2,s,mem);
        return mem[i] = res;
    }
```

### 3. dp O\(n\) time and space, this can be converted from \#2 with copy and paste.

```java
int numDecodings(string s) {
        int n = s.size();
        vector<int> dp(n+1);
        dp[n] = 1;
        for(int i=n-1;i>=0;i--) {
            if(s[i]=='0') dp[i]=0;
            else {
                dp[i] = dp[i+1];
                if(i<n-1 && (s[i]=='1'||s[i]=='2'&&s[i+1]<'7')) dp[i]+=dp[i+2];
            }
        }
        return s.empty()? 0 : dp[0];   
    }
```

### 4. dp constant space

因为这道题目实际上只与之前的两位数有关，所以我们其实可以只用两个变量保存之前的数据就好了

```java
int numDecodings(string s) {
        int p = 1, pp, n = s.size();
        for(int i=n-1;i>=0;i--) {
            int cur = s[i]=='0' ? 0 : p;
            if(i<n-1 && (s[i]=='1'||s[i]=='2'&&s[i+1]<'7')) cur+=pp;
            pp = p;
            p = cur;
        }
        return s.empty()? 0 : p;   
    }
    
```

## 尾递归

作者：知乎用户  
链接：https://www.zhihu.com/question/20761771/answer/92233964  
来源：知乎  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。  
  
这是尾递归：  


```text
function f(x) {
   if (x === 1) return 1;
   return f(x-1);
}
```

（这个程序没什么意义，仅作为理解辅助之用）。

这不是尾递归：

```text
function f(x) {
   if (x === 1) return 1;
   return 1 + f(x-1);
}
```

后者不是尾递归，是因为该函数的最后一步操作是用1加上f\(x-1\)的返回结果，因此，最后一步操作不是调用自身。注意，后面这段代码的递归也发生在函数末尾，但它不是尾递归。**尾递归的判断标准是函数运行最后一步是否调用自身，而不是是否在函数的最后一行调用自身。**因此阶乘n!的递归求法，尽管看起来递归发生在函数末尾，其实也不是尾递归：

```text
function factorial(n) {
   if (n === 1) return 1;
   return n * factorial(n-1); // 最后一步不是调用自身，因此不是尾递归
}
```

使用尾递归可以带来一个好处：因为进入最后一步后不再需要参考外层函数（caller）的信息，因此没必要保存外层函数的stack，递归需要用的stack只有目前这层函数的，因此避免了栈溢出风险。

一些语言提供了尾递归优化，当识别出使用了尾递归时，会相应地只保留当前层函数的stack，节省内存。

所以，在没有尾递归优化的语言，如java, python中，鼓励用迭代iteration来改写尾递归；在有尾递归优化的语言如Erlang中，鼓励用尾递归来改写其他形式的递归。

 关于如何改写，参考：[尾调用优化 - 阮一峰的网络日志](https://link.zhihu.com/?target=http%3A//www.ruanyifeng.com/blog/2015/04/tail-call.html)

## [306. Additive Number](https://leetcode.com/problems/additive-number/description/)

关于尾递归转迭代的题目，可以注意复习这一道题目

