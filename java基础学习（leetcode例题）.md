[TOC]

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

## 五.[448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)（简单）

> 这题我都不想写题解，因为我的解题方法很奇葩，基础版很简单，就是在创建一个数组，长度为nums.length,然后遍历nums，`nums[i] - 1`作为下标索引位置标记新数组（哈希表），最后遍历一遍新数组，看哪个位置没被标记，哪里就是没出现的数字

\- 时间复杂度: *O(n)*

\- 空间复杂度: *O(n)*

+ **but but but，题目说有进阶版，不去额外占用空间**，这我就来劲了，想了好久，写了一坨，惨不忍睹，一般代码写不下去的时候，删掉它！重新写，这是我认为最好的方法
+ 仔细阅读题目条件和数字范围，发现数组数字大小肯定不会大于`nums.length`，那这就好办了，把自己作为哈希表，出现过的数字对应下标对应的位置就加个`nums.length`就好了，然后最后看哪个位置的数字大小小于`nums.length`。（这很绕嘴，不如直接上代码）
+ List是一个接口，官方文档对list的解释`public interface List<E>`，所以初始化时候不能用`List<Integer>ans = new List<>();`必须用`LinkedList`或者`ArrayList`等实现类来初始化。

``````java
public List<Integer> findDisappearedNumbers(int[] nums) {
        int len = nums.length;
        for(int i = 0; i < len; i++){
            nums[(nums[i] - 1)%len] += len;
        }
        List<Integer>ans = new ArrayList<>();
        for(int i = 0; i < len; i++){
            if(nums[i] <= len) ans.add(i + 1);
        }
        return ans;
    }
``````

## 六.[6. Z 字形变换](https://leetcode.cn/problems/zigzag-conversion/)(中等)

> 不要被唬住，很简单一道题，看着复杂，做着简单
>
> 他说是z型，但是不要看成z，看成v就行，每个v是一组
>
> ```ma
> 输入：s = "PAYPALISHIRING", numRows = 4
> 输出："PINALSIGYAHRPI"
> 解释：
> P     I    N
> A   L S  I G
> Y A   H R
> P     I
> ```
>
> 第一个v：PAYPAL
>
> 第二个v：ISHIRI
>
> 第三个v：NG
>
> 看第一行，PIN，也就是s数组第0个、第6个、第12个，至于为什么6倍增长，因为z是四行的时候，每个v大小就是4，我们可以总结规律，LenOfV is (numsRows - 1) * 2
>
> 这样就简单了，第二行就是s的第1个+第5个，然后自增6，第三行同理，最后一行只有v里的一个元素

+ 应避免用String类型进行频繁修改，因为String类型不可修改，如果用Str 1 = Str1 + Str2，会重新给Str1开辟新空间，浪费空间，还费时间，第一次我就用的String作为ans，时间、空间复杂度才超过leetcode提交记录的20%多，但是把String 类型变为StringBuilder类型会节省很多时间、空间，均超过了98%的提交记录

\- 时间复杂度: *O(n)*

\- 空间复杂度: *O(n)*

``````java
public String convert(String s, int numRows) {
        int PerZ = (numRows - 1) * 2;
        int len = s.length();
        StringBuilder ans = new StringBuilder();
        if (PerZ == 0) return s;
        for (int j = 0; j < len; j += PerZ) {
            ans.append(s.charAt(j)) ;
        }
        for (int i = 1; i < PerZ / 2; i++) {
            for (int j = i; j < len; j += PerZ) {
                ans.append(s.charAt(j)) ;
                if (PerZ - i * 2 + j < len) ans.append(s.charAt(PerZ - i * 2 + j));
            }
        }
        for (int j = PerZ / 2; j < len; j += PerZ) {
            ans.append(s.charAt(j));
        }
        return ans.toString();
    }
``````



## 七.[807. 保持城市天际线](https://leetcode.cn/problems/max-increase-to-keep-city-skyline/)(中等)

>贪心，这题不需要考虑下标，因为表格是n*n的，不管横着遍历还是竖着遍历，只要遍历完就行，不用思考下标写的对不对

+ 三目运算符：`条件?a:b`   条件成立执行a，不成立执行b

``````java
class Solution {
    public int maxIncreaseKeepingSkyline(int[][] grid) {
        int len = grid.length;
        int ans = 0;
        int[][] maxArr = new int[2][len];
        for(int i = 0;i < len;i++){	//求出每行、每列的最大值
            for(int j = 0;j < len;j++){
                maxArr[0][i] = maxArr[0][i] < grid[i][j] ? grid[i][j]:maxArr[0][i];
                maxArr[1][i] = maxArr[1][i] < grid[j][i] ? grid[j][i]:maxArr[1][i];
            }
        }
        for(int i = 0; i < len; i++){	//为了不影响原有高度，需要取所在行和列最高值的最小值
            for(int j = 0;j < len;j++){
                ans += maxArr[0][i] < maxArr[1][j]?maxArr[0][i] : maxArr[1][j];
                ans -= grid[i][j];
            }
        }
        return ans;
    }
}
``````

## 八.[100352. 交换后字典序最小的字符串](https://leetcode.cn/problems/lexicographically-smallest-string-after-a-swap/)（简单，406周赛）

- 时间复杂度、空间复杂度击败100%

> 修改字符串某位用`setCharAt`，但是不能是String类型
>
> toString方法可以将StringBuilder转换成字符串

``````JAVA
public String getSmallestString(String s) {
        StringBuilder ans = new StringBuilder(s);
        int len = s.length();
        for(int i = 0; i < len - 1;i++){
            if(s.charAt(i) > s.charAt(i+1) && s.charAt(i)%2 == s.charAt(i+1)%2){
                char temp = s.charAt(i);
                ans.setCharAt(i,ans.charAt(i+1));
                ans.setCharAt(i + 1,temp);
                return ans.toString();
            }
        }
        return ans.toString();
    }
``````

## 九.[100368. 从链表中移除在数组中存在的节点](https://leetcode.cn/problems/delete-nodes-from-linked-list-present-in-array/)(中等、406周赛)

- 时间复杂度、空间复杂度击败100%

> 将数组排序，然后二分查找即可，此时查找数组中存在的时间复杂度为排序用的时间复杂度*o(MAX(m,n)logn)*，如果不排序，顺序查找的话时间复杂度为*O(mn)*
>
> 考察链表基本操作、二分查找

``````JAVA
public boolean inNums(int n,int[] nums){
        int le = 0;
        int ri = nums.length - 1;
        while(le <= ri){
            int mid = (le + ri) / 2;
            if(nums[mid] == n) return true;
            else if(nums[mid] < n) le = mid + 1;
            else ri = mid - 1;
        }
        return false;
    }
    public ListNode modifiedList(int[] nums, ListNode head) {
        Arrays.sort(nums);
        ListNode last = head;
        ListNode nextNode = head.next;
        while(nextNode != null){
            if(inNums( nextNode.val,nums)){
                last.next = nextNode.next;
            }
            else last = nextNode;
            nextNode = nextNode.next;
        }
        if(inNums(head.val,nums)) return head.next;
        else return head;
    }
