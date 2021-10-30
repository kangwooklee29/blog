### 1. race condition

\- 특정 메모리 주소에 있는 변수 하나의 값을 서로 다른 프로세스 또는 서로 다른 CPU(multiprocessor system일 경우)가 동시에 조작하는 경우(한쪽에서는 값을 증가시키고 다른 쪽에서는 값을 감소시키는 경우 등)가 있을 수 있다. 이처럼 한 메모리 주소의 값을 서로 다른 주체가 변경하고자 하는 상황을 race condition이라 한다.

\- 예를 들어 프로세스 A가 한 변수의 값을 가져가 이를 증가시키는 연산을 하고 결과값을 저장하러 왔는데, 그 사이에 프로세스 B가 그 변수의 값을 감소시키는 연산을 하고 그 결과값을 먼저 저장해놓은 경우가 있을 수 있다. 이 경우 프로세스 A가 더 나중에 그 주소의 변수값을 변경했으므로, 프로세스 B가 그 변수의 값을 감소시킨 사실은 메모리 기록에서 사라지게 된다. 이처럼 race condition에서는 synchronize가 중요한 문제가 된다.


### 2. race condition에 대한 커널의 대처법

\- race condition은 커널과 관련해서 흔히 일어나는 상황이므로(일반적인 사용자 프로세스는 각자 메모리의 독립된 영역만을 사용하므로 race condition은 일반적인 사용자 프로세스에서 흔히 일어나는 상황은 아니다), 커널과 관련된 race condition을 유형별로 알아보고 그에 대한 대처법을 먼저 확인할 수 있다.

#### 1) 커널의 CPU 점유 중 interrupt가 발생한 경우

\- 커널의 CPU 점유 중 interrupt가 발생하면 커널은 연산을 중단하고 interrupt 처리 루틴을 수행해야 하는데, interrupt 발생 전 값을 변경하고자 한 변수가 있어 **그 값을 읽어온 상태일 때 interrupt가 발생**했고 interrupt 처리 루틴 수행 과정에서 먼저 그 값을 변경하는 경우가 있을 수 있다. 

\- 이 경우, interrupt가 발생했다 하더라도 **일단 하던 연산을 계속 수행하게 한 후 그 연산이 끝나면 그제야 interrupt 처리 루틴을 수행**하게 하는 식으로 race condition을 해결할 수 있다.

#### 2) 프로세스가 I/O 장치 접근 위해 커널을 호출한 상황에서 timer interrupt가 발생한 경우

\- 프로세스 A가 I/O 장치 접근을 위해 커널을 호출한 상황에서 timer interrupt가 발생한 경우에, 그 다음 프로세스인 프로세스 B도 수행 도중 I/O 장치 접근을 위해 커널을 호출해 두 프로세스에서 모두 커널을 호출했고 또 커널이 호출된 각 상황에서 동일한 변수의 값을 변경하는 경우가 있을 수 있다.

\- 이 경우, interrupt가 발생했다 하더라도 **커널 모드에 있다면 일단 하던 연산을 계속 수행하게 한 후 그 연산이 끝나면 그제야 context switch**를 하게 하는 식으로 race condition을 해결할 수 있다.

#### 3) multiprocessor 환경에서 공유 메모리 상의 커널 영역의 변수를 사용하는 경우

\- 이 경우는 interrupt가 발생하는 상황이 아니므로 'interrupt가 발생했다 하더라도 일단 하던 연산을 계속 수행하게 한다'는 식으로는 문제를 해결할 수 없다.

\- 다음 두 방법으로 race condition을 해결할 수 있다.

(1) 커널을 사용하는 CPU는 한 번에 하나만 허용한다.

(2) 커널이 shared memory에 접근할 때마다 그 데이터에 lock을 걸어 다른 CPU가 그 데이터에 접근한다 하더라도 값을 변경할 수 없게 한다.



### 3. critical-section problem을 프로그램적으로 해결하기 위한 조건

\- 프로세스가 공유 데이터를 사용하는 경우, 그 프로세스의 코드 중에는 공유 데이터에 접근하는 코드가 있기 마련이다(이를 critical section이라 한다). 현재 수행하는 프로세스 코드가 critical section에 진입할 때, 프로그램적으로 공유 데이터에 lock을 걸어 도중에 interrupt가 일어난다 하더라도 현재 프로세스가 lock을 건 공유 데이터는 사용 못하게 하는 식으로 race condition을 해결할 수 있다(프로그램적 해결).

\- critical-section problem을 해결하는 프로그램이 갖추어야 할 조건은 다음과 같다.

(1) 한 프로세스가 critical section 부분을 수행 중이라면 다른 모든 프로세스들은 각자 자신의 critical section에 들어가면 안된다(mutual exclusion).

