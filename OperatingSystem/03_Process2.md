- Thread
    
    > "A Thread (or lightweight process) is a basic unit of CPU utilization"
    > 
    
    > 쓰레드는 CPU 이용의 기본 단위이다.
    > 
    - Process 하나에 CPU 수행 단위만 여러개 두는 것을 Thread라고 함.
    - 공유할 수 있는 건 최대한 공유함 (code section, data section, OS resource)
    - **별도로 CPU 수행과 관련된 자원들만 별도로 가지고 있음 (stack space, PC, register)**
- 전통적인 개념의 heavyweight process는 하나의 thread를 가지고 있는 task로 볼 수 있다.

## Thread를 사용할 경우 장점

- 다중 스레드로 구성된 테스크 구조에서는 하나의 서버 스레드가 blocked (waiting) 상태인 동안에도 동일한 테스크 내의 다른 스레드가 실행(running)되어 더 빠른 처리를 할 수 있다.
- 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율(throughtput)과 성능 향상을 얻을 수 있다.
- 스레드를 사용하면 병렬성을 높일 수 있다.

## Thread의 이점

- Responsiveness
    - 멀티 쓰레드 웹 - 데이터가 작은 텍스트를 먼저 그려주고, 데이터가 큰 이미지를 나중에 그린다.
- Resource Sharing
    - 똑같은 일을 하는 프로그램이 여러개가 있을 때, 별도의 프로세스로 사용하는 것보다 하나의 프로세스로 두고 그 안에 CPU 수행 단위만 여러개 두게 되면 자원을 좀 더 효율적으로 사용할 수 있음.
- Economy
    - process를 새로 만드는 거보다 thread를 생성하는 오버헤드가 훨씬 작음
    - context switch는 굉장히 큰 오버헤드지만, thread의 cpu switch가 훨씬 비용이 적다.
- Utilization of MP Architectures
    - CPU가 여러개인 경우 각각의 쓰레드가 서로 다른 cpu에서 병렬적으로 일을 수행할 수 있음