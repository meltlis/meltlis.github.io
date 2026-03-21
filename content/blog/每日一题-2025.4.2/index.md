---
title: '每日一题 2025.4.2'
date: '2025-03-04 00:39:01'
authors:
  - me
categories:
  - '算法题'
  - 'leetcode'
  - '动态规划'
  - '困难'
toc: true
---

总感觉在哪里做过, 但就是做不出来。鏖战了2小时才写出来。
<!--more-->

1278\. 分割回文串 III
----------------

给你一个由小写字母组成的字符串 `s`，和一个整数 `k`。

请你按下面的要求分割字符串：

*   首先，你可以将 `s` 中的部分字符修改为其他的小写英文字母。
*   接着，你需要把 `s` 分割成 `k` 个非空且不相交的子串，并且每个子串都是回文串。

请返回以这种方式分割字符串所需修改的最少字符数。

**示例 1：**

**输入：** s = "abc", k = 2
**输出：** 1
**解释：** 你可以把字符串分割成 "ab" 和 "c"，并修改 "ab" 中的 1 个字符，将它变成回文串。

**示例 2：**

**输入：** s = "aabbc", k = 3
**输出：** 0
**解释：** 你可以把字符串分割成 "aa"、"bb" 和 "c"，它们都是回文串。

**示例 3：**

**输入：** s = "leetcode", k = 8
**输出：** 0

**提示：**

*   `1 <= k <= s.length <= 100`
*   `s` 中只含有小写英文字母。

[https://leetcode.cn/problems/palindrome-partitioning-iii/solutions/](https://leetcode.cn/problems/palindrome-partitioning-iii/solutions/)


回文数的动态规划应该算是比较经典的情况了。想出状态转移方程也应该不会太难。

很显然, 有字符串长度和分割字串数目这两个参数。然后再寻找两个参数和状态之间的关系。

可以发现, **如果将长度为i字符串分割j块, 其实也就相当于前i-l个字符组成的字符串分割成j-1块, 再加上后l个字符组成的字符串组合**。已经分割好的字符串需要改变的字母数量是很好得出的(虽然我在这里因为少加了个前边界卡了1个小时)。
**于是, 只要把前i-l个字符组成的字符串分割j-1块后需要修改的字母数加上后l个字符组成的字符串组合需要修改的字母数加上, 就是总共需要修改的字母数。**

再选取所有分割方式中需要修改字母数最少的情况,于是得到状态转移方程:

```
dp[i][j] = min(dp[i][j], dp[i - l][j - 1] + f(i-l,i));
```

为方便传递参数, 后面的程序中写成了

```
dp[i][j] = min(dp[m][j - 1] + f(s, m, i - 1), dp[i][j]);
```

其中, f(i-l,i)是字符串i - l到i需要修改的字符数量。这个只需要统计前半的字符串与后半的字符串倒序有几个字符不同就好了。

最后就是边界条件。当j = 1时, 显然dp[i][1]就等于f(0,i), i - l >= j - 1。
题解:
```
class Solution {
public:
    int f(string s, int a, int b){
        int i, n = 0;
        for(i = a; i <= (a + b)/2; i++){
            if(s[i] != s[b - i + a])n++;
        }
        return n;
    }

    int palindromePartition(string s, int k) {
        int n = s.size(), i, j, m;
        vector<vector<int>> dp(n + 1, vector<int>(k + 1, INT_MAX));
        dp[0][0] = 0;
        for(i = 1; i <= n; i++){
            for(j = 1; j <= min(i, k); j++){
                if(j == 1){
                    dp[i][j] = f(s, 0, i - 1);
                }
                else
                    for(m = j - 1; m < i; m++){
                        dp[i][j] = min(dp[m][j - 1] + f(s, m, i - 1), dp[i][j]);
                    }
            }
        }
        return dp[n][k];
    }
};
```