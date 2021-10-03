---
layout: post
title: N-Queen(프로그래머스 > 연습문제 > N-Queen)
---
N x N 격자판에서의 간단한 DFS 문제.

```cpp
/*

1. N x N 격자판에 체스의 퀸을 N개 배치하되 각 퀸이 서로 공격할 수 없는 위치에만 퀸을 배치할 경우,
   가능한 배치의 경우의 수를 구하는 문제.
   
2. 다음 알고리즘으로 코드를 구현했다.
   (1) i번째 행에서 퀸을 놓기 합당한 열을 탐색한다.
     - i-1번째 행까지 총 i-1개의 퀸이 배치되어 있으므로, i번째 행의 j열에 퀸을 배치한다 할 때
       그 열에 퀸을 배치하면 그 이전에 배치된 i-1개의 퀸에게 공격받는지를 각각 체크한다.
     - j열에 퀸을 배치할 수 있다면 스택에 그 퀸의 위치를 넣고 재귀호출로 i+1번째 행에 대해 
       같은 과정을 반복한다. 배치할 수 없다면 j+1열에 퀸을 배치하고 같은 과정을 반복한다.
   (2) N번째 행까지 모두 퀸을 놓았다면 answer 값을 1 더한다.

3. 어렸을 적 백트래킹을 처음 배울 때 구현력 부족으로 씁쓸한 기억이 있는 문제.
   구현력이 부족하면 문제를 해결하기 위해 머릿속에 떠오르는 온갖 개념을 다 코드로 구현하려
   애쓰다 쓸데없이 코드가 길어지기 쉬운 문제다.
   간결한 구현의 핵심은 쓸데없는 코드를 쳐내고 문제에서 필요한 개념만 골라 구현하는 것이다.
   N퀸 문제는 재귀호출 때마다 각 줄에 하나의 퀸을 차례대로 배치하는 식으로 로직을 구현하면
   군더더기 있는 코드를 쓸 여지가 거의 사라지게 된다.

   아래 코드에서는 (1)변수명을 직관적으로 설정하였고 (2)함수 호출 때 불필요한 parameter를
   줄여 직관성을 높였으며 (3) C++ 11에서 추가된 범위기반 for문, C++ 17에서 추가된
   structured binding을 활용하여 가독성을 높여보았다. 또 if문도 가능한 최소로 줄여보았다.
*/


#include <vector>
#include <cmath>

using namespace std;

vector<pair<int, int>> queen_pos;
int N, answer;

bool is_safe(int i, int j)
{
    for (auto [p_i, p_j] : queen_pos)
        if (j == p_j || abs(p_j - j) == i - p_i)
            return false;
    return true;
}

void count_cases(int i)
{
    if (i == N) 
        answer++;
    else 
        for (int j=0; j<N; j++)
            if (is_safe(i, j))
            {
                queen_pos.push_back({i, j});
                count_cases(i+1);
                queen_pos.pop_back();
            }
}

int solution(int n) {
    N = n;
    count_cases(0);
    return answer;
}
```