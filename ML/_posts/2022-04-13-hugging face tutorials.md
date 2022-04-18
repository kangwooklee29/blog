### 추론 파이프라인

Model Hub의 어떤 모델이든 pipeline()을 사용하여 텍스트 생성, 이미지 분할 및 오디오 분류와 같은 다양한 작업에 대한 추론을 쉽게 할 수 있다. pipeline()은 어떤 양식을 사용해 본 경험이 없거나 모델의 코드를 잘 알고 있지 않더라도 사용할 수 있다.


#### 파이프라인 사용법

모든 작업에는 그에 관한 pipeline()이 있지만 모든 특정 작업 파이프라인을 포함하는 일반적인 pipeline() 추출을 사용하는 것이 더 간단하다. pipeline()은 당신이 수행할 작업에 대해 추론할 수 있는 기본 모델 및 토큰라이저를 자동으로 로드한다.

1. pipeline()을 만들 때 어떤 추론을 할지 지정한다.

```python
from transformers import pipeline

generator = pipeline(task="text-generation")
```

2. pipeline()에 입력 텍스트를 전달한다.

```python
generator("Three Rings for the Elven-kings under the sky, Seven for the Dwarf-lords in their halls of stone")
[{'generated_text': 'Three Rings for the Elven-kings under the sky, Seven for the Dwarf-lords in their halls of stone, Seven for the Iron-priests at the door to the east, and thirteen for the Lord Kings at the end of the mountain'}]
```

입력이 두 개 이상인 경우 입력 내용을 list로 전달한다.

```python
generator(
    [
        "Three Rings for the Elven-kings under the sky, Seven for the Dwarf-lords in their halls of stone",
        "Nine for Mortal Men, doomed to die, One for the Dark Lord on his dark throne",
    ]
)
```


pipeline()에 작업에 대한 추가 매개 변수를 포함할 수도 있다. 텍스트 생성 작업에는 출력을 제어하기 위한 몇 가지 매개 변수가 있는 generate() 메서드가 있다. 예를 들어 출력을 두 개 이상 생성하려면 num_return_sequences 매개 변수를 설정한다.

```python
generator(
    "Three Rings for the Elven-kings under the sky, Seven for the Dwarf-lords in their halls of stone",
    num_return_sequences=2,
)
```


#### 모델과 tokenizer 고르기


Model Hub의 모든 모델에 대해 pipeline()을 사용할 수 있다. Model Hub에 특정 작업에 사용할 모델을 필터링할 수 있는 태그가 있다. 적절한 모델을 선택 후 해당 AutoModelFor 및 [AutoTokenizer] 클래스로 로드한다. 예를 들어, 인과 관계 언어 모델링 작업을 위해 AutoModelForCausalLM 클래스를 로드한다.

```python
from transformers import AutoTokenizer, AutoModelForCausalLM

tokenizer = AutoTokenizer.from_pretrained("distilgpt2")
model = AutoModelForCausalLM.from_pretrained("distilgpt2")
```

수행할 작업에 대한 pipeline()을 만들고 로드한 모델 및 tokenizer를 지정한다.

```python
from transformers import pipeline

generator = pipeline(task="text-generation", model=model, tokenizer=tokenizer)
```

pipeline()에 입력 텍스트를 전달하여 텍스트를 생성한다.


```python
generator("Three Rings for the Elven-kings under the sky, Seven for the Dwarf-lords in their halls of stone")
[{'generated_text': 'Three Rings for the Elven-kings under the sky, Seven for the Dwarf-lords in their halls of stone, Seven for the Dragon-lords (for them to rule in a world ruled by their rulers, and all who live within the realm'}]
```

### AutoClass를 사용하여 사전 훈련된 인스턴스 로드

수많은 서로 다른 Transformer 아키텍처 중에서 하나를 골라 당신의 체크 포인트에 대한 아키텍처를 만드는 것이 어려울 수 있다. 라이브러리를 쉽고 간단하며 유연하게 사용할 수 있게 하자는 Transformers 핵심 철학의 일환으로, AutoClass는 주어진 체크 포인트에 대한 올바른 아키텍처를 자동으로 추론하고 로드한다. from_pretrained 메소드를 사용하면 모든 아키텍처에 대해 사전 훈련된 모델을 신속하게 로드할 수 있으므로 처음부터 모델을 훈련시키는 데 시간과 리소스를 투자할 필요가 없다. 이처럼 특정 체크 포인트에 구애받지 않는 코드를 생성한다는 것은, 어떤 코드가 어느 한 체크 포인트에 대해 잘 작동한다면 설사 아키텍처가 다르더라도 그것이 비슷한 작업을 위해 훈련되었다면 다른 체크 포인트에 대해서도 잘 작동한다는 것을 의미한다.


> '아키텍처'는 모델의 골격을 뜻하며, '체크 포인트'는 주어진 아키텍처가 가진 가중치(파라미터)를 뜻한다. 예를 들어, BERT는 아키텍처고, BERT-base-uncased는 체크포인트다. '모델'은 아키텍처, 체크포인트 모두를 지칭하는 일반 용어다.


#### AutoTokenizer

