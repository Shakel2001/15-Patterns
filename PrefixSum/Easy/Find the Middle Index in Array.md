1991\. Find the Middle Index in Array
=====================================

Problem
-------

We are given a **0-indexed integer array** `nums`.

A **middleIndex** is defined as an index `i` where:

`sum(nums[i+1..n-1])}sum(nums[0..i-1]) = sum(nums[i+1..n-1])`

Special cases:

-   If `i == 0`, left sum is **0**.

-   If `i == n-1`, right sum is **0**.

We need to return the **leftmost middleIndex**, or `-1` if none exists.

* * * * *

Examples
--------

### Example 1

**Input:**\
`nums = [2, 3, -1, 8, 4]`

**Process:**

-   Index 0 → left=0, right=14 ≠

-   Index 1 → left=2, right=11 ≠

-   Index 2 → left=5, right=12 ≠

-   Index 3 → left=4, right=4 ✅

**Output:**\
`3`

* * * * *

### Example 2

**Input:**\
`nums = [1, -1, 4]`

**Process:**

-   Index 0 → left=0, right=3 ≠

-   Index 1 → left=1, right=4 ≠

-   Index 2 → left=0, right=0 ✅

**Output:**\
`2`

* * * * *

### Example 3

**Input:**\
`nums = [2, 5]`

**No index works** → Output = `-1`

* * * * *

Approaches
----------

### 1\. Optimized One-Pass (Using total sum) ✅

-   First compute `totalSum` of array.

-   Maintain a running `leftSum`.

-   At each index `i`:

    -   Right sum = `totalSum - leftSum - nums[i]`

    -   If `leftSum == rightSum` → return `i`.

-   Otherwise, update `leftSum += nums[i]`.

**Code:**
```cpp
class Solution {
public:
    int findMiddleIndex(vector<int>& nums) {
        int totalSum = 0;
        for (int x : nums) totalSum += x;

        int leftSum = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (leftSum == totalSum - leftSum - nums[i]) {
                return i;
            }
            leftSum += nums[i];
        }
        return -1;
    }
};
```

-   **Time Complexity:** `O(n)` (single scan).

-   **Space Complexity:** `O(1)` (no extra array).

* * * * *

### 2\. Prefix Sum Arrays (Left & Right Sums)

-   Compute prefix sums from left (`lP[i] = sum of nums[0..i]`).

-   Compute prefix sums from right (`rP[i] = sum of nums[i..n-1]`).

-   For each index `i`, if `lP[i] == rP[i]`, return `i`.

**Code:**
```cpp
class Solution {
public:
    int findMiddleIndex(vector<int>& nums) {
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
            if (lP[i] == rP[i]) return i;
        }
        return -1;
    }
};
```

-   **Time Complexity:** `O(n)` (prefix + suffix + scan).

-   **Space Complexity:** `O(n)` (extra arrays).

* * * * *

Summary
-------

| Approach | Time | Space | Notes |
| --- | --- | --- | --- |
| Prefix Arrays | O(n) | O(n) | Explicit left & right sums |
| Optimized Scan | O(n) | O(1) | Best approach ✅ |
