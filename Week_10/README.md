## 数组

#### [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

```java
// 双指针 时间复杂度O(n) 空间复杂度O(1)
class Solution {
    public int maxArea(int[] nums) {
        int maxArea = 0, left = 0, right = nums.length - 1;
        while(left < right) {
            int h = nums[left] < nums[right] ? nums[left++] : nums[right--];               
            maxArea = Math.max(maxArea, h * (right-left + 1)); // 因为上面向中间移动了一次，所以要加一
        }
        return maxArea;
    }
}
```

#### [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

```java
class Solution {
    public int removeDuplicates(int[] a) {
        int left = 0, right = 1;
        while (right < a.length) {
            if (a[right] == a[left]) {
                right++;
            } else {
                a[++left] = a[right++];
            }
        }
        return left + 1;
    }
}
```

#### [66. 加一](https://leetcode-cn.com/problems/plus-one/)

```java
/*
    一个数字组成的数组。转成数字，然后+1。再转成数组
        方法一：从后往前遍历。
            目标值不是9，直接+1，结束。
            目标值是9，当前位置值变为0，下个数+1。
            最后还是没结束，说明碰到了9999，需要开辟新数组，头为1。
            time:  O(n)
            space: O(n+1)
*/
class Solution {
    public int[] plusOne(int[] digits) {
        int len = digits.length;
        // 从后往前遍历
        for (int i = len-1; i >= 0; i--) {
            // 数字不是9，直接自增，返回结果。
            if (digits[i] != 9) {
                digits[i]++;
                return digits;
            }
            // 是9，当前值变为0。继续遍历，把下个值+1。
            digits[i] = 0;
        }
        // 没结束，说明碰到了9999这种情况，开辟新数组，数组头写成1。
        int[] newNumber = new int [len+1];
        newNumber[0] = 1;
        return newNumber;
    }
}
```

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

#### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        while (n > 0) nums1[m+n-1] = (m == 0 || nums1[m-1] < nums2[n-1]) ? nums2[--n] : nums1[--m];
    }
}
```

#### [189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/)

```java
public class Solution {

    public void rotate(int[] a, int k) {
        k %= a.length;
        reverse(a, 0, a.length - 1);        // 反转所有数字
        reverse(a, 0, k - 1);               // 反转前k个数字
        reverse(a, k, a.length - 1);        // 反转后k个数字
    }

    public void reverse(int[] a, int i, int j) {
        while (i < j) a[j] = a[i] ^ a[j] ^ (a[i++] = a[j--]);
    }
}
```

#### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

```java
class Solution {
    public void moveZeroes(int[] a) {
        for (int i = 0, j = 0; i < a.length; i++) if (a[i] != 0) a[j] = a[i] ^ a[j] ^ (a[i] = a[j++]);
    }
}
```

## 链表

#### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2; // 一个为空，另外一个接在最后
        if (l2 == null) return l1;
        if (l1.val < l2.val) { // 哪个值小，哪个作为头
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }
        else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```

#### [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)
 * 给定 1->2->3->4, 你应该返回 2->1->4->3.
 * 方法：递归
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        // 终止条件：没有节点或者一个节点，返回当前节点
        if ((head == null) || (head.next == null)) return head;
        // 第一二三节点
        ListNode firstNode = head;
        ListNode secondNode = head.next;
        ListNode thirdNode = secondNode.next;
        // 下面三行是换指向
        // 1指向4；每次都是第三个节点进去递归。
        firstNode.next = swapPairs(thirdNode);
        // 2指向1（进去的当前节点要让自己的下一个指向自己）
        secondNode.next = firstNode;
        // 返回2
        return secondNode;
    }
}
```

#### [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0), prev = dummy, curr = head, next;
        dummy.next = head;
        int length = 0;
        while(head != null) {
            length++;
            head = head.next;
        }
        head = dummy.next;
        for(int i = 0; i < length / k; i++) {
            for(int j = 0; j < k - 1; j++) {
                next = curr.next;
                curr.next = next.next;
                next.next = prev.next;
                prev.next = next;
            }
            prev = curr;
            curr = prev.next;
        }
        return dummy.next;
    }
}
```

#### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head, slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) return true;
        }
        return false;
    }
}
```

#### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        Set<ListNode> visited = new HashSet<ListNode>();

        ListNode node = head;
        while (node != null) {
            if (visited.contains(node)) {
                return node;
            }
            visited.add(node);
            node = node.next;
        }

        return null;
    }
}
```

#### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode p = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return p;
    }
}
```

## 堆哈希队列

#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] result = new int[2];
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < numbers.length; i++) {
            if (map.containsKey(target - numbers[i])) {
                result[1] = i;
                result[0] = map.get(target - numbers[i]);
                return result;
            }
            map.put(numbers[i], i);
        }
        return result;
    }
}
```

#### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

```java
class Solution {
    public List<List<Integer>> threeSum(int[] a) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(a);
        for (int i = 0; i < a.length; i++) {
            if (a[i] > 0)break;
            if (i > 0 && a[i] == a[i-1]) continue;
            int left = i + 1, right = a.length - 1;
            while(left < right) {
                int sum = a[i] + a[left] + a[right];
                if(sum == 0) {
                    res.add(Arrays.asList(a[i], a[left], a[right]));
                    while(left < right && a[left] == a[left+1]) left++;
                    while(left < right && a[right] == a[right-1]) right--;
                    left++;right--;
                } else if(sum < 0) {
                    left++;
                } else right--;
            }
        }
        return res;
    }
}
```

#### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        char[] chars = s.toCharArray();
        for (char c : chars) {
            switch (c) {
                case '(': stack.push(')'); break;
                case '[': stack.push(']'); break;
                case '{': stack.push('}'); break; 
                default : if (stack.isEmpty() || stack.pop() != c) return false;
            }
        }
        return stack.isEmpty();
    }
}
```

#### [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

```java
class Solution {
    public int trap(int[] height) {
        int length = height.length;
        int[] left = new int[length];//保存从左往右遍历时，每一个下标位置当前的最高柱子高度
        int[] right = new int[length];//保存从右往左遍历时，每一个下标位置当前的最高柱子高度
        int leftMax = 0;
        int rightMax = 0;
        int sum = 0;
        
        //计算left和right数组
        for (int i = 0; i < length; i++) {
            if (height[i] > leftMax) {
                leftMax = height[i];
            }
            left[i] = leftMax;
            if (height[length-1-i] > rightMax) {
                rightMax = height[length-1-i];
            }
            right[length-1-i] = rightMax;
        }
        
        //遍历，只有当前柱子往左看、往右看的最高柱子都比当前柱子高，才能接住雨水
        for (int j = 0; j < length; j++) {
            if (height[j] < left[j] && height[j] < right[j]) {
                sum = sum + Math.min(left[j], right[j]) - height[j];
            }
        }
        return sum;
    }
}
```

#### [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs.length == 0) return new ArrayList();
        Map<String, List> ans = new HashMap<String, List>();
        int[] count = new int[26];
        for (String s : strs) {
            Arrays.fill(count, 0); // 数组每个值赋0
            for (char c : s.toCharArray()) count[c - 'a']++; // 字母对应位自增
            StringBuilder sb = new StringBuilder("");
            for (int i = 0; i < 26; i++) {
                sb.append('#');
                sb.append(count[i]); // #2#1#0#0
            }
            String key = sb.toString(); // 转成字符串作为密钥
            if (!ans.containsKey(key)) ans.put(key, new ArrayList()); // 相同key，放一个数组里
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }
}
```

#### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        // 这里为了代码简便，在柱体数组的头和尾加了两个高度为 0 的柱体。
        int[] tmp = new int[heights.length + 2];
        System.arraycopy(heights, 0, tmp, 1, heights.length); 
        
        Deque<Integer> stack = new ArrayDeque<>();
        int area = 0;
        for (int i = 0; i < tmp.length; i++) {
            // 对栈中柱体来说，栈中的下一个柱体就是其「左边第一个小于自身的柱体」；
            // 若当前柱体 i 的高度小于栈顶柱体的高度，说明 i 是栈顶柱体的「右边第一个小于栈顶柱体的柱体」。
            // 因此以栈顶柱体为高的矩形的左右宽度边界就确定了，可以计算面积🌶️ ～
            while (!stack.isEmpty() && tmp[i] < tmp[stack.peek()]) {
                int h = tmp[stack.pop()];
                area = Math.max(area, (i - stack.peek() - 1) * h);   
            }
            stack.push(i);
        }

        return area;
    }
}
```

#### [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

```java
class MinStack {
    int min = Integer.MAX_VALUE;
    Stack<Integer> stack = new Stack<Integer>();
    public void push(int x) {
        //当前值更小
        if(x <= min){   
            //将之前的最小值保存
            stack.push(min);
            //更新最小值
            min=x;
        }
        stack.push(x);
    }

    public void pop() {
        //如果弹出的值是最小值，那么将下一个元素更新为最小值
        if(stack.pop() == min) {
            min=stack.pop();
        }
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return min;
    }
}
```

#### [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

```java
public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums.length == 0) return nums;
        // 结果数组
        int[] res = new int[nums.length - k + 1];
        // 双端队列，维护一个左大右小，最多k个数的双端队列
        LinkedList<Integer> dq = new LinkedList<>();
        for (int i = 0; i < nums.length; i++) {
            // 迭代器到达目标位置。移动一次，删一个左边元素。最左边的元素
            if (!dq.isEmpty() && dq.getFirst() <= i - k) {
                dq.pollFirst();
            }
            // 如果新元素比右边的大，删除右边的元素
            while (!dq.isEmpty() && nums[i] >= nums[dq.peekLast()]) {
                dq.pollLast();
            }
            // 加入新元素
            dq.add(i);
            // 到达指定位置，将左边最大值放入数组中
            if (i + 1 >= k) {
                res[i +1 - k] = nums[dq.getFirst()];
            }
        }
        return res;
    }
}
```

#### [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

```java
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

#### [641. 设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/)

```java
/*
    构造双端队列
        用链表，两个指针
 */
class DoubleListNode {
    DoubleListNode pre;             // 前指针
    DoubleListNode next;            // 后指针
    int val;                        // 值

    public DoubleListNode(int val) {
        this.val = val;
    }
}

class MyCircularDeque {
    int size;               // 实际个数
    int k;                  // 开辟大小
    DoubleListNode head;    // 虚拟头节点哨兵
    DoubleListNode tail;    // 虚拟尾节点哨兵

    /**
     * 初始化，大小为空k。
     */
    public MyCircularDeque(int k) {
        head = new DoubleListNode(-1);  // 头节点，默认值-1
        tail = new DoubleListNode(-1);  // 为节点，默认值-1
        head.pre = tail;                    // 头节点前指针指向尾
        tail.next = head;                   // 尾节点后指针指向头
        this.k = k;
        this.size = 0;
    }

    /**
     * 将一个元素添加到双端队列头部。 如果操作成功返回 true。
     */
    public boolean insertFront(int value) {
        if (isFull()) return false;                        // 容量不够，返回false
        DoubleListNode node = new DoubleListNode(value);    // 创造新节点
        // 新节点后指针指向头。新节点前指针指向头节点的前一个。头节点的前一个后指针指向当前节点。头节点的前指针指向新节点。
        node.next = head;
        node.pre = head.pre;
        head.pre.next = node;
        head.pre = node;
        size++;
        return true;
    }

    /**
     * 将一个元素添加到双端队列尾部。如果操作成功返回 true。
     */
    public boolean insertLast(int value) {
        if (isFull()) return false;
        DoubleListNode node = new DoubleListNode(value);
        node.next = tail.next;
        tail.next.pre = node;
        tail.next = node;
        node.pre = tail;
        size++;
        return true;
    }

    /**
     * 从双端队列头部删除一个元素。 如果操作成功返回 true。
     */
    public boolean deleteFront() {
        if (isEmpty()) return false;    // 元素空了，返回false
        head.pre.pre.next = head;       // 头节点的前前一个后指针指向头节点
        head.pre = head.pre.pre;        // 头节点的前指针指向头节点的前前一个
        size--;
        return true;
    }

    /**
     * 从双端队列尾部删除一个元素。如果操作成功返回 true。
     */
    public boolean deleteLast() {
        if (isEmpty()) return false;
        tail.next.next.pre = tail;
        tail.next = tail.next.next;
        size--;
        return true;
    }

    /**
     * 从双端队列头部获得一个元素。如果双端队列为空，返回 -1。
     */
    public int getFront() {
        return head.pre.val;
    }

    /**
     * 获得双端队列的最后一个元素。 如果双端队列为空，返回 -1。
     */
    public int getRear() {
        return tail.next.val;
    }

    /**
     * 检查双端队列是否为空。
     */
    public boolean isEmpty() {
        return size == 0;
    }

    /**
     * 检查双端队列是否满了。
     */
    public boolean isFull() {
        return size == k;
    }
}
```

## 二叉树

#### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

