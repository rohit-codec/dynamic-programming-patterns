# Dynamic Programming (DP)

Dynamic Programming (DP) is an algorithmic technique used to solve problems by breaking them into smaller subproblems, solving each subproblem once, and storing its result for future use.

---

## 1. What is Dynamic Programming?

Dynamic Programming is mainly useful when a problem has:

1. **Overlapping Subproblems**
2. **Optimal Substructure**

### Overlapping Subproblems

The same smaller problem is solved multiple times.

Example:

```text
                fib(5)
               /      \
           fib(4)      fib(3)
           /   \       /   \
       fib(3) fib(2) fib(2) fib(1)
```

`fib(3)` and `fib(2)` are calculated repeatedly.

DP stores their answers:

```cpp
dp[2] = 1;
dp[3] = 2;
```

---

### Optimal Substructure

The optimal answer to a larger problem can be constructed from optimal answers to smaller subproblems.

---

# 2. Why Use DP?

DP is useful when:

* Brute force has many repeated calculations.
* The same subproblems occur multiple times.
* The problem asks for:

  * Minimum
  * Maximum
  * Number of ways
  * Possibility
  * Longest
  * Shortest
  * Maximum profit
  * Minimum cost

Common clues:

```text
Minimum ...
Maximum ...
Number of ways ...
Can we ...
Is it possible ...
Longest ...
Shortest ...
```

---

# 3. DP vs Other Techniques

| Technique           | Main Idea                               |
| ------------------- | --------------------------------------- |
| Recursion           | Break into smaller problems             |
| Backtracking        | Try all possible choices                |
| Greedy              | Choose the locally best option          |
| Divide & Conquer    | Solve independent subproblems           |
| Dynamic Programming | Store and reuse overlapping subproblems |

---

# 4. How to Identify DP

Look for these patterns:

### 1. Number of Ways

```text
How many ways can...
Count possible...
Number of ways...
```

Examples:

* Climbing Stairs
* Unique Paths
* Coin Change II
* Decode Ways

---

### 2. Minimum

```text
Minimum cost
Minimum operations
Minimum number of coins
Minimum path
```

---

### 3. Maximum

```text
Maximum profit
Maximum sum
Maximum value
Maximum length
```

---

### 4. Boolean / Possibility

```text
Can we...
Is it possible...
Can we reach...
Can we form...
```

Usually:

```cpp
dp[i] = true / false;
```

---

### 5. Choice-Based Problems

Look for:

```text
Take / Don't Take
Pick / Skip
Include / Exclude
Buy / Sell
Move Left / Move Right
Move Down / Move Right
```

These are strong DP signals.

---

# 5. DP Problem-Solving Framework

For every DP problem, follow:

```text
1. Identify the state
        ↓
2. Identify the choices
        ↓
3. Find the transition
        ↓
4. Define the base case
        ↓
5. Choose Memoization / Tabulation
        ↓
6. Optimize space if possible
```

---

# 6. Step 1 — Define the State

Ask:

> What information uniquely identifies my subproblem?

Examples:

### 1D

```cpp
dp[i]
```

Answer for state/index `i`.

### 2D

```cpp
dp[i][j]
```

Answer for state `(i, j)`.

### 3D

```cpp
dp[i][j][k]
```

Used when three pieces of information are required.

---

# 7. Step 2 — Identify Choices

Ask:

> What choices can I make at the current state?

Example:

```text
              Element
              /     \
           Take     Skip
```

Then:

```cpp
dp[i] = max(take, skip);
```

---

# 8. Step 3 — Find the Transition

Convert the choices into a recurrence.

Example:

```cpp
dp[i] = max(
    dp[i - 1],
    nums[i] + dp[i - 2]
);
```

---

# 9. Step 4 — Define Base Cases

Find the smallest possible states.

Example:

```cpp
dp[0] = 0;
dp[1] = 1;
```

