## 字典树

### 复习树

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823144814151.png" alt="image-20200823144814151" style="zoom: 50%;" />

### 复习二叉搜索树

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823144845357.png" alt="image-20200823144845357" style="zoom: 50%;" />

### 字典树

#### 基本结构

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823144918313.png" alt="image-20200823144918313" style="zoom: 67%;" />

#### 基本性质

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823144936009.png" alt="image-20200823144936009" style="zoom: 50%;" />

#### 内部实现

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823145011664.png" alt="image-20200823145011664" style="zoom:67%;" />

#### 核心思想

> 利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。

#### 实战题目

#### [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

```java
class Trie {


    private final TireNode root;


    public Trie() {
        root = new TireNode();
    }
    
    class TireNode {
        TireNode[] next;
        private boolean isEnd;


        public TireNode() {
            isEnd = false;
            next = new TireNode[26];
        }
    }


    public void insert(String word) {
        TireNode node = root;
        for (char c : word.toCharArray()) {
            if (node.next[c - 'a'] == null) {
                node.next[c - 'a'] = new TireNode();
            }
            node = node.next[c - 'a'];
        }
        node.isEnd = true;
    }


    public boolean search(String word) {
        TireNode node = root;
        for (char c : word.toCharArray()) {
            node = node.next[c - 'a'];
            if (node == null) {
                return false;
            }
        }
        return node.isEnd;
    }


    public boolean startsWith(String prefix) {
        TireNode node = root;
        for (char c : prefix.toCharArray()) {
            node = node.next[c - 'a'];
            if (node == null) {
                return false;
            }
        }
        return true;
    }
}
```

#### [212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/)

```java
public List<String> findWords(char[][] board, String[] words) {
    List<String> res = new ArrayList<>();
    //判断每个单词
    for (String word : words) {
        if (exist(board, word)) {
            res.add(word);
        }
    }
    return res;
}
//下边是 79 题的代码
public boolean exist(char[][] board, String word) {
    int rows = board.length;
    if (rows == 0) {
        return false;
    }
    int cols = board[0].length;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (existRecursive(board, i, j, word, 0)) {
                return true;
            }
        }
    }
    return false;
}

private boolean existRecursive(char[][] board, int row, int col, String word, int index) {
    if (row < 0 || row >= board.length || col < 0 || col >= board[0].length) {
        return false;
    }
    if (board[row][col] != word.charAt(index)) {
        return false;
    }
    if (index == word.length() - 1) {
        return true;
    }
    char temp = board[row][col];
    board[row][col] = '$';
    boolean up = existRecursive(board, row - 1, col, word, index + 1);
    if (up) {
        board[row][col] = temp; //将 board 还原, 79 题中的代码没有还原，这里必须还原
        return true;
    }
    boolean down = existRecursive(board, row + 1, col, word, index + 1);
    if (down) {
        board[row][col] = temp;//将 board 还原, 79 题中的代码没有还原，这里必须还原
        return true;
    }
    boolean left = existRecursive(board, row, col - 1, word, index + 1);
    if (left) {
        board[row][col] = temp;//将 board 还原, 79 题中的代码没有还原，这里必须还原
        return true;
    }
    boolean right = existRecursive(board, row, col + 1, word, index + 1);
    if (right) {
        board[row][col] = temp;//将 board 还原, 79 题中的代码没有还原，这里必须还原
        return true;
    }
    board[row][col] = temp;
    return false;
}
```

## 并查集

### 场景

- 组团、配对
- A是不是B的朋友
- 朋友圈功能

### 基本操作

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823145254939.png" alt="image-20200823145254939" style="zoom: 50%;" />

### 并查集图形解释

#### 初始化

每个元素拥有parent数组指向自己

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823145329461.png" alt="image-20200823145329461" style="zoom:33%;" />

#### 查询

对任何一个元素，看他的parent再看他的parent，直到parent等于自己找到领头元素

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823145345729.png" alt="image-20200823145345729" style="zoom:33%;" />

#### 合并

把e的parent指向a

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823145406269.png" alt="image-20200823145406269" style="zoom:33%;" />

#### 路径压缩

把b c d的parent都指向a

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823145419922.png" alt="image-20200823145419922" style="zoom:33%;" />

### Java代码实现并查集

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823145445000.png" alt="image-20200823145445000" style="zoom: 50%;" />

### 作业

#### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

```java
class Solution {
    public int numIslands(char[][] grid) {
        int islandNum = 0;
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                if(grid[i][j] == '1'){
                    infect(grid, i, j);
                    islandNum++;
                }
            }
        }
        return islandNum;
    }
    //感染函数
    public void infect(char[][] grid, int i, int j){
        if(i < 0 || i >= grid.length ||
           j < 0 || j >= grid[0].length || grid[i][j] != '1'){
            return;
        }
        grid[i][j] = '2';
        infect(grid, i + 1, j);
        infect(grid, i - 1, j);
        infect(grid, i, j + 1);
        infect(grid, i, j - 1);
    }
}
```



#### [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

