724\. Find Pivot Index https://leetcode.com/problems/find-pivot-index/

======================

Problem
-------

We are given an integer array `nums`.

The **pivot index** is defined as the index `i` where:

`sum(nums[i+1..n-1])}sum(nums[0..i-1]) = sum(nums[i+1..n-1])`

-   If `i == 0`, left sum = 0.

-   If `i == n-1`, right sum = 0.

-   Return the **leftmost** pivot index, or `-1` if none exists.

* * * * *

Examples
--------

### Example 1

**Input:**\
`nums = [1,7,3,6,5,6]`

**Process:**

-   Index 0 → left=0, right=27 ≠

-   Index 1 → left=1, right=20 ≠

-   Index 2 → left=8, right=17 ≠

-   Index 3 → left=11, right=11 ✅

**Output:**\
`3`

* * * * *

### Example 2

**Input:**\
`nums = [1,2,3]`

-   No index balances → Output = `-1`.

* * * * *

### Example 3

**Input:**\
`nums = [2,1,-1]`

**Process:**

-   Index 0 → left=0, right=0 ✅

**Output:**\
`0`

* * * * *

Visual Example
--------------

For `nums = [1,7,3,6,5,6]`
```

Index:        0    1    2    3    4    5

Nums:         1    7    3    6    5    6

Left Sum:     0    1    8   11   17   22

Right Sum:   27   20   17   11    6    0
```

Pivot index is `3` (since left=11, right=11).

* * * * *

Approaches
----------

### 1\. Optimized One-Pass (Total Sum + Running Left Sum) ✅

-   Compute total sum.

-   Maintain a running `leftSum`.

-   At each index `i`:

    -   Right sum = `totalSum - leftSum - nums[i]`

    -   If left == right → return `i`.

-   Otherwise, update left sum.

**Code:**
```cpp
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int total = 0;
        for (int x : nums) total += x;

        int left = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (left == total - left - nums[i]) {
                return i;
            }
            left += nums[i];
        }
        return -1;
    }
};
```

-   **Time Complexity:** `O(n)`

-   **Space Complexity:** `O(1)`

* * * * *

### 2\. Prefix Sum Arrays (Brute Force Style)

-   Compute prefix sums from left and right.

-   Check condition for each index.

**Code:**
```cpp
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int n = nums.size();
        vector<int> lP(n), rP(n);

        lP[0] = nums[0];
        for (int i = 1; i < n; i++) {
            lP[i] = lP[i-1] + nums[i];
        }

        rP[n-1] = nums[n-1];
        for (int i = n-2; i >= 0; i--) {
            rP[i] = rP[i+1] + nums[i];
        }

        for (int i = 0; i < n; i++) {
            int left = (i == 0) ? 0 : lP[i-1];
            int right = (i == n-1) ? 0 : rP[i+1];
            if (left == right) return i;
        }
        return -1;
    }
};
```

-   **Time Complexity:** `O(n)`

-   **Space Complexity:** `O(n)`

* * * * *

Summary
-------

| Approach | Time | Space | Notes |
| --- | --- | --- | --- |
| Prefix Sum Arrays | O(n) | O(n) | Stores left & right sums |
| Optimized Scan | O(n) | O(1) | Best approach ✅ |
