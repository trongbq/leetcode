# Two Sum

- Problem: https://leetcode.com/problems/two-sum/
- Level: Easy

### Explanation

A clear approach we can see right away is to use two loops to check sum of every possible pairs that might match the target sum. The time complexity is O(n^2).

```python
while i < N - 1:
    j = i+1
    while j < N:
        if nums[i] + nums[j] == target:
            return [i, j]
        j += 1
    i += 1
```

We can use a hash to reduce the time spent on looking for second number, the time complexity is O(n) and space complexity to O(n).

### Code

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        N = len(nums)
        i = 0
        
        while i < N - 1:
            j = i+1
            while j < N:
                if nums[i] + nums[j] == target:
                    return [i, j]
                j += 1
            i += 1
            
        return [-1, -1]
```

### Complexity

- Time: O(n^2)
- Space: O(1)
