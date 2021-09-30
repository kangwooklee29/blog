---
layout: post
title: 프로그래머스 > Summer/Winter Coding(2019) > 지형이동
---

Prim's algorithm을 통해 간단하게 해결할 수 있는 MST 문제.

```C++
/*
1. N x N 크기의 격자칸에 격자칸이 갖는 '높이'가 주어지고 height라는 정수값이 주어질 때, 
   한 격자칸에서 다른 격자칸으로 이동할 때 높이차가 height 이하라면 이동하며 발생하는 cost가 
   없으나 height를 초과한다면 그 초과한 값만큼 cost가 발생한다. 이때, 발생하는 cost 합이 
   최소가 되도록 모든 격자점을 방문할 때 그 최솟값을 구하는 문제.
   
   
2. N x N 격자를, 각 격자점을 노드로 각 격자점의 상하좌우 이동경로를 간선으로 하는 
   노드 N x N개인 트리로 볼 수 있다. 이 경우 간선의 cost는 두 격자 간 높이차가 된다.
   문제를 이런 관점에서 바라볼 경우 이 문제는 minimum spanning tree 문제가 된다.
   다만 이 문제의 경우 Kruskal's algorithm보다는 Prim's algorithm이
   더 간단한 구현이 가능해 Prim's algorithm으로 구현해보았다.
   
   
3. Prim's algorithm은 임의의 한 정점을 minimum spanning tree에 우선 포함한 후
   이와 연결돼있으면서 cost가 가장 작은 간선을 minimum spanning tree에 계속 추가하는 식으로
   minimum spanning tree를 구하는 알고리즘이다.
   
   '이와 연결돼있으면서 cost가 가장 작은 간선을 트리에 추가'하는 단계에서 간선을 탐색할 때
   순차적으로 간선을 탐색하는 식으로 코드를 구현하면 이 부분만의 시간복잡도만 해도
   O(n^2)이 될 위험이 있다.
   
   그러나 트리에 간선을 추가할 때마다 그 간선과 연결된 다른 간선들을 priority queue에 넣어두면
   트리에 추가할 간선을 탐색하는 단계를 O(logn)의 시간복잡도 이내에 해결할 수 있다.
   
   이 점에 주목하여 이 코드에서는 priority queue 자료구조를 적극 사용했다.
   
   
4. 사용되는 변수명을 간단하고 직관적일 수 있록 주의했고, 불필요한 if를 줄이기 위해 while문이나 
   C의 (A?B:C) 구문을 적극 사용하여 가독성이 좋은 코드를 쓰기 위해 노력했다.
   
   if는 로직의 큰 흐름을 좌우하는 매우 중요한 구문이기 때문에, 가독성의 관점에서 코드를 볼 때
   if 구문이 등장할 때마다 그 부분을 정확하게 이해하기 위해 많은 에너지를 쓰게 된다.
   따라서 if문의 남발은 코드의 가독성을 떨어트리는 요인이 될 수 있다. 
   더 간결하면서도 더 직관적인 표현으로 구문을 해결할 수 있다면 다른 표현을 사용하는 것이 좋다.
*/

#include <vector>
#include <queue>
#include <cmath>

using namespace std;

struct comp
{
    bool operator() (vector<int> a, vector<int> b)
    {
        return a[0] > b[0];
    }
};

bool is_visit[300][300];
priority_queue<vector<int>, vector<vector<int>>, comp > pq;

int solution(vector<vector<int>> land, int height) {
    int dr[4] = {0, 0, 1, -1};
    int dc[4] = {1, -1, 0, 0}, N = land.size();

    pq.push({0,0,0});
    
    int answer = 0;
    while(pq.empty() == false)
    {
        answer += ((pq.top()[0] <= height) ? 0 : pq.top()[0]);
        int r = pq.top()[1], c = pq.top()[2];
        is_visit[r][c] = true;

        for (int k=0; k<4; k++)
        {
            int nr = r + dr[k], nc = c + dc[k];
            if (nr == N || nc == N || nr < 0 || nc < 0 || is_visit[nr][nc])
                continue;
            pq.push({abs(land[r][c] - land[nr][nc]), nr, nc});
        }   
        
        while(pq.empty() == false && is_visit[pq.top()[1]][pq.top()[2]])
            pq.pop();
    }
    return answer;
}
```