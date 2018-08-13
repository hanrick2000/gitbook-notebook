# Missing Number 类，元素交换，数组环形跳转, Duplicate

[556. Next Greater Element III](https://leetcode.com/problems/next-greater-element-iii/description/)

注意开头 `char[] arr = (n+"").toCharArray()`的用法：

int型的元素想要交换内部的元素位置，还是采用这种做法最简便。其实直接在String上做in-place的交换的时候，并不会节省太多的空间，因为String是immutable的，所以一旦操作，就会产生新的string

