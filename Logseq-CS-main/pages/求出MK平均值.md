- ## 题目
	- 给你两个整数 m 和 k ，以及数据流形式的若干整数。你需要实现一个数据结构，计算这个数据流的 MK 平均值 。
	- MK 平均值 按照如下步骤计算：
	- 如果数据流中的整数少于 m 个，MK 平均值 为 -1 ，否则将数据流中最后 m 个元素拷贝到一个独立的容器中。
	  从这个容器中删除最小的 k 个数和最大的 k 个数。
	  计算剩余元素的平均值，并 向下取整到最近的整数 。
	  请你实现 MKAverage 类：
	- MKAverage(int m, int k) 用一个空的数据流和两个整数 m 和 k 初始化 MKAverage 对象。
	  void addElement(int num) 往数据流中插入一个新的元素 num 。
	  int calculateMKAverage() 对当前的数据流计算并返回 MK 平均数 ，结果需 向下取整到最近的整数 。
- ## Code
	- 方法一:TreeMap+队列
	- 平均值=容器中间部分总和/总数目m-2k
	- 三个treeMap分别维护最大k个数，最小k个数，中间值m-2k个数,（key为对应int值，value为重复个数）
	- arrayQueue维护插入数据队列
	- 每次添加新数据，先加入到treeMap中，如果q的长度超过界限了，就把队列最前端的元素从相应TreeMap中删掉
	- 之后检测最大最小k个数的treeMap长度是否高于k，过界就移入mid
	- 如果小于k，并且此时mid不为空时，用mid去填补
	- (整个过程需要维护mid总和)
	-
	- ```java
	  class MKAverage {
	  
	      private int m, k;
	      private long s;
	      private int size1, size3;
	      private Deque<Integer> q = new ArrayDeque<>();
	      private TreeMap<Integer, Integer> lo = new TreeMap<>();
	      private TreeMap<Integer, Integer> mid = new TreeMap<>();
	      private TreeMap<Integer, Integer> hi = new TreeMap<>();
	  
	      public MKAverage(int m, int k) {
	          this.m = m;
	          this.k = k;
	      }
	  
	      public void addElement(int num) {
	          if (lo.isEmpty() || num <= lo.lastKey()) {
	              lo.merge(num, 1, Integer::sum);
	              ++size1;
	          } else if (hi.isEmpty() || num >= hi.firstKey()) {
	              hi.merge(num, 1, Integer::sum);
	              ++size3;
	          } else {
	              mid.merge(num, 1, Integer::sum);
	              s += num;
	          }
	          q.offer(num);
	          if (q.size() > m) {
	              int x = q.poll();
	              if (lo.containsKey(x)) {
	                  if (lo.merge(x, -1, Integer::sum) == 0) {
	                      lo.remove(x);
	                  }
	                  --size1;
	              } else if (hi.containsKey(x)) {
	                  if (hi.merge(x, -1, Integer::sum) == 0) {
	                      hi.remove(x);
	                  }
	                  --size3;
	              } else {
	                  if (mid.merge(x, -1, Integer::sum) == 0) {
	                      mid.remove(x);
	                  }
	                  s -= x;
	              }
	          }
	          for (; size1 > k; --size1) {
	              int x = lo.lastKey();
	              if (lo.merge(x, -1, Integer::sum) == 0) {
	                  lo.remove(x);
	              }
	              mid.merge(x, 1, Integer::sum);
	              s += x;
	          }
	          for (; size3 > k; --size3) {
	              int x = hi.firstKey();
	              if (hi.merge(x, -1, Integer::sum) == 0) {
	                  hi.remove(x);
	              }
	              mid.merge(x, 1, Integer::sum);
	              s += x;
	          }
	          for (; size1 < k && !mid.isEmpty(); ++size1) {
	              int x = mid.firstKey();
	              if (mid.merge(x, -1, Integer::sum) == 0) {
	                  mid.remove(x);
	              }
	              s -= x;
	              lo.merge(x, 1, Integer::sum);
	          }
	          for (; size3 < k && !mid.isEmpty(); ++size3) {
	              int x = mid.lastKey();
	              if (mid.merge(x, -1, Integer::sum) == 0) {
	                  mid.remove(x);
	              }
	              s -= x;
	              hi.merge(x, 1, Integer::sum);
	          }
	      }
	  
	      public int calculateMKAverage() {
	          return q.size() < m ? -1 : (int) (s / (q.size() - k * 2));
	      }
	  }
	  
	  /**
	   * Your MKAverage object will be instantiated and called as such:
	   * MKAverage obj = new MKAverage(m, k);
	   * obj.addElement(num);
	   * int param_2 = obj.calculateMKAverage();
	   */
	  ```