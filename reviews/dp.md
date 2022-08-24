# 动态规划
## 剑指offer:粉刷房子
 ```java
 class Solution {
    public int minCost(int[][] cs) {
        int n = cs.length;
        int a = cs[0][0], b = cs[0][1], c = cs[0][2];
        for (int i = 1; i < n; i++) {
            int d = Math.min(b, c) + cs[i][0];
            int e = Math.min(a, c) + cs[i][1];
            int f = Math.min(a, b) + cs[i][2];
            a = d; b = e; c = f;
        }
        return Math.min(a, Math.min(b, c));
    }
}
```
## LC.312 打气球
> https://leetcode.cn/problems/burst-balloons/
 ```java
class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;
        int[][] rec = new int[n + 2][n + 2];
        int[] val = new int[n + 2];
        val[0] = val[n + 1] = 1;
        for (int i = 1; i <= n; i++) {
            val[i] = nums[i - 1];
        }
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 2; j <= n + 1; j++) {
                for (int k = i + 1; k < j; k++) {
                //扎气球时，假设从i开始扎到j停止，最优的分数为dpij，那么实际上等价于在中间选择一个最优的k(k在i和j之间)，使得最后扎k然后再扎剩余的有最多的分数。于是，就有如下的求法，即dpij=最后扎了ijk的分数+在ik之间的分数+jk之间的分数
                    int sum = val[i] * val[k] * val[j];
                    sum += rec[i][k] + rec[k][j];
                    rec[i][j] = Math.max(rec[i][j], sum);
                }
            }
        }
        return rec[0][n + 1];
    }
}
```

## LC.64 最小路径和
> https://leetcode.cn/problems/minimum-path-sum/
 ```java
class Solution {
    public int minPathSum(int[][] grid) {
        if(grid==null || grid.length==0 || grid[0].length==0){
            return 0;
        }

        int m=grid.length;
        int n=grid[0].length;
        int[][] dp= new int[m][n];
        dp[0][0]=grid[0][0];
        for(int i=1;i<m;i++){
            dp[i][0]=dp[i-1][0]+grid[i][0];
        }

        for(int j=1;j<n;j++){
            dp[0][j]=dp[0][j-1]+grid[0][j];
        }

        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j]=Math.min(dp[i-1][j],dp[i][j-1])+grid[i][j];
            }
        }

        return dp[m-1][n-1];

    }
}
```
## 斐波那契数列
> https://leetcode.cn/problems/length-of-longest-fibonacci-subsequence/

由于要寻找的斐波那契数列与新纳入的数组元素有关，可以考虑用动态规划来做。首先考虑一个二维的状态，即用dp i j来表示*以第i位元素为最后一位且以第j位元素为倒数第二位的最长序列*，那么当arr_i-arr_j在数组的前面的元素中可以找得到的情况下，dp_ij就可以在长度3与（dp_jt +1）中取更大的了。然后开始求解这个方程，首先要明确，i从第一位元素开始遍历，但j需要紧贴着i往前，因此顺序为i从0到数组最后一位，j从i的前一位i-1开始直到0为止。

那么这个过程中还可以再优化一下，首先是如果arr_i-arr_j大于arr_j，说明即使存在一个t满足arr_t=arr_i-arr_j，根据单调性，t>j，但这与t位于j之前是矛盾的，所以直接舍去；其次，由于j是从i-1处往前遍历的，当目前搜索得到的最大长度为ans时，如果j+2已经小于等于ans，说明即使j往前、加上i本身所有的元素都可以组成符合条件的数列，它也不会比ans更长了，因此也可以舍去。代码如下。

 ```java
class Solution {
    public int lenLongestFibSubseq(int[] arr) {
        int n=arr.length;
        int ans=0;
        int[][] dp=new int[n][n];
        
        Map<Integer,Integer> map=new HashMap<Integer,Integer>();
        for(int i=0;i<n;i++){
            map.put(arr[i],i);
        }

        for(int i=0;i<n;i++){
            for(int j=i-1;j>=0 && j+2>ans;j--){
                if(arr[i]-arr[j]>=arr[j]){
                    break;
                }
                int t=map.getOrDefault(arr[i]-arr[j],-1);
                if(t==-1){
                    continue;
                }
                dp[i][j]=Math.max(3,dp[j][t]+1);
                ans=Math.max(ans,dp[i][j]);
            }
        }

        return ans;
        
    }
}
```

## LC.730
> https://leetcode.cn/problems/count-different-palindromic-subsequences/
```java

```

