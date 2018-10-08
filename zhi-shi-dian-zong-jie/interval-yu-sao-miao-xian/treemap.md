# TreeMap

 1、TreeMap是根据key进行排序的，它的排序和定位需要依赖比较器或覆写Comparable接口，也因此不需要key覆写hashCode方法和equals方法，就可以排除掉重复的key，而HashMap的key则需要通过覆写hashCode方法和equals方法来确保没有重复的key。 

2、TreeMap的查询、插入、删除效率均没有HashMap高，一般只有要对key排序时才使用TreeMap。 

3、TreeMap的key不能为null，而HashMap的key可以为null。 

## API

 [`ceilingEntry`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html#ceilingEntry-K-)`(`[`K`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html) `key)`

Returns a key-value mapping associated with the least key greater than or equal to the given key, or `null` if there is no such key.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><a href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html#ceilingKey-K-"><code><br />c</code></a>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"> <a href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html#firstKey--"><code>firstKey</code></a><code>()</code>Returns
        the first (lowest) key currently in this map.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><a href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html#ceilingEntry-K-"><code>ceilingEntry</code></a><code>(</code>
          <a
          href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html"><code>K</code>
            </a><code> key)</code>
        </p>
        <p>Returns a key-value mapping associated with the least key greater than
          or equal to the given key, or <code>null</code> if there is no such key.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html#floorKey-K-"><code>floorKey</code></a><code>(</code>
        <a
        href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html"><code>K</code>
          </a><code> key)</code>Returns the greatest key less than or equal to the given
          key, or <code>null</code> if there is no such key.</td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html#higherKey-K-"><code>higherKey</code></a><code>(</code>
        <a
        href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html"><code>K</code>
          </a><code> key)</code>Returns the least key strictly greater than the given
          key, or <code>null</code> if there is no such key.</td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html#lowerKey-K-"><code>lowerKey</code></a><code>(</code>
        <a
        href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html"><code>K</code>
          </a><code> key)</code>Returns the greatest key strictly less than the given
          key, or <code>null</code> if there is no such key.</td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html#lastKey--"><code>lastKey</code></a><code>()</code>Returns
        the last (highest) key currently in this map.</td>
    </tr>
  </tbody>
</table>## [My Calendar I](https://leetcode.com/problems/my-calendar-i/description/)

这道题目一开始想的是用HashMap存储开始与结尾的值，然后用binary search取寻找keyset\(\)，得到相应的index。

然后发现TreeMap就是这样一个对key的值进行排序的hash map。

class MyCalendar {

```java
TreeMap<Integer, Integer> map;

public MyCalendar() {
    map = new TreeMap<>();

}

public boolean book(int start, int end) {
    Integer higher =  map.ceilingKey(start),
        lower = map.floorKey(start);
    if((lower==null||map.get(lower)<=start) && (higher==null || higher>=end)) {
        map.put(start, end);
        return true;
    } 
    return false;

    }
}
```

## [Online Election](https://leetcode.com/problems/online-election/description/)

```java
class TopVotedCandidate {

    TreeMap<Integer, Integer> t2Id = new TreeMap<>();
    
    public TopVotedCandidate(int[] persons, int[] times) {
        Map<Integer, Integer> id2Cnt = new HashMap<>();  
        int maxf = 0;
        int maxId = -1;
        int len = persons.length;
        for (int i = 0; i < len; i++) {
            int p = persons[i];
            int t = times[i];
            Integer prev = id2Cnt.getOrDefault(p, 0);
            id2Cnt.put(p, prev + 1);
            if (prev + 1 >= maxf) {
                maxf = prev + 1;
                maxId = p;
            }
            t2Id.put(t, maxId);
        }
    }
    
    public int q(int t) {
        int floorT = t2Id.floorKey(t);
        return t2Id.get(floorT);
    }
}


/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * TopVotedCandidate obj = new TopVotedCandidate(persons, times);
 * int param_1 = obj.q(t);
 */
```



