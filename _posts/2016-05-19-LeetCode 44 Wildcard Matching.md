---
layout: post
title: LeetCode 44. Wildcard Matching
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
Use dynamic programing.
dj[i][j] = 

1. dj[i - 1][j - 1] if p[i] == s[j] or p[i] == '?'
2. dj[i - 1][j] or dj[i][j - 1] if p[i] == '*'
3. false

### Optimization
Let's look at a pattern `a*****`, it is equal to pattern `a*`, so remove the repeated `*`.

### Python Code
```python
class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        
        
        # dj[i][j] = dj[i - 1][j - 1] if p[i] == s[j] or p[i] == '?'
        #         = dj[i - 1][j] or dj[i][j - 1] if p[i] == '*'
        #         = False
        p = self.pre_treat_p(p)
        dj = []
        for i in range(len(s)):
            dj.append([])
            for j in range(len(p)):
                if p[j] == '?':
                    if i == 0 and j == 0:
                        dj[i].append(1)
                    elif i == 0 or j == 0:
                        dj[i].append(0)
                    else:
                        dj[i].append(dj[i - 1][j - 1])
                elif p[j] == '*':
                    if i == 0 and j == 0:
                        dj[i].append(1)
                    elif i == 0:
                        dj[i].append(dj[i][j - 1])
                    elif j == 0:
                        dj[i].append(dj[i - 1][j])
                    else:
                        dj[i].append(dj[i - 1][j] or dj[i][j - 1])
                else:
                    if s[i] == p[j]:
                        if i == 0 and j == 0:
                            dj[i].append(1)
                        elif i == 0 or j == 0:
                            dj[i].append(0)
                        else:
                            dj[i].append(dj[i - 1][j - 1])
                    else:
                        dj[i].append(0)

        if dj and dj[0]:
            return dj[-1][-1]
        if dj and not dj[0]:
            return False

        if not s:
            if not p or p == '*':
                return True
        return False



    def pre_treat_p(self, p):
        i = 0
        new_p = ''
        while i < len(p):
            if p[i] == '*':
                new_p += p[i]
                while i < len(p) and p[i] == '*':
                    i += 1
                continue
            new_p += p[i]
            i += 1
        return new_p
```
