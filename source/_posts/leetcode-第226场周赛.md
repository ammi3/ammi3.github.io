---
title: leetcode.第226场周赛
date: 2021-02-02 21:49:37
tags:
---

### 题目1：1742.盒子中小球的最大数量

你在一家生产小球的玩具厂工作，有 n 个小球，编号从 lowLimit 开始，到 highLimit 结束（包括 lowLimit 和 highLimit ，即 n == highLimit - lowLimit + 1）。另有无限数量的盒子，编号从 1 到 infinity 。

你的工作是将每个小球放入盒子中，其中盒子的编号应当等于小球编号上每位数字的和。例如，编号 321 的小球应当放入编号 3 + 2 + 1 = 6 的盒子，而编号 10 的小球应当放入编号 1 + 0 = 1 的盒子。

给你两个整数 lowLimit 和 highLimit ，返回放有最多小球的盒子中的小球数量。如果有多个盒子都满足放有最多小球，只需返回其中任一盒子的小球数量。
<!--more-->

示例1
```
输入：lowLimit = 1, highLimit = 10
输出：2
解释：
盒子编号：1 2 3 4 5 6 7 8 9 10 11 ...
小球数量：2 1 1 1 1 1 1 1 1 0  0  ...
编号 1 的盒子放有最多小球，小球数量为 2 。
```

#### 题解
1. 对于区间`[lowLimit, highLimit]`中的每一个值，求其每位的和sum
2. 将(sum, count)这种表示sum出现次数的键-值对存入Map
3. 在遍历区间`[lowLimit, highLimit]`的同时，去更新出现次数最多的sum对应的区间数

#### 参考代码
```java
class Solution {
    public int countBalls(int lowLimit, int highLimit) {
      int res = 0;
      Map<Integer, Integer> map = new HashMap<>();
      for(int i = lowLimit; i <= highLimit; i++) {
        int sum = 0, tmp = i;
        while(tmp > 0) {
          sum += tmp % 10;
          tmp /= 10;
        }
        int val = map.getOrDefault(sum, 0);
        map.put(sum, val+1);
        res = Math.max(res, map.get(sum));
      }
      return res;
    }
}
```


### 题目2：1743. 从相邻元素对还原数组

存在一个由 n 个不同元素组成的整数数组 nums ，但你已经记不清具体内容。好在你还记得 nums 中的每一对相邻元素。

给你一个二维整数数组 adjacentPairs ，大小为 n - 1 ，其中每个 adjacentPairs[i] = [ui, vi] 表示元素 ui 和 vi 在 nums 中相邻。

题目数据保证所有由元素 nums[i] 和 nums[i+1] 组成的相邻元素对都存在于 adjacentPairs 中，存在形式可能是 [nums[i], nums[i+1]] ，也可能是 [nums[i+1], nums[i]] 。这些相邻元素对可以 按任意顺序 出现。

返回 原始数组 nums 。如果存在多种解答，返回 其中任意一个 即可。

示例1
```
输入：adjacentPairs = [[2,1],[3,4],[3,2]]
输出：[1,2,3,4]
解释：数组的所有相邻元素对都在 adjacentPairs 中。
特别要注意的是，adjacentPairs[i] 只表示两个元素相邻，并不保证其 左-右 顺序。
```

#### 题解
1. 遍历二维数组中的所有元素，找到**入度，出度分别为1的两个值**。出度为1：起点，入度为1：终点
2. 将二维数组中相邻的元素存入Map
3. 得到起点`也就是数组的第一个元素`后通过Map来组建结果

#### 参考代码
```java
class Solution {
    public int[] restoreArray(int[][] adjacentPairs) {
      int start = 0;
      Map<Integer, List<Integer>> map = new HashMap<>();
      for(int[] cur : adjacentPairs) {
        int u = cur[0];
        int v = cur[1];

        List<Integer> list1 = map.getOrDefault(u, new ArrayList<>());
        list1.add(v);

        List<Integer> list2 = map.getOrDefault(v, new ArrayList<>());
        list2.add(u);

        map.put(u, list1);
        map.put(v, list2);
      }

      for(Map.entry<Integer, List<Integer>> entry : map.entrySet()) {
        if(entry.getValue().size() == 1) {
          start = entry.getKey();
          break;
        }
      }
 
      Set<Integer> set = new HashSet<>();
      int[] res = new int[map.size()];
      res[0] = start;
      set.add(start);
      int index = 1;
      while(index < res.length) {
        List<Integer> cur = map.get(res[index-1]);
        for(int j = 0; j < cur.size(); j++) {
          if(!set.contains(cur.get(j))) {
            res[index++] = cur.get(j);
            set.add(cur.get(j));
            break
          }
        }
      }
      return res;
    }
```

### 题目3：1744. 你能在你最喜欢的那天吃到你最喜欢的糖果吗？

