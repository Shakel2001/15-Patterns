[1480\. Running Sum of 1d Array](https://leetcode.com/problems/running-sum-of-1d-array/)
========================================================================================

Problem
-------

Given an array `nums`. We define a **running sum** of an array as:

`runningSum[i]=nums[0]+nums[1]+⋯+nums[i]`

Return the running sum of `nums`.

* * * * *

Examples
--------

### Example 1

**Input:**\
`nums = [1, 2, 3, 4]`

**Process (step by step):**

-   `runningSum[0] = 1`

-   `runningSum[1] = 1 + 2 = 3`

-   `runningSum[2] = 1 + 2 + 3 = 6`

-   `runningSum[3] = 1 + 2 + 3 + 4 = 10`

**Output:**\
`[1, 3, 6, 10]`

**Visual:**

`nums:        [1,  2,  3,  4]
runningSum:  [1,  3,  6, 10]`

* * * * *

### Example 2

**Input:**\
`nums = [1, 1, 1, 1, 1]`

**Output:**\
`[1, 2, 3, 4, 5]`

**Visual:**

`nums:        [1, 1, 1, 1, 1]
runningSum:  [1, 2, 3, 4, 5]`

* * * * *

### Example 3

**Input:**\
`nums = [3, 1, 2, 10, 1]`

**Output:**\
`[3, 4, 6, 16, 17]`

**Visual:**

`nums:        [3,  1,  2, 10,  1]
runningSum:  [3,  4,  6, 16, 17]`

* * * * *

Logic / Approach
----------------

This is a **prefix sum problem**.

-   Start with a variable `s = 0` (cumulative sum).

-   Traverse the array.

-   For each index `i`:

    -   Add `nums[i]` to `s`.

    -   Store `s` in `runningSum[i]`.

This way, each position holds the sum of all previous elements including itself.

* * * * *

Code (C++)
----------
```cpp
class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {
        vector<int> prefixsum(nums.size());
        int s = 0;
        for (int i = 0; i < nums.size(); i++) {
            s += nums[i];           // accumulate sum
            prefixsum[i] = s;       // store running sum
        }
        return prefixsum;
    }
};
```

* * * * *

Complexity Analysis
-------------------

-   **Time Complexity:** `O(n)`

    -   We traverse the array once.

-   **Space Complexity:** `O(n)` (extra array `prefixsum`)

    -   Can be optimized to `O(1)` if we modify `nums` in-place.
