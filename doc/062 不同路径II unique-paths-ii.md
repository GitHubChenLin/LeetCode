原题链接： [https://leetcode-cn.com/problems/unique-paths-ii/description/](https://leetcode-cn.com/problems/unique-paths-ii/description/)

## 题目
[63] 不同路径 II

 一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

 机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

![robot_maze](../img/robot_maze.png)
 现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

 网格中的障碍物和空位置分别用 1 和 0 来表示。

 说明：m 和 n 的值均不超过 100。

 示例 1:

 输入:

 [

 [0,0,0],

 [0,1,0],

 [0,0,0]

 ]

 输出: 2

 解释:

 3x3 网格的正中间有一个障碍物。

 从左上角到右下角一共有 2 条不同的路径：

 1\. 向右 -> 向右 -> 向下 -> 向下

 2\. 向下 -> 向下 -> 向右 -> 向右

## 思路
这题是 `062 不同路径`的变题，按照分析来看，依旧可以使用动态规划去解，即`N(m,n) = N(m-1,n) + N(m,n-1)`，但是此题多了很多不少坑：
- 如果障碍物在出发点，即 [[0]]，无论如何之后是否有障碍，都无法到达终点（N = 0）。
![063(1)](../img/063(1).png)
- 如果数组的长度为1或者数组[1]的长度为1，即为条形且任意一格出现障碍，也无法到达终点。
![063(2)](../img/063(2).png)
所以，在遍历数组时当i=0或者j=0不能像062一样，全部设为1，而是要单独设值。上述两点也可并起来一起考虑。

- 其余地方出现障碍，设为零。

总结：这题难度就在于有多个地方需要考虑，不能单纯地将有障碍的一格设为零，然后运用`N(m,n) = N(m-1,n) + N(m,n-1)`公式。

## Code
```java
class Solution {
   public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] num = new int[m][n];
        if(obstacleGrid[0][0] == 1){
            return 0;
        }

        for(int i = 0;i<m;i++){
            if((m == 1 || n == 1) && obstacleGrid[i][0] == 1){
                return 0;
            }
            if(obstacleGrid[i][0] != 1){
                num[i][0] = 1;
            }else{
                break;
            }
        }

        for(int i = 0;i<m;i++){
            for(int j = 1;j < n;j++){
                if(i == 0){
                    if((m == 1 || n == 1) && obstacleGrid[i][j] == 1){
                        return 0;
                    }
                    if(obstacleGrid[i][j] != 1){
                        num[0][j]= 1;
                    }else{
                        break;
                    }
                }else{
                    if(obstacleGrid[i][j] == 1){
                        num[i][j] = 0;
                    }else{
                        num[i][j] = num[i-1][j]+num[i][j-1];
                    }
                }
            }
        }
        return num[m-1][n-1];
    }
}
```