给你一个下标从 0 开始的正整数数组 candiesCount ，其中 candiesCount[i] 表示你拥有的第 i 类糖果的数目。同时给你一个二维数组 queries ，其中 queries[i] = [favoriteTypei, favoriteDayi, dailyCapi] 。

你按照如下规则进行一场游戏：

你从第 0 天开始吃糖果。
你在吃完 所有 第 i - 1 类糖果之前，不能 吃任何一颗第 i 类糖果。
在吃完所有糖果之前，你必须每天 至少 吃 一颗 糖果。
请你构建一个布尔型数组 answer ，满足 answer.length == queries.length 。answer[i] 为 true 的条件是：在每天吃 不超过 dailyCapi 颗糖果的前提下，你可以在第 favoriteDayi 天吃到第 favoriteTypei 类糖果；否则 answer[i] 为 false 。注意，只要满足上面 3 条规则中的第二条规则，你就可以在同一天吃不同类型的糖果。

请你返回得到的数组 answer 。

示例1
```
输入：candiesCount = [7,4,5,3,8], queries = [[0,2,2],[4,2,4],[2,13,1000000000]]
输出：[true,false,true]
提示：
1- 在第 0 天吃 2 颗糖果(类型 0），第 1 天吃 2 颗糖果（类型 0），第 2 天你可以吃到类型 0 的糖果。
2- 每天你最多吃 4 颗糖果。即使第 0 天吃 4 颗糖果（类型 0），第 1 天吃 4 颗糖果（类型 0 和类型 1），你也没办法在第 2 天吃到类型 4 的糖果。换言之，你没法在每天吃 4 颗糖果的限制下在第 2 天吃到第 4 类糖果。
3- 如果你每天吃 1 颗糖果，你可以在第 13 天吃到类型 2 的糖果。
```

#### 题解
1. 在吃第i类糖果时，前i类糖果必须吃完。那么要想在favorityDayi天吃到favoriteTypei类糖果，所以我们在吃第i类糖果时，已经吃了[preSum[i], preSum[i+1]]颗糖果
2. 对于queries中的每一个元素，我们判断此时已经吃的糖果数目的范围是否在[preSum[i], preSum[i+1]]内
    1. 若在此区间内，返回True
    2. 不在此区间内，返回False

#### 参考代码
```java
class Solution {
    public boolean[] canEat(int[] candiesCount, int[][] queries) {
      // 因为必须吃完第i-1类糖果之后，才能吃第i类糖果
      // 所以我们可以吃第i类糖果时已经吃了[preSum[i], preSum[i+1]]颗糖果
      // 所以只需要判定我们当前吃糖果的数量是否在那个区间内
      long[] preSum = new long[candiesCount.length+1];
      preSum[0] = 0;
      for(int i = 0; i < candiesCount.length; i++) {
        preSum[i+1] = candiesCount[i] + preSum[i];
      }

      boolean[] answer = new boolean[queries.length];

      // 由于天数是从0开始的，所以在计算favorityDay时应+1
      int i = 0;
      for(int[] cur : queries) {
        int favorityType = cur[0], favorityDay = cur[1], dailyCap = cur[2];

        // [min, max]与[preSum[i], preSum[i+1]]若有交集，返回True，否则，返回false
        answer[i++] = !((favorityDay + 1) > preSum[favorityType + 1] || ((long) (favorityDay + 1) * dailyCap < preSum[favorityType] + 1));
      }
      return answer;
    }
}
```


### 题目4：1745. 回文串分割 IV

给你一个字符串 s ，如果可以将它分割成三个 非空 回文子字符串，那么返回 true ，否则返回 false 。

当一个字符串正着读和反着读是一模一样的，就称其为 回文字符串 。

示例1
```
输入：s = "abcbdd"
输出：true
解释："abcbdd" = "a" + "bcb" + "dd"，三个子字符串都是回文的。
```

提示：
1. 3 <= s.length <= 2000
2. s​​​​​​ 只包含小写英文字母。

#### 题解
1. 判断一个字符串是否能被分割为3个非空回文子串，所以需要两个间断点将该字符串分割为三个子串
2. 对于分割后的三个子串，依次判断是否为非空回文串
3. 这里使用动态规划来存储字符[i, j]区间的子串是否为回文串


#### 参考代码
```java
class Solution {
    public boolean checkPartitioning(String s) {
      int len = s.length();
      boolean[][] dp = new boolean[len][len];

      for(int j = 0; j < len; j++) {
        for(int i = j; i >= 0; i--) {
          boolean flag = s.charAt(i) == s.charAt(j);
          if(i == j) dp[i][j] = true;
          else if(flag && j - i > 1 && dp[i + 1][j-1]) dp[i][j] = true;
          else if(flag && j - i == 1) dp[i][j] = true;
        }
      }

      for(int i = 1; i < len - 1; i++) {
        for(int j = i; j < len - 1; j++) {
          if(dp[i][j] && dp[0][i-1] && dp[j+1][len-1]) return true;
        }
      }

      return false;
    }
}
```
