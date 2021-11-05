### 1. 파일의 디스크 저장 방식

#### 1) contiguous allocation

\- 프로세스를 쪼개지 않고 메모리의 연속된 영역에 할당하는 방법과 유사하게, 파일을 쪼개지 않고 디스크의 연속된 영역에 할당하는 방법이다.

\- 프로세스의 메모리 연속 할당에서는 메모리를 균일한 크기로 나누는 방법이 있었지만, 파일의 디스크 연속 할당에서는 파일 하나의 크기가 워낙 천차만별이어서 그런 식으로 디스크 크기를 균일하게 나눠 할당하거나 하지는 않는다. 


\- contiguous allocation의 장점

- I/O가 빠르다. 디스크의 I/O 시간의 대부분이 헤드가 이동하는 시간인데, contiguous allocation은 헤드를 움직이지 않고 I/O를 할 수 있기 때문에 I/O 시간이 압도적으로 빠를 수밖에 없다. (따라서 공간 효율보다 속도 효율이 중요한 swapping이나 realtime file용으로는 contiguous allocation이 충분히 효율적이다.)

- 파일의 내부 정보 어디든 위치정보만으로 임의접근 할 수 있다.


\- contiguous allocation의 단점 

- external fragmentation이 발생한다.

- 각 파일 사이 간격을 얼만큼 떨어트려야 하는지가 문제가 된다. 아주 촘촘하게 설정한다면 파일에 내용을 추가하여 파일에 할당되는 영역을 넓히는 것이 불가능해지고, 너무 크게 설정한다면 internal fragmentation이 발생하는 문제가 생긴다.



#### 2) indexed allocation

\- 메모리 주소 할당의 paging과 같이, 파일, 디스크를 균일한 크기로 각각 나눠 디스크의 각 블록에 파일의 각 조각을 저장하고, paging의 page table 같이 각 조각의 디스크 내 위치를 특정 블록(이를 인덱스 블록이라 한다)에 저장하는 방법이다.

\- 파일의 크기가 매우 클 경우 하나의 블록 안에 그 파일의 디스크 내 위치를 모두 저장할 수 없는 경우가 있을 수 있다. 이 문제를 해결하는 데는 두 가지 방법이 있다.

- 인덱스 블록을 여러 개 두고 각 블록의 끝부분에 다음 인덱스 블록의 디스크 내 위치를 저장한다.

- 인덱스 블록들을 가리키는 또 다른 인덱스 블록을 둔다. (paging의 2단계 페이징과 같은 방법.)


\- indexed allocation의 장점

- external fragmentation이 발생하지 않는다. (단, internal fragmentation은 발생할 수 있다.)

- 파일의 각 조각에 임의 접근이 가능하다.

\- indexed allocation의 단점

- 아무리 작은 파일이라 해도 파일 하나가 갖는 블록 수가 최소 둘 이상이다. (파일은 대부분 크기가 아주 작은 편이므로 프로세스의 paging의 경우와 달리 이는 **매우 큰 단점**이다.) 


#### 3) linked allocation

\- 파일, 디스크를 균일한 크기로 각각 나눠 디스크의 각 블록에 파일의 각 조각을 저장하되, 동시에 **각 블록의 마지막에 그 블록의 다음 블록의 디스크 내 위치를 저장**하는 방법이다. 

\- linked allocation의 장점

- external fragmentation이 발생하지 않는다. (단, internal fragmentation은 발생할 수 있다.) 그러면서도 인덱스 블록이 필요 없다.

\- linked allocation의 단점

- 파일의 각 부분으로의 임의접근이 불가능하며, 파일의 저장 블록이 너무 제각각 떨어져 있기 때문에 디스크의 헤드가 움직이는 시간이 길어져 I/O 효율이 나빠진다.

- 블록 중 어느 하나만 배드 섹터여도 그 이후 파일 내용에는 접근이 불가능해진다.

- 각 블록의 끝에 다음 블록의 디스크 내 위치를 저장해야 하기 때문에, 다른 방식에 비할 때 한 블록에 들어가는 데이터 양이 줄어든다.



### 2. 여러 file system

1) UNIX의 file system


- inode list: 파일들의 metadata 및 각 파일의 인덱스 블록이 저장되는 블록 영역. 파일 하나의 metadata 및 그 파일의 인덱스 블록이 저장되는 블록 영역 하나를 inode라 한다. (inode list는 UNIX의 file system의 특징으로, UNIX에서는 파일의 metadata가 디렉토리가 아니라 inode list에 저장돼있다. UNIX의 디렉토리에는 그 디렉토리에 속하는 **파일명만** 저장될 뿐이다.)

  - direct blocks: inode 하나에는 direct blocks 영역이 있다. 이 영역은 그 파일의 인덱스 블록이 저장된다.

  - single indirect: 이 역시 inode가 갖는 블록 영역의 하나로, 인덱스 블록들을 가리키는 인덱스 블록이 저장돼 있다. paging의 2단계 page table에 대응된다.

  - double indirect: paging의 3단계 page table에 대응되는 인덱스 블록 영역.

- data block: 파일 데이터가 저장되는 블록 영역. data block 영역에 저장된 파일에는 metadata가 존재하지 않으며, 오직 inode 번호가 저장돼있을 뿐이다.

- super block: inode list와 data block 사이의 경계, data block 중 어떤 블록이 데이터가 들어있는 블록인지 등의 정보가 저장되는 블록 영역.


2) MS-DOS의 file system(FAT file system)

- data block: 파일 데이터가 저장되는 블록 영역. UNIX의 경우와 달리 파일의 metadata가 data block 영역의 디렉토리 파일 블록에 저장된다. 더 나아가, 파일의 정보가 시작되는 데이터 블록의 번호까지 같이 저장된다.

