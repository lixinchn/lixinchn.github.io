---
layout: post
title: LeetCode 44. Wildcard Matching. Improved.
---

### Details
Implement wildcard pattern matching with support for '?' and '*'.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```

### Analysis
Two pointers: i_s and i_p, which are for traveling string and pattern.

1. s[i_s] == p[i_p], continue to match the rest.
2. p[i_p] == `?`, continue to match the rest.
3. p[i_p] == `*`, which also means match, but `*` means match any sequence of characters. Now save the position of pattern and string and go on matching.
4. s[i_s] != p[i_p]
  - If there is last_star position we saved. Set i_p next position of last_star, set i_s position of next saved position and go on matching.
  - If there is not last_star, return false.


### Python Code
```python
class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        i_s, i_p = 0, 0
        len_s, len_p = len(s), len(p)
        last_star, last_s = -1, -1
        while i_s < len_s:
            if i_p < len_p and (p[i_p] == s[i_s] or p[i_p] == '?'):
                i_p += 1
                i_s += 1
                continue
            if i_p < len_p and p[i_p] == '*':
                last_star = i_p
                i_p += 1
                last_s = i_s
                continue
            if last_star > -1:
                i_p = last_star + 1
                i_s = last_s + 1
                last_s += 1
                continue
            return False

        while i_p < len_p and p[i_p] == '*':
            i_p += 1

        return i_p >= len_p
```


