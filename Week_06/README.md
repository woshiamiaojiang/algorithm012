## 算法训练营-第六周-动态规划

### 递归代码模板

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816222526226.png" alt="image-20200816222526226" style="zoom: 33%;" />

### 递归技巧

- 找到最近最简方法，将其拆解成可重复解决的问题
- 数学归纳法
- 本质：寻找重复性->计算机指令集

### 分治

> 把大的问题分解成几个子问题，子问题分成更小的问题分别计算，把结果返回，把结果聚合在一起

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816222554152.png" alt="image-20200816222554152" style="zoom: 33%;" />

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816222602716.png" alt="image-20200816222602716" style="zoom:33%;" />



### 树形结合

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816222638161.png" alt="image-20200816222638161" style="zoom:33%;" />

### 动态规划

![image-20200816223000828](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816223000828.png)

### 实战例题

**斐波那契数列**

```java
int fib(int n) {
    return n <= 1 ? n : fib(n-1) + fib(n-2);
}
```

代码优化+记忆化搜索

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816223051058.png" alt="image-20200816223051058" style="zoom:33%;" />

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816223121705.png" alt="image-20200816223121705" style="zoom: 50%;" />

与其递归+记忆化搜索

不如一个自底向上循环

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816223221167.png" alt="image-20200816223221167" style="zoom: 67%;" />

总结

![image-20200816223240501](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816223240501.png)

**路径计数**

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816223257266.png" alt="image-20200816223257266" style="zoom: 67%;" />

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816223324464.png" alt="image-20200816223305017" style="zoom: 50%;" />

### DP关键点

![image-20200816223324464](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/image-20200816223324464.png)

### 实战题目

#### [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

https://leetcode-cn.com/problems/unique-paths/solution/fen-xi-gui-lu-kai-pi-er-wei-shu-zu-huo-yi-wei-shu-/

#### [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

https://leetcode-cn.com/problems/unique-paths-ii/solution/bi-62ti-duo-liao-zhang-ai-wu-fen-xi-hou-kai-pi-nli/

#### [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

https://leetcode-cn.com/problems/longest-common-subsequence/solution/kai-pi-yi-ge-er-wei-zi-fu-chuan-shu-zu-by-wo-shi-a/

#### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

https://leetcode-cn.com/problems/climbing-stairs/solution/san-chong-fang-fa-by-wo-shi-a-miao-jiang/

#### [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

https://leetcode-cn.com/problems/triangle/solution/kai-pi-yi-wei-shu-zu-cong-xia-wang-shang-bian-li-b/

#### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

https://leetcode-cn.com/problems/maximum-subarray/solution/dong-tai-gui-hua-yao-yao-xuan-ze-lian-xu-yao-yao-x/

## 本周作业

### 中等

 [最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)（亚马逊、高盛集团、谷歌在半年内面试中考过）

