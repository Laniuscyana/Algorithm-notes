# 数组

## LC.31下一个排列
> https://leetcode.cn/problems/next-permutation/
 ```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n=nums.length;
        int i=n-2;
        while(i>=0 && nums[i]>=nums[i+1]){
            i--;
        }

        if(i>=0){
            int j=n-1;
            while(j>=0 && nums[i]>=nums[j]){
                j--;
            }
            swtch(nums,i,j);
        }
        reverse(nums,i+1);

    }
    
    public void reverse(int[] nums,int start){
        int i=start,j=nums.length-1;
        while(i<j){
            swtch(nums,i,j);
            i++;
            j--;
        }
    }

    public void swtch(int[] nums, int i,int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }
}
```

### 拓展：LC.556下一个更大元素III
> https://leetcode.cn/problems/next-greater-element-iii/

 ```java
class Solution {
    public int nextGreaterElement(int n) {
        char[] nums=Integer.toString(n).toCharArray();
        int i=nums.length-2;
        //从最低位开始，寻找最小的满足该位置上的数字小于后一位数字的，假如是严格递减或者半递减的序列，实际上就无法满足题目要求
        //注意到这里选择从倒数第二开始进行推导，得到的i正好就是需要和下面得到的j进行交换的数字，这可以把i的起始位置改成最后一位开始，但这样其实得到的是需要置换的后面一位，下面和j的交换也需要做相同的处理
        //当序列是严格或半递减时，i会等于-1
        //这一次的筛选，实际上保证了i+1之后是严格的或者半递减的序列，对后面的求解是非常关键的
        while(i>=0 && nums[i]>=nums[i+1]){
            i--;
        }
        if(i<0){
            return -1;
        }
        
        //寻找与i交换位置的数字，这一次需要从最后一位开始找起，为了保证得到的数比原数字大，必须满足条件是在j位置上的数字需要大于在i位置上的数字，因此循环条件是当i上的数字大于j位置上的数字就将j往左移动一位，直到j位置上的数字大于i位置上的数字
        int j=nums.length-1;
        while(j>=0 && nums[i]>=nums[j]){
            j--;
        }
        //当寻找到满足条件的j后，对i和j位置上的数字进行翻转
        //非常关键的一点是，在这个过程中，保证了j右边的所有数字都是严格或者与i位置上的数字相等的，而且由第一步i的产生可知，从j+1开始的序列是从i+1开始的子序列，因此也是严格递减或者半递减的序列
        swtch(nums,i,j);
        //在进行了i和j位置上的交换以后，由于现在j位置上的数字变成了原本i位置上的数字，而由于i和j的选取保证了i位置和j位置的大小关系，以及j+1开始到结尾的序列是严格递减或者半递减的，因此置换之后，从j的位置开始，仍然是递减的序列，进行翻转后得到的是该排列中最小的数字
        reverse(nums,i+1);
        long ans=Long.parseLong(new String(nums));
        return ans>Integer.MAX_VALUE?-1:(int)ans;
    }

    public void reverse(char[] nums,int start){
        int i=start;
        int j=nums.length-1;
        while(i<j){
            swtch(nums,i,j);
            i++;
            j--;
        }
    }

    public void swtch(char[] nums,int i,int j){
        char temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }
}
```
## LC.41 缺失的第一个正数
> https://leetcode.cn/problems/first-missing-positive/
```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        if(nums.length==0 || nums==null){
            return 0;
        }

        for(int i=0;i<nums.length;i++){
            while(nums[i]>0 && nums[i]<=nums.length && nums[nums[i]-1]!=nums[i]){
                swap(nums,i, nums[i]-1);
            }
        }
        
        for(int i=0;i<nums.length;i++){
            if(nums[i]!=i+1){
                return i+1;
            }
        }

        return nums.length+1;
    }

    public void swap(int[] nums,int i, int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }
}
```

## LC.53最大子数组和
> https://leetcode.cn/problems/maximum-subarray/

更新prev时的重要设定，在当前数加上过去的和与当前数之间做选择，这种做法使得更新时遇到前面的加起来全都更小时可以直接从当前位置开始

 ```java
class Solution {
    public int maxSubArray(int[] nums) {
        int ans=nums[0];
        int prev=0;
        for(int i=0;i<nums.length;i++){
            prev=Math.max(nums[i]+prev,prev);
            ans=Math.max(ans,prev);
        }
        return ans;
    }
}
```

