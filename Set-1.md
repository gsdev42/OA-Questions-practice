# Potato Sack Bundling Problem

## Problem Statement

Golu is a rich potato farmer. During harvesting season, the workers at his farm packed his potatoes into sacks of different weights. When Golu reached the market to sell his produce, he realised that the Market Association had standardised the buy-sell transactions. Now buyers in the market could buy potatoes only in one standardised weight, and they could carry a specified number of bags only.

Given the standardised weight and number of bags, help Golu to determine how many different ways he can bunch his sacks together to make a sale.

Read the input from STDIN and print the output to STDOUT. Do not write arbitrary strings anywhere in the program, as these contribute to the standard output and testcases will fail.

## Input Format

First line has an integer, W, which is the standardised weight which a buyer can buy. Second line has two integers, N and X, where N is the number of bags a buyer can carry, and X is the number of sacks Golu has. Third line has X integers, the weights of Golu's sacks, separated by single white space.

## Output Format

Single line has an integer, S, which is the number of ways in which the standard weight W can be formed as the sum of Golu's sacks' weights.

## Constraints

1) Weights of X sacks are unique and entered in the ascending order of their weights.
2) 0 < W <= 500
3) 0 < N <= 50
4) 0 < X <= 100

## Sample Input 1

```
20
3 10
1 2 4 5 10 11 13 15 17 19
```

## Sample Output 1

```
4
```

## Explanation 1

Standardised weight in which sale can happen, W = 20  
Number of bags buyers can carry N = 3  
Number of sacks with Golu, x = 10  
Weights of 10 sacks: 1 2 4 5 10 11 13 15 17 19.

Different ways in which the sacks can be bunched to match the standardized weight are:
- 1 2 17
- 1 4 15
- 2 5 13
- 4 5 11

So the number of ways S = 4

## Sample Input 2

```
45
7 19
1 2 4 5 6 8 9 10 12 13 15 16 17 18 19 20 21 23 24
```

## Sample Output 2

```
12
```

## Explanation 2

Standardised weight in which sale can happen, W = 45  
Number of bags buyers can carry N = 7  
Number of sacks with Golu, x = 19

Different ways in which the sacks can be bunched to match the standardised weight are:
- 1 2 4 5 6 8 19
- 1 2 4 5 6 9 18
- 1 2 4 5 6 10 17
- 1 2 4 5 6 12 15
- 1 2 4 5 8 9 16
- 1 2 4 5 8 10 15
- 1 2 4 5 8 12 13
- 1 2 4 6 8 9 15
- 1 2 4 6 9 10 13
- 1 2 5 6 8 10 13
- 1 2 5 6 9 10 12
- 1 4 5 6 8 9 12

Hence output is 12.

## Starter Code

```cpp
#include <iostream>
using namespace std;

int bagsSum(int target, int nbags, int n, int V[]) {
    // target contains the standardized weight which a buyer can buy.
    // nbags contains the number of bags a buyer can carry.
    // n contains the number of sacks Golu has.
    // V contains the array of n weights of Golu's sacks.
    
    int sum = -1;
    
    // WRITE YOUR CODE HERE
    // Store the output in "sum"
    
    return sum;
}

int main() {
    int target, nbags, n;
    cin >> target >> nbags >> n;
    
    int V[n];
    for (int i = 0; i < n; i++) {
        cin >> V[i];
    }
    
    cout << bagsSum(target, nbags, n, V);
    
    return 0;
}
```

---

# Solution

## Approach

This is a classic recursion problem where we need to find the number of ways to select exactly N sacks such that their total weight equals the target weight W.

For each sack, we have two choices:
1. Include the current sack in our selection
2. Don't include the current sack in our selection

We use recursion to explore all possible combinations and count the valid ones.

## My Solution- brute force

```cpp
#include <iostream>
#include <bits/stdc++.h>  
using namespace std;

int f(int idx, int target,int bags,int *V,int n){
    if(bags==0 && target==0){
        return 1;
    }
    if(idx>=n || target<0|| bags==0){
        return 0;
    }
     int nottake=0;
   nottake=f(idx+1,target,bags,V,n);
    
    bool cantake=false;
    int take=0;
    if(V[idx]<=target){
        cantake=true;
        take=f(idx+1,target-V[idx],bags-1,V,n);
    }
    return take+nottake;
}

int bagsSum(int target, int nbags, int n, int V[]) {
    // target contains the standardized weight which a buyer can buy.
    // nbags contains the number of bags a buyer can carry.
    // n contains the number of sacks Golu has.
    // V contains the array of n weights of Golu's sacks.
    
    int sum = 0;
    sum=f(0,target,nbags,V,n);
    
    return sum;
}

int main() {
    int target, nbags, n;
    cin >> target >> nbags >> n;
    
    int V[n];
    for (int i = 0; i < n; i++) {
        cin >> V[i];
    }
    
    cout << bagsSum(target, nbags, n, V);
    
    return 0;
}
```

