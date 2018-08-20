# Array中的Binary Search（待续）

## Arrays.binarySearch\(\)的用法

```java
import java.util.Arrays;

public class Arraydemo1 {
    public static void main (String [] args){
        //定义一个int型的数组
        int []arr ={8,1,2,5,4,9};

        System.out.println("toString:"+Arrays.toString(arr));
        //调用Arrays的排序方法
        Arrays.sort(arr);
        System.out.println("toString:"+Arrays.toString(arr));
        System.out.println("--------------------------------");
        System.out.println("binarySearch:"+Arrays.binarySearch(arr, 8));//4
        System.out.println("binarySearch:"+Arrays.binarySearch(arr, 26));//-7
        System.out.println("binarySearch:"+Arrays.binarySearch(arr, 6));//-5
        System.out.println("binarySearch:"+Arrays.binarySearch(arr, 0));//-1
    }

}

结果:

排序前:[8, 1, 2, 5, 4, 9]
排序后:[1, 2, 4, 5, 8, 9]
--------------------------------
关键字8的返回值是:4
关键字26的返回值是:-7
关键字6的返回值是:-5
关键字0的返回值是:-1

分析:
 当我们查找的关键字是8在该数组中,则返回的结果是该数组排好序之后的下标4;(找到关键字索引从0开始)
 接着我们查找关键字26,我们发现这个关键字并不在该数组中,并且26比数组中的所有元素都大,所以返回值是数组length+1,也就是6+1=7;  (没有找到关键字索引从1开始）
 接着我们查找关键字6,由于排好序的数组中第一个大于6的数是8所以返回值就是8所在的索引5;
 (没有找到关键字索引从1开始）
 接着我们查找关键字0,由于在该数组中比0稍大的数是1,则返回1的索引1;(没有找到关键字索引从1开始）

 之所以计算插入点值时索引要从1开始算，是因为-0=0，如果从0开始算，那么上面例子中关键字2和关键字4的返回值就一样了。

这篇博客我是参考了下面这个兄弟的博客园的总结才明白的:
[参考原文]( http://www.cnblogs.com/qingergege/p/5658292.html)
```

## [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

这道题虽然和常规的Binary Search不太一样，但是思考之后，寻找单调区间的思路是一样的

**可以根据 A\[mid\] 与 A\[right\] 的大小关系，先行判断 mid 一定位于哪一端；**

**对于已经确定 mid 左/右 的数组，必然有一段区间满足单调性，可以利用来做 binary search.**

```java
class Solution {
    public int search(int[] nums, int target) {
        int len = nums.length;
        if(len==0) return -1;
        //if(len==1) return nums[0] == target?0:-1;
        int hi = len - 1, lo = 0;
        while(lo<hi){
            int mid = (hi-lo)/2+lo;
            
            if(nums[mid]==target) return mid;
            
            if(nums[mid]>nums[hi]){
                if(target<nums[mid]&&target>=nums[lo]){
                    hi = mid-1;
                }else{
                    lo = mid+1;
                }
            }else{
                if(target>nums[mid]&&target<=nums[hi])
                    lo=mid+1;
                else
                    hi = mid-1;
            }
        }
        
        return nums[lo] == target ? lo : -1;
    }
}
```

## 154. Find Minimum in Rotated Sorted Array II

与上面那个题目类似，只是数组中存在重复的元素

题目变难的原因在于出现了新情况，即我们不知道最小值到底在 mid 的左边还是右边，比如【3,\[3\],1,3】，中间的 3 和两端值都相等，无法正确地直接砍掉一半。

* **我们依然可以靠 A\[mid\] 与 A\[right\] 的大小关系来二分搜索；**
  * **A\[mid\] &gt; A\[right\] 时，mid 在左半边，最小值一定在 mid 右侧；**
  * **A\[mid\] &lt; A\[right\] 时，mid 在右半边，最小值一定在 mid 左侧；**
* **A\[mid\] == A\[right\] 时，无法判断，把 right 向左挪一格。**

```java
class Solution {
    public int findMin(int[] nums) {
        int len = nums.length;
        int lo = 0, hi = len-1;
        while(lo<hi){
            int mid = (hi-lo)/2+lo;
            
            if(nums[mid]<nums[hi])
                hi = mid;
            else if(nums[mid]>nums[hi])
                lo = mid+1;
            else
                hi--;
        }
        return nums[lo];
    }
}
```

### follow up:

* This is a follow up problem to [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/).
* Would allow duplicates affect the run-time complexity? How and why?

## [540. Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/description/)

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int len = nums.length;
        int lo = 0, hi = len-1;
        while(lo<hi){
            int mid=(hi-lo)/2+lo;
            if(mid%2==1) mid-=1;
            if(nums[mid]!=nums[mid+1])
                hi=mid;
            else
                lo=mid+2;
        }
        return nums[lo];
    }
}
```

## [873. Length of Longest Fibonacci Subsequence](https://leetcode.com/problems/length-of-longest-fibonacci-subsequence/description/)

## [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/description/)

