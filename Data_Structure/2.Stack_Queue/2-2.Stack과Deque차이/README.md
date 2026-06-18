# Stack 클래스와 Deque 인터페이스의 차이

> - Java의 `Stack`은 인터페이스가 아니라 `Vector`를 상속한 클래스이고, `Deque`는 양 끝 연산을 정의한 표준 인터페이스
> - `Stack`은 `Vector` 상속 탓에 LIFO와 무관한 인덱스 접근 연산이 노출되고, 모든 메서드가 동기화되어 있으며, 순회 순서마저 직관과 어긋남
> - 공식 javadoc도 스택이 필요하면 `Stack` 대신 `Deque`(`ArrayDeque`)를 쓰라고 명시적으로 권장

## Java의 Stack은 클래스다

실제 `java.util.Stack`은 인터페이스가 아니라 클래스이고 레거시 동적 배열인 `Vector`를 상속한다.

```java
public class Stack<E> extends Vector<E> {
    // ...
}
```

`Vector`는 자바 1.0 시절의 컬렉션으로, 모든 메서드가 `synchronized`로 묶여 있는 스레드 안전 동적 배열이다.

- 문제 대부분이 이 상속에서 비롯됨
- 자바 설계자들도 나중에 `Stack`을 잘못된 설계로 인정하고 `Deque`로 대체하도록 안내

## 기존 Stack 클래스의 문제점

|     문제      |                   내용                    |
|:-----------:|:---------------------------------------:|
| 상속(IS-A) 오용 |   `Vector`를 상속해 LIFO와 무관한 연산이 모두 노출됨    |
|  불필요한 동기화   | 모든 메서드가 `synchronized` → 단일 스레드에서도 락 비용 |
|  순회 순서 혼란   |      `for`/iterator 순회 시 LIFO가 아님       |

### 1. 상속으로 인한 캡슐화 붕괴

스택은 `push`/`pop`/`peek`만 노출해야 하는데, `Vector`를 상속한 탓에 임의 위치 접근 연산까지 그대로 외부에 노출된다.

```java
void main() {
    Stack<Integer> stack = new Stack<>();
    stack.push(1);
    stack.push(2);
    stack.add(0, 99);   // LIFO 위반인데 컴파일
    stack.remove(1);    // 스택이 할 수 없어야 할 연산

}
```

### 2. 불필요한 동기화 비용

`Vector` 상속으로 모든 연산이 `synchronized`라, 멀티스레드가 아닌 상황에서도 락 비용이 늘 따라붙는다.

- 단일 스레드에서는 모니터 락 획득·해제가 순수 오버헤드
- 게다가 메서드 단위 동기화는 복합 연산(검사 후 pop 등)의 원자성을 보장하지도 못해, 실제 동시성 안전에도 충분치 않음
- 동시성이 필요하면 차라리 `ConcurrentLinkedDeque`처럼 목적에 맞게 설계된 컬렉션을 쓰는 편이 나음

### 3. 직관과 어긋나는 순회 순서

`Vector` 기반이라 인덱스 0이 가장 먼저 들어온 바닥 요소다.

```java
void main() {
    Stack<Integer> s = new Stack<>();
    s.push(1);
    s.push(2);
    s.push(3);
    for (int x : s)
        System.out.print(x);  // 1 2 3  (top인 3이 먼저일 줄 알았으나 반대)

}
```

`for`문이나 iterator로 순회하면 스택의 LIFO 순서가 아니라 삽입 순서(바닥 → top)로 나옴

## Deque 인터페이스

`java.util.Deque`(Java 6+)는 양 끝 삽입·삭제를 정의한 인터페이스로, 스택과 큐를 모두 표현할 수 있다.

| 스택 연산 |  Deque 대응 연산  |
|:-----:|:-------------:|
| push  |  `addFirst`   |
|  pop  | `removeFirst` |
| peek  |  `peekFirst`  |

`Vector`를 상속하지 않으므로 인덱스 접근 연산이 없어 LIFO 캡슐화가 지켜지고, 기본 구현체 `ArrayDeque`는 동기화가 없어 빠르다.

|    구분     |     Stack (class)      |    Deque (interface)    |
|:---------:|:----------------------:|:-----------------------:|
|    타입     |    `Vector` 상속 클래스     |          인터페이스          |
|    동기화    |  전 메서드 `synchronized`  |    없음 (필요 시 외부에서 감쌈)    |
| 임의 인덱스 접근 | `get(i)` 등 노출 (캡슐화 붕괴) |      없음 (LIFO 유지)       |
|   순회 순서   |    바닥 → top (직관 위배)    |   top → 바닥 (LIFO 일관)    |
|    성능     |    느림 (락 + Vector)     | 빠름 (`ArrayDeque` 원형 배열) |
|    권장     |       비권장 (레거시)        |           권장            |

### ArrayDeque가 빠른 이유

`ArrayDeque`는 원형 배열(circular array)에 head/tail 인덱스를 두고 동작한다.

- 양 끝 삽입·삭제를 배열 끝에서 인덱스만 회전시켜 O(1)로 처리하고, 데이터 이동이 없음
- 연속된 배열이라 캐시 지역성이 좋아, 노드가 흩어지는 `LinkedList`보다 순회·접근이 빠름

## 권장 사용법

스택이 필요하면 `ArrayDeque`를 스택으로 쓴다.

```java
void main() {
    Deque<Integer> stack = new ArrayDeque<>();
    stack.push(1);
    stack.push(2);
    stack.pop();   // 2
    stack.peek();  // 1
}
```

공식 문서도 새 코드에서는 `Stack` 대신 `Deque`를 쓰라고 직접 권장한다. 아래는 `Stack` javadoc에 그대로 적혀 있는 문장이다.

> A more complete and consistent set of LIFO stack operations is provided by the `Deque` interface  
> and its implementations, which should be used in preference to this class.  
> — `java.util.Stack` 클래스 javadoc

- 주의: `ArrayDeque`는 `null`을 저장할 수 없음 (`null`을 비어 있음 신호로 쓰기 때문에 넣으면 `NullPointerException`)
- 멀티스레드 환경에서 스택/덱이 필요하면 `ConcurrentLinkedDeque`나 `LinkedBlockingDeque` 같은 동시성 컬렉션을 사용하고, `Stack`의 동기화에 기대지 않음
