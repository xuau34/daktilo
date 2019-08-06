---
layout: post
comment: true
title:  "Circuit Board"
subtitle: "KS Summer Academy"
date:   2019-08-06 20:30:01
categories: [programming]
---		
		

# Circuit Board

## Description

> [link](https://codingcompetitions.withgoogle.com/kickstart/round/0000000000050ff2/0000000000150aae)

Given `R * C` board. Each square has a property, called `thickness`. Find the max area of retangle that the max difference of `thickness` in each row is no greater than `K`.

* 15 secs
* 1GB
* 1 ≤ `T` ≤ 50.
* 1 ≤ `R` ≤ 300.
* 1 ≤ `C` ≤ 300.
* 0 ≤ `Vi, j` ≤ 10^3 for all `i`, `j`.
* 0 ≤ `K` ≤ 103.


## Thoughts

* Because the limitation is on `row`, the greedy thought is that if this range `[l,r]` is valid in previous `row`, I should always take them.
* In each column, if `[l,r]` is valid, gain the previous one. If not, the `count` will be zero.
* In each row, I will need to know the `Min`, `Max`, and `count` to expand the dp values.
* Assume that I always expand the the range by the tail.

```
  definition- 
  dp[l][r] = {Min, Max, count}

  transfer-
  dp[l][r] = { min(dp[l][r-1].Min, dp[r][r].Min), max(dp[l][r-1].Max, dp[r][r].Max), if(this.Max - this.Min <= K) count += (r-l+1) else count = 0};
```

### Other thoughts

I tried to use deque to get the answer of each row. But it will be harder to make the transfer works.


## Code 

Time complexity: `O(T*R*C^2)` = `O(1,350,000,000)`

Space complexity: `O(3*C^2)` = `O(90000)`
	
```c++
/*
  Using dp in every row - 
  (To transfer the state to the next row, we need the range and the current max square number)
  dp[l][r] = {Min, Max, count}
  //And assuming we all expand the tail
  dp[l][r] = { min(dp[l][r-1].Min, dp[r][r].Min), max(dp[l][r-1].Max, dp[r][r].Max), if(this.Max - this.Min <= K) count += (r-l+1) else count = 0};
  //For convenient accessing to array, using closed range [], so the lenth of a range will be (r-l+1)
 
 */

#include "bits/stdc++.h"
using namespace std;
class data{
public:
  int Min, Max, count;
  data(): Min(0), Max(0), count(0){
  }
  data( int x, int _count ): Min(x), Max(x), count(_count){
  }
};
int main(void){
  int caseNumber;
  scanf("%d", &caseNumber);
  for(int Case = 1; Case <= caseNumber; ++Case){
    int R, C, K;
    scanf("%d%d%d", &R, &C, &K);
    vector< vector<int> > board(R, vector<int>(C) );
    for(int r = 0; r < R; ++r){
      for(int c = 0; c < C; ++c){
        scanf("%d", &(board[r][c]));
      }
    }
    int maxNum = R;
    vector< vector<data> > dp(C, vector<data>(C) );
    for(int row = 0; row < R; ++row){
      for(int i = 0; i < C; ++i) dp[i][i] = data(board[row][i], row+1);
      for(int len = 1; len < C; ++len){
        for(int l = 0; l + len < C; ++l){
          int r = l + len;
          dp[l][r].Min = min(dp[l][r-1].Min, dp[r][r].Min);
          dp[l][r].Max = max(dp[l][r-1].Max, dp[r][r].Max);
          if(dp[l][r].Max - dp[l][r].Min <= K){
            dp[l][r].count += (len + 1);
            if(dp[l][r].count > maxNum) maxNum = dp[l][r].count;
          }else{
            dp[l][r].count = 0;
          }
        }
      }
    }
    printf("Case #%d: %d\n", Case, maxNum);
  }
}
```

