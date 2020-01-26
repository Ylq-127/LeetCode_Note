# 题目描述:  正则表达式匹配

给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '\*' 的正则表达式匹配。

```
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
```
所谓匹配，是要涵盖**整个**字符串 s的，而不是部分字符串。

**说明 :**

1. s 可能为空，且只包含从 a-z 的小写字母。

2. p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 /*。

**示例 1:**
```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```
**示例 2:**
```
输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```

**示例 3:**
```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```

**示例 4:**
```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```
**示例 5:**
```
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
```

# 解题思路:
此题与剑指offer第19题一样

方法一：回溯法，我们先分析下如何匹配一个字符，现在只考虑字符'.'，不考虑'\*'看一下：

如果字符串和模式串的当前字符相等，那么我们继续匹配它们的下一个字符；如果模式串中的字符是'.'，那么它可以匹配字符串中的任意字符，我们也可以继续匹配它们的下一个字符。

接下来，把字符'\*'考虑进去，它可以匹配任意次的字符，当然出现0次也可以。

我们分两种情况来看：

模式串的下一个字符不是'\*'，也就是上面说的只有字符'.'的情况。 如果字符串中的第一个字符和模式串中的第一个字符相匹配，那么字符串的模式串都向后移动一个字符，然后匹配剩余的字符串和模式串。如果字符串中的第一个字符和模式中的第一个字符不相匹配，则直接返回false。

模式串的下一个字符是'\*'，此时就要复杂一些。 因为可能有多种不同的匹配方式。

选择一：无论字符串和模式串当前字符相不相等，我们都将模式串后移两个字符，相当于把模式串中的当前字符和''忽略掉，因为''可以匹配任意次的字符，所以出现0次也可以。

选择二：如果字符串和模式串当前字符相等，则字符串向后移动一个字符。而模式串此时有两个选择：

1、我们可以在模式串向后移动两个字符，继续匹配；

2、也可以保持模式串不变，这样相当于用字符'\*'继续匹配字符串，也就是模式串中的字符'\*'匹配字符串中的字符多个的情况。

方法二：利用插入排序的思想，外层循环遍历数组，找``0``元素所在的位置；内层循环找当前``0``元素所在位置之后的第一个非零元素。当当前位置的值为``0``时执行内层循环，假设当前位置``i``的值``num[i]=0``，内层循环的``j = i++``，即从当前0元素所在的下一个位置开始查找非零元素；当``nums[j] != 0``时，``nums[i] = nums[j]``，``nums[j]=0``，即交换``nums[i]``、``nums[j]``的值，并退出内层循环，执行外层循环。 时间复杂度(n<sup>2</sup>)

方法三：利用冒泡排序的思想，两层循环，内层从最后开始交换，时间复杂度为O(n<sup>2</sup>)
 
# 时间复杂度：
  方法一： O(n)
  
  方法二： O(nm)
  
# 空间复杂度
  方法一：O(n)
  
  方法二：O(nm)
  
# 代码

## [C++](./Regular-Expression-Matching.cpp):

###  方法一： 构建辅助数组：
```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        if(nums.empty()) return;
        vector<int> res;
        int n = nums.size();
        for (int i =0;i<n;i++)
        {
            if (nums[i]!=0)
              res.push_back(nums[i]);
        }
        for (int i =0;i<n;i++)
        {
            if (nums[i]==0)
              res.push_back(nums[i]);
        }
    }
};
```

###  方法二： 插入排序
```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        if(nums.empty()) return;
        int n = nums.size();
        for (int i =0;i<n;i++)
        {
            if (nums[i]==0)
            {
                for (int j=i+1;j<n;j++)
                {
                    if (nums[j]!=0)
                    {
                        int temp = nums[j];
                        for (int k=j;k>i;k--)
                        {
                            nums[k]=nums[k-1];
                        }
                        nums[i] = temp;
                        break;
                    }
                }
            }
        }    
    }
};
```

## [Python:](https://github.com/bryceustc/LeetCode_Note/blob/master/python/Regular-Expression-Matching/Regular-Expression-Matching.py)
###  方法一：回溯法
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        res = []
        n = len(nums)
        if n==0:
            return
        for i in range(n):
            if nums[i]!=0:
                res.append(nums[i])
        for i in range(n):
            if nums[i]==0:
                res.append(nums[i])
        nums=res
```
### 方法二 ：插入排序
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        if n==0:
            return
        for i in range(0,n):
            if nums[i]==0:
                for j in range(i+1,n):
                    if nums[j]!=0:
                        temp = nums[j]
                        for k in range (j,i,-1):
                            nums[k] = nums[k-1]
                        nums[i] = temp
                        break
```

# 参考

  -  [剑指offer_19题_正则表达式匹配](https://github.com/bryceustc/CodingInterviews/blob/master/RegularExpressionsMatching/README.md)




