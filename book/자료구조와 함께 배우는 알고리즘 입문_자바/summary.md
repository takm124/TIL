[TOC]



# 3.검색

> 03-1 검색 알고리즘



- 선형 검색 : 무작위로 늘어놓은 데이터 모임에서 검색 수행
- 이진 검색 : 일정한 규칙으로 늘어놓은 데이터 모임에서 아주 빠른 검색 수행
- 해시법  : 추가, 삭제가 자주 일어나는 데이터 모임에서 아주 빠른 검색 수행
  - 체인법 : 같은 해시 값의 데이터를 선형리스트로 연결
  - 오픈 주소 법 : 데이터를 위한 해시 값이 충돌할 때 재해시하는 방법



이번 챕터는 배열 검색에 대한 설명



> 03-2 선형 검색

배열에서 검색하는 방법 가운데 가장 기본적인 알고리즘

- 선형 검색 : 직선 모양으로 늘어선 배열에서 원하는 키 값을 갖는 요소를 만날 때까지 맨 앞부터 순서대로 검색하는 것

  - 선형 검색(linear search) 혹은 순차 검색(sequential search)

  - 검색이 종료 되는 조건 : 검색할 값을 발견하지 못하고 배열의 끝을 지나갈 경우

    ​										 검색할 값과 같은 요소를 발견한 경우

- 보초법 (sentinel method) : 배열의 끝에 찾고자 하는 값을 보초로 넣는 것





> 03-3 이진 검색

- 이진검색의 전제 조건 : 데이터 값이 이미 정렬되어 있다는 것 (오름차순 혹은 내림차순)



```java
// 이진검색 구현

class BinSearch {
    // 요솟수가 n인 배열 a에서 key와 같은 요소를 이진 검색
    static int binSearch (int[] a, int n, int key) {
        int pl = 0;          // 검색 범위의 첫 인덱스
        int pr = n - 1;      // 검색 범위의 끝 인덱스
        
        do {
            int pc = (pl + pr) / 2; // 중앙 요소 인덱스
            if (a[pc] == key) {
                return pc; //성공
            } else if (a[pc] < key) {
                pl = pc + 1; //검색 범위를 뒤쪽 절반으로 좁힘
            } else {
                pr = pc - 1; // 검색 범위를 앞쪽 절반으로 좁힘
            }
        } while (pl <= pr);
        
        return -1;
    }
}
```





- 복잡도(complexity) : 알고리즘의 성능을 객관적으로 평가하는 기준
  - 시간 복잡도 : 실행에 필요한 시간 평가
  - 공간 복잡도 : 메모리 영역과 파일 공간이 얼마나 필요한가 평가





# 4. 스택과 큐

> 04-1 스택



- 스택 : 데이터를 일시적으로 저장하기 위한 자료구조, Last-in, First-out
  - 스택에 데이터를 넣는건 push, 꺼내는 작업은 pop
  - Java 프로그램에서 메서드를 호출하고 실행할 떄 프로그램 내부에서 스택을 사용함



```java
// 스택 구현

class IntStack {
    int max;   // 스택 용량
    int ptr;   // 스택 포인터
    int[] stk; // 스택의 본체
    
    // 실행 시 예외 : 스택이 비어 있음
    public class EmptyIntStackException extends RuntimeException {
        public EmptyIntStackException() {}
    }
    
    // 실행 시 예외 : 스택이 가득 참
    public class OverflowIntStackException extends RuntimeException {
        public OverflowIntStackException(){}
    }
    
    // 생성자 (constructor)
    public IntStack(int capacity) {
        max = capacity;
        ptr = 0;
        try {
            stk = new int[max];        // 스택 본체용 배열 생성
        } catch (OutOfMemoryError e) { // 생성할 수 없음
            max = 0;
        }
    }
    
    // push method
    public int push(int x) throws OverflowIntStackException {
        if (ptr >= max) {
            throw new OverflowIntStackException();
        }
        return stk[ptr++] = x;
    }
    
    // pop method
    public int pop() throws EmptyIntStackException {
        if (ptr <= 0) {
            throw new EmptyIntStackException();
        }
        return stk[--ptr];
    }
    
    // peek method : 스택의 꼭대기에 있는 데이터 확인
    public int peek() throws EmptyIntStackException {
        if (ptr <= 0) {
            throw new EmptyIntStackException();
        }
        return stk[ptr - 1];
    }
}
```



> 04-2 큐



- 큐 (Queue) : 스택과 비슷하지만 선입선출의 자료구조 (First in First out)
  - 데이터에 넣는 작업을 enqueue, 빼는 작업을 dequeue
  - 데이터를 꺼내는 쪽은 front, 넣는 쪽은 rear



- 단순 배열로 큐를 구현하게되는 dequeue 과정에서 데이터를 모두 앞으로 옮겨야 하는 과정이 필요함
  - 배열 요소를 앞쪽으로 옮기지 않는 큐를 구현하기 위해 링 버퍼(ring buffer) 자료구조 사용
  - ring buffer : 처음과 끝이 연결되어있는 자료구조, front와 rear로 처음과 끝 구분



```java
// int형 큐

public class IntQueue {
    private int max;   //큐의 용량
    private int front; //첫 번째 요소 커서
    private int rear;  //마지막 요소 커서
    private int num;   //현재 데이터 수
    private int[] que; // 큐 본체
    
    // 실행 시 예외 : 큐 비어 있음
    public class EmptyIntStackException extends RuntimeException {
        public EmptyIntStackException() {}
    }
    
    // 실행 시 예외 : 큐 가득 참
    public class OverflowIntStackException extends RuntimeException {
        public OverflowIntStackException(){}
    }
    
    // 생성자 (constructor)
    public IntQueue(int capacity) {
        num = front = rear = 0;
        max = capacity;
        try {
            que = new int[max];
        } catch (OutOfMemoryError e) {
            max = 0;
        }
    }
    
    // enqueue
    public int enque(int x) throws OverflowIntStackException {
        if (num >= max) {
            throw new OverflowIntStackException(); // 큐가 가득 찬 경우
        }
        
        que[rear++] = x;
        num++;
        if (rear == max) rear = 0;
        
        return x;

    }
    
    // dequeue
    public int deque() throws EmptyIntStackException {
        if (num <= 0) {
            throw new EmptyIntStackException();
        }
        
        int x = que[front++];
        num--;
        
        if (front = max) front = 0;
        return x;
    }
}
```



# 5. 재귀 알고리즘

> 05-1 재귀의 기본

- 재귀 : 어떤 사건이 자기 자신을 포함하고 다시 자기 자신을 사용하여 정의될 때 재귀적이라고 함



```java
// 팩토리얼

class Factorial {
    static int factorial (int n) {
        if (n > 0) {
            return n * factorial(n-1);
        } else {
            return 1;
        }
    }
}
```



