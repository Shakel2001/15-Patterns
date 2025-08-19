[1732\. Find the Highest Altitude](https://leetcode.com/problems/find-the-highest-altitude/description/)
========================================================================================================

Problem
-------

A biker goes on a road trip.

-   There are `n+1` points at different altitudes.

-   The biker starts at **point 0** with **altitude = 0**.

-   You are given an integer array `gain` of length `n` where:

    `gain[i]=net gain in altitude between point i and point i+1`

Return the **highest altitude** reached during the trip.

* * * * *

Examples
--------

### Example 1

**Input:**\
`gain = [-5, 1, 5, 0, -7]`

**Process (step by step altitudes):**

-   Start at 0

-   0 + (-5) = -5

-   -5 + 1 = -4

-   -4 + 5 = 1

-   1 + 0 = 1

-   1 + (-7) = -6

**Altitudes:** `[0, -5, -4, 1, 1, -6]`

**Highest altitude = 1**

* * * * *

### Example 2

**Input:**\
`gain = [-4, -3, -2, -1, 4, 3, 2]`

**Altitudes:**\
`[0, -4, -7, -9, -10, -6, -3, -1]`

**Highest altitude = 0**

* * * * *

Approaches
----------

### 1\. Brute Force (Compute full altitude array)

-   Start with an array `altitudes` of size `n+1`.

-   `altitudes[0] = 0` (starting point).

-   For each `i`, compute `altitudes[i+1] = altitudes[i] + gain[i]`.

-   Scan the array to find the maximum altitude.

**Code:**
```cpp
class Solution {
public:
    int largestAltitude(vector<int>& gain) {
        int n = gain.size();
        vector<int> altitudes(n+1, 0); // store all altitudes
        altitudes[0] = 0;

        for (int i = 0; i < n; i++) {
            altitudes[i+1] = altitudes[i] + gain[i];
        }

        int maxAlt = altitudes[0];
        for (int alt : altitudes) {
            maxAlt = max(maxAlt, alt);
        }

        return maxAlt;
    }
};
```

-   **Time Complexity:** `O(n)` (two passes: build + find max).

-   **Space Complexity:** `O(n)` (store altitudes).

* * * * *

### 2\. Prefix Sum Solution (Cleaner, still using array)

We can directly use a **prefix sum array** like in [Running Sum](https://leetcode.com/problems/running-sum-of-1d-array/).

**Code:**
```cpp
class Solution {
public:
    int largestAltitude(vector<int>& gain) {
        int n = gain.size();
        vector<int> prefix(n+1, 0);  // store cumulative altitudes
        prefix[0] = 0;

        for (int i = 0; i < n; i++) {
            prefix[i+1] = prefix[i] + gain[i];
        }

        return *max_element(prefix.begin(), prefix.end());
    }
};
```

-   **Time Complexity:** `O(n)`

-   **Space Complexity:** `O(n)`

* * * * *

### 3\. Optimized Solution (No extra array, just track max) ✅

Instead of storing all altitudes, just maintain:

-   `s` = current altitude

-   `maxAlt` = maximum seen so far

**Code:**
```cpp
class Solution {
public:
    int largestAltitude(vector<int>& gain) {
        int s = 0, maxAlt = 0;
        for (int g : gain) {
            s += g;                  // update current altitude
            maxAlt = max(maxAlt, s); // update max altitude
        }
        return maxAlt;
    }
};
```

-   **Time Complexity:** `O(n)`

-   **Space Complexity:** `O(1)`

* * * * *

Summary
-------

| Approach | Time | Space | Notes |
| --- | --- | --- | --- |
| Brute Force | O(n) | O(n) | Store all altitudes, then scan |
| Prefix Sum | O(n) | O(n) | Use prefix array, max element |
| Optimized | O(n) | O(1) | Track running sum + max only ✅ |
