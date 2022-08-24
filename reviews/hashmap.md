# 哈希表

## LC. 1 两数之和
> https://leetcode.cn/problems/two-sum/
```java
class Solution{
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer,Integer> map=new HashMap<Integer,Integer>();
        for(int i=0;i<nums.length;i++){
            if(map.containsKeys(target-nums[i])){
                return new int[]{map.get(target-nums[i]),i};
            }
            map.put(nums[i],i);
        }
        return new int[0];
    }
}
```

## LC.20有效的括号
> https://leetcode.cn/problems/valid-parentheses/
```java
class Solution {
    private static final Map<Character,Character> map=new HashMap<Character,Character>(){{
        put('{','}');put('[',']');put('(',')');put('?','?');
    }};
    public boolean isValid(String s) {
        if(s.length()>0 && !map.containsKey(s.charAt(0))){
            return false;
        }
        LinkedList<Character> stack=new LinkedList<Character>(){{add('?');}};
        for(Character c:s.toCharArray()){
            if(map.containsKey(c)){
                stack.addLast(c);
            }else if(map.get(stack.removeLast())!=c){
                return false;
            }
        }
        return stack.size()==1;
    }
}
```
