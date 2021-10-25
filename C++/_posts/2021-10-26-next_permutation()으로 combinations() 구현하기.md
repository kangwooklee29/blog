```cpp
/*
  값이 true와 false 둘뿐인 원소로 이루어진 vector에 대해 next_permutation() 연산을 수행하면,
  각 인덱스별로 true 또는 false 값을 갖는 모든 경우의 순열을 구할 수 있다.
  이러한 성질을 이용하여 combinations() 함수를 간단히 구현할 수 있다.
*/

vector<vector<int>> combinations(vector<int> items, int r)
{
    vector<vector<int>> comb_list;
    vector<bool> sw;
    for (int i=0; i<items.size(); i++)
        sw.insert(sw.begin(), i<r);
    do
    {
        comb_list.push_back({});
        for (int i=0; i<sw.size(); i++)
            if (sw[i]) comb_list.back().push_back(items[i]);
    } while(next_permutation(sw.begin(), sw.end()));
    return comb_list;
}
```