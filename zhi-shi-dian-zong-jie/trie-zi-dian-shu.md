# Trie, 字典树\(待续\)

也先贴一个[CSDN](https://blog.csdn.net/v_july_v/article/details/6897097)的帖子吧，不光讲了Trie，还有后缀树的相关内容。

然后[leetcode](https://leetcode.com/problems/implement-trie-prefix-tree/solution/)这篇文章也讲的很好。

#### 1.1 什么是Trie树 <a id="11-&#x4EC0;&#x4E48;&#x662F;trie&#x6811;"></a>

Trie树，即字典树，又称单词查找树或键树，是一种树形结构，是一种哈希树的变种。典型应用是用于统计和排序大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。它的优点是：最大限度地减少无谓的字符串比较，查询效率比哈希表高。

Trie的核心思想是空间换时间。利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。

它有3个基本性质：

* 根节点不包含字符，除根节点外每一个节点都只包含一个字符。
* 从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串。
* 每个节点的所有子节点包含的字符都不相同。

#### 1.2 树的构建 <a id="12-&#x6811;&#x7684;&#x6784;&#x5EFA;"></a>

举个在网上流传颇广的例子，如下：

题目：给你100000个长度不超过10的单词。对于每一个单词，我们要判断他出没出现过，如果出现了，求第一次出现在第几个位置。

分析：这题当然可以用hash来解决，但是本文重点介绍的是trie树，因为在某些方面它的用途更大。比如说对于某一个单词，我们要询问它的前缀是否出现过。这样hash就不好搞了，而用trie还是很简单。

现在回到例子中，如果我们用最傻的方法，对于每一个单词，我们都要去查找它前面的单词中是否有它。那么这个算法的复杂度就是O\(n^2\)。显然对于100000的范围难以接受。现在我们换个思路想。假设我要查询的单词是abcd，那么在他前面的单词中，以b，c，d，f之类开头的我显然不必考虑。而只要找以a开头的中是否存在abcd就可以了。同样的，在以a开头中的单词中，我们只要考虑以b作为第二个字母的，一次次缩小范围和提高针对性，这样一个树的模型就渐渐清晰了。

好比假设有b，abc，abd，bcd，abcd，efg，hii 这6个单词，我们构建的树就是如下图这样的：

![](https://mnmunknown.gitbooks.io/algorithm-notes/content/trie_1.jpg)

当时第一次看到这幅图的时候，便立马感到此树之不凡构造了。单单从上幅图便可窥知一二，好比大海搜人，立马就能确定东南西北中的到底哪个方位，如此迅速缩小查找的范围和提高查找的针对性，不失为一创举。

ok，如上图所示，对于每一个节点，从根遍历到他的过程就是一个单词，如果这个节点被标记为红色，就表示这个单词存在，否则不存在。

那么，对于一个单词，我只要顺着他从根走到对应的节点，再看这个节点是否被标记为红色就可以知道它是否出现过了。把这个节点标记为红色，就相当于插入了这个单词。

这样一来我们查询和插入可以一起完成（重点体会这个查询和插入是如何一起完成的，稍后，下文具体解释），所用时间仅仅为单词长度，在这一个样例，便是10。

我们可以看到，trie树每一层的节点数是26^i级别的。所以为了节省空间。我们用动态链表，或者用数组来模拟动态。空间的花费，不会超过单词数×单词长度。

#### 1.3 前缀查询 <a id="13-&#x524D;&#x7F00;&#x67E5;&#x8BE2;"></a>

上文中提到”比如说对于某一个单词，我们要询问它的前缀是否出现过。这样hash就不好搞了，而用trie还是很简单“。下面，咱们来看看这个前缀查询问题：

已知n个由小写字母构成的平均长度为10的单词,判断其中是否存在某个串为另一个串的前缀子串。下面对比3种方法：

* 最容易想到的：即从字符串集中从头往后搜，看每个字符串是否为字符串集中某个字符串的前缀，复杂度为O\(n^2\)。
* 使用hash：我们用hash存下所有字符串的所有的前缀子串，建立存有子串hash的复杂度为O\(n_len\)，而查询的复杂度为O\(n\)_ O\(1\)= O\(n\)。
* 使用trie：因为当查询如字符串abc是否为某个字符串的前缀时，显然以b,c,d....等不是以a开头的字符串就不用查找了。所以建立trie的复杂度为O\(n_len\)，而建立+查询在trie中是可以同时执行的，建立的过程也就可以成为查询的过程，hash就不能实现这个功能。所以总的复杂度为O\(n_len\)，实际查询的复杂度也只是O\(len\)。（说白了，就是Trie树的平均高度h为len，所以Trie树的查询复杂度为O（h）=O（len）。好比一棵二叉平衡树的高度为logN，则其查询，插入的平均时间复杂度亦为O（logN））。

下面解释下上述方法3中所说的为什么hash不能将建立与查询同时执行，而Trie树却可以：

在hash中，例如现在要输入两个串911，911456，如果要同时查询这两个串，且查询串的同时若hash中没有则存入。那么，这个查询与建立的过程就是先查询其中一个串911，没有，然后存入9、91、911；而后查询第二个串911456，没有然后存入9、91、911、9114、91145、911456。因为程序没有记忆功能，所以并不知道911在输入数据中出现过，只是照常以例行事，存入9、91、911、9114、911...。也就是说用hash必须先存入所有子串，然后for循环查询。

而trie树中，存入911后，已经记录911为出现的字符串，在存入911456的过程中就能发现而输出答案；倒过来亦可以，先存入911456，在存入911时，当指针指向最后一个1时，程序会发现这个1已经存在，说明911必定是某个字符串的前缀。

读者反馈@悠悠长风：关于这点，我有不同的看法。hash也是可以实现边建立边查询的啊。当插入911时，需要一个额外的标志位，表示它是一个完整的单词。在处理911456时，也是按照前面的查询9,91,911，当查询911时，是可以找到前面插入的911，且通过标志位知道911为一个完整单词。那么就可以判断出911为911456的前缀啊。虽然trie树更适合这个问题，但是我认为hash也是可以实现边建立，边查找。

至于，有关Trie树的查找，插入等操作的实现代码，网上遍地开花且千篇一律，诸君尽可参考，想必不用我再做多余费神。

#### 1.4 查询 <a id="14-&#x67E5;&#x8BE2;"></a>

Trie树是简单但实用的数据结构，通常用于实现字典查询。我们做即时响应用户输入的AJAX搜索框时，就是Trie开始。本质上，Trie是一颗存储多个字符串的树。相邻节点间的边代表一个字符，这样树的每条分支代表一则子串，而树的叶节点则代表完整的字符串。和普通树不同的地方是，相同的字符串前缀共享同一条分支。下面，再举一个例子。给出一组单词，inn, int, at, age, adv, ant, 我们可以得到下面的Trie：

![](https://mnmunknown.gitbooks.io/algorithm-notes/content/trie_2.gif)

可以看出：

* 每条边对应一个字母。
* 每个节点对应一项前缀。叶节点对应最长前缀，即单词本身。
* 单词inn与单词int有共同的前缀“in”, 因此他们共享左边的一条分支，root-&gt;i-&gt;in。同理，ate, age, adv, 和ant共享前缀"a"，所以他们共享从根节点到节点"a"的边。
* 查询操纵非常简单。比如要查找int，顺着路径i -&gt; in -&gt; int就找到了。

搭建Trie的基本算法也很简单，无非是逐一把每则单词的每个字母插入Trie。插入前先看前缀是否存在。如果存在，就共享，否则创建对应的节点和边。比如要插入单词add，就有下面几步：

* 考察前缀"a"，发现边a已经存在。于是顺着边a走到节点a。
* 考察剩下的字符串"dd"的前缀"d"，发现从节点a出发，已经有边d存在。于是顺着边d走到节点ad
* 考察最后一个字符"d"，这下从节点ad出发没有边d了，于是创建节点ad的子节点add，并把边ad-&gt;add标记为d。

#### 1.5 Trie树的应用 <a id="15-trie&#x6811;&#x7684;&#x5E94;&#x7528;"></a>

除了本文引言处所述的问题能应用Trie树解决之外，Trie树还能解决下述问题（节选自此文：海量数据处理面试题集锦与Bit-map详解）：

3、有一个1G大小的一个文件，里面每一行是一个词，词的大小不超过16字节，内存限制大小是1M。返回频数最高的100个词。

9、1000万字符串，其中有些是重复的，需要把重复的全部去掉，保留没有重复的字符串。请怎么设计和实现？

10、 一个文本文件，大约有一万行，每行一个词，要求统计出其中最频繁出现的前10个词，请给出思想，给出时间复杂度分析。

13、寻找热门查询： 搜索引擎会通过日志文件把用户每次检索使用的所有检索串都记录下来，每个查询串的长度为1-255字节。假设目前有一千万个记录，这些查询串的重复读比较高，虽然总数是1千万，但是如果去除重复和，不超过3百万个。一个查询串的重复度越高，说明查询它的用户越多，也就越热门。请你统计最热门的10个查询串，要求使用的内存不能超过1G。

\(1\) 请描述你解决这个问题的思路；

\(2\) 请给出主要的处理流程，算法，以及算法的复杂度。

### 与Hash Table的比较

 Although hash table has O\(1\)O\(1\) time complexity for looking for a key, it is not efficient in the following operations :

* Finding all keys with a common prefix.
* Enumerating a dataset of strings in lexicographical order.

Another reason why trie outperforms hash table, is that as hash table increases in size, there are lots of hash collisions and the search time complexity could deteriorate to O\(n\)O\(n\), where nn is the number of keys inserted. Trie could use less space compared to Hash Table when storing many keys with the same prefix. In this case using trie has only O\(m\)O\(m\) time complexity, where mm is the key length. Searching for a key in a balanced tree costs O\(m \log n\)O\(mlogn\) time complexity.

## [Implement Trie \(Prefix Tree\)](https://leetcode.com/problems/implement-trie-prefix-tree/)

当涉及到key-value对的，还是使用ArrayList\[\]这样的数据效率更高（或者叫做Array Trie）

针对这段代码，在多说几个要注意的地方：

（1）java中定义新的数据结构时，class要放在class solution之外。同时不要有public之类的关键字。public应该给主要Solution的那个class

（2）对于需要固定头的树类/链表类结构，记得用一个tmp保存下来这个值，不然它会随着操作改变。有需要的时候使用dummy

### 复杂度分析

插入：

* Time complexity : O\(m\)O\(m\) In each step of the algorithm we search for the next key character. In the worst case the algorithm performs mm operations.
* Space complexity : O\(1\)O\(1\)

查找：

* Time complexity : O\(m\)O\(m\)
* Space complexity : O\(1\)O\(1\)

```java
class TrieNode{
    public char val;
    public boolean isWord;
    public TrieNode[] children = new TrieNode[26];
    public TrieNode(){}
    public TrieNode(char c){
        TrieNode node = new TrieNode();
        node.val = c;
        
    }
}

public class Trie {

    private TrieNode root;
    
    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
        root.val = ' ';
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode cur = root; 
        for(int i=0;i<word.length();i++){
            char tmp = word.charAt(i);
            if(cur.children[tmp-'a']==null)
                cur.children[tmp-'a'] = new TrieNode(tmp);
            cur = cur.children[tmp-'a'];
        }
        cur.isWord = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode cur = root;
        for(int i=0;i<word.length();i++){
            char tmp = word.charAt(i);
            if(cur.children[tmp-'a']==null) return false;
            cur = cur.children[tmp-'a'];
        }
        return cur.isWord;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode cur = root;
        for(int i=0;i<prefix.length();i++){
            char tmp = prefix.charAt(i);
            if(cur.children[tmp-'a']==null) return false;
            cur = cur.children[tmp-'a'];
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

## [Add and Search Word - Data structure design](https://leetcode.com/problems/add-and-search-word-data-structure-design/)

如果这道题目没有"."的前缀查询这个不确定因素，直接使用hashmap就可以了。

 因为没有 “前缀查询” 这个可以明显区分 trie 和 hashmap 查询性能的操作。

Tries的构造，既可以使用array，又可以使用HashMap。

这道题目同样可以使用两种做法，同样的代码逻辑，把 map 由 HashMap 换成 array 就过了，说明速度上的差距啊。

* **HashMap trie:**
  * 优点
    * 支持所有 Character
    * 相对更省空间
  * 缺点
    * 查询时间相对长
    * 尤其是有 wildcard 做 DFS 时
* **Array trie:**
  * 优点
    * 查询速度快，尤其是有 wildcard 做 DFS 时
  * 缺点
    * 每个 node 空间占用大
    * 比较依赖指定字符集，比如 'a-z' 这种

#### LeetCode OJ 事实说明，这题更适合用 array trie 去解，也算是一个根据输入信息选择合适implementation 的好例子，同样也可以推广到 union-find 的两种 implementation. <a id="leetcode-oj-&#x4E8B;&#x5B9E;&#x8BF4;&#x660E;&#xFF0C;&#x8FD9;&#x9898;&#x66F4;&#x9002;&#x5408;&#x7528;-array-trie-&#x53BB;&#x89E3;&#xFF0C;&#x4E5F;&#x7B97;&#x662F;&#x4E00;&#x4E2A;&#x6839;&#x636E;&#x8F93;&#x5165;&#x4FE1;&#x606F;&#x9009;&#x62E9;&#x5408;&#x9002;implementation--&#x7684;&#x597D;&#x4F8B;&#x5B50;&#xFF0C;&#x540C;&#x6837;&#x4E5F;&#x53EF;&#x4EE5;&#x63A8;&#x5E7F;&#x5230;-union-find-&#x7684;&#x4E24;&#x79CD;-implementation"></a>

### hash map版本

```java
public class WordDictionary {
    private class TrieNode{
        Character chr;
        HashMap<Character, TrieNode> map;
        boolean isWord;

        public TrieNode(){
            map = new HashMap<Character, TrieNode>();
        }
        public TrieNode(char chr){
            this.chr = chr;
            map = new HashMap<Character, TrieNode>();
            isWord = false;
        }
    }

    TrieNode root = new TrieNode();

    // Adds a word into the data structure.
    public void addWord(String word) {
        TrieNode cur = root;
        for(int i = 0; i < word.length(); i++){
            char chr = word.charAt(i);
            if(!cur.map.containsKey(chr)){
                cur.map.put(chr, new TrieNode(chr));
            }
            cur = cur.map.get(chr);
        }
        cur.isWord = true;
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        return search(word, 0, root);
    }

    public boolean search(String word, int index, TrieNode node){
        if(index == word.length()) return node.isWord;
        char chr = word.charAt(index);

        if(chr == '.'){
            Set<Character> keySet = node.map.keySet();
            for(Character next : keySet){
                if(search(word, index + 1, node.map.get(next))) return true;
            }
            return false;
        } else {
            if(!node.map.containsKey(chr)) {
                return false;
            } else {
                return search(word, index + 1, node.map.get(chr));
            }
        }
    }
}
```

### Array版本

```java

class WordDictionary {
    
    private class TrieNode{
    public boolean isWord;
    public char val;
    public TrieNode[] children = new TrieNode[26];
    public TrieNode(){};
    public TrieNode(char c){
        TrieNode node = new TrieNode();
        node.val = c;
    }
}
    
    private TrieNode root;
    /** Initialize your data structure here. */
    public WordDictionary() {
        root = new TrieNode();
        root.val = ' ';
    }
    
    /** Adds a word into the data structure. */
    public void addWord(String word) {
        TrieNode cur = root;
        for(int i=0;i<word.length();i++){
            char tmp = word.charAt(i);
            
            if(cur.children[tmp-'a']==null){
                cur.children[tmp-'a'] = new TrieNode(tmp);
            }
            cur = cur.children[tmp-'a'];
        }
        cur.isWord = true;
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    public boolean search(String word) {
        return search(word, 0, root);
    }
    
    private boolean search(String word, int index, TrieNode node){
        if(index==word.length()) return node.isWord;
        char temp = word.charAt(index);
        int chrIndex = temp - 'a';
        
        if(temp == '.'){
            for(int i = 0; i < 26; i++){
                if(node.children[i] != null){
                    if(search(word, index + 1, node.children[i])) return true;
                }
            }
            return false;
        } else {
            if(node.children[chrIndex] == null) {
                return false;
            } else {
                return search(word, index + 1, node.children[chrIndex]);
            }
        }
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```

