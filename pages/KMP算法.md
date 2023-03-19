- ```java
  public class StringUtils {
      /** 获取字符串s的next[]数组 */
      public static int[] getNextSeq(String s) {
          int size = s.length();
          int i = 0;
          int j = -1;
          int[] next = new int[size];
          next[0] = -1;
          while (i < size - 1) {
              if ((j == -1) || (s.charAt(i) == s.charAt(j))) {
                  j++;
                  i++;
                  next[i] = j;
              } else {
                  j = next[j];
              }
          }
          for (int k = 0; k <= size - 1; k++) {
              System.out.print(next[k]);
              System.out.print(", ");
          }
          System.out.println();
          return next;
      }
      public static int getIndexKMP(String s, String t, int pos) {
          int sSize = s.length();
          int tSize = t.length();
          int i = pos;
          int j = 0;
          int[] next = getNextSeq(t);
          while (i < sSize && j < tSize) {
              if ((j == -1) || (s.charAt(i) == t.charAt(j))) {
                  if (j > -1) {
                      System.out.println(s.charAt(i) + "=" + t.charAt(j));
                  }
                  i++;
                  j++;
              } else {
                  j = next[j];
              }
          }
          if (j >= tSize) {
              return i - tSize;
          }
          return -1;
      }
  }
  ```
-