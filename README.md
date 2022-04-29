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

### 1.2 242 有效的异位词
创建两个字典，添加元素之后返回字典
(python3)中字典已经有序
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        set_s = {}
        set_t = {}
        for i in s:
            set_s[i] = set_s.get(i, 0) + 1
        for i in t:
            set_t[i] = set_t.get(i, 0) + 1
        return set_s == set_t
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
### 3.3 424. 替换后的最长重复字符
1. 和最大无无重复字串很像，最大无重复字串的判断条件是是否存在有重复，而此题的判断的条件是
经过k次替换之后是否符合条件。
2. 记录出现次数最多的字母频次为 maxf, 那么 窗口值 (r - l + 1) - maxf  <= k 就是符合条件的
3. 当右指针滑动之后，更新count里面的值，计算 maxf 的值 然后更新窗口值，直到满足条件，满足条件之后更新结果的值

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        count = {}
        l = 0
        res = 0
        for r in range(len(s)):
            count[s[r]] = 1 + count.get(s[r], 0)
            maxf = max(count.values())
            while (r - l + 1) - maxf > k:
                count[s[l]] -= 1
                l += 1
            res = max(res, r - l + 1)
        return res
```

### 3.4 567字符串的排列
首先要保证两个滑动串口的大小一致，二者同步滑动
dict1 = {a:0, b:1} 和 dict2 = {b:1}是不一样的，所以用数组来模拟其实可能会更简单
左右指针同时右移一格，保证滑动窗口不变
右指针移动之后 更新 s2list (+)
左指针移动之后 更新 s1list (-)
每次都判断 s1list == s2list
```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        if len(s1) > len(s2):
            return False
        l = 0
        r = len(s1)
        s1_list = [0] * 26
        s2_list = [0] * 26
        for i in range(len(s1)):
            s1_list[ord(s1[i])- ord('a')] += 1
            s2_list[ord(s2[i]) - ord('a')] += 1
        if s1_list == s2_list:
            return True
        for r in range(r, len(s2)):
            if s1_list == s2_list:
                return True
            s2_list[ord(s2[r]) - ord('a')] += 1
            s2_list[ord(s2[l]) - ord('a')] -= 1
            l += 1
        if s1_list == s2_list:
            return True
        return False
```
