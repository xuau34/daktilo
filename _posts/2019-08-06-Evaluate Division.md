---
layout: post
comment: true
title:  "Evaluate Division"
subtitle: "KS Summer Academy"
date:   2019-08-06 18:30:01
categories: [programming]
---		
		

# Evalutae Division

## Description

> [link](https://leetcode.com/problems/evaluate-division/)

Given `N` equations, formatted as `str1 / str2 = real_number`, queries `str3 / str4`.

If the answer is undefined, count as `-1.0`.

Return all answers in a vector array.


## Thoughts

* Let's called `str1` is related to `str2` in the equation.  
  If there's `str3` realted to `str2`, then I can get the value of `str3 / str1` or `str1 / str3`, which is called **transitive**.
  
* The first thing in every query, I need know whether the answer exists or not.		
  * **Union Find Set**
  * Not using standard set because I'm not sure how to merge two sets
  
* How can I transform string into hash value?
  * Manually, or **map + incremental id**
  
Therefore, I can use set to maintain the relation, plus the value that `this element / head`.

## Further settings

* In the union set, I need to maintain the value that `this element / head` while updating `parent`.
* The `this element / head` in `head` node will always be `1.0`.
* When merging, I need to update this value of merged head.

The rest is easy to calculate by hand~

## Code 

Assuming `N` equations, `Q` queries.

**Union Find Set**'s time complexity is averagely `O(1)`.

Time complexity: `O(N*1+Q*1)` = `O(N+Q)`

For every node, there's 3 * int space consumed.

Space complexity: `O(N*3)` = `O(N)`
	
```c++
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        /* Find set first, if not, the first will be the set head;
           represent as child / head */
        vector<disjointSetNode> nodes;
        map<string, int> hashTable;
        for( int i = 0; i < equations.size(); ++i){
            const auto& equation = equations[i];
            int hashValA = insertNodes(nodes, hashTable, equation[0]);
            int hashValB = insertNodes(nodes, hashTable, equation[1]);
            uniteSet( nodes, hashValA, hashValB, values[i]);
        }
        vector<double> answers;
        for(const auto& query: queries){
            int hashValA = findHashTable(hashTable, query[0]);
            int hashValB = findHashTable(hashTable, query[1]);
            if(hashValA >= 0 && hashValB >= 0){
                answers.push_back( getBdivideA(nodes, hashValA, hashValB) );
            }else{
                answers.push_back( -1 );
            }
        }
        return answers;
    }
private:
    class disjointSetNode{
    public:
        int parent;
        double dividedVal;
        disjointSetNode(int _parent, int _dividedVal): parent(_parent), dividedVal(_dividedVal){
        }   
    };
    int insertNodes(vector<disjointSetNode>& nodes, map<string, int>& hashTable, string X ){
        int hashVal = insertHashTable(hashTable, X, nodes.size());
        if(hashVal == nodes.size()){
            nodes.push_back( disjointSetNode(hashVal, 1) );
        }
        return hashVal;
    }
    int insertHashTable(map<string, int>& hashTable, string X, int nextHashVal){
        const auto ite = hashTable.find(X);
        if( ite == hashTable.end() ){
            hashTable[X] = nextHashVal;
            return nextHashVal;
        }
        return ite -> second;
    }
    int findHashTable(const map<string, int>& hashTable, string X){
        const auto ite = hashTable.find(X);
        if( ite == hashTable.end() ) return -1;
        return ite -> second;
    }
    pair<int, double> findParent( vector<disjointSetNode>& nodes, int idx ){
        if( nodes[idx].parent == idx ){
            nodes[idx].dividedVal = 1;
            return make_pair(idx, 1);
        }
        pair<int, double> parAndVal = findParent( nodes, nodes[idx].parent);
        nodes[idx].dividedVal *= parAndVal.second;
        nodes[idx].parent = parAndVal.first;
        return make_pair(nodes[idx].parent, nodes[idx].dividedVal);
    }
    void uniteSet( vector<disjointSetNode>& nodes, int A, int B, double BdivideA){
        pair<int, double> parAndValA = findParent(nodes, A);
        pair<int, double> parAndValB = findParent(nodes, B);
        if(parAndValA.first == parAndValB.first) return;
        nodes[parAndValB.first].parent = parAndValA.first;
        nodes[parAndValB.first].dividedVal = (parAndValA.second / parAndValB.second) / BdivideA;
    }
    double getBdivideA( vector<disjointSetNode>& nodes, int A, int B){
        pair<int, double> parAndValA = findParent(nodes, A);
        pair<int, double> parAndValB = findParent(nodes, B);
        if(parAndValA.first != parAndValB.first) return -1;
        return parAndValA.second / parAndValB.second;
    }
    
};
```

