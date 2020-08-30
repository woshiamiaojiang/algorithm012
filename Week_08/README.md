## 位运算

### 十进制转二进制								
​		余数短除法除以二							
​		![image-20200830124401124](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200830130826.png)							

### 位运算						

![image-20200830131129825](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200830131131.png)

![image-20200830124452803](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200830130834.png)



### 异或

​	**定义：**

​			相同为 0，不同为 1。也可用“不进位加法”来理解。							
​	**操作特点**							
​		x ^ 0 = x						
​		x ^ 1s = ~x // 注意 1s = ~0						
​		x ^ (~x) = 1s						
​		x ^ x = 0						
​		c = a ^ b => a ^ c = b, b ^ c = a // 交换两个数						
​		a ^ b ^ c = a ^ (b ^ c) = (a ^ b) ^ c // associative		

### 指定位置的位运算

1. 将 x 最右边的 n 位清零：x & (~0 << n)							
2. 获取 x 的第 n 位值（0 或者 1）： (x >> n) & 1							
3. 获取 x 的第 n 位的幂值：x & (1 << n)							
4. 仅将第 n 位置为 1：x | (1 << n)							
5. 仅将第 n 位置为 0：x & (~ (1 << n))							
6. 将 x 最高位至第 n 位（含）清零：x & ((1 << n) - 1)	

### 实战位运算要点		

• 判断奇偶：							
	x % 2 == 1 —> (x & 1) == 1						
	x % 2 == 0 —> (x & 1) == 0						
• x >> 1 —> x / 2.							
	即： x = x / 2; —> x = x >> 1;						
	mid = (left + right) / 2; —> mid = (left + right) >> 1;						
• X = X & (X-1) 清零最低位的 1							
• X & -X => 得到最低位的 1							
• X & ~X => 0	

### 实战题目								

