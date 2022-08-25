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

## LC.217 存在重复元素
> https://leetcode.cn/problems/contains-duplicate/
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set=new HashSet<Integer>();
        for(int num:nums){
            if(!set.add(num)){
                return true;
            }
        }
        return false;
    }
}
```

## LC.535 TinyURL的解密与加密
> https://leetcode.cn/problems/encode-and-decode-tinyurl/
```java
public class Codec {
    private Map<Integer,String> dataBase=new HashMap<Integer,String>();
    private Random random=new Random();

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        int x;
        while(true){
            x=random.nextInt();
            if(!dataBase.containsKey(x)){
                break;
            }
        }
        dataBase.put(x,longUrl);
        return "http://tinyurl.com/"+x;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        int p=shortUrl.lastIndexOf('/')+1;
        int x=Integer.parseInt(shortUrl.substring(p));
        return dataBase.get(x);
    }
}

// public class Codec {
//     private Map<Integer, String> dataBase = new HashMap<Integer, String>();
//     private Random random = new Random();

//     public String encode(String longUrl) {
//         int key;
//         while (true) {
//             key = random.nextInt();
//             if (!dataBase.containsKey(key)) {
//                 break;
//             }
//         }
//         dataBase.put(key, longUrl);
//         return "http://tinyurl.com/" + key;
//     }

//     public String decode(String shortUrl) {
//         int p = shortUrl.lastIndexOf('/') + 1;
//         int key = Integer.parseInt(shortUrl.substring(p));
//         return dataBase.get(key);
//     }
// }


// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```
