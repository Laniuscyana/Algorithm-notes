# 滑动窗口

## LC.3 无重复字符的最长子串
> https://leetcode.cn/problems/longest-substring-without-repeating-characters/
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n=s.length();
        int start=0;
        int ans=0;
        Map<Character,Integer> map=new HashMap<Character,Integer>();

        for(int end=0;end<n;end++){
            char ws=s.charAt(end);
            if(map.containsKey(ws)){
                start=Math.max(start,map.get(ws));
            }
            ans=Math.max(ans, end-start+1);
            map.put(ws,end+1);
        }
        return ans;
    }
}
```

## LC.30 串联所有单词的子串
> https://leetcode.cn/problems/substring-with-concatenation-of-all-words/
 ```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> res =new ArrayList<Integer>();
        int m=words.length;
        int n=words[0].length();
        int ls=s.length();
 
        for(int i=0;i<n;i++){
            //判断长度是否够，不够就返回空集；
            if(i+m*n>ls){
                break;
            }
            
            //每次都新生成一个哈希表，每次循环的哈希表对下一次循环的哈希表都没有信息增量，所以每次新生成一个用于维护滑动窗口；
            Map<String, Integer> differ = new HashMap<String, Integer>();
            
            //第一个滑动窗口的生成；
            for(int j=0;j<m;j++){
                String word=s.substring(i+j*n,i+(j+1)*n);
                differ.put(word,differ.getOrDefault(word, 0) + 1);
            }
            for(String word:words){
                differ.put(word,differ.getOrDefault(word,0)-1);
                if(differ.get(word)==0){
                    differ.remove(word);
                }
            }
            //第二个及以后的滑动窗口生成，注意判断条件start不等于i才进行滑动窗口的滑动；并且判断第一个窗口是否符合要求；
            for(int start=i;start<ls-m*n+1;start +=n){
                if(start!=i){
                    String word=s.substring(start+(m-1)*n,start+m*n);
                    differ.put(word,differ.getOrDefault(word,0)+1);
                    //为什么这里需要判断一下word是否等于0？因为前面的滑动窗口可能遗留了东西下来，如果滑动到这里匹配上了，就需要把这个word删掉；
                    if (differ.get(word) == 0) {
                    differ.remove(word);
                    }
 
                    word=s.substring(start-n,start);
                    differ.put(word,differ.getOrDefault(word,0)-1);
                    if (differ.get(word) == 0) {
                    differ.remove(word);
                    }
                }
                if(differ.isEmpty()){
                    res.add(start);
                }
            }
        }
        return res;
    }
}
```
  
## LC.219 存在重复元素II
> https://leetcode.cn/problems/contains-duplicate-ii/
```java

```

## LC.220 存在重复元素III
> https://leetcode.cn/problems/contains-duplicate-iii/
```java

```
