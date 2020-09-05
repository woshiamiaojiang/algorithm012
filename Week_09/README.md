## 高级动态规划

### 递归、分治、回溯

不是非此即彼的关系，而是你中有我，我中有你

### 递归

自己调用自己

#### 递归模板

```java
// Java
public void helper(int level, int param) {
    // 递归终止条件 
    if (level > MAX_LEVEL) {
        // 处理最后的结果
        return;
    }
    // 处理当前层逻辑
    process(level, param);
    // 下探到下一层
    helper( level: level + 1, newParam);
    // 清理当前层 
}
```

### 分治

#### 分治模板

```java
private static int divide_conquer(Problem problem, ) {
  
  if (problem == NULL) {
    int res = process_last_result();
    return res;     
  }
  subProblems = split_problem(problem)
  
  res0 = divide_conquer(subProblems[0])
  res1 = divide_conquer(subProblems[1])
  
  result = process_result(res0, res1);
  
  return result;
}
```

###  动态规划

#### DP模板

```java
// Java
public void recur(int level, int param) { 
  // terminator 
  if (level > MAX_LEVEL) { 
    // process result 
    return; 
  }
  // process current logic 
  process(level, param); 
  // drill down 
  recur( level: level + 1, newParam); 
  // restore current status 
 
}
```

#### DP vs 递归/分治

![image-20200905200824917](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200905223918.png)

### 常见DP与动态方程

#### 爬楼梯

![image-20200905200855605](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200905223921.png)

#### 不同路径

![image-20200905200913980](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200905223925.png)

#### 打家劫舍

![image-20200905200925634](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200905223927.png)

#### 最小路径和

![image-20200905200934969](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200905223929.png)

#### 股票买卖

![image-20200905200946383](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200905223931.png)

## 字符串

### 遍历字符串

<img src="https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200905223933.png" alt="image-20200905201520424" style="zoom:50%;" />

### 字符串比较

```java
String x = new String(“abb”);
String y = new String(“abb”);
x == y —-> false
x.equals(y) —-> true
x.equalsIgnoreCase(y) —-> true
```

### 实战例题

#### [387. 字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

```java
class Solution {
    public int firstUniqChar(String s) {
        for(int i=0; i<s.length(); i++){
            int first = s.indexOf(s.charAt(i));
            int last = s.lastIndexOf(s.charAt(i));
            if(first ==  last){
                return i;
            }
        }
        return -1;
    }
}
```

#### [8. 字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

```java
class Solution {
    public int myAtoi(String str) {
        int sign = 0;
        long ans = 0;
        boolean num = false;
        for(char c:str.trim().toCharArray()) {
           if(c >= '0' && c <= '9') {
                num = true;
                ans = ans * 10 + c - '0';
                if(sign >= 0 && ans > Integer.MAX_VALUE) return Integer.MAX_VALUE;
                if(sign == -1 && sign*ans < Integer.MIN_VALUE) return Integer.MIN_VALUE;
            } else {
                if(c == '+' || c == '-') {
                    if(sign != 0 || num)
                        break;
                    sign = c == '+'?1:-1;
                } else break;
            }
        }
        return (int)((sign == 0?1:sign)*ans);
    }
}
```

#### [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0)   return "";
        for (int i = 0; i < strs[0].length(); i++) {
                char c = strs[0].charAt(i);
                for (int j = 1; j < strs.length; j++) {

                    if(strs[j].length() == i || c!= strs[j].charAt(i))
                        return strs[0].substring(0,i);
            }
        }
        return strs[0];
    }
}
```

#### [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

```java
class Solution {
    public void reverseString(char[] s) {
        if (s == null) return;
        for (int i = 0, j = s.length - 1; i < j; ++i ,--j)  
            s[i] ^= s[j] ^ (s[j] = s[i]);
    }
}
```

#### [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

```java
class Solution {
    public String reverseWords(String s) {
        String[] words = s.trim().split(" +");
        Collections.reverse(Arrays.asList(words));
        return String.join(" ", words);
    }
}
```

#### [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
         List<Integer> list = new ArrayList<>();
        if (s == null || s.equals("") || s.length() < p.length()) return list;
        int[] hash = new int[256];
        for (int j = 0; j < p.length(); j++) {
            hash[p.charAt(j)]++;
        }
        int count = p.length();
        int left = 0, right = 0;
        while (right < s.length()) {
            if (hash[s.charAt(right++)]-- >= 1) count--;
            if (count == 0) list.add(left);
            if ((right - left) == p.length() && hash[s.charAt(left++)]++ >= 0) count++;
        }
        return list;
    }
}
```

#### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        String res = "";
        boolean[][] dp = new boolean[n][n];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                dp[i][j] = s.charAt(i) == s.charAt(j) && (j - i < 2  || dp[i + 1][j - 1]);
                if (dp[i][j] && j - i + 1 > res.length()) {
                    res = s.substring(i, j + 1);
                }
            }
        }
        return res;
    }
}
```





![img](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200905223955.png)



#### [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int len1 = word1.length(), len2 = word2.length();
        int[][] dp = new int[len1 + 1][len2 + 1];
        for (int i = 0; i <= len1; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j <= len2; j++) {
            dp[0][j] = j;
        }
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = 1 + Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]);
                }
            }
        }
        return dp[len1][len2];
    }
}
```



#### [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

```java
public class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        if (text1.isEmpty() || text2.isEmpty()) return 0;
        int m = text1.length(), n = text2.length();
        int[][] a = new int[m][n];
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++) {
                if (text1.charAt(i) == text2.charAt(j)) {
                    if (i - 1 < 0 || j - 1 < 0) a[i][j] = 1; // 考虑边界情况
                    // 相等则为，左上角的值+1
                    else a[i][j] = a[i-1][j-1] + 1;
                } else {
                    // 考虑边界情况
                    if (i == 0 && j == 0) a[i][j] = 0;
                    else if (i == 0) a[i][j] = Math.max(0, a[i][j-1]);
                    else if (j == 0) a[i][j] = Math.max(0, a[i-1][j]);
                    // 如果字符不相等，则当前值为上值或左值中大的那个
                    else a[i][j] = Math.max(a[i-1][j], a[i][j-1]);
                }
            }
        return a[m-1][n-1];
    }
}
```

#### [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

```java
class Solution {
    public boolean isMatch(String s, String p) {
        if (p.isEmpty()) return s.isEmpty();
        boolean firstMatch = !s.isEmpty() && (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.');
        if (p.length() >= 2 && p.charAt(1) == '*') {
            return isMatch(s, p.substring(2)) || (firstMatch && isMatch(s.substring(1), p));
        } else {
            return firstMatch && isMatch(s.substring(1), p.substring(1));
        }
    }
}
```

#### [44. 通配符匹配](https://leetcode-cn.com/problems/wildcard-matching/)

```java
class Solution {
    public boolean isMatch(String s, String p) {
        boolean[][] value = new boolean[p.length()+1][s.length()+1];
        value[0][0] = true;
        for(int i = 1;i <= s.length();i++){
            value[0][i] = false;
        }
        for(int i = 1;i <= p.length(); i++){
            if(p.charAt(i-1) == '*'){
                value[i][0] = value[i-1][0];
                for(int j = 1;j <= s.length(); j++){
                    value[i][j] = (value[i][j-1] || value[i-1][j]);
                }
            }else if(p.charAt(i-1) == '?'){
                value[i][0] = false;
                for(int j = 1;j <= s.length(); j++){
                    value[i][j] = value[i-1][j-1];
                }
            }else {
                value[i][0] = false;
                for(int j = 1;j <= s.length(); j++){
                    value[i][j] = s.charAt(j-1) == p.charAt(i-1) && value[i-1][j-1];
                }
            }

        }
        return value[p.length()][s.length()];

    }
}
```