## LC.55 跳跃游戏
> https://leetcode.cn/problems/jump-game/

```java
class Solution {
    public boolean canJump(int[] nums) {
        //设定一个最远能够达到的距离的下标max，每次跳跃后维护它
        int max=0;
        //对每个下标，先判断上一个跳跃后，是否能达到现在的下标，如果可以，那么就计算在当前下标位置最远能达到的地方，如果不能达到，返回false
        for(int i=0;i<nums.length;i++){
            if(i>max){
                return false;
            }
            max=Math.max(max,i+nums[i]);
        }
        return true;
    }
}
```

### 拓展：LC.45 跳跃游戏II
> https://leetcode.cn/problems/jump-game-ii/
```java
class Solution {
    public int jump(int[] nums) {
        if(nums ==null || nums.length<2){
            return 0;
        }

        int res=0;
        int curmax=0;
        int maxnext=0;
        //每次在上次能跳到的范围end内选择一个能跳的最远的位置,也就是能跳到max_far位置的点作为下次的起跳点
        for(int i=0;i<nums.length-1;i++){
            maxnext=Math.max(maxnext,i+nums[i]);
            if(i==curmax){
                res++;
                curmax=maxnext;
            }
        }
        return res;

    }
}
```



## LC.134 加油站
> https://leetcode.cn/problems/gas-station/

注意题目的要求是从数组中任意一个位置出发，判断能否回到原来的位置，因此起点是可以自由选取的。因此想到一次遍历的方法，首先从一个点出发，记录已有的石油和花费的石油数量之差，然后遍历整个数组，如果中途出现了过不去的情况就返回，重新在下一个位置开始。但这样其实是臃肿的，实际上有更简单的做法：不管从哪里开始走，如果整个循环一次之后用掉的石油大于加油站能提供的石油那么可以直接返回false。

因此，考虑一个更简单的想法，用一个整数total储存全程的加油站加油量和全程的消耗油量的差值，再用另一个整数start代表加油站的起始，即需要返回的答案。为了求得能够出发的点，也为了判断能不能选取该点出发，再用一个整数sum来储存当前的加油站已有油量与路程消耗油量之差。

具体的做法是，先初始化以上提到的三个变量，全部设定初始值为零。然后，从加油站的第0个加油站开始，就开始计算这一段的油量剩余值，这个过程是独立的，不管剩余油量是正是负，会从头遍历到尾。接着，判断sum的值是否小于零，（第一次循环可以跳过）如果小于零，那么说明上一次循环中，剩余油量不足以支持到达目前的加油站，因此重置sum为当前的加油站出发后到下一个加油站的剩余量；如果sum大于零，说明可以从上一个加油站顺利开车过来，因此给sum加上从这个加油站出去后的剩余油量。完成这一步后，周而复始。

遍历整个数组后，判断total是否大于零，如果total大于0，说明油量足以支持环绕一周，直接返回循环中计算得到的start，反之返回false。
 ```java
 class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int n=gas.length;
        //设置三个变量，total用于判断是否能走完全程，sum用于判断能否走当前路程，start用于记录此时的起点站
        int total=0;
        int sum=0;
        int start=0;

        for(int i=0;i<n;i++){
            //首先不管能不能从i开始，都记录全程已有和花费的石油数量
            total +=(gas[i]-cost[i]);
            //至少一次循环后，判断能否从上一次的位置开始走，如果sum小于零说明不能从上一次的位置开始走，重置开始位置，为i
            if(sum<0){
                sum= gas[i]-cost[i];
                start=i;
            }else{
            //如果sum大于零，那么将sum加上这次路程后的剩余石油量，注意sum此处仅仅用于能不能继续走
                sum +=(gas[i]-cost[i]);
            }

        }
        return total<0 ? -1 : start;

    }
}
```



