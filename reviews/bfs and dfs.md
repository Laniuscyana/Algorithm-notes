# 广度优先搜索和深度优先搜索
## 广度优先搜索
### LC.515
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
        if(root==null){
            return new ArrayList<Integer>();
        }

        Queue<TreeNode> queue=new ArrayDeque<TreeNode>();
        List<Integer> res=new ArrayList<Integer>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int sz=queue.size();
            int maxval=Integer.MIN_VALUE;
            while(sz>0){
                sz--;
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
