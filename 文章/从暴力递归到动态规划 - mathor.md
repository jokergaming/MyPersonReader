> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [wmathor.com](https://wmathor.com/index.php/archives/973/)

> 动态规划没有那么难，但是很多老师在讲课的过程中讲的并不好，由此写下一篇文章记录学习过程 1. 题目链接：LeetCode518 从暴力递归开始暴力递归就是尝试，这道题比较简单，从 coins[0] 开始尝...

动态规划没有那么难，但是很多老师在讲课的过程中讲的并不好，由此写下一篇文章记录学习过程

#### 1. 题目链接：[LeetCode518](https://leetcode-cn.com/problems/coin-change-2/description/) 展开目录

![](https://pic.imgdb.cn/item/609ba7f9d1a9ae528ff4e56b.png#shadow)

##### 从暴力递归开始展开目录

暴力递归就是尝试，这道题比较简单，从 coins [0] 开始尝试，coins [0] 选 0 个，那么用剩下的 coins [1...n] 凑出 amount 的数量为 a；coins [0] 选 1 个，那么用剩下的 coins [1...n] 凑出 amount-coins [0] 的数量为 b；coins [0] 选 2 个，那么用剩下的 coins [1...n] 凑出 amount-coins [0]*2 的数量为 c...... 依次继续下去，a+b+c+... 的结果就为最终答案

```
public static int coins1(int[] coins,int amount) {

    if(coins == null || coins.length == 0)

        if(amount == 0)

            return 1;

        else 

            return 0;

    return process1(coins,0,amount);

}

    

//index:可以自由使用index及其之后所有的钱

//amount：目标钱数

public static int process1(int[] coins,int index,int amount) {

    int res = 0;

    if(amount == 0)

        return 1;

    if(amount != 0 && index >= coins.length)

        return 0;

    else

        for(int num = 0;coins[index] * num <= amount;num++)

            res += process1(coins,index + 1,amount - coins[index] * num);

    return res;

}

```

##### 初级优化展开目录

考虑这么一个问题，假设可选的钱数有 200,100,50,10，目标钱数是 1000，选了 0 张 200,4 张 100，或者选了 1 张 200,2 张 100，这两种情况剩下的目标 i 都是 index = 2,amount = 600。只要 index 和 amount 确定，后面的值一定都是一样的，会产生很多重复的递归过程，所以第一个优化版本就有了，用一个 map 保存所有记录，当遇到同样过程的时候就直接调用

```
//key:"index_aim"

//value:返回值

public static HashMap<String,Integer> map = new HashMap<>();

    

public static int coins1(int[] coins,int amount) {

    if(coins == null || coins.length == 0)

        if(amount == 0)

            return 1;

        else 

            return 0;

    return process1(coins,0,amount);

}

    

//index:可以自由使用index及其之后所有的钱

//amount：目标钱数

public static int process1(int[] coins,int index,int amount) {

    int res = 0;

    if(amount == 0)

        return 1;

    if(amount != 0 && index >= coins.length)

        return 0;

    else

        for(int num = 0;coins[index] * num <= amount;num++) {

            String key = String.valueOf(index+1) + "_" + String.valueOf(amount - coins[index] * num);

            if(map.containsKey(key))

                res += map.get(key);

            else

                res += process1(coins,index + 1,amount - coins[index] * num);

        }

    map.put(String.valueOf(index+1) + "_" + String.valueOf(amount - coins[index] * amount), res);

    return res;

}

```

##### 高级优化展开目录

构建一张二维表（N=coins.length），画红色叉的部分就是最终需要的答案，有一些值可以直接根据边界条件直接写出，就先写在表中，然后我们观察一个一般位置 (index,amount)，根据递归式可知，二维表中 (index,amount) 位置的值是 (index+1,amount)+(index+1,amount-coins [index])+... 的值，因此从倒数第二排以直网上填满整张表，最终答案也就出来了，中间没有任何重复算的部分  
![](https://pic.imgdb.cn/item/609ba806d1a9ae528ff542f7.png#shadow)举个例子，假设 amount=5，coins []={1,2,5}，画出来的表如下，蓝色部分表示根据边界条件直接写的值，黑色部分表示推导的值，红色部分表示最终要求的结果  
![](https://pic.imgdb.cn/item/609ba83ad1a9ae528ff6cd12.png#shadow)

```
public static int coins1(int[] coins,int amount) {

    if(coins == null || coins.length == 0)

        if(amount == 0)

            return 1;

        else 

            return 0;

    return process1(coins,0,amount);

}

    

public static int process1(int[] coins,int index,int amount) {

    int n = coins.length;

    int[][] dp = new int[n + 1][amount+1];

    //首先将能填的都填上

    for(int i = 0;i <= n;i++)

        dp[i][0] = 1;

    for(int i = 1;i <= amount;i++)

        dp[n][i] = 0;

    

    //开始循环填表

    for(int i = n - 1;i >= 0;i--) {//行

        for(int j = 1;j <= amount;j++) {//列

            for(int k = j;k >= 0;k -= coins[i])

                dp[i][j] += dp[i + 1][k];

        }

    }

    return dp[0][amount];

}

```

##### 最终优化展开目录

假设我们现在正在算 ，$dp [i][j]，dp [i][j]=dp [i+1][j]+dp [i+1][j-conins [i]]+...$；又因为 $dp [i][j-coins [i]]=dp [i+1][j-coins [i]\times2]+dp [i+1][j-coins [i]\times3]$，所以 $dp [i][j]=dp [i+1][j]+dp [i][j-coins [i]]$  
![](https://pic.imgdb.cn/item/609ba84ad1a9ae528ff742ce.png#shadow)

```
class Solution {

    public static HashMap<String,Integer> map = new HashMap<>();

    public int change(int amount, int[] coins) {

        map.clear();

        if(coins == null || coins.length == 0) {

            if(amount == 0)

                return 1;

            else 

                return 0;

        }

        return process1(coins,0,amount);

    }

    public static int process1(int[] coins,int index,int amount) {

        int n = coins.length;

        int[][] dp = new int[n + 1][amount+1];

        //首先将能填的都填上

        for(int i = 0;i <= n;i++)

            dp[i][0] = 1;

        

        //开始循环填表

        for(int i = n - 1;i >= 0;i--) {//行

            for(int j = 1;j <= amount;j++) {//列

                dp[i][j] = dp[i + 1][j];

                dp[i][j] += (j - coins[i]) >= 0 ? dp[i][j-coins[i]] : 0;

            }

        }

        return dp[0][amount];

    }

}

```

#### 2. 题目链接：[LeetCode486](https://leetcode-cn.com/problems/predict-the-winner/description/) 展开目录

![](https://pic.imgdb.cn/item/609ba854d1a9ae528ff78e62.png#shadow)

##### 从暴力递归开始展开目录

对于这个问题，我们假设先选的人最终得到分数的函数是 int f (int [] arr,int i,int j)，i 表示最左边的下标，j 表示最右边的下标，对于先选的人来说，边界条件就是当 i=j 时，返回 arr [i]；对于后选的人来说，最终得到分数的函数是 int s (int [] arr,int i,int j)，同样的，边界条件是 i=j 时，返回 0  

如果不是边界条件，先选的人得到的分数递归式就应该是：i 位置拿走然后加上 s (arr,i+1,j) 和 j 位置拿走然后加上 s (arr,i,j-1) 两者中较大的那个；对于后选的那个来说拿走的肯定是 f (arr,i+1,j) 和 f (arr,i,j-1) 中较小的那个，因此暴力递归版本的代码就出来了

```
class Solution {

    public boolean PredictTheWinner(int[] arr) {

        if(arr == null || arr.length == 0)

            return true;

        if(f(arr,0,arr.length - 1) >= s(arr,0,arr.length - 1) || arr.length == 1)

            return true;

        return false;

    }

    public static int f(int[] arr,int i,int j) {

        if(i == j)

            return arr[i];

        return Math.max(arr[i] + s(arr,i + 1,j),arr[j] + s(arr,i,j-1));

    }

    

    public static int s(int[] arr,int i,int j) {

        if(i == j)

            return 0;

        return Math.min(f(arr,i + 1,j),f(arr,i,j - 1));

    }

}

```

##### 动态规划展开目录

这道题不是很一样，因为一个函数的值需要另一个函数来确定，两个函数是相互影响的，因此需要两张表分别存，下图蓝色部分是因为边界条件可以直接填的，黑色部分按照顺序进行填写，红色部分是最终答案。解释一下，因为 i 不可能大于 j，i==j 的时候就返回了，因此 i 大于 j 的部分全部都不用填  
![](https://pic.imgdb.cn/item/609ba85ed1a9ae528ff7dc01.png#shadow)

#### 3. 某里笔试题展开目录

现有一条坐标轴，一个机器人初始停留在 m 位置，可以走 p 步，如果在 n 位置只能往左走，如果在 1 位置只能往右走，问最终停留在 k 上有多少种情况

##### 从暴力递归开始展开目录

特殊情况单独进行递归即可

```
//n：一共有n个位置

//m：初始停留的位置

//p：可以走的步数

//k：最终停留的位置

public static int ways(int n,int m,int p,int k) {

    if(n < 2 || m < 1 || m > n || p < 0 || k < 1 || k > n)

        return 0;

    if(p == 0)

        return m == k ? 1 : 0;

    int res = 0;

    if(m == 1)

        res += ways(n,m + 1,p - 1,k);

    else if(m == n)

        res += ways(n,m - 1,p - 1,k);

    else

        res += ways(n,m + 1,p - 1,k) + ways(n,m - 1,p - 1,k);

    return res;

}

```

##### 动态规划展开目录

见下图，类似一个杨辉三角  
![](https://pic.imgdb.cn/item/609ba868d1a9ae528ff82351.png#shadow)

```
//n：一共有n个位置

//m：初始停留的位置

//p：可以走的步数

//k：最终停留的位置

public static int ways(int n,int m,int p,int k) {

    int[][] dp = new int[p + 1][n + 1];

    dp[0][k] = 1;

    for(int i = 1;i <= p;i++) {//行，p

        for(int j = 1;j <= n;j++) {//列， n

            if(j == 1)

                dp[i][j] = dp[i - 1][j + 1];

            if(j == n)

                dp[i][j] = dp[i - 1][j - 1];

            else

                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j + 1];

        }

    }

    return dp[p][m];

}

```

#### 4. 题目链接：[LeetCode10](https://leetcode-cn.com/problems/regular-expression-matching/description/) 展开目录

![](https://pic.imgdb.cn/item/609ba872d1a9ae528ff8653c.png#shadow)

##### 从暴力递归开始展开目录

假设有一个函数 boolean f (int i,int j)，含义是 s 字符串从 i 开始往后的字符能否和 p 字符串从 j 位置开始往后匹配成功，返回值是 boolean，分三种情况

*   j <p.length - 1 && p[j+1] != '*'

在这种情况下只有当 (p [j] = s [i] || p [j] = '.') 时才能继续匹配 f (i+1,j+1)，否则直接返回 false

*   j <p.length - 1 && p[j+1] == '*'

这种情况有特别多分支，假设 s 串为 "aaaab"，p 串为 "c× 某某某", 此时就需要 c× 配合，将 c 变为 0 个，然后判断 f (i,j+2) 能否匹配；假设 s 串为 "aaab"，p 串为 "a× 某某某"，此时就需要枚举 a× 变成几个 a，假设变成 0 个 a，那就要判断 f (i,j+2) 能否匹配，假设变成 1 个 a，那就需要判断 f (i+1,j+2) 能否匹配，假设变成 2 个 a，那就要判断 f (i+2,j+2) 能否匹配，以直往下....

```
class Solution {

    static char[] str,exp;

    public boolean isMatch(String s, String p) {

        str = s.toCharArray();

        exp = p.toCharArray();

        return process(0,0);

    }

    public static boolean process(int i,int j) {

        if(j == exp.length)

            return i == str.length;

        if(j + 1 == exp.length || exp[j + 1] != '*') 

            return i != str.length && (exp[j] == str[i] || exp[j] == '.') 

            && process(i + 1,j + 1);

        //exp的j+1位置不仅有字符，并且exp[j] == '*'

        while(i != str.length && (exp[j] == str[i] || exp[j] == '.')) {

            if(process(i,j+2)) 

                return true;

            i++;

        }

        return process(i,j+2);

    }

}

```

##### 动态规划展开目录

先把最后两列和最后一行填满，然后依次填即可

```
class Solution {

    static char[] str,exp;

    public boolean isMatch(String str, String exp) {

        if(str == null || exp == null)

        return false;

        char[] s = str.toCharArray();

        char[] e = exp.toCharArray();

        boolean[][] dp = initDpMap(s,e);

        for(int i = s.length - 1; i > -1 ;i--) {

            for(int j = e.length - 2;j > -1;j--) {

                if(e[j + 1] != '*')

                    dp[i][j] = (s[i] == e[j] || e[j] == '.') && dp[i + 1][j + 1];

                else {

                    int si = i;

                    while(si != s.length && (s[si] == e[j] || e[j] == '.')) {

                        if(dp[si][j + 2]) {

                            dp[i][j] = true;

                            break;

                        }

                        si++;

                    }

                    if(dp[i][j] != true)

                        dp[i][j] = dp[si][j + 2];

                }

            }

        }

        return dp[0][0];

    }

    public static boolean[][] initDpMap(char[] s,char[] e) {

        int slen = s.length;

        int elen = e.length;

        boolean[][] dp = new boolean[slen + 1][elen + 1];

        dp[slen][elen] = true;

        for(int i = elen - 2;i > -1;i -= 2) {

            if(e[i] != '*' && e[i + 1] == '*')

                dp[slen][i] = true;

            else

                break;

        }

        if(slen > 0 && elen > 0) 

            if((e[elen - 1] == '.' || s[slen - 1] == e[elen - 1])) 

                dp[slen - 1][elen - 1] = true;

        return dp;

    }

}

```