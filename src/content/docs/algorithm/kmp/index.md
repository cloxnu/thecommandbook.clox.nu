---
title: KMP 算法
---

# KMP 算法

[Knuth-Morris-Pratt 字符串查找算法](https://zh.wikipedia.org/wiki/KMP%E7%AE%97%E6%B3%95) （简称为 KMP 算法）可在一个字符串 `S` 内查找一个词 `W` 的出现位置。一个词在不匹配时本身就包含足够的信息来确定下一个匹配可能的开始位置，此算法利用这一特性以避免重新检查先前匹配的字符。

以下示例是在 `s` 中寻找 `p`

样例：

| `p` | a | b | a | b | a | b | c | a |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| `idx` | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| PMT | 0 | 0 | 1 | 2 | 3 | 4 | 0 | 1 |
| `next` | -1 | 0 | 0 | 1 | 2 | 3 | 4 | 0 |

PMT (Partial Match Table) 部分匹配表，是指字符串的前缀集合与后缀集合的交集中最长元素的长度

比如 ababab 的所有前缀集合和后缀集合中交集最长元素是 前四个 "abab" 和后四个 "abab"，所以 `idx` 为 5 的 'b' 的 PMT 为 4

`next` 数组是指，当当前 `pattern` 指针指向的值失配时，需要将当前指针指向对应索引的值继续匹配

```python
def get_next(p: str) -> list:
    next = [0] * len(p)
    next[0] = -1
    i, j = 0, -1
    while i < len(p) - 1:
        if j == -1 or p[i] == p[j]:
            i += 1
            j += 1
            next[i] = j
        else:
            j = next[j]
    return next


def kmp(s: str, p: str) -> int:
    i, j = 0, 0
    next = get_next(p)
    while i < len(s) and j < len(p):
        # print(i, s[i], j, p[j])
        if j == -1 or s[i] == p[j]:
            i += 1
            j += 1
        else:
            j = next[j]  # 失配，调转去 next 里指向的值继续匹配
    if j == len(p):  # 匹配成功
        return i - j
    return -1
```