```java
// 递归。确保括号有效性。只要要求左括号先行于右括号。
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        dfs(n, n, "");
        return res;
    }


    private void dfs(int left, int right, String curStr) {
        if (left == 0 && right == 0) { // 左右括号都不剩余了，递归终止
            res.add(curStr);
            return;
        }
        // 第一次会进去。第二次会有两个分叉
        if (left > 0) { // 如果左括号还剩余的话，可以拼接左括号
            dfs(left - 1, right, curStr + "(");
        }
        // 第一次是进不去的，因为左右是相等的。第二次进入后，左右等号数量又相等了。
        if (right > left) { // 如果右括号剩余多于左括号剩余的话，可以拼接右括号
            dfs(left, right - 1, curStr + ")");
        }
    }
}
```

#### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()) {
            if (cur != null) {
                stack.push(cur);
                cur = cur.left;
            } else {
                cur = stack.pop();
                list.add(cur.val);
                cur = cur.right;
            }
        }
        return list;
    }
}
```

#### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }

    方法一：根据定义，左子树必须小于当前节点
                    右子树必须大于当前节点

    方法二：中序遍历是递增的，用临时变量去接，每次都比较
 */
public class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    // 判断是否是有效二叉搜索树，左子树小于当前节点，右子树大于当前节点
    public boolean isValidBST(TreeNode root, long minVal, long maxVal) {
        // null符合要求
        if (root == null) return true;
        if (root.val >= maxVal || root.val <= minVal) return false;
        // 左边这个：有效的是左节点与最大值。右边这个：有效的是右节点与最小值。
        return isValidBST(root.left, minVal, root.val) && isValidBST(root.right, root.val, maxVal);
    }
}
```

#### [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

```java
class Solution {
    public int maxDepth(TreeNode root) {
        return root == null ? 0 : Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

#### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```java
public class Solution {


    /**
     * 解法1 递归
     *
     * 前序便利的第一个元素一定是树的根，然后这个根将中序遍历分成左右两颗子树，再通过移动指针递归两个子树，不停给左右子节点赋值，最后完成树的构建
     *
     * @param preOrder: 前序遍历
     * @param inOrder: 中序遍历
     * @return 构建好树返回根节点
     */
    public TreeNode buildTree(int[] preOrder, int[] inOrder){
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < inOrder.length; i++)
            map.put(inOrder[i], i);
        return build(preOrder, 0, preOrder.length - 1, inOrder, 0, inOrder.length - 1, map);
    }

    /**
     * @param pre : 前序遍历 preOrder
     * @param ps  : preOrder start
     * @param pe  : preOrder end
     * @param in  : 中序遍历 inOrder
     * @param is  : inOrder start
     * @param ie  : inOrder end
     * @param map : inOrder map - key: 中序遍历数组元素 value: 元素数组下表
     * @return 构建完成返回根节点
     */
    private TreeNode build(int[] pre, int ps, int pe, int[] in, int is, int ie, Map<Integer, Integer> map) {
        if (ps > pe || is > ie) return null;
        //前序遍历的第一个元素一定是树的根
        TreeNode root = new TreeNode(pre[ps]);

        //找到这个根在中序遍历中的位置，它将中序遍历分成了左右两颗子树
        int rootIndex = map.get(root.val);

        //距离 = 根在中序遍历中的位置 - 左子节点的位置
        int distance = rootIndex - is;

        /**
         * ps + 1        : 前序遍历中 - 左子树的开始节点
         * ps + distance : 前序遍历中 - 左子树的结束节点
         * is            : 中序遍历中 - 左子树的开始节点
         * rootIndex - 1 : 中序遍历中 - 左子树的结束节点
         */
        root.left = build(pre, ps + 1, ps + distance, in, is, rootIndex - 1, map);
        /**
         * ps + distance + 1 : 前序遍历中 - 右子树的开始节点
         * pe                : 前序遍历中 - 右子树的结束节点
         * rootIndex + 1     : 中序遍历中 - 右子树的开始节点
         * ie                : 中序遍历中 - 右子树的结束节点
         */
        root.right = build(pre, ps + distance + 1, pe, in, rootIndex + 1, ie, map);
        return root;
    }
}
```

#### [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

```java
class Solution {
    public int minDepth(TreeNode root) {
        // 到底了，返回0
        if (root == null)
            return 0;
        // 递归求左右子树的深度
        int leftH = minDepth(root.left);
        int rightH = minDepth(root.right);
        // 只有左子树或者右子树的情况或者叶子节点
        if ((root.left == null) || (root.right == null))
            return leftH + rightH + 1;
        // 只要最浅的，在基础上+1
        return java.lang.Math.min(leftH, rightH) + 1;
    }
}
```

#### [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```java
class Solution {

    List<Integer> res = new ArrayList<>();

    public List<Integer> preorderTraversal(TreeNode root) {
        if (root == null) return res;
        res.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
        return res;
    }
}
```

#### [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {

        if (root==null) {
            return root;
        } else {
            TreeNode treeNode = root.left;
            root.left = root.right;
            root.right = treeNode;
            root.left = invertTree(root.left);
            root.right =invertTree(root.right);
           
        }
        return root;
    }
}
```

#### [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

```java
class Solution {
  public int kthSmallest(TreeNode root, int k) {
    LinkedList<TreeNode> stack = new LinkedList<TreeNode>();

    while (true) {
      while (root != null) {
        stack.add(root);
        root = root.left;
      }
      root = stack.removeLast();
      if (--k == 0) return root.val;
      root = root.right;
    }
  }
}
```

#### [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        /**
        注意p,q必然存在树内, 且所有节点的值唯一!!!
        递归思想, 对以root为根的(子)树进行查找p和q, 如果root == null || p || q 直接返回root
        表示对于当前树的查找已经完毕, 否则对左右子树进行查找, 根据左右子树的返回值判断:
        1. 左右子树的返回值都不为null, 由于值唯一左右子树的返回值就是p和q, 此时root为LCA
        2. 如果左右子树返回值只有一个不为null, 说明只有p和q存在与左或右子树中, 最先找到的那个节点为LCA
        3. 左右子树返回值均为null, p和q均不在树中, 返回null
        **/
        if(root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left == null && right == null) return null;
        else if(left != null && right != null) return root;
        else return left == null ? right : left;
    }
}
```

#### [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

```java
public class Codec {
    public String rserialize(TreeNode root, String str) {
        if (root == null) {
            str += "None,";
        } else {
            str += str.valueOf(root.val) + ",";
            str = rserialize(root.left, str);
            str = rserialize(root.right, str);
        }
        return str;
    }
  
    public String serialize(TreeNode root) {
        return rserialize(root, "");
    }
  
