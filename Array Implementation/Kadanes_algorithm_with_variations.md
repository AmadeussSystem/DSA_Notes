## ⚡ Kadane’s Algorithm Implementations in C++

```cpp
#include <vector>
#include <algorithm>

using std::vector;
using std::max;
using std::min;

// 🔹 Brute Force: O(n^2)
int bruteForce(vector<int>& nums) {
    int maxSum = nums[0];
    for (int i = 0; i < nums.size(); i++) {
        int curSum = 0;
        for (int j = i; j < nums.size(); j++) {
            curSum += nums[j];
            maxSum = max(maxSum, curSum);
        }
    }
    return maxSum;
}

// 🔹 Kadane's Algorithm: O(n)
int kadanes(vector<int>& nums) {
    int maxSum = nums[0];
    int curSum = 0;
    for (int n : nums) {
        curSum = max(curSum, 0);
        curSum += n;
        maxSum = max(maxSum, curSum);
    }
    return maxSum;
}

// 🔹 Sliding Window Variation of Kadane's: returns start & end index of max subarray
vector<int> slidingWindow(vector<int> nums) {
    int maxSum = nums[0];
    int curSum = 0;
    int maxL = 0, maxR = 0;
    int L = 0;

    for (int R = 0; R < nums.size(); R++) {
        if (curSum < 0) {
            curSum = 0;
            L = R;
        }
        curSum += nums[R];
        if (curSum > maxSum) {
            maxSum = curSum;
            maxL = L;
            maxR = R;
        }
    }
    return vector<int>{maxL, maxR};
}

// 🔹 Circular Array Variation of Kadane's
int maxSubarraySumCircular(vector<int>& nums) {
    int totalSum = 0;
    int curMax = 0, maxSum = nums[0];
    int curMin = 0, minSum = nums[0];

    for (int num : nums) {
        curMax = max(curMax + num, num);
        maxSum = max(curMax, maxSum);

        curMin = min(curMin + num, num);
        minSum = min(curMin, minSum);

        totalSum += num;
    }

    // If all numbers are negative, maxSum will be the maximum element.
    if (maxSum < 0) return maxSum;

    // Otherwise, take the max of (normal Kadane) and (circular wraparound)
    return max(maxSum, totalSum - minSum);
}

// 🔹 Maximum Product Subarray (Kadane's-style with negatives flip)
int maxProduct(vector<int>& nums) {
    int curMaxProd = nums[0];
    int curMinProd = nums[0];
    int maxProd = nums[0];

    for (int i = 1; i < nums.size(); i++) {
        int num = nums[i];

        int tempMax = max({num, curMaxProd * num, curMinProd * num});
        int tempMin = min({num, curMaxProd * num, curMinProd * num});

        curMaxProd = tempMax;
        curMinProd = tempMin;

        maxProd = max(maxProd, curMaxProd);
    }

    return maxProd;
}
