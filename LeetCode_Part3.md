# Leetcode Part3 共55道
## 111 Binary Tree Level Order Traversal II
#### _easy_
#### 描述：层序遍历二叉树，要求最后的一层在最前面
#### 思路：深度优先，广度优先。
#### 代码：
#### solution1 DFS :
```
public List<List<Integer>> levelOrderBottom(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        List<List<Integer>> wrapList = new LinkedList<List<Integer>>();
        
        if(root == null) return wrapList;
        
        queue.offer(root);
        while(!queue.isEmpty()){
            int levelNum = queue.size();
            List<Integer> subList = new LinkedList<Integer>();
            for(int i=0; i<levelNum; i++) {
                if(queue.peek().left != null) queue.offer(queue.peek().left);
                if(queue.peek().right != null) queue.offer(queue.peek().right);
                subList.add(queue.poll().val);
            }
            wrapList.add(0, subList);
        }
        return wrapList;
    }
```
#### solution2 BFS ：
```
        public List<List<Integer>> levelOrderBottom(TreeNode root) {
            List<List<Integer>> wrapList = new LinkedList<List<Integer>>();
            levelMaker(wrapList, root, 0);
            return wrapList;
        }
        
        public void levelMaker(List<List<Integer>> list, TreeNode root, int level) {
            if(root == null) return;
            if(level >= list.size()) {
                list.add(0, new LinkedList<Integer>());
            }
            levelMaker(list, root.left, level+1);
            levelMaker(list, root.right, level+1);
            list.get(list.size()-level-1).add(root.val);
        }

```
## 112 Balanced Binary Tree
#### _easy_
#### 描述：查看二叉树是否是二叉平衡数
#### 思路：先求子节点的高度，通过左右子树高度来判断。
#### 代码：
```
public boolean isBalanced(TreeNode root) {
    boolean[] result = new boolean[] {true};
    helper(root, 1, result);
    return result[0];
}
public int helper(TreeNode root, int depth, boolean result[]) {
    if (root == null) {    
        return depth;
    }
    int leftDepth = helper(root.left, depth + 1, result);
    int rightDepth = helper(root.right, depth + 1, result);
    if (result[0] &&  Math.abs(rightDepth - leftDepth) > 1) {
        result[0] =  false;
    }
    return Math.max(leftDepth, rightDepth);
}
```
## 113 Minimum Depth of Binary Tree
#### _easy_
#### 描述：给定一个二叉树，返回离叶子节点的最小距离
#### 思路：用递归，值得注意的是，如果一个节点左或者右子树为空，这种情况的多加考虑。下面代码比较巧妙。
#### 代码：
```
public int minDepth(TreeNode root) {
        if(root == null) return 0;
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        return (left == 0 || right == 0) ? left + right + 1: Math.min(left,right) + 1;
       
    }
```
## 114 Path Sum
#### _easy_
#### 描述：给定一个二叉树，和一个整数值。问是否存在从根节点到叶子节点的路径和为该整数值
#### 思路：依然递归。
#### 代码：
```
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root==null)
            return false;
        if(sum==root.val && root.left==null && root.right==null)
            return true;
        return hasPathSum(root.left, sum-root.val) || hasPathSum(root.right, sum-root.val);
    }
```
## 115 Symmetric Tree
#### _easy_
#### 描述：判断二叉树是否对称
#### 思路：详细见代码。
#### 代码：
#### solution1 递归
```
public boolean isSymmetric(TreeNode root) {
    return root==null || isSymmetricHelp(root.left, root.right);
}

private boolean isSymmetricHelp(TreeNode left, TreeNode right){
    if(left==null || right==null)
        return left==right;
    if(left.val!=right.val)
        return false;
    return isSymmetricHelp(left.left, right.right) && isSymmetricHelp(left.right, right.left);
}
```
#### solution2 非递归
```
public boolean isSymmetric(TreeNode root) {
    if(root==null)  return true;
    
    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode left, right;
    if(root.left!=null){
        if(root.right==null) return false;
        stack.push(root.left);
        stack.push(root.right);
    }
    else if(root.right!=null){
        return false;
    }
        
    while(!stack.empty()){
        if(stack.size()%2!=0)   return false;
        right = stack.pop();
        left = stack.pop();
        if(right.val!=left.val) return false;
        
        if(left.left!=null){
            if(right.right==null)   return false;
            stack.push(left.left);
            stack.push(right.right);
        }
        else if(right.right!=null){
            return false;
        }
            
        if(left.right!=null){
            if(right.left==null)   return false;
            stack.push(left.right);
            stack.push(right.left);
        }
        else if(right.left!=null){
            return false;
        }
    }
    
    return true;
}
```
## 116 Maximum Depth of Binary Tree
#### _easy_
#### 描述：返回二叉树的最大深度。
#### 思路：最小深度的变种，最后一个return的逻辑很巧妙。
#### 代码：
```
public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        return (left == 0 || right == 0) ?left + right+1:Math.max(left,right)+1;
    }
```
## 117 Convert Sorted Array to Binary Search Tree
#### _easy_
#### 描述：给定一个增序数组，返回一个二叉平衡查找数
#### 思路：递归即可。
#### 代码：
```
public TreeNode sortedArrayToBST(int[] nums) {
        return formBST(nums, 0, nums.length-1);
    }
    
    public TreeNode formBST(int[] nums, int l, int r){
        if(l>r) return null;
        int mid = (l+r+1)/2;
        TreeNode node = new TreeNode(nums[mid]);
        if(l==r) return node;
        node.left = formBST(nums,l,mid-1);
        node.right = formBST(nums,mid+1,r);
        return node;
    }
```
## 118 Same Tree
#### _easy_
#### 描述：判断两个树是否相等
#### 思路：利用递归。感觉树的题都是可以利用递归来做的。
#### 代码：
```
if(p == null || q == null)
            return p == q;
        if(p.val != q.val)
            return false;
        return isSameTree(p.left,q.left) &&isSameTree(p.right,q.right);
```
## 119 Binary Tree Zigzag Level Order Traversal
#### _medium_
#### 描述：给定一个二叉树，返回层次遍历，不过按照Z字形遍历。
#### 思路：递归实现。利用一个%2。实现该功能。
#### 代码：
```
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList();
        helper(res,root,0);
        return res;
    }
    public void helper(List<List<Integer>> res,TreeNode root,int level){
        if(root == null)
            return;
        if(res.size() <= level)
            res.add(new LinkedList<Integer>());
        if(level % 2 == 0)
            res.get(level).add(root.val);
        
        else
            res.get(level).add(0,root.val);

        helper(res,root.left,level+1);
            helper(res,root.right,level+1);
    }
```
## 120 Construct Binary Tree from Inorder and Postorder Traversal
#### _medium_
#### 描述：给定一个树的中序和后序遍历，求这个树
#### 思路：利用递归，详细见代码。
#### 代码：
```
public TreeNode buildTree(int[] inorder, int[] postorder) {
        TreeNode nowNode = buildChildTree(postorder,postorder.length -1,inorder,0,postorder.length -1);
        return nowNode;
    }
    public TreeNode buildChildTree(int[] postorder,int postIndex,int[] inorder,int start,int end){
        TreeNode nowNode = null;
        int flag = -1;
        for(int i = start;i <= end;i++){
            if(postorder[postIndex] == inorder[i])
                flag = i;
        }
        if(flag >= 0){
            nowNode = new TreeNode(inorder[flag]);
            nowNode.left = buildChildTree(postorder,postIndex+flag-end-1,inorder,start,flag -1);
            nowNode.right = buildChildTree(postorder,postIndex-1,inorder,flag + 1,end);
        }
        return nowNode;
    }
```
## 121 Path Sum II
#### _medium_
#### 描述：给定一个二叉树和一指定的数sum。求出所以从根节点到叶子节点路径和为sum的路径。
#### 思路：递归调用，回溯法。有两个值得注意的地方，第一必须要到达根节点，才能结束（因为有负数的存在所以）。第二要复制list，采用new ArrayList<>(list)方法。
#### 代码：
```
public List<List<Integer>> pathSum(TreeNode root, int sum) {
	List<List<Integer>> res = new ArrayList<>();
	List<Integer> list = new ArrayList<>();
	helper(res, list, root, sum);
	return res;
}
private void helper(List<List<Integer>> res, List<Integer> list, TreeNode root, int sum) {
	if (root == null) return;
	list.add(root.val);
	if (root.left == null && root.right == null && root.val == sum) {
		res.add(new ArrayList<>(list));
	}
	helper(res, list, root.left, sum - root.val);
	helper(res, list, root.right, sum - root.val);
	list.remove(list.size() - 1);
}
```
## 122 Populating Next Right Pointers in Each Node
#### _medium_
#### 描述：给定一个二叉树，节点中多了一个指针，指向层次遍历中的后一个元素。每层的最后一个指向null
#### 思路：简单的迭代，层序遍历的变种。
#### 代码：
```
    public void connect(TreeLinkNode root) {
        while(root != null){
            TreeLinkNode tempChild = new TreeLinkNode(0);
            TreeLinkNode currentChild = tempChild;
            while(root!=null){
                if(root.left != null) { currentChild.next = root.left; currentChild = currentChild.next;}
                if(root.right != null) { currentChild.next = root.right; currentChild = currentChild.next;}
                root = root.next;
            }
            root = tempChild.next;
        }
     }
```
## 123 Unique Binary Search Trees
#### _medium_
#### 描述：给定数n，问n个节点的二分搜素数有多少种
#### 思路：动态规划，在part1里有更难的，问的是列出这些情况。
#### 代码：
```
    public int numTrees(int n) {
        int [] G = new int[n+1];
        G[0] = G[1] = 1;
    
        for(int i=2; i<=n; ++i) {
    	    for(int j=1; j<=i; ++j) {
    		G[i] += G[j-1] * G[i-j];
    	    }
        }

        return G[n];
    }
```
## 124 Sum Root to Leaf Numbers
#### _medium_
#### 描述：给定一个二叉树，返回所有从根节点到叶子节点的路径之和
#### 思路：递归。
#### 代码：
```
 public int sumNumbers(TreeNode root) {
        List<Integer> res = new LinkedList();
        helper(root,res,0);
        int ans=0;
        for(int i : res)
            ans += i;
        return ans;
    }
    public void helper(TreeNode root,List<Integer> res,int sum){
        if(root == null)
            return ;
        if(root.left == null && root.right == null)
            res.add(sum *10 + root.val);
        else{
            if(root.left != null)
                helper(root.left,res,sum*10+ root.val);
            if(root.right != null)
                helper(root.right,res,sum*10+ root.val);
        } 
    }
```
## 125 Binary Search Tree Iterator
#### _medium_
#### 描述：给定一个二叉平衡搜索树，求实现next，和hasnext方法
#### 思路：我的思路是利用中序遍历（solution2）。另一种思路是利用栈来实现（solution1）。不过我的在构造函数上耗时较多，利用栈的方法在next上耗时较多。
#### 代码：
#### solution 1
```
public class BSTIterator {
    private Stack<TreeNode> stack = new Stack<TreeNode>();
    
    public BSTIterator(TreeNode root) {
        pushAll(root);
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.isEmpty();
    }

    /** @return the next smallest number */
    public int next() {
        TreeNode tmpNode = stack.pop();
        pushAll(tmpNode.right);
        return tmpNode.val;
    }
    
    private void pushAll(TreeNode node) {
        for (; node != null; stack.push(node), node = node.left);
    }
}
```
#### solutin 2
```
public class BSTIterator {
    List<Integer> inorder;
    int nowIndex;
    public BSTIterator(TreeNode root) {
        inorder = new LinkedList();
        inorderFunction(root);
        nowIndex = 0;
    }
    public void inorderFunction(TreeNode root){
        if(root == null)
            return;
        inorderFunction(root.left);
        inorder.add(root.val);
        inorderFunction(root.right);
 
    }
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return nowIndex < inorder.size();
    }

    /** @return the next smallest number */
    public int next() {
        return inorder.get(nowIndex++);
    }
}
```
## 126 Binary Tree Right Side View
#### _medium_
#### 描述：找到每层的最后一个元素
#### 思路：一开始我以为只要从右子树开始就成，结果发现当右子树都为的左右节点都为空时，这一层的最后一个节点在左子树上面。所以还是利用层次遍历来写。最后一定要记得root为null的情况。
#### 代码：
```
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new LinkedList<Integer>();
        if(root == null)
            return res;
        LinkedList<TreeNode> queue =new LinkedList<TreeNode>();
        queue.add(root);
        while(!queue.isEmpty()){
            int num = queue.size();
            int last = queue.getLast().val;
            res.add(last);
            for(int i = 1;i <= num;i++){
                TreeNode head = queue.pop();
                if(head.left != null)queue.add(head.left);
                if(head.right != null )queue.add(head.right);
            }
        }
        return res;
    }
```
## 127 Validate Binary Search Tree
#### _medium_
#### 描述：判断一个树是否是二叉搜索树。
#### 思路：利用前序，中序遍历即可判断。我一开始打算用递归（不是遍历的递归）来写，就是先判断父节点，在判断左右节点。这个方法一点也不好。
#### 代码：
```
public boolean isValidBST (TreeNode root){
		   Stack<TreeNode> stack = new Stack<TreeNode> ();
		   TreeNode cur = root ;
		   TreeNode pre = null ;		   
		   while (!stack.isEmpty() || cur != null) {			   
			   if (cur != null) {
				   stack.push(cur);
				   cur = cur.left ;
			   } else {				   
				   TreeNode p = stack.pop() ;
				   if (pre != null && p.val <= pre.val) {					   
					   return false ;
				   }				   
				   pre = p ;					   
				   cur = p.right ;
			   }
		   }
		   return true ; 
	   }
```
## 128 House Robber III
#### _medium_
#### 描述：依然是house robber，这次是一个二叉树。
#### 思路：依然是递归公式依然没有变，dp[i]= Math.max(dp[i-1],dp[i-2]+num[i]);只不过是变成了祖父节点和父节点和当前节点。用递归可以做，但是还是用动态规划减轻复杂度。
#### 代码：
```
public int rob(TreeNode root) {
    int[] res = robSub(root);
    return Math.max(res[0], res[1]);
}

private int[] robSub(TreeNode root) {
    if (root == null) return new int[2];
    
    int[] left = robSub(root.left);
    int[] right = robSub(root.right);
    int[] res = new int[2];

    res[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
    res[1] = root.val + left[0] + right[0];
    
    return res;
}
```
## 129 Binary Tree Postorder Traversal
#### _hard_
#### 描述：后序遍历二叉树
#### 思路：详细见代码。
#### 代码：
```
private void traversePartPostorder(BinaryNode<T> rootNode,  
	Stack<BinaryNode<T>> stack) {  
	BinaryNode<T> currentNode = rootNode;  
	while (currentNode != null) {  
		if(currentNode.getLeftChild() != null){  
		//当前节点有左孩子  
			if(currentNode.getRightChild() != null){  
			//如果当前节点有右孩子优先让右孩子进入  
				stack.push(currentNode.getRightChild());  
			}  
			stack.push(currentNode.getLeftChild());  
		}else{  
			stack.push(currentNode.getRightChild());  
		}             
		currentNode = stack.peek();  
	}  
	//将最后放进去的空节点弹出  
	stack.pop();  
}  
public void postorderTraverse() {  
	Stack<BinaryNode<T>> stack = new Stack<BinaryNode<T>>();  
	BinaryNode<T> currentNode = root;  
	if(currentNode != null){  
		stack.push(currentNode);  
	}  
	while(!stack.isEmpty()){  
		if((stack.peek().getLeftChild() != currentNode) && (stack.peek().getRightChild() != currentNode)){  
			traversePartPostorder(stack.peek(),stack);  
		}  
		currentNode = stack.pop();  
		System.out.println(currentNode.getData());  
	}  
}  
```
## 130 Binary Tree Maximum Path Sum
#### _hard_
#### 描述：给定一个二叉树，求权值最大的路径。
#### 思路：这题和之前有一道题（求数组中的最大子数组的和）可能有点相同，所以我之前的思路全部往那道题走了。但是实际上是不一样的，因为数组中的数组扩张只有一种路径，而这题有四种路径。所以不行。所以正确的思路是利用分治的方法，有一个子方法求左右子树的以左右子节点为头的最大路径和，这样就可以更新最大路径长（左右子树子路径和加上根节点的值），另外要返回以根节点为头节点的最大路径和，这样就得选择左右子路径中最大的一个加上根节点的值。
#### 代码：
```
public class Solution {
    int maxValue;
    
    public int maxPathSum(TreeNode root) {
        maxValue = Integer.MIN_VALUE;
        maxPathDown(root);
        return maxValue;
    }
    
    private int maxPathDown(TreeNode node) {
        if (node == null) return 0;
        int left = Math.max(0, maxPathDown(node.left));
        int right = Math.max(0, maxPathDown(node.right));
        maxValue = Math.max(maxValue, left + right + node.val);
        return Math.max(left, right) + node.val;
    }
}
```
## 131 Recover Binary Search Tree
#### _hard_
#### 描述：在一个二叉搜索树中，有两个节点错误的交换了，求将这二叉搜索恢复正常。
#### 思路：主要利用中序遍历，找到两个位置错误的节点。我之前的思路是先打印出该二叉树的中序遍历，然后找到错误的节点位置，然后遍历二叉树交换错误的值。下面代码就很巧妙，在中序遍历二叉树的方法里顺便找到那两个错误节点，其中preElemet是二叉树中序遍历root节点前面一个节点，如果位置没有出错的话，preElement的值是小于root的值的。另外下面代码里在找到第二个节点判断方法要注意一点，不是以secondElement为空来判断的，因为中序遍历里第二个位置错误的节点是最后一个小于前面节点值的数。
#### 代码：
```
public class Solution {
    
    TreeNode firstElement = null;
    TreeNode secondElement = null;
    // The reason for this initialization is to avoid null pointer exception in the first comparison when prevElement has not been initialized
    TreeNode prevElement = new TreeNode(Integer.MIN_VALUE);
    
    public void recoverTree(TreeNode root) {
        
        // In order traversal to find the two elements
        traverse(root);
        
        // Swap the values of the two nodes
        int temp = firstElement.val;
        firstElement.val = secondElement.val;
        secondElement.val = temp;
    }
    
    private void traverse(TreeNode root) {
        
        if (root == null)
            return;
            
        traverse(root.left);
        
        // Start of "do some business", 
        // If first element has not been found, assign it to prevElement (refer to 6 in the example above)
        if (firstElement == null && prevElement.val >= root.val) {
            firstElement = prevElement;
        }
    
        // If first element is found, assign the second element to the root (refer to 2 in the example above)
        if (firstElement != null && prevElement.val >= root.val) {
            secondElement = root;
        }        
        prevElement = root;

        // End of "do some business"

        traverse(root.right);
}
```
## 132 Maximum XOR of Two Numbers in an Array
#### _medium_
#### 描述：给定一个数组，求数组内任意两个数字的最大异或值。
#### 思路：有两种方法，第一种思想是：变量mask得到每个变量的前i个值，然后tmp用于得到最新一位是否可以为1。最后查看是否有任意两个数字的前i位的异或为tmp。这样一直遍历到最小位。（solution1,这种类型的题比较少，比较难想到）。第二种是利用字典树的方法，将每一位都当做树的两个分支，然后遍历数组中的每一位元素，找到和他与或最大的数。
#### 代码：
#### solution 1
```
public class Solution {
    public int findMaximumXOR(int[] nums) {
        int max = 0, mask = 0;
        for(int i = 31; i >= 0; i--){
            mask = mask | (1 << i);
            Set<Integer> set = new HashSet<>();
            for(int num : nums){
                set.add(num & mask);
            }
            int tmp = max | (1 << i);
            for(int prefix : set){
                if(set.contains(tmp ^ prefix)) {
                    max = tmp;
                    break;
                }
            }
        }
        return max;
    }
}
```
#### solution2
```
    class Trie {
        Trie[] children;
        public Trie() {
            children = new Trie[2];
        }
    }
    
    public int findMaximumXOR(int[] nums) {
        if(nums == null || nums.length == 0) {
            return 0;
        }
        // Init Trie.
        Trie root = new Trie();
        for(int num: nums) {
            Trie curNode = root;
            for(int i = 31; i >= 0; i --) {
                int curBit = (num >>> i) & 1;
                if(curNode.children[curBit] == null) {
                    curNode.children[curBit] = new Trie();
                }
                curNode = curNode.children[curBit];
            }
        }
        int max = Integer.MIN_VALUE;
        for(int num: nums) {
            Trie curNode = root;
            int curSum = 0;
            for(int i = 31; i >= 0; i --) {
                int curBit = (num >>> i) & 1;
                if(curNode.children[curBit ^ 1] != null) {
                    curSum += (1 << i);
                    curNode = curNode.children[curBit ^ 1];
                }else {
                    curNode = curNode.children[curBit];
                }
            }
            max = Math.max(curSum, max);
        }
        return max;
    }
```
## 133 Number of Islands
#### _medium_
#### 描述：给定一个二维数组，里面全是1或者0，给出一个定义：当1的上下左右有其他1就认为这两个1在一个联通分量里，问该二维数组由多少联通分量。
#### 思路：用回溯法，碰到1就将同在一个联通分量的1全部改为0。这样就可以得到结果了。
#### 代码：
```
private int n;
private int m;

public int numIslands(char[][] grid) {
    int count = 0;
    n = grid.length;
    if (n == 0) return 0;
    m = grid[0].length;
    for (int i = 0; i < n; i++){
        for (int j = 0; j < m; j++)
            if (grid[i][j] == '1') {
                DFSMarking(grid, i, j);
                ++count;
            }
    }    
    return count;
}

private void DFSMarking(char[][] grid, int i, int j) {
    if (i < 0 || j < 0 || i >= n || j >= m || grid[i][j] != '1') return;
    grid[i][j] = '0';
    DFSMarking(grid, i + 1, j);
    DFSMarking(grid, i - 1, j);
    DFSMarking(grid, i, j + 1);
    DFSMarking(grid, i, j - 1);
}
```
## 134 Convert Sorted List to Binary Search Tree
#### _medium_
#### 描述：给定一个排好序的链表，根据这个链表构建一个平衡二叉搜索树
#### 思路：因为是排好序的链表，所以中间节点就是树的根节点。然后根据这个方法依次处理链表的左半部，和右半边。（求链表的中间节点用fast，slow方法）
#### 代码：
```
public TreeNode sortedListToBST(ListNode head) {
    if(head==null) return null;
    return toBST(head,null);
}
public TreeNode toBST(ListNode head, ListNode tail){
    ListNode slow = head;
    ListNode fast = head;
    if(head==tail) return null;
    
    while(fast!=tail&&fast.next!=tail){
        fast = fast.next.next;
        slow = slow.next;
    }
    TreeNode thead = new TreeNode(slow.val);
    thead.left = toBST(head,slow);
    thead.right = toBST(slow.next,tail);
    return thead;
}
```
## 135 Surrounded Regions
#### _medium_
#### 描述：给定一个二维字符数组，里面元素有X和O组成。求按照以下要求来修改数组：当数组里的O组成的块被X包围时，将这个块里的O改为X。当O元素位置为边界时，不算被包围。
#### 思路：回溯法，从数组边界的O元素开始遍历，将从边界O开始的块全部改成* 符号，最后再次遍历全部元素，将O改成X，* 改成O。我想法是遍历数组的元素的O元素，结果发现不好做，还是从边界的O元素开始遍历好一点。
#### 代码：
```
    int [] dirX={-1,0,1,0};
    int [] dirY={0,-1,0,1};
    public void solve(char[][] board) {
        if (board == null || board.length ==0 || board[0].length==0) return;
         int m = board.length;
         int n = board[0].length;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 || i == m-1 || j == 0 || j == n-1) {
                    if (board[i][j] == 'O') {
                       this.explore(i,j,m,n,board);
                    }
                }
            }
        }

       for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                }
                else if (board[i][j] == '*'){              
                    board[i][j] = 'O';
                }
            }
        }   
    }
    public void explore(int x, int y, int row, int col ,char[][] grid){
        if(!shouldExplore(x,y,row,col,grid)){
            return;   
        }
        grid[x][y]='*';
           for(int i=0;i<4;i++){
               this.explore(x+ dirX[i],y+dirY[i], row, col,grid);
           }
    }
    public boolean shouldExplore(int x, int y, int row, int col,char[][] grid){
      if(x>=0 && x<row && y>=0 && y<col && grid[x][y]=='O'){          
                return true;
            }
            return false;
        }  
```
## 136 Clone Graph
#### _medium_
#### 描述：给定一个图结构，求该图的复制
#### 思路：利用一个hashMap来保存该图的新节点，结果回溯法逐个复制图的结构。
#### 代码：
```
    public Map<Integer, UndirectedGraphNode> hashMap = new HashMap<Integer, UndirectedGraphNode>();
    
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if (node == null) return null;
        
        UndirectedGraphNode clone = new UndirectedGraphNode(node.label);
        hashMap.put(node.label, clone);
        for (UndirectedGraphNode neighbor: node.neighbors) {
            UndirectedGraphNode neighborClone = hashMap.get(neighbor.label);
            clone.neighbors.add(neighborClone == null ? cloneGraph(neighbor) : neighborClone);
        }
        return clone;
    }
```
## 137 Majority Element II
#### _medium_
#### 描述：给定一个长度为n的数组，求数组内重复次数大于n/3的数。
#### 思路：和求重复次数大于 n/2的方法类似，设置两个变量，来存储可能重复次数大于n/3的数。如果不等于就将次数减一，如果为零就替换变量。详细见代码。
#### 代码：
```
def majorityElement(self, nums):
    if not nums:
        return []
    count1, count2, candidate1, candidate2 = 0, 0, 0, 1
    for n in nums:
        if n == candidate1:
            count1 += 1
        elif n == candidate2:
            count2 += 1
        elif count1 == 0:
            candidate1, count1 = n, 1
        elif count2 == 0:
            candidate2, count2 = n, 1
        else:
            count1, count2 = count1 - 1, count2 - 1
    return [n for n in (candidate1, candidate2)
                    if nums.count(n) > len(nums) // 3]
```
## 138 Minimum Size Subarray Sum
#### _medium_
#### 描述：给定一个数组和一个正整数sum，问数组内大于等于sum的连续子数组的最短长度是多少？
#### 思路：利用两个点表示子数组的左边和右边，右边每次更新一个数，左边界往右边走，同时更新最短长度。
#### 代码：
```
public int minSubArrayLen(int s, int[] a) {
  if (a == null || a.length == 0)
    return 0;
  
  int i = 0, j = 0, sum = 0, min = Integer.MAX_VALUE;
  
  while (j < a.length) {
    sum += a[j++];
    
    while (sum >= s) {
      min = Math.min(min, j - i);
      sum -= a[i++];
    }
  }
  
  return min == Integer.MAX_VALUE ? 0 : min;
}
```
## 139 Remove Nth Node From End of List
#### _medium_
#### 描述：给定一个链表，删除倒数第n个节点。
#### 思路：设立两个指针，前一个先往前走n步，最后在一起走到null。这个后面走的就是倒数第n个节点。链表问题可以考虑一下用两个指针跑，用这种解法的题也不少，比如判断链表是否有环，得到链表中间节点等待。
#### 代码：
```
public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode first = head;
        ListNode second = dummy;
        while(first != null){
            if(n-- <= 0)
                second = second.next;
            first = first.next;    
        }
        second.next = second.next.next;
        return dummy.next;
    }
```
## 140 Partition List
#### _medium_
#### 描述：给定一个链表和一个指定值n，将链表小于n的放在左侧，大于等于的放在右侧，但是不能改变链表的相对顺序
#### 思路：用两个链表来存储小于和大于等于的。不过最重要的点是两个链表可以一开始创建一个头结点，这样会少好多代码，更加简洁。
#### 代码：
```
ListNode dummy1 = new ListNode(0), dummy2 = new ListNode(0);  //dummy heads of the 1st and 2nd queues
    ListNode curr1 = dummy1, curr2 = dummy2;      //current tails of the two queues;
    while (head!=null){
        if (head.val<x) {
            curr1.next = head;
            curr1 = head;
        }else {
            curr2.next = head;
            curr2 = head;
        }
        head = head.next;
    }
    curr2.next = null;          //important! avoid cycle in linked list. otherwise u will get TLE.
    curr1.next = dummy2.next;
    return dummy1.next;
```
## 141 Linked List Cycle
#### _easy_
#### 描述：给定一个链表，问是否存在一个环
#### 思路：用两个指针，一个走两步，一个走一步。如果存在环，则两个指针就有存在相等的可能。下面代码我写的，我觉得有一部分写的不错。
#### 代码：
```
    public boolean hasCycle(ListNode head) {
        if(head == null)
            return false;
        ListNode fast = head;
        ListNode slow = head;
        do{
            if(fast == null || (fast = fast.next) == null )
                break;
            fast = fast.next;
            slow = slow.next;
        }while(fast != slow);
        return fast == slow;
    }
```
## 142 Linked List Cycle II
#### _medium_
#### 描述：给定一个链表，如果这个链表存在环，则返回环的起始地方，没有环则返回null。
#### 思路：利用上一题的算法，得到相遇的节点，然后两个指针，一个是相遇的节点，一个是head，两个节点往后移动。相等则表明是起始地方。
#### 代码：
```
    public ListNode detectCycle(ListNode head) {
        ListNode meet = meetNode(head);
        if(meet == null)
            return null;
        while(meet != head){
            meet = meet.next;
            head = head.next;
        }
        return meet;
    }
    public ListNode meetNode(ListNode head){
        ListNode fast = head;
        ListNode slow = head;
        do{
            if(fast == null || (fast = fast.next) == null)
                break;
            fast = fast.next;
            slow = slow.next;
        }while(fast != slow);
        return fast;
    }
```
## 143 Rotate List
#### _medium_
#### 描述：给定一个链表和一个给定的数n，n表示依n次将末尾的节点头插。
#### 思路：找到倒数n % length个节点，让这个节点作为头结点。要注意的是先得求得链表长度length，像我之前是偷懒没有写，直接用n--来做，结果超时了。。。
#### 代码：
```
    public ListNode rotateRight(ListNode head, int k) {
        if(k == 0 ||head == null||head.next == null)
            return head;
        ListNode fast = head;
        ListNode slow = head;
        ListNode temp = head;
        int length= 0;
        while(temp != null){
            temp = temp.next;
            length ++;
        }
        k = k % length;
        while(k > 0){
            fast = fast.next;
            k--;
        }
        if(fast == head)
            return head;
        while(fast.next != null){
            fast = fast.next;
            slow = slow.next;
        }
        fast.next = head;
        head = slow.next;
        slow.next = null;
        
        return head;
    }
```
## 144 IP to CIDR
#### _easy_  （个人认为是hard难度）
#### 描述：给定一个ip值，和固定值n，返回一连串ip和子网掩码，这些结果代表n个ip地址（从给定ip地址算起）
#### 思路：整体思路是先找到从右边数第一个为一的数，这样加上遮掩到这位的子网掩码的网络就能从当前ip算起到这位能组成最大的ip个数。同时找到之后还得改变着数，从而找到下一个右边为一的数。和人为的做法类似。但是就是难写难想。记得其中的几个技巧，X& -X得到右边起第一个1（比如x为1000000100 x & -x 为0000000100。）
#### 代码：
```
class Solution {  
    public  List<String> ipToCIDR(java.lang.String ip, int range) {  
        long x = 0;  
        //获得一个ip地址每一部分  
        String[] ips = ip.split("\\.");  
        //将整ip地址看为一个整体，求出整体的int表示  
        for (int i = 0; i < ips.length; ++i) {  
            x = Integer.parseInt(ips[i]) + x * 256;  
        }  
        List<String> ans = new ArrayList<>();  
        while (range > 0) {  
            //求出二进制表示下的最低有效位的位数能表示的地址的数量  
            //如果为奇数，则=1，即以原单个起始ip地址为第一块  
            //如果为偶数，则二进制表示下的最低有效位的位数能表示的地址的数量  
            long step = x & -x;  
            //如果大于range，则需要缩小范围  
            while (step > range) step /= 2;  
            //不大于需要的range，开始处理  
            //求出现在能表示的step个地址的地址块  
            ans.add(longToIP(x, (int)step));  
            //x加上以求出的地址块  
            x += step;  
            //range减去以表示的地址块  
            range -= step;  
        }//直到range<0  
        return ans;  
    }  
    static String longToIP(long x, int step) {  
        int[] ans = new int[4];  
        //&255操作求出后8位十进制表示  
        ans[0] = (int) (x & 255);  
        //右移8位，即求下一个块  
        x >>= 8;  
        ans[1] = (int) (x & 255);  
        x >>= 8;  
        ans[2] = (int) (x & 255);  
        x >>= 8;  
        ans[3] = (int) x;  
        int len = 33;  
        //每一位就可以表示2个  
        while (step > 0) {  
            len --;  
            step /= 2;  
        }  
        return ans[3] + "." + ans[2] + "." + ans[1] + "." + ans[0] + "/" + len;  
    }  
} 
```
## 145 Permutations
#### _medium_
#### 描述：给定一个数组，数组内每个元素都不一样，数组内元素组成的排列有多少种？
#### 思路：回溯法，递归方法里通过和第一个元素交换，避免取得相同元素。我一开始的想法是将数组转换成list这样就能比较方便的去除已取得的元素。
#### 代码：
```
    public  List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> list = new LinkedList();
	    if(nums!=null&&nums.length>0) {
		    PermutationHelper(nums, 0, list);
	    }
	    return list;
    }

    private  void PermutationHelper(int[] nums, int i, List<List<Integer>> list) {
	    if(i==nums.length-1) {
		    List<Integer> l=new ArrayList<>();
		    for (int k : nums) {
			    l.add(k);
		    }
		    list.add(l);
	    }else {
		    for(int j=i;j<nums.length;j++) {
			    swap(nums, i, j);
			    PermutationHelper(nums, i+1, list);
		        swap(nums, i, j);
            }
        }
    }
    public  void swap(int array[],int i,int j) {
	    int temp=array[j];
	    array[j]=array[i];
	    array[i]=temp;
    }
```
## 146 Combinations
#### _medium_
#### 描述：给定一个n和k，求1到n中有多少种长度为k的组合。
#### 思路：回溯法做，我之前用迭代结果出错了。以为是先连续取k-1个数，再顺序去最后一个数，结果发现前k-1个可以不连续。。。真菜
#### 代码：
```
 public static List<List<Integer>> combine(int n, int k) {
		List<List<Integer>> combs = new ArrayList<List<Integer>>();
		combine(combs, new ArrayList<Integer>(), 1, n, k);
		return combs;
	}
	public static void combine(List<List<Integer>> combs, List<Integer> comb, int start, int n, int k) {
		if(k==0) {
			combs.add(new ArrayList<Integer>(comb));
			return;
		}
		for(int i=start;i<=n;i++) {
			comb.add(i);
			combine(combs, comb, i+1, n, k-1);
			comb.remove(comb.size()-1);
		}
	}
```
## 147 Gray Code
#### _medium_
#### 描述：给定一个n，返回一个list，其中的元素最高位为n，以0为起点，每个元素的二进制之和之前的元素的二进制只有一位的不同。该list包含所有的数（2^n个）。比如n=2，list等于00,01,11,10（0,1,3,2）或者等于00,10,11,01（0,2,3,1）。
#### 思路：有两种算法，一个是迭代，第i位的元素为：i ^ i >> 2.另一个是回溯法，具体的看solution2，我现在也讲不清。之前我以为很简单就是从零开始，先从第零开始，每一位的数变成1，然后在从零开始每一位的数变成0.结果发现少了好多种变化。记住 << 优先级小于 -。还有一种方法就是这些list(n)，前一半和后一半的区别是，1）最高位是0和1，2）最高位之后的就是倒序
#### 代码：
#### solution1:
```
public List<Integer> grayCode(int n) {
        List<Integer> result = new LinkedList<>();
        for (int i = 0; i < 1<<n; i++) result.add(i ^ i>>1);
        return result;
    }
```
#### solution2:
```
int nums = 0;
    public List<Integer> grayCode(int n) {
        List<Integer> ret = new ArrayList<>();
        backTrack(n, ret);
        return ret;
    }
    
    private void backTrack(int n, List<Integer> ret) {
        if (n == 0) {
            ret.add(nums);
            return;
        }
        else {
            backTrack(n - 1, ret);
            nums ^= (1 << n - 1);
            backTrack(n - 1, ret);
        }
    }
```
## 148 Subsets II
#### _medium_
#### 描述：给定一个数组，返回这个数组的所有子数组
#### 思路：回溯法的基本运用。
#### 代码：
```
public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        Arrays.sort(nums);
        traceBack(temp,res,nums,0);
        return res;
    }
    public  void traceBack(List<Integer> temp,List<List<Integer>> res,int[] nums,int start){
        res.add(new ArrayList<>(temp));
        int i = start;
        while (i < nums.length){
            
            
            temp.add(nums[i]);
            traceBack(temp,res,nums,i+1);
            temp.remove(temp.size()-1);
            
            i++;
            while (i < nums.length  && nums[i] == nums[i-1]) i++;
        }
    }
```
## 149 Palindrome Partitioning
#### _medium_
#### 描述：给定一个字符串，返回该字符串的子串全部是回文串的组合，比如aab，则返回[a,a,b],[aa,b];
#### 思路：虽然看起来难，但是是回溯法的简单应用，首先从起始字符开始算起，得到已这个字符为起始的回文子串，然后从该子串末尾位置重新开始回溯。
#### 代码：
```
List<List<String>> resultLst;
	    ArrayList<String> currLst;
	    public List<List<String>> partition(String s) {
	        resultLst = new ArrayList<List<String>>();
	        currLst = new ArrayList<String>();
	        backTrack(s,0);
	        return resultLst;
	    }
	    public void backTrack(String s, int l){
	        if(currLst.size()>0 //the initial str could be palindrome
	            && l>=s.length()){
	                List<String> r = (ArrayList<String>) currLst.clone();
	                resultLst.add(r);
	        }
	        for(int i=l;i<s.length();i++){
	            if(isPalindrome(s,l,i)){
	                if(l==i)
	                    currLst.add(Character.toString(s.charAt(i)));
	                else
	                    currLst.add(s.substring(l,i+1));
	                backTrack(s,i+1);
	                currLst.remove(currLst.size()-1);
	            }
	        }
	    }
	    public boolean isPalindrome(String str, int l, int r){
	        if(l==r) return true;
	        while(l<r){
	            if(str.charAt(l)!=str.charAt(r)) return false;
	            l++;r--;
	        }
	        return true;
	    }
```
## 150 Permutations II
#### _medium_
#### 描述：给定一个数组，返回不同的排列的结果。比如（1，1,2）为（1,1,2），（1,2,1），（2,1,1）.
#### 思路：简单来说就是将数组排序，将使用过的数标记好，然后从未使用过的数中挑选进入备选中。可以用回溯法，详见solution1。我之前打算用交互数组元素的方法来标记已使用的元素，结果发现会导致重复结果。所以还是用一个数组作为标记的比较好。solution2利用了链表，更快速。
#### 代码：
#### solution 1
```
public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(nums==null || nums.length==0) return res;
        boolean[] used = new boolean[nums.length];
        List<Integer> list = new ArrayList<Integer>();
        Arrays.sort(nums);
        dfs(nums, used, list, res);
        return res;
    }

    public void dfs(int[] nums, boolean[] used, List<Integer> list, List<List<Integer>> res){
        if(list.size()==nums.length){
            res.add(new ArrayList<Integer>(list));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(used[i]) continue;
            if(i>0 &&nums[i-1]==nums[i] && !used[i-1]) continue;
            used[i]=true;
            list.add(nums[i]);
            dfs(nums,used,list,res);
            used[i]=false;
            list.remove(list.size()-1);
        }
    }
```
#### solution2
```
class Node{
        Node next = null;
        int val;
        Node(int val){
            this.val = val;
        }
        void add(Node nd){
            nd.next = next;
            next = nd;
        }
        void remove(){
            if (next != null){
                next = next.next;
            }
        }
    }

    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
	int n = nums.length;
        Node head = new Node(0);
        Node tmp = head;
        for (int i = 0; i < n; i++){
            tmp.add(new Node(nums[i]));
            tmp = tmp.next;
        }
        List<List<Integer>> ans = new ArrayList<>();
        dfs(head, new ArrayList<>(), ans);
        return ans;
    }
    
    void dfs(Node head, List<Integer> path, List<List<Integer>> ans){
        if (head.next == null){
            ans.add(new ArrayList<>(path));
            return;
        }
        Node tmp = head;
        while (tmp.next != null){
            Node next = tmp.next;
            if (tmp != head && tmp.val == next.val){
                tmp = next;
                continue;
            }
            tmp.remove();
            path.add(next.val);
            dfs(head, path, ans);
            path.remove(path.size() - 1);
            tmp.add(next);
            tmp = next;
        }
    }
```
## 151 Min Stack
#### _easy_
#### 描述：求一个数据结构，和栈类似，但是能求栈中最小的值
#### 思路：利用两个栈，一个栈存列表中依次最小的值。PS：值得注意的点是，要判断栈是否为空
#### 代码：
```
class MinStack {

    /** initialize your data structure here. */
    Stack<Integer> num;
    Stack<Integer> min;
    public MinStack() {
        num = new Stack<Integer>();
        min = new Stack<Integer>();
    }
    
    public void push(int x) {
        num.push(x);
        if(min.isEmpty() || x <= min.peek())
            min.push(x);
    }
    
    public void pop() {
        int out = num.pop();
        if(!min.isEmpty() && out == min.peek())
            min.pop();
    }
    
    public int top() {
        return num.peek();
    }
    
    public int getMin() {
        return min.peek();
    }
}
```
## 152 
#### _medium_
#### 描述：
#### 思路：
#### 代码：
```

```
## 153 
#### _medium_
#### 描述：
#### 思路：
#### 代码：
```

```
## 154 
#### _medium_
#### 描述：
#### 思路：
#### 代码：
```

```
## 155 
#### _medium_
#### 描述：
#### 思路：
#### 代码：
```

```
