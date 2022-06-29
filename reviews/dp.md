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
## 打气球
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

## 最小路径和
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


