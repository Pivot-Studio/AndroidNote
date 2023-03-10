## Code
	- Alice 和 Bob 用几堆石子在做游戏。几堆石子排成一行，每堆石子都对应一个得分，由数组 stoneValue 给出。
	- Alice 和 Bob 轮流取石子，Alice 总是先开始。在每个玩家的回合中，该玩家可以拿走剩下石子中的的前 1、2 或 3 堆石子 。比赛一直持续到所有石头都被拿走。
	- 每个玩家的最终得分为他所拿到的每堆石子的对应得分之和。每个玩家的初始分数都是 0 。比赛的目标是决出最高分，得分最高的选手将会赢得比赛，比赛也可能会出现平局。
	- 假设 Alice 和 Bob 都采取 最优策略 。如果 Alice 赢了就返回 "Alice" ，Bob 赢了就返回 "Bob"，平局（分数相同）返回 "Tie" 。
## 题目
	- 方法一：动态规划
	- 与[[预测赢家]]思路相似，从最后一步选择开始分析
	- dp[i]代表i到末尾两者得分差值的最大值
	- ```java
	  class Solution {
	      public String stoneGameIII(int[] stoneValue) {
	          int len=stoneValue.length;
	          int[] dp=new int[len+1];
	          for(int i=len-1;i>=0;i--){
	              dp[i]=stoneValue[i]-dp[i+1];
	              if(len-i>1){
	                 dp[i]=Math.max(stoneValue[i]+stoneValue[i+1]-dp[i+2],dp[i]);
	              }
	              if(len-i>2){
	                  dp[i]=Math.max(stoneValue[i]+stoneValue[i+1]+stoneValue[i+2]-dp[i+3],dp[i]);
	              }
	          }
	          if (dp[0] == 0) {
	              return "Tie";
	          } else {
	              return dp[0] > 0 ? "Alice" : "Bob";
	          }
	      }
	  }
	  ```