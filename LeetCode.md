# Tree

## 94、二叉树的中序遍历

[94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

// 题解1：中序遍历 递归写法
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null){
            return new LinkedList<Integer>();
        }

        List<Integer> list = new LinkedList<Integer>();
        helper(list, root);
        return list;
    }

    private static void helper(List list, TreeNode node){
        if(node == null){
            return;
        }
        helper(list,node.left);  
        list.add(node.val);
        helper(list,node.right);   
    }
    // 题解2：中序遍历 非递归写法
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null){
            return new LinkedList<Integer>();
        }
        List<Integer> list = new LinkedList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        while(!stack.empty() || root != null){
            if(root != null){
                stack.push(root);
                root = root.left;
            } else {
                root = stack.pop();
                list.add(root.val);
                root = root.right;
            }
        }

        return list;
    }

}
}
```

96、不同的二叉搜索树

[96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

```JAVA
class Solution {
    // 动态规划
    public int numTrees(int n) {
        if(n == 0) return 1;
        if(n == 1) return 1;

        int[] G = new int[n+1];
        G[0] = 1;
        G[1] = 1;

        for(int i = 2; i <= n; i++) {
            for(int j = 1; j <= i; j++) { // 当前以 j 作为根结点的搜索二叉树 个数
                G[i] += G[j - 1] * G[i - j];
            }
        }
        
        return G[n];
    }
}
```



## 98、验证二叉搜索树

[98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

```JAVA
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
 /**
    思路：中序遍历打印的结果是递增的。
  */

// 方法1
class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) { // base case
            return true;
        }
        // 访问左子树
        if (!isValidBST(root.left)) {
            return false;
        }
        // 访问当前节点：如果当前节点小于等于中序遍历的前一个节点，说明不满足BST，返回 false；否则继续遍历。
        if (root.val <= pre) {
            return false;
        }
        pre = root.val;

        // // 访问右子树
        // if(!isValidBST(root.right)){
        //     return false;
        // }

        // return true;
        return isValidBST(root.right);
    }
}

// 方法2
// class Solution {
//     public boolean isValidBST(TreeNode root) {
//         if(root != null) {
        
//             long pre = Long.MIN_VALUE;
//             Stack<TreeNode> stack = new Stack<TreeNode>();
            
        
//             while(!stack.isEmpty() || root != null ){
//                 if(root != null){
//                     stack.push(root);
//                     root = root.left;
//                 } else {
//                     root = stack.pop();
                
//                     // 比较当前结点的值 与 前一个节点的值，若当前结点的值小 则返回 false
//                     if(root.val <=  pre){
//                         return false;
//                     } else {
//                         pre = root.val;
//                     }
                
//                     root = root.right;
//                 }
//             }
//         }
//         return true;
//     }
// }

// 方法3
// class Solution {
//     public boolean isValidBST(TreeNode root) {
//         if(root == null) return true;

//         List<Integer> list = unRecurInOrder(root);
//         int preValue = (int)list.get(0);

//         for(int i = 1; i < list.size(); i++){
//             if(preValue >= list.get(i)){
//                 return false;
//             }

//             preValue = list.get(i);
//         }
//         return true;
//     }
//     /**
//         中序遍历Tree，将结点的值 存入一个List里返回。
//      */
//     private static List<Integer> unRecurInOrder(TreeNode root){
//         if(root == null){
//             return new ArrayList<Integer>();
//         }
//         // 1 预备
//         List<Integer> list = new ArrayList<Integer>();
//         Stack<TreeNode> stack = new Stack<TreeNode>();

//         // 2 模拟
//         while( !stack.isEmpty() || root != null) {
//             if(root != null) { // root 此时表示 当前结点
//                 stack.push(root);
//                 root = root.left;
//             } else {
//                 root = stack.pop();
//                 list.add(root.val);
//                 root = root.right;
//             }
//         }

//         return list;
//     }
// }
```



## 226、翻转二叉树

[226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

```JAVA
 // 递归写法
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;

        TreeNode pre = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(pre);

        return root;
    }

}

