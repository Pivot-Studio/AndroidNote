- ## 题目
	- 给定一个整数数组  `nums` 和一个正整数 `k`，找出是否有可能把这个数组分成 `k` 个非空子集，其总和都相等。
- ## Code
	- 方法一：动态规划
	- 与[[火柴拼正方形]]思路完全相同
	- ```java
	  class Solution {
	      public boolean canPartitionKSubsets(int[] nums, int k) {
	          int sum=0;
	          for(int num:nums){
	              sum+=num;
	          }
	          if(sum%k!=0)return false;
	          sum/=k;
	          int len=nums.length;
	          int[] dp=new int[(1<<len)];
	          Arrays.fill(dp,-1);
	          dp[0]=0;
	          for(int i=1;i<(1<<len);i++){
	              for(int j=1,l=0;j<(1<<len);j<<=1,l++){
	                  if((i&j)==0){
	                      continue;
	                  }
	                  int last=i^(j);
	                  if(dp[last]>=0&&dp[last]+nums[l]<=sum){
	                      dp[i]=(dp[last]+nums[l])%sum;
	                  }
	              }
	          }
	          return dp[(1<<len)-1]==0;
	      }
	  }
	  ```