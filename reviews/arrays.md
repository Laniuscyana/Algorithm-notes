# 数组
## LC.134
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

## LC.324 摆动排序
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

## LC.556下一个更大元素III
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
## LC.31下一个排列
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
