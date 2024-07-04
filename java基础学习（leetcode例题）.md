# java基础学习（leetcode例题）

## 一.[3099. 哈沙德数](https://leetcode.cn/problems/harshad-number/)(简单)

1.int to String：

+ Interger.toString(int)
+ String.valueof(int)

``````java
public int sumOfTheDigitsOfHarshadNumber(int x) {
        int total = 0;
        String s = Integer.toString(x);
        for (char ch : s.toCharArray()) {
            total += ch - '0';
        }
        return x % total == 0 ? total : -1;
    }
``````



## 二.[3153. 所有数对中数位不同之和](https://leetcode.cn/problems/sum-of-digit-differences-of-all-pairs/)(中等)

1.数组创建

+ int ls[][] = new int\[10\]\[10\]
+ new 数组长度可以是变量，不一定是常量
+ 每个元素默认初始值是0
+ 数组长度：`ls.length` 不用加括号，但是String长度要加括号

2.字符串遍历

+ s位于i位的字符读取：s.charAt(i)
+ `for(char ch:s.toCharArray())`

``````java
public long sumDigitDifferences(int[] nums) {
        long ans = 0;
        int len = Integer.toString(nums[0]).length();
        long ls[][] = new long[10][len];	//每列存放该位字符（0～9）出现的次数
        for(int i:nums){
            String s = Integer.toString(i);
            for(int j = 0;j < len;j++){
                ls[s.charAt(j)-'0'][j]++;	//遍历
            }
        }
        for(int i = 0;i < len;i++){
            long total = nums.length;
            long temp = 0;
            for(int j = 0;j < 10;j++){
                temp += ls[j][i]*(total-ls[j][i]);	//不同的次数就是自己和别的数的乘积，最后除2，因为“1”和“2”重复会计算两次
            }
            ans += temp/2;
        }
        return ans;
    }
``````

## 三.[查询数组中元素的出现位置](https://leetcode.cn/problems/find-occurrences-of-an-element-in-an-array/)(中等)

1.ArrayList

+ 在Java中，ArrayList是一个动态数组
+ `arraylist.size()`方法求长度
+ `ArrayList.add()`方法追加元素

2.ArrayList转int[]数组

+ ```java
  ArrayList<Integer>list = new ArrayList<>();
  int[] intArray = list.stream().mapToInt(Integer::intValue).toArray();
  ```

``````java
public int[] occurrencesOfElement(int[] nums, int[] queries, int x) {
        ArrayList<Integer> list = new ArrayList<>();	//定义动态数组
        int[] ans = new int[queries.length];
        for(int i  = 0;i <nums.length;i++){
            if(nums[i] == x)
                list.add(i);	//把nums里等于x的元素位置存入动态数组
        }
        int len = list.size();
        for(int i = 0;i < queries.length;i++){
            if(queries[i] > len) ans[i] = -1;	//查询位置超出范围就证明没有那么多元素
            else ans[i] = list.get(queries[i] - 1);	//查询位置即记录的位置，但是查询位置的下标从1开始，需要减1
        }
        return ans;
    }
``````

## 四.[幸福值最大化的选择方案](https://leetcode.cn/problems/maximize-happiness-of-selected-children/)(中等)

1.[我的题解](https://leetcode.cn/problems/maximize-happiness-of-selected-children/solutions/2830766/zui-you-hua-xuan-ze-tan-xin-by-nifty-i3o-tv2z/)

2.调库排序

+ `Arrays.sort(int[])`

``````java
class Solution {
    public long maximumHappinessSum(int[] happiness, int k) {
      //解释在题解
        long ans = 0;
        int n = happiness.length;
        if(k <= Math.log(n)/Math.log(2)){
            for(int i = 0; i < k;i++){
                long max_num = 0;
                int flag = -1;
                for(int j = 0 ;j < n;j++){
                    if(max_num < happiness[j]){
                        max_num = happiness[j];
                        flag = j;
                    }
                }
                if((max_num - i) <= 0) break;
                ans += max_num-i;
                happiness[flag] = 0;
            }
        }
        else{
            Arrays.sort(happiness);
            for(int i = n-1;i > n-1-k;i--){
                int j = n - i - 1;
                if((happiness[i] - j) > 0) ans += (happiness[i] - j);
                else break;
            }
        }
        return ans;
    }
}
``````









