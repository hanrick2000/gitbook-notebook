# LinkedList, 反转与删除

## LinkedList常用操作

* 翻转
* 找 kth element
* partition
* 排序
* 合并

## 常用技巧

### dummy node

Dummy node主要是运用在删除节点的时候，可以避免头指针的判断。

但是过度刀reverse上，也可以运用这种思想，所以dummy node应用的地方还是挺广泛的

* 快慢指针
* 多个 ptr

## 例题

下面三个题目都采用了dummy的写法

### [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

reverse这类题目还是比较绕的，但是还是有相对固定的一个模板，可以参照下面的写法

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
    public ListNode reverseList(ListNode head) {
        if(head==null) return head;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        
        ListNode start = dummy.next;
        ListNode then = start.next;
        while(start!=null&&then!=null){
            start.next = then.next;
            then.next = dummy.next;
            dummy.next = then;
            then = start.next;
        }
        return dummy.next;
    }
}
```

### [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;
        int count = n-m;
        while(m-1>0){
            pre = pre.next;
            m--;
        }
        ListNode start = pre.next;
        ListNode then = start.next;
        while(count>0){
            start.next = then.next;
            then.next = pre.next;
            pre.next = then;
            then = start.next;
            count--;
        }
        return dummy.next;
    }
}
```

### [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

涉及链表删除操作的时候，稳妥起见都用 dummy node，省去很多麻烦。因为不一定什么时候 head 就被删了。

注意最后删除元素的写法

```java
 slow.next = slow.next.next;
```

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode fast = dummy;
        ListNode slow = dummy;
        while(n-1>0){
            fast = fast.next;
            n--;
        }
        
        while(fast.next.next!=null){
            fast = fast.next;
            slow = slow.next;
        }
        
        slow.next = slow.next.next;
        
        return dummy.next;
    }
}
```

## 构造一个链表

### \*\*\*\*[**Add Two Numbers II**](https://leetcode.com/submissions/detail/165366099/) **（向前面插值）**

这道题目注意两个地方：

（1）reverse的时候（或者说涉及到反序）考虑使用**Stack**

（2）注意在LinkedList的开头插值的写法：

```java
 ListNode dummy = new ListNode(0);
 ListNode head = new ListNode(sum/10);
            dummy.val = sum%10;
            head.next = dummy;
            dummy = head;
```

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
        Stack<Integer> s1 = new Stack<Integer>();
        Stack<Integer> s2 = new Stack<Integer>();
        
        while(l1!=null){
            s1.push(l1.val);
            l1 = l1.next;
        }
        
        while(l2 != null){
            s2.push(l2.val);
            l2 = l2.next;
        }
        
        ListNode dummy = new ListNode(0);
        int sum = 0; 
        while(!s1.isEmpty()||!s2.isEmpty()){
            int n1 = s1.isEmpty()?0:s1.pop();
            int n2 = s2.isEmpty()?0:s2.pop();
            sum +=  n1 + n2;
            
            ListNode head = new ListNode(sum/10);
            dummy.val = sum%10;
            head.next = dummy;
            dummy = head;
            sum /=10;
            
        }
        return dummy.val == 0?dummy.next:dummy;
    }
}
```

