### 1. matplotlib

#### 1) 그래프 그리기

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3], [4, 5, 6]) #plot() 대신 scatter()를 쓸 수도 있다. 이 경우 그래프 대신 점만 찍힌다.
plt.show()
```

\- matplotlib.pyplot의 plot(), scatter() 등의 함수를 이용해서 좌표평면에 그래프를 그릴 수 있다. 이들 함수는 인자로 좌표평면 위에 찍어야 할 점의 x좌표, y좌표를 list형으로 받아 그에 해당하는 위치에 점을 찍는다. 

- plot(): 인자로 전달된 점들 사이를 직선으로 잇는 꺾은선 그래프를 그린다. matplotlib에서 공식적으로 곡선을 그릴 수 있는 방법은 지원되지 않고, 보통은 numpy.arange()로 간격이 1보다 작은 실수들이 연속해서 담겨 리턴된 array를 plot()의 인자로 전달하여 곡선에 가까운 그래프 개형을 얻는다.

- scatter(): 단순히 인자로 전달된 점들을 그대로 좌표평면에 찍기만 한다.

- 그래프를 그리는 함수에 인자로 label을 지정하여 전달하면 그 함수로써 그려지는 그래프의 이름을 지정할 수 있다. 다만 이렇게 지정된 이름은 범례를 표시하게 하는 함수(legend())를 호출해야 그 이름이 그래프와 함께 표시된다.

```python
plt.plot(x, func1(x), label="func1")
plt.legend()
```

\- 이들 함수는 단지 메모리 상에 저장된 좌표평면 공간에 점을 찍을 뿐이며, 점이 찍혀있는 좌표평면 공간을 화면에 보이게 하려면 show() 함수를 호출해야 한다.


#### 2) show()로 보여지는 화면을 조작하는 방법


(1) show()로 보여지는 화면의 크기 조절하기

```python
plt.figure(figsize=(w, h))
```


(2) 그래프, 축에 이름 붙이기

```python
plt.title("제목")
plt.xlabel("x축")
plt.ylabel("y축")
```


(3) 그려지는 x축, y축의 범위 지정하기

```python
plt.axis([x_min, x_max, y_min, y_max])
```

(4) 축에 표시되는 숫자 지정하기

```python
plt.xticks(list1)
plt.yticks(list2)
```



### 2. matplotlib와 seaborn이 지원하는 여러 그래프들

```python
plt.bar(x, y) #각 x에 대응되는 y의 값을 막대그래프로 그린다.
plt.pie([40, 40, 3, 3], label=['이', '윤', '심', '안']) #인자로 전달된 list의 각 원소 크기가 그 원소가 원형 그래프에서 차지하는 크기가 되는 원형 그래프를 그린다. label로 지정된 list의 각 원소가 인자로 전달된 list의 각 원소가 갖는 이름이 된다.
plt.boxplot((a1, a2)) #인자로 전달된 값들(list형)의 최댓값, 최솟값, 중간값, 1/4값, 3/4값에 관한 정보를 하나의 box 모양 그래프에 담아 그린다. 인자로 받는 튜플의 크기는 하나 이상일 수 있으며, 인자로 받은 튜플의 각 성분 list에 대응되는 박스가 x축을 따라 하나씩 순서대로 그려진다.
```


```python
import seaborn as sns

plt.hist(y, bins=list1) #y의 값들로 히스토그램을 그린다. x축은 bins로 지정된 리스트에 대응되는 범위가 그려지며, y의 값들 중 각 범위에 해당하는 값의 개수가 그때의 x값 범위에 대응되는 y값이 된다.
sns.kdeplot(y, shade=True) #y의 값들로 그려진 히스토그램을 좀 더 smooth한 곡선으로 그린다. 인자로 shade를 True로 지정하여 전달되면 곡선 아래가 색칠된다.

d3 = pd.read_csv("dataframe1.csv")
sns.countplot(x=d3["col_name"]) #x로 DataFrame의 열의 Series를 지정하여 인자로 전달하면, 그 열의 각 값들을 x축으로 하고 또 groupby() 연산을 하며, 거기서 count 그룹함수를 적용한 값을 각 x에 대응되는 y값으로 하는 막대그래프를 그린다.
sns.catplot(x="col1", y="col2", data=d3) #data로 DataFrame을 지정하고 x와 y에 각각 DataFrame의 열 이름을 지정하여 전달하면, x축을 따라 x로 지정된 열에 해당하는 각 값이 각 영역을 차지하고 그때 y로 지정된 열에 해당하는 값이 y값이 되어 그래프가 그려진다. kind를 지정하여 인자로 전달하면 그려지는 모습을 strip에서 violin 등으로 변경할 수도 있다. 기능이 비슷하면서 다른 관점에서 데이터를 볼 수 있는 stripplot(), swarmplot() 등도 있다.
sns.heatmap(d3.corr()) #인자로 전달된 DataFrame의 각 열 사이 상관관계를 히트맵으로 그린다.
```
