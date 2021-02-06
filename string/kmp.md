# Kmp

KMP \(Knuth-Morris-Pratt\) algorithm is a substring search algorithm.

LPS: Longest proper Prefix which is also Suffix. For example, `"abcdeabc"` has the LPS `"abc"`. Note that the string itself can't be an LPS of itself.

## Algorithm

1. Generate the LPS array.
2. Take advantage of the LPS array so that once we find a mismatch, we can efficiently jump to an earlier point and continue searching.

For the pattern string `t` we are searching, the `lps` array is of the same length as `t`. `lps[i]` is the length of LPS of `t[0..i]`.

Example:

```
t   = a b a b x a b a b x g
lps = 0 0 1 2 0 1 2 3 4 5 0
```

## Implementation

```cpp
// OJ: https://leetcode.com/problems/implement-strstr/
// Author: github.com/lzl124631x
// Time: O(M+N)
// Space: O(N)
class Solution {
    vector<int> getLps(string &s) {
        int N = s.size();
        vector<int> lps(N);
        for (int i = 1; i < N; ++i) {
            int j = lps[i - 1];
            while (j > 0 && s[i] != s[j]) j = lps[j - 1];
            if (s[i] == s[j]) ++j;
            lps[i] = j;
        }
        return lps;
    }
public:
    int strStr(string s, string t) {
        if (t.empty()) return 0;
        int M = s.size(), N = t.size(), i = 0, j = 0;
        auto lps = getLps(t);
        while (i < M) {
            if (s[i] == t[j]) {
                ++i;
                ++j;
            }
            if (j == N) return i - j;
            if (i < M && s[i] != t[j]) {
                if (j) j = lps[j - 1];
                else ++i;
            }
        }
        return -1;
    }
};
```

## Problems

* [28. Implement strStr\(\) \(Easy\)](https://leetcode.com/problems/implement-strstr/)
* [214. Shortest Palindrome \(Hard\)](https://leetcode.com/problems/shortest-palindrome/)
* [1392. Longest Happy Prefix \(Hard\)](https://leetcode.com/problems/longest-happy-prefix/)
* [459. Repeated Substring Pattern (Easy)](https://leetcode.com/problems/repeated-substring-pattern/)

## Reference

* [https://youtu.be/GTJr8OvyEVQ](https://youtu.be/GTJr8OvyEVQ)
* [https://www.geeksforgeeks.org/kmp-algorithm-for-pattern-searching/](https://www.geeksforgeeks.org/kmp-algorithm-for-pattern-searching/)

