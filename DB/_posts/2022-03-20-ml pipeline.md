---
title: Spark MLlib로 ML Pipeline 기반 ML 모델 만들기
---

### 0. 기본 개념

#### 1) transformer

\- 입력으로 주어진 데이터프레임에 새 컬럼을 추가해 새 데이터프레임을 만드는 함수. 여러 프레임워크에서 transform()이라는 이름의 메서드로 호출할 수 있으며, 그 결과로 데이터프레임이 리턴된다.

\- ML 모델도 어떻게 보면 입력으로 주어진 데이터프레임에 '예측값'이라는 새 컬럼을 추가해 새 데이터프레임을 만드므로 일종의 transformer라고 볼 수 있다.

#### 2) estimator

\- 데이터프레임을 입력으로 받아 ML 모델을 만드는 함수. 여러 프레임워크에서 fit()이라는 이름의 메서드로 호출할 수 있으며, 그 결과로 ML 모델(transformer)이 리턴된다.

\- ML Pipeline도 어떻게 보면 데이터프레임을 입력으로 받아 ML 모델을 만드므로 일종의 estimator라고 볼 수 있다.



### 1. Spark 세션 열기

```python
!pip install pyspark py4j

from pyspark.sql import SparkSession

spark1 = SparkSession.builder.appName("app1").getOrCreate()
```

### 2. 데이터 로드

```python
!wget https://s3-geospatial.s3-us-west-2.amazonaws.com/titanic.csv #Titanic 승객 정보로 생존자를 예측하는 ML 모델을 만들어보자.

data1 = spark1.read.csv('./titanic.csv', header=True, inferSchema=True)

data1.printSchema() #titanic.csv의 schema를 볼 수 있다.
data1.show() #titanic.csv의 내용을 볼 수 있다.
data1.select(['*']).describe().show() #titanic.csv의 각 컬럼의 개수, 평균, 최댓값, 최솟값 같은 정보를 볼 수 있다.

```

### 3. data cleansing

#### 1) estimator 만들기

```python
from pyspark.ml.feature import Imputer, StringIndexer, VectorAssembler, MinMaxScaler

imputer1 = Imputer(strategy='mean', inputCols = ['Age'], outputCols = ['AgeImputed']) #imputer1은, fit() 메서드로 호출하면 '입력으로 받은 데이터프레임의 Age 컬럼의 결측치를 그 컬럼의 평균으로 채운 AgeImputed라는 컬럼을 추가하는 모델'을 리턴하는 estimator이다.
indexer1 = StringIndexer(inputCol = 'Gender', outputCol = 'GenderIndexed')
assembler1 = VectorAssembler(inputCols=['Pclass', 'SibSp', 'Parch', 'Fare', 'AgeImputed', 'GenderIndexed'], outputCol='features')
scaler1 = MinMaxScaler(inputCol = 'features', outputCol = 'features_scaled')
```

#### 2) data cleansing

```python
data1_selected = data1.select(['Survived', 'Pclass', 'Gender', 'Age', 'SibSp', 'Parch', 'Fare']) #이 컬럼들만 모델 훈련에 사용
```

(1) 결측치 채우기

```python
imputer1_model = imputer1.fit(data1_selected) #data1_selected라는 데이터프레임을 입력으로 하여 imputer1 estimator를 호출한다. 결과값으로 ML 모델을 리턴받는다.
data1_cleaned = imputer1_model.transform(data1_selected) #imputer1_model에 담긴 모델에 data1_selected라는 데이터프레입을 입력으로 하여 impute 연산이 처리된 새로운 데이터프레임을 리턴받는다.
```

(2) 문자열 인덱싱

```python
indexer1_model = indexer1.fit(data1_cleaned) 
data1_cleaned = indexer1_model.transform(data1_cleaned) 
```

(3) feature vector 만들기

```python
vec1 = assembler1.transform(data1_cleaned)
```

(4) 데이터값 크기를 0과 1사이로 scaling

```python
scaler1_model = scaler1.fit(vec1) 
vec1 = scaler1_model.transform(vec1) 
```


### 4. binary classification model 만들고 평가해보기

#### 1) train, test set 나누기

```python
train, test = vec1.randomSplit([0.7, 0.3])
```


#### 2) 모델 만들고 예측 결과 얻기
```python
from pyspark.ml.classification import LogisticRegression

lr1 = LogisticRegression(featuresCol='features_scaled', labelCol='Survived')
lr1_model = lr1.fit(train)
prediction1 = lr1_model.transform(test)
```


#### 3) 성능평가

```python
from pyspark.ml.evaluation import BinaryClassificationEvaluator

ev1 = BinaryClassificationEvaluator(labelCol='Survived', metricName='areaUnderROC')
ev1.evaluate(prediction1)
```


### 4. data cleansing부터 성능 평가까지 진행하는 ML Pipeline 만들기

#### 1) ML Pipeline 만들기

```python
from pyspark.ml.classification import LogisticRegression
from pyspark.ml import Pipeline

lr1 = LogisticRegression(featuresCol='features_scaled', labelCol='Survived')
pipeline1 = Pipeline(stages=[imputer1, indexer1, assembler1, scaler1, lr1])
```

#### 2) train, test set 나누고 예측 결과 얻고 성능평가

```python
#data1_selected = data1.select(['Survived', 'Pclass', 'Gender', 'Age', 'SibSp', 'Parch', 'Fare'])

train, test = data1_selected.randomSplit([0.7, 0.3])

pipeline1_model = pipeline1.fit(train)
prediction2 = pipeline1_model.transform(test)

ev1.evalutate(prediction2)
```


### 5. ML Tuning

\- 위에서 만든 ML Pipeline을 이용해 하이퍼파라미터를 결정하는 cross validation을 자동으로 수행할 수 있다.

```python
from pyspark.ml.tuning import ParamGridBuilder, CrossValidator

grid1 = (ParamGridBuilder().addGrid(lr1.maxIter, [1, 5, 10]).build())
cv1 = CrossValidator(estimator=pipeline1, estimatorParamMaps=grid1, evaluator=ev1, numFolds=5)

cv1_model = cv1.fit(train)
prediction3 = cv1_model.transform(test)

ev1.evaluate(prediction3)
```
