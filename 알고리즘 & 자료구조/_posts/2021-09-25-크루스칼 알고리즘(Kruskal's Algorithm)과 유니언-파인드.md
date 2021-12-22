---
layout: post
title: 크루스칼 알고리즘(Kruskal's algorithm)과 유니언-파인드
---
```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

struct comp
{
    bool operator()(vector<int> a, vector<int> b)
    {
        return a[2] > b[2];
    }
};

priority_queue<vector<int>, vector<vector<int>>, comp> pq;
vector<int> parents(1, 0), ranks(1, 1);

int root(int i)
{
    if (parents[i] != i)
        return parents[i] = root(parents[i]); 
        //find 연산을 하면서, parents[i]가 바로 루트를 가리키도록 배열값을 갱신한다.
        //갱신을 하지 않는다면 find 연산 때마다 시간복잡도가 최악의 경우 O(n)이 된다.
        //그러나 이처럼 path compression을 하면 find 연산의 시간복잡도가 O(logn)까지 줄어든다.
    else
        return i;
}

int kruskal()
{
    int answer = 0;
    while(pq.empty() == false)
    {
        int r0 = root(pq.top()[0]), r1 = root(pq.top()[1]);
        if ( r0 != r1 ) //이 간선의 두 노드가 서로 다른 트리에 속한다면
        {               //당연히 각 루트노드 또한 서로 다를 것.
        
            answer += pq.top()[2];
            
            parents[r0] = r1;
            /*
            parents[r0] 값을 갱신함으로써 r0의 트리를 r1의 서브트리로 병합(union)한다.
            path compression이 없다면 이 코드는 신장트리를 불균형하게 만들 위험이 있어,
            이 경우 find 연산의 최악의 경우에 시간복잡도가 O(n)이 될 수 있다. 
            
            이 코드 대신, 다음과 같이 각 부분트리의 높이를 ranks 배열에 저장해두었다가
            union 연산 때마다 높이가 낮은 트리를 높이가 높은 트리의 서브트리로 붙이는 식으로
            union 연산을 수행하면(union by rank) path compression 없이도 시간복잡도가 
            O(logn)까지 줄어든다.
            
            if (ranks[r0] < ranks[r1]) 
                parents[r0] = r1; //r0를 r1 밑에 붙인다.
            else
                parents[r1] = r0; //r1을 r0 밑에 붙인다.
                
            if (ranks[r0] == ranks[r1]) 
                ranks[r0]++; //r1의 트리는 r0의 서브트리가 되었으므로 ranks[r0]가 증가.
                
            ranks[] 배열값의 증가는 두 값이 같은 경우일 때만 해도 충분하다.
            두 값이 다를 때에는 트리의 병합 이후에도 트리의 높이가 증가하지 않기 때문이다.
            */
        }
        pq.pop();
    }
    return answer;
}

int main()
{
    int V, E;
    cin >> V >> E;
    for (int i=1; i<=V; i++)
    {
        parents.push_back(i);
        ranks.push_back(1);
    }
    
    for (int i=0; i<E; i++)
    {
        int n1, n2, w;
        cin >> n1 >> n2 >> w;
        pq.push({n1, n2, w});
    }
    
    cout << kruskal();
    return 0;
}
```