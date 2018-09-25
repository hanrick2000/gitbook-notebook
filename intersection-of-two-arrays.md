# Intersection of Two Arrays

题目本省很简单（easy难度，有人说FB的电面是一个medium和一个easy）

主要是后面的三个follow-up

## [Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/description/)

**Follow up:**

* What if the given array is already sorted? How would you optimize your algorithm? \(Binary search\)
* What if nums1's size is small compared to nums2's size? Which algorithm is better? \(hash map\)
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

3——

* If only nums2 cannot fit in memory, put all elements of nums1 into a HashMap, read chunks of array that fit into the memory, and record the intersections.
* If both nums1 and nums2 are so huge that neither fit into the memory, sort them individually \(external sort\), then read 2 elements from each array at a time in memory, record intersections.

最先想到的方法是使用hash map：

Let **m**=nums1.size\(\), and **n**=nums2.size\(\)

**Solution 1: hashtable** \(using unordered\_map\).

* time complexity: max\(O\(m\), O\(n\)\)
* space complexity: choose one O\(m\) or O\(n\) &lt;--- So choose the smaller one if you can

然后想到了

**Solution 2: sort + binary search**

* time complexity: max\(O\(mlgm\), O\(nlgn\), **O\(mlgn\)**\) or max\(O\(mlgm\), O\(nlgn\), **O\(nlgm\)**\)
* O\(mlgm\) &lt;-- sort first array
* O\(nlgn\) &lt;--- sort second array
* O\(mlgn\) &lt;--- for each element in nums1, do binary search in nums2
* O\(nlgm\) &lt;--- for each element in nums2, do binary search in nums1
* space complexity: depends on the space complexity used in your sorting algorithm, bounded by max\(O\(m\), O\(n\)\)



