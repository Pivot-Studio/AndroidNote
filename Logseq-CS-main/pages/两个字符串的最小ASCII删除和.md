- ## 题目
	- 给定两个字符串`s1` 和 `s2`，返回 *使两个字符串相等所需删除字符的 **ASCII **值的最小和 *。
- ## Code
	- 方法一：动态规划
	- dp[i][j]代表[0-i]段的s1和[0-j]段的s2的最小删除和
	- i,j范围为（length>i>=0）(length>j>=0)
	- 分析dp[i][j]状态方程
	- 补充从1，1开始循环缺失的边缘dp数值
	- ```java
	  class Solution {
	      public int minimumDeleteSum(String s1, String s2) {
	          int m = s1.length(), n = s2.length();
	          int[][] dp = new int[m + 1][n + 1];
	          for (int i = 1; i <= m; i++) {
	              dp[i][0] = dp[i - 1][0] + s1.codePointAt(i - 1);
	          }
	          for (int j = 1; j <= n; j++) {
	              dp[0][j] = dp[0][j - 1] + s2.codePointAt(j - 1);
	          }
	          for (int i = 1; i <= m; i++) {
	              int code1 = s1.codePointAt(i - 1);
	              for (int j = 1; j <= n; j++) {
	                  int code2 = s2.codePointAt(j - 1);
	                  if (code1 == code2) {
	                      dp[i][j] = dp[i - 1][j - 1];
	                  } else {
	                      dp[i][j] = Math.min(dp[i - 1][j] + code1, dp[i][j - 1] + code2);
	                  }
	              }
	          }
	          return dp[m][n];
	      }
	  }
	  
	  ```