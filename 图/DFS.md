### 深度优先遍历DFS：

   假设初始状态是图中所有顶点均未被访问，则从某个顶点v出发，首先访问该顶点，然后依次从它的各个未被访问的邻接点出发深度优先搜索遍历图，直至图中所有和v有路径相通的顶点都被访问到。 若此时尚有其他顶点未被访问到，则另选一个未被访问的顶点作起始点，重复上述过程，直至图中所有顶点都被访问到为止。
#### DFS应用：
使用 DFS 对一个图进行遍历时，能够遍历到的节点都是从初始节点可达的，DFS 常用来求解 单点连通性、单点可达性、单点路径等。
#### 实现：
使用栈来保存当前节点信息，当遍历新节点返回时能够继续遍历当前节点。也可以用递归。
#### LeetCode相关题解：
#####   [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/description/)

给定一个包含了一些 0 和 1的非空二维数组 grid , 一个 岛屿 是由四个方向 (水平或垂直) 的 1 (代表土地) 构成的组合。你可以假设二维矩阵的四个边缘都被水包围着。
找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为0。)
~~~
示例 1:
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
 
对于上面这个给定矩阵应返回 6。注意答案不应该是11，因为岛屿只能包含水平或垂直的四个方向的‘1’。

示例 2:
[[0,0,0,0,0,0,0,0]]对于上面这个给定的矩阵, 返回 0。

注意: 给定的矩阵grid 的长度和宽度都不超过 50。 
~~~
##### 解题思路:
使用dfs计算每个岛屿的面积，在dfs期间，将岛中每个点的值设置为0，表示为已标记。
~~~ java
/*
*使用DFS，先用二维数组存储四个方向，然后分别递归遍历每个点
*遍历过的点将其置为0，不再遍历
*/
class Solution {
    private int m, n;
    private int[][] direction = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};//四个方向
    public int maxAreaOfIsland(int[][] grid) {
        m = grid.length;//行
        n = grid[0].length;//列 
        int maxArea = 0;
        if (m == 0 || grid == null) {
            return 0;
        }
        for (int i = 0;i < m;i++) {
            for (int j = 0;j < n;j++) {
                maxArea = Math.max(maxArea,dfs(grid,i,j));
            }
        }
        return maxArea;
    }
    private int dfs(int [][] grid,int i,int j){
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 0) {
            return 0;
        }
        grid[i][j] = 0;//设为已标记
        int area = 1;
        for (int [] d : direction) {
            area += dfs(grid,i + d[0],j + d[1]);
        }
        return area;
    }
}
~~~
##### [200. Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

##### 
给定一个由 '1'（陆地）和 '0'（水）组成的的二维网格，计算岛屿的数量。一个岛被水包围，并且它是通过水平方向或垂直方向上相邻的陆地连接而成的。你可以假设网格的四个边均被水包围。

~~~ 
Example 1:
Input:
11110
11010
11000
00000

Output: 1

Example 2:
Input:
11000
11000
00100
00011

Output: 3
~~~
##### 解题思路：
本题跟上面leetcode695有点类似，都是岛屿问题，可以将矩阵表示看成一张有向图，把问题转换为对每个点进行DFS遍历，如果有遍历完一个，那么岛屿数量+1
~~~ java
class Solution {
    private int directions[][] = {{0,1},{0,-1},{1,0},{-1,0}};
    private int m , n;
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        m = grid.length;
        n = grid[0].length;
        int num = 0;//岛屿数量
        for (int i = 0;i < m;i++) {
            for (int j = 0;j < n;j++) {
                if (grid[i][j] != '0') {
                    dfs(grid,i,j);
                    num++;
                }
            }
        }
        return num;
    }
    private void dfs(char [][] grid,int i,int j){
        if(i >= m || i < 0 || j >= n || j < 0 || grid[i][j] =='0'){
            return;
        }
        grid[i][j] = '0';//遍历过的不再遍历
        for(int d[] : directions){
            dfs(grid,i + d[0],j + d[1]);
        }
    }
}
~~~
##### [547. Friend Circles](https://leetcode.com/problems/friend-circles/)

班上有 N 名学生。其中有些人是朋友，有些则不是。他们的友谊具有是传递性。如果已知 A 是 B 的朋友，B 是 C 的朋友，那么我们可以认为 A 也是 C 的朋友。所谓的朋友圈，是指所有朋友的集合。

给定一个 N * N 的矩阵 M，表示班级中学生之间的朋友关系。如果M[i][j] = 1，表示已知第 i 个和 j 个学生互为朋友关系，否则为不知道。你必须输出所有学生中的已知的朋友圈总数。
~~~
Input: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
Output: 2

