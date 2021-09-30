# ComputerScience

## System Programming

### 컴퓨터 구조의 기본

시스템 프로그래밍이란?

- `컴퓨터 시스템을 동작시키는 프로그램`
- 컴퓨터 시스템 = 하드웨어 + 운영체제 이러한 프로그램 작성을 위한 라이브러리는 Windows 운영체제에 의해 제공 된다.

## 컴퓨터 시스템의 주요 구성 요소

1. CPU → 컴퓨터 구조 (Computer Architecture)
2. 캐쉬(Cache) → 컴퓨터 구조 (Computer Architecture)
3. 메인 메모리 → 운영체제 (Operating System)
4. 하드 디스크 → 운영체제 (Operating System)

## 컴퓨터 하드웨어의 구성

1. CPU(Central Processing Unit)
    
    > 중앙 처리 장치
    1. ALU(Arithmetic Logic Unit)
        1. 실제 연산을 담당하는 블록
        2. 기본적인 연산은 덧셈, 뺄셈, AND, OR와 같은 연산이다. (산술 연산과 논리 연산)
    2. 컨트롤 유닛
        1. 명령어를 해석하고, 그 해석된 결과에 따라서 적절한 신호를 다른 CPU블록에 보내는 역할을 한다.
        2. `10011010 0011010` 등으로 명령이 요청될 때 ALU는 논리 연산과 산술연산밖에 할줄 모른다. 이를 컨틀롤 유닛이 구분하고 ALU에게 신호를 보낸다.
    3. Register Set
        1. 예를 들어 ALU가 작업 중이어서 다음 작업을 처리하지 못할 때 CPU 내부에 임시적으로 데이터를 저장하기 위한 조그마한 메모리 공간이 필요하다.
        2. 이 메모리 공간이 **Register**이다. 
        3. 레지스터는 2진 데이터(Binary Data) 저장을 위한 저장장치이다.
    4. Bus Interface
        1. CPU, RAM, Hard disk, sound card등이 데이터를 주고 받기 위해서 어떠한 매개체가 있어야 하는 데 이것이 I/O 버스이다.
        2. I/O 버스의 통신 방식을 이해하고 있어야만, 데이터를 주고 받을 수 있는 데, **CPU 내에서는 I/O 버스의 통신방식을 이해하고 있는 그 무엇인가가 있어야만 한다.**
        3. 데이터 전송에 따른 프로토콜, 통신방식을 이해하고 있는 녀석이 바로 Bus Interface이다.
    - 번외: 클럭 신호(Clock Pulse)
        - 클럭 신호는 타이밍을 제공하기 위해서 필요한 것.
        - 가령 예를 들어 CPU 1.6Mhz 이면 1초에 1,600,000번 클럭을 발생시키고 이 클럭에 맞춰서 일을 한다.
        - 그런데 왜? 클럭신호에 맞춰서만 일을 할까? 그냥 되는대로 열심히 일하면 더 성능이 좋아질텐데 말이다.
        - 예를 들어) 연산 장치 - 버퍼 - 출력장치가 있을 때, 연산장치의 input으로 계속해서 값이 들어올 때, 연산 장치의 연산 속도와 출력장치의 데이터를 가져가는 속도가 다르다고 하면 문제가 발생한다.
        - 만약 출력장치가 더 빠르다면 출력장치는 이미 가져간 데이터를 한번 더 가져가고,
        - 연산 장치가 더 빠르다면, 버퍼를 덮어 쓰게 되어 연산결과의 일부가 출력되지 않는 문제점이 발생하게 될것이다.
2. Main Memory
    
    > 컴파일이 완료된 프로그램 코드가 올라가서 실행되는 영역 `프로그램 실행을 위해 존재하는 메모리`
    > 
3. 입 출력 버스(Input/Output Bus)