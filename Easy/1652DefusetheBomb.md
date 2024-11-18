# Problem
You have a bomb to defuse, and your time is running out! Your informer will provide you with a circular array code of length of n and a key k.

To decrypt the code, you must replace every number. All the numbers are replaced simultaneously.

    If k > 0, replace the ith number with the sum of the next k numbers.
    If k < 0, replace the ith number with the sum of the previous k numbers.
    If k == 0, replace the ith number with 0.

As code is circular, the next element of code[n-1] is code[0], and the previous element of code[0] is code[n-1].

Given the circular array code and an integer key k, return the decrypted code to defuse the bomb!

 

Example 1:

Input: code = [5,7,1,4], k = 3
Output: [12,10,16,13]
Explanation: Each number is replaced by the sum of the next 3 numbers. The decrypted code is [7+1+4, 1+4+5, 4+5+7, 5+7+1]. Notice that the numbers wrap around.

Example 2:

Input: code = [1,2,3,4], k = 0
Output: [0,0,0,0]
Explanation: When k is zero, the numbers are replaced by 0. 

Example 3:

Input: code = [2,4,9,3], k = -2
Output: [12,5,6,13]
Explanation: The decrypted code is [3+9, 2+3, 4+2, 9+4]. Notice that the numbers wrap around again. If k is negative, the sum is of the previous numbers.

# Solution
```cpp
class Solution {
public:
    vector<int> decrypt(vector<int>& code, int k) {
        int n = code.size();
        vector<int> ans(n, 0);

        // k = 0
        if (k == 0) return ans;

        for (int i = 0; i < n; i++) {
            code.push_back(code[i]);
        }
        
        if (k > 0) {
            vector<int> prefix(n + n, 0);
            prefix[0] = code[0];
            for (int i = 1; i < n + n; i++)
                prefix[i] = prefix[i - 1] + code[i];
            for (int i = 0; i < n; i++)
                ans[i] = prefix[i + k] - prefix[i];
            return ans;
        }

        vector<int> suffix(n + n, 0);
        suffix[n + n - 1] = code[n + n - 1];

        for (int i = n + n - 2; i >= 0; i--)
            suffix[i] = suffix[i + 1] + code[i];
        for (int i = n; i < n + n; i++)
            ans[i - n] = suffix[i + k] - suffix[i];
        return ans;
    }
};
```