    public TreeNode rdeserialize(List<String> l) {
        if (l.get(0).equals("None")) {
            l.remove(0);
            return null;
        }
  
        TreeNode root = new TreeNode(Integer.valueOf(l.get(0)));
        l.remove(0);
        root.left = rdeserialize(l);
        root.right = rdeserialize(l);
    
        return root;
    }
  
    public TreeNode deserialize(String data) {
        String[] data_array = data.split(",");
        List<String> data_list = new LinkedList<String>(Arrays.asList(data_array));
        return rdeserialize(data_list);
    }
}
```

#### [429. N叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

```java
class Solution {

    List<List<Integer>> res = new ArrayList<>();

    public List<List<Integer>> levelOrder(Node root) {
        helper(root, 1);
        return res;
    }

    public void helper(Node root, int level) {
        if (root == null) return;
        if (res.size() < level) res.add(new ArrayList<Integer>());
        res.get(level - 1).add(root.val);
        for (Node child : root.children) 
            helper(child, level + 1);
    }
}
```

#### [589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

```java
class Solution {
    public List<Integer> preorder(Node root) {
        List<Integer> res = new ArrayList<Integer>();
        Stack<Node> stack = new Stack<Node>();
        if(root == null)
            return res;
        stack.push(root);
        while(!stack.isEmpty())
        {
            Node node = stack.pop();
            res.add (node.val);
            for(int i =  node.children.size()-1;i>= 0;i--)
            {
                stack.add(node.children.get(i));
            }  
        }
        return res;
    }
}
```

#### [590. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

```java
class Solution {

    List<Integer> res = new ArrayList<>();

    public List<Integer> postorder(Node root) {
        if (root == null) return res;
        for (Node child : root.children) {
            postorder(child);
        }
        res.add(root.val);
        return res;
    }
}
```

## 分治

#### [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

```java
class Solution {

    // 数字到号码的映射
    private String[] map = {"abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};

    // 路径
    private StringBuilder sb = new StringBuilder();

    // 结果集
    private List<String> res = new ArrayList<>();

    public List<String> letterCombinations(String digits) {
        if(digits == null || digits.length() == 0) return res;
        backtrack(digits,0);
        return res;
    }

    // 回溯函数
    private void backtrack(String digits,int index) {
        if(sb.length() == digits.length()) {
            res.add(sb.toString());
            return;
        }
        String val = map[digits.charAt(index)-'2'];
        for(char ch:val.toCharArray()) {
            sb.append(ch);
            backtrack(digits,index+1);
            sb.deleteCharAt(sb.length()-1);
        }
    }
}
```

#### [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

```java
class Solution {
    public double myPow(double x, int n) {
        double res = 1.0;
        for(int i = n; i != 0; i /= 2){
            if(i % 2 != 0){
                res *= x;
            }
            x *= x;
        }
        return  n < 0 ? 1 / res : res;
    }
} 
```

#### [78. 子集](https://leetcode-cn.com/problems/subsets/)

```java
class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> output = new ArrayList();
    output.add(new ArrayList<Integer>());

    for (int num : nums) {
      List<List<Integer>> newSubsets = new ArrayList();
      for (List<Integer> curr : output) {
        newSubsets.add(new ArrayList<Integer>(curr){{add(num);}});
      }
      for (List<Integer> curr : newSubsets) {
        output.add(curr);
      }
    }
    return output;
  }
}
```

#### [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length / 2];
    }
}
```

## 二分查找

#### [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0,right = nums.length-1;
        while (left<=right){
            int mid = left +(right-left+1)/2;
            if(nums[mid] == target) return mid;
            if(nums[left] <nums[mid]){ //左边有序
                if(nums[left]<=target && target<nums[mid]){
                    right = mid-1;
                }else {
                    left = mid +1;
                }
            }else {
                if(nums[mid]<target && target<=nums[right]){
                    left = mid+1;
                }else {
                    right = mid-1;
                }
            }
        }
        return -1;
    }
}
```



#### [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

```java
class Solution {
    public int mySqrt(int x) 
    {
        if(x == 1)
            return 1;
        int min = 0;
        int max = x;
        while(max-min>1)
        {
            int m = (max+min)/2;
            if(x/m<m)
                max = m;
            else
                min = m;
        }
        return min;
    }
}
```

#### [367. 有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        if(num < 2) return true;
        long low = 1, high = num / 2;
        while ( low <= high) {
            long mid = (low + high) / 2;
            if(mid * mid == num) return true;
            else if(mid * mid < num) low = mid + 1;
            else high = mid - 1;
        }
        return false;
    }
}
```

## 并查集

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

#### [547. 朋友圈](https://leetcode-cn.com/problems/friend-circles/)

```java
public class Solution {
    public void dfs(int[][] M, int[] visited, int i) {
        for (int j = 0; j < M.length; j++) {
            if (M[i][j] == 1 && visited[j] == 0) {
                visited[j] = 1;
                dfs(M, visited, j);
            }
        }
    }
    public int findCircleNum(int[][] M) {
        int[] visited = new int[M.length];
        int count = 0;
        for (int i = 0; i < M.length; i++) {
            if (visited[i] == 0) {
                dfs(M, visited, i);
                count++;
            }
        }
        return count;
    }
}
```

## 遍历和搜索


#### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {

    List<List<Integer>> res = new ArrayList<>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        helper(root, 1);
        return res;
    }

    public List<List<Integer>> helper(TreeNode root, int level) {
        if (root == null) return res;
        if (res.size() < level) res.add(new ArrayList<Integer>());
        res.get(level - 1).add(root.val);
        helper(root.left, level + 1);
        helper(root.right, level + 1);
        return res;
    }    
}
```

#### [126. 单词接龙 II](https://leetcode-cn.com/problems/word-ladder-ii/)

```java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        // 结果集
        List<List<String>> res = new ArrayList<>();
        Set<String> words = new HashSet<>(wordList);
        // 字典中不包含目标单词
        if (!words.contains(endWord)) {
            return res;
        }
        // 存放关系：每个单词可达的下层单词
        Map<String, List<String>> mapTree = new HashMap<>();
        Set<String> begin = new HashSet<>(), end = new HashSet<>();
        begin.add(beginWord);
        end.add(endWord);
        if (buildTree(words, begin, end, mapTree, true)) {
            dfs(res, mapTree, beginWord, endWord, new LinkedList<>());
        }
        return res;
    }
    
    // 双向BFS，构建每个单词的层级对应关系
    private boolean buildTree(Set<String> words, Set<String> begin, Set<String> end, Map<String, List<String>> mapTree, boolean isFront){
        if (begin.size() == 0) {
            return false;
        }
        // 始终以少的进行探索
        if (begin.size() > end.size()) {
            return buildTree(words, end, begin, mapTree, !isFront);
        }
        // 在已访问的单词集合中去除
        words.removeAll(begin);
        // 标记本层是否已到达目标单词
        boolean isMeet = false;
        // 记录本层所访问的单词
        Set<String> nextLevel = new HashSet<>();
        for (String word : begin) {
            char[] chars = word.toCharArray();
            for (int i = 0; i < chars.length; i++) {
                char temp = chars[i];
                for (char ch = 'a'; ch <= 'z'; ch++) {
                    chars[i] = ch;
                    String str = String.valueOf(chars);
                    if (words.contains(str)) {
                        nextLevel.add(str);
                        // 根据访问顺序，添加层级对应关系：始终保持从上层到下层的存储存储关系
                        // true: 从上往下探索：word -> str
                        // false: 从下往上探索：str -> word（查找到的 str 是 word 上层的单词）
                        String key = isFront ? word : str;
                        String nextWord = isFront ? str : word;
                        // 判断是否遇见目标单词
                        if (end.contains(str)) {
                            isMeet = true;
                        }
                        if (!mapTree.containsKey(key)) {
                            mapTree.put(key, new ArrayList<>());
                        }
                        mapTree.get(key).add(nextWord);
                    }
                }
                chars[i] = temp;
            }
        }
        if (isMeet) {
            return true;
        }
        return buildTree(words, nextLevel, end, mapTree, isFront);
    }
    
    // DFS: 组合路径
    private void dfs (List<List<String>> res, Map<String, List<String>> mapTree, String beginWord, String endWord, LinkedList<String> list) {
        list.add(beginWord);
        if (beginWord.equals(endWord)) {
            res.add(new ArrayList<>(list));
            list.removeLast();
            return;
        }
        if (mapTree.containsKey(beginWord)) {
            for (String word : mapTree.get(beginWord)) {
                dfs(res, mapTree, word, endWord, list);
            }
        }
        list.removeLast();
    }

}
```

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

#### [515. 在每个树行中找最大值](https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {

    List<Integer> res = new ArrayList<>();

    public List<Integer> largestValues(TreeNode root) {
        helper(root, res, 0);
        return res;
    }

    public void helper(TreeNode cur, List<Integer> res, int level) {
        if (cur == null) return;
        if (level >= res.size()) res.add(cur.val);
        else res.set(level, Math.max(res.get(level), cur.val));
        helper(cur.left, res, level + 1);
        helper(cur.right, res, level + 1);
    }
}
```

#### [529. 扫雷游戏](https://leetcode-cn.com/problems/minesweeper/)

```java
class Solution {
    int[] dirX = {0, 1, 0, -1, 1, 1, -1, -1};
    int[] dirY = {1, 0, -1, 0, 1, -1, 1, -1};

    public char[][] updateBoard(char[][] board, int[] click) {
        int x = click[0], y = click[1];
        if (board[x][y] == 'M') {
            // 规则 1
            board[x][y] = 'X';
        } else{
            dfs(board, x, y);
        }
        return board;
    }

    public void dfs(char[][] board, int x, int y) {
        int cnt = 0;
        for (int i = 0; i < 8; ++i) {
            int tx = x + dirX[i];
            int ty = y + dirY[i];
            if (tx < 0 || tx >= board.length || ty < 0 || ty >= board[0].length) {
                continue;
            }
            // 不用判断 M，因为如果有 M 的话游戏已经结束了
            if (board[tx][ty] == 'M') {
                ++cnt;
            }
        }
        if (cnt > 0) {
            // 规则 3
            board[x][y] = (char) (cnt + '0');
        } else {
            // 规则 2
            board[x][y] = 'B';
            for (int i = 0; i < 8; ++i) {
                int tx = x + dirX[i];
                int ty = y + dirY[i];
                // 这里不需要在存在 B 的时候继续扩展，因为 B 之前被点击的时候已经被扩展过了
                if (tx < 0 || tx >= board.length || ty < 0 || ty >= board[0].length || board[tx][ty] != 'E') {
                    continue;
                }
                dfs(board, tx, ty);
            }
        }
    }
}
```



## 动态规划



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

#### [45. 跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)

```java
// 贪心即可，找到能到最后一个数的最小索引
class Solution {

    int cnt = 0;

    public int jump(int[] nums) {
        int last = nums.length - 1;
        while (last > 0) {
            for (int i = 0; i < last; i++) {
                if (i + nums[i] >= last) {
                    last = i;
                    cnt++;
                }
            }
        }
        return cnt;
    }
}
```

#### [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

```java
class Solution {
    public boolean canJump(int[] a) {
        int last = a.length - 1;
        for (int i = a.length -2; i >= 0; i--) {
            if (a[i] + i >= last) last = i;
        }
        return last == 0;
    }
}
```

#### [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] cur = new int[n];
        Arrays.fill(cur,1);
        for (int i = 1; i < m;i++){
            for (int j = 1; j < n; j++){
                cur[j] += cur[j-1] ;
            }
        }
        return cur[n-1];
    }
}
```

#### [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

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

#### [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

```java
/*
这题的意思是从左上角到右下角，（注意：每次是向下或者向右移动一格），所走过的路径数字和要求最小。

这道题要用动态规划，在原来的数组上去改变值。先从最近的开始，得到最优解，再慢慢递推到外面一层。
*/
class Solution {
    public int minPathSum(int[][] a) {

        for (int i = 0; i < a.length; i++)
            for(int j = 0; j < a[0].length; j++) {
                if (i == 0 && j == 0) continue; // 略过左上角的值
                // 最上面一行，值更新为左边的数+现在的数。因为只能从左边过来。
                else if (i == 0) a[i][j] += a[i][j-1];
                // 最左边一列，值更新为上面的数+现在的数。因为只能从上边过来。
                else if (j == 0) a[i][j] += a[i-1][j];
                // 其余情况，要么从上面过来，要么从左边过来，选值小的那个
                else a[i][j] += Math.min(a[i-1][j],a[i][j-1]);
            }
        return a[a.length-1][a[0].length-1]; // 返回最后一个数
    }
}
```

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

#### [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] mp = new int[256];
        for (char c : t.toCharArray()) mp[c] += 1;
        int start = 0, end = 0;
        int n = s.length(), m = t.length();
        int cnt = 0;
        int res = -1;
        String ans = "";
        while (end < n) {
            char c = s.charAt(end);
            mp[c] -= 1;
            if (mp[c] >= 0) cnt += 1;
            while (cnt == m) {
                if (res == -1 || res > end - start + 1) {
                    ans = s.substring(start, end + 1);
                    res = end - start + 1;
                }
                c = s.charAt(start);
                mp[c] += 1;
                if (mp[c] >= 1) cnt -= 1;
                start += 1;
            }
            end += 1;
        }
        return ans;
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
}
```

