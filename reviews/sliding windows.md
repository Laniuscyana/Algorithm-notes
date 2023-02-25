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

        for (int end = 0;end < n;end++) {
            char ws = s.charAt(end);
            if (map.containsKey(ws)) {
                start = Math.max(start, map.get(ws));
            }
            ans = Math.max(ans, end-start+1);
            map.put(ws, end+1);
        }
        return ans;
    }
}
```

### 拓展：微软面试题
题目描述：给定字符串s，其中都是英文小写字母。如果s中的子串含有的每种字符都是偶数个，那么这样的子串就是达标子串，子串要求是连续串，返回s中达标子串的最大长度。
条件：1 <= s.length() <= 10^5.

```java
class Solution {
    public int maxlengthofyessubstring(String s) {
        Map<Integer,Integer> map = new HashMap<>();
        map.put(0, -1);
        int n = s.length();
        int status = 0;
        int ans = 0;
        for(int i = 0; i < n; i++) {
            status ^= 1 << (s.charAt(i) - 'a');
            if(map.containsKey(status)) {
                ans = Math.max(ans, i - map.get(status));
            }else {
                map.put(status,i);
            }
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
                    //此处将右边的单词移入滑动窗口
                    String word=s.substring(start+(m-1)*n,start+m*n);
                    differ.put(word,differ.getOrDefault(word,0)+1);
                    //为什么这里需要判断一下word是否等于0？因为前面的滑动窗口可能遗留了东西下来，如果滑动到这里匹配上了，就需要把这个word删掉；
                    if(differ.get(word) == 0) {
                        differ.remove(word);
                    }
                    
                    //此处将左边的单词移出滑动窗口
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
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(map.containsKey(nums[i])){
                if(i-map.get(nums[i])<=k){
                    return true;
                }
            }
            map.put(nums[i],i);
        }
       return false;
    }
}
```

## LC.220 存在重复元素III
> https://leetcode.cn/problems/contains-duplicate-iii/
```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if(k<1 || t<0){
            return false;
        }
        HashMap<Long,Long> map=new HashMap<>();
        for(int i=0;i<nums.length;i++){
            long remapnum=(long)nums[i]-Integer.MIN_VALUE;
            long bucket=remapnum/((long)t+1);
            if(map.containsKey(bucket) || (map.containsKey(bucket-1) && remapnum-map.get(bucket-1)<=t) || (map.containsKey(bucket+1) && map.get(bucket+1)-remapnum<=t) ){
                return true;
            }

            if(map.entrySet().size()>=k){
                long lastBucket=((long)nums[i-k]-Integer.MIN_VALUE)/((long)t+1);
                map.remove(lastBucket);
            }
            map.put(bucket,remapnum);
        }
        return false;

    }
}
```

## LC.239 滑动窗口最大值
> https://leetcode.cn/problems/sliding-window-maximum/
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        PriorityQueue<int[]> queue = new PriorityQueue<>((a,b) ->(b[0]-a[0]));
        int[] ans = new int[n-k+1];
        int idx = 0;
        for(int i = 0; i < n; i++) {
            queue.add(new int[]{nums[i],i});
            if(i >= k - 1) {
                while(queue.peek()[1] <= i - k) {
                    queue.poll();
                }
                ans[idx++] = queue.peek()[0];
            }
            
        }
        return ans;
    }
}
```