// 非递归写法
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;

        TreeNode cur;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while(!queue.isEmpty()) {
            cur = queue.poll();
            TreeNode pre = cur.left;
            cur.left = cur.right;
            cur.right = pre;

            if(cur.left != null){
                queue.offer(cur.left);
            }
            if(cur.right != null){
                queue.offer(cur.right);
            }

        }
        return root;
    }
}
```

## 236、二叉树的最近公共祖先

[236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)



```JAVA
// 注意：p 和 q 均存在于给定的二叉树中！！！
 // 情况1：root 的左 / 右子树中都不包含 p,q
 // 情况2：p、q是同侧的
 // 情况3：p、q是异侧的
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    // 1.终止条件：① 越过叶子节点 ② 找到了目标节点
    if(root == null || root == p || root == q){ // 子树p、q谁先给找到谁就是先祖
        return root;
    }
    // 2. 递归获取信息，每一个树，都具备两个的信息
    // 信息1：要求左边返回 有没有找到
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    // 信息2：要求右边返回 有没有找到
    TreeNode right = lowestCommonAncestor(root.right, p, q);


    // 3. 已经获取到了所有信息，开始处理我们的信息，然后我们好往上一层传递我们处理的结果。
    // 情况1: p、q不在root为根的树中
    if(left == null && right == null) { // 越过叶子节点
        return null; // 说明 root 的左 / 右子树中都不包含 p,q
    }
    // 情况2: 同侧
    if(left == null && right != null){
        return right;
    }        
    if(left != null && right == null){
        return left;
    }
    // 情况3：异侧
    return root; // if(left != null && right != null)
    }
}
```



## 543: 二叉树的直径

[543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

<img src="C:\HYF\Typora\LeetCode.assets\image-20210914194040733.png" alt="image-20210914194040733" style="zoom:33%;" />

==注意==：设置一个全局变量maxD来记录任意两个节点之间的最长距离，因为中间可能子树部分就已经达到最长。如图所示，路线①和路线②的对比

```JAVA
class Solution {
    int maxD = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        if(root == null) return 0;
        // 有两层结点路径才为1
        depth(root);
        return maxD;

    }
    
    public  int depth(TreeNode root) {
        if(root == null) return 0;

        int left = depth(root.left);
        int right = depth(root.right);

        maxD = Math.max(maxD, left + right);

        return Math.max(left,right) + 1;
    }
}
```



## 337. 打家劫舍 III

[337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

结点的种类：爷爷、儿子、孙子

结点的状态：选 or 不选

容器：HashMap  ① y --> 记录 ==选择== 当前节点后，能盗取的最高金额

​							  ② n --> 记录==不选择==当前节点后，能盗取的最高金额

记，当前节点为cur；当前节点的左子节点为l；当前节点的右子节点为r

$y(cur) = cur.val + n(l) + n(r) $

$n(cur) = MAX( y(l) + n(l)) + MAX( y(r) + n(r));$



```Java
class Solution {
    // 1. 建立 y 和 n 对应的HashMap
    Map<TreeNode, Integer> y = new HashMap<TreeNode, Integer>();
    Map<TreeNode, Integer> n = new HashMap<TreeNode, Integer>();
    public int rob(TreeNode root) {
		if(root == null) return 0;
        dfs(root);
        return Math.max(y.getOrDefault(root,0),n.getOrDefault(root,0));
    }
    public void dfs(TreeNode root) {
        if(root == null) return;
        // 后序遍历，左 右 头
        dfs(root.left);
        dfs(root.right);
        // 计算当前节点两种状态的值，然后，放到对应的容器里面 + 
        y.put(root, root.val + n.getOrDefault(root.left,0) + n.getOrDefault(root.right,0));
        n.put(root,Math.max(n.getOrDefault(root.left,0), y.getOrDefault(root.left,0)) + Math.max(n.getOrDefault(root.right,0), y.getOrDefault(root.right,0)) );
    }
}
```





# stack

## 20、有效的括号

#### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

```JAVA
class Solution {
	public boolean isValid(String s) {
		// 1. 准备存储左括号的容器
		Stack<Character> stack = new Stack<>();
		// 2. 准备Map， key: 右括号，value: 左括号
		Map<Character,Character> map = new HashMap<>();
		map.put('}','{');
		map.put(']','[');
		map.put(')','(');
		
		for(int i = 0; i < s.length(); i++){
			if( !map.containsKey(s.charAt(i))) {
				// map 不包含该key，说明该key是一个左方向符号
				stack.push(s.charAt(i)); 
			} else {
				// map 包含该key，说明是一个右括号，此时判断该右括号出现的位置合不合法
				// rule 1：遇到一个右括号，之前一定要有左括号，因此存储左括号的stack不能为空
				// rule 2: 第一个遇到的右括号，与stack 的 栈顶 的 元素是左右配对的。换言之，遇到一个右括号，他马上就应该个栈顶匹配掉
				if( stack.isEmpty() || stack.pop() != map.get(s.charAt(i))){
					return false;
				}
			}
		}
		return stack.isEmpty();
	}
}
```



```JAVA
// perfect solution: 
// O(n)->time complexity 
// O(1)->space complexity
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null) {
            return true;
        }

        // 找到前半部分链表的尾节点并反转后半部分链表
        ListNode firstHalfEnd = endOfFirstHalf(head);
        ListNode secondHalfStart = reverseList(firstHalfEnd.next); //从屁股往前

        // 判断是否回文
        ListNode p1 = head;
        ListNode p2 = secondHalfStart;
        boolean result = true;
        while (result && p2 != null) {
            if (p1.val != p2.val) {
                result = false;
            }
            p1 = p1.next;
            p2 = p2.next;
        }        

        // 还原链表并返回结果
        firstHalfEnd.next = reverseList(secondHalfStart);
        return result;
    }
    /**
        完成链表的反转
     */
    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;// 改变结点结构前，先找到next结点暂存
            curr.next = prev;// 完成当前结点指向关系的反转
            prev = curr;
            curr = nextTemp; //curr 最后一次结束后变成null跳出，prev记录链表最后一个结点
        }
        return prev;
    }
    /**
    快慢指针找到链表的中点
    快指针走2步，慢指针走1步
    快指针到最后，慢指针到中间部分
     */
    private ListNode endOfFirstHalf(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
}
```



```java
// normal solution:
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null) return false;
        if(head.next == null) return true;
        int size = 0;
        ListNode  p = head; // 指针

        while(p != null){
            size++;
            p = p.next;
        }
    
        //得到链表长度，为奇数，则不是回文链表
        Stack<ListNode> stack= new Stack<>();
        for(int i = 0; i < size/2; i++){
            stack.push(head);
            head = head.next;
        }
        // head此时 是后半部分的开头位置
        if (size % 2 == 1) head = head.next;

        while(!stack.isEmpty()){
            if(stack.pop().val != head.val){
                return false;
            }
            head = head.next;
        }
        return true;
    }
}
```

## 155、最小栈

#### [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

```JAVA
class MinStack {
    