Base cases prevent invalid recursive calls and initialize tabulation.

---

# 10. Two Ways to Implement DP

## A. Memoization — Top Down

Start from the original problem and recursively solve smaller states.

```cpp
int solve(int n) {
    if(n <= 1)
        return n;

    if(dp[n] != -1)
        return dp[n];

    return dp[n] = solve(n - 1) + solve(n - 2);
}
```

### Characteristics

* Uses recursion
* Top-down
* Stores calculated states
* Easy to derive from recursive solutions

---

## B. Tabulation — Bottom Up

Start with base cases and build the answer iteratively.

```cpp
dp[0] = 0;
dp[1] = 1;

for(int i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
}
```

### Characteristics

* Usually uses loops
* Bottom-up
* No recursion stack
* Often easier to optimize

---

# 11. Memoization vs Tabulation

| Feature        | Memoization            | Tabulation                    |
| -------------- | ---------------------- | ----------------------------- |
| Approach       | Top Down               | Bottom Up                     |
| Implementation | Recursion              | Iteration                     |
| Stack          | Uses recursion stack   | No recursion stack            |
| States         | May skip unused states | Usually calculates all states |
| Derivation     | Usually easier         | Often more efficient          |

A useful strategy:

```text
Recursive solution
      ↓
Memoization
      ↓
Tabulation
      ↓
Space Optimization
```

---

# 12. Major DP Patterns

## Pattern 1 — 1D DP

State:

```cpp
dp[i]
```

Used when the answer depends mainly on previous positions.

Examples:

* Fibonacci
* Climbing Stairs
* House Robber
* Min Cost Climbing Stairs
* Decode Ways

Typical form:

```cpp
dp[i] = f(dp[i - 1], dp[i - 2], ...);
```

### Recognition

```text
Previous positions
Adjacent elements
Steps
Index-based decisions
```

---

# Pattern 2 — Take / Skip

At every element:

```text
             Element
             /     \
          Take     Skip
```

Examples:

* House Robber
* Subsequence problems
* 0/1 Knapsack
* Subset Sum

Typical:

```cpp
dp[i] = max(take, skip);
```

or:

```cpp
dp[i][sum] =
    dp[i - 1][sum] ||
    dp[i - 1][sum - arr[i]];
```

### Recognition

```text
Choose some elements
Pick or don't pick
Include or exclude
```

---

# Pattern 3 — 0/1 Knapsack

Each item can be used **at most once**.

State:

```cpp
dp[i][capacity]
```

Transition:

```cpp
dp[i][w] = max(
    dp[i - 1][w],
    value[i] + dp[i - 1][w - weight[i]]
);
```

Examples:

* 0/1 Knapsack
* Subset Sum
* Partition Equal Subset Sum

---

# Pattern 4 — Unbounded Knapsack

Each item can be used **multiple times**.

Examples:

* Coin Change
* Coin Change II
* Rod Cutting
* Unbounded Knapsack

Main difference:

```text
0/1 Knapsack
→ Item used once

Unbounded Knapsack
→ Item can be reused
```

---

# Pattern 5 — Subset / Partition DP

Typical question:

```text
Can some elements produce target?
```

State:

```cpp
dp[i][sum]
```

Typical transition:

```cpp
dp[i][sum] =
    dp[i - 1][sum] ||
    dp[i - 1][sum - arr[i]];
```

Examples:

* Subset Sum
* Partition Equal Subset Sum
* Target Sum
* Count Subsets
* Minimum Subset Difference

### Recognition

```text
Subset
Partition
Target Sum
Divide array into groups
```

---

# Pattern 6 — Grid DP

Used for matrix/grid problems.

State:

```cpp
dp[i][j]
```

Example:

```text
→
↓
```

Typical transition:

```cpp
dp[i][j] =
    dp[i - 1][j] +
    dp[i][j - 1];
```

Examples:

* Unique Paths
* Minimum Path Sum
* Grid Paths
* Dungeon Game

