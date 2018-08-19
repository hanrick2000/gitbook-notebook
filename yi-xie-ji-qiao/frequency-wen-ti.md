# frequency问题

## [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/description/)

这道题目和**Top k Frequent Element**是姊妹题。思想都是用Hash Map去记录频率，然后再通过遍历Value Set，用frequency作为Array的index，进而对frequency进行排序。

要注意的是，如果只是单纯对List里面的元素排Lecigically的序，用自带的sort函数就好了

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> map = new HashMap<>();
        List<String>[] bucket = new ArrayList[words.length];
        for(String word:words){
            map.put(word, map.getOrDefault(word, 0)+1);
        }
        
        for(String key:map.keySet()){
            int frequency = map.get(key);
            if(bucket[frequency]==null)
                bucket[frequency] = new ArrayList<>();
            bucket[frequency].add(key);
        }
        
        List<String> res = new ArrayList<>();
        for(int i=bucket.length-1;i>=0&&res.size()<k;i--){
            if(bucket[i]!=null){
                Collections.sort(bucket[i]);
                res.addAll(bucket[i]);
            }
                
        }
        return res.subList(0, k);
    }
}

```

当然也可以直接Ovrride sort函数对key set进行排序，但是耗时回长很多

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> count = new HashMap();
        for (String word: words) {
            count.put(word, count.getOrDefault(word, 0) + 1);
        }
        List<String> candidates = new ArrayList(count.keySet());
        Collections.sort(candidates, (w1, w2) -> count.get(w1).equals(count.get(w2)) ?
                w1.compareTo(w2) : count.get(w2) - count.get(w1));

        return candidates.subList(0, k);
    }
}
```

