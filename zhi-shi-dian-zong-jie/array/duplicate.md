# Duplicate

## [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/description/)

```java
class Solution {
    public int findDuplicate(int[] nums) {
        if(nums.length>1){
            int slow = nums[0];
            int fast = nums[nums[0]];
            
            while(slow!=fast){
                slow = nums[slow];
                fast = nums[nums[fast]];
            }
            
            fast = 0;
            while(fast!=slow){
                fast = nums[fast];
                slow = nums[slow];
            }
            return slow;
        }
        return -1;
    }
}
```

**Complexity Analysis**

* Time complexity : O\(n\)O\(n\)

  For detailed analysis, refer to [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/solution/#approach-2-floyds-tortoise-and-hare-accepted).

* Space complexity : O\(1\)O\(1\)

  For detailed analysis, refer to [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/solution/#approach-2-floyds-tortoise-and-hare-accepted).

这个算法叫[Floyd's Cycle Detection Algorithm](https://blog.csdn.net/l947069962/article/details/77774737)

