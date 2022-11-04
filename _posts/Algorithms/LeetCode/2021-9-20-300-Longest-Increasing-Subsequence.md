---
title: LeetCode-300. 最长上升子序列
author: zjpzhao
date: 2021-09-20 21:00:00 +0800
categories: [Algorithms,LeetCode]
math: true
tags: [贪心,DP,二分]

---
# 关联
[LeetCode-673. 最长递增子序列的个数](/posts/673-Number-of-Longest-Increasing-Subsequence)

# 方法1 DP
## 思考
$dp[i]$表示前i个元素中，以$num[i]$元素结尾（必选）的最长*上升子序列*长度，写出下列状态转移方程：
$$dp[i]=max(dp[j])+1,\ 0\le j \lt i,\ num[j]<num[i].$$ 

整体的最长*上升子序列*数量是：$LIS=max{dp[i]},\ 0\le i \lt n.$

## 举例

| $nums$ | 10  |  9  |  2  |  5  |  3  |  7  |  101  |  18   |
|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:-----:|:-----:|
|  $dp$  |  1  |  0  |  0  |  0  |  0  |  0  |   0   |   0   |
|  $dp$  |  1  |  1  |  0  |  0  |  0  |  0  |   0   |   0   |
|  $dp$  |  1  |  1  |  1  |  0  |  0  |  0  |   0   |   0   |
|  $dp$  |  1  |  1  |  1  |  2  |  0  |  0  |   0   |   0   |
|  $dp$  |  1  |  1  |  1  |  2  |  2  |  3  | **4** | **4** 

## 代码


```cpp
class Solution {

public:

	int lengthOfLIS(vector<int>& nums){

		int n=nums.size();

		if(n==0) return 0;

		vector<int> dp(n,0);

		for(int i=0;i<n;i++){//求dp[i]

			dp[i]=1;

			for(int j=0;j<i;j++){

				if(nums[j]<nums[i])

					dp[i]=max(dp[i],dp[j]+1);
			}
		}
		return *max_element(dp.begin(),dp.end());
	}
};
```

|

## 复杂度分析

- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$

---
# 方法2 贪心+二分
## 思考
我们让新添加进来的末尾元素尽可能的小，让上升的幅度小一些，以便得到更长的子序列。所以这里采用一个数组d来记录长度为length的最长*上升子序列*的最小末尾元素，初始值为$d[1]=nums[0]$，注意是从1开始。

因为*上升子序列*要求严格递增，则数组d也是单调递增的，也就是说越长的*上升子序列*最小末尾元素也就越大。根据数组d的单调性，我们这里查找可以采用二分法，伪代码如下： 


遍历$nums$，更新$len$，和数组d，对于$nums[i]$：  

① 如果$nums[i]>d[len]$，则$d[++len]=nums[i]$  

② 否则在$d[1:len]$中二分查找：第一个比$nums[i]$小的数$d[k]$，同时$d[k+1]=nums[i]$

## 举例

|       | **0** |          **8**           | **4** | **12** | **2** | $nums[i]$ | ①/② | 
|:-----:|:-----:|:------------------------:|:-----:|:------:|:-----:|:---------:|:---:|
| **d** |   0   |                          |       |        |       |     0     |     |
| **d** |   0   |            8             |       |        |       |     8     |  ①  |
| **d** |   0   | <font color=red>4</font> |       |        |       |     4     |  ②  |
| **d** |   0   |            4             |  12   |        |       |    12     |  ①  |
| **d** |   0   | <font color=red>2</font> |  12   |        |       |     2     |  ②  |

##  代码
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
       int n=nums.size();
       if(n==0) return 0;
       int len=1;
       vector<int> d(n+1,0);
       d[len]=nums[0];
       for(int i=1;i<n;i++){//从nums[1]开始遍历nums
           if(nums[i]>d[len]) d[++len]=nums[i];
           else {
               int l=1,r=len,pos=0;
			   //注意d是从d[1]开始的；如果找不到说明所有的数都比nums[i]大，此时更新d[1]，所以初始pos设0
               while(l<=r){
                   int mid=((r-l)>>1)+l;
                   if(d[mid]<nums[i]){
                       pos=mid;
                       l=mid+1;
                   }
                   else r=mid-1;
               }
               d[pos+1]=nums[i];
           }
       }
       return len;
    }
};
```

## 复杂度分析
- 时间复杂度：$O(n\log(n))$
- 空间复杂度：$O(n)$


更多细节参考[$LeetCode$官方题解](https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-by-leetcode-soluti/)