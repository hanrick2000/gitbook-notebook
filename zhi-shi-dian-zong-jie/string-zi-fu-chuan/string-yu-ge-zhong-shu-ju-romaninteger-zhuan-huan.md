# String与各种数据（Roman/Integer）转换

最近这一类的题目做的有点多，总结起来：

1. 直接用String类型比用String Builder来的简单，因为可以拼接任意长度的字符串，直接用"+"就好了。而且使用string可以更轻松地解决从前面插入的问题。但是又评论指出，用String builder的效率更高。总之优先用String吧
2. 遇到固定几种组合的数据，可以用Array直接把这些组合列出来，用下标去搜索就好了
3. 活用trim\(\)函数，注意使用时应该直接return String.trim\(\);
4. 这类问题可以考虑使用switch

## [273. Integer to English Words](https://leetcode.com/problems/integer-to-english-words/description/)

```java
class Solution {
    private final String[] LESS_THAN_20 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
private final String[] TENS = {"", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
private final String[] THOUSANDS = {"", "Thousand", "Million", "Billion"};

    public String numberToWords(int num) {
        if(num==0) return "Zero";
    
        int i=0;
        String res = "";
        while(num>0){
            if(num%1000!=0)
                res = helper(num%1000)+THOUSANDS[i]+" "+res;
            num/=1000;
            i++;
        }
        
        
        return res.trim();
    }
    private String helper(int num){
            if (num == 0)
                return "";
            else if(num<20) return LESS_THAN_20[num]+" ";
            else if(num<100)
                return TENS[num/10]+" "+helper(num%10);
            else
                return LESS_THAN_20[num / 100] + " Hundred " + helper(num % 100); 
        }
}
```

## [8. String to Integer \(atoi\)](https://leetcode.com/problems/string-to-integer-atoi/description/)（Microsoft）

这道题目也是烦人的代表，各种corner case。有校友说他在面微软的时候，被问了很多这个问题的内容，要注意准备：

我觉得这个问题在对是否超越Integer类型的处理上十分有趣

```java
 if(Integer.MAX_VALUE / 10 < base || Integer.MAX_VALUE / 10 == base && Integer.MAX_VALUE % 10 < digit)
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
```

```java
class Solution {
    public int myAtoi(String str) {
        str = str.trim();
        int len = str.length();
        if(len<=0) return 0;
        char[] ch =  str.toCharArray();
        
        int index = 0, sign = 1, base = 0;
        if(ch[index]=='+'||ch[index]=='-'){
            sign = ch[index]=='+'?1:-1;
            index++;
        }
        while(index<len){
            int digit = ch[index] - '0';
            if(digit<0||digit>9) break;
            
            if(Integer.MAX_VALUE / 10 < base || Integer.MAX_VALUE / 10 == base && Integer.MAX_VALUE % 10 < digit)
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            
            base = base*10 + digit;
            index++;
        }
        return base*sign;
    }
}
```

