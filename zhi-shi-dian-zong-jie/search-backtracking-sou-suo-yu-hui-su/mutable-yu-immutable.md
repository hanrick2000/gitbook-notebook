# Mutable 与 Immutable

对于backtracking，如果使用的是mutable的变量，即dfs得时候，用的是by reference指向同一个object/ collection，那就需要使用backtracking（即抹掉最后一个元素）

 利用 immutable 默认生成新 copy 的办法，当你 dfs 之间不是 by reference 指向同一个 object / collection ，也就不需要 backtracking 了。

### **Mutable**：

StringBuilder，List

通常使用`sb.toString()` 和 `new ArrayList(list)` 来新生成元素来避免答案中的元素改变

比如

#### [491. Increasing Subsequences](https://leetcode.com/problems/increasing-subsequences/description/)

```java
class Solution {
    Set<List<Integer>> res = new HashSet<List<Integer>>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        helper(nums, 0, new ArrayList<Integer>());
        List ans = new ArrayList(res);
        return ans;
    }
    
    private void helper(int[] nums, int pos, List<Integer> list){
        if(list.size()>=2)
            res.add(new ArrayList(list));
        
        for(int i=pos;i<nums.length;i++){
            if(list.size()==0||list.get(list.size()-1)<=nums[i]){
                list.add(nums[i]);
                helper(nums,i+1, list);
                list.remove(list.size()-1);
            }
        }
        
    }
}
```

### **Immutable**: 

String

