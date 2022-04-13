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

수많은 서로 다른 Transformer 아키텍처 중에서 하나를 골라 당신의 체크 포인트에 대한 아키텍처를 만드는 것이 어려울 수 있다. 라이브러리를 쉽고 간단하며 유연하게 사용할 수 있게 하자는 Transformers 핵심 철학의 일환으로, AutoClass는 주어진 체크 포인트에 대한 올바른 아키텍처를 자동으로 추론하고 로드한다. from_pretrained 메소드를 사용하면 모든 아키텍처에 대해 사전 훈련된 모델을 신속하게 로드할 수 있으므로 처음부터 모델을 훈련시키는 데 시간과 리소스를 투자할 필요가 없다. 이처럼 특정 체크 포인트에 구애받지 않는 코드를 생성한다는 것은, 어떤 코드가 어느 한 체크 포인트에서 작동한다면 설사 아키텍처가 다르더라도 그것이 비슷한 작업을 위해 훈련되었다면 다른 체크 포인트에서도 잘 작동한다는 것을 의미한다.


> '아키텍처'는 모델의 골격을 뜻하며, '체크포인트'는 주어진 아키텍처에 대한 가중치를 뜻한다. 예를 들어, BERT는 아키텍처고, BERT-base-uncased는 체크포인트다. '모델'은 아키텍처, 체크포인트 모두를 지칭하는 일반 용어다.


