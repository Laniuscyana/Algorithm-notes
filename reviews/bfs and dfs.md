# 广度优先搜索和深度优先搜索
二叉树的定义
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```
## 广度优先搜索
### LC.104 二叉树的最大深度
> https://leetcode.cn/problems/maximum-depth-of-binary-tree/
```java
class Solution{
    public int maxDepth(TreeNode root){
        if(root==null){
            return 0;
        }

        Deque<TreeNode> queue=new LinkedList<TreeNode>();
        queue.offer(root);
        int ans=0;
        while(!queue.isEmpty()){
            int size=queue.size();
            while(size>0){
                TreeNode node=queue.poll();
                if(node.left!=null){
                    queue.offer(node.left);
                }
                if(node.right!=null){
                    queue.offer(node.right);
                }
                size--;
            }
            ans++;
        }
        return ans;

    }
}
```
### LC.515：求二叉树每一层的节点的最大值
<span id="jump"> </span>
 ```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        // BFS
        //特殊情况判断，如果二叉树为空，直接返回空集；
        if(root==null){
            return new ArrayList<Integer>();
        }
        
        //生成一个队列和一个链表，分别用于存储节点值和每层最大节点值
        Queue<TreeNode> queue=new ArrayDeque<TreeNode>();
        List<Integer> res=new ArrayList<Integer>();
        queue.offer(root);
        //每层添加节点时，把该层所有节点都添加进去
        while(!queue.isEmpty()){
            int sz=queue.size();
            int maxval=Integer.MIN_VALUE;
            while(sz>0){
                sz--;
                //由于队列是先进先出的，所以每次弹出某层最后一个节点后，才会再弹出下一层的二叉树节点
                TreeNode t=queue.poll();
                maxval=Math.max(maxval,t.val);
                if(t.left!=null){
                    queue.offer(t.left);
                }
                if(t.right!=null){
                    queue.offer(t.right);
                }
            }
            res.add(maxval);
        }
        return res;


    }
}
 ```
[深度优先搜索方法](#jump2)

## 深度优先搜索

### LC.241 为运算表达式设置优先级
 ```java
class Solution {
    char[] s;
    public List<Integer> diffWaysToCompute(String expression) {
        s=expression.toCharArray();
        return dfs(0,s.length-1);
    }

    List<Integer> dfs(int l, int r){
        //生成一个链表
        List<Integer> ans=new ArrayList<Integer>();
        for(int i=l;i<=r;i++){
            //确保找到运算符，如果是数字就停止该次循环寻找下一个字符，直到下一个字符是运算符为止
            if(s[i]>='0' && s[i]<='9'){
                continue;
            }
            //找到运算符后，用递归搜两边所有可能的组合，即该符号右边所有可能出现的加减乘结果以及左边所有可能出现的加减乘结果
            List<Integer> l1=dfs(l,i-1);
            List<Integer> l2=dfs(i+1,r);
            //以上搜完后，最后算该符号位置的加减乘结果
            for(int a:l1){
                for(int b:l2){
                    int cur=0;
                    if(s[i]=='-'){
                        cur=a-b;
                    }else if(s[i]=='+'){
                        cur=a+b;
                    }else{
                        cur=a*b;
                    }
                    ans.add(cur);
                }
            }
        }
        
        //如果全部搜完都没有发现运算符，那么加入该数字，注意当没有运算符时，要把字符转化为十进制数加入
        if(ans.isEmpty()){
            int temp=0;
            for(int i=l;i<=r;i++){
                temp=temp*10+s[i]-'0';
            }
            ans.add(temp);
        }
        return ans;
    }
}
```


### LC.515：求二叉树每一层的节点的最大值
<span id="jump2"> </span>
 ``` java
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
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        //DFS
        if(root==null){
            return new ArrayList<Integer>();
        }

        List<Integer> res=new ArrayList<Integer>();
        dfs(res,root,0);
        return res;
    }

    public void dfs(List<Integer> res,TreeNode root, int curheight){
        //递归调用自身，优先按深度加入左节点，当所有左节点被加入后，再从最底层开始加入右节点
        if(curheight==res.size()){
            //如果当前是左子树的左节点，则直接加入
            res.add(root.val);
        }else{
            //如果不是，那么比较大小后更新
            res.set(curheight,Math.max(root.val,res.get(curheight)));
        }

        if(root.left!=null){
            //如果节点的左子节点不为空，那么把左子节点加入
            dfs(res,root.left,curheight+1);
        }
        if(root.right!=null){
            //如果节点的右子节点不为空，那么把右子节点加入
            dfs(res,root.right,curheight+1);
        }
    }
}
 }
```
[广度优先搜索方法](#jump)

## LC.623 在二叉树中增加一行
> https://leetcode.cn/problems/add-one-row-to-tree/
```java
class Solution {
    public TreeNode addOneRow(TreeNode root, int val, int depth) {
        if(root==null){
            return null;
        }
        if(depth==1){
            return new TreeNode(val,root,null);
        }

        if(depth==2){
            root.left=new TreeNode(val,root.left,null);
            root.right=new TreeNode(val,null,root.right);
        }else{
            root.left=addOneRow(root.left,val,depth-1);
            root.right=addOneRow(root.right,val,depth-1);
        }

        return root;
    }
}
```
### LC.814二叉树剪枝
> https://leetcode.cn/problems/binary-tree-pruning/

 ```java
class Solution{
    public TreeNode pruneTree(TreeNode root) {
        if(root==null){
            return root;
        }
        
        root.left=pruneTree(root.left);
        root.right=pruneTree(root.right);
        
        if(root.val==0 && root.left==null && root.right==null){
            return null;
        }
        
        return root;
    }
}
```
