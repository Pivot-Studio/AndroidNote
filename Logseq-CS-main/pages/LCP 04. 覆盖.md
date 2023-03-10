- ## 题目
	- 你有一块棋盘，棋盘上有一些格子已经坏掉了。你还有无穷块大小为1 * 2的多米诺骨牌，你想把这些骨牌不重叠地覆盖在完好的格子上，请找出你最多能在棋盘上放多少块骨牌？这些骨牌可以横着或者竖着放。
	- 输入：n, m代表棋盘的大小；broken是一个b * 2的二维数组，其中每个元素代表棋盘上每一个坏掉的格子的位置。
	- 输出：一个整数，代表最多能在棋盘上放的骨牌数。
- ## Code
	- ### 方法一：匈牙利算法
		- ```java
		  // 匈牙利算法
		  class Solution {
		      private boolean[] visit;//表示点访问过
		      private int[] link;// 表示与当前点相连的点
		  
		      private boolean[][] board;//
		      private int[][] dir = { { 1, 0 }, { -1, 0 }, { 0, -1 }, { 0, 1 } };
		  
		      public int domino(int n, int m, int[][] broken) {
		          if (broken.length == 0) {// 无坏掉的
		              return n * m >> 1;
		          }
		  
		          // 初始化棋盘，false表示坏掉
		          board = new boolean[n][m];
		          for (int i = 0; i < n; ++i) {
		              Arrays.fill(board[i], true);
		          }
		          for (int[] b : broken) {
		              board[b[0]][b[1]] = false;
		          }
		  
		          return hungary();
		      }
		  
		      private int hungary() {
		          int n = board.length, m = board[0].length;
		          visit = new boolean[n * m];
		          link = new int[n * m];
		          Arrays.fill(link, -1);
		          int ret = 0;
		         
		          for (int r = 0; r < n; ++r) {
		              for (int c = ((r & 1) == 0 ? 0 : 1); c < m; c += 2) {//r为奇数c=1,r为偶数c=0
		                  if (board[r][c]) {
		                      Arrays.fill(visit, false);
		                      if (find(r, c)) {
		                          ++ret;
		                      }
		                  }
		              }
		          }
		          return ret;// 返回最大匹配数
		      }
		  
		      private boolean find(int row, int col) {
		          int n = board.length, m = board[0].length;
		          for (int[] d : dir) {// 四个相邻点
		              int r = row + d[0];
		              int c = col + d[1];
		              if (r < 0 || r >= n || c < 0 || c >= m) {
		                  continue;// 越界
		              }
		              int v2 = r * m + c;
		              if (board[r][c] && !visit[v2]) {// 完好并且未访问过
		                  visit[v2] = true;
		                  if (link[v2] == -1 || find(link[v2] / m, link[v2] % m)) {//不存在链接或者四个相邻点只一存在增广路径
		                      link[v2] = row * m + col;
		                      return true;// 找到增广路径
		                  }
		              }
		          }
		          return false;// 找不到增广路径
		      }
		  }
		  ```