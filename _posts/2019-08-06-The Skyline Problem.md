---
layout: post
comment: true
title:  "The Skyline Problem"
subtitle: "KS Summer Academy"
date:   2019-08-06 20:56:01
categories: [programming]
---		
		

# Evalutae Division

## Description

> [link](https://leetcode.com/problems/the-skyline-problem/)

Given a list of buildings/retangles based on the `y=0`. Each building is represented by `[L, R, H]`, `L`,`R` is the x-coordinate, and `H` is the height. Return a list of `key points`, the left endpoint of a horizontal line segment. 


## Thoughts

* For horizontal lines, I only need the most left point and with the higher (`y`) position.
* If it's on the right edge of a building, I need to exclude this building to get the valid highest building.
* The y-zero horizontal line counts after the first building. (`x` is greater than the first building)
* So I need a base y-0 line, and also `L` to represent the `x` position from `0` to `LLong_Max`.
	* Using `LLong_Max`	because the rightmost edge may be `INT_MAX`
* Everytime I need to make sure that `L` is valid and correct, and to maintain `priority_queue` containing those horizontal lines, so that I can get the current highest line for possible key point.

## Detailed setting

To traverse buildings from left to right, `x` from `0` to `LLong_Max`.

* Using customized compare function, for each building `L, R, H`
	* `L` -> `H`(greater) -> `R`

To maintain `L` and the `que` -

* Initialization:
	* `L` is the left edge of first building
	* `que` needs to contain the height and the right edge in x-coordinate, and to be pushed the y-0 horizontal line

* There're three cases when traversing `L`:
	1. There's another building that covers some with this current line.
		* If covered in the right edge - 
			1. They're in the same height
			2. Not in the same height
	2. There's no building that covers

* `que`'s top should always be valid. So if the right edge in the top is less or equal to `L`, it should be poped out.

* Analyze them by hand, or jump to the code

## Code 

Assuming `N` buildings.

Time complexity: `O(NlogN)`

Space complexity: `O(N)`
	
```c++
bool cmp(const vector<int>& a, const vector<int>& b){
    if(a[0] == b[0]){
        if(a[2] == b[2]) return a[1] < b[1];
        return a[2] > b[2];
    }
    return a[0] < b[0];
}
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        vector<vector<int>> keyPoints;
        if(buildings.size() == 0) return keyPoints;
        sort(buildings.begin(), buildings.end(), cmp);
        int idx = 0, N = buildings.size(), height = 0;
        long long L = buildings[0][0];
        priority_queue< pair<int,long long> > que;
        que.push( make_pair(0, LLONG_MAX) );
        while( L < LLONG_MAX ){
            clean( que, L );
            if( que.top().first > height ){
                keyPoints.push_back( {L, que.top().first } );
                height = que.top().first;
            }
            if( idx < N && buildings[idx][0] <= que.top().second ){
                L = buildings[idx][0];
                if( buildings[idx][0] == que.top().second && buildings[idx][2] != que.top().first) height = -1;
                que.push( make_pair(buildings[idx][2], buildings[idx][1]) );
                idx += 1;
            }else{
                L = que.top().second;
                height = -1;
                que.pop();
            }
        }
        return keyPoints;
    }
private:
    void clean(priority_queue< pair<int,long long> >& que, int L){
        while(que.top().second <= L) que.pop();
    }
};
```

