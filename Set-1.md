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