거의 모든 NLP 작업은 시작할 때 AutoTokenizer를 먼저 수행한다. AutoTokenizer는 입력된 내용을 모델에서 처리할 수 있는 형식으로 변환한다.

```python
from transformers import AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")

print(tokenizer("In a hole in the ground there lived a hobbit."))
```

#### AutoModel

AutoModelFor 클래스를 사용하면 주어진 작업에 대해 사전 훈련된 모델을 로드할 수 있다. 예를 들어, AutoModelForSequenceClassification.from_pretrained()는 시퀀스 분류 모델을 로드한다.


```python
from transformers import AutoModelForSequenceClassification
model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased")
```

다음과 같이 간단히 똑같은 체크 포인트를 사용하는 다른 작업에 대한 아키텍처를 로드한다.

```python
from transformers import AutoModelForTokenClassification
model = AutoModelForTokenClassification.from_pretrained("distilbert-base-uncased")
```


### 전처리

어떤 데이터를 ML 모델에서 사용하려면 데이터를 모델에서 사용 가능한 형식으로 전처리해야 한다. 모델은 원시 텍스트, 이미지 또는 오디오를 이해하지 못하므로 이 입력들을 숫자로 변환해 텐서로 조립해야 한다. 

텍스트 데이터 전처리의 주요 도구는 Tokenizer다. 텍스트는 Tokenizer에 의해 특정 규칙에 따라 토큰으로 분할되며, 이 토큰은 숫자로 변환되어 모델에 입력되는 텐서가 된다. 모델에 필요한 추가 입력도 Tokenizer를 거쳐 추가된다.

> 사전 훈련된 모델을 사용하는 경우 그에 관련된 사전 학습된 tokenizer를 사용해야 한다. 이것이 보장돼야 사전 학습에 사용된 corpus가 tokenize된 것과 같은 방식으로 입력 텍스트를 tokenize한 후 입력할 수 있으며, 또 사전 훈련 때와 같은 토큰-to-인덱스(일반적으로 vocab이라고 함)를 사용할 수 있다.

AutoTokenizer 클래스를 사용하면 사전 훈련된 tokenizer를 빠르게 로드할 수 있다. 이때 모델을 사전 훈련할 때 사용한 vocab을 다운로드한다.

#### 토큰화

AutoTokenizer.from_pretrained()를 사용하여 사전 훈련 된 tokenizer를 로드한다.

```python
from transformers import AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
```

그 다음에 입력 문장(또는 문장들이 담긴 리스트)을 tokenizer에 넣는다.

```python
encoded_input = tokenizer("Do not meddle in the affairs of wizards, for they are subtle and quick to anger.")
```

tokenizer가 반환하는 딕셔너리의 세 요소는 다음과 같다.

- input_ids: 문장의 각 토큰이 갖는 인덱스. 이 정보만으로도 tokenizer.decode() 메서드에 넣으면 원래 문장을 다시 알 수 있다. (다만 모델에 따라 종종 tokenizer가 문장 앞뒤로 [CLS], [SEP] 토큰을 추가하므로 이 경우 decode 후 얻게 되는 문장에는 이 토큰들이 추가된 것이 보여진다.)

```python
print(tokenizer.decode(encoded_input["input_ids"]))
```

- token_type_ids: 문장에 둘 이상의 시퀀스가 있을 때 그 토큰이 어느 시퀀스에 속하는지를 나타내는 요소

- attention mask: 실제 단어/패딩 토큰(시퀀스 길이를 맞추기 위해 빈칸에 넣는 정보값 없는 토큰) 여부를 체크해주는 요소


#### 패딩

한 묶음의 문장이 입력으로 주어질 때, 이들의 길이가 항상 같진 않다. 그런데 모델의 입력인 텐서는 크기가 균일해야 하기 때문에 이는 문제다. 패딩은, 이 문제를 해결하기 위해 토큰이 적은 문장에 특수 패딩 토큰을 추가하여 텐서의 크기를 균일하게 하는 작업이다.

패딩 파라미터를 True로 설정하면 배치의 짧은 시퀀스와 가장 긴 시퀀스의 크기가 서로 일치하게 된다.

```python
batch_sentences = [
    "But what about second breakfast?",
    "Don't think he knows about second breakfast, Pip.",
    "What about elevensies?",
]
encoded_input = tokenizer(batch_sentences, padding=True)
```

#### 잘라내기

반대로, 시퀀스가 모델이 처리하기에 너무 길 수 있다. 이 경우 시퀀스를 더 짧은 길이로 잘라야 한다.

truncation 매개 변수를 True로 설정하면 모델이 허용하는 최대 길이로 시퀀스를 잘라낸다.

```python
encoded_input = tokenizer(batch_sentences, padding=True, truncation=True)
```

#### 텐서 만들기

PyTorch의 경우 tokenizer의 return_tensors 매개 변수로 pt를 사용하면 모델에 입력으로 전달하고자 하는 문장을 모델에 실제 입력할 텐서 형태로 변환할 수 있다.

```python
encoded_input = tokenizer(batch, padding=True, truncation=True, return_tensors="pt")
```

