# Priority Queue/Heap

## 改变Priority Queue内部的比较方式

```java
 PriorityQueue<Map.Entry<Character, Integer>>  q = new PriorityQueue<>(
            (a,b) -> a.getValue() !=b.getValue() ? b.getValue()-a.getValue() : a.getKey()-b.getKey());
```

### 构造大顶堆

```java
PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
```

就是再new PQ的时候在后面的括号中重新定义PQ内部的比较方式

例题: 621

