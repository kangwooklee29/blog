### 추론 파이프라인

pipeline()을 사용하면 Model Hub의 모든 모델을 텍스트 생성, 이미지 분할 및 오디오 분류와 같은 다양한 작업에 대한 추론을 위해 쉽게 사용할 수 있다. 어떤 양식을 사용해 본 경험이 없거나 모델의 코드를 잘 알고 있지 않더라도 pipeline()을 사용할 수 있다. 이 튜토리얼에서는 다음 사항을 설명한다.

- 추론에 pipeline()을 사용한다.

- 특정한 tokenizer나 모델을 사용한다.

- 오디오나 시각 작업에 pipline()을 사용한다.


#### 파이프라인 사용

모든 작업에는 그에 관한 pipeline()이 있지만 모든 특정 작업 파이프라인을 포함하는 일반적인 pipeline() 추출을 사용하는 것이 더 간단하다. pipeline()은 당신이 수행할 작업에 대해 추론할 수 있는 기본 모델 및 토큰라이저를 자동으로 로드한다.

1. pipeline()을 만들고 추론할 작업을 지정하여 시작한다.

```python
from transformers import pipeline

generator = pipeline(task="text-generation")
```

2. 입력 텍스트를 pipeline()에 전달한다.

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


작업에 대한 추가 매개 변수를 pipeline()에 포함할 수도 있습니다. 텍스트 생성 작업에는 출력을 제어하기 위한 몇 가지 매개 변수가 있는 generate() 메서드가 있습니다. 예를 들어 출력을 두 개 이상 생성하려면 num_return_sequences 매개 변수를 설정하십시오.