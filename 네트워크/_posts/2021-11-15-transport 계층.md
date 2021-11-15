### 1. multiplexing과 demultiplexing

\- 상위 계층에서 여러 단위로 나온 데이터를 하위 계층에서 하나로 묶는 것을 multiplexing이라 한다. 

- application 계층의 여러 소켓에서 나온 message를 transport 계층에서 multiplexing한 단위: segment

- transport 계층의 여러 segemnt를 network 계층에서 multiplexing한 단위: packet

- network 계층의 여러 packet을 link 계층에서 multiplexing한 단위: frame

\- multiplexing을 한 결과 메시지의 맨 앞 부분에는 항상 헤더가 위치하는데, 이 헤더에는 그 메시지가 보내질 네트워크 상의 주소값이 저장되게 된다.

\- multiplexing 된 메시지를 리시버 측에서 해석해 각 소단위가 전송돼야 할 상위 계층 단위를 구분해 배분하는 작업을 demultiplexing이라 한다.

### 2. segment

\- 