```java
class Solution {
    public void solve(char[][] a) {
        // 两行或者两列的情况不用考虑，直接返回
        if (a.length < 3 || a[0].length < 3) return;
        // m行n列
        int m = a.length, n = a[0].length;
        // 第1列与最后1列
        for (int i = 0; i < m; i++) {
            dfs(a, i, 0);
            dfs(a, i, n - 1);
        }
        // 第1行与最后1行，跳过重复的
        for (int j = 1; j < n-1; j++) {
            dfs(a, 0, j);
            dfs(a, m - 1, j);
        }
        // 最后将所有的遍历一遍，是O的没被打标记，抹成X。打标记V的改成O。
        for (int i = 0; i < m; i++) 
            for (int j = 0; j < n; j++) {
                if (a[i][j] == 'O') a[i][j] = 'X';
                if (a[i][j] == 'V') a[i][j] = 'O';
            }
    }

    // i行j列
    public void dfs(char[][] a, int i, int j) {
        // 递归结束条件：越界或者不是O，就不再递归下去
        if(i < 0 || i >= a.length || j < 0 || j >= a[0].length || a[i][j] != 'O') return;
        a[i][j] = 'V';
        dfs(a, i-1, j);
        dfs(a, i+1, j);
        dfs(a, i, j-1);
        dfs(a, i, j+1);
    }
}
```

## 高级搜索

### 搜索进阶

- 暴力
- 优化：不重复（斐波那契）、剪枝（括号生成）
- 搜索方向：
  - 深度优先
  - 广度优先
  - 双向搜索
  - 启发式搜索

### 零钱置换

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823145740758.png" alt="image-20200823145740758" style="zoom: 50%;" />

### 剪枝

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823145759660.png" alt="image-20200823145759660" style="zoom:33%;" />

### 回溯

> 尝试试错思想，分步解决问题。
>
> 当发现当前分步不能得出正确答案，将回退多步。
>
> 通过其他分步寻求答案。

### 实战

#### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

```java
public class Solution {
    public int climbStairs(int n) {
        if (n < 3) return n;
        int f1 = 1, f2 = 2, tmp = 0;
        for(int i = 2; i < n; i++) {
            tmp = f1 + f2;
            f1 = f2;
            f2 = tmp;
        }
        return f2;
    }
}
```

#### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

```java
// 递归。确保括号有效性。只要要求左括号先行于右括号。
class Solution {

    List<String> res = new ArrayList<>();

    public List<String> generateParenthesis(int n) {
        dfs(n, n, "");
        return res;
    }

    private void dfs(int left, int right, String str) {
        if (left == 0 && right == 0) { // 结束条件：左右括号都不剩余了
            res.add(str);
            return;
        }
        // 下钻，第一次会进去。第二次会有两个分叉
        if (left > 0) { // 如果左括号还剩余的话，可以拼接左括号
            dfs(left - 1, right, str + "(");
        }
        // 第一次是进不去的，因为左右是相等的。第二次进入后，左右等号数量又相等了。
        if (right > left) { // 如果右括号剩余多于左括号剩余的话，可以拼接右括号
            dfs(left, right - 1, str + ")");
        }
        // 清除全局临时变量
    }
}
```

#### [51. N皇后](https://leetcode-cn.com/problems/n-queens/)

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

#### [36. 有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        Set seen = new HashSet();
        for (int i=0; i<9; ++i) {
            for (int j=0; j<9; ++j) {
                char number = board[i][j];
                if (number != '.')
                    if (!seen.add(number + " in row " + i) ||
                        !seen.add(number + " in column " + j) ||
                        !seen.add(number + " in block " + i/3 + "-" + j/3))
                        return false;
            }
        }
        return true;
    }
}
```

## 双向BFS

### 双向连通图

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823150242631.png" alt="image-20200823150242631" style="zoom:50%;" />

<img src="C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823150250741.png" alt="image-20200823150250741" style="zoom: 50%;" />

### 实战题目

#### [127. 单词接龙](https://leetcode-cn.com/problems/word-ladder/)

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        Set<String> reached = new HashSet<>();
        reached.add(beginWord);
        wordSet.remove(beginWord);
        int level = 1;
        while (!reached.isEmpty()) {
            Set<String> reachedNext = new HashSet<>();
            for (String s : reached) {
                for (int i = 0; i < s.length(); i++) {
                    char[] c = s.toCharArray();
                    for (char j = 'a'; j <= 'z'; j++) {
                        c[i] = j;
                        String newS = new String(c);
                        if (wordSet.remove(newS)) {
                            reachedNext.add(newS);
                            if (endWord.equals(newS)) return level + 1;
                        }
                    }
                }
            }
            reached = reachedNext;
            level++;
        }
        return 0;
    }
}
```

#### [433. 最小基因变化](https://leetcode-cn.com/problems/minimum-genetic-mutation/)

