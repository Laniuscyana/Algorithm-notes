# 广度优先搜索和深度优先搜索
## 广度优先搜索
### LC.515：求二叉树每一层的节点的最大值
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
