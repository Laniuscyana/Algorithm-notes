# 图的深度优先和广度优先搜索

## LC.207 课程表
> https://leetcode.cn/problems/course-schedule/
```java
class Solution {
    //用于储存有向图
    List<List<Integer>> edges;
    //用于标记有向图节点的搜索情况，0为未搜索，1为搜索中，2为已经搜索
    int[] visited;
    //模拟栈，末尾为栈底，开头为栈顶
    int[] results;
    //记录是否有环，初始无环状态，false代表有环
    boolean valid=true;
    //栈下标
    int index;

    public int[] findOrder(int numCourses, int[][] prerequisites) {
        edges = new ArrayList<>();
        //初始化有向图
        for(int i = 0; i < numCourses; i++) {
            edges.add(new ArrayList<Integer>());
        }

        //初始化节点状态，模拟栈和栈下标
        visited = new int[numCourses];
        results = new int[numCourses];
        index = numCourses - 1;

        //导入有向图节点数据，info的结构为[后修课程，先修课程]，因此，在对应【先修课程】的位置插入后修课程，意义为先修课程指向后修课程（但后修课程不能够指向先修课程，出现环直接结束）
        for(int[] info : prerequisites) {
            edges.get(info[1]).add(info[0]);
        }

        //开始搜素，为了保证搜索的有效性，每次循环要检查valid是否为真
        for(int i = 0;i < numCourses && valid; i++){
            if(visited[i] == 0) {
                dfs(i);
            }
        }

        //如果有环，返回空
        if(!valid){
            return new int[0];
        }

        //搜索结束且无环，返回栈
        return results;
    }

    public void dfs(int u) {
        //标记搜索的节点为搜索中；这一步的必要性：搜索可能在发现环时终结，因此不能等搜索完直接标记已完成搜索
        visited[u] = 1;

        //开始搜索，注意这里环的判定有两次，如果去掉未搜索环节的环判定条件，可能会在已经有环的情况下完成搜索
        for(int v : edges.get(u)) {
            if(visited[v] == 0){
                dfs(v);
                if(!valid){
                    return;
                }
            }else if(visited[v] == 1){
                valid=false;
                return;
            }
        }

        //顺利搜索完，更新节点状态
        visited[u] = 2;
        //将节点入栈
        results[index--] = u;
    }
}
```
## LC.210 课程表II
> https://leetcode.cn/problems/course-schedule-ii/
```java

```
