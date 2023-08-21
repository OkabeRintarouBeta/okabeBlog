---
author: zihui
pubDatetime: 2023-07-13T15:22:00Z
title: Backpack Problem Summary
postSlug: backpack-problem-summary
featured: true
draft: false
tags:
  - dynamic programming
  - leetcode
ogImage: ""
description: 01 and complete backpack basics and examples on leetcode
---

01 and complete backpack basics and examples on leetcode.

## Table of contents

## Category

![img](../../../public/assets/leetcode/backpack/category.png)

## 01 Backpack

### 2D dynamic programming

| object\weight | 0   | 1   | 2   |
| ------------- | --- | --- | --- |
| object0       |     |     |     |
| object1       |     |     |     |
| object2       |     |     |     |
| object3       |     |     |     |

dp[i][j] : the total value when choosing between 0 - i th object, reaching weight j

```latex
dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i]]+value[i])
```

1. Boundary Cases
   1. no weight, traverse till the $i^{th}$ object
   2. only the 0th object
      1. when j<weight[0], dp[0][j]=0
      2. when j>=weight[0],dp[0][j]=value[i];
2. Traverse Order
   - both object-first and bag-first are OK for 01 backpack
     - why? they both visit the upper and upper-left of the array for a given block
3. Sample Code
   ```C++
    for(int i = 1; i < weight.size(); i++) {
      for(int j = 0; j <= bagweight; j++) {
          if (j < weight[i]) dp[i][j] = dp[i - 1][j];
          else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
      }
    }
   ```

### 1D Dynamic Programming

1. Boundary Case:none, just initialize dp[i]=0
2. Code

   ```C++
    for(int i = 0; i < weight.size(); i++) {
      for(int j = bagWeight; j >= weight[i]; j--) {
          dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
      }
    }
   ```

   ```python
   print "hello"
   ```

   - Notice that bag weight is traversed in reverse order, this could make sure that each item is only used for one time.
   - Can we reverse the order of objects and bag weight?
     - No!
     - Based on the value in the top-left, we decide the value in the bottom-right, therefore we have to make sure that the value in the upper-left is not changed.

### Example Problems

#### 416. Partition Equal Subset Sum

[link](https://leetcode.com/problems/partition-equal-subset-sum/description/)

#### 1049. Last Stone Weight II

[link](https://leetcode.com/problems/last-stone-weight-ii/)

- Take both value and weight to be stone weight
- Try to split the stones to two parts with the closest weight sum(value)

#### 474. Ones and Zeros

[link](https://leetcode.com/problems/ones-and-zeroes/)

#### 494. Target Sum

[link](https://leetcode.com/problems/target-sum/)

- HINT;
  - the number of ways to get x is the same as the number of ways to get -x(just switch "+" and "-")

#### Complete Backpack

- Different from 01 backpack, each item could be used for many times.

  Therefore, the only difference in code is traversing the bag weight from small to large.

- Code
  ```C++
  for(int i = 0; i < weight.size(); i++) {
    for(int j = weight[i]; j <= bagWeight ; j++) {
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
  }
  ```
  The order for two for-loops could switch