``````

## 十.[100361. 切蛋糕的最小总开销 I](https://leetcode.cn/problems/minimum-cost-for-cutting-cake-i/description/)（中等、406周赛）

> + 排序：首先，对水平切割线和垂直切割线的位置进行排序，这是为了确保在处理切割时，我们总是按照从低到高的顺序考虑切割线。
> + 双指针：使用两个指针（i_m 和 j_n）来分别追踪当前考虑的水平切割线和垂直切割线。这两个指针从数组的末尾开始向前移动，因为排序后，最大的切割线位置在数组的末尾。
> + 切割成本计算：每次选择切割线时（无论是水平还是垂直），都会计算这条切割线产生的成本。这个成本是通过将切割线的位置值乘以这条切割线将分割出的行数（对于水平切割）或列数（对于垂直切割）加一来得到的。加一是因为每次切割都会新增一个区域。
> + 更新计数器：对于每次选择的切割线，都会更新相应的计数器（HorCnt 表示水平切割的次数，VerCnt 表示垂直切割的次数）。这些计数器用于确定当前切割线将分割出多少个小矩形。
> + 循环终止条件：当两个指针中的任一个到达数组的开头（即没有更多的切割线可以考虑）时，循环终止。然后，处理剩余的切割线（如果有的话），因为它们将只影响一个维度（行或列）。

+ 时间复杂度、空间复杂度击败100%

\- 时间复杂度: *O(nlogn + mlogm)*

\- 空间复杂度: *O(1)*

``````JAVA
public int minimumCost(int m, int n, int[] horizontalCut, int[] verticalCut) {
        Arrays.sort(horizontalCut);
        Arrays.sort(verticalCut);
        int i_m = horizontalCut.length - 1;
        int j_n = verticalCut.length - 1;
        int VerCnt = 0;
        int HorCnt = 0;
        int ans = 0;
        while(i_m >= 0 || j_n >= 0){
            if(i_m < 0){
                while (j_n >= 0){
                    ans += verticalCut[j_n] * (HorCnt+1);
                    j_n--;
                }
                break;
            } else if (j_n < 0) {
                while (i_m >= 0){
                    ans += horizontalCut[i_m] * (VerCnt+1);
                    i_m--;
                }
                break;
            }
            if(horizontalCut[i_m] > verticalCut[j_n]){
                ans += horizontalCut[i_m] * (VerCnt+1);
                HorCnt++;
                i_m--;
            }
            else{
                ans += verticalCut[j_n] * (HorCnt+1);
                VerCnt++;
                j_n--;
            }
        }
        return ans;
``````

## 11.[切蛋糕的最小总开销 II](https://leetcode.cn/problems/minimum-cost-for-cutting-cake-ii/)(困难，406周赛)

>至于这题题干和上面题目有哪里不一样，我始终没找出来，提交了一遍才发现，错误输出是一堆负数，原来是int溢出了，再一看返回值是long，那尝试把ans改成long不就好了，果然，AC

``````java
public long minimumCost(int m, int n, int[] horizontalCut, int[] verticalCut) {
        Arrays.sort(horizontalCut);
        Arrays.sort(verticalCut);
        int i_m = horizontalCut.length - 1;
        int j_n = verticalCut.length - 1;
        int VerCnt = 0;
        int HorCnt = 0;
        long ans = 0;
        while(i_m >= 0 || j_n >= 0){
            if(i_m < 0){
                while (j_n >= 0){
                    ans += verticalCut[j_n] * (HorCnt+1);
                    j_n--;
                }
                break;
            } else if (j_n < 0) {
                while (i_m >= 0){
                    ans += horizontalCut[i_m] * (VerCnt+1);
                    i_m--;
                }
                break;
            }
            if(horizontalCut[i_m] > verticalCut[j_n] ){
                ans += horizontalCut[i_m] * (VerCnt+1);
                HorCnt++;
                i_m--;
            }
            else{
                ans += verticalCut[j_n] * (HorCnt+1);
                VerCnt++;
                j_n--;
            }
        }
        return ans;
    }
``````

## 12.[找到两个数组中的公共元素](https://leetcode.cn/problems/find-common-elements-between-two-arrays/)(简单)

> 哈希表秒了，题干给的范围很小，数字大小只有0到100，数组下标作为哈希的key就行了
>
> java new数组默认值为0

``````JAVA
public int[] findIntersectionValues(int[] nums1, int[] nums2) {
        int hash[][] = new int[2][100];
        int len1 = nums1.length;
        int len2 = nums2.length;
        for(int i = 0; i < len1;i++){
            hash[0][nums1[i] - 1]++;
        }
        for(int i = 0; i < len2;i++){
            hash[1][nums2[i] - 1]++;
        }
        int[] ans = new int[2];
        for(int i = 0; i < 100;i++){
            if(hash[1][i] != 0) ans[0] += hash[0][i];
            if(hash[0][i] != 0) ans[1] += hash[1][i];
        }
        return ans;
    }
``````

## 13.[两数相加](https://leetcode.cn/problems/add-two-numbers/)（中等）

> 时间复杂度超越100%

+ 一次遍历，利用链表的灵活性，可以指向任意结点，短的链表没了直接指向长链表没遍历的部分，不推荐我自己写的代码。。。有点乱
+ Java的链表比c or cpp简单，不需要考虑带不带`*`、`&`等等，不用考虑摆弄寄存器，访问地址什么的，封装好了比较容易理解。

``````java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode ans = l1;
        int upper = 0;
        ListNode last = l1;
        int flag = 1;
        while (true){
            int temp1 = l1 != null ? l1.val:0;
            int temp2 = l2 != null ? l2.val:0;
            l1.val  = ( (temp1 + temp2)/flag + upper);
            upper = l1.val / 10;
            l1.val %= 10;
            if(l1.next == null) {
                flag = 2;
                l1.next = l2.next;
                if (upper == 0) break;
                else {
                    if(l2.next == null)
                        l1.next = new ListNode(0);
                }
            }
            if(l2.next == null){
                flag = 2;
                l2.next = l1.next;
                if (upper == 0) break;
            }
            l1 = l1.next;
            l2 = l2.next;
        }
        return ans;
    }
``````

## 14.[ 长度最小的子数组](https://leetcode.cn/problems/2VG8Kg/)（中等）

> 滑动窗口，相较于官方答案有一些小小的优化，如果确定最小的长度就是 小于等于3了，那就不必考虑子数组长度大于3的情况了，这时候i 和j可以一起向后滑动，不需要先滑一个，再考虑第二个。
>
> 时间复杂度超过100%

+ 常见的系统给定的常量：`Math.PI、Integer.MAX_VALUE/MIN_VALUE等`

