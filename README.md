# NeetCode with Chinese

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
