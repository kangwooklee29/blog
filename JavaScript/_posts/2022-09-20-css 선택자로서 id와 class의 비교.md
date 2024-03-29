---
title: CSS 선택자로서 id와 class의 비교
---


흔히 알려진 지식으로, 'id 선택자는 페이지 내에서 한 id가 한 엘리먼트만을 지칭하고 class 선택자는 여러 엘리먼트를 동시에 지칭할 수 있다' 정도가 알려져 있다. 그런데 어떤 엘리먼트가 페이지 내에서 한 번만 쓰일지 아니면 여러 번 쓰일지 확신할 수 없을 때는 어떤 선택자를 쓰는 게 더 나을까? class 선택자를 쓰는 게 나으며, 한 번만 쓰일 가능성이 압도적으로 높은 엘리먼트라 하더라도 되도록이면 class 선택자를 쓰는 게 더 낫고 id 선택자는 피하는 편이 낫다. 

이는 이런 이유가 있기 때문이다. 극단적인 경우를 가정해보자. 페이지를 구현하며 각 엘리먼트의 CSS 세팅을 변경하고 있는데 아무리 해도 특정 엘리먼트가 변경사항이 잘 적용이 안 되는 경우를 생각할 수 있다. 이 경우 !important 속성을 추가하여 상속을 무시하고 의도한 스타일 적용이 나타나도록 하고자 하는 유혹을 강하게 느끼게 되는데, 만약 이 유혹에 지나치게 흔들려 페이지 내 모든 엘리먼트에 !important 속성이 붙게 돼버렸다면 그 페이지는 나중에 기존과 전혀 다른 테마의 CSS 세팅을 하고 싶어도 모든 엘리먼트의 속성을 하나하나 다 변경해야 하는 상황이 발생하게 된다. 

우선순위가 낮은 선택자 위주로 엘리먼트에 대한 CSS 적용을 고려하고 우선순위가 높은 선택자는 아주 예외적 상황에나 적용을 고려하는 게 이득인 이유는 이와 같은 상황이 얼마든지 일어날 수 있기 때문이다. 예를 들어 어떤 엘리먼트가 페이지 내에 단 한 번만 등장할 것이라고 확신하고 id 선택자를 사용해 구현한 후 나중에 다른 엘리먼트에 그 엘리먼트에 적용한 속성을 그대로 적용하고 싶을 때 또 새로운 id 선택자를 설정하여 똑같은 내용을 베껴넣었을 경우, 두 엘리먼트 공통적으로 동시에 변경하고 싶은 속성이 있더라도 일일이 고치는 것은 번거롭고 또 몇가지 사항을 잊어 수정이 다 제대로 이루어지지 않는 등 곤란한 일이 발생하게 된다.
