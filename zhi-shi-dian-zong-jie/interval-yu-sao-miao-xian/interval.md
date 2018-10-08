# Interval

Interval 类问题中最常用的技巧，就是自定义 IntervalComparator，把输入按照 startTime 升序排序。

对于任意两个区间A与B，如果

* A.end &gt; B.start 并且
* A.start &lt; B.end
* 则 A 与 B 重叠。

按 start 排序后，数组有了单调性，上面的判断条件就简化成了 A.end &gt; B.start 则一定重叠.

排序后的 Interval 扫描过程中，为了保证正确性，要格外小心多个 Interval 在同一 x 时，处理的先后顺序，比如 skyline problem.

处理这类问题的时候，很有可能会用到TreeMap熟悉下 TreeMap 的常用 API.

> * get\(\)
> * put\(\)
> * containsKey\(\)
> * size\(\)
> * values\(\) : 返回 Collection&lt;&gt; of V
> * lowerKey\(K key\) ： 返回最大的小于参数的Key，没有则 null
> * higherKey\(K key\)：返回最小的大于参数的Key, 没有则 null
> * firstKey\(\) : 返回最小 key
> * lastKey\(\) : 返回最大 key

## [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/description/)

```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
class Solution {
    public boolean canAttendMeetings(Interval[] intervals) {
        if(intervals == null || intervals.length == 0) return true;
        
        Arrays.sort(intervals, new Comparator<Interval>(){
            public int compare(Interval i1, Interval i2) {
                return i1.start - i2.start;
            }
        });
        
        for(int i=0;i<intervals.length-1;i++) {
            if(intervals[i].end>intervals[i+1].start)
                return false;
        }
        return true;
    }
}
```

## [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)

fb 面经高频题，follow up 是返回那个 overlap 最多的区间。

**另一个 fb onsite 面经题里，首先给 n 个一维 interval，返回任意重合最多的点；然后给 n 个二维 rectangle，返回任意重复最多的点。**

\*\*\*\*

## [Insert Interval](https://leetcode.com/problems/insert-interval/)

基本的思路是一样的，要加强记忆的是，List的排序用Collections.sort\(\)，这个函数一样可以定制Comparator

```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
class Solution {
    public List<Interval> merge(List<Interval> intervals) {
        List<Interval> res = new ArrayList<>();
        if(intervals==null || intervals.size()==0) return res;
        
        Collections.sort(intervals, new Comparator<Interval>(){
            public int compare(Interval i1, Interval i2) {
                return i1.start - i2.start;
            }
        });
        int start = intervals.get(0).start, end = intervals.get(0).end;
        for(int i=1;i<intervals.size();i++) {
            if(intervals.get(i).start>end) {
                res.add(intervals.get(i-1));
                start = intervals.get(i).start;
                end = intervals.get(i).end;
            } else {
                end = Math.max(end, intervals.get(i).end);
                intervals.get(i).start = intervals.get(i-1).start;
                intervals.get(i).end = end;
            }
        }
        res.add(intervals.get(intervals.size()-1));
        return res;
    }
}
```

\*\*\*\*



