### 1. multiplexing과 demultiplexing

\- 상위 계층에서 여러 단위로 나온 데이터를 하위 계층에서 하나로 묶는 것을 multiplexing이라 한다. 

- application 계층의 여러 소켓에서 나온 message를 transport 계층에서 multiplexing한 단위: segment

- transport 계층의 여러 segemnt를 network 계층에서 multiplexing한 단위: packet

- network 계층의 여러 packet을 link 계층에서 multiplexing한 단위: frame

\- multiplexing을 한 결과 메시지의 맨 앞 부분에는 항상 헤더가 위치하는데, 이 헤더에는 그 메시지가 보내질 네트워크 상의 주소값이 저장되게 된다.

\- multiplexing 된 메시지를 리시버 측에서 해석해 각 소단위가 전송돼야 할 상위 계층 단위를 구분해 배분하는 작업을 demultiplexing이라 한다.

### 2. UDP의 segement 헤더와 demultiplexing

#### 1) UDP의 segment 헤더

\- UDP의 segement의 헤더는 다음 4개의 필드로 이루어져 있다.

- source port 번호: 16비트가 할당되므로, 이 port 번호가 가질 수 있는 값은 0부터 65535 사이 정수이다.

- destination port 번호: 각 메시지가 리시버의 어떤 포트 번호에 전달되는지 그 값들이 저장돼 있다. 이 값들을 기준으로 demultiplexing을 하게 된다.

- segment의 길이

- checksum


#### 2) UDP의 demultiplexing

\- TCP는 서버와 클라이언트 사이 메시지 전송이 있을 때마다 매번 그 전송만을 위해 새로 소켓을 생성하지만, UDP의 경우 각 프로세스마다 정해진 하나의 소켓만을 사용한다. UDP의 segment 헤더가 단순한 것은 이 때문이다.


### 3. TCP의 segment 헤더와 demultiplexing