```java
public class Solution {
    int minCnt = Integer.MAX_VALUE;

    public int minMutation(String start, String end, String[] bank) {
        dfs(new HashSet<>(), 0, start, end, bank);
        return (minCnt == Integer.MAX_VALUE) ? -1 : minCnt;
    }

    private void dfs(HashSet<String> step, int cnt,
                     String cur, String end, String[] bank) {
        if (cur.equals(end)) minCnt = Math.min(cnt, minCnt);
        for (String str : bank) {
            int diff = 0;
            for (int i = 0; i < str.length(); i++)
                if (cur.charAt(i) != str.charAt(i) && ++diff > 1)
                    break; // 变化步骤超过1，跳过
            if (diff == 1 && !step.contains(str)) {
                step.add(str);
                dfs(step, cnt+1, str, end, bank);
                step.remove(str);
            }
        }
    }
}
```

## 启发式搜索

### 估价函数

![image-20200823150704155](C:\Users\Aspirin\AppData\Roaming\Typora\typora-user-images\image-20200823150704155.png)

### 实战题目

#### [1091. 二进制矩阵中的最短路径](https://leetcode-cn.com/problems/shortest-path-in-binary-matrix/)

```java
class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        if (grid[0][0] == 1) {
            return -1;
        }
        final int N = grid.length;
        final int[][] DIRECTIONS = new int[][]{{-1, 0}, {0, 1}, {1, 0}, {0, -1}, {-1, -1}, {-1, 1}, {1, -1}, {1, 1}};
        boolean[][] visited = new boolean[N][N];
        LinkedList<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0});
        int depth = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            depth++;
            for (int i = 0; i < size; i++) {
                int[] node = queue.removeFirst();
                if (node[0] == N - 1 && node[1] == N - 1) {
                    return depth;
                }
                for (int[] direction : DIRECTIONS) {
                    int x = node[0] + direction[0];
                    int y = node[1] + direction[1];
                    // 排除：1、越界 2、已访问 3、阻塞
                    if (x < 0 || x >= N || y < 0 || y >= N || visited[x][y] || grid[x][y] == 1) {
                        continue;
                    }
                    visited[x][y] = true;
                    queue.addLast(new int[]{x, y});
                }
            }
        }
        return -1;
    }
}
```

#### [773. 滑动谜题](https://leetcode-cn.com/problems/sliding-puzzle/)

```java
class Solution {
    public int slidingPuzzle(int[][] board) {
        int R = board.length, C = board[0].length;
        int sr = 0, sc = 0;

        //Find sr, sc
        search:
            for (sr = 0; sr < R; sr++)
                for (sc = 0; sc < C; sc++)
                    if (board[sr][sc] == 0)
                        break search;

        int[][] directions = new int[][]{{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        PriorityQueue<Node> heap = new PriorityQueue<Node>((a, b) ->
            (a.heuristic + a.depth) - (b.heuristic + b.depth));
        Node start = new Node(board, sr, sc, 0);
        heap.add(start);

        Map<String, Integer> cost = new HashMap();
        cost.put(start.boardstring, 9999999);

        String target = Arrays.deepToString(new int[][]{{1,2,3}, {4,5,0}});
        String targetWrong = Arrays.deepToString(new int[][]{{1,2,3}, {5,4,0}});

        while (!heap.isEmpty()) {
            Node node = heap.poll();
            if (node.boardstring.equals(target))
                return node.depth;
            if (node.boardstring.equals(targetWrong))
                return -1;
            if (node.depth + node.heuristic > cost.get(node.boardstring))
                continue;

            for (int[] di: directions) {
                int nei_r = di[0] + node.zero_r;
                int nei_c = di[1] + node.zero_c;

                // If the neighbor is not on the board or wraps incorrectly around rows/cols
                if ((Math.abs(nei_r - node.zero_r) + Math.abs(nei_c - node.zero_c) != 1) ||
                        nei_r < 0 || nei_r >= R || nei_c < 0 || nei_c >= C)
                    continue;

                int[][] newboard = new int[R][C];
                int t = 0;
                for (int[] row: node.board)
                    newboard[t++] = row.clone();

                // Swap the elements on the new board
                newboard[node.zero_r][node.zero_c] = newboard[nei_r][nei_c];
                newboard[nei_r][nei_c] = 0;

                Node nei = new Node(newboard, nei_r, nei_c, node.depth+1);
                if (nei.depth + nei.heuristic >= cost.getOrDefault(nei.boardstring, 9999999))
                    continue;
                heap.add(nei);
                cost.put(nei.boardstring, nei.depth + nei.heuristic);
            }
        }

        return -1;
    }
}

class Node {
    int[][] board;
    String boardstring;
    int heuristic;
    int zero_r;
    int zero_c;
    int depth;
    Node(int[][] B, int zr, int zc, int d) {
        board = B;
        boardstring = Arrays.deepToString(board);

        //Calculate heuristic
        heuristic = 0;
        int R = B.length, C = B[0].length;
        for (int r = 0; r < R; ++r)
            for (int c = 0; c < C; ++c) {
                if (board[r][c] == 0) continue;
                int v = (board[r][c] + R*C - 1) % (R*C);
                // v/C, v%C: where board[r][c] should go in a solved puzzle
                heuristic += Math.abs(r - v/C) + Math.abs(c - v%C);
            }
        heuristic /= 2;
        zero_r = zr;
        zero_c = zc;
        depth = d;
    }
}
```

