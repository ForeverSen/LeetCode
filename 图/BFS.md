### 广度优先遍历BFS：
   广度优先搜索算法(Breadth First Search)，又称为"宽度优先搜索"或"横向优先搜索"，简称BFS。

它的思想是：从图中某顶点v出发，在访问了v之后依次访问v的各个未曾访问过的邻接点，然后分别从这些邻接点出发依次访问它们的邻接点，并使得“先被访问的顶点的邻接点先于后被访问的顶点的邻接点被访问，直至图中所有已被访问的顶点的邻接点都被访问到。如果此时图中尚有顶点未被访问，则需要另选一个未曾被访问过的顶点作为新的起始点，重复上述过程，直至图中所有顶点都被访问到为止。需要注意，遍历过的节点不能再次被遍历。
#### BFS应用：
BFS可用于解决2类问题：

从A出发是否存在到达B的路径；
从A出发到达B的最短路径(这个应该叫最少步骤合理，第一次遍历到目的节点，其所经过的路径为最短路径)；注意，使用 BFS 只能求解无权图的最短路径。
#### 实现：
使用队列来存储每一轮遍历得到的节点,对于遍历过的节点，应该将它标记，防止重复遍历。
#### 典例：
计算在网格中从原点到特定点的最短路径长度

[[1,1,0,1],
[1,0,1,0],
[1,1,1,1],
[1,0,1,1]]

- 1 表示可以经过某个位置，求解从 (0, 0) 位置到 (tr, tc) 位置的最短路径长度。
- 由于每个点需要保存x坐标，y坐标以及长度，所以必须要用一个类将三个属性封装起来。
- 由于bfs每次只将距离加一，所以当位置抵达终点时，此时的距离就是最短路径了。
 ~~~ java
private static class Position {
    int r, c, length;
    public Position(int r, int c, int length) {
        this.r = r;
        this.c = c;
        this.length = length;
    }
}

 public static int minPathLength(int[][] grids, int tr, int tc) {
        int[][] next = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        int m = grids.length, n = grids[0].length;
        Queue<Position> queue = new LinkedList<>();
        queue.add(new Position(0, 0, 1));
        while (!queue.isEmpty()) {
            Position pos = queue.poll();
            for (int i = 0; i < 4; i++) {//4是数组next的长度
                Position nextPos = new Position(pos.r + next[i][0], pos.c + next[i][1], pos.length + 1);
                if (nextPos.r < 0 || nextPos.r >= m || nextPos.c < 0 || nextPos.c >= n) continue;
                if (grids[nextPos.r][nextPos.c] != 1) continue;
                grids[nextPos.r][nextPos.c] = 0;
                if (nextPos.r == tr && nextPos.c == tc) return nextPos.length;
                queue.add(nextPos);
            }
        }
        return -1;
    }

 ~~~

#### LeetCode相关题解：
[279. Perfect Square(Medium)](https://leetcode.com/problems/perfect-squares/)
##### 题目描述：
给定一个正整数n，求和为n的最小全平方数（例如，1，4，9，16，…）。
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.

Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
##### 解题思路:
- 思路来源参考@CyC2018Leetcode刷题题解
- 可以将每个整数看成图中的一个节点，如果两个整数之差为一个平方数，那么这两个整数所在的节点就有一条边。
- 要求解最小的平方数数量，就是求解从节点 n 到节点 0 的最短路径。
- 首先生成平方数序列放入数组，然后通过队列，每次减去一个平方数，把剩下的数加入队列，也就是通过bfs的方式，当此时的数刚好等于平方数，则满足题意，由于每次循环level加一，所以最后输出的level就是需要的平方数个数
```java
class Solution {
    public int numSquares(int n) {
        List<Integer> squares = getSquares(n);//获取所有平方数，存在链表中
        Queue<Integer> queue = new LinkedList<>();
        boolean[] marked = new boolean[n + 1];//这里是n+1，比较好计算
        queue.add(n);
        marked[n] = true;
        int level = 0;//level为第几层
        while(!queue.isEmpty()){
            int size = queue.size();         
            level++;
            while(size-- > 0){
                int top = queue.poll();
                for(int s : squares){
                    if (top - s==0){
                        return level;
                    }
                    if (top - s < 0){
                        break;
                    }
                    if (marked[top - s]){
                        continue;
                    }
                    marked[top - s] =true;//标记为true的不再遍历
                    queue.add(top - s);
                }
                
            }
        }
        return n;
    }
    //先定义一个求小于n的所有平方数的方法
    private List getSquares(int n){
        List<Integer> list = new ArrayList<>();
        int square = 1;
        int i = 3;
        while (square <= n){
            list.add(square);
            square += i;
            i += 2;
        }
        return list;
    }
}
```