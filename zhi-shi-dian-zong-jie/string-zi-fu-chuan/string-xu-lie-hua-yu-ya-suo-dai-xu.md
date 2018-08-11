# String, 序列化与压缩（待续）

解决这一类问题的关键应该就是Escape字符

#### [Escape ](https://en.wikipedia.org/wiki/Escape_character)符的定义是："An escape character is a character which invokes an alternative interpretation on subsequent characters in a character sequence." {#escape-符的定义是：an-escape-character-is-a-character-which-invokes-an-alternative-interpretation-on-subsequent-characters-in-a-character-sequence}

## [Encode and Decode Strings](https://leetcode.com/problems/encode-and-decode-strings/)

这道题目现在被锁了，但是LintCode有一道类似的题目，但是我觉得应该是LeetCode上的test case更复杂，corner case更多，多Escape的选择更麻烦。等解锁以后要好好来想一想这个题目。

### [LintCode 659](https://www.lintcode.com/problem/encode-and-decode-strings/description)

这个代码看起来就非常简短，应该是考虑的待续还不够。就简单注意一下StringBuilder, 可以append String和Char, append char以后都是String。String.split\(里面的内容要用String类型\) 

```java
public class Solution {
    /*
     * @param strs: a list of strings
     * @return: encodes a list of strings to a single string.
     */
    public String encode(List<String> strs) {
        // write your code here
        if(strs==null||strs.size()==0) return "";
        StringBuilder sb = new StringBuilder();
        
        for(int i=0;i<strs.size();i++){
            sb.append(strs.get(i));
            sb.append(",");
        }
        return sb.toString();
    }

    /*
     * @param str: A string
     * @return: dcodes a single string to a list of strings
     */
    public List<String> decode(String str) {
        // write your code here
        List<String> res = new ArrayList<>();
        int len = str.length();
        if(str==null||len==0) return res;
        String[] strs = str.split(",");
        for(String s:strs){
            res.add(s);
        }
        return res;
    }
}

```

## \(Google\) [字符串嵌套压缩 / 解压缩](http://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=191823&highlight=google)\*\*\*\*

### \*\*\*\*[**变种1：**394. Decode String](https://leetcode.com/problems/decode-string/description/)

[http://www.1point3acres.com/bbs/thread-189365-1-1.html](http://www.1point3acres.com/bbs/thread-189365-1-1.html)

上来大概问了5分钟background然后直接上题，一个string decompression的题。。不知道是不是原题反正没见过。。题目如下.

2\[abc\]3\[a\]c =&gt; abcabcabcaaac; 2\[ab3\[d\]\]2\[cc\] =&gt; abdddabdddcc 输入 输出 一开始用了一个栈，写着写着嵌套的逻辑卡住了，最后用俩stack解决。。然后follow-up问的是不要输出string而是输出解压后的K-th character，主要也还是嵌套情况就从内到外把疙瘩解开以后再算。。然后我问俩问题就结束了。整体感觉妹子面试官人很nice 反应很快而且不是特别picky的那种。

还是不要太为难自己，建两个栈吧，特别是String和Integer混合出现的时候，一个栈会多出很多额外复杂的判断

同时注意栈顶元素和未压栈元素\(这里的res\)之间的关系

```java
class Solution {
    public String decodeString(String s) {
        Stack<String> stack = new Stack<>();
        Stack<Integer> num = new Stack<>();
        int len = s.length();
        String res = "";
        for(int i=0;i<len;i++){
            if(Character.isDigit(s.charAt(i))){
                int count = 0;
                while(Character.isDigit(s.charAt(i))){
                    count = 10*count+(s.charAt(i)-'0');
                    i++;
                }
                i--;
                num.push(count);
            }else if(s.charAt(i)==']'){
                StringBuilder sb = new StringBuilder(stack.pop());
                int repeat = num.pop();
                while(repeat>0){
                    sb.append(res);
                    repeat--;
                }
                res = sb.toString();
                
            }else if(s.charAt(i)=='['){
                stack.push(res);
                res = "";
            }else{
                res=res+s.charAt(i);
            }
        }
        return res;
    }
}
```

**变种2：**

“3a2\[mtv\]ac”, decompress to: aaamtvmtvac，括号可以嵌套。 这个我觉得不是很难，大概花了15分钟理清了思路并写好了代码，大概就是找匹配括号递归解，面试官也找不到bug表示认同。

但吊诡的地方来了，面试官说把这种字符串compress回去...这显然有多种情况，于是我问是不是要求压缩后最短，面试官说肯定越短越好。 比如对于aaaa, 肯定4a比2\[aa\]好。