(2) 모든 프로세스가 critical section을 수행하고 있지 않다면, critical section에 들어가고자 하는 프로세스는 critical section으로 들어갈 수 있어야 한다(progress).

(3) 어떤 프로세스가 critical section 부분을 수행하겠다고 요청을 했다면 그 프로세스는 (다른 프로세스들만 critical section을 수행하여 그 프로세스는 critical section을 수행하지 못하는) starvation에 빠지지 않고 결국 그 critical section을 수행할 수 있어야 한다(bounded waiting).


### 4. critical-section problem을 프로그램적으로 해결하는 알고리즘들

#### 1) '현재 critical section 진입 권한을 갖는 프로세스의 ID'를 변수로 저장하기

\- 현재 critical section 진입 권한을 갖는 프로세스 ID를 변수로 저장하고, (1)**그 ID와 일치하는 프로세스만 critical section에의 진입을 허용**하고 그 외의 프로세스는 critical section 앞에서 무한 대기 (2)프로세스는 critical section 수행 후 즉시 그 진입권을 다른 프로세스에 양도하게 하는 알고리즘을 쓸 수 있다.

\- 이 알고리즘은 mutual exclusion은 충족하나 progress 조건을 충족하지 않는다. 또, critical section 진입권을 양도받은 프로세스는 critical section을 수행하는 경우가 그리 많지 않은데 양도한 프로세스는 critical section을 수행해야 하는 코드가 많다면 이처럼 'critical section 수행 즉시 진입권 양도' 방식은 전체적으로 비효율적일 수 있다.

#### 2) '각 프로세스가 현재 critical section으로 진입을 필요로 하는지'를 저장하는 플래그 배열을 사용하기

(1) 각 프로세스가 현재 critical section으로 진입을 필요로 하는지 여부를 저장하는 플래그 배열을 만든다.

(2) critical section으로 진입을 필요로 하는 상황이 되면, 그 프로세스의 플래그 값을 true로 변경한다.

(3) 다른 프로세스들의 플래그 값이 true인 동안은 critical section 앞에서 무한 대기하고, 모든 플래그 값이 false가 되었을 때 critical section에 진입한다.

(4) critical section을 수행한 후에는 플래그 값을 false로 변경한다.

\- 이 알고리즘도 mutual exclusion은 충족하나 progress 조건을 충족하지 않는다. 


#### 3) 플래그 배열과 critical section 진입권 갖는 프로세스 ID를 저장하는 변수 동시에 사용하기(Peterson's algorithm)


(1) 각 프로세스가 현재 critical section으로 진입을 필요로 하는지 여부를 저장하는 플래그 배열을 만든다.

(2) critical section으로 진입을 필요로 하는 상황이 되면 그 프로세스의 플래그 값을 true로 변경하고, **critical section 진입권을 갖는 프로세스 ID를 현재 프로세스가 아니라 그 다음 프로세스로 설정**한다.

(3) **현재 critical section 진입권을 갖는 프로세스의 플래그 값이 true인 동안은 critical section 앞에서 무한 대기**하고, 플래그 값이 false인 프로세스가 진입권을 갖게 되었을 때 critical section에 진입한다.

(4) critical section을 수행한 후에는 플래그 값을 false로 변경한다.

\- critical section 진입권을 자꾸 다른 프로세스에 양도하다 보면 결국 플래그 값이 false인 프로세스에게 진입권이 양도되는 순간이 반드시 오므로, 이 알고리즘은 progress 조건을 충족한다. 단, 대기하는 코드를 while문을 사용해 구현하면 대기하는 시간 동안 외부 변화 여부와 무관하게 쉬지 않고 조건문 충족 여부를 연산하는 'busy waiting' 문제가 있다. 이 경우, 대기 상태에 들어간 프로세스들을 blocked 상태로 설정하고 blocked 상태인 프로세스들을 큐에 넣어 대기하게 하여 문제를 해결할 수 있다. (그런데 blocked 상태에서 wakeup 상태로 전환하는 데는 overhead가 커질 수 있으므로, critical section이 짧은 경우라면 그냥 busy waiting이 더 효율적일 수도 있다.)

\- 또, critical section 진입 전 단계의 코드를 (a)플래그값을 true로 변경하고 (b)critical section 진입권을 양도하는 식으로 일일이 구현하면 둘 중 어느 한 작업만 겨우 막 수행했는데 거기서 갑자기 interrupt가 발생해 불완전한 상태에서 다른 프로세스가 critical section으로 들어오는 문제가 발생할 수 있다. 다만 이 경우는 일련의 과정을 atomic하게 구현하는 코드와(이때 사용하는 자료형을 흔히 semaphore라 한다) 이를 지원하는 하드웨어를 사용하면 간단하게 해결할 수 있다.