Input: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
Output: 1
~~~
##### 解题思路：
 DFS中的朋友圈问题，注意这是个N*N矩阵，可以使用一个hasVisited数组, 依次判断每个节点, 如果其未访问, 朋友圈数加1并对该节点进行dfs搜索标记所有访问到的节点。
 ~~~ java
class Solution {
    private int m;    //N*N矩阵 
    public int findCircleNum(int[][] M) {
        if (M.length == 0 || M == null) {
            return -1;
        }
        m = M.length;
        boolean [] hasVisited = new boolean[m]; //是否访问
        int circleNum  = 0;
        for (int i = 0; i < m; i++) {
            if (!hasVisited[i]) {
                dfs(M,i,hasVisited);
                circleNum++;
            }
            
        } 
        return circleNum;
    }
    private void dfs(int [][] M,int i,boolean [] hasVisited) {
        hasVisited[i] = true;
        for (int k = 0; k < m; k++) {
            if (M[i][k] == 1 && !hasVisited[k])
                dfs(M,k,hasVisited);
        }
    }
}
 ~~~

##### [207. Course Schedule](https://leetcode.com/problems/course-schedule/)

现在你总共有 n 门课需要选，记为 0 到 n-1。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: [0,1]

给定课程总量以及它们的先决条件，判断是否可能完成所有课程的学习？
~~~
输入: 2, [[1,0]] 
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。

输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
~~~
##### 解题思路：
构建逆邻接表，实现深度优先遍历，其实就是检测这个有向图中有没有环，只要存在环，课程就不能完成。另外，当访问一个结点的时候，应该递归访问它的前驱结点，直至前驱结点没有前驱结点为止。
~~~ java
/**
*采用深度优先遍历解决有无环
*如果采用BFS会更快
**/
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer> [] graphic = new List[numCourses];
        boolean [] marked = new boolean[numCourses];//判断那个数是否被标记
        for (int i = 0;i < numCourses;i++){
            graphic[i] = new ArrayList();//对每个数字生成一个链表
        }
        for (int [] pre : prerequisites){
            graphic[pre[0]].add(pre[1]);
        }
        for (int i = 0;i < numCourses;i++){
            if(hasCycle(graphic,marked,i)){
                return false;
            }
        }
        return true;
    }
    //判断是否含环
    private boolean hasCycle(List<Integer> [] graphic,boolean [] marked,int cur){
        if (marked[cur]){
            return true;//如果被标记，则直接返回true
        }
        marked[cur] = true;
        for (int next : graphic[cur]){
            if (hasCycle(graphic,marked,next)){
                return true;
            }
        }
        marked[cur] = false;
        return false;
    }
}
~~~
##### [130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/description/)

给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。
示例:

X X X X
X O O X
X X O X
X O X X
运行你的函数后，矩阵变为：

X X X X
X X X X
X X X X
X O X X
解释:

被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

##### 解题思路：
首先对矩阵边界上所有的O做深度优先搜索，将相连的O更改为 * ，然后编辑数组，将数组中O更改为X，将数组中 * 更改为O。
~~~ java
class Solution {
    private int m,n;
    private int [] [] directions = {{0,1},{0,-1},{1,0},{-1,0}};

    public void solve(char[][] board) {
        if (board == null || board.length == 0){
            return;
        }
        m = board.length;
        n = board[0].length;
        
        //边界值遍历
        for (int i = 0; i < m; i++) {
            dfs(board, i, 0);
            dfs(board, i, n-1);
        }
        for (int j = 0; j < n; j++) {
            dfs(board, 0, j);
            dfs(board, m - 1, j);
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++){
                if(board[i][j] == 'O') {
                    board [i][j] = 'X'; //将被包围在中间的置为X
                }else if (board[i][j] == '*'){
                    board [i][j] = 'O'; //将边界的置为O
                }
            }
        }
        
    }
    private void dfs(char [][] board, int i, int j) {
        if (i < 0 || i >= m || j < 0 || j >= n || board[i][j] != 'O'){
            return;
        }
        board[i][j] = '*';  //将被包围的O，并且O不在边界 置为其他符号
        for (int [] d: directions) {
            dfs(board, i + d[0],j + d[1]);
        }
    }

}
~~~
#### 总结：
通过刷题知道DFS类型的解题思路，大概有岛屿问题，朋友圈问题，此时解题模板也是类似的，用递归遍历，遍历过了点设为已标记。