---
layout: post
title: coordinate compression, rank transform of a matrix
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [cp]
comments: true
---

### Problem

Given an m x n matrix, return a new matrix answer where answer[row][col] is the rank of matrix[row][col].

The rank is an integer that represents how large an element is compared to other elements. It is calculated using the following rules:

The rank is an integer starting from 1. If two elements p and q are in the same row or column, then: If p < q then rank(p) < rank(q) If p == q then rank(p) == rank(q) If p > q then rank(p) > rank(q) The rank should be as small as possible. It is guaranteed that the answer is unique under the given rules.

**Example 1:**

Input: matrix = [[1,2],[3,4]]

Output: [[1,2],[2,3]]

Explanation:

The rank of matrix[0][0] is 1 because it is the smallest integer in its row and column.

The rank of matrix[0][1] is 2 because matrix[0][1] > matrix[0][0] and matrix[0][0] is rank 1.

The rank of matrix[1][0] is 2 because matrix[1][0] > matrix[0][0] and matrix[0][0] is rank 1.

The rank of matrix[1][1] is 3 because matrix[1][1] > matrix[0][1], matrix[1][1] > matrix[1][0], and both matrix[0][1] and matrix[1][0] are rank 2.

**Example 2:**

Input: matrix = [[7,7],[7,7]]


Output: [[1,1],[1,1]]

**Example 3:**

Input: matrix = [[20,-21,14],[-19,4,19],[22,-47,24],[-19,4,19]]

Output: [[4,2,3],[1,3,4],[5,1,6],[1,3,4]]

**Example 4:**

Input: matrix = [[7,3,6],[1,4,5],[9,8,2]]

Output: [[5,1,4],[1,2,3],[6,3,1]]

Constraints:

m == matrix.length

n == matrix[i].length

1 <= m, n <= 500

-109 <= matrix[row][col] <= 109


Answer/Final code:

```c
coordinate compression / index compression
class Solution {
    vector<int> compress(vector<int> a) {
        vector<int> s = a;
        sort(s.begin(), s.end());
        s.resize( unique(s.begin(), s.end()) - s.begin() );
        for(int i = 0; i < (int) a.size(); i++) {
            a[i] = lower_bound(s.begin(), s.end(), a[i]) - a.begin();
        }
        return a;
    }
public:
    vector<vector<int>> matrixRankTransform(vector<vector<int>>& matrix) {
        
    }
};
sort all the values
give the new id to arr[i][j] max id form row i or col j
 
. 2 .
1 ? .
. 1 .
1 ? 3
we need to consider both 4s
 
for every tuple (4, row, col) {
    remember what it transforms to
}
 
 
X = max(in_row, in_col) + 1
    
for(i) {
    for(j) {
        tuples.push(arr[i][j], i, j);
    }
}
sort(tuples) // O(N*M*log(N*M))
    
    
O(N*M*log(...))
    
    
    
How to find a group of cells that should be all equal?
    
. . . . 
7 . . . 
. . . . 
. . 7 7 
7 . 7 . 
. . . .
    
for every row, store all occurrences of every value
 
map<int, vector<int>> where_in_row[HEIGHT], where_in_col[WIDTH];
where_in_row[0] is a map describing row 0
where_in_row[0][7] is a list of occurrences of value 7 in row 0
 
void dfs(int row, int col) { O(H*W*(H+W))
    cc.push_back({row, col});
    visited[row][col] = true;
    int x = matrix[row][col];
    vector<int> tmp = where_in_row[row][x];
    where_in_row[row][x].clear();
    for(int c2 : tmp) {
        if not visited[row][c2]:
            dfs(row, c2);
    }
    
    tmp = ...col...;
    ...col....clear()
    for(int r2 : tmp) {
        if not visited[r2][col]:
            dfs(r2, col);
    }
}
    
int maxr[HEIGHT], maxc[WIDTH];
for tuple (x, row, col) in sorted_tuples:
    if not visited[row][col]:
        // find CC with this cell
        dfs(row, col)
int maxr[HEIGHT], maxc[WIDTH];
for tuple (x, row, col) in sorted_tuples:
    if not visited[row][col]:
        // find CC with this cell
        dfs(row, col)
        
        int new_value = 0;
        for every cell in cc:
            new_value = max(new_value, maxr[cell.first], maxc[cell.second])
        for every cell in cc:
            new_matrix[this cell] = new_value + 1;
            maxr[cell.first] = max(..., new_value + 1)
            maxc[...] = ...
            
    
    
 
. . . 0
. . . 2
. . 3 3
. . . 1
2 2 3
```
