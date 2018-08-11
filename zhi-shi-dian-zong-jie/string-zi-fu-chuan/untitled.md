# String 大数运算/ Linked List

## [Add Binary](https://leetcode.com/problems/add-binary/)

这题代码简洁的重点在于处理“终止条件”，因为两个 string 很可能长度不一，也有可能两个 string 加完了之后还有进位没处理。

这段代码给出的答案是：

* **输入长短不一就都放在 while loop 里，在读取字符时把短的做 padding.**
* **While loop 里三个 'OR' 的条件保证了三种不同情况下都会继续读取，而其他两个自动 pad 0.**

```java
class Solution {
    public String addBinary(String a, String b) {
        int len1 = a.length()-1, len2 = b.length()-1;
        int carry = 0;
        int n1 = 0, n2 = 0;
        StringBuilder sb  = new StringBuilder();
        
        while(len1>=0||len2>=0){
            if(len1<0){
                n1 = 0;
            }else
                n1 = a.charAt(len1--) - '0';
            if(len2<0)
                n2 = 0;
            else
                n2 = b.charAt(len2--) - '0';
            int sum = n1 + n2 + carry;
            int temp = sum%2;
            carry  = sum/2;
            sb.append(temp);
            
        }
        if(carry==1)
            sb.append(1);
        return sb.reverse().toString();
    }
}
```

## [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

 上题完全一样的思路应用在链表上就是这样了。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode head = dummy;
        int carry = 0;
        
        while(l1!=null||l2!=null||carry>0){
            int n1 = (l1==null?0:l1.val);
            int n2 = (l2==null?0:l2.val);
            int sum = n1 + n2 + carry;
            
            head.next = new ListNode(sum%10);
            head = head.next;
            carry = sum/10;
            
            
            if(l1 != null) l1 = l1.next;
            if(l2 != null) l2 = l2.next;
        }
        
        return dummy.next;
    }
}
```

## [Multiply Strings](https://leetcode.com/problems/multiply-strings/)

这个[帖子](https://leetcode.com/problems/multiply-strings/discuss/17605/Easiest-JAVA-Solution-with-Graph-Explanation) 里面的方法算法巧妙地应用了乘法地特性，同时注意了下面三个技巧：

* **两个位数为 m 和 n 的数字相乘，乘积不会超过 m + n 位。**
* **乘法操作从右往左计算时，每次完成相加就确定了当前 digit.**
* **同一个 digit 多次修改，用 int\[ \].**

**使得代码更加高效**

```java
class Solution {
    public String multiply(String num1, String num2) {
        int len1 = num1.length(), len2 = num2.length();
        char[] arr1 = num1.toCharArray();
        char[] arr2 = num2.toCharArray();
        int[] pos = new int[len1+len2];
        
        int n1 = 0, n2 = 0;
        for(int i=len1-1;i>=0;i--){
            for(int j=len2-1;j>=0;j--){
                int mul = (arr1[i]-'0')*(arr2[j]-'0');
                int p1 = i+j, p2 = i+j+1;
                int sum  = mul + pos[p2];
                
                pos[p1] += sum/10;
                pos[p2] = sum%10;
            }
        }
        
        StringBuilder sb = new StringBuilder();
        for(int p : pos) if(!(sb.length() == 0 && p == 0)) sb.append(p);
        return sb.length() == 0 ? "0" : sb.toString();
    }
}
```

