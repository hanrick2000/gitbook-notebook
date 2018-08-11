# 类Union Find问题（待续）

其实图类题目的模板性很强，基本上都是数组作为图的节点，保存其他的点。

而且很多时候用Hash，就有一点Union Find的感觉

## （FB） [399. Evaluate Division](https://leetcode.com/problems/evaluate-division/description/)

```java
class Solution {
    Set<String> set = new HashSet<>();
    Map<String, ArrayList<String>> graph = new HashMap<>();
    Map<String, ArrayList<Double>> value = new HashMap<>();
    public double[] calcEquation(String[][] equations, double[] values, String[][] queries) {
        
        
        
        for(int i=0;i<equations.length;i++){
            String node1=equations[i][0];
            String node2=equations[i][1];
            if(!graph.containsKey(node1)){
                graph.put(node1, new ArrayList<String>());
                value.put(node1, new ArrayList<Double>());
            }
            if(!graph.containsKey(node2)){
                graph.put(node2, new ArrayList<String>());
                value.put(node2, new ArrayList<Double>());
            }
            graph.get(node1).add(node2);
            graph.get(node2).add(node1);
            value.get(node1).add(values[i]);
            value.get(node2).add(1/values[i]);
        }
        
        double[] res = new double[queries.length];
        for(int i=0;i<queries.length;i++){
            String[] query = queries[i];
            res[i] = dfs(query[0], query[1], 1.0);
            if(res[i]==0.0) res[i]=-1.0;
        }
        return res;
    }
    
    private double dfs(String s, String e, double v){
        if(set.contains(s)) return 0.0;
        if(!graph.containsKey(s)) return 0.0;
        if(s.equals(e)) return v;
        set.add(s);
        
        ArrayList<String> strList = graph.get(s);
        ArrayList<Double> valList = value.get(s);
        double tmp = 0.0;
        for(int i=0;i<strList.size();i++){
            tmp = dfs(strList.get(i), e, v*valList.get(i));
            if(tmp!=0.0)
                break;
        }
        set.remove(s);
        return tmp;
    }
}
```



