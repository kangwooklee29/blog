### 1. seaborn.catplot()

```python
import seaborn as sns
sns.catplot(x="col1", y="col2", hue="col3", col="col4", data=df1)
```

catplot()은 하나의 DataFrame 안에 있는 여러 개의 속성 사이 관계를 추론할 때 사용할 수 있는 그래프 도시 함수이다.

- data: 그래프를 그릴 DataFrame을 지정하는 인자이다.

- x: 그래프의 x축으로 할 속성을 지정하는 인자이다. 

 data로 DataFrame을 지정하고 x와 y에 각각 DataFrame의 열 이름을 지정하
 여 전달하면, x축을 따라 **x로 지정된 열에 해당하는 각 값이 x축상의 각 영역을 차지**하고 그때 y로 지정된 열에 해당하는 값이 y값이 되어 그래프가 그려진다. kind를 지정하여 인자로 전달하면 그려지는 모습을 strip에서 violin 등으로 변경할 수도 있다. 기능이 비슷하면서 다른 관점에서 데이터를 볼 수 있는 stripplot(), swarmplot() 등도 있다.

- col: 이를 지정하여 인자로 전달하면, 여기서 지정한 열의 값이 무엇인지를 기준으로 전체 영역을 x축을 따라 나눠 그래프를 그린다.

- hue: 이를 지정하여 인자로 전달하면, 여기서 지정한 열의 각 값마다 그래프를 그린다.