``````java
public int minSubArrayLen(int target, int[] nums) {
        int ans = Integer.MAX_VALUE;
        int i = 0, j = 0;
        int len = nums.length;
        int Cnt = 0;
        boolean flag = false;
        while(i < len){
            Cnt += nums[i];
            if(Cnt >= target){
                flag = true;
                ans = Math.min(ans,i - j + 1);
            }
            while(Cnt - nums[j] >= target){
                Cnt -= nums[j++];
                ans = Math.min(ans,i - j + 1);
            }
            if(flag) {
                Cnt -= nums[j++];
            }
            i++;
        }
        if(ans == Integer.MAX_VALUE) return 0;
        return ans;
    }
``````

## 15.[ 得到更多分数的最少关卡数目](https://leetcode.cn/problems/minimum-levels-to-gain-more-points/)(中等)

> int(-3/2) = -1,取的是ceiling，但是int(3/2) = 1,取的又是floor，所以找大于一半的策略时候会出错，故一律采用floor，即`Math.floorDiv`

``````java
public int minimumLevels(int[] possible) {
        int Cnt = 0;
        int len = possible.length;
        for(int i:possible){	//第一次遍历，找到全部走完获得的最大分数
            Cnt += i;
            if(i == 0) Cnt -= 1;
        }
        Cnt = Math.floorDiv(Cnt, 2);
        int CntAlice = 0;
        for(int i = 0; i < len - 1; i++){		//第二次遍历，找到分数大于一半的情况，这样后面路径分数就一定小于一半，这就是最优解
            CntAlice += possible[i];
            if(possible[i] == 0) CntAlice -= 1;
            if(CntAlice > Cnt) return i + 1;
        }
        return -1;
    }
``````



## 16.[删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)(中等)

>双指针，快指针遍历，慢指针做为存储位置

+ `nums[index++] = a`先执行`nums[index] = a`，再去执行自增操作，++在前面就先执行自增

``````java
public int removeDuplicates(int[] nums) {
        int len = nums.length;
        int index = 1;
        int Cnt = 1;
        for(int i = 1; i < len; i++){
            if(nums[i - 1] != nums[i]){
                Cnt = 0;
                nums[index++] = nums[i];
                Cnt++;
            }else{
                if(Cnt < 2){
                    nums[index++] = nums[i];
                    Cnt++;
                }
            }
        }
        return index;
    }
``````

## 17.[环形链表](https://leetcode.cn/problems/linked-list-cycle/)(简单，面试题)

> 快慢指针，因为进入环就会死循环，必须让两个指针都进入环，这样一快一慢，最终肯定会相遇

+ java实现链表简单，不需要考虑指针，地址什么的，面试给空白的调试器实现链表+程序，java比较简单一些，一个List类，其中包括两个字段，一个是val值，一个是List类型，指向next

``````java
public boolean hasCycle(ListNode head) {
        ListNode i = head;
        ListNode j = head;
        while(i != null && j != null){
            if(i.next != null) i = i.next.next;
            else return false;
            j = j.next;
            if(i == j) return true;
        }
        return false;
    }
``````

## 18.[满足距离约束且字典序最小的字符串](https://leetcode.cn/problems/lexicographically-smallest-string-after-operations-with-constraint/)(中等)

> 由于java语言的特性，字符串不能原地修改，每次修改字符串都是在原有的基础上新建一个字符串，然后进行替换操作，而c /c++字符串则可以原地修改，所以我们只好新建一个`StringBuilder`类型来存储返回值
>
> 时间复杂度超越100%
>
> 空间复杂度是依托。。
>
> 这题用到一个贪心 + 类似取模的操作，题干说只有小写字母，大于z就类似取模

+ 在Java中，字符和数字可以相加，结果是一个整数。这是因为Java会将字符自动提升为整数类型（ASCII值），然后进行加法运算。如果结果超过了ASCII码的最大值（127），那么结果将是对应的Unicode字符的码点值。  
+ 例如，字符 'A' 的ASCII值是65，如果我们将其与数字60相加，结果将是125，这是一个有效的ASCII值。但是，如果我们将 'A' 与数字70相加，结果将是135，这超过了ASCII的最大值，但是它是一个有效的Unicode码点值，对应于某个Unicode字符。

``````java
public String getSmallestString(String s, int k) {
        StringBuilder ans = new StringBuilder();
        int len = s.length();
        for (int i = 0; i < len; i++) {
            if(k > 0) {
                if (s.charAt(i) + k > 'z') {
                    ans.append('a');
                } else {
                    ans.append(s.charAt(i) - k < 'a' ? 'a' : (char) (s.charAt(i) - k));
                }
                int temp = Math.abs(s.charAt(i) - ans.charAt(i));
                k -= 26 - temp < temp ? 26 - temp : temp;
            }
            else{
                ans.append(s.substring(i));
                break;
            }
            
        }
        return ans.toString();
    }
``````

## 19.[找到 K 个最接近的元素](https://leetcode.cn/problems/find-k-closest-elements/)(中等)

> 二分查找+滑动窗口（双指针）
>
> 首先利用二分查找找到大致位置（arr[pos] 要么等于x，要么大于k且arr[pos - 1] 小于 x），然后选取大致位置的左面k个，窗口大小为k，然后进行滑动，如果右侧有最优解，就把左面舍弃
>
> 确保二分查找下标正确

\- 时间复杂度: *O(MAX(k, logn))*

\- 空间复杂度: *O(1)*

``````java
public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int le = 0;
        int ri = arr.length - 1;
        int mid;
        int pos = -1;
        while(le < ri){		//二分查找大致位置
            mid = (ri - le)/2 + le;
            if(arr[mid] < x) le = mid + 1;
            else if(arr[mid] > x) ri = mid - 1;
            else{
                pos = mid;
                break;
            }
        }
        if(pos == -1) pos = ri;
        if(pos - k < 0){		//判断左面是否有k个元素，如果没有就从0开始
            le = 0;
            ri = k - 1;
        }else{
            le = pos - k;
            ri = pos - 1;
        }
        while(ri != arr.length - 1){		//进行滑动，如果右侧有最优解，就把左面舍弃
            if(Math.abs(arr[le] - x) > Math.abs(arr[ri + 1] - x)){
                le++;
                ri++;
            }else{
                break;
            }
        }
        List<Integer> ans = new ArrayList<>();
        for(int i = le; i <= ri;i++){
            ans.add(arr[i]);
        }
        return ans;
    }
``````

## 20.[棒球比赛](https://leetcode.cn/problems/baseball-game/)(简单)

> 简单题就用简单题的思路，大部分简单题都是按照题干实现就行，不需要考虑太多问题，本题利用了栈的思想，可以模拟栈，当然一看是简单题，也可以用自带的栈实现
>
> if - else if - else 情况多的时候可以用switch-case
>
> `Integer.valueOf()`和`String.valueOf`一起记有奇效，string转int就用Integer的valueOf，int转string就是Sting下的valueOf
>
> 可以使用java 的for-each循环
>
> ``````java
> for (Type item : collection) {
>     // 使用item进行操作
> }
> ``````

