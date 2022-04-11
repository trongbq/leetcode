# Search Insert Position

- Problem: https://leetcode.com/problems/maximum-subarray/
- Level: Easy

### Explanation

The bruteforce solution would be trying to get sum of all possible contiguous arrays and get the maximum value of those sums.

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        N = len(nums)
        
        ms = float('-inf')
        cs = None
        
        # Iterate through all contiguous subarrays and select one with max sum
        for i in range(1, N+1):
            for j in range(0, N-i+1):
                s = sum(nums[j:j+i])
                if s > ms:
                    ms = s
                    cs = nums[j:j+i]
                    
        return ms
```

That approach has O(n^3) time complexity due to two loops and one more loop for calculating sum. We can avoid the third loop by reusing sum of previous contiguous array: sum of current contiguous subarray is equal to sum of previous contiguous subarray substract by its first tiem and plus last item or current subarray. This will reduce time complexity to O(n^2).

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        N = len(nums)
        
        ms = float('-inf')
        cs = None
        
        # Iterate through all contiguous subarrays and select one with max sum
        for i in range(1, N+1):
            prev_sum = 0
            for j in range(0, N-i+1):
                # Cache sum value by keeping previous sum and move to one position
                s = sum(nums[j:j+i]) if j == 0 else prev_sum - nums[j-1] + nums[j+i-1]
                if s > ms:
                    ms = s
                    cs = nums[j:j+i]
                prev_sum = s
                
        return ms
```

We can do better, notice contiguous subarray that ends at position `i` has maximum sum either `nums[i]` or `nums[i]` plus max sum of contiguous subarray that ends at position `i-1`. If max sum of contiguous subarray that ends at position `i-1` is positive, then we can combine that subarray and current position to form a new subarray that has sum greater than current value at position `i`.

This is recursive pattern:

```
max_sum(nums, n) = max(nums[n], nums[n] + max_sum(n-1))
```

### Code

Use iterative approach to solve,we have:

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        N = len(nums)
        
        ms = nums[0]
        prev = nums[0]
        for i in range(1, N):
            s = prev + nums[i] if prev > 0 else nums[i]
            if s > ms:
                ms = s
            prev = s
                
        return ms
```

### Complexity

- Time: O(n)
- Space: O(1)
