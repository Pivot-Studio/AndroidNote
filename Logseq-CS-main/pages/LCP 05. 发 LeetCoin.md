- ## 题目
	- 力扣决定给一个刷题团队发LeetCoin作为奖励。同时，为了监控给大家发了多少LeetCoin，力扣有时候也会进行查询。
	- 该刷题团队的管理模式可以用一棵树表示：
	- 团队只有一个负责人，编号为1。除了该负责人外，每个人有且仅有一个领导（负责人没有领导）；
	  不存在循环管理的情况，如A管理B，B管理C，C管理A。
	-
	- 力扣想进行的操作有以下三种：
		- 给团队的一个成员（也可以是负责人）发一定数量的LeetCoin；
		- 给团队的一个成员（也可以是负责人），以及他/她管理的所有人（即他/她的下属、他/她下属的下属，……），发一定数量的LeetCoin；
		- 查询某一个成员（也可以是负责人），以及他/她管理的所有人被发到的LeetCoin之和
	- 输入：
	  N表示团队成员的个数（编号为1～N，负责人为1）；
	  leadership是大小为(N - 1) * 2的二维数组，其中每个元素[a, b]代表b是a的下属；
	  operations是一个长度为Q的二维数组，代表以时间排序的操作，格式如下：
	  operations[i][0] = 1: 代表第一种操作，operations[i][1]代表成员的编号，operations[i][2]代表LeetCoin的数量；
	  operations[i][0] = 2: 代表第二种操作，operations[i][1]代表成员的编号，operations[i][2]代表LeetCoin的数量；
	  operations[i][0] = 3: 代表第三种操作，operations[i][1]代表成员的编号；
	- 输出：返回一个数组，数组里是每次查询的返回值（发LeetCoin的操作不需要任何返回值）。由于发的LeetCoin很多，请把每次查询的结果模1e9+7 (1000000007)。
- ## Code
	- ### 方法一：树状数组
	  ```java
	  class Solution {
	      int p;
	      int[] left,right;//分别记录i节点的左右分支对应的index
	      long[] d,e;//d代表a[i]-a[i-1],e=d*i,便于树状数组查询计算
	      public int[] bonus(int n, int[][] leadership, int[][] operations) {
	          left=new int[n+1];
	          right=new int[n+1];
	          List<Integer>[] edges=new List[n+1];
	          for(int i=1;i<=n;i++){
	              edges[i]=new ArrayList<>();
	          }
	          for(int[] ls:leadership){
	              edges[ls[0]].add(ls[1]);
	          }
	          p=1;
	          dfs(1,edges);//初始化树状数组
	          d=new long[p+1];
	          e=new long[p+1];
	          List<Integer> ans=new ArrayList<>();
	          int mod=(int)(1e9+7);
	          for(int[] op:operations){
	              if(op[0]==1){
	                  rangeUpdate(left[op[1]],left[op[1]],op[2]);
	              }else if(op[0]==2){
	                  rangeUpdate(left[op[1]],right[op[1]],op[2]);
	              }else{
	                  ans.add((int)(rangeQuery(left[op[1]],right[op[1]])%mod));
	              }
	          }
	          return ans.stream().mapToInt(Integer::valueOf).toArray();
	      }
	      public void dfs(int i,List<Integer>[] edges){
	          left[i]=p;
	          for(int e:edges[i]){
	              p++;
	              dfs(e,edges);
	          }
	          right[i]=p;
	      }
	      public void rangeUpdate(int l,int r,int v){
	          update(l,v);
	          update(r+1,-v);
	      }
	      public void update(int i,int v){
	          int x=i;
	          while(i<=p){
	              d[i]+=v;
	              e[i]+=x*v;
	              i+=lowBit(i);
	          }
	      }
	      public long rangeQuery(int l,int r){
	          return query(r)-query(l-1);
	      }
	      public long query(int i){
	          int x=i;
	          long sum=0;
	          while(i>0){
	              sum+=(x+1)*d[i]-e[i];
	              i-=lowBit(i);
	          }
	          return sum;
	      }
	      public int lowBit(int x){
	          return x&(-x);
	      }
	  }
	  ```