## LC.229 多数元素II
> https://leetcode.cn/problems/majority-element-ii/
```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        int can1=0;
        int can2=0;
        int count1=0;
        int count2=0;

        List<Integer> ans=new ArrayList<Integer>();

        for(int num:nums){
            if(count1>0 && can1==num){
                count1++;
            }else if(count2>0 && can2==num){
                count2++;
            }else if(count1==0){
                can1=num;
                count1++;
            }else if(count2==0){
                can2=num;
                count2++; 
            }else if(num!=can1 && num!=can2){
                count1--;
                count2--;
            }
        }

        count1=0;
        count2=0;

        for(int num:nums){
            if(num==can1){
                count1++;
            }else if(num==can2){
                count2++;
            }
        }
        if(count1>nums.length/3){
            ans.add(can1);
        }
        if(count2>nums.length/3){
            ans.add(can2);
        }
        return ans;
    }
}
```

## LC.324 摆动排序II
> https://leetcode.cn/problems/wiggle-sort-ii/

摆动排序，非常朴素的一个想法是，将原数组排序后，逢偶数选前半部分，逢奇数选后半部分。

 ```java
class Solution {
    public void wiggleSort(int[] nums) {
        int[] clone=nums.clone();
        Arrays.sort(clone);
        int left=(nums.length-1)/2, right=nums.length-1;

        for(int i=0;i<nums.length;i++){
            if(i%2==0){
                nums[i]=clone[left--];
            }else{
                nums[i]=clone[right--];
            }
        }

    }
}
```

那么是否有其他复杂度更低、且占用空间更小的解法？

### LC.565 数组嵌套

解题思路：由于本题中原始数组各不相同且都在0,n-1之间，所以可以考虑直接模拟。
注意到题目的要求是当S数组中出现重复时停止添加元素，那么在该题中，为了防止出现重复元素，可以将访问过的元素重制为范围之外的任意数字，并设置为循环停止的条件。

 ```java
class Solution {
    public int arrayNesting(int[] nums) {
        int ans=0;
        int n=nums.length;
        for(int i=0;i<n;i++){
            int cnt=0;
            while(nums[i]<n){
                int num=nums[i];
                nums[i]=n;
                i=num;
                cnt++;
            }
            ans=Math.max(cnt,ans);
        }
        return ans;
    }
}
```

## LC.1200 绝对最小差
 ```java
class Solution {
    public List<List<Integer>> minimumAbsDifference(int[] arr) {
        Arrays.sort(arr);
        int delta=Integer.MAX_VALUE;

        List<List<Integer>> ans=new ArrayList<List<Integer>>();
        for(int i=0;i<arr.length-1;i++){
            int dif=arr[i+1]-arr[i];
            if(dif<delta){
                delta=dif;
                ans.clear();
                List<Integer> par=new ArrayList<Integer>();
                par.add(arr[i]);
                par.add(arr[i+1]);
                ans.add(par);
            }else if(dif==delta){
                List<Integer> par=new ArrayList<Integer>();
                par.add(arr[i]);
                par.add(arr[i+1]);
                ans.add(par);
            }
        }
        return ans;

    }
}
```

## LC.1252 奇数值单元格数目
 ```java
class Solution {
    public int oddCells(int m, int n, int[][] indices) {
        int[] rows=new int[m];
        int[] cols=new int[n];
        for(int[] index:indices){
            rows[index[0]]++;
            cols[index[1]]++;
        }

        int oddx=0;
        int oddy=0;

        for(int i=0;i<m;i++){
            if((rows[i] & 1)!=0){
                oddx++;
            }
        }
        for(int j=0;j<n;j++){
            if((cols[j] & 1)!=0){
                oddy++;
            }
        }

        return oddy*(m-oddx)+oddx*(n-oddy);
    }
}

```

## LC.1403
> https://leetcode.cn/problems/minimum-subsequence-in-non-increasing-order/
```java
class Solution {
    public List<Integer> minSubsequence(int[] nums) {
        Arrays.sort(nums);
        List<Integer> ans=new ArrayList<Integer>();
        int n=nums.length;
        
        if(n==1){
            ans.add(nums[n-1]);
            return ans;
        }

        int sum=0;
        int[] fsum=new int[n+1];
        fsum[1]=nums[0];
      
        for(int i=2;i<n;i++){
            fsum[i] +=nums[i-1]+fsum[i-1];
        }
        

        for(int i=n-1;i>=0;i--){
            sum +=nums[i];
            ans.add(nums[i]);
            if(sum>fsum[i]){
                return ans;
            }
        }
        return ans;
    }
}
```