## Algorithm Explanation

1. **Base Cases:**
   - If `bags == 0 && target == 0`: We've used exactly N bags and achieved the target weight → return 1
   - If `idx >= n || target < 0 || bags == 0`: Invalid state → return 0

2. **Recursive Cases:**
   - **Don't take current sack:** Call `f(idx+1, target, bags, V, n)`
   - **Take current sack:** If `V[idx] <= target`, call `f(idx+1, target-V[idx], bags-1, V, n)`

3. **Return:** Sum of both possibilities

## Time Complexity
- **Time:** O(2^n) in worst case, where n is the number of sacks
- **Space:** O(n) for recursion stack depth
---

# The Abhaneri Village Stepwell Problem

## Problem Statement

The Abhaneri village stepwell has not been in use for a while. Stepwells are wells or ponds in which water is reached by descending a set of steps to the water level.

The stepwell in Abhaneri went out of use because the steps were very disorganised and people found it difficult to ascend/descend them. To correct this situation, the villagers decide to reconstruct the steps such that the heights of adjacent steps differ exactly by 1 unit.

The steps are in two directions, East-West and North-South. For symmetry, the villagers decide that the final steps in both directions must be identical. In each direction, the steps should be strictly descending, reach the water level, and then be strictly ascending. The lowest step, which is the water level, must have an equal number of steps to the left and to the right of it.

The existing N steps can be modified by reducing or increasing the height. Reducing or increasing the height of a step by 1 unit is counted as one change. Since funds are limited and stepwell construction requires many resources, the villagers want to make as few changes as possible.

Write a program to determine the minimum number of changes needed such that the villagers can rebuild their stepwell in the described way.

## Input/Output Instructions

Read the input from STDIN and print the output to STDOUT. Do not write arbitrary strings while reading the input or while printing, as these contribute to the standard output and testcases will fail.

## Constraints

* N is always an odd number
* 3 ≤ N ≤ 10001
* 0 ≤ step height ≤ 10000000

## Input Format

The first line of input contains N, the number of total steps in one direction (including the water level step).
The second line of input contains the steps in the East-West direction, separated by a single space.
The third line of input contains the steps in the North-South direction, separated by a single space.

## Output Format

The output contains the minimum number of total changes needed in both directions to reconstruct the stepwell.

## Sample Test Cases

### Sample Input 1
```
3
1 2 3
3 2 2
```

### Sample Output 1
```
3
```

**Explanation:** There are 3 steps (including the water level). East-West steps are 1 2 3. North-South steps are 3 2 2. To minimize the changes, the final steps in both directions should be in the form of 3 2 3. To reach this, East-West steps will need 2 changes (first step height increases from 1 to 3), while North-South will need 1 change (last step height increases from 2 to 3). So a total of 3 changes are required.

### Sample Input 2
```
5
2 3 0 1 4
3 3 2 3 1
```

### Sample Output 2
```
10
```

**Explanation:** One possibility is to reconstruct the stepwell with the final steps as 2 1 0 1 2. This will need 12 changes in both directions. Another possibility is 4 3 2 3 4. This will need 10 changes. Another possibility is 3 2 1 2 3. This will also need 10 changes. So, minimum 10 changes are required.

---

## Solution: Median-Based Optimization

### Key Insights

1. **Pattern Recognition**: The target stepwell pattern must be symmetric around the water level at position k = (N-1)/2
2. **Mathematical Formula**: Each position i has target height = h + abs(i - k), where h is the water level height
3. **Optimization Problem**: We need to find the optimal h that minimizes the total cost for both directions
4. **Median Property**: For minimizing sum of absolute deviations, the median is the optimal choice

### Algorithm

The problem can be transformed into finding the optimal h that minimizes:
```
Cost = Σ|a[i] - (h + abs(i-k))| + Σ|b[i] - (h + abs(i-k))|
```

This can be rewritten as:
```
Cost = Σ|(a[i] - abs(i-k)) - h| + Σ|(b[i] - abs(i-k)) - h|
```

The optimal h is the median of all transformed values: `{a[i] - abs(i-k), b[i] - abs(i-k)}` for all i.

### C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

typedef long long llint;

