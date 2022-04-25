# NeetCode with Chinese
## 1. 数组 && Hash
### 1.1  217 存在重复元素
直接定义一个set，遍历一遍，无则添加，有则返回正确
遍历完之后没有返回错误
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        exists = set()
        for i in nums:
            if i not in exists:
                exists.add(i)
            else:
                return True
        return False
```

## 2. 双指针

### 2.5 接雨水
对于接雨水这题，主要是要找到对于当前格子而言，其左边的最大值以及右边的最大值。
取二者之间的较小值与当前的格子值相减，大于0的值就是当前格子可以存储的值。
用数学表达则是
`res = min(leftMax, rightMax) - height[i]`
很自然的想法是利用两个数组来存储每个值的leftMax，和rightMax。
当然这种额外的开销是可以省略的。因为只要知道leftMax 和 rightMax之中较小的那一个，
无论另一个有多大，较小的那个值就是上限。所以每次都去从leftMax 和 rightMax之中找寻最小的那个，然后进行
更新。
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        if not height: return 0
        l, r = 0, len(height) - 1
        leftMax, rightMax = height[l], height[r]
        res = 0
        while l < r:
            if leftMax < rightMax:
                l += 1
                leftMax = max(leftMax, height[l])
                res += leftMax - height[l]
            else:
                r -= 1
                rightMax = max(rightMax, height[r])
                res += rightMax - height[r]
        return res
```

## 3. 滑动窗口
### 3.1 最佳买卖股票时机
1. 当 右边的值大于左边的值的时候 才有利润 才进行操作
2. 当 右边的值小于左边的值的时候 新的最小值出现了 进行更新
3. 每次都要将右边的指针移动
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        l, r = 0 , 1
        max_profit = 0
        while r < len(prices):
            if prices[l] < prices[r]:
                profit = prices[r] - prices[l]
                max_profit = max(max_profit, profit)
            else:
                l = r
            r += 1
        return max_profit
```
### 3.2 最大的无重复子串

滑动窗口的题，右指针往右滑动，当发现有重复的元素的时候，持续移动左边的指针，
直到没有重复元素为止，每次添加新的元素之后，更新结果的大小。
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_set = set()
        l, r = 0, 0
        res = 0
        while r < len(s):
            while s[r] in char_set:
                char_set.remove(s[l])
                l += 1
            char_set.add(s[r])
            r += 1
            res = max(res, len(char_set))
        return res
```
