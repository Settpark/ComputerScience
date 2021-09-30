## 컴퓨터 시스템 구조

- 컴퓨터는 CPU + Memory와 I/O Device들로 이루어짐.

### CPU

- CPU가 반복하는 작업: memory에서 기계어 읽고 실행하고, interrupt line 확인하고를 반복함.
    - registers: memory보다 더 빠르고, 작은 메모리 장치
    - mode bit: 현재 실행되고 있는 프로그램이 운영체제인지 사용자 프로그램인지 구분해주는 역할
    - interrupt line: 다른 프로그램을 실행하고 있을 때, 입출력 하드웨어 혹은 예외 상황이 발생하여 처리가 필요한 경우 이를 CPU에게 알려주는 것을 interrupt라고 하는 데 이 interrupt를 수신하는 곳이다.
- mode bit
    - 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치
    - 사용자의 프로그램을 실행할 때는 1(=사용자 모드), 운영체제가 실행중일 때는 0(= 커널모드, 시스템 모드)
    - interrupt나 exception 발생 시, 하드웨어가 mode bit을 0으로 바꿈.
    - 사용자 프로그램에게 CPU를 넘기기 전에 mode bit을 1로 셋팅
    - 0일때는 I/O device에도 접근 가능, 1일때는 제한된 instruction만 접근 가능.
- CPU는 I/O 요청에 대한 처리가 필요한 경우 직접 접근하지 않고 device controller를 통해서 함.

### Memory

- CPU의 작업공간
- 실행되고 있는 사용자 프로그램은 직접 I/O Device에게 작업을 요청할 수 없다. 
오직 운영체제를 통해서만 요청 가능하다. (보안상의 이유로)
- 메모리에서 실행되고 있는 운영체제는 CPU에 interrupt가 들어오면 cpu제어권을 가져온다.
- memory controller
    - DMA controller와 cpu가 동일한 작업을 하지 않도록 중재하는 역할을 함.

### IO Device

- 공통사항
    - disk, keyboard, printer, monitor 등을 말함.
    - I/O device에는 각각에 작은 cpu들이 붙어있는데 이를 device controller라고 함.
        - I/O Device controller
            - I/O 장치 유형을 관리하는 일종의 작은 CPU
            - 제어 정보를 위해 control register, status register를 가짐
            - local buffer를 가짐 (일종의 data register)
            - memory controller도 deivce이기 때문에 같은 개념임
        - I/O는 실제 디바이스와 로컬 버퍼 사이에서 일어남
        - Device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림.
    - device 컨트롤러도 작은 memory공간이 필요한데, 그것을 local buffer라고 함.
- disk
    - 보조기억장치라고 불리고 Input/Output 모두 시행함.
    

### IO의 수행

- 모든 입출력 명령은 특권 명령
- 사용자 프로그램은 어떻게 IO하는가?
    - system call
        - 사용자 프로그램은 운영체제에게 IO 요청
    - trap을 이용하여 인터럽트 벡터의 특정 위치로 이동
    - 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
    - 올바른 IO요청인지 확인 후 IO 수행
    - IO 완료 시 제어권을 시스템콜 다음 명령으로 옮김

### Timer

- 한 프로그램이 CPU를 독점 하는 것을 막는 역할을 한다.
- 예를 들어, timer가 경과하면 CPU의 제어권이 사용자 프로그램으로 부터 운영체제로 넘어가도록 되어있다. (interrupt 발생)
- 타이머는 매 클럭 마다 1씩 감소
- 그럼 운영체제는 다음 프로그램에게 timer의 값 셋팅 후, 다음 프로그램으로 cpu제어권을 주게 된다.
- time sharing을 구현하기 위해 널리 이용 됨

### DMA (direct memory access) controller

- CPU가 데이터를 전송할 때 일일이 전부 신경쓰면 속도도 나오지 않고 CPU가 다른 일을 하지 못함.
- CPU는 source, destination 그리고 전송할 바이트 수만 DMA에게 전달하고 DMA가 이를 다 처리하고 나면 CPU에게 다 처리됐음을 알리는 interrupt를 딱 한번만 걸게 됨.

### Interrupt

- 인터럽트
    - 인터럽트 당한 시점의 레지스터와 program counter를 save 한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.
    - interrupt (넓은 의미)
        - interrupt(하드 웨어 인터럽트): 하드웨어가 발생시킨 인터럽트
        - Trap(소프트 웨어 인터럽트)
            - exception: 프로그램이 오류를 발생시킨 경우
            - system call: 프로그램이 커널 함수를 요청한 경우
    - 인터럽트 관련 용어
        - interrupt 벡터
            - 해당 인터럽트의 처리 루틴 주소를 가지고 있음 (정의되어 있는 함수의 주소)
        - 인터럽트 처리 루틴 (= Interrupt Service Routine, Interrupt handler)
            - 해당 인터럽트를 처리하는 커널 함수 (운영체제 내부 코드에 다 정의되어 있음)

### 시스템 콜

- 사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것