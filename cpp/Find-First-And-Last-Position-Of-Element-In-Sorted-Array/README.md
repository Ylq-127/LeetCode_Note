# 题目描述: 在排序数组中查找元素的第一个和最后一个位置

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 O(logn) 级别。

如果数组中不存在目标值，返回 [-1, -1]。

示例 1:
```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
```

示例 2:
```
输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```
  
# 解题思路:
  [二分查找算法](./BinarySearch.md)
  
  1). 使用两次二分查找法
  
  2). 第一次二分查找目标值的左边界，第二次查找目标值的右边界。（注意：二分法的三种情况，查找某一个数，查找左边界，查找右边界）

# 时间复杂度：
  O(logn) + O(logn) , O(logn)
# 空间复杂度
  O(1)
# 代码

## [C++](./Find-First-And-Last-Position-Of-Element-In-Sorted-Array.cpp):
### 方法一：两次二分查找
```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
      vector<int> res = {-1,-1};
      int n = nums.size();
      if (nums.empty()) return res;
      int start = 0;
      int end = n;
      while (end > start)   // 寻找左边界，搜索区间为左闭右开
      {
        int mid = start + (end - start)/2;
        if (nums[mid]==target)
        {
            end = mid;
        }
        if (nums[mid]<target)
        {
            start = mid + 1;
        }
        if (nums[mid]>target)
        {
            end = mid;
        }
      }
      if (start==n || nums[start]!=target) return res;
      res[0] = start;
      start = 0;
      end = n;
      while(end > start) //寻找右边界，搜索区间为左开右闭
      {
        int mid = start + (end - start)/2;
        if (nums[mid]==target) 
        {
            start = mid + 1;
        }
        if (nums[mid] < target)
        {
            start = mid + 1;
        }
        if (nums[mid] > target)
        {
            end = mid;
        }
      }
      res[1] = end - 1;
      return res;
    }
};
```

## 方法二：线性扫描直接两次遍历，寻找左右边界（时间复杂度并不满足要求）
```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
      vector<int> res = {-1,-1};
      int n = nums.size();
      if (nums.empty()) return res; 
      for(int i=0;i<n;i++)
      {
          if(nums[i]==target)
          {
              res[0] = i;
              break;
          }
      }
      if (res[0]==-1) return res;
      for (int j=n-1;j>=0 ;j--)
      {
          if (nums[j]==target) 
          {
              res[1] =j;
              break;
          }
      }
      return res;
    }
};
```



## [Python](LeetCode_Note/python/Find-First-And-Last-Position-Of-Element-In-Sorted-Array/Find-First-And-Last-Position-Of-Element-In-Sorted-Array.py)
### 方法一：两次二分法
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)
        res= [-1,-1]
        if n==0:
            return res
        start = 0
        end = n
        while end > start:
            mid=start + (end-start)//2
            if nums[mid]<target:
                start= mid +1
            else:
                end = mid
        if start == n or nums[start]!=target:
            return res
        res[0]=start
        start = 0
        end = n
        while end > start:
            mid = start + (end-start)//2
            if nums[mid]<=target:
                start=mid+1
            else:
                end = mid
        res[1] =end-1
        return res
```

### 方法二：线性扫描直接两次遍历，寻找左右边界（时间复杂度并不满足要求）
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)
        res= [-1,-1]
        if n==0:
            return res
        for i in range(n):
            if nums[i]== target:
                res[0] = i
                break
        if res[0] == -1:
            return res
        for j in range (n-1,-1,-1):
            if nums[j] == target:
                res[1]=j
                break
        return res
```

# 参考：
 - [二分查找算法](./BinarySearch.md)