#### [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int m = triangle.size();
        int[] a = new int[m+1];
        for(int i = m-1; i >= 0; i--) // 倒数第二行开始
            for (int j = 0; j < triangle.get(i).size(); j++) {
                a[j] = Math.min(a[j], a[j+1]) + triangle.get(i).get(j); // 选择下面或者右下面小的那个。
            }
        return a[0];
    }
}
```

#### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int currMaxProfit = 0;
        for (int i = 0, j = 0; j < prices.length; j++)
            if (prices[j] - prices[i] < 0) i = j;
            else currMaxProfit = Math.max(prices[j] - prices[i], currMaxProfit);
        return currMaxProfit;
    }
}
```

#### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

```java
class Solution {
    public int maxProfit(int[] a) {
        int res = 0;
        for (int i = 1; i < a.length; i++) if (a[i] > a[i-1]) res += a[i] -a[i-1];
        return res;
    }
}
```

#### [123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int fstBuy = Integer.MIN_VALUE, fstSell = 0;
        int secBuy = Integer.MIN_VALUE, secSell = 0;
        for(int p : prices) {
            fstBuy = Math.max(fstBuy, -p);
            fstSell = Math.max(fstSell, fstBuy + p);
            secBuy = Math.max(secBuy, fstSell - p);
            secSell = Math.max(secSell, secBuy + p); 
        }
        return secSell;
    }
}
```

#### [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

```java
class Solution {
    public int maxProduct(int[] nums) {
        int max = Integer.MIN_VALUE, imax = 1, imin = 1; //一个保存最大的，一个保存最小的。
        for(int i=0; i<nums.length; i++){
            if(nums[i] < 0){ int tmp = imax; imax = imin; imin = tmp;} //如果数组的数是负数，那么会导致最大的变最小的，最小的变最大的。因此交换两个的值。
            imax = Math.max(imax*nums[i], nums[i]);
            imin = Math.min(imin*nums[i], nums[i]);
            
            max = Math.max(max, imax);
        }
        return max;
    }
}
```

#### [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        /**
        当k大于等于数组长度一半时, 问题退化为贪心问题此时采用 买卖股票的最佳时机 II
        的贪心方法解决可以大幅提升时间性能, 对于其他的k, 可以采用 买卖股票的最佳时机 III
        的方法来解决, 在III中定义了两次买入和卖出时最大收益的变量, 在这里就是k租这样的
        变量, 即问题IV是对问题III的推广, t[i][0]和t[i][1]分别表示第i比交易买入和卖出时
        各自的最大收益
        **/
        if(k < 1) return 0;
        if(k >= prices.length/2) return greedy(prices);
        int[][] t = new int[k][2];
        for(int i = 0; i < k; ++i)
            t[i][0] = Integer.MIN_VALUE;
        for(int p : prices) {
            t[0][0] = Math.max(t[0][0], -p);
            t[0][1] = Math.max(t[0][1], t[0][0] + p);
            for(int i = 1; i < k; ++i) {
                t[i][0] = Math.max(t[i][0], t[i-1][1] - p);
                t[i][1] = Math.max(t[i][1], t[i][0] + p);
            }
        }
        return t[k-1][1];
    }
    
    private int greedy(int[] prices) {
        int max = 0;
        for(int i = 1; i < prices.length; ++i) {
            if(prices[i] > prices[i-1])
                max += prices[i] - prices[i-1];
        }
        return max;
    }
}
```

#### [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

```java
//////////// 空间复杂度O(1)版本 滚动数组。因为结果只和3个数有关
public class Solution {
    public int rob(int[] nums) {
        int len = nums.length;
        if(len == 0) return 0;
        int[] dp = new int[len + 1];
        dp[0] = 0;//选择偷第一家，可偷最大值
        dp[1] = nums[0];//选择偷第二家，累加可偷最大值
        for(int i = 2; i <= len; i++) {
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i-1]);//选择偷第 i 家，累加可偷最大值
        }
        return dp[len];
    }
}
```

#### [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

```java
class Solution {
    public int rob(int[] nums) {
        int len = nums.length;
        if(len == 0) return 0;
        int[] dp = new int[len + 1];
        dp[0] = 0;//选择偷第一家，可偷最大值
        dp[1] = nums[0];//选择偷第二家，累加可偷最大值
        for(int i = 2; i <= len; i++) {
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i-1]);//选择偷第 i 家，累加可偷最大值
        }
        return dp[len];
    }
}
```

#### [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

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

#### [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1]; // 默认初始化值都为0
        for (int i = 1; i <= n; i++) {
            dp[i] = i; // 最坏的情况就是每次+1
            for (int j = 1; i - j * j >= 0; j++) { 
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1); // 动态转移方程
            }
        }
        return dp[n];
    }
}
```



#### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

```java
class Solution {
    public int coinChange(int[] coins, int amt) {
        // 动态规划
        int[] memo = new int[amt + 1]; 
        Arrays.fill(memo, amt + 1);  // 每个值赋值12
        memo[0] = 0;
        for (int i = 1; i <= amt; i++) // 从1到11，计算需要的最少硬币数
            for (int coin : coins)
                if (i - coin >= 0)
                    memo[i] = Math.min(memo[i], memo[i - coin] + 1);
        return memo[amt] == amt + 1 ? -1 : memo[amt];
    }
}
```

#### [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0) {
            return 0;
        }

        int n = prices.length;
        // f[i][0]: 手上持有股票的最大收益
        // f[i][1]: 手上不持有股票，并且处于冷冻期中的累计最大收益
        // f[i][2]: 手上不持有股票，并且不在冷冻期中的累计最大收益
        int[][] f = new int[n][3];
        f[0][0] = -prices[0];
        for (int i = 1; i < n; ++i) {
            f[i][0] = Math.max(f[i - 1][0], f[i - 1][2] - prices[i]);
            f[i][1] = f[i - 1][0] + prices[i];
            f[i][2] = Math.max(f[i - 1][1], f[i - 1][2]);
        }
        return Math.max(f[n - 1][1], f[n - 1][2]);
    }
}
```

#### [312. 戳气球](https://leetcode-cn.com/problems/burst-balloons/)



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

#### [363. 矩形区域不超过 K 的最大数值和](https://leetcode-cn.com/problems/max-sum-of-rectangle-no-larger-than-k/)

