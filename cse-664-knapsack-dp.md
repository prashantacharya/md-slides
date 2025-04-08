---
marp: true
title: Knapsack Problem
author: Prashant Acharya
page-margin: 30px
---

# Knapsack Problem

### By Prashant Acharya

---

# Definition

Given:

- A set of $n$ items: $\{(w_1, v_1), (w_2, v_2), ..., (w_n, v_n)\}$, where:
  - $w_i$ is the weight of item $i$
  - $v_i$ is the value of item $i$
- A knapsack with a maximum capacity $W$

The goal is to find a subset $S \subseteq \{1, ..., n\}$ that maximizes:

$$
\sum_{i \in S} v_i
$$

---

# Definition

Subject to:

$$
\sum_{i \in S} w_i \leq W
$$

---

# Practical Use Cases

- The knapsack problem is used in many real-world applications, such as:
  - Resource allocation in project management
  - Scheduling tasks in a project
  - Optimizing the allocation of resources in a network
  - Optimizing the allocation of resources in a warehouse

Therefore, solving it efficiently is crucial for many practical applications.

---

# Complexity Class Analysis

The knapsack problem is **NP-Hard** because we are evaluating all possible permutations of items and their corresponding values and weights. This results in an exponential time complexity, as the problem requires checking all subsets of items.

---

# Transition to Pseudo-Polynomial Time

While the knapsack problem is NP-Hard, it can be solved in **pseudo-polynomial time** using **dynamic programming (DP)**. This approach helps in finding an optimal solution in $O(nW)$ time, where $n$ is the number of items and $W$ is the maximum weight.

The detailed analysis of this approach will follow as we dive deeper into the algorithm.

---

# Algorithm for DP

Here is the dynamic programming algorithm for the 0/1 knapsack problem:

1. **Initialization**:
   - Create a matrix $dp[i][w]$ where $i$ represents the first $i$ items and $w$ represents the weight capacity.
   - $dp[i][w]$ stores the maximum value achievable with the first $i$ items and a capacity $w$.

---

# Algorithm for DP

2. **Recurrence**:
   - For each item $i$ and each weight capacity $w$:
     - If $w_i \leq w$, we can either include or exclude the item:
       $$
       dp[i][w] = \max(dp[i-1][w], dp[i-1][w-w_i] + v_i)
       $$
     - Otherwise, exclude the item:
       $$
       dp[i][w] = dp[i-1][w]
       $$

---

# Example: Filling the DP Matrix

Let’s consider the following items:

- Item 1: Weight = 3, Value = 4
- Item 2: Weight = 2, Value = 3
- Item 3: Weight = 4, Value = 5

And the knapsack capacity $W = 7$.

The process to fill the DP matrix is as follows:

1. For $dp[1][w]$, check if item 1 can fit for each $w$ and update the value.
2. Continue filling the matrix for items 2 and 3.

This results in the final matrix as shown in the next slide.

---

# Example: DP Matrix Structure

For the knapsack problem, the DP matrix is structured as follows:

| Item \ Capacity            | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| -------------------------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Item 0 (no items)          | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| Item 1 (Weight=3, Value=4) | 0   | 0   | 0   | 4   | 4   | 4   | 4   | 4   |
| Item 2 (Weight=2, Value=3) | 0   | 0   | 3   | 4   | 4   | 4   | 7   | 7   |
| Item 3 (Weight=4, Value=5) | 0   | 0   | 3   | 4   | 5   | 5   | 7   | 8   |

- The rows represent the items, and the columns represent the weight capacity from 0 to the maximum weight.

---

# Backtracking to Find Optimal Values

To determine which items should be included in the optimal solution, we backtrack from the last cell in the DP matrix:

1. Start at $dp[n][W]$.
2. If $dp[n][W] \neq dp[n-1][W]$, it means item $n$ is included.
3. Subtract the weight of the included item from $W$ and continue backtracking.

Repeat the process for all items, and you’ll obtain the optimal subset of items to include in the knapsack.

---

# Example of Backtracking

We will backtrack from $dp[3][7] = 8$ to find which items are included in the optimal solution.

---

# Backtracking Step 1

Start with $dp[3][7] = 8$.

- $dp[3][7] \neq dp[2][7] = 7$, meaning item 3 is **included**.
- Subtract the weight of item 3: $W = 7 - 4 = 3$.

Move to $dp[2][3]$.

---

# Backtracking Step 2

Now at $dp[2][3] = 4$:

- $dp[2][3] \neq dp[1][3] = 4$, meaning item 2 is **not** included.
- Move to $dp[1][3]$.

---

# Final Backtracking Step

At $dp[1][3] = 4$:

- $dp[1][3] \neq dp[0][3] = 0$, meaning item 1 is **included**.
- Subtract the weight of item 1: $W = 3 - 3 = 0$.

---

# Optimal Solution

After backtracking, the items included in the optimal solution are:

- Item 1 (Weight = 3, Value = 4)
- Item 3 (Weight = 4, Value = 5)

Thus, the optimal subset of items that maximizes the knapsack value is **Item 1 and Item 3**.

---

# Pseudo-Polynomial Time Solution

The dynamic programming solution to the knapsack problem is **pseudo-polynomial** because:

1. The time complexity is $O(nW)$, where $n$ is the number of items and $W$ is the maximum weight of the knapsack.
2. If $W$ is large, this approach could still take a long time to run, even though the number of items $n$ is relatively small.
3. The problem is not polynomial with respect to the input size, but it is polynomial with respect to the problem parameters $n$ and $W$.

---

# Thank You!

### Any questions?