### Recognition

```text
Matrix
Grid
Move right/down
Coordinates
```

---

# Pattern 7 — String DP

Especially common when two strings are involved.

State:

```cpp
dp[i][j]
```

Usually represents the answer for:

```text
first i characters of string A
first j characters of string B
```

Examples:

* Longest Common Subsequence
* Edit Distance
* Distinct Subsequences
* Interleaving String

---

# Pattern 8 — LCS

Longest Common Subsequence.

If characters match:

```cpp
dp[i][j] = 1 + dp[i - 1][j - 1];
```

Otherwise:

```cpp
dp[i][j] = max(
    dp[i - 1][j],
    dp[i][j - 1]
);
```

Recognition:

```text
Two strings
Matching characters
Common subsequence
```

---

# Pattern 9 — LIS

Longest Increasing Subsequence.

State:

```cpp
dp[i]
```

means:

> Longest increasing subsequence ending at index `i`.

Transition:

```cpp
if(nums[j] < nums[i])
    dp[i] = max(dp[i], dp[j] + 1);
```

Examples:

* LIS
* Maximum Sum Increasing Subsequence
* Russian Doll Envelopes

Recognition:

```text
Subsequence
Increasing
Decreasing
Ordering
```

---

# Pattern 10 — Interval DP

State:

```cpp
dp[l][r]
```

represents the answer for interval `[l, r]`.

Usually split at `k`:

```text
[l ........ r]
      k
     / \
[l..k] [k+1..r]
```

Typical structure:

```cpp
for(int len = 2; len <= n; len++) {

    for(int l = 0; l + len - 1 < n; l++) {

        int r = l + len - 1;

        for(int k = l; k < r; k++) {

            // Split interval
        }
    }
}
```

Examples:

* Matrix Chain Multiplication
* Burst Balloons
* Palindrome Partitioning
* Optimal BST

Recognition:

```text
Range
Interval
Split at k
Merge two parts
```

---

# Pattern 11 — Tree DP

State is usually based on a tree node.

```cpp
dp[node][state]
```

Example states:

```text
Take node
Don't take node
```

Example:

**House Robber III**

```text
dp[node][0] = don't take node
dp[node][1] = take node
```

Tree DP combines answers from children.

---

# Pattern 12 — Graph DP

Useful especially on a DAG.

Examples:

* Longest Path in DAG
* Number of Paths
* Minimum/Maximum path problems

Typical state:

```cpp
dp[node]
```

For DAG problems, topological ordering is often useful.

---

# Pattern 13 — State Machine DP

Used when a problem has different states and transitions.

Example:

```text
Buy
Sell
Hold
Cooldown
```

Typical state:

```cpp
dp[i][state]
```

Examples:

* Best Time to Buy and Sell Stock
* Stock with Cooldown
* Stock with Transaction Fee

Recognition:

```text
Actions
States
Transitions
```

---

# Pattern 14 — Bitmask DP

Used when the state represents a subset.

```cpp
dp[mask]
```

or:

```cpp
dp[mask][i]
```

A bit in `mask` represents whether an element has been selected/visited.

Examples:

* Travelling Salesman Problem
* Assignment Problems
* Visit Every City

Recognition:

```text
Small n
Subset of elements
Visit/select each element
```

---

# Pattern 15 — Digit DP

Used for counting numbers satisfying digit-based constraints.

Typical state:

```cpp
dp[pos][tight][started][...]
```

Examples:

* Count numbers ≤ N
* Numbers with restricted digits
* Numbers satisfying digit properties

This is an advanced DP pattern.

---

# 13. DP Pattern Cheat Sheet