https://leetcode-cn.com/problems/minimum-path-sum/solution/cong-shang-dao-xia-yi-ceng-yi-ceng-bian-li-zai-yua/

 [解码方法](https://leetcode-cn.com/problems/decode-ways)（亚马逊、Facebook、字节跳动在半年内面试中考过）

 [最大正方形](https://leetcode-cn.com/problems/maximal-square/)（华为、谷歌、字节跳动在半年内面试中考过）

 [任务调度器](https://leetcode-cn.com/problems/task-scheduler/)（Facebook 在半年内面试中常考）

 [回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)（Facebook、苹果、字节跳动在半年内面试中考过）

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int n = matrix.length;
        if(n == 0) return 0;
        int m = matrix[0].length;
        if(m == 0) return 0;
        int[][] dp = new int[n][m];
        int result = 0;
        //初始化base case
        for(int i = 0 ;i<n;i++) {
            dp[i][0] = matrix[i][0] - '0';
            result = Math.max(dp[i][0],result);
        }
        for(int i = 0 ;i<m;i++){
            dp[0][i] = matrix[0][i] - '0';
            result = Math.max(dp[0][i],result);
        }
        for(int i = 1; i<n; i++){
            for(int j = 1; j<m; j++){
                //计算dp[i][j];
                if(matrix[i][j] == '1'){
                    dp[i][j] = Math.min(Math.min(dp[i-1][j-1],dp[i-1][j]),dp[i][j-1]) + 1;
                    result = Math.max(dp[i][j],result);
                }
                 
            }
        }
        return result*result;

    }
}
```

### 困难

[最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)（字节跳动、亚马逊、微软在半年内面试中考过）

```java
public class Solution {
    private static int calc(char[] chars, int i, int flag, int end, char cTem) {
        int max = 0, sum = 0, currLen = 0, validLen = 0;
        for (; i != end; i += flag) {
            sum += (chars[i] == cTem ? 1 : -1);
            currLen++;
            if (sum < 0) {
                max = max > validLen ? max : validLen;
                sum = 0;
                currLen = 0;
                validLen = 0;
            } else if (sum == 0) {
                validLen = currLen;
            }
        }
        return max > validLen ? max : validLen;
    }

    public int longestValidParentheses(String s) {
        char[] chars = s.toCharArray();
        return Math.max(calc(chars, 0, 1, chars.length, '('), calc(chars, chars.length - 1, -1, -1, ')'));
    }
}
```

[编辑距离](https://leetcode-cn.com/problems/edit-distance/)（字节跳动、亚马逊、谷歌在半年内面试中考过）

[矩形区域不超过 K 的最大数值和](https://leetcode-cn.com/problems/max-sum-of-rectangle-no-larger-than-k/)（谷歌在半年内面试中考过）

[青蛙过河](https://leetcode-cn.com/problems/frog-jump/)（亚马逊、苹果、字节跳动在半年内面试中考过）

[分割数组的最大值](https://leetcode-cn.com/problems/split-array-largest-sum)（谷歌、亚马逊、Facebook 在半年内面试中考过）

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        int n = nums.length;
        int[][] f = new int[n + 1][m + 1];
        for (int i = 0; i <= n; i++) {
            Arrays.fill(f[i], Integer.MAX_VALUE);
        }
        int[] sub = new int[n + 1];
        for (int i = 0; i < n; i++) {
            sub[i + 1] = sub[i] + nums[i];
        }
        f[0][0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= Math.min(i, m); j++) {
                for (int k = 0; k < i; k++) {
                    f[i][j] = Math.min(f[i][j], Math.max(f[k][j - 1], sub[i] - sub[k]));
                }
            }
        }
        return f[n][m];
    }
}
```

[学生出勤记录 II ](https://leetcode-cn.com/problems/student-attendance-record-ii/)（谷歌在半年内面试中考过）

[最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)（Facebook 在半年内面试中常考）

```java
class Solution {
    Map<Character, Integer> ori = new HashMap<Character, Integer>();
    Map<Character, Integer> cnt = new HashMap<Character, Integer>();

    public String minWindow(String s, String t) {
        int tLen = t.length();
        for (int i = 0; i < tLen; i++) {
            char c = t.charAt(i);
            ori.put(c, ori.getOrDefault(c, 0) + 1);
        }
        int l = 0, r = -1;
        int len = Integer.MAX_VALUE, ansL = -1, ansR = -1;
        int sLen = s.length();
        while (r < sLen) {
            ++r;
            if (r < sLen && ori.containsKey(s.charAt(r))) {
                cnt.put(s.charAt(r), cnt.getOrDefault(s.charAt(r), 0) + 1);
            }
            while (check() && l <= r) {
                if (r - l + 1 < len) {
                    len = r - l + 1;
                    ansL = l;
                    ansR = l + len;
                }
                if (ori.containsKey(s.charAt(l))) {
                    cnt.put(s.charAt(l), cnt.getOrDefault(s.charAt(l), 0) - 1);
                }
                ++l;
            }
        }
        return ansL == -1 ? "" : s.substring(ansL, ansR);
    }

    public boolean check() {
        Iterator iter = ori.entrySet().iterator(); 
        while (iter.hasNext()) { 
            Map.Entry entry = (Map.Entry) iter.next(); 
            Character key = (Character) entry.getKey(); 
            Integer val = (Integer) entry.getValue(); 
            if (cnt.getOrDefault(key, 0) < val) {
                return false;
            }
        } 
        return true;
    }
}
```

[戳气球](https://leetcode-cn.com/problems/burst-balloons/)（亚马逊在半年内面试中考过）

```java
class Solution {
    public int[][] rec;
    public int[] val;

    public int maxCoins(int[] nums) {
        int n = nums.length;
        val = new int[n + 2];
        for (int i = 1; i <= n; i++) {
            val[i] = nums[i - 1];
        }
        val[0] = val[n + 1] = 1;
        rec = new int[n + 2][n + 2];
        for (int i = 0; i <= n + 1; i++) {
            Arrays.fill(rec[i], -1);
        }
        return solve(0, n + 1);
    }

    public int solve(int left, int right) {
        if (left >= right - 1) {
            return 0;
        }
        if (rec[left][right] != -1) {
            return rec[left][right];
        }
        for (int i = left + 1; i < right; i++) {
            int sum = val[left] * val[i] * val[right];
            sum += solve(left, i) + solve(i, right);
            rec[left][right] = Math.max(rec[left][right], sum);
        }
        return rec[left][right];
    }
}
```