- FAT(fast allocation table): data block 영역의 데이터 블록 개수와 같은 엔트리 개수를 갖는 테이블로서, 각 엔트리는 그 엔트리에 해당하는 데이터 블록의 다음 데이터 블록의 번호를 담고 있다. 예를 들어 어떤 파일이 데이터 블록 5번 - 12번 - 7번 순으로 구성되어 있다면, FAT의 5번 엔트리에는 12가, 12번 엔트리에는 7이, 7번 엔트리에는 EOF가 저장된다.

\- FAT file system은 linked allocation 방식으로 작동하는 file system이지만 linked allocation의 단점을 모두 극복한 file system이다. (1)아주 좁은 FAT 영역을 통해 접근하고자 하는 데이터 블록 번호를 곧바로 얻어낼 수 있으니 직접 접근이 가능해 I/O 속도가 매우 빠르며 (2)FAT 영역에만 손상이 없다면 linked allocation 특유의 배드 섹터 이슈에서도 자유롭다. (3)데이터 블록의 다음 데이터 블록 번호가 FAT 영역에 저장돼 있으므로 다른 allocation 방식보다 한 블록이 갖는 데이터 양이 줄어들지도 않는다. 단, FAT 영역이 손상되면 결국 linked allocation의 단점이 다시 살아나므로, FAT file system에서는 보통 FAT 영역의 백업을 둘 이상 두고 있다.



### 3. free space management

새 파일을 만든다거나 기존 파일의 크기를 늘리는 등의 문제에 있어 data block 영역 내에 빈 공간이 어디에 얼마나 있는지 정보를 아는 것이 중요하다. 다음은 data block 영역 내 빈 공간 정보를 관리하는 여러 방법이다.


#### 1) bit map(bit vector)

\- data block 영역 내 **데이터 블록 개수만큼 bit**를 두어 데이터 블록이 비어있으면 0, 데이터가 있으면 1로 저장하게 하는 방식이다. 

\- 파일은 연속된 데이터 블록 영역에 저장하는 것이 효율적인데, 이 방법은 연속된 빈 공간을 찾는 데 효율적이어서 그런 부분에서는 장점이 큰 방식이다.

#### 2) linked list

\- 각 비어있는 데이터 블록 안에 다음 비어있는 데이터 블록 번호를 하나씩 저장해두는 방식이다. 연속적인 가용 공간을 찾기 어렵고 비효율적이다.

#### 3) grouping, counting

\- grouping은 paging이나 index allocation처럼 **빈 데이터 블록 번호를 저장하는 인덱스 블록**을 두는 방식이다. 이 방식도 연속적인 가용 공간을 찾는 데는 비효율적이다.

\- grouping 방식에서, 만약 특정 데이터 블록이 비어있고 그 뒤로 몇개의 데이터 블록이 연속해서 비어있다면, 인덱스 블록의 엔트리에 빈 데이터 블록의 번호를 저장하면서 거기서부터 몇개의 데이터 블록이 연속해서 비어있는지를 같이 저장하는 방법이 있다. 이를 counting이라 하며, 이러한 방식을 사용하면 grouping의 장점을 취하면서 연속적인 가용 공간도 매우 효율적으로 찾을 수 있다. 많은 프로그램이 연속된 데이터 블록 영역을 반납하는 특성이 있어, 이 방식은 실익이 크다.


### 4. 디렉토리 구현

#### 1) 디렉토리 파일의 내용 구현 방식

(1) linear list

\- 디렉토리에 속한 파일들의 이름과 metadata를 디렉토리 파일 내에 linear list 형태로 저장하는 방식이다. 구현은 간단하지만, 파일 하나의 metadata를 얻기 위해 O(n)의 순차탐색을 해야 하므로 비효율적이다.

(2) hash table

\- 디렉토리에 속한 파일들의 이름에 hash 함수를 적용해 hash table의 그 key에 해당하는 엔트리에 그 파일의 metadata를 저장하는 방식이다. 

#### 2) 긴 파일명의 구현

\- 파일의 metadata 중에서 파일명에 할당되는 byte 수는 정해져 있어, 파일명을 이보다 길게 설정하는 것은 원칙적으로 불가능하다. 그러나 이보다 긴 파일명을 사용해야 하는 경우는 얼마든지 있기 마련이므로, 현재 대부분의 OS는 긴 파일명을 지원한다. 파일명이 원래 파일명에 할당되는 byte 수를 넘어설 경우 그 파일이 속한 디렉토리 파일의 끝부분에 그 이후의 파일명이 저장된다. (그리고 원래 파일명이 저장되는 곳의 끝부분에는 그 이후 이어지는 파일명의 위치 정보가 저장된다.)



### 5. VFS, NFS

#### 1) VFS(virtual file system)

\- 한 컴퓨터 시스템에서 서로 다른 종류의 file system을 다뤄야 하는 경우가 있을 수 있다. 이때 만약 각 file system마다 다른 system call을 사용한다면 사용자 입장에서 file system 제어에 어려움이 있을 수밖에 없다. 한 종류의 system call로 여러 file system을 통제할 수 있게 하는 OS의 계층을 두어 이 문제를 해결할 수 있다. 이러한 OS 계층을 VFS라 한다.


#### 2) NFS(network file system)

\- 로컬의 system call을 통해 네트워크 상의 다른 컴퓨터 시스템의 file system을 다뤄야 하는 경우가 있을 수 있다. 이때, NFS 모듈과 RPC/XDR 프로토콜을 이용하면 로컬의 system call을 네트워크 상의 다른 컴퓨터 시스템의 file system 전달할 수 있다. 


