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
