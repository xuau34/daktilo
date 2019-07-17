---
layout: post
comment: true
title:  "Energy Stones"
subtitle: "Failed :("
date:   2019-07-16 15:49:01
categories: [programming]
---		
		

# Energy Stones

## Description

> [link](https://codingcompetitions.withgoogle.com/kickstart/round/0000000000050eda/00000000001198c3)

There're `N` stones, with `E` energy. Duda can obtain all `E` at the moment he eats it. Yet, every stone might lose `L` enery every seconds, and costs him `S` seconds to eat it all.

> Yup, he must eat it all no matter if he has gained the energy.

* 1 ≤ `T` ≤ 100.
* 1 ≤ `N` ≤ 100.
* 1 ≤ `Si` ≤ 100.
* 1 ≤ `Ei` ≤ 10^5.
* 0 ≤ `Li` ≤ 10^5.


## Failed Thoughts

* I didn't think of a greedy order, so I kept thinking how I can remember what stones I have eaten.
	
## Analysis Overlook

* Assume there're only two stones: `i`th and `j`th.  
	When Duda eats `i`th stone, he must lose the energe `Si*Lj` from `j`th.  
	Therefore, we can choose those that won't lose much energe to eat first.  
	* Namely, `Si*Lj` < `Sj*Li` for all `j`.
	
* How to sort it?  
	* According to this formula, it can be `Si/Li` < `Sj/Lj`.
	
*  What happend if `Li` is `0`
	* From the first formula, we can know that this `i` should be eaten at the end.

* Conclude - we can obtain the max value by 0/1 Knapsack  
  `i` & `i+1` are in the order mentioned above.  

```c++
	
dp[T][i] = max( dp[T + S[i] ][i + 1] + max(0, E[i]-L[i]*T),
				   dp[T][i])
```

## Code 

Time complexity: `O(N*N*S)` = `O(10^6)`

Space complexity: `O(N*S)` = `O(10^4)`
	
```c++
#include "bits/stdc++.h"
using namespace std;
int S[105], E[105], L[105], N;
int dp[10100][105];

int solve( pair<double, int> indexes[], int T, int i){
  if(T > 10095 || i >= N) return 0;
  int idx = indexes[i].second;
  if(dp[T][i] >= 0) return dp[T][i];
  return dp[T][i] = max( solve(indexes, T + S[idx], i+1) + max(0,E[idx] - T * L[idx]),
                         solve(indexes, T, i+1) );
}

int main(void){
  int T;
  scanf("%d", &T);
  for(int Case = 1; Case <= T; ++Case){
    scanf("%d", &N);
    pair<double, int> indexes[N]; 
    for(int i = 0; i < N; ++i){
      scanf("%d%d%d", S + i, E + i, L + i);
      if(L[i] > 0) indexes[i].first = ((double) S[i])/L[i];
      else indexes[i].first = 500;
      indexes[i].second = i;
    }
    sort(indexes, indexes + N);
    memset(dp, -1, sizeof(dp));
    int ans = 0;
    for(int i = 0; i < N; ++i){
      ans = max(ans, solve(indexes, 0, i));
    }
    printf("Case #%d: %d\n", Case, ans);
  }
}

```

