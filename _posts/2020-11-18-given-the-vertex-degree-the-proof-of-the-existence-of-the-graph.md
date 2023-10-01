---
layout: post
title: given the vertex degree, the proof of the existence of the graph
subtitle: There's lots to learn!
gh-repo: breylaude/blog
gh-badge: [star, fork, follow]
tags: [cp]
comments: true
---

{: .box-note}
Day 4 individual competition C problem

Description: Give the number of vertices of the graph and the degree of each vertex (at most there is only one edge between each two vertices) to prove the existence of the graph.

Proof: Sort the vertices of the graph by degree, find the vertex with the largest degree (set the degree $n0$) to remove this vertex, and reduce the number of vertices of the next $n0$ vertices by one. Repeat the above steps until the degree of the remaining vertices is 0, then the graph exists, otherwise the graph does not exist.

C code:

```c
#include<stdio.h>

int d[1010];
void qsort(int a[], int s, int e);

int main(){
    int n, i, j, sum;
    scanf("%d", &n);//Enter the number of vertices
    sum = 0;
    for(i = 0; i <n; i++){
      scanf("%d", &d[i]);//Enter the degree of each vertex
      sum += d[i];
    }
    if(sum% 2 == 1){ //If the total degree is odd, it does not exist
      printf("NO\n");
      return0;
    }
    qsort(d, 0, n-1);//Sort first
    for(i = n-1; i >= 0; i--){
      if(d[i]> i){ //If the maximum degree is greater than the number of remaining vertices, it does not exist
        printf("NO\n");
        return0;
      }else{//Otherwise remove the largest vertex
        for(j = 0; j <d[i]; j++){
          d[ij-1]--;
          if(d[ij-1] <0){//If there is a negative degree, it does not exist
            printf("NO\n");
            return0;
          }
        }
        qsort(d, 0, i-1);//sort again
      }
    }
    printf("YES\n");//Meet all conditions, exist
    return0;
}

void qsort(int a[], int s, int e){
  int i = s, j = e, temp = a[s];
  if(s <e){
    while(i != j){
      while(a[j]> temp && i <j) j--;
      if(i <j) a[i++] = a[j];
      while(a[i] <temp && i <j) i++;
      if(i <j) a[j--] = a[i];
    }
    a[i] = temp;
    qsort(a, s, i-1);
    qsort(a, i+1, e);
  }
}
```
