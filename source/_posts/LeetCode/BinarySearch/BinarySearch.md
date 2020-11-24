---
title: 二分搜索
tags:
  - 算法
  - LeetCode
  - 二分搜索
categories:
  - LeetCode
comment: true
abbrlink: 9d399abe
date: 2020-11-24 10:34:21
---

二分查找（Binary Search）也叫作折半查找。二分查找有两个要求，一个是数列有序，另一个是数列使用顺序存储结构。时间复杂度`O(logn)`

<!--more-->


# 基本的二分搜索

基本的二分搜索，就是只要找到 target 就返回。

例如：[1, 2, 4, 4, 4, 5, 6], target = 4
- 第一次中位数是 a[3] = 4 
- 找到target，直接返回 index=3

## 算法思路
1. 因为初始化 right = nums.length - 1
2. 所以决定了「搜索区间」是 [left, right]
3. 所以决定了 while (left <= right)
4. 大于 target , right = mid-1
5. 小于 target , left = mid+1
6. 因为我们只需找到一个 target 的索引即可, 所以当 nums[mid] == target 时可以立即返回



## 算法 golang 模板
```go
func Basic_bound(nums []int, target int) int {
	var (
		left int = 0
		right int = len(nums) - 1
	)

	for left <= right {
		mid := left + (right - left) / 2;
		if nums[mid] < target {
			left = mid + 1;
		} else if nums[mid] > target {
			right = mid - 1;
		} else if nums[mid] == target {
			return mid;
		}
	}
	// 直接返回
	return -1;
}
```


# 寻找左侧边界的二分搜索
寻找左侧边界的二分搜索，就是找到左边第一个等于 target 的值返回。

例如：[1, 2, 4, 4, 4, 5, 6], target = 4
- 第一次中位数是 a[3] = 4 
- target = a[3] ，但有可能 index=3 之前还有等于 4 的值，因此向左缩小范围
- 找到左边第一个等于 4 的 index = 3 返回



## 算法思路
1. 因为初始化 right = nums.length - 1
2. 所以决定了「搜索区间」是 [left, right]
3. 所以决定了 while (left <= right)
4. 大于 target , right = mid-1
5. 小于 target , left = mid+1
6. 等于 target , 向左收缩，right = mid-1
7. 判断 left 是否越界
8. 最终的 left 就是找到 从左边开始的第一个等于 target 的下标值



## 算法 golang 模板
```go
func Left_bound(nums []int, target int) int {
	var (
		left int = 0
		right int = len(nums) - 1
	)

	for left <= right {
		mid := left + (right - left) / 2;
		if nums[mid] < target {
			left = mid + 1;
		} else if nums[mid] > target {
			right = mid - 1;
		} else if nums[mid] == target {
			// 别返回，锁定左侧边界
			right = mid - 1;
		}
	}
	// 最后要检查 left 越界的情况
	if left >= len(nums) || nums[left] != target {
		return -1;
	}
	return left;
}
```




# 寻找右侧边界的二分查找



## 算法思路
1. 因为初始化 right = nums.length - 1
2. 所以决定了「搜索区间」是 [left, right]
3. 所以决定了 while (left <= right)
4. 大于 target , right = mid-1
5. 小于 target , left = mid+1
6. 等于 target , 向右收缩，left = mid+1
7. 判断 right 是否越界
8. 最终的 right 就是找到 从左边开始的第一个等于 target 的下标值




## 算法 golang 模板
```go
func Right_bound(nums []int, target int) int {
	var (
		left int = 0
		right int = len(nums) - 1
	)

	for left <= right {
		mid := left + (right - left) / 2;
		if nums[mid] < target {
			left = mid + 1;
		} else if nums[mid] > target {
			right = mid - 1;
		} else if nums[mid] == target {
			// 别返回，锁定左侧边界
			left = mid + 1;
		}
	}
	// 最后要检查 left 越界的情况
	if right < 0 || nums[right] != target {
		return -1;
	}
	return right;
}
```