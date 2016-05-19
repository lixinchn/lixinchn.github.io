---
layout: post
title: LeetCode 10. Regular Expression Matching
---
### Details
Implement regular expression matching with support for '.' and '*'.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```

### Analysis
Let pointer_s and pointer_p to go through s and p. We meet two conditions:

1. the next position of the pointer_p is not '*'
2. the next position of the pointer_p is '*'

For the first condition:

- If the position of the pointer_p is '.', we continue to match the rest.
- If the position of the pointer_p is equal to the pointer_s, we continue to match the rest.

For the second condition:

- If the position of the pointer_p is '.', we must check from the position of pointer_s to the end of string, because '.*' can match any characters.
- If the position of the pointer_p is not '.', which means it is 'a-b' or '0-9' or other characters, we must check from the position of pointer_s to the position of string which character is not equal pointer_p.

### Return
If pointer_p is at the end of pattern.

1. If pointer_s is at the end of string, it is matching now.
2. Otherwise, it is not matching.

### Optimization
1. Let's look at a pattern 'a*****', it is equal to pattern 'a*', so remove the repeated '*'.
2. Let's look at a pattern 'a*a*a*a*a*', it is equal to pattern 'a*', so remove the repeated 'a*'.


### Python Code
```python
class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        p = self.remove_repeated_star(p)
        return self.do(s, p, 0, 0, len(s), len(p))

    def remove_repeated_star(self, p):
        new_p = ''
        previous_star_char = ''
        i, len_p = 0, len(p)
        while i < len_p:
            if i + 1 < len_p and p[i + 1] == '*' and p[i] == previous_star_char:
                i += 2
                continue
            if p[i] == '*':
                if i > 0:
                    previous_star_char = new_p[-1]
                new_p += p[i]
                i += 1
                while i < len_p and p[i] == '*':
                    i += 1
                continue
            new_p += p[i]
            i += 1
            previous_star_char = ''
        return new_p

    def do(self, s, p, i_s, i_p, len_s, len_p):
        if i_p >= len_p:
            return i_s >= len_s

        if i_p + 1 < len_p and p[i_p + 1] == '*':
            while i_s < len_s and (s[i_s] == p[i_p] or p[i_p] == '.'):
                if self.do(s, p, i_s, i_p + 2, len_s, len_p):
                    return True
                i_s += 1
            return self.do(s, p, i_s, i_p + 2, len_s, len_p)
        else:
            return (i_s < len_s and (p[i_p] == '.' or p[i_p] == s[i_s])) and self.do(s, p, i_s + 1, i_p + 1, len_s, len_p)
```

