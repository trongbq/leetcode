# Search Insert Position

- Problem: https://leetcode.com/problems/search-insert-position/
- Level: Easy

### Explanation

We can easily use O(n) algorithm to find the position of `target` but since the description requires O(logn) algorithm, and input is a sorted array, we need to use binary search to  solve it.

There is a bit extra work when the target is not in the list, and we need to find the position it would be if it's in the list, but it's trivial to implement so.

### Code

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        l, h = 0, len(nums) - 1;

        while l < h:
            m = l + (h - l) // 2
            if nums[m] == target:
                return m
            elif nums[m] > target:
                h = m - 1
            else:
                l = m + 1
        
        # Can not find target in the list
        # Target would be in `l` position or right after `l`
        return l if nums[l] >= target else l + 1
```

### Complexity

- Time: O(logn)
- Space: O(1)
