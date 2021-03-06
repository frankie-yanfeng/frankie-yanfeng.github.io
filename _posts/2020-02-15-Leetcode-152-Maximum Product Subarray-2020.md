---
layout:     post   				    # 使用的布局（不需要改）
title:      Leetcode-152 Maximum Product Subarray 				# 标题
subtitle:   Leetcode-152 #副标题
date:       2020-02-15 				# 时间
author:     frankie 						# 作者
header-img: img/tag-bg-o.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Leetcode
    - DP
    - Array
---

# Iteration based on the start and end positions - O(n^2)

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {

        int tmp = 1;
        int maxProdResult = INT_MIN;
        int len = nums.size();

        if (len == 1)
            return nums[0];

        for(int i = 0; i < len; i++) {          //start position

            tmp = nums[i];

            if (nums[i] > maxProdResult)
               maxProdResult = nums[i];

            for(int j = i+1; j < len; j++) {    //end position
                tmp *= nums[j];

                if (tmp > maxProdResult)
                    maxProdResult = tmp;
            }
        }

        return maxProdResult;
    }
};
```

# Dynamic Programming - O(n)

[递推公式](https://leetcode.com/problems/maximum-product-subarray/discuss/201223/python-code)

[C++ Solution](https://leetcode.com/problems/maximum-product-subarray/discuss/307430/4ms-C%2B%2B-solution)

[C++ Solution](https://leetcode.com/problems/maximum-product-subarray/discuss/48417/Straightforward-implementation-of-the-suggested-solution-in-10-lines-of-C%2B%2B)

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {

        int len = nums.size();

        if (len == 0)
            return 0;

        if (len == 1)
            return 1;

        vector<int> DpMax;
        vector<int> DpMin;

        DpMax[0] = nums[0];
        DpMin[0] = nums[0];

        for ( int i = 0; i < len; i++) {
            if (nums[i] > 0){
                DpMax[i] = max(nums[i], DpMax[i-1] * nums[i]);
                DpMin[i] = min(nums[i], DpMax[i-1] * nums[i]);
            }
            else {
                DpMax[i] = max(nums[i], DpMin[i-1] * nums[i]);
                DpMin[i] = min(nums[i], DpMax[i-1] * nums[i]);
            }
        }

        return *max_element(DpMax.begin(), DpMax.end());
    }
};
```