``````java
public int calPoints(String[] operations) {
        Stack<String>stk = new Stack<>();
        for(String s:operations){
            if(s.equals("+")) {
                String up = stk.pop();
                String down = stk.pop();
                stk.push(down);
                stk.push(up);
                int ans = Integer.valueOf(up)+Integer.valueOf(down);
                stk.push(String.valueOf(ans));
            }
            else if(s.equals("C")) stk.pop();
            else if(s.equals("D")){
                String temp = stk.pop();
                int ans = Integer.valueOf(temp);
                ans*=2;
                stk.push(temp);
                stk.push(String.valueOf(ans));
            }
            else stk.push(s);
        }
        int ans = 0;
        while(!stk.empty()){
            ans += Integer.valueOf(stk.pop());
        }
        return ans;
    }
``````



## 21.[统计同质子字符串的数目](https://leetcode.cn/problems/count-number-of-homogenous-substrings/)(中等)

> `1e9 + 7`是浮点型，得转换成int型，不然不能取模
>
> 这题不需要考虑字符串内容是什么，只需要考虑个数，`aabbbcccc`我们可以分成"aa","bbb","cccc"，将相同的分成一组，两个相同的可以划分为3个同质子串`a,a,aa`
>
> aaaa的同质子串个数，我们发现是1+2+3+4，恰好和每次访问字符串时候的cnt一致，所以只需要一次遍历即可。

``````java
public int countHomogenous(String s) {
        int ans = 0;
        int len = s.length();
        char lastCh = s.charAt(0);
        int cnt = 0;
        for(int i = 0; i < len; i++){
            char c = s.charAt(i);
            if(lastCh == c){
                cnt++;
            }else{
                cnt = 1;
                lastCh = c;
            }
            ans = (ans + cnt)%(int)(1e9 + 7);
        }
        return ans;
    }
``````

## 22.[替换隐藏数字得到的最晚时间](https://leetcode.cn/problems/latest-time-by-replacing-hidden-digits/)(简单)

> 把所有情况列出来就行，做个简单题放松一下
>
> 注意小时的第一个数和第二个数对应关系，第一个数是2，第二个数最大就是3，第二个数比3大，那第一个数最大就是1

`````java
public String maximumTime(String time) {
        StringBuilder ans = new StringBuilder();
        for(int i = 0; i < 5; i++){
            if(time.charAt(i) == '?'){
                if(i == 0){
                    if(time.charAt(i + 1) >= '4' && time.charAt(i + 1) <= '9') ans.append('1');
                    else ans.append('2');
                }else if(i == 1){
                    if(ans.charAt(0) == '2') ans.append('3');
                    else ans.append('9');
                }else if(i == 3) ans.append('5');
                else ans.append('9');
            }else ans.append(time.charAt(i));
        }
        return ans.toString();
    }
`````

## 23.[完成所有任务的最少初始能量](https://leetcode.cn/problems/minimum-initial-energy-to-finish-tasks/)（困难）

> `(o1, o2) -> (o2[1]-o2[0])-(o1[1]-o1[0])`Lambda 表达式，也称匿名函数，类似于JavaScript的箭头函数
>
> 可以用重写比较器来实现
>
> ``````java
> Comparator<int[]>comparator = new Comparator<int[]>() {
>             @Override
>             public int compare(int[] o1, int[] o2) {
>                 return Integer.compare(o2[1] - o2[0], o1[1] - o1[0]);
>             }
>         };
> Arrays.sort(tasks, comparator);
> ``````
>
> 实测，lambda表达式会比重写比较器快，来自github copilot解释：
>
> 原因主要有两点：
>
> + 语法简洁：Lambda表达式的语法更加简洁，没有额外的类定义和方法定义，因此在解析和编译时可能更快。  
> + 运行时优化：Java运行时对Lambda表达式进行了优化。Lambda表达式在Java 8及以后的版本中被引入，这些版本的Java运行时包含了对Lambda表达式的优化，例如逃逸分析和即时编译（JIT）等技术，可以提高Lambda表达式的执行效率。
>
> 题目：排序+贪心
>
> 首先按照“最贪的人”从大到小排序，这个最贪的人指的是申请的最低能量高，但是实际能量又很少的人，例如[[1,1], [1,3]]，第二个人就比第一个人贪，它明明只需要1个能量，但是却申请三个，此时多出了两个可以给第一个人使用。如果第一个人先执行，那一共就需要四个能量，最后还余出来俩能量，非常的浪费！但是如果先执行第二个人的，那最后就需要三个能量，最后剩一个能量不可避免。

``````java
public int minimumEffort(int[][] tasks) {
        Arrays.sort(tasks, (o1, o2) -> (o2[1]-o2[0])-(o1[1]-o1[0]));
        int len = tasks.length;
        int ans = 0;
        int cur = 0;
        for (int i = 0; i < len; i++){
            int temp = (tasks[i][1] - cur) > 0 ? (tasks[i][1] - cur) : 0;
            ans += temp;
            cur += temp;
            cur -= tasks[i][0];
        }
        return ans;
    }
``````

## 24.[分割字符串的方案数](https://leetcode.cn/problems/number-of-ways-to-split-a-string/)(中等)

> 那个类型转换搞了我好一阵，对于这两个表达式：
>
> 1. **`(int)((long)(le+1)(ri+1)%MOD)`**
>
>    - 这里 `(le+1)` 和 `(ri+1)` 都先被提升为 `long` 类型。
>    - 然后两者相乘 `(long)(le+1) * (ri+1)`，结果是 `long` 类型，不容易溢出。
>    - 接着 `%MOD` 操作也在 `long` 范围内进行，防止了溢出。
>
> 2. **`(int)((long)((le+1)(ri+1))%MOD)`**
>
>    - 这里 `((le+1)(ri+1))` 是先在默认的 `int` 范围内进行乘法运算。
>    - 如果结果超出了 `int` 的范围，就会溢出，即使之后转换为 `long` 也无法补救。
>    - 之后的 `%MOD` 作用于已经溢出的值，因此结果是不正确的。
>
> 要避免溢出，可以确保在乘法之前就将操作数提升到足够大的数据类型。
>
> 这题不难，首先判断字符串中“1”的个数能否被3整除，不能肯定不行
>
> 然后如果全是0，就变成高中的排列组合问题了，在0000空隙插入两个隔板，即C(3,2)
>
> 最后也是隔板问题0110010100011，先分成“011”，“00“，”101”，“000”，“11”，分成五份，意思是第1,3,5份是不可分割部分，2,4块可分割，因为2,4块在中间，两个隔板一个放在第二块，一个放在第四块，隔板可以放在该块的最左面或者最右面，即C(2+1,1)*C(3+1,1);

