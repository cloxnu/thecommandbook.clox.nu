---
title: 背包问题
---

# 背包问题

[背包问题](https://zh.wikipedia.org/wiki/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98) 是一种组合优化的 NP 完全问题。问题可以描述为：给定一组物品，每种物品都有自己的重量和价格，在限定的总重量内，我们如何选择，才能使得物品的总价格最高。

## 0-1 背包

0-1 背包

有 n 个物品，物品有两个属性，代价 cost，价值 value，背包在容量 space 的限制下可以装物品的最大价值？

假设物品数 n = 3；costs = [1, 2, 5]；values = [3, 7, 15]；space = 7

| j \ dp[j] \ space | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| init | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 3 | 3 | 3 | 3 | 3 | 3 | 3 |
| 1+2 | 0 | 3 | 7 | 10 | 10 | 10 | 10 | 10 |
| 1+2+3 | 0 | 3 | 7 | 10 | 10 | 15 | 18 | 22 |

```python
def zero_one_back(n: int, costs: list, values: list, space: int):
    dp = [0] * (space + 1)

    for i in range(n):  # 0-1 背包问题中，这两层循环不可以交换
        for j in reversed(range(space + 1)):
            if j < costs[i]:
                break  # 这里因为 j 是有序的（越来越小），所以可以用 break，否则用 continue
            dp[j] = max(dp[j], dp[j - costs[i]] + values[i])

    return dp[space]
```

## 完全背包

有 n 个物品，物品的数量无限，物品有两个属性，代价 cost，价值 value，背包在容量 space 的限制下可以装物品的最大价值？

假设物品数 n = 3；costs = [1, 2, 5]；values = [3, 7, 15]；space = 7

| j \ dp[j] \ space | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| init | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 3 | 6 | 9 | 12 | 15 | 18 | 21 |
| 1+2 | 0 | 3 | 7 | 10 | 14 | 17 | 21 | 24 |
| 1+2+3 | 0 | 3 | 7 | 10 | 14 | 17 | 21 | 24 |

```python
def complete_back(n: int, costs: list, values: list, space: int):
    dp = [0] * (space + 1)

    for i in range(n):  # 完全背包问题中，这两层循环可以交换
        for j in range(space + 1):
            if j < costs[i]:
                continue  # 这里是 continue
            dp[j] = max(dp[j], dp[j - costs[i]] + values[i])
        print(dp)

    return dp[space]
```

## 装满 0-1 背包

有 n 个物品，物品有两个属性，代价 cost，价值 value，背包在容量 space 的限制下可以装物品的最 **小** 价值（要求背包必须装满）？

思路：初始化时将除 0 以外的数定义为 'inf'，代表背包无法装到这个容量，若题目改为最大，则初始化为 '-inf'，并将 min 改为 max 即可

假设物品数 n = 4；costs = [2, 3, 5, 7]；values = [3, 5, 7, 9]；space = 10

| j \ dp[j] \ space | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| init | 0 | inf | inf | inf | inf | inf | inf | inf | inf | inf | inf |
| 1 | 0 | inf | 3 | inf | inf | inf | inf | inf | inf | inf | inf |
| 1+2 | 0 | inf | 3 | 5 | inf | 8 | inf | inf | inf | inf | inf |
| 1+2+3 | 0 | inf | 3 | 5 | inf | 7 | inf | 10 | 12 | inf | 15 |
| 1+2+3+4 | 0 | inf | 3 | 5 | inf | 7 | inf | 9 | 12 | 12 | 14 |

```python
def filled_zero_one_back(n: int, costs: list, values: list, space: int):
    dp = [float('inf')] * (space + 1)
    dp[0] = 0

    for i in range(n):  # 0-1 背包问题中，这两层循环不可以交换
        for j in reversed(range(space + 1)):
            if j < costs[i]:
                break  # 这里因为 j 是有序的（越来越小），所以可以用 break，否则用 continue
            dp[j] = min(dp[j], dp[j - costs[i]] + values[i])
        print(dp)

    return dp[space]
```

## 装满完全背包

有 n 个物品，物品的数量无限，物品有两个属性，代价 cost，价值 value，背包在容量 space 的限制下可以装物品的最 **小** 价值（要求背包必须装满）？

思路：初始化时将除 0 以外的数定义为 'inf'，代表背包无法装到这个容量，若题目改为最大，则初始化为 '-inf'，并将 min 改为 max 即可

假设物品数 n = 4；costs = [2, 3, 5, 7]；values = [3, 5, 7, 9]；space = 10

| j \ dp[j] \ space | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| init | 0 | inf | inf | inf | inf | inf | inf | inf | inf | inf | inf |
| 1 | 0 | inf | 3 | inf | 6 | inf | 9 | inf | 12 | inf | 15 |
| 1+2(1) | 0 | inf | 3 | 5 | 6 | inf | 9 | inf | 12 | 15 | 15 |
| 1+2(2) | 0 | inf | 3 | 5 | 6 | 8 | 9 | 11 | 12 | 14 | 15 |
| 1+2+3 | 0 | inf | 3 | 5 | 6 | 7 | 9 | 10 | 12 | 13 | 14 |
| 1+2+3+4 | 0 | inf | 3 | 5 | 6 | 7 | 9 | 9 | 12 | 12 | 14 |

```python
def filled_complete_back(n: int, costs: list, values: list, space: int):
    dp = [float('inf')] * (space + 1)
    dp[0] = 0

    for i in range(n):  # 完全背包问题中，这两层循环可以交换
        for j in range(space + 1):
            if j < costs[i]:
                continue  # 这里是 continue
            dp[j] = min(dp[j], dp[j - costs[i]] + values[i])
        print(dp)

    return dp[space]
```