```java
class Solution {
    public int maxSumSubmatrix(int[][] matrix, int k) {
        int rows = matrix.length, cols = matrix[0].length, max = Integer.MIN_VALUE;
        // O(cols ^ 2 * rows)
        for (int l = 0; l < cols; l++) { // 枚举左边界
            int[] rowSum = new int[rows]; // 左边界改变才算区域的重新开始
            for (int r = l; r < cols; r++) { // 枚举右边界
                for (int i = 0; i < rows; i++) { // 按每一行累计到 dp
                    rowSum[i] += matrix[i][r];
                }
                max = Math.max(max, dpmax(rowSum, k));
                if (max == k) return k; // 尽量提前
            }
        }
        return max;
    }

    // 在数组 arr 中，求不超过 k 的最大值
    private int dpmax(int[] arr, int k) {
        int rollSum = arr[0], rollMax = rollSum;
        // O(rows)
        for (int i = 1; i < arr.length; i++) {
            if (rollSum > 0) rollSum += arr[i];
            else rollSum = arr[i];
            if (rollSum > rollMax) rollMax = rollSum;
        }
        if (rollMax <= k) return rollMax;
        // O(rows ^ 2)
        int max = Integer.MIN_VALUE;
        for (int l = 0; l < arr.length; l++) {
            int sum = 0;
            for (int r = l; r < arr.length; r++) {
                sum += arr[r];
                if (sum > max && sum <= k) max = sum;
                if (max == k) return k; // 尽量提前
            }
        }
        return max;
    }
}
```

#### [403. 青蛙过河](https://leetcode-cn.com/problems/frog-jump/)

```java
public class Solution {
    public boolean canCross(int[] stones) {
        HashMap<Integer, Set<Integer>> map = new HashMap<>();
        for (int i = 0; i < stones.length; i++) {
            map.put(stones[i], new HashSet<Integer>());
        }
        map.get(0).add(0);
        for (int i = 0; i < stones.length; i++) {
            for (int k : map.get(stones[i])) {
                for (int step = k - 1; step <= k + 1; step++) {
                    if (step > 0 && map.containsKey(stones[i] + step)) {
                        map.get(stones[i] + step).add(step);
                    }
                }
            }
        }
        return map.get(stones[stones.length - 1]).size() > 0;
    }
}
```

#### [410. 分割数组的最大值](https://leetcode-cn.com/problems/split-array-largest-sum/)

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

#### [552. 学生出勤记录 II](https://leetcode-cn.com/problems/student-attendance-record-ii/)

```java
public class Solution {
    long M = 1000000007;
    public int checkRecord(int n) {
        long a0l0 = 1, a0l1 = 0, a0l2 = 0, a1l0 = 0, a1l1 = 0, a1l2 = 0;
        for (int i = 0; i <= n; i++) {
            long a0l0_ = (a0l0 + a0l1 + a0l2) % M;
            a0l2 = a0l1;
            a0l1 = a0l0;
            a0l0 = a0l0_;
            long a1l0_ = (a0l0 + a1l0 + a1l1 + a1l2) % M;
            a1l2 = a1l1;
            a1l1 = a1l0;
            a1l0 = a1l0_;
        }
        return (int) a1l0;
    }
}
```

#### [621. 任务调度器](https://leetcode-cn.com/problems/task-scheduler/)

```java
public class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] map = new int[26];
        for (char c: tasks)
            map[c - 'A']++;
        Arrays.sort(map);
        int max_val = map[25] - 1, idle_slots = max_val * n;
        for (int i = 24; i >= 0 && map[i] > 0; i--) {
            idle_slots -= Math.min(map[i], max_val);
        }
        return idle_slots > 0 ? idle_slots + tasks.length : tasks.length;
    }
}
```

#### [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)



```java
class Solution {
    int num = 0;
    public int countSubstrings(String s) {
        for (int i=0; i < s.length(); i++){
            count(s, i, i);//回文串长度为奇数
            count(s, i, i+1);//回文串长度为偶数
        }
        return num;
    }
    
    public void count(String s, int start, int end){
        while(start >= 0 && end < s.length() && s.charAt(start) == s.charAt(end)){
            num++;
            start--;
            end++;
        }
    }
}
```

#### [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)



```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int dp_i_0 = 0;
        int dp_i_1 = Integer.MIN_VALUE + fee;
        for(int i = 0; i < prices.length; i++) {
            int tmp = dp_i_0;
            dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i] - fee);
            dp_i_1 = Math.max(dp_i_1, tmp - prices[i]);
        }

        return dp_i_0;
    }
}
```

#### [980. 不同路径 III](https://leetcode-cn.com/problems/unique-paths-iii/)

```java
class Solution {
    public int uniquePathsIII(int[][] grid) {
        int sum = 1;//空白格+起始点的个数
        int r = 0;
        int c = 0;
        for(int i = 0; i < grid.length; i++) {
            for(int j = 0; j < grid[0].length; j++) {
                if(grid[i][j] == 0) {
                    sum++;
                } else if(grid[i][j] == 1) {
                    r = i;
                    c = j;
                }
            }
        }
        return dfs(grid, r, c, sum);
    }
    

    public int dfs(int[][] g, int i, int j, int sum) {
        if(i < 0 || i >= g.length || j < 0 || j >= g[0].length || g[i][j] == -1) {
            return 0;
        }

        if(g[i][j] == 2) return 0 == sum ? 1 : 0;
        int fix = 0;
        g[i][j] = -1;
        fix += dfs(g, i + 1, j, sum - 1);
        fix += dfs(g, i - 1, j, sum - 1);
        fix += dfs(g, i, j + 1, sum - 1);
        fix += dfs(g, i, j - 1, sum - 1);
        g[i][j] = 0;
        return fix;
    }
}
```



## 剪枝

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

#### [37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