`````java
static final int MOD = 1000000007;
    public int numWays(String s) {
        int ans = 0;
        int CntOfOne = 0;
        int len = s.length();
        for(int i = 0; i < len; i++){
            if(s.charAt(i) == '1') CntOfOne++;
        }
        if(CntOfOne % 3 == 0 && len >= 3){
            if(CntOfOne == 0) return (int) ((long) (len - 1) * (len - 2) / 2 % MOD);
            int temp = 0;
            int le = 0;
            int ri = 0;
            for(int i = 0; i < len; i++){
                if(s.charAt(i) == '1') temp++;
                else{
                    if(temp == CntOfOne / 3) le++;
                    else if(temp == 2 * CntOfOne / 3) ri++;
                }
            }
            return (int)((long)(le+1)*(ri+1)%MOD);
        }
        return 0;
    }
`````

## 25.[ 双模幂运算](https://leetcode.cn/problems/double-modular-exponentiation/)(中等)

> 这道题很有意思，其实最主要是发现` (a * b)%p =(a%p * b%p)%p`，发现了这个int才不会溢出，还需要自己写幂函数，而且需要快速幂，例如3^7 可以分为 3^1 * 3^2 * 3^4，3^9 可以分为 3^1 * 3^2 * 3^4 *(3 * 3）
>
> \- 时间复杂度: *O(nlogm)*

``````java
public int myPow(int a,int b,int mod){
        int ans = a % mod;
        int cnt = 1;
        int i = 1;
        while(true){
            if(i * 2 >= b){
                for(int j = i;j < b;j++) ans = (ans*a)%mod;
                break;
            }
            ans = (ans*ans)%mod;
            cnt += i;
            i *= 2;
        }
        return ans;
    }
    public List<Integer> getGoodIndices(int[][] variables, int target) {
        List<Integer>ans = new ArrayList<>();
        int i = -1;
        for(int[] arr:variables){
            i++;
            if(arr[3] <= target) continue;	//如果m比target还小，那取模m后肯定不会大于m，也不可能得出target
            int val = myPow(myPow(arr[0],arr[1],10),arr[2],arr[3]);
            if(val == target) ans.add(i);
        }
        return ans;
    }
``````

## 26.[颜色分类](https://leetcode.cn/problems/sort-colors/)(中等)

> 这是一道典型的双指针问题
>
> 这是三个数字的问题，思考，如果简化成只有0和1怎么解决排序问题，利用两个指针，一头一尾，尾指针是1，就向前移动，头指针只有是0的时候才向后移动，如果头指针不为0且尾指针为0就交换，这样我们便得到左面全0右面全1的数组
>
> 同理，我们如果先把1和2都考虑成1，那就变成了上面的问题了，首先我们假设[2, 1, 0, 0, 2, 1]，我们可以先设法把所有的0放在最左面，
>
> `[0, 1, 0, 2, 2, 1]`  *交换0号位和3号位*
>
> `[0, 0, 1, 2, 2, 1]`  *交换1号位和2号位*
>
> 此时我们发现，所有的0，都跑到了最左面，同理，我们可以对后面的1和2使用相同方法
>
> \- 时间复杂度: *O(n)*

``````java
public void swap(int[]nums, int i ,int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    public void sortColors(int[] nums) {
        int i = 0;
        int j = nums.length - 1;
        while(i < j){
            while(i < nums.length && nums[i] == 0) i++;
            while(j >=0 && nums[j] != 0) j--;
            if(i < j)
                swap(nums,i,j);
        }
        j = nums.length - 1;
        while(i < j){
            while(i < nums.length && nums[i] == 1) i++;
            while(j >= 0 && nums[j] == 2) j--;
            if(i < j)
                swap(nums,i,j);
        }
    }
``````

## 27.[划分字母区间](https://leetcode.cn/problems/partition-labels/)(中等)

>经典的贪心算法
>
>我们需要记录每个字符第一次出现的位置，存到hash里，例如`abca`，a第一次出现的位置是0，b是1，c是2，下一个a不用管，因为之前已经出现过
>
>做好准备工作后我们需要从后往前遍历字符串，例如`aabcbdc`，最后一个是c，但是第一次出现c的位置并不在最后，所以我们知道最后一个子串至少是第一次出现c到最后一个c那么长（记为index）
>
>然后看倒数第二个d，是第一次出现，不用管。
>
>倒数第三个是b，不是第一次出现，那我们就更新子串长度到`min（第一次出现b的位置，index）`
>
>一直遍历到index，子串里如果有不满足条件的情况index会一直往前走
>
>满足条件这就可以被切割了。
>
>同理，依次找到最小的满足条件的子串
>
>\- 时间复杂度: *O(n)*

``````java
public List<Integer> partitionLabels(String s) {
        List<Integer> ans = new ArrayList<>();
        int[] hash = new int[26];
        int len = s.length();
        for(int i = 0; i < len; i++)
            if(hash[s.charAt(i) - 'a'] == 0 && s.charAt(i) != s.charAt(0)) hash[s.charAt(i) - 'a'] = i;
        int index = len - 1;
        int piece = 1;
        for(int i = len - 1; i >= 0;i--){
            if(i == index && hash[s.charAt(i) - 'a'] == i) {
                ans.add(piece);
                index--;
                piece = 1;
            }else{
                index = Math.min(index, hash[s.charAt(i) - 'a']);
                piece++;
            }
        }
        return ans.reversed();
    }
``````

## 28.[覆盖所有点的最少矩形数目](https://leetcode.cn/problems/minimum-rectangles-to-cover-points/)（中等）

> 排序+贪心
>
> 首先可以看题干，既然只统计矩形的个数，那就不必考虑矩形的高，既然无论如何都要花销一个矩形，那何不按照最宽取，这样还能包括尽可能多的点。
>
> 我们进行排序，将x点从小到大排序即可，每花费一个矩形，我们就把所有在这个矩形的点囊括进去，一旦出现不在这个矩形里的，就新增一个矩形即可
>
> lambda表达式：`(o1,o2)->(o1[0]-o2[0])`，意思是排序时按照数组的第一个元素大小排序
>
> + 第一次提交，sort部分`Arrays.sort(points, Comparator.comparingInt(o -> o[0]))`时间复杂度超越16.62%
> + 第二次提交，改成`Arrays.sort(points, (o1,o2)->(o1[0]-o2[0]));，超越59%
> + 第三次提交，把for-each语句`for(int[]arr : points)`改成下标索引的方式，超越93.40%
>
> 也不知道这样优化有没有意义，但是看着好看（doge）

``````java
public int minRectanglesToCoverPoints(int[][] points, int w) {
        int ans = 0;
        Arrays.sort(points, (o1,o2)->(o1[0]-o2[0]));
        int index = points[0][0];
        ans++;
        int len = points.length;
        for(int i = 0; i < len; i++){
            if(points[i][0] > index + w){
                index = points[i][0];
                ans++;
            }
        }
        return ans;
    }
``````

## 29[找出分区值](https://leetcode.cn/problems/find-the-value-of-the-partition/)(中等)

> 又是一道想出来规律就简单的题，不管怎么样，只要找到数组中a - b相差最小的差值，就是结果，因为比b小可以放在b左面，比a大可以放在a右面
>
> 排序即可
>
> 超越100%时间复杂度

``````java
public int findValueOfPartition(int[] nums) {
        Arrays.sort(nums);
        int len = nums.length;
        int minNum = (int)1e9;
        for(int i = 1; i < len; i++){
            if(nums[i]-nums[i - 1] < minNum) minNum = nums[i]-nums[i - 1];
            if(minNum == 0) return 0;
        }
        return minNum;
    }
``````

## 30.[心算挑战](https://leetcode.cn/problems/uOAnQW/)(简单)

> 排序＋贪心
>
> 其实看到题干要求，说数字范围0 到 1000，其实用哈希也行，1000很小了
>
> 先排序然后把最大的cnt个数字相加，还要把cnt个数字里最小的奇数和偶数挑出来，方便替换，用cnt里最小的数替换cards其他数字的最大值，这才是最贪的方案，如果和是奇数，那就把除了cnt个数的cards里的最大的奇数和偶数挑出来，看把cnt里的奇数替换成偶数代价小还是偶数换奇数代价小
>
> 需要考虑边界情况和特殊情况
>
> 例如[1,1,1,1,1] , cnt = 3就是特殊情况，没有可替换的偶数

``````java
public int maxmiumScore(int[] cards, int cnt) {
        int ans = 0;
        Arrays.sort(cards);
        int len = cards.length;
        int odd = 0;
        int even = 0;
        for(int i = len - cnt; i < len; i++){
            if(even == 0 && cards[i] % 2 == 0) even =cards[i];
            if(odd == 0 && cards[i]%2 == 1) odd = cards[i];
            ans += cards[i];
        }
        if(ans % 2 == 1){
            int even1 = 0;
            int odd1 = 0;
            for(int i = len - cnt - 1; i >= 0; i--){
                if(even1 == 0 && cards[i] % 2 == 0)
                    even1 = cards[i];
                if(odd1 == 0 && cards[i] % 2 == 1)
                    odd1 = cards[i];
                if(even1 != 0 && odd1 != 0)
                    break;
            }
            if((even == 0 && even1 == 0) || (even1 == 0 && odd1 == 0)||(even == 0 && odd == 0))
                return 0;
            else{
                if(even == 0) ans += even1 - odd;
                else ans += Math.max(odd1 - even ,even1 - odd);
            }
        }
        return ans;
    }
``````

## 31.[缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/)(困难)

> 第一次秒困难题，时间复杂度超越100%
>
> \- 时间复杂度: *O(n)*
>
> \- 空间复杂度: *O(1)*
>
> 题干限制很多，时间复杂度o(n)，说明不让排序，不让暴力
>
> 空间复杂度o(1)，说明不让开辟新数组
>
> 我的方法是置换，例如[3,4,-1,1]，从头遍历，第一个数是3，就把`nums[3-1]`和第一个数置换，此时`nums[3-1]`已经正确，我之所以减一，是因为找最小正整数，然后再看换好的第一个数，是-1，此时≤0，或者大于数组长度的都不需要考虑，一律置0就好，至于为什么大于数组长度的数不考虑，因为肯定有比这个数小的正整数。
>
> 然后i++，以此类推，直到把所有数都归位，最后扫描一遍，如果哪个数不对位，就缺这个数。

``````java
public int firstMissingPositive(int[] nums) {
        int len = nums.length;
        int i = 0;
        while(i < len){
            if(nums[i] > len || nums[i] <= 0){
                nums[i] = 0;
                i++;
            }else if(i + 1 == nums[i]){
                i++;
            }else{
                if(nums[nums[i] - 1] == nums[i]) i++;
                else{
                    int temp = nums[nums[i] - 1];
                    nums[nums[i] - 1] = nums[i];
                    nums[i] = temp;
                }
            }
        }
        for(i = 0; i < len; i++){
            if(nums[i] != i + 1) return i + 1;
        }
        return len + 1;
    }
``````



## 32.[直角三角形](https://leetcode.cn/problems/right-triangles/)（中等）

> 时间复杂度超过O(MN)就会超时
>
> 第一次遍历为了记录每行多少1和每列多少1
>
> 第二次遍历只需要找到直角三角形的直角即可，然后`(arrCol[j] - 1)*(arrRow[i] - 1)`即为这个直角顶点组成的三角形数目，相加即可

``````java
public long numberOfRightTriangles(int[][] grid) {
        long ans = 0;
        int hei = grid.length;
        int wid = grid[0].length;
        int[] arrRow = new int[hei];
        int[] arrCol = new int[wid];
        for(int i = 0; i < hei; i++)
            for(int j = 0; j < wid; j++){
                if(grid[i][j] == 1) {
                    arrRow[i]++;
                    arrCol[j]++;
                }
            }
        for(int i = 0; i < hei; i++){
            for (int j = 0; j < wid; j++){
                if(grid[i][j] == 1){
                    ans += (arrCol[j] - 1)*(arrRow[i] - 1);
                }
            }
        }
        return ans;
    }
``````

## 33.[ 正方形中的最多点数](https://leetcode.cn/problems/maximum-points-inside-the-square/)(中等)

> 做完题看官方题解，有个好听的名字，叫`维护次小半径`，我觉得就是贪心
>
> 有个概念叫切比雪夫距离：max(∣*x*∣,∣*y*∣)，不难理解0到points[i]的切比雪夫距离就是包括该点的最小正方形的二分之一边长，中心点固定在（0,0），所以就能画出正方形
>
> 题干说只有小写字母，我们该形成条件反射--哈希表，题干还说不重复，那正方形里最多有26个点
>
> 那我们就能想到，如果有两个a，那我们正方形的最大情况就是把切比雪夫距离大的那个a去掉，例如（1,1）和（4,4），那最大三角形四个边就是（±3，±3），因为如果比三还大，那就囊括（4,4）了，这就叫`次小半径`
>
> 有个坑，就是数组初始值是0，如果是(0,0)，那就毁了，这个点让我吃了一下罚时，所以我们得吧初始值设置成-1

``````java
public int getMax(int a,int b){
        return Math.max(Math.abs(a),Math.abs(b));
    }
    public int maxPointsInsideSquare(int[][] points, String s) {
        int[] hash = new int[26];
        Arrays.fill(hash,-1);
        int len = s.length();
        int maxNum = (int)1e9;
        int ans = 0;
        for(int i = 0; i < len; i++){
            int temp = getMax(points[i][0],points[i][1]);
            if(hash[s.charAt(i) - 'a'] != -1){
                maxNum = Math.min(maxNum,Math.max(temp,hash[s.charAt(i) - 'a']) - 1);
                hash[s.charAt(i) - 'a'] = Math.min(temp,hash[s.charAt(i) - 'a']);
            }else{
                hash[s.charAt(i) - 'a'] = temp;
            }
        }
        for(int i = 0; i < 26; i++){
            if(hash[i] <= maxNum && hash[i] != -1) ans++;
        }
        return ans;
    }
``````

## 34.[二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)（简单，热题100）

> 用队列实现层序遍历（广度优先搜索，BFS）我是利用空指针代表分层
>
> 示例1队列表示：3、null、9、20、null、15、7
>
> 显然用几个null分隔就几层
>
> Java的队列要用`LinkedList<>()`初始化
>
> `add`向队尾加入元素
>
> `remove`将队头元素删除，并返回队头元素
>
> `peek`返回队头元素

``````java
public int maxDepth(TreeNode root) {
        int ans = 1;
        Queue<TreeNode>que = new LinkedList<>();
        que.add(root);
        if(root == null) return 0;
        boolean flag = true;
        while(flag){
            que.add(null);
            flag = false;
            while (que.peek() != null){
                TreeNode temp = que.remove();
                if(temp.left != null) {
                    que.add(temp.left);
                    flag = true;
                }
                if(temp.right != null) {
                    que.add(temp.right);
                    flag = true;
                }
            }
            if(flag) ans++;
            que.remove();
        }
        return ans;
    }
``````

## 35.[相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)(简单，热题100)

> 思考，有相交是什么情况，A和B从等长位置开始遍历，例如lenA = 5，lenB = 6，那从A = 0，B = 1的位置开始遍历即可，这样才会有可能相遇
>
> 至于为什么从等长位置遍历，我的思路是：从后往前推，但是这是单向链表，所以排除掉从后往前推的可能，那我们就从短链表的最开始遍历即可，因为超出短短链表的部分肯定不会有重合
>
> \- 时间复杂度: *O(m + n)*
>
> \- 空间复杂度: *O(1)*

``````java
 public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = 0,lenB = 0;
        ListNode tempA = headA,tempB = headB;
        while(tempA != null){
            tempA = tempA.next;
            lenA++;
        }
        while(tempB != null){
            tempB = tempB.next;
            lenB++;
        }
        tempA = headA;
        tempB = headB;
        while(lenA > lenB){
            tempA = tempA.next;
            lenA--;
        }
        while(lenB > lenA){
            tempB = tempB.next;
            lenB--;
        }
        while(tempB != tempA){
            tempB = tempB.next;
            tempA = tempA.next;
        }
        return tempA;
    }
``````

## 36[反转链表](https://leetcode.cn/problems/reverse-linked-list/)(简单，热题100)

> 经典数据结构基础题，java对类的引用是地址引用，也就是修改`ListNode ans`的字段值之后，之前在别处引用的`temp = ans`的temp字段也会改变,这点我觉得不如c语言清晰（虽然指针很繁琐）

``````java
public ListNode reverseList(ListNode head) {
        if(head == null) return head;
        ListNode ans = head,temp = head.next;
        ans.next = null;
        while(temp != null){
            ListNode temp1 = ans;
            ListNode temp2 = temp.next;
            ans = temp;
            temp.next = temp1;
            temp = temp2;
        }
        return ans;
    }
``````

## 37.[不相交的线](https://leetcode.cn/problems/uncrossed-lines/)(中等)

> 经典动态规划，nums1的第i位和nums2的第j位进行比较时候，要么取nums1前i-1位和nums2前j位的最优解，要么取nums1前i位和nums2前j-1位的最优解，要么取nums1前i -1位和nums2前j-1位加上i,j本位的最优解
>
> 状态转移方程：

$$
dp[i][j] = 
 \begin{cases}
 dp[i-1][j-1] + 1,\,\,nums1[i] == nums2[j]\\
 MAX(dp[i-1][j],dp[i][j-1]),\,\,nums1[i] != nums2[j]\\
 \end{cases}
$$

``````java
public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int len1 = nums1.length;
        int len2 = nums2.length;
        int[][] dp = new int[len1 + 1][len2 + 1];
        for(int i = 0; i < len1; i++){
            for(int j = 0; j < len2; j++){
                int temp = 0;
                if(nums1[i] == nums2[j]) temp = 1;
                dp[i + 1][j + 1] = Math.max(Math.max(dp[i][j + 1], dp[i + 1][j]) ,dp[i][j] + temp);
            }
        }
        return dp[len1][len2];
    }
``````

## 38.[实现一个魔法字典](https://leetcode.cn/problems/implement-magic-dictionary/)（中等）

> 为什么java暴力解法时间复杂度会超过100%，而优化的解法反而更慢
>
> 判断两个字符串长度是否相等，相等则判断有几个不同，有超过一个不同就不匹配，要是只有一个不同就返回true即可

``````java
class MagicDictionary {

    String[] dict;

    public MagicDictionary() {
    }

    public void buildDict(String[] dictionary) {
        this.dict = dictionary;
    }

    public boolean search(String searchWord) {
        for(String s:dict){
            if(s.length() == searchWord.length()){
                int cnt = 0;
                for(int i = 0; i < s.length(); i++){
                    if(s.charAt(i) != searchWord.charAt(i)) cnt++;
                    if(cnt > 1) break;
                }
                if(cnt == 1) return true;
            }
        }
        return false;
    }
}

``````

## 39.[实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)（中等）

> 上面的题暴力做的有点心虚，补一道字典树的题，字典树是一个冷门的概念，难倒不难，就是多叉树的变形，但是没听过这个概念会懵。
>
> 每个结点有27个地址，0到25代表字母，存储下一个指向，26代表是否截止，例如`abc`，node.next[0]指向新的node，新node.next[1]指向另一个新的node，以此类推，直到最后，c字母指向一个新的node，这个新的node只有第26位有指向，也就是`temp.next[26] = new Node();`这样就能判断是否截止
>
> 判断前缀还是完全包含只需要判断是不是在遍历完word后面就截止了

``````java
class Node{
    Node[] next;
    Node(){
        next = new Node[27];
    }
}
class Trie {
    Node root;
    public Trie() {
        root = new Node();
    }

    public void insert(String word) {
        int len = word.length();
        Node temp = root;
        for(int i = 0; i < len; i++){
            if(temp.next[word.charAt(i) - 'a'] == null){
                temp.next[word.charAt(i) - 'a'] = new Node();
            }
            temp = temp.next[word.charAt(i) - 'a'];
        }
        temp.next[26] = new Node();
    }

    public boolean search(String word) {
        Node temp = root;
        int len = word.length();
        for(int i = 0; i < len; i++){
            if(temp.next[word.charAt(i) - 'a'] == null) return false;
            temp = temp.next[word.charAt(i) - 'a'];
        }
        if(temp.next[26] == null) return false;
        return true;
    }

    public boolean startsWith(String prefix) {
        Node temp = root;
        int len = prefix.length();
        for(int i = 0; i < len; i++){
            if(temp.next[prefix.charAt(i) - 'a'] == null) return false;
            temp = temp.next[prefix.charAt(i) - 'a'];
        }
        return true;
    }
}
``````

## 40.[岛屿数量](https://leetcode.cn/problems/number-of-islands/)(中等，热题100)

> DFS，用栈记录一个岛屿中为陆地的下标，如示例1：
>
> ```java
> 输入：grid = [
>   ["1","1","1","1","0"],
>   ["1","1","0","1","0"],
>   ["1","1","0","0","0"],
>   ["0","0","0","0","0"]
> ]
> 输出：1
> ```
>
> 从（0，0）开始，这个位置是1，那相邻的上下左右有1就证明和他是一个岛屿，入栈，然后一个个出栈，出栈的元素周围如果有1则接着入栈，直到栈从有元素到空，则是一个岛屿

``````java
public int numIslands(char[][] grid) {
        int cnt = 0;
        int hei = grid.length;
        int wid = grid[0].length;
        for(int i = 0; i < hei; i++){
            for(int j = 0; j < wid; j++){
                if(grid[i][j] == '1'){
                    cnt++;
                    Stack<int[]> stk = new Stack<>();
                    int[] temp = new int[]{i , j};
                    stk.push(temp);
                    while(!stk.isEmpty()){
                        grid[temp[0]][temp[1]] = '0';
                        if(temp[0] > 0 && grid[temp[0] - 1][temp[1]] == '1') stk.push(new int[]{temp[0] - 1, temp[1]});
                        if(temp[1] > 0 && grid[temp[0]][temp[1] - 1] == '1') stk.push(new int[]{temp[0], temp[1] - 1});
                        if(temp[0] < hei - 1 && grid[temp[0] + 1][temp[1]] == '1') stk.push(new int[]{temp[0] + 1, temp[1]});
                        if(temp[1] < wid - 1 && grid[temp[0]][temp[1] + 1] == '1') stk.push(new int[]{temp[0], temp[1] + 1});
                        temp = stk.pop();
                    }
                }
            }
        }
        return cnt;
    }
``````

## 41.[螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)（中等，热题100）

> 这没什么好说的，就是下标别记错了就行
>
> direct记录方向， (int)direct/4作为遍历限制，因为除了第一圈，其余圈都不能遍历到最边缘，不然就错了
>
> `for(;y < col - temp; y++,i++) ans.add(matrix[x][y]);`后面进行`y--`是因为for循环会把y自增到`y = col - temp`，下一次执行时候会越界

``````java
 public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> ans = new ArrayList<>();
        int direct = 0;     //direct % 4 == 0 is to right, 1 is to down, 2 is to left ...
        int i = 0, len = matrix.length * matrix[0].length;
        int row = matrix.length;
        int col = matrix[0].length;
        int x = 0, y = 0;
        while(i < len){
            int temp = (direct + 1) / 4;
            if(direct % 4 == 0){
                for(;y < col - temp; y++,i++) ans.add(matrix[x][y]);
                y--;
                x++;
            } else if (direct % 4 == 1) {
                for(;x < row - temp; x++,i++) ans.add(matrix[x][y]);
                x--;
                y--;
            }else if (direct % 4 == 2) {
                for(;y >= 0 + temp; y--,i++) ans.add(matrix[x][y]);
                y++;
                x--;
            }else {
                for(;x >= 0 + temp; x--,i++) ans.add(matrix[x][y]);
                x++;
                y++;
            }
            direct++;
        }
        return ans;
    }
``````

## 42. [特殊数组 I](https://leetcode.cn/problems/special-array-i/)(简单)

> 这每日一题没什么说的，判断i和i-1的元素奇偶性是否相等即可

``````java
 public boolean isArraySpecial(int[] nums) {
        int len =nums.length;
        for(int i = 1; i < len; i++){
            if(nums[i] % 2 == nums[i - 1] % 2) return false;
        }
        return true;
    }
``````

## 43.[最小路径和](https://leetcode.cn/problems/minimum-path-sum/)(中等，面试经典150)

> 经典二维动态规划
>
> 状态转移方程：
> $$
> dp[i][j] = 
> dp[i][j] + min(dp[i - 1][j], dp[i][j - 1])
> $$
> 我们看[[1,3],[1,5]]这个数组，移动到（0,1）是不是只能是1+3，那我们可以把（0,1）位置记录为1+3即4
>
> 移动到（1,0）只能是1+1，把（1,0）记录为2
>
> 移动到（1,1）要么是（1,0）->（1,1），要么是（0,1）->（1,1），那我们取最小值即可，为2 + 5 = 7，即答案

``````java
public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        for(int i = 1; i < n; i++) grid[0][i] += grid[0][i - 1];
        for(int i = 1; i < m; i++) grid[i][0] += grid[i - 1][0];
        for(int i = 1; i < m; i++)
            for(int j = 1; j < n; j++){
                grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);
            }
        return grid[m - 1][n - 1];
    }
``````

## 44.[矩阵中的最大得分](https://leetcode.cn/problems/maximum-difference-score-in-a-grid/)(中等)

> 不要把这道题想的太复杂，这步那步的
>
> 思考：4 -> 12 和4 -> 8 -> 12和4 -> 1000000 ->12的结果是不是一样，答案是一样，所以我们只需在数组中找出俩数，第二个数在第一个数的右 or 下，第二个数减第一个数得到的答案最大，就ok了
>
> 还是二维dp（动态规划），状态转移方程：
> $$
> dp[i][j] = 
> min(dp[i - 1][j], dp[i][j - 1])
> $$
> 我们在状态转移时候得先维护ans，即先`ans = max(ans, dp[i][j] - temp);`其中temp就是上面状态转移方程的值
>
> 我们需要知道每次状态转移的意义，`dp[i][j]`的意义是前i行j列的矩阵的最小值是`dp[i][j]`
>
> 我做时候懒得把List换成数组了，结果写一半搞得我要累死，什么get什么set的好麻烦的

`````java
public int maxScore(List<List<Integer>> grid) {
        int ans = (int)(-1e9);
        int m = grid.size();
        int n = grid.get(0).size();
        for(int i = 1; i < n; i++){
            ans = Math.max(ans, grid.get(0).get(i) - grid.get(0).get(i - 1));
            grid.get(0).set(i, Math.min(grid.get(0).get(i), grid.get(0).get(i - 1)));
        }
        for(int i = 1; i < m; i++){
            ans = Math.max(ans, grid.get(i).get(0) - grid.get(i - 1).get(0));
            grid.get(i).set(0, Math.min(grid.get(i).get(0), grid.get(i - 1).get(0)));
        }
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                int temp = Math.min(grid.get(i).get(j - 1), grid.get(i - 1).get(j));
                ans = Math.max(ans, grid.get(i).get(j) - temp);
                grid.get(i).set(j, Math.min(temp, grid.get(i).get(j)));
            }
        }
        return ans;
    }
`````