llint findMoves(int n, llint a[], llint b[]) {
    int k = (n - 1) / 2;  // Middle position (water level)
    
    // The target pattern is: h+k, h+k-1, h+k-2, ..., h+1, h, h+1, ..., h+k-2, h+k-1, h+k
    // This can be written as: h + abs(i - k) for each position i
    
    // To find optimal h, we transform the problem:
    // Cost = Σ|a[i] - (h + abs(i-k))| + Σ|b[i] - (h + abs(i-k))|
    // This becomes: Σ|(a[i] - abs(i-k)) - h| + Σ|(b[i] - abs(i-k)) - h|
    
    vector<llint> adjusted_values;
    
    // Collect all adjusted values
    for (int i = 0; i < n; i++) {
        adjusted_values.push_back(a[i] - abs(i - k));
        adjusted_values.push_back(b[i] - abs(i - k));
    }
    
    // Sort to find median
    sort(adjusted_values.begin(), adjusted_values.end());
    
    // Find median (for even number of elements, any middle element works)
    llint optimal_h = adjusted_values[adjusted_values.size() / 2];
    
    // Ensure h is non-negative (water level can't be negative)
    optimal_h = max(0LL, optimal_h);
    
    // Calculate total cost with optimal h
    llint totalCost = 0;
    for (int i = 0; i < n; i++) {
        llint targetHeight = optimal_h + abs(i - k);
        totalCost += abs(a[i] - targetHeight);
        totalCost += abs(b[i] - targetHeight);
    }
    
    return totalCost;
}

int main() {
    int n;
    cin >> n;
    
    llint a[n], b[n];
    for (int i = 0; i < n; i++) cin >> a[i];
    for (int i = 0; i < n; i++) cin >> b[i];
    
    cout << findMoves(n, a, b);
    
    return 0;
}
```

---

## Dry Run Example

Let's trace through Sample Input 2:

**Input:**
- n = 5
- a = {2, 3, 0, 1, 4} (East-West steps)
- b = {3, 3, 2, 3, 1} (North-South steps)

### Step 1: Calculate k (water level position)
```
k = (n - 1) / 2 = (5 - 1) / 2 = 2
```
So the water level is at position 2 (0-indexed).

### Step 2: Understand the target pattern
The target pattern will be: `h+2, h+1, h, h+1, h+2`
- Position 0: h + abs(0-2) = h + 2
- Position 1: h + abs(1-2) = h + 1  
- Position 2: h + abs(2-2) = h + 0 = h (water level)
- Position 3: h + abs(3-2) = h + 1
- Position 4: h + abs(4-2) = h + 2

### Step 3: Calculate adjusted values
For each position i, calculate:
- `a[i] - abs(i - k)`
- `b[i] - abs(i - k)`

**Position 0:** abs(0-2) = 2
- a[0] - 2 = 2 - 2 = 0
- b[0] - 2 = 3 - 2 = 1

**Position 1:** abs(1-2) = 1
- a[1] - 1 = 3 - 1 = 2
- b[1] - 1 = 3 - 1 = 2

**Position 2:** abs(2-2) = 0
- a[2] - 0 = 0 - 0 = 0
- b[2] - 0 = 2 - 0 = 2

**Position 3:** abs(3-2) = 1
- a[3] - 1 = 1 - 1 = 0
- b[3] - 1 = 3 - 1 = 2

**Position 4:** abs(4-2) = 2
- a[4] - 2 = 4 - 2 = 2
- b[4] - 2 = 1 - 2 = -1

### Step 4: Collect all adjusted values
```
adjusted_values = {0, 1, 2, 2, 0, 2, 0, 2, 2, -1}
```

### Step 5: Sort the adjusted values
```
sorted: {-1, 0, 0, 0, 1, 2, 2, 2, 2, 2}
```

### Step 6: Find median
We have 10 values, so median is at index 10/2 = 5
```
optimal_h = sorted[5] = 2
```

### Step 7: Ensure non-negative
```
optimal_h = max(0, 2) = 2
```

### Step 8: Calculate total cost with h = 2
Target heights: h + abs(i - k) = 2 + abs(i - 2)
- Position 0: target = 2 + 2 = 4
- Position 1: target = 2 + 1 = 3
- Position 2: target = 2 + 0 = 2
- Position 3: target = 2 + 1 = 3
- Position 4: target = 2 + 2 = 4

**Final target pattern: [4, 3, 2, 3, 4]**

### Cost calculation:

**For East-West direction (a = {2, 3, 0, 1, 4}):**
- Position 0: |2 - 4| = 2
- Position 1: |3 - 3| = 0
- Position 2: |0 - 2| = 2
- Position 3: |1 - 3| = 2
- Position 4: |4 - 4| = 0
- **East-West cost = 2 + 0 + 2 + 2 + 0 = 6**

**For North-South direction (b = {3, 3, 2, 3, 1}):**
- Position 0: |3 - 4| = 1
- Position 1: |3 - 3| = 0
- Position 2: |2 - 2| = 0
- Position 3: |3 - 3| = 0
- Position 4: |1 - 4| = 3
- **North-South cost = 1 + 0 + 0 + 0 + 3 = 4**

### Final Result
**Total cost = 6 + 4 = 10**

This matches the expected output! The algorithm found that the optimal stepwell pattern is [4, 3, 2, 3, 4] with a water level height of h = 2, requiring exactly 10 changes total.

## Time Complexity
- **Time Complexity:** O(n log n) due to sorting
- **Space Complexity:** O(n) for storing adjusted values

