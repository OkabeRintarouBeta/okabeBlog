---
author: zihui
pubDatetime: 2023-08-21T15:22:00Z
title: Subarray Problem Summary
postSlug: subarray-problem
featured: true
draft: false
tags:
  - sliding window
  - prefix sum
  - dynamic programming
  - leetcode
ogImage: ""
description: Diffferent types of subarray problems
---

This article summarizes different kinds of sliding window problems on leetcode

## Table of contents

## Count subarrays tha satisfy a certain condition

### Sliding Windows

Overview:

- For the array A, for each subarray with ending at index j, slide through the left boundary i until the condition is not satisfied any more.
- For each i, figure out the number of subarrays are added(if an array satisfy the condition, then it is likely that its subarray also satisfy it)

Example Problems:

- [2799. Count Complete Subarrays in an Array](https://leetcode.com/problems/count-complete-subarrays-in-an-array/)
- [713. Subarray Product Less Than K
  ](https://leetcode.com/problems/subarray-product-less-than-k/)
- [1358. Number of Substrings Containing All Three Characters](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/)

### Hash Map & Prefix Sum

For each prefix sum, record the times/position.
Example Problems:

- [560. Subarray Sum Equals K
  ](https://leetcode.com/problems/subarray-sum-equals-k/)

### Priority Queue (need to keep track of the maximum and minimum inside a subarray)

Example Problems:

- [2762. Continuous Subarrays
  ](https://leetcode.com/problems/continuous-subarrays/)

## Check whether there is a subarray that meets a condition

### Prefix sum

This kind of problem usually involves the sum of a subarray
Example:

- [523. Continuous Subarray Sum
  ](https://leetcode.com/problems/continuous-subarray-sum/) **Easy to go wrong for boundary cases!**

## Locating the start and end of the qualified subarray

e.g. Find longest string
Example:

- [1759. Count Number of Homogenous Substrings](https://leetcode.com/problems/count-number-of-homogenous-substrings/)
- [2730. Find the Longest Semi-Repetitive Substring](https://leetcode.com/problems/find-the-longest-semi-repetitive-substring/)
