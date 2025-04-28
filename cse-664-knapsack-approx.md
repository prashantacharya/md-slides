---
marp: true
title: Knapsack Problem
author: Prashant Acharya
page-margin: 30px
---

# Knapsack Problem

### By Prashant Acharya

---

# Problem Definition

Given:

- $n$ items: $\{(w_i, v_i)\}$
- Knapsack capacity $W$

Find subset $S \subseteq \{1, ..., n\}$ maximizing:

$$
\sum_{i \in S} v_i
$$

Subject to:

$$
\sum_{i \in S} w_i \leq W
$$

---

# Practical Use Cases

- Resource allocation
- Project scheduling
- Network optimization
- Warehouse logistics

Efficient solutions have major real-world impacts.

---

# Complexity Class

- **Knapsack is NP-Hard**: exploring all subsets leads to exponential time.
- **Pseudo-polynomial solution** exists via **Dynamic Programming (DP)** in $O(nW)$ time.

---

# Dynamic Programming Approach

1. Define $dp[i][w]$ = max value using first $i$ items within weight $w$.
2. Recurrence:
   - If $w_i \leq w$:
     $$
     dp[i][w] = \max(dp[i-1][w], dp[i-1][w-w_i] + v_i)
     $$
   - Else:
     $$
     dp[i][w] = dp[i-1][w]
     $$

---

# Example: Exact DP

Items:

- Item 1: (Weight=3, Value=4)
- Item 2: (Weight=2, Value=3)
- Item 3: (Weight=4, Value=5)

Capacity $W = 7$

---

# Example: Exact DP Table

| Item \ Capacity | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| --------------- | --- | --- | --- | --- | --- | --- | --- | --- |
| None            | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| Item 1          | 0   | 0   | 0   | 4   | 4   | 4   | 4   | 4   |
| Item 2          | 0   | 0   | 3   | 4   | 4   | 7   | 7   | 7   |
| Item 3          | 0   | 0   | 3   | 4   | 5   | 7   | 8   | 9   |

Optimal Value: **9**

---

# Limitation of Standard DP

- Time complexity $O(nW)$.
- If $W$ or item values are very large ➔ **slow**.

We need an **approximate** solution that is **faster** when values are large.

---

# Approximate DP Solution

### Value Scaling Idea:

1. Choose an error margin $\epsilon > 0$.
2. Set scaling factor:
   $$
   K = \frac{\epsilon \times V_{\text{max}}}{n}
   $$
3. Scale down values:
   $$
   v'_i = \left\lfloor \frac{v_i}{K} \right\rfloor
   $$
4. Run modified DP using scaled $v'_i$.
5. Approximate solution within $(1 - \epsilon)$ of the optimal.

---

# Key Difference: Standard vs Approximate DP

| Aspect                | Standard DP               | Approximate DP (Value Scaling)   |
| :-------------------- | :------------------------ | :------------------------------- |
| DP Table Dimension    | Based on **weight** ($w$) | Based on **scaled value** ($v'$) |
| Table Entry Meaning   | Max value for weight $w$  | Min weight for value $v'$        |
| What We Fill In Table | **Values**                | **Weights**                      |
| Goal                  | Maximize value            | Minimize weight                  |

---

# New Example for Approximate DP

Items:

| Item | Weight | Value |
| :--: | :----: | :---: |
|  1   |   2    |  300  |
|  2   |   3    |  500  |
|  3   |   4    |  600  |

Capacity $W = 5$.

Values are large → Approximate DP can save time!

---

# Approximate DP Scaling

- Set $\epsilon = 0.5$ (50% error margin).
- Scaling factor:
  $$
  K = \frac{0.5 \times 600}{3} = 100
  $$
- Scaled values:

| Item | Original Value | Scaled Value |
| :--: | :------------: | :----------: |
|  1   |      300       |      3       |
|  2   |      500       |      5       |
|  3   |      600       |      6       |

---

# Approximate DP Table Update Rule

For each item $i$ and each scaled value $v'$:

$$
dp[i][v'] = \min\left( dp[i-1][v'],\ dp[i-1][v' - v'_i] + w_i \right)
$$

- $v'_i$ = scaled value of item $i$
- $w_i$ = weight of item $i$
- Apply only if $v' - v'_i \geq 0$

✅ Choose the minimum between:

- Not picking item $i$
- Picking item $i$ (if possible)

---

# Approximate DP Table

| Item (Scaled Value, Weight) | 0   | 1   | 2   | 3   | 4   | 5   | 6   | ... | 14  |
| --------------------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| None                        | 0   | ∞   | ∞   | ∞   | ∞   | ∞   | ∞   | ... | ∞   |
| Item 1 (3, 2)               | 0   | ∞   | ∞   | 2   | ∞   | ∞   | ∞   | ... | ∞   |
| Item 2 (5, 3)               | 0   | ∞   | ∞   | 2   | ∞   | 3   | ∞   | ... | ∞   |
| Item 3 (6, 4)               | 0   | ∞   | ∞   | 2   | ∞   | 3   | 4   | ... | ∞   |

✅ Now, we fill **minimum weight** needed to reach scaled value!

---

# Summary

| Approach       | Time Complexity   | Table Size         | Accuracy               |
| -------------- | ----------------- | ------------------ | ---------------------- |
| Standard DP    | $O(nW)$           | $n × W$            | Exact                  |
| Approximate DP | $O(n^2/\epsilon)$ | $n × (n/\epsilon)$ | $(1-\epsilon)$ Optimal |

- **Standard DP**: Accurate but slow if $W$ or values are huge.
- **Approximate DP**: Faster when values are large, small accuracy trade-off.

---

# Backtracking in Approximate DP

✅ After filling the DP table:

- Find the **largest scaled value $v^*$** such that:

$$
dp[n][v^*] \leq W
$$

- Then, **backtrack** to find selected items.

---

# Backtracking Steps

1. Set `current_value = v^*`
2. For each item $i$ from $n$ down to $1$:
   - If:
     $$
     dp[i][\text{current\_value}] == dp[i-1][\text{current\_value}]
     $$
     ➔ **Item $i$ not taken**
   - Else:
     ➔ **Item $i$ taken**
     - Update:
       $$
       \text{current\_value} = \text{current\_value} - v'_i
       $$

✅ Repeat until `current_value = 0`

---

# Backtracking Intuition

- **No change** in DP value ➔ **item not taken**.
- **Change** in DP value ➔ **item was taken**, and we "pay" the scaled value.
- Walk backward until no value remains.

🎯 This reconstructs the selected subset!

---

# Error Margin in Approximate DP

✅ If you choose an error margin $\epsilon$:

- The approximate solution value is guaranteed to be at least:

$$
(1 - \epsilon) \times (\text{Optimal Value})
$$

---

For example:

- $\epsilon = 0.3$ (30% margin)
- Optimal value = 100

Then the approximate value is at least:

$$
(1 - 0.3) \times 100 = 70
$$

✅ You lose **at most 30 units** of value compared to the optimum.

✅ **Knapsack weight limit is still respected** — only value is affected!

---

# Thank You!

### Any questions?
