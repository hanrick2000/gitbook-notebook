# Union-Find，并查集基础

并查集是一个用于解决 disjoint sets 类问题的常用数据结构，用于处理集合中

* **元素的归属 find**
* **集合的融合 union**
* **Online algorithm, stream of input**
* **计算 number of connected components**
* **不支持 delete**

[CSDN 有一个非常好的总结帖子](http://blog.csdn.net/dm_vincent/article/details/7655764)

处理这类题目，可以直接写一个UF的class出来，再在主函数中调用接口就好了

## Union-Find的class模板

下面是Princeton算法课中的UF模板，采用了array的写法：

注意在32行的循环，这其实利用了一个路径压缩的优化，将所有节点都联通到同一个根节点上，降低了查找的时间。

```java
public class WeightedQuickUnionPathCompressionUF {
    private int[] parent;  // parent[i] = parent of i
    private int[] size;    // size[i] = number of sites in tree rooted at i
                           // Note: not necessarily correct if i is not a root node
    private int count;     // number of components

    public WeightedQuickUnionPathCompressionUF(int N) {
        count = N;
        parent = new int[N];
        size = new int[N];
        for (int i = 0; i < N; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    /**
     * @return the number of components
     */
    public int count() {
        return count;
    }


    /**
     * @return the component identifier for the component containing site 
     */
    public int find(int p) {
        int root = p;
        while (root != parent[root])
            root = parent[root];
        while (p != root) {
            int newp = parent[p];
            parent[p] = root;
            p = newp;
        }
        return root;
    }

   /**
     * @return true if the two sites p and qare in the same component;
     *         false otherwise
     */
    public boolean connected(int p, int q) {
        return find(p) == find(q);
    }

    /**
     * Merges the component containing site p with the 
     * the component containing site q.
     *
     * @param  p the integer representing one site
     * @param  q the integer representing the other site
     */
    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ) return;

        // make smaller root point to larger one
        if (size[rootP] < size[rootQ]) {
            parent[rootP] = rootQ;
            size[rootQ] += size[rootP];
        }
        else {
            parent[rootQ] = rootP;
            size[rootP] += size[rootQ];
        }
        count--;
    }
```

但是再实际应用中用HashMap来做parent和Size有着更优越的性能，比如下面这个题目：

### [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

#### 其实我们建的是对于每一个元素的 1-1 mapping，或者说是一个 元素之间的 graph，表示 join 关系。 {#其实我们建的是对于每一个元素的-1-1-mapping，或者说是一个-元素之间的-graph，表示-join-关系。}

中间有一个 \[2147483646,-2147483647,0,2,2147483644,-2147483645,2147483645\] 的 test case 始终出 bug，把 find 函数的返回 type 从 int 改到 Integer 就好了。

看来以后不能总是假设 int 和 Integer 是完全相等的，尤其是这种在 hashmap 里以 Integer 为 Key 的情况，要尽可能的保持类型正确。

```java
public class Solution {
    private class WeightedUnionFind{
        private HashMap<Integer, Integer> parent;
        private HashMap<Integer, Integer> size;
        private int maxSize;

        public WeightedUnionFind(){}
        public WeightedUnionFind(int[] nums){
            int N = nums.length;
            parent = new HashMap<Integer, Integer>();
            size = new HashMap<Integer, Integer>();
            maxSize = 1;

            for(int i = 0; i < N; i++){
                parent.put(nums[i], nums[i]);
                size.put(nums[i], 1);
            }
        }

        private int getMaxSize(){
            return this.maxSize;
        }

        // With path compression
        public Integer find(Integer num){
            if(!parent.containsKey(num)) return null;

            Integer root = num;
            while(root != parent.get(root)){
                root = parent.get(root);
            }
            while(num != root){
                Integer next = parent.get(num);
                parent.put(num, root);
                num = next;
            }

            return root;
        }

        public void union(int p, int q){
            Integer pRoot = find(p);
            Integer qRoot = find(q);

            if(pRoot == null || qRoot == null) return;
            if(pRoot == qRoot) return;

            int pSize = size.get(pRoot);
            int qSize = size.get(qRoot);



            if(pSize > qSize){
                parent.put(qRoot, pRoot);
                size.put(pRoot, pSize + qSize);
                maxSize = Math.max(maxSize, pSize + qSize);
            } else {
                parent.put(pRoot, qRoot);
                size.put(qRoot, pSize + qSize);
                maxSize = Math.max(maxSize, pSize + qSize);
            }
        }


    }
    public int longestConsecutive(int[] nums) {
        if(nums == null || nums.length == 0) return 0;

        WeightedUnionFind uf = new WeightedUnionFind(nums);

        for(int num : nums){
            if(num != Integer.MIN_VALUE) uf.union(num, num - 1);
            if(num != Integer.MAX_VALUE) uf.union(num, num + 1);
        }

        return uf.maxSize;
    }
}
```

* **仅仅对于这题而言，还有其他的做法，比如每次看到新元素都往两边扫。不过不是这章内容的主题。**
* **另一种简单易懂的做法是用 HashMap 动态维护 “区间”，思路简洁易懂，面试遇到这题的话推荐还是用 HashMap，别用并查集。**

```java
public int longestConsecutive(int[] nums) {
    int maxLen = 0;
    if(nums == null) return maxLen;
    Set<Integer> unvisited = getSet(nums);
    for (int n : nums){
        if(unvisited.isEmpty())break;
        if(!unvisited.remove(n))continue;
        int len = 1;
        int leftOffset = -1;
        int rightOffset = 1;
        while(unvisited.remove(n+leftOffset--))len++;
        while(unvisited.remove(n+rightOffset++))len++;
        maxLen = Math.max(maxLen,len);
    }
    return maxLen;

}

private Set<Integer> getSet( int [] array){
    Set<Integer> set = new HashSet<>();
    for(int n : array) set.add(n);
    return set;
}
```

### [Friend Circles](https://leetcode.com/problems/friend-circles/description/)

leetcode 323求图的连通分量的一道题目被lock住了，但是Friend Circle这道题目其实就是求联通分量的变形：

#### hash map 版本

```java
class Solution {
    
    
    
    class UF{
        Map<Integer, Integer> parent;
        Map<Integer, Integer> size;
        int count;
        
        private UF(int N){
            parent = new HashMap<>();
            size = new HashMap<>();
            count = N;
            for(int i=0;i<N;i++){
                parent.put(i,i);
                size.put(i,1);
            }
        }
        
        private int find(int p){
            if(!parent.containsKey(p)) return -1;
            
            int root = p;
            while(root!=parent.get(root)){
                root = parent.get(root);
            }
            while(root!=p){
                int next = parent.get(p);
                parent.put(p, root);
                p = next;
            }
            return root;
        }
        
        private void union(int p, int q){
            int pRoot = find(p);
            int qRoot = find(q);
            
            if(pRoot==-1||qRoot==-1) return;
            if(pRoot==qRoot) return;
            
            int pSize = size.get(p);
            int qSize = size.get(q);
            
            if(pSize>qSize){
                parent.put(qRoot, pRoot);
                size.put(pRoot, pSize+qSize);
                count--;
            }else{
                parent.put(pRoot, qRoot);
                size.put(qRoot, pSize+qSize);
                count--; 
            }
        }
    }
    
    public int findCircleNum(int[][] M) {
        int N = M.length;
        UF uf = new UF(N);
        
        for(int i=0;i<N;i++){
            for(int j=i+1;j<N;j++){
                if(M[i][j]==1){
                    uf.union(i,j);
                }
            }
        }
        return uf.count;
    }
}
```

#### Array版本

使用Array还是会比使用Hash Map来的快的，但是使用HashMap的损耗在我看来其实没有那么大，但是还是提供一个Array版本的代码供以后参考：

```java
public class Solution {
    class UnionFind {
        private int count = 0;
        private int[] parent, rank;
        
        public UnionFind(int n) {
            count = n;
            parent = new int[n];
            rank = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }
        
        public int find(int p) {
        	while (p != parent[p]) {
                parent[p] = parent[parent[p]];    // path compression by halving
                p = parent[p];
            }
            return p;
        }
        
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            if (rootP == rootQ) return;
            if (rank[rootQ] > rank[rootP]) {
                parent[rootP] = rootQ;
            }
            else {
                parent[rootQ] = rootP;
                if (rank[rootP] == rank[rootQ]) {
                    rank[rootP]++;
                }
            }
            count--;
        }
        
        public int count() {
            return count;
        }
    }
    
    public int findCircleNum(int[][] M) {
        int n = M.length;
        UnionFind uf = new UnionFind(n);
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (M[i][j] == 1) uf.union(i, j);
            }
        }
        return uf.count();
    }
}

```



