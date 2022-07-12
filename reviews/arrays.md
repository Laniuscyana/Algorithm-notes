# 数组
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

## LC.496 下一个更大元素I
> https://leetcode.cn/problems/next-greater-element-i/

求解子序列在母序列中的下一个最大元素，最朴素的想法是对子序列的每一个元素都求出它在母序列的位置，然后去寻找下一个更大元素。进一步地，可以考虑这样一个解法，既然子序列的每一个元素都在母序列中出现，为了节省时间，可以先找出母序列中每一个元素的更大元素，放入哈希表中，即（母序列元素，对应的下一个更大元素），然后对子序列的每一个元素，在哈希表中找到对应的value即可。

 ```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
       Map<Integer,Integer> map=new HashMap<Integer,Integer>();
       Deque<Integer> stack=new ArrayDeque<Integer>();
       for(int i=nums2.length-1;i>=0;i--){
           int num=nums2[i];
           while(!stack.isEmpty() && num>=stack.peek()){
               stack.pop();
           }
           map.put(num,stack.isEmpty()?-1:stack.peek());
           stack.push(num);
       }

       int[] ans=new int[nums1.length];
       for(int i=0;i<nums1.length;i++){
           ans[i]=map.get(nums1[i]);
       }
       return ans;
    }
}
```

## LC.503 下一个更大元素II
> https://leetcode.cn/problems/next-greater-element-ii/

用单调栈来解决这道题，首先，由于这是一个循环数组，实际求解过程中需要将前n-1个元素衔接到原数组的最后一个元素后面；实际操作中可以不用生成而是实际循环2n-1词，但每次对i取模。

先生成一个栈，然后将第一个元素的**下标**压入栈中；此时将第二个元素与栈顶编号对应的元素进行比较，如果栈顶的编号对应的数组元素小于第二个元素，说明第二个元素正是它的下一个最大元素。弹出栈顶元素，并用ans数组记录弹出的编号，正好对应第二个元素，此时原数组的第一个元素的下一个更大元素求解完成。然后，将第二个元素的下标压入栈中，重复刚才的过程。

这里需要注意一点，在用代码表示**栈顶编号对应的元素如果小于即将入栈的元素，那么说明即将入栈的元素正是它的下一个更大元素，反之，如果栈顶编号对应的元素大于即将入栈的元素，说明即将入栈的元素不是它的下一个更大元素，此时对栈不做弹出操作，只将即将入栈的元素压入；当栈中有两个及以上的元素时，每一次都检查栈是否不空，以及栈顶元素对应的数组元素是否小于即将入栈的元素，如果两个条件都满足，那么就弹出栈顶元素，用ans数组记录，直到栈为空或者栈顶对应的数组元素大于即将入栈的元素**。

 ```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int n=nums.length;
        int[] ans=new int[n];
        Arrays.fill(ans,-1);
        Deque<Integer> stack=new ArrayDeque<Integer>();
        for(int i=0;i<2*n-1;i++){
            while(!stack.isEmpty() && nums[stack.peek()]<nums[i%n]){
                ans[stack.pop()]=nums[i%n];
           }
           stack.push(i%n);
        }
        return ans;
    }
}
```

## LC.556下一个更大元素III
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
