# 其他不知道什么类型题
## LC.710
 ```java
class Solution {
    Map<Integer,Integer> b2w;
    Random random;
    int bound;

    public Solution(int n, int[] blacklist) {
        b2w=new HashMap<Integer,Integer>();
        random= new Random();
        int m=blacklist.length;
        bound=n-m;
        Set<Integer> black=new HashSet<Integer>();
        for(int b:blacklist){
            if(b>=bound){
                black.add(b);
            }
        }

        int w=bound;
        for(int b:blacklist){
            if(b<bound){
                while(black.contains(w)){
                    //这里是为了保证w绝对不在黑名单内，注意到取值的时候，是大于等于bound的都进去了，所以black这个哈希集里是有可能有bound存在的，那么就需要把w往后挪到直到w不在这个黑名单里，才能使得最后映射的时候准确映射到黑名单之外的东西；
                    ++w;
                }
                b2w.put(b,w);
                ++w;
            }
        }
    }
    
    public int pick() {
        int x=random.nextInt(bound);
        //注意这个getOrDefault!这是为了使最后取值取到被映射的那个不在黑名单上的词
        return b2w.getOrDefault(x,x);

    }
}
```

## LC.871(贪心加优先队列（堆）)
 ```java
class Solution {
    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        //生成一个优先队列，指定优先弹出队列中最大值
        PriorityQueue<Integer> q=new PriorityQueue<Integer>((a,b)->b-a);
        //以n代表stations的长度，有n个加油站
        int n=stations.length;
        //以remain表示剩余油量
        int remain=startFuel;
        //以loc表示目前行使的位置
        int loc=0;
        //以idx表示去到的加油站
        int idx=0;
        //以ans表示加油的次数，即最后的返回值
        int ans=0;
        while(loc<target){
            //判断remain是否为零？如果为零，说明上一次循环中成功进行，且清空了油量，那么执行以下命令
            if(remain==0){
                //判断队列是否不为空？如果不为空，说明仍然可以进行循环，没有走遍所有的情况
                if(!q.isEmpty() && ++ans>=0){
                    remain +=q.poll();
                }else{
                    //如果以上条件没有满足，说明不存在符合要求的加油站，返回-1;
                    return -1;
                }
            }
            
            //可以行驶的最大路程就是油箱剩余，所以令loc等于remain，然后清空remain
            loc +=remain;
            remain=0;
            //每次，当idx没有遍历到最后一个加油站，且加油站都在能到达的最大范围内时，将沿途所有的加油站的油量放入优先队列，直到idx超出n或者加油站所在位置超出汽车油量所能到达的最大位置
            while(idx<n && stations[idx][0]<=loc){
                q.add(stations[idx++][1]);
            }
        }
        return ans;
    }
}
```
