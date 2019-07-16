---
layout: post
comment: true
title:  "Maximal Rectangle"
subtitle: "I passed it~ <3"
date:   2019-07-16 15:49:01
categories: [programming]
---		
		

# Maximal Rectangle

## Description

> [link](https://leetcode.com/problems/maximal-rectangle/)

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```

## Thoughts

* If I want to know the current grid's largest retangle, I'll need
	* the largest one in the same row
	* the largest one in the same col
	* the largest one for (row-1,col-1)
* If I can maintain all these, I can know there're 4 possible rectangles from these info - 
	* from the same row
	* from the same col
	* from the previous one: to make it valid, it need to be tested by the largest one in the same row and col.
	* start from current grid
	

## Code 

* `dp1`: the one in same row
* `dp2`: the one in same col
* `dp` : the one in previous grid (row - 1, col - 1)
	* Because it'll need the previous one (col - 1), which will be covered up by update, I use another array `dp0` to keep track the updated one.

--

Assume the grid's height is `N` and width is `M`.

Time complexity: `O(NM)`

Space complexity: `O(M)`
	
```c++
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        if(matrix.size() == 0) return 0;
        vector<int> dp1(matrix[0].size()+1, INT_MAX), dp2(matrix[0].size()+1, INT_MAX);
        vector<pair<int,int> > dp(matrix[0].size()+1, make_pair(INT_MAX, INT_MAX) );
        
        //for the first row
        int ans = 0;
        for(int j = 1; j <= matrix[0].size(); ++j){
            if(matrix[0][j-1] - '0'){
                dp1[j] = min(dp1[j - 1], j);
                dp2[j] = 1;
                dp[j] = min(dp[j-1], make_pair(1, j) );
                ans = max(ans, j - dp1[j] + 1);
            }
        }
        
        //the rest rows
        for(int i = 2; i <= matrix.size(); ++i){
            vector<pair<int,int> > dp0(matrix[0].size()+1, make_pair(INT_MAX, INT_MAX) );
            for(int j = 1; j <= matrix[0].size(); ++j){
                if(matrix[i-1][j-1] - '0'){ //if this grid is '1'
                    dp1[j] = min(dp1[j - 1], j);
                    dp2[j] = min(dp2[j], i);
                    
                    //update dp0
                    int x = max(dp2[j], dp[j-1].first), y = max(dp1[j], dp[j-1].second);
                    int temp;
                    if(x != INT_MAX && y != INT_MAX){ //I can use the previous one (row-1,col-1)
                        temp = (i - x + 1) * (j - y + 1);
                        dp0[j] = make_pair(x, y);
                    }else {
                        temp = 1;
                        dp0[j] = make_pair(i, j);
                    }
                    //If using the same row's will be larger
                    if(dp1[j] != INT_MAX && (j - dp1[j] + 1) > temp) {
                        temp = j - dp1[j] + 1;
                        dp0[j] = make_pair(i, dp1[j]);
                    }
                    //If using the same col's will be larger
                    if(dp2[j] != INT_MAX && (i - dp2[j] + 1) > temp) dp0[j] = make_pair(dp2[j], j);
                    
                    //update ans
                    ans = max(ans, (i - dp0[j].first + 1) * (j - dp0[j].second + 1));
                }else{ //if this grid is '0'
                    dp1[j] = dp2[j] = INT_MAX;
                    dp0[j] = make_pair(INT_MAX, INT_MAX);
                }
            }
            dp.swap(dp0);
        }
        return ans;
    }
};
```