    Stack<Integer> stack = new Stack();
    Stack<Integer> minStack = new Stack();

    public MinStack() {
        minStack.push(Integer.MAX_VALUE);
    }
    
    public void push(int val) {
        // 两个栈 始终保持元素个数是一致的
        stack.push(val);
        minStack.push(Math.min(minStack.peek(), val));

    }
    
    public void pop() {
        if( stack == null) return;
        stack.pop();
        minStack.pop();
    }
    
    public int top() {
        if( stack == null) return Integer.MAX_VALUE;
        return stack.peek();
    }
    
    public int getMin() {
        if( minStack == null) return Integer.MAX_VALUE;
        return minStack.peek();
    }
}
```



## 234、回文链表

#### [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

```JAVA
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null) {
            return true;
        }

        // 找到前半部分链表的尾节点并反转后半部分链表
        ListNode firstHalfEnd = endOfFirstHalf(head);
        ListNode secondHalfStart = reverseList(firstHalfEnd.next); //从屁股往前

        // 判断是否回文
        ListNode p1 = head;
        ListNode p2 = secondHalfStart;
        boolean result = true;
        while (result && p2 != null) {
            if (p1.val != p2.val) {
                result = false;
            }
            p1 = p1.next;
            p2 = p2.next;
        }        

        // 还原链表并返回结果
        firstHalfEnd.next = reverseList(secondHalfStart);
        return result;
    }
    /**
        完成链表的反转
     */
    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;// 改变结点结构前，先找到next结点暂存
            curr.next = prev;// 完成当前结点指向关系的反转
            prev = curr;
            curr = nextTemp; //curr 最后一次结束后变成null跳出，prev记录链表最后一个结点
        }
        return prev;
    }
    /**
    快慢指针找到链表的中点
    快指针走2步，慢指针走1步
    快指针到最后，慢指针到中间部分
     */
    private ListNode endOfFirstHalf(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

