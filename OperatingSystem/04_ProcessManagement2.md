# Process Management 2

## 프로세스와 관련한 시스템 콜

### fork() create a child (copy)

- A process is created by the fork() system call.
    - creates a new address space that is a duplicate of the caller

### exec() overlay new image

- A process can execute a different program by the exec() system call.
    - replaces the memory image of the caller with a new program.
- exec() 다음 줄은 실행하지 않음.
    
    ```swift
    int main() {
    	prinf("1");
    	execlp("echo", "echo", "hello", "3", char(*) 0);
    	prinf("2"); //실행되지 않음
    }
    ```
    

### wait() sleep until child is done

- wait() system call
    - 프로세스 A가 wait() 시스템 콜을 호출하면
        - 커널은 child가 종료될 때까지 프로세스 A를 sleep시킨다. (block 상태)
        - Child process가 종료되면 커널은 프로세스 A를 깨운다. (ready 상태)  리눅스의 CLI

### exit() frees all the resources, notify parent

- exit() system call
    - 프로세스의 종료
        - 자발적 종료
            - 마지막 statement 수행 후 exit() 시스템 콜을 통해
            - 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어줌
        - 비자발적 종료
            - 부모 프로세스가 자식 프로세스를 강제 종료시킴
                - 자식 프로세스가 한계치를 넘어서는 자원 요청
                - 자식에게 할당된 테스크가 더 이상 필요하지 않음
            - 키보드로 kill, break 등을 친 경우
            - 부모가 종료하는 경우
                - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료 됨

## 프로세스 간 협력

- 독립적 프로세스
    - 프로세스는 각자의 주소공간을 가지고 수행되므로 원칙적으로 하나의 프로세는 다른 프로세스의 수행에 영향을 미치지 못함.
- 협력 프로세스
    - 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음.
- 프로세스 간 협력 메커니즘 (IPC: Interprocess Communication)
    - 메시지를 전달하는 방법
        - message passing: **커널을 통해 메시지 전달**
            - message system
                - 프로세스 사이에 공유 변수(shared variable)를 일체 사용하지 않고 통신하는 시스템
            - Direct Communication
                - 통신하는 프로세스의 이름을 명시적으로 표시
                    - Process P → Process Q
                    - Send(Q, message), Receive(P, message)
            - Indirect Communication
                - mailbox (또는 port)를 통해 메시지를 간접 전달
                - Process P → Mailbox M → Process Q
                - Send(M, message) → Receive(M, message)
    - 주소 공간을 공유하는 방법
        - shared memory: 서로 다른 프로세스 간에도 일부 주소 공간을 공유하는 shared memory 메커니즘이 있음
        - thread: thread는 사실상 하나의 프로세스이므로 프로세스간 협력으로 보기는 어렵지만, 동일한 프로세스를 구성하는 thread들 간에는 주소공간을 공유하므로 협력이 가능

# CPU Scheduling

## CPU and I/O Bursts in Program Execution

### CPU Burst

- 프로세스 수행 중에 연속적으로 CPU를 사용하는 단절된 구간

### I/O Burst

- 프로그램 수행 중에 I/O작업이 끝날때까지 block되는 구간

**프로세스는 CPU Burst와 I/O Burst를 반복적으로 번갈아가면서 수행한다.**

### I/O Bound job이란?

- 입출력이 많은 작업으로 cpu를 상대적으로 조금 사용하는 대신 시간이 많이 걸립니다.
- many short cpu bursts.

### CPU Bound job이란?

- 상대적으로 CPU를 많이 소비하는 작업을 일컫습니다.
- few very long cpu bursts.

여러 종류의 job(=process)이 섞여 있기 때문에 CPU 스케줄링이 필요하다.

- interactive job에게 적절한 response 제공 요망
- CPU와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용

### CPU Scheduler (운영체제안의 코드)

- Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.

### Dispatcher (운영체제안의 코드)

- CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다.
- 이 과정을 Context Switch(문맥 교환) 이라고 한다.

### CPU scheduling이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다.

1. Running → Blocked(예: I/O 요청하는 시스템 콜)
2. Running → Ready(예: 할당시간 만료로 timer interrupt)
3. Blocked → Ready(예: I/O 완료 후 인터럽트) priority에 의한 스케줄링
4. Terminate

1,4에서의 스케줄링은 nonpreemptive (=강제로 빼앗기지 않고 자진 반납)

All other scheduling is preemptive(=강제로 빼앗음)