| Clue in Problem     | Pattern              |
| ------------------- | -------------------- |
| Previous positions  | 1D DP                |
| Take / Skip         | Knapsack / 1D DP     |
| Target Sum          | Subset DP            |
| Partition           | Knapsack / Subset DP |
| Grid                | Grid DP              |
| Two Strings         | 2D String DP         |
| Longest Subsequence | LIS / LCS            |
| Range / Interval    | Interval DP          |
| Tree                | Tree DP              |
| DAG                 | Graph DP             |
| Buy / Sell / Hold   | State Machine DP     |
| Small `n` + subsets | Bitmask DP           |
| Digits + `N`        | Digit DP             |

---

# 14. DP Complexity

If:

```text
Number of states = S
Transitions per state = T
```

Then generally:

```text
Time  = O(S × T)
Space = O(S)
```

Examples:

### 1D

```cpp
dp[n]
```

Usually:

```text
Time  = O(n)
Space = O(n)
```

### 2D

```cpp
dp[n][m]
```

Usually:

```text
Time  = O(n × m)
Space = O(n × m)
```

---

# 15. Space Optimization

If the current state only depends on a few previous states, the full DP table may not be required.

Example:

```cpp
int prev2 = 0;
int prev1 = 1;

for(int i = 2; i <= n; i++) {
    int cur = prev1 + prev2;

    prev2 = prev1;
    prev1 = cur;
}
```

Instead of:

```cpp
dp[n]
```

we use:

```text
prev2
prev1
```

Complexity:

```text
Time  = O(n)
Space = O(1)
```

---

# 16. The Golden DP Formula

Whenever you are stuck, ask:

```text
What is my STATE?
       ↓
What are my CHOICES?
       ↓
What smaller STATES do those choices create?
       ↓
How do I combine their answers?
       ↓
What is the BASE CASE?
```

The most important DP skills are:

```text
State
Choice
Transition
Base Case
```

---

# 17. DP Learning Roadmap

Recommended order:

```text
Fibonacci
   ↓
Climbing Stairs
   ↓
1D DP
   ↓
Take / Skip
   ↓
House Robber
   ↓
0/1 Knapsack
   ↓
Subset Sum
   ↓
Partition DP
   ↓
Unbounded Knapsack
   ↓
Grid DP
   ↓
String DP
   ↓
LCS
   ↓
LIS
   ↓
State Machine DP
   ↓
Interval DP
   ↓
Tree DP
   ↓
DAG DP
   ↓
Bitmask DP
   ↓
Digit DP
```

---

# 18. Quick DP Recognition Formula

When solving a new problem:

```text
                 DP?
                  │
        ┌─────────┴─────────┐
        │                   │
 Repeated subproblem?    No repetition
        │                   │
       Yes                 Other technique
        │
 Optimal substructure?
        │
       Yes
        │
       DP
```

Then identify:

```text
State
  ↓
Choice
  ↓
Transition
  ↓
Base Case
  ↓
Memoization
  ↓
Tabulation
  ↓
Space Optimization
```

---

# 19. Most Important DP Patterns to Master

Focus heavily on these first:

```text
1. 1D DP
2. Take / Skip
3. 0/1 Knapsack
4. Subset Sum
5. Unbounded Knapsack
6. Grid DP
7. String DP
8. LCS
9. LIS
10. State Machine DP
11. Interval DP
12. Tree DP
```

Advanced:

```text
13. DAG DP
14. Bitmask DP
15. Digit DP
```

---

# 20. Final DP Mindset

Do not memorize:

```text
dp[i] = ...
```

Instead learn to derive it.

Always think:

```text
              PROBLEM
                 ↓
            What changes?
                 ↓
              STATE
                 ↓
          What can I choose?
                 ↓
             CHOICES
                 ↓
        What happens next?
                 ↓
           TRANSITION
                 ↓
          Smallest case?
                 ↓
           BASE CASE
                 ↓
       Memoization / Tabulation
                 ↓
        Space Optimization
```

> **DP = State + Choices + Transition + Base Case + Reuse of Answers**

Once you become comfortable identifying the **state**, most DP problems become much easier.
