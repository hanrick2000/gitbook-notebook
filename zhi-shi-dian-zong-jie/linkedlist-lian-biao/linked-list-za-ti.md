# Linked List杂题

## [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

第一次听到这个题目，还是Intel当时的面试题目。现在看来真的是很简单的一个题目，遗憾当时错失了很好的一个机会

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = len(headA), lenB = len(headB);
        while(lenA>lenB){
            headA=headA.next;
            lenA--;
        }
        
        while(lenB>lenA){
            headB=headB.next;
            lenB--;
        }
        
        while(headA!=headB){
            headA=headA.next;
            headB=headB.next;
        }
        
        return headA;
    }
    
    private int len(ListNode list){
        if(list==null) return 0;
        if(list.next==null) return 1;
        ListNode head = list;
        int len = 1;
        while(true){
            head=head.next.next;
            if(head==null)
                return 2*len;
            else if(head.next==null)
                return 2*len+1;
            len++;
            
        }
    }
}
```

## [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)

#### 拼接链表，认准多个 dummy node. {#拼接链表，认准多个-dummy-node}

```java
public class Solution {
    public ListNode oddEvenList(ListNode head) {
        ListNode oddDummy = new ListNode(0);
        ListNode evenDummy = new ListNode(0);
        ListNode odd = oddDummy;
        ListNode even = evenDummy;

        boolean isOdd = true;
        while(head != null){
            if(!isOdd){
                even.next = head;
                even = even.next;
            } else {
                odd.next = head;
                odd = odd.next;
            }
            head = head.next;
            isOdd = !isOdd;
        }
        odd.next = evenDummy.next;
        even.next = null;

        return oddDummy.next;
    }
}
```

## [143. Reorder List](https://leetcode.com/problems/reorder-list/description/)

这道题目综合了Linked list的reverse和，merge，算是非常综合的一道题目。

**复习的时候可以多留意一下**

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
    public void reorderList(ListNode head) {
        if(head==null||head.next==null) return;
        
        ListNode prev=null, fast = head, slow = head, l1 = head;
        while(fast!=null&&fast.next!=null){
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }
        
        prev.next = null;
        ListNode l2 = reverse(slow);
        
        merge(l1, l2);
    }
    
    private ListNode reverse(ListNode node){
        ListNode dummy = new ListNode(0);
        dummy.next = node;
        ListNode prev = dummy;
        ListNode cur = node;
        while(cur.next!=null){
            ListNode tmp = cur.next;
            cur.next = tmp.next;
            tmp.next = prev.next;
            prev.next = tmp;
        }
        return dummy.next;
    }
    
    private void merge(ListNode l1, ListNode l2){
        while(l1!=null){
            ListNode n1 = l1.next, n2 = l2.next;
            l1.next = l2;
            
            if(n1==null)
                break;
            l2.next = n1;
            l1 = n1;
            l2 = n2;
        }
        
        
    }
}
```

