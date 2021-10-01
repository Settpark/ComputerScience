# System Structure & Program Execution 2

## 동기식 입출력과 비동기식 입출력

### 동기식 입출력 (synchronous I/O)

- I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
- 구현 방법 1
    - I/O가 끝날 때까지 CPU를 낭비시킴
    - 매시점 하나의 I/O만 일어날 수 있음
- 구현 방법 2
    - I/O가 완료될 때가지 해당 프로그램에서 CPU를 가져옴
    - I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움
    - 다른 프로그램에게 CPU를 줌

### 비동기식 입출력 (asynchronous I/O)

- I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감.

두 경우 모두 IO의 완료는 인터럽트로 알려줌

## DMA controller

- 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기  위해 사용
- CPU의 중재 없이 device controller가 device의 buffer storage의 내용을 메모리에 block 단위로 직접 전송
- 바이트 단위가 아니라 block 단위로 인터럽트를 발생 시킴

## 서로 다른 입출력 명령어

- I/O를 수행하는 sepcial instruction에 의해 (일반적인 I/O)
- memory mapped I/O에 의해 (I/O 장치도 메모리 주소의 연장)

## 저장 장치 계층 구조

- Primary (Executable) (휘발성) (CPU에서 직접 접근하여 처리 가능하다)
    1. Registers
    2. Cache Memory
    3. Main Memory
- Secondary (비휘발성)
    1. Magnetic Disk
    2. Optical Disk
    3. Magnetic Tape
    - 숫자가 작을 수록 빠르고 비쌈
    
- Caching: copying information into faster stoarge system

## 프로그램의 실행 (메모리 load)

1. 물리적 메모리(physical memory)에 바로 생성 되지 않고 virtual memory에 생성 됨
2. 프로그램을 실행하게 되면 address space라는 프로그램만의 독자적인 주소공간이 생기게 됨.
3. 이렇게 생긴 주소공간을 physical Memory에 올려서 프로그램을 실행하게 됨
4. 하지만 이렇게 생긴 주소공간 전체를 올려서 실행하지 않음 (물리적 메모리가 낭비 되기 때문) 필요한 부분만 올리게 됨.
5. 사용 되지 않을 때 메모리에서 해제 하게 됨, 하지만 경우에 따라서(종료되기 전까지 보관이 필요한 경우) swap area(=메모리의 연장 공간)라는 디스크 공간에 저장하기 도 함.
6. address translation: address space → physical memory로 변경하는 과정 (운영체제가 하지 않고 하드웨어가 함)

## 커널 주소 공간의 내용

- code: 시스템 콜 인터럽트 처리 코드, 자원 관리를 위한 코드, 편리한 서비스 제공을 위한 코드
- data: 운영체제가 사용하는 여러 자료구조들이 정의 되어 있음.
- stack: 사용자 프로그램들이 커널의 함수를 불러서 사용하기 위한 공간

## 사용자 프로그램이 사용하는 함수

- 함수(function)
    - 사용자 정의 함수
        - 자신의 프로그램에서 정의한 함수
    - 라이브러리 함수
        - 자신의 프로그램에서 정의하지 않고 가져다 쓴 함수
        - 자신의 프로그램의 실행 파일에 포함되어 있다.
    - 커널 함수
        - 운영체제 프로그램의 함수
        - 커널 함수의 호출 = 시스템 콜
        

## 프로그램 실행 단계

- program begin(user  mode) → system call (kernel mode) → return from kernel(user mode) → program ends(kernel mode)