#### [115. 不同的子序列](https://leetcode-cn.com/problems/distinct-subsequences/)

```java
class Solution {
    public int numDistinct(String s, String t) {
        // dp[0]表示空串
        int[] dp = new int[t.length() + 1];
        // dp[0]永远是1，因为不管S多长，都只能找到一个空串，与T相等
        dp[0] = 1;

        //t的字典
        int[] map = new int[128];
        Arrays.fill(map, -1);

        //从尾部遍历的时候可以遍历 next类似链表 无重复值时为-1，
        //有重复时例如从rabbit的b开始索引在map[b] = 2 next[2] 指向下一个b的索引为3
        // for (int j = t.length() - 1; j >= 0; j--) {
        //     if (t.charAt(j) == s.charAt(i)) {
        //        dp[j + 1] += dp[j];
        //     }
        // }
        //这段代码的寻址就可以从map[s.charAt(i)] 找到索引j 在用next[j] 一直找和 s.charAt(i)相等的字符 其他的就可以跳过了
        //所以这个代码的优化 关键要理解 上面的一维倒算
        int[] nexts = new int[t.length()];
        for(int i = 0 ; i < t.length(); i++){
            int c = t.charAt(i);
            nexts[i] = map[c];
            map[c] = i;
        }

        for (int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            for(int j = map[c]; j >= 0; j = nexts[j]){
                dp[j + 1] += dp[j];
            }
        }
        return dp[t.length()];
    }
}
```

### 字符串匹配算法

