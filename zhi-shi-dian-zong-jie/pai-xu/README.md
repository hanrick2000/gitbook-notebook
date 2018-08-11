# 排序

## 对复杂（复合）结构体的排序

实际运用中经常可以看到复合结构体，要对他们进行排序时候，往往需要重写Comparator.

比如在[Merge Interval](https://leetcode.com/problems/merge-intervals/description/) 中要对其内部的Interval结构进行比较，就采用一下的重写方法

```java

        List<Interval> res = new ArrayList<>();
        Collections.sort(intervals, new Comparator<Interval>(){
            @Override
            public int compare(Interval i, Interval j){
                return i.start - j.start;
            }
        });
```

还有简便的一行的写法

```java
intervals.sort((i,j)->Integer.compare(i.start, j.start));
```

但是他的运行效率好像不及前一种。

### 内部的正序与逆序

在重写排序的时候，a-b输出从小到大，b-a输出从大到小

而对于PQ，可以直接运用以下命令构造大顶堆

```java
PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
```

针对这个专题，我觉得下面这个题目挺有意思的

#### [406. Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/description/)

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        
        int len = people.length;
        if(len<=1) return people;
        
        Arrays.sort(people, new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                if (b[0] == a[0]) return a[1] - b[1];
                return b[0] - a[0];
            }
        });
        
        
        List<int[]> tmp = new ArrayList<>(); 
        for(int i=0;i<len;i++){
            tmp.add(people[i][1], new int[]{people[i][0], people[i][1]});
        }
        
        int[][] res = new int[len][2];
        int i = 0;
        for(int[] pair:tmp){
            res[i][0] = pair[0];
            res[i++][1] = pair[1];
        }
        return res;
    }
}
```

