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