- [Boyer-Moore 算法](https://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html)
- [Sunday 算法](https://blog.csdn.net/u012505432/article/details/52210975)
- [字符串匹配暴力法代码示例](https://shimo.im/docs/8G0aJqNL86wWrPUE)
- [Rabin-Karp 代码示例](https://shimo.im/docs/1wnsM7eaZ6Ab9j9M)
- [KMP 字符串匹配算法视频](https://www.bilibili.com/video/av11866460?from=search&seid=17425875345653862171)
- [字符串匹配的 KMP 算法](http://www.ruanyifeng.com/blog/2013/05/Knuth–Morris–Pratt_algorithm.html)



## 本周作业

简单

- [字符串中的第一个唯一字符
  ](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)（亚马逊、微软、Facebook 在半年内面试中考过）
- [反转字符串 II ](https://leetcode-cn.com/problems/reverse-string-ii/)（亚马逊在半年内面试中考过）
- [翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)（微软、字节跳动、苹果在半年内面试中考过）
- [反转字符串中的单词 III ](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)（微软、字节跳动、华为在半年内面试中考过）
- [仅仅反转字母](https://leetcode-cn.com/problems/reverse-only-letters/)（字节跳动在半年内面试中考过）
- [同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)（谷歌、亚马逊、微软在半年内面试中考过）
- [验证回文字符串 Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)（Facebook 在半年内面试中常考）

中等

- 在学习总结中，写出[不同路径 2 ](https://leetcode-cn.com/problems/unique-paths-ii/)这道题目的状态转移方程。
- [最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)（字节跳动、亚马逊、微软在半年内面试中考过）
- [解码方法](https://leetcode-cn.com/problems/decode-ways/)（字节跳动、亚马逊、Facebook 在半年内面试中考过）
- [字符串转换整数 (atoi) ](https://leetcode-cn.com/problems/string-to-integer-atoi/)（亚马逊、微软、Facebook 在半年内面试中考过）
- [找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)（Facebook 在半年内面试中常考）
- [最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)（亚马逊、字节跳动、华为在半年内面试中常考）

困难

- [最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)（亚马逊、字节跳动、华为在半年内面试中考过）
- [赛车](https://leetcode-cn.com/problems/race-car/)（谷歌在半年内面试中考过）
- [通配符匹配](https://leetcode-cn.com/problems/wildcard-matching/)（Facebook、微软、字节跳动在半年内面试中考过）
- [不同的子序列](https://leetcode-cn.com/problems/distinct-subsequences/)（MathWorks 在半年内面试中考过）



#### [387. 字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

```java
class Solution {
    public int firstUniqChar(String s) {
        for(int i=0; i<s.length(); i++){
            int first = s.indexOf(s.charAt(i));
            int last = s.lastIndexOf(s.charAt(i));
            if(first ==  last){
                return i;
            }
        }
        return -1;
    }
}
```

#### [541. 反转字符串 II](https://leetcode-cn.com/problems/reverse-string-ii/)

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] a = s.toCharArray();
        for (int start = 0; start < a.length; start += 2 * k) {
            int i = start, j = Math.min(start + k - 1, a.length - 1);
            while (i < j) {
                char tmp = a[i];
                a[i++] = a[j];
                a[j--] = tmp;
            }
        }
        return new String(a);
    }
}
```

#### [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

```java
class Solution {
    public String reverseWords(String s) {
        String[] words = s.trim().split(" +");
        Collections.reverse(Arrays.asList(words));
        return String.join(" ", words);
    }
}
```

#### [557. 反转字符串中的单词 III](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)

```java
class Solution {
    public String reverseWords(String s) {
	String[] strs = s.split(" ");
	StringBuffer buffer = new StringBuffer();
	for (int i = 0; i < strs.length; i++) {
		buffer.append(new StringBuffer(strs[i]).reverse().toString());
		buffer.append(" ");
	}
	return buffer.toString().trim();
    }
}
```

#### [917. 仅仅反转字母](https://leetcode-cn.com/problems/reverse-only-letters/)

```java
class Solution {
    public String reverseOnlyLetters(String S) {
        Stack<Character> letters = new Stack();
        for (char c: S.toCharArray())
            if (Character.isLetter(c))
                letters.push(c);

        StringBuilder ans = new StringBuilder();
        for (char c: S.toCharArray()) {
            if (Character.isLetter(c))
                ans.append(letters.pop());
            else
                ans.append(c);
        }

        return ans.toString();
    }
}
```

#### [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        char[] ch1 = s.toCharArray();
        char[] ch2 = t.toCharArray();
        int len = s.length();
        for (int i = 0; i < len; i++) {
            if(s.indexOf(ch1[i]) != t.indexOf(ch2[i])){
                return false;
            }
        }
        return true;
    }
}
```

#### [680. 验证回文字符串 Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)

```java
class Solution {
    int del = 0;  //记录删除的字符次数
    public boolean validPalindrome(String s) {
        int i = 0,j = s.length()-1;
        while(i < j){
            if(s.charAt(i) == s.charAt(j)){
                i++;
                j--;
            }else{
                //不相等的话，若没有删除字符，则删除左边或右边的字符再判断；若删除过一次，则不是回文串
                if(del == 0){
                    del++;
                    return validPalindrome(s.substring(i,j)) || validPalindrome(s.substring(i+1,j+1));
                }
                return false;
            }
        }
        return true;
    }
}
```

#### [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

状态转移方程

![image-20200905223658077](https://hexo-1257630696.cos.ap-singapore.myqcloud.com/img/20200905224004.png)

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] a) {
        int[] res = new int[a[0].length]; // 开辟n列数组
        res[0] = 1; // 首值设为1
        for (int i = 0; i < a.length; i++) 
            for (int j = 0; j < a[0].length; j++)
         		if (a[i][j] == 1) res[j] = 0; // 障碍物
                else if (j > 0) res[j] += res[j-1]; // 左边值与原值相加
        return res[a[0].length - 1];
    }	
}
```

#### [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        /**
        dp[i]: 所有长度为i+1的递增子序列中, 最小的那个序列尾数.
        由定义知dp数组必然是一个递增数组, 可以用 maxL 来表示最长递增子序列的长度. 
        对数组进行迭代, 依次判断每个数num将其插入dp数组相应的位置:
        1. num > dp[maxL], 表示num比所有已知递增序列的尾数都大, 将num添加入dp
           数组尾部, 并将最长递增序列长度maxL加1
        2. dp[i-1] < num <= dp[i], 只更新相应的dp[i]
        **/
        int maxL = 0;
        int[] dp = new int[nums.length];
        for(int num : nums) {
            // 二分法查找, 也可以调用库函数如binary_search
            int lo = 0, hi = maxL;
            while(lo < hi) {
                int mid = lo+(hi-lo)/2;
                if(dp[mid] < num)
                    lo = mid+1;
                else
                    hi = mid;
            }
            dp[lo] = num;
            if(lo == maxL)
                maxL++;
        }
        return maxL;
    }
}
```

#### [91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)

```java
public class Solution {

    public int numDecodings(String s) {
        int len = s.length();
        if (len == 0) {
            return 0;
        }

        // dp[i] 以 s[i] 结尾的前缀子串有多少种解码方法
        // dp[i] = dp[i - 1] * 1 if s[i] != '0'
        // dp[i] += dp[i - 2] * 1 if  10 <= int(s[i - 1..i]) <= 26
        int[] dp = new int[len];

        char[] charArray = s.toCharArray();
        if (charArray[0] == '0') {
            return 0;
        }
        dp[0] = 1;

        for (int i = 1; i < len; i++) {
            if (charArray[i] != '0') {
                dp[i] = dp[i - 1];
            }

            int num = 10 * (charArray[i - 1] - '0') + (charArray[i] - '0');
            if (num >= 10 && num <= 26) {
                if (i == 1) {
                    dp[i]++;
                } else {
                    dp[i] += dp[i - 2];
                }
            }
        }
        return dp[len - 1];
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        String s = "10";
        int res = solution.numDecodings(s);
        System.out.println(res);
    }
}
```

#### [8. 字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

```java
class Solution {
    public int myAtoi(String str) {
        int sign = 0;
        long ans = 0;
        boolean num = false;
        for(char c:str.trim().toCharArray()) {
           if(c >= '0' && c <= '9') {
                num = true;
                ans = ans * 10 + c - '0';
                if(sign >= 0 && ans > Integer.MAX_VALUE) return Integer.MAX_VALUE;
                if(sign == -1 && sign*ans < Integer.MIN_VALUE) return Integer.MIN_VALUE;
            } else {
                if(c == '+' || c == '-') {
                    if(sign != 0 || num)
                        break;
                    sign = c == '+'?1:-1;
                } else break;
            }
        }
        return (int)((sign == 0?1:sign)*ans);
    }
}
```

#### [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
         List<Integer> list = new ArrayList<>();
        if (s == null || s.equals("") || s.length() < p.length()) return list;
        int[] hash = new int[256];
        for (int j = 0; j < p.length(); j++) {
            hash[p.charAt(j)]++;
        }
        int count = p.length();
        int left = 0, right = 0;
        while (right < s.length()) {
            if (hash[s.charAt(right++)]-- >= 1) count--;
            if (count == 0) list.add(left);
            if ((right - left) == p.length() && hash[s.charAt(left++)]++ >= 0) count++;
        }
        return list;
    }
}
```

#### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        String res = "";
        boolean[][] dp = new boolean[n][n];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                dp[i][j] = s.charAt(i) == s.charAt(j) && (j - i < 2  || dp[i + 1][j - 1]);
                if (dp[i][j] && j - i + 1 > res.length()) {
                    res = s.substring(i, j + 1);
                }
            }
        }
        return res;
    }
}
```

#### [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

```java
/*
题解：
一般对于最长XX问题容易想到利用DP求解，在这题中利用逆向DP，设状态dp[i]为从i到len - 1中，以i开头的最长合法子串长度：

初始化：dp[len - 1] = 0
如果s[i] = ')'，则跳过，因为不可能有由'('开头的串
如果s[i] = '('，则需要找到右括号和它匹配，可以跳过以i + 1开头的合法子串，则需要看j = i + dp[i + 1] + 1是否为右括号。如果没越界且为右括号，那么有dp[i] = dp[i + 1] + 2，此外在这个基础上还要将j + 1开头的子串加进来（只要不越界）
*/
class Solution {
    public int longestValidParentheses(String s) {
        int n = s.length();
        if (n < 2) return 0;
        int res = 0;
        int[] dp = new int[n];
        for (int i = n - 2; i >= 0; i--) {
            if (s.charAt(i) == '(') {
                int j = i + dp[i + 1] + 1;
                if (j < n && s.charAt(j) == ')') {
                    dp[i] = dp[i + 1] + 2;
                    if (j + 1 < n) dp[i] += dp[j + 1];
                }
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```

#### [818. 赛车](https://leetcode-cn.com/problems/race-car/)

```java
class Solution {
    public int racecar(int target) {
        int[] dp = new int[target + 3];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0; dp[1] = 1; dp[2] = 4;

        for (int t = 3; t <= target; ++t) {
            int k = 32 - Integer.numberOfLeadingZeros(t);
            if (t == (1<<k) - 1) {
                dp[t] = k;
                continue;
            }
            for (int j = 0; j < k-1; ++j)
                dp[t] = Math.min(dp[t], dp[t - (1<<(k-1)) + (1<<j)] + k-1 + j + 2);
            if ((1<<k) - 1 - t < t)
                dp[t] = Math.min(dp[t], dp[(1<<k) - 1 - t] + k + 1);
        }

        return dp[target];  
    }
}
```



#### [44. 通配符匹配](https://leetcode-cn.com/problems/wildcard-matching/)

```java
class Solution {
    public boolean isMatch(String s, String p) {
        boolean[][] value = new boolean[p.length()+1][s.length()+1];
        value[0][0] = true;
        for(int i = 1;i <= s.length();i++){
            value[0][i] = false;
        }
        for(int i = 1;i <= p.length(); i++){
            if(p.charAt(i-1) == '*'){
                value[i][0] = value[i-1][0];
                for(int j = 1;j <= s.length(); j++){
                    value[i][j] = (value[i][j-1] || value[i-1][j]);
                }
            }else if(p.charAt(i-1) == '?'){
                value[i][0] = false;
                for(int j = 1;j <= s.length(); j++){
                    value[i][j] = value[i-1][j-1];
                }
            }else {
                value[i][0] = false;
                for(int j = 1;j <= s.length(); j++){
                    value[i][j] = s.charAt(j-1) == p.charAt(i-1) && value[i-1][j-1];
                }
            }

        }
        return value[p.length()][s.length()];

    }
}
```

#### [115. 不同的子序列](https://leetcode-cn.com/problems/distinct-subsequences/)

```java
class Solution {
    public int numDistinct(String s, String t) {
        // dp[0]表示空串
        int[] dp = new int[t.length() + 1];
        // dp[0]永远是1，因为不管S多长，都只能找到一个空串，与T相等
        dp[0] = 1;

        //t的字典
        int[] map = new int[128];
        Arrays.fill(map, -1);

        //从尾部遍历的时候可以遍历 next类似链表 无重复值时为-1，
        //有重复时例如从rabbit的b开始索引在map[b] = 2 next[2] 指向下一个b的索引为3
        // for (int j = t.length() - 1; j >= 0; j--) {
        //     if (t.charAt(j) == s.charAt(i)) {
        //        dp[j + 1] += dp[j];
        //     }
        // }
        //这段代码的寻址就可以从map[s.charAt(i)] 找到索引j 在用next[j] 一直找和 s.charAt(i)相等的字符 其他的就可以跳过了
        //所以这个代码的优化 关键要理解 上面的一维倒算
        int[] nexts = new int[t.length()];
        for(int i = 0 ; i < t.length(); i++){
            int c = t.charAt(i);
            nexts[i] = map[c];
            map[c] = i;
        }

        for (int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            for(int j = map[c]; j >= 0; j = nexts[j]){
                dp[j + 1] += dp[j];
            }
        }
        return dp[t.length()];
    }
}
```