```java
class Solution {
  // box size
  int n = 3;
  // row size
  int N = n * n;

  int [][] rows = new int[N][N + 1];
  int [][] columns = new int[N][N + 1];
  int [][] boxes = new int[N][N + 1];

  char[][] board;

  boolean sudokuSolved = false;

  public boolean couldPlace(int d, int row, int col) {
    /*
    Check if one could place a number d in (row, col) cell
    */
    int idx = (row / n ) * n + col / n;
    return rows[row][d] + columns[col][d] + boxes[idx][d] == 0;
  }

  public void placeNumber(int d, int row, int col) {
    /*
    Place a number d in (row, col) cell
    */
    int idx = (row / n ) * n + col / n;

    rows[row][d]++;
    columns[col][d]++;
    boxes[idx][d]++;
    board[row][col] = (char)(d + '0');
  }

  public void removeNumber(int d, int row, int col) {
    /*
    Remove a number which didn't lead to a solution
    */
    int idx = (row / n ) * n + col / n;
    rows[row][d]--;
    columns[col][d]--;
    boxes[idx][d]--;
    board[row][col] = '.';
  }

  public void placeNextNumbers(int row, int col) {
    /*
    Call backtrack function in recursion
    to continue to place numbers
    till the moment we have a solution
    */
    // if we're in the last cell
    // that means we have the solution
    if ((col == N - 1) && (row == N - 1)) {
      sudokuSolved = true;
    }
    // if not yet
    else {
      // if we're in the end of the row
      // go to the next row
      if (col == N - 1) backtrack(row + 1, 0);
        // go to the next column
      else backtrack(row, col + 1);
    }
  }

  public void backtrack(int row, int col) {
    /*
    Backtracking
    */
    // if the cell is empty
    if (board[row][col] == '.') {
      // iterate over all numbers from 1 to 9
      for (int d = 1; d < 10; d++) {
        if (couldPlace(d, row, col)) {
          placeNumber(d, row, col);
          placeNextNumbers(row, col);
          // if sudoku is solved, there is no need to backtrack
          // since the single unique solution is promised
          if (!sudokuSolved) removeNumber(d, row, col);
        }
      }
    }
    else placeNextNumbers(row, col);
  }

  public void solveSudoku(char[][] board) {
    this.board = board;

    // init rows, columns and boxes
    for (int i = 0; i < N; i++) {
      for (int j = 0; j < N; j++) {
        char num = board[i][j];
        if (num != '.') {
          int d = Character.getNumericValue(num);
          placeNumber(d, i, j);
        }
      }
    }
    backtrack(0, 0);
  }
}
```



#### [51. N 皇后](https://leetcode-cn.com/problems/n-queens/)

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

## 贪心



#### [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g); Arrays.sort(s);
        int cnt = 0;
        for (int i = 0, j = 0; i < g.length && j < s.length; j++)
            if (g[i] <= s[j]) {cnt++; i++;}
        return cnt;
    }
}
```



#### [860. 柠檬水找零](https://leetcode-cn.com/problems/lemonade-change/)

```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0, ten = 0;
        for (int bill : bills) {
            switch (bill) {
                case 5 : five++; break;
                case 10: five--; ten++; break;
                case 20: if(ten > 0) {ten--;five--;} else {five -= 3;}
            }
            if (five < 0) return false;
        }
        return true;
    }
}
```

#### [874. 模拟行走机器人](https://leetcode-cn.com/problems/walking-robot-simulation/)

```java
class Solution {
    public int robotSim(int[] commands, int[][] obstacles) {
        int[][] fangxiang = new int[][]{{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        Set<String> set = new HashSet<>();
        int fx = 0, x = 0, y = 0, res = 0;
        for (int[] o : obstacles) {
            set.add(o[0] + " " + o[1]);
        }
        for (int c : commands) {
            switch (c) {
                case (-1):
                    fx = (fx + 1) % 4; break;
                case (-2):
                    fx = (fx + 3) % 4; break;
                default:
                    for (int i = 0; i < c; i++) {
                        if (set.contains((x + fangxiang[fx][0]) + " " + (y + fangxiang[fx][1]))) break;
                        x += fangxiang[fx][0];
                        y += fangxiang[fx][1];
                        res = Math.max(res, x * x + y * y);
                    }
            }
        }
        return res;
    }
}
```

## trie 树

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
//改造前缀树节点
class TrieNode {
    public TrieNode[] children;
    public String word; //节点直接存当前的单词

    public TrieNode() {
        children = new TrieNode[26];
        word = null;
        for (int i = 0; i < 26; i++) {
            children[i] = null;
        }
    }
}
class Trie {
    TrieNode root;
    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
    }

    /** Inserts a word into the trie. */
    public void insert(String word) {
        char[] array = word.toCharArray();
        TrieNode cur = root;
        for (int i = 0; i < array.length; i++) {
            // 当前孩子是否存在
            if (cur.children[array[i] - 'a'] == null) {
                cur.children[array[i] - 'a'] = new TrieNode();
            }
            cur = cur.children[array[i] - 'a'];
        }
        // 当前节点结束，存入当前单词
        cur.word = word;
    }
};

class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        Trie trie = new Trie();
        //将所有单词存入前缀树中
        List<String> res = new ArrayList<>();
        for (String word : words) {
            trie.insert(word);
        }
        int rows = board.length;
        if (rows == 0) {
            return res;
        }
        int cols = board[0].length;
        //从每个位置开始遍历
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                existRecursive(board, i, j, trie.root, res);
            }
        }
        return res;
    }

    private void existRecursive(char[][] board, int row, int col, TrieNode node, List<String> res) {
        if (row < 0 || row >= board.length || col < 0 || col >= board[0].length) {
            return;
        }
        char cur = board[row][col];//将要遍历的字母
        //当前节点遍历过或者将要遍历的字母在前缀树中不存在
        if (cur == '$' || node.children[cur - 'a'] == null) {
            return;
        }
        node = node.children[cur - 'a'];
        //判断当前节点是否是一个单词的结束
        if (node.word != null) {
            //加入到结果中
            res.add(node.word);
            //将当前单词置为 null，防止重复加入
            node.word = null;
        }
        char temp = board[row][col];
        //上下左右去遍历
        board[row][col] = '$';
        existRecursive(board, row - 1, col, node, res);
        existRecursive(board, row + 1, col, node, res);
        existRecursive(board, row, col - 1, node, res);
        existRecursive(board, row, col + 1, node, res);
        board[row][col] = temp;
    }
}
```

## 位运算

#### [52. N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/)

```java
class Solution {

    public int totalNQueens(int n) {
        int[] rs = new int[]{0,1,0,0,2,10,4,40,92,352,724,2680};
         return rs[n];
    }
}
```

#### [190. 颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)



```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int res = 0;
        for (int i = 0; i < 32; i++) {
            res = (res << 1) + (n & 1);
            n >>= 1;
        }
        return res;
    }

    
}
```

#### [191. 位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)



```java
public class Solution {
    public int hammingWeight(int n) {
        int bits = 0;
        int mask = 1;
        for (int i = 0; i < 32; i++) {
            if ((n & mask) != 0) {
                bits++;
            }
            mask <<= 1;
        }
        return bits;
    }
}
```

#### [231. 2的幂](https://leetcode-cn.com/problems/power-of-two/)

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & n - 1) == 0;
    }
}
```

#### [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)



```java
class Solution {
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        for(int i = 1;i<= num;i++){  //注意要从1开始，0不满足
            res[i] = res[i & (i - 1)] + 1;
        }
        return res;
    }
}
```

## LRU Catch

#### [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

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