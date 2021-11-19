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


### 3. RDT 프로토콜

\- TCP가 reliable한 data transfer를 지원하는 프로토콜이라고 했는데, 사실 transport 계층보다 하위 계층에서는 data transfer가 reliable하지 않으며 TCP가 reliable한 것은 reliable한 data transfer를 위하여 이를 지원하는 각종 기능을 구현했기 때문이다.

\- TCP를 이해하기에 앞서, 보다 이상적인 상황에서 reliable한 data transfer가 어떻게 이루어지는지를 이해해볼 수 있다. 이처럼 이상적인 상황에서 reliable한 data transfer를 지원하는 프로토콜을 'RDT 프로토콜'이라고 해보자. 구체적으로 RDT 프로토콜을 (1)message를 에러 없이 전송하는 것을 지원하며 (2)특히 message를 유실 없이 전송하는 것을 지원하는 프로토콜이라고 해보자.

#### 1) message의 유실이 없고, meassage에 에러가 발생하지도 않는 경우

- 이 경우 RDT 프로토콜이 특별히 추가 기능을 지원할 필요는 없다. 이때의 RDT 프로토콜을 RDT 1.0이라 하자.


#### 2) message의 유실은 없지만 전송된 message에 에러가 발생할 수 있는 경우(RDT 2.x)

- 이 경우 RDT 프로토콜이 다음 기능을 지원하게 하여 reliable한 data transfer를 보장할 수 있다. (RDT 2.0)

(1) 헤더에 checksum을 두어 에러 여부를 판단(error detection)

(2) 리시버 측에서 에러 없는 메시지를 받았다면 ACKs(acknowledgements) 메시지를, 에러 있는 메시지를 받았다면 NAKs(negative acknowledgements) 메시지를 전송(feedback)

(3) 리시버로부터 NAKs를 받았다면, 이전 메시지를 재전송(retransmission)


- RDT 2.0의 경우, 리시버 측이 보낸 ACK에 에러가 발생해 NAK처럼 보일 수 있다는 문제가 있다. 이 경우, 리시버로부터 NAK를 받았다면 무조건 기존 메시지를 재차 보내되 이 메시지가 종전 메시지와 동일한 메시지인지 아닌지만 구분하는 비트(sequence number)를 헤더에 추가해 보내는 식으로 문제를 해결할 수 있다. (RDT 2.1)

- 리시버가 메시지를 받았을 때 feedback으로 ACK나 NAK를 보내는 게 아니라, **수신한 메시지의 sequence number를 feedback으로 보내는 프로토콜**을 생각할 수 있다. (RDT 2.2) 이런 식으로 구현하더라도 리시버 측으로 보낸 메시지가 에러 없이 잘 전달됐는지 에러 메시지가 전달됐는지를 확인하여 재전송 절차의 수행 여부를 결정할 수 있다.


### 3) message에 에러가 발생할 수 있고, message의 유실이 있을 수도 있는 경우(RDT 3.0)

- 이 경우 feedback이 돌아오는 데까지 걸린 시간으로 message의 유실 여부를 파악하여 재전송 절차의 수행여부를 결정하는 프로토콜을 생각할 수 있다. (RDT 3.0) 

- RDT 3.0에서는 timer를 얼마나 오래 설정해야 하는지가 중요한 문제가 된다. timer가 길면 message 유실에 대응하는 속도가 느려지지만 message 유실이 아님에도 불구하고 재전송 절차를 수행해 발생하는 오버헤드를 줄일 수 있으며, timer가 짧으면 message 유실에 대응하는 속도가 빨라지지만 message 유실이 아님에도 불구하고 재전송 절차를 수행해 발생하는 오버헤드가 커진다.




### 4. pipelined protocols

RDT 프로토콜은 한 번 패킷을 송신하면 그에 대한 feedback이 돌아올 때까지 다음 패킷을 송신하지 않는다. 이 때문에, 통신이 이뤄지는 전체 시간 중 실제로 데이터 송/수신이 차지하는 시간의 비율이 매우 낮아지게 된다. 이는 대단히 비효율적이므로, 실제 TCP는 한꺼번에 여러 개의 패킷을 보낸 후 그 이후에 돌아오는 각 패킷에 대한 feedback을 확인하여 패킷의 유실이나 에러 여부를 확인한다. 이처럼 한꺼번에 여러 개의 패킷을 보내는 식으로 통신 효율을 높인 프로토콜을 pipelined protocol라 한다. 다음은 pipelined protocol을 구성하는 방식들이다.

#### 1) go-back-N

\- go-back-N 방법은 다음과 같은 방식으로 동작한다.

(1) 각 메시지는 처음부터 끝까지 순서대로 번호가 붙어있으며, 앞 번호부터 한 번에 여러 개의 메시지를 하나의 단위(window)로 묶어 송신한다.

(2) 리시버 측에서는 메시지를 수신할 때마다 0번부터 n번까지 모든 메시지를 잘 수신했다는 메시지(ACK(n))를 feedback으로 리턴한다. 

- 이때, 리턴하는 번호 n이 방금 막 수신한 메시지 번호와 다를 수 있다. 예를 들어 하나의 window로 0, 1, 2, 3, 4번 메시지가 보내졌는데 이 중 2번 메시지가 유실됐다면, 리시버 측에서는 **3, 4번 메시지를 수신한 경우에도 ACK(1)을 두 번 반복해 feedback으로 리턴**한다.

(3) feedback을 수신할 때마다 window가 시작하는 메시지 번호를 그 feedback에 맞춘다. 예를 들어, feedback으로 ACK(1)가 들어오면 window가 시작하는 메시지 번호는 2번으로 변경된다.

- 예를 들어 하나의 window로 0, 1, 2, 3, 4번 메시지가 송신되었는데 feedback으로 ACK(1)가 두 번 들어왔다면, 이는 곧 2번 메시지와 그 이후 메시지 중 어느 하나가 함께 유실되었음을 뜻한다. 이 경우 window가 시작하는 메시지 번호는 2번으로 변경되며, 재송신되는 window는 2, 3, 4, 5, 6번 메시지가 된다. 즉, **3, 4번 메시지 중 어느 메시지가 유실됐는지 알 수 없으므로 그냥 이를 한꺼번에 다시 재송신**하는 것이다.


\- 실제로 이 방식으로 프로토콜을 만들 경우 사용하게 될 window는 메시지를 수천 개 담아야 할 것인데, 이 방식은 메시지 유실이 발생할 때마다 메시지 수천 개를 중복해서 재송신해야 하며 이는 매우 비효율적이다.


#### 2) selective repeat

\- 