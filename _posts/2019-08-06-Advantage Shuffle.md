---
layout: post
comment: true
title:  "Advantage Shuffle"
subtitle: "KS Summer Academy"
date:   2019-08-06 19:45:01
categories: [programming]
---		
		

# Advantage Shuffle

## Description

> [link](https://leetcode.com/problems/advantage-shuffle/)

Given two array with same size. The `advantage of A` is the number of indices `i` for which `A[i] > B[i]`.	
Return any permutation with the max-advantage of A.

* 1 ≤ `A.length` = `B.length` ≤ 10000.
* 0 ≤ `A[i]` ≤ 10^9. 
* 0 ≤ `B[i]` ≤ 10^9.


## Thoughts

To solve this problem, the best way is to put the least greater number in `A` compared to `B[i]`. If there's no number met this need, put the smallest number.

### First thought is
For the least greater number in `A`, I need **sorted** `A` and **binary search**. For the smallest number, I need a **pointer** to keep track the current possible position of min value.

Everytime I put a value, I need to maintain the **sorted** property of `A`. So the best way is to change the value I've put into `-1`, and swap it to the front, which doesn't work for the swaped front may not be in the order.

### Second thought is
Also sort `B` array with its indexes, as `pair of value and index`. So that I can choose value from two points in `A`, and then put the value according the index in the pair.

## Code 

Time complexity: `O(NlogN+N)` = `O(NlogN)`

Space complexity: `O(N)` 
	
```c++
class Solution {
public:
    vector<int> advantageCount(vector<int>& A, vector<int>& B) {
        int N = A.size();
        vector<pair<int,int>> valAndIdxB(N);
        sort(A.begin(), A.end());
        preprocess(B, valAndIdxB);
        vector<int> permutatedA(N);
        int L = 0, biggerIdx = 0;
        while(biggerIdx < N && A[biggerIdx] <= valAndIdxB[0].first) ++ biggerIdx;
        if(biggerIdx == N) return A;
        for(int i = 0; i < N; ++i){
            while(biggerIdx < N && A[biggerIdx] <= valAndIdxB[i].first) ++ biggerIdx;
            if(biggerIdx == N){
                while(L < N && A[L] < 0) ++L;
                permutatedA[valAndIdxB[i].second] = A[L];
                A[L] = -1;
            }else{
                permutatedA[valAndIdxB[i].second] = A[biggerIdx];
                A[biggerIdx] = -1;
            }
        }
        return permutatedA;
    }
private:
    void preprocess(const vector<int>& A, vector<pair<int,int>>& valAndIdxA){
        for(int i = 0; i < A.size(); ++i){
            valAndIdxA[i] = make_pair(A[i], i);
        }
        sort(valAndIdxA.begin(), valAndIdxA.end());
    }
};
```

