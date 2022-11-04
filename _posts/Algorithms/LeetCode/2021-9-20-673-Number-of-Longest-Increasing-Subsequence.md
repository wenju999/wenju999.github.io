---
title: LeetCode-673. 最长递增子序列的个数 
author: zjpzhao
date: 2021-09-20 21:00:00 +0800
categories: [Algorithms,LeetCode]
math: true
tags: [贪心,DP,二分,前缀和]     # TAG names should always be lowercase
---
# 关联
先阅读[LeetCode-300. 最长上升子序列](/posts/300-Longest-Increasing-Subsequence)，在此基础上进行扩展。
此题相当于就是在300题的基础上增加记录历史信息。
# 方法1 DP
## 思考
在[LeetCode-300. 最长上升子序列](/posts/300-Longest-Increasing-Subsequence)的方法一的基础上，增加一个数组$cnt$，$cnt[i]$表示以$num[i]$结尾的最长*上升子序列*的个数。设$nums$的最长*上升子序列*的长度为$maxlen$，则对于所有$dp[i]=maxlen$的i，累加对应的$cnt[i]$即可得到答案。

## 代码
```cpp
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int n=nums.size();
        if(n==0) return 0;
        int maxlen=0,ans=0;
        vector<int> dp(n),cnt(n);//dp记录以nums[i]结尾的最长上升子序列长度，cnt记录以nums[i]结尾的最长上升子序列的个数
        for(int i=0;i<n;i++){
            dp[i]=1;
            cnt[i]=1;
            for(int j=0;j<i;j++){
                if(nums[j]<nums[i]){
                    if(dp[i]<dp[j]+1){
                        dp[i]=dp[j]+1;//更新了更长的长度，所以同时需要更新该长度的上升子序列个数
                        cnt[i]=cnt[j];//重新计数，个数从num[i]那里继承过来
                    }
                    else if(dp[i]==dp[j]+1)//说明同样的长度，也可以从num[j]过来，所以把长度j的个数也加进来
                        cnt[i]+=cnt[j];
                }
            }
            //遍历每个数组元素的时候，都记录一下当前最大长度和对应的个数
            if(dp[i]>maxlen){
                maxlen=dp[i];//更新最大长度
                ans=cnt[i];//重新计数
            }else if(dp[i]==maxlen){
                ans+=cnt[i];//累加计数
            }
        }
        return ans;
    }
};
```

## 复杂度分析

- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$

---
# 方法2 贪心+二分+前缀和
## 思考
1. 同样在[LeetCode-300. 最长上升子序列](/posts/300-Longest-Increasing-Subsequence)方法二基础上，将数组d扩展成为2维的，不止记录**最小**末尾元素，一维数组$d[i]$记录：**所有**能构成长度i的最长*上升子序列*的<u>末尾元素</u>，即记录的历史信息。具体地，将300题方法二的②中$d[k+1]=nums[i]$换成：将$nums[i]$置于$d[k+1]$数组末尾，$d[i]$是单调非增的。

2. 同样定义一个二维数组$cnt$，$cnt[i][j]$记录以$d[i][j]$为结尾的最长*上升子序列*个数，计算$cnt[i][j]$就是将所有满足$d[i−1][k]<d[i][j]$的$c[i-1][k]$累加到$cnt[i][j]$，最后答案是$cnt[maxlen]$的所有元素和。

3. 由于$d[i]$是单调非增的。所以可以采用二分法得到上面提到的k。
另外，将$cnt$改为前缀和，开头为0，则$d[i][j]$对应的最长上升子序列的个数是$cnt[i-1][-1]-cnt[i-1][k]$（[-1]表示数组最后一个元素）

## 前缀和

原本表示$a[1:n]$如下

| 1      | 2      | 3      | ... | n-1      | n      |
| ------ | ------ | ------ | --- | -------- | ------ |
| $a[1]$ | $a[2]$ | $a[3]$ | ... | $a[n-1]$ | $a[n]$ | 


表示成前缀和$b[1:i]$如下

| 1      | 2                     | 3                     | ... | n-1                     | n                     |
| ------ | --------------------- | --------------------- | --- | ----------------------- | --------------------- |
| $a[1]$ | $\sum_{i=1}^{2} a[i]$ | $\sum_{i=1}^{3} a[i]$ | ... | $\sum_{i=1}^{n-1} a[i]$ | $\sum_{i=1}^{n} a[i]$ | 

有$a[i]=b[i]-b[i-1]$，前缀和的好处是在存入的时候就进行累加，最后可以省去最后遍历的次数（求原本表示中$a[k:i]$的元素和需要遍历然后累加，使用前缀和表示后只需要用$b[i]-b[k-1]$即可）

## 代码
```cpp
class Solution {
    int binarySearch(int n, function<bool(int)> f) {
        int l = 0, r = n;
        while (l < r) {
            int mid = l + ((r - l) >> 2);
            if (f(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

public:
    int findNumberOfLIS(vector<int> &nums) {
        vector<vector<int>> d, cnt;
        for (int v : nums) {
            int i = binarySearch(d.size(), [&](int i) { return d[i].back() >= v; });
            int c = 1;
            if (i > 0) {
                int k = binarySearch(d[i - 1].size(), [&](int k) { return d[i - 1][k] < v; });
                c = cnt[i - 1].back() - cnt[i - 1][k];
            }
            if (i == d.size()) {
                d.push_back({v});
                cnt.push_back({0, c});
            } else {
                d[i].push_back(v);
                cnt[i].push_back(cnt[i].back() + c);
            }
        }
        return cnt.back().back();
    }
};
```

## 复杂度分析
- 时间复杂度：$O(n\log(n))$
- 空间复杂度：$O(n)$


更多细节参考[$LeetCode$官方题解](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/solution/zui-chang-di-zeng-zi-xu-lie-de-ge-shu-by-w12f/)