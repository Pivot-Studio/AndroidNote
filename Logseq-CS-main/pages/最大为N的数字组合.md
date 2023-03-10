- ## 题目
	- 给定一个按 非递减顺序 排列的数字数组 digits 。你可以用任意次数 digits[i] 来写的数字。例如，如果 digits = ['1','3','5']，我们可以写数字，如 '13', '551', 和 '1351315'。
	- 返回 可以生成的小于或等于给定整数 n 的正整数的个数 。
- ## Code
	- 方法一：动态规划
	- 从n的最高位向最低位遍历
	- dp[i][0]代表由digits进行组合，数等于n的前i位数的组合个数
	- dp[i][1]代表由digits进行组合，数小于n的前i位数的组合个数
	- 遍历每一个digits值，将此值作为个位数，
	- 当比较的是n的前i位数(i>1)时，无论digits值为啥，其本身个位数都满足条件，
	- 如果等于n的对应位的值，则dp[i][0]+=dp[i-1][0],dp[i][1]=dp[i-1][1]
	- 如果小于n的对应位的值，dp[i][0]+=dp[i-1][1]+dp[i-1][0]
	- 如果大于n的对应位的值，dp[i][0]+=dp[i-1][0]
	- 最后返回小于和等于的个数dp[k][0] + dp[k][1];
	- ```java
	  class Solution {
	      public int atMostNGivenDigitSet(String[] digits, int n) {
	          String s = Integer.toString(n);
	          int m = digits.length, k = s.length();
	          int[][] dp = new int[k + 1][2];
	          dp[0][1] = 1;
	          for (int i = 1; i <= k; i++) {
	              for (int j = 0; j < m; j++) {
	                	if(i!=1)dp[i][0]++;
	                  if (digits[j].charAt(0) == s.charAt(i - 1)) {
	                    	dp[i][0]+=dp[i-1][0];
	                      dp[i][1] = dp[i - 1][1];
	                  } else if (digits[j].charAt(0) < s.charAt(i - 1)) {
	                      dp[i][0] += dp[i - 1][1]+dp[i - 1][0];
	                  }else{
	                    	dp[i][0] += dp[i - 1][0];
	                  }
	              }
	          }
	          return dp[k][0] + dp[k][1];
	      }
	  }
	  ```
-