- [位 1 的个数](https://leetcode-cn.com/problems/number-of-1-bits/)（Facebook、苹果在半年内面试中考过）
- [2 的幂](https://leetcode-cn.com/problems/power-of-two/)（谷歌、亚马逊、苹果在半年内面试中考过）
- [颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)（苹果在半年内面试中考过）
- [N 皇后](https://leetcode-cn.com/problems/n-queens/description/)（字节跳动、亚马逊、百度在半年内面试中考过）
- [N 皇后 II ](https://leetcode-cn.com/problems/n-queens-ii/description/)（亚马逊在半年内面试中考过）
- [比特位计数](https://leetcode-cn.com/problems/counting-bits/description/)（字节跳动、Facebook、MathWorks 在半年内面试中考过）

​					

## 布隆过滤器


###	定义	

​		一个很长的二进制向量和一系列随机映射函数。布隆过滤器可以用于检索							
​		一个元素是否在一个集合中。							
​		优点是空间效率和查询时间都远远超过一般的算法，							
​		缺点是有一定的误识别率和删除困难。							

###	特点		


​		可以判断一定不存在							

###	示意图	

![image-20200830124856474](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200830130910.png)							
									

## LRU cache							


###	基本知识			

​		**两个要素： 大小 、替换策略**							
​		• Hash Table + Double LinkedList							
​		• O(1) 查询							
​		• O(1) 修改、更新						

###	工作示例								

![image-20200830125014744](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200830130913.png)

### 实现

```java
class LRUCache {
    /**
     * 缓存映射表
     */
    private Map<Integer, DLinkNode> cache = new HashMap<>();
    /**
     * 缓存大小
     */
    private int size;
    /**
     * 缓存容量
     */
    private int capacity;
    /**
     * 链表头部和尾部
     */
    private DLinkNode head, tail;


    public LRUCache(int capacity) {
        //初始化缓存大小，容量和头尾节点
        this.size = 0;
        this.capacity = capacity;
        head = new DLinkNode();
        tail = new DLinkNode();
        head.next = tail;
        tail.prev = head;
    }


    /**
     * 获取节点
     * @param key 节点的键
     * @return 返回节点的值
     */
    public int get(int key) {
        DLinkNode node = cache.get(key);
        if (node == null) {
            return -1;
        }
        //移动到链表头部
         (node);
        return node.value;
    }


    /**
     * 添加节点
     * @param key 节点的键
     * @param value 节点的值
     */
    public void put(int key, int value) {
        DLinkNode node = cache.get(key);
        if (node == null) {
            DLinkNode newNode = new DLinkNode(key, value);
            cache.put(key, newNode);
            //添加到链表头部
            addToHead(newNode);
            ++size;
            //如果缓存已满，需要清理尾部节点
            if (size > capacity) {
                DLinkNode tail = removeTail();
                cache.remove(tail.key);
                --size;
            }
        } else {
            node.value = value;
            //移动到链表头部
            moveToHead(node);
        }
    }


    /**
     * 删除尾结点
     *
     * @return 返回删除的节点
     */
    private DLinkNode removeTail() {
        DLinkNode node = tail.prev;
        removeNode(node);
        return node;
    }


    /**
     * 删除节点
     * @param node 需要删除的节点
     */
    private void removeNode(DLinkNode node) {
        node.next.prev = node.prev;
        node.prev.next = node.next;
    }


    /**
     * 把节点添加到链表头部
     *
     * @param node 要添加的节点
     */
    private void addToHead(DLinkNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }


    /**
     * 把节点移动到头部
     * @param node 需要移动的节点
     */
    private void moveToHead(DLinkNode node) {
        removeNode(node);
        addToHead(node);
    }


    /**
     * 链表节点类
     */
    private static class DLinkNode {
        Integer key;
        Integer value;
        DLinkNode prev;
        DLinkNode next;


        DLinkNode() {
        }


        DLinkNode(Integer key, Integer value) {
            this.key = key;
            this.value = value;
        }
    }
}
```

## 排序

### 总览

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200830130917.png" alt="image-20200830125115823" style="zoom:50%;" />			

### 时间复杂度

![image-20200830125409236](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200830130918.png)


​									
​	初级排序 - O(n^2)								
​		1. 选择排序（Selection Sort）							
​			每次找最小值，然后放到待排序数组的起始位置。						
​		2. 插入排序（Insertion Sort）							
​			从前到后逐步构建有序序列；对于未排序数据，在已排序序列中从后						
​			向前扫描，找到相应位置并插入。						
​		3. 冒泡排序（Bubble Sort）							
​			嵌套循环，每次查看相邻的元素如果逆序，则交换。						
​	高级排序 - O(N*LogN)								
​		• 快速排序（Quick Sort）							
​			数组取标杆 pivot，将小元素放 pivot左边，大元素放右侧，然后依						
​			次对右边和右边的子数组继续快排；以达到整个序列有序。						
​									
​		• 归并排序（Merge Sort）— 分治							
​			1. 把长度为n的输入序列分成两个长度为n/2的子序列；						
​			2. 对这两个子序列分别采用归并排序；						
​			3. 将两个排序好的子序列合并成一个最终的排序序列				
​		快排 vs 归并							
​			归并 和 快排 具有相似性，但步骤顺序相反						
​			归并：先排序左右子数组，然后合并两个有序子数组						
​			快排：先调配出左右子数组，然后对于左右子数组进行排序						
​		• 堆排序（Heap Sort） — 堆插入 O(logN)，取最大/小值 O(1)							
​			1. 数组元素依次建立小顶堆						
​			2. 依次取堆顶元素，并删除						
​	特殊排序 - O(n)								
​		• 计数排序（Counting Sort）							
​			计数排序要求输入的数据必须是有确定范围的整数。将输入的数据值转化为键存						
​			储在额外开辟的数组空间中；然后依次把计数大于 1 的填充回原数组						
​		• 桶排序（Bucket Sort）							
​			桶排序 (Bucket sort)的工作的原理：假设输入数据服从均匀分布，将数据分到						
​			有限数量的桶里，每个桶再分别排序（有可能再使用别的排序算法或是以递归方						
​			式继续使用桶排序进行排）。						
​		• 基数排序（Radix Sort）							
​			基数排序是按照低位先排序，然后收集；再按照高位排序，然后再收集；依次类						
​			推，直到最高位。有时候有些属性是有优先级顺序的，先按低优先级排序，再按						
​			高优先级排序。		        

###												实战题目

- [数组的相对排序](https://leetcode-cn.com/problems/relative-sort-array/)（谷歌在半年内面试中考过）
- [有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)（Facebook、亚马逊、谷歌在半年内面试中考过）
- [力扣排行榜](https://leetcode-cn.com/problems/design-a-leaderboard/)（Bloomberg 在半年内面试中考过）
- [合并区间](https://leetcode-cn.com/problems/merge-intervals/)（Facebook、字节跳动、亚马逊在半年内面试中常考）
- [翻转对](https://leetcode-cn.com/problems/reverse-pairs/)（字节跳动在半年内面试中考过）

## 本周作业

简单

- [位 1 的个数](https://leetcode-cn.com/problems/number-of-1-bits/)（Facebook、苹果在半年内面试中考过）
- [2 的幂](https://leetcode-cn.com/problems/power-of-two/)（谷歌、亚马逊、苹果在半年内面试中考过）
- [颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)（苹果在半年内面试中考过）
- 用自己熟悉的编程语言，手写各种初级排序代码，提交到学习总结中。
- [数组的相对排序](https://leetcode-cn.com/problems/relative-sort-array/)（谷歌在半年内面试中考过）
- [有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)（Facebook、亚马逊、谷歌在半年内面试中考过）

中等

- [LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/#/)（亚马逊、字节跳动、Facebook、微软在半年内面试中常考）
- [力扣排行榜](https://leetcode-cn.com/problems/design-a-leaderboard/)（Bloomberg 在半年内面试中考过）
- [合并区间](https://leetcode-cn.com/problems/merge-intervals/)（Facebook、字节跳动、亚马逊在半年内面试中常考）

困难

- [N 皇后](https://leetcode-cn.com/problems/n-queens/description/)（字节跳动、亚马逊、百度在半年内面试中考过）
- [N 皇后 II ](https://leetcode-cn.com/problems/n-queens-ii/description/)（亚马逊在半年内面试中考过）
- [翻转对](https://leetcode-cn.com/problems/reverse-pairs/)（字节跳动在半年内面试中考过）

### 位 1 的个数

```java
public int hammingWeight(int n) {
    int sum = 0;
    while (n != 0) {
        sum++;
        n &= (n - 1);
    }
    return sum;
}
```

### 2 的幂

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return (n > 0) && (n & (n-1)) == 0;   
    }
}
```

### 颠倒二进制位

```java
public static int reverse(int i) {

    i = (i & 0x55555555) << 1 | (i >>> 1) & 0x55555555;
    i = (i & 0x33333333) << 2 | (i >>> 2) & 0x33333333;
    i = (i & 0x0f0f0f0f) << 4 | (i >>> 4) & 0x0f0f0f0f;
    i = (i << 24) | ((i & 0xff00) << 8) |
        ((i >>> 8) & 0xff00) | (i >>> 24);
    return i;
}
```

### 数组的相对排序

```java
class Solution {
    // 桶排序，先把arr2中的数按顺序拿完，在把桶中剩下的按顺序拿完。
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
            int[] nums = new int[1001];
            int[] res = new int[arr1.length];
            //遍历arr1,统计每个元素的数量
            for (int i : arr1) {
                nums[i]++;
            }
            //遍历arr2,处理arr2中出现的元素
            int index = 0;
            for (int i : arr2) {
                while (nums[i]>0){
                    res[index++] = i;
                    nums[i]--;
                }
            }
            //遍历nums,处理剩下arr2中未出现的元素
            for (int i = 0; i < nums.length; i++) {
                while (nums[i]>0){
                    res[index++] = i;
                    nums[i]--;
                }
            }
            return res;
    }
}
```

### 有效的字母异位词

```java
/*


    1.暴力 sort 遍历比较 
        时间复杂度 O(nlogn)
        空间复杂度 O(1)
    2.哈希表 比较字母出现的频次
        或者26个大小的数组，一边自增，一边自减。所有的都为0则相等。
        时间复杂度O(n)
        空间复杂度O(26)
*/
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length())
            return false;
        int[] table = new int[26];
        for (int i = 0; i < s.length(); i++) {
            table[s.charAt(i) - 'a']++;
        }
        for (int i = 0; i < t.length(); i++) {
            table[t.charAt(i) - 'a']--;
            if (table[t.charAt(i) - 'a'] < 0) { // 长度是相等的。所以判断<0就够了。
                return false;
            }
        }
        return true;
    }
}
```

### [LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/#/)

```java
public class LRUCache {
    class DLinkedNode {
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;
        public DLinkedNode() {}
        public DLinkedNode(int _key, int _value) {key = _key; value = _value;}
    }

    private Map<Integer, DLinkedNode> cache = new HashMap<Integer, DLinkedNode>();
    private int size;
    private int capacity;
    private DLinkedNode head, tail;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
        // 使用伪头部和伪尾部节点
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        moveToHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            // 如果 key 不存在，创建一个新的节点
            DLinkedNode newNode = new DLinkedNode(key, value);
            // 添加进哈希表
            cache.put(key, newNode);
            // 添加至双向链表的头部
            addToHead(newNode);
            ++size;
            if (size > capacity) {
                // 如果超出容量，删除双向链表的尾部节点
                DLinkedNode tail = removeTail();
                // 删除哈希表中对应的项
                cache.remove(tail.key);
                --size;
            }
        }
        else {
            // 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            node.value = value;
            moveToHead(node);
        }
    }

    private void addToHead(DLinkedNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }

    private void removeNode(DLinkedNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void moveToHead(DLinkedNode node) {
        removeNode(node);
        addToHead(node);
    }

    private DLinkedNode removeTail() {
        DLinkedNode res = tail.prev;
        removeNode(res);
        return res;
    }
}
```

### 力扣排行榜



### [合并区间](https://leetcode-cn.com/problems/merge-intervals/)

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        // 先按照区间起始位置排序
        Arrays.sort(intervals, (v1, v2) -> v1[0] - v2[0]);
        // 遍历区间
        int[][] res = new int[intervals.length][2];
        int idx = -1;
        for (int[] interval: intervals) {
            // 如果结果数组是空的，或者当前区间的起始位置 > 结果数组中最后区间的终止位置，
            // 则不合并，直接将当前区间加入结果数组。
            if (idx == -1 || interval[0] > res[idx][1]) {
                res[++idx] = interval;
            } else {
                // 反之将当前区间合并至结果数组的最后区间
                res[idx][1] = Math.max(res[idx][1], interval[1]);
            }
        }
        return Arrays.copyOf(res, idx + 1);
    }
}
```



### [N 皇后](https://leetcode-cn.com/problems/n-queens/description/)

```java
class Solution {
  int rows[];
  // "hill" diagonals
  int hills[];
  // "dale" diagonals
  int dales[];
  int n;
  // output
  List<List<String>> output = new ArrayList();
  // queens positions
  int queens[];

  public boolean isNotUnderAttack(int row, int col) {
    int res = rows[col] + hills[row - col + 2 * n] + dales[row + col];
    return (res == 0) ? true : false;
  }

  public void placeQueen(int row, int col) {
    queens[row] = col;
    rows[col] = 1;
    hills[row - col + 2 * n] = 1;  // "hill" diagonals
    dales[row + col] = 1;   //"dale" diagonals
  }

  public void removeQueen(int row, int col) {
    queens[row] = 0;
    rows[col] = 0;
    hills[row - col + 2 * n] = 0;
    dales[row + col] = 0;
  }

  public void addSolution() {
    List<String> solution = new ArrayList<String>();
    for (int i = 0; i < n; ++i) {
      int col = queens[i];
      StringBuilder sb = new StringBuilder();
      for(int j = 0; j < col; ++j) sb.append(".");
      sb.append("Q");
      for(int j = 0; j < n - col - 1; ++j) sb.append(".");
      solution.add(sb.toString());
    }
    output.add(solution);
  }

  public void backtrack(int row) {
    for (int col = 0; col < n; col++) {
      if (isNotUnderAttack(row, col)) {
        placeQueen(row, col);
        // if n queens are already placed
        if (row + 1 == n) addSolution();
          // if not proceed to place the rest
        else backtrack(row + 1);
        // backtrack
        removeQueen(row, col);
      }
    }
  }

  public List<List<String>> solveNQueens(int n) {
    this.n = n;
    rows = new int[n];
    hills = new int[4 * n - 1];
    dales = new int[2 * n - 1];
    queens = new int[n];

    backtrack(0);
    return output;
  }
}
```



### [N 皇后 II ](https://leetcode-cn.com/problems/n-queens-ii/description/)

```java
class Solution {
   private int count = 0;
    public int totalNQueens(int n) {
        if (n < 1) return 0;
        dfs(0,0,0,0,n);
        return count;
    }

    private void dfs(int row, int col, int pie, int na,int n) {
        if (row >= n) {
            count++;
            return;
        }
        int bit = (~(col | pie | na)) // 获取当前空位 标识为1 
            & ((1<<n) - 1);  // 去掉所有高位
        while (bit > 0){//遍历所有空位
            int p = bit & -bit; //获取最后的空位 1
            /**
             col | p 表示 p 位被占用
             (pie | p ) << 1 ,表示pie往斜左方移一位 被占用
             (na | p) >> 1,表示na往斜右方移一位 被占用
            **/
            dfs(row + 1,col | p,(pie | p ) << 1,(na | p) >> 1,n);
            bit &= (bit - 1); // 打掉最后的1
        }
    }
}
```

