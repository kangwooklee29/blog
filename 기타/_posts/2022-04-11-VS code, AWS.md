---
title: VS code에서 AWS 접속하기
---


```
Host host1
    HostName xxx.xxx.xxx.xxx
    IdentityFile "C:\Users\user_id\pem1.pem"
    User user_id1
```

- Host의 이름은 어떤 값을 적어도 상관 없다. VS code에서 이 호스트에 접속할 때 사용할 이름을 적는다.
- HostName은 인스턴스의 퍼블릭 IP 주소를 적어야 한다. (퍼블릭 DNS가 아니다.)
- IdentityFile은 구체적인 pem 파일의 경로를 적어야 하며, ""로 둘러싸 적는다. 
- User는 인스턴스에서 설정한 사용자 이름을 적는다.