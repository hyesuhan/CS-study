## 그래프와 트리의 차이점이 무엇인가요?
> 그래프는 노드와 간선으로 이루어진 일반적인 연결 구조이고, 트리는 그중에서 사이클이 없고 계층 구조를 가지는 특수한 그래프입니다.
> 
> 그래프와 트리의 가장 큰 차이는 계층 구조와 사이클 존재 여부입니다.
> 
> 그래프는 노드와 간선으로 이루어진 일반적인 자료구조로, 노드 간 연결이 자유롭고 사이클이 존재할 수도 있습니다. 또한 두 노드 사이에 여러 경로가 존재할 수 있고, 루트 노드나 부모-자식 관계가 반드시 존재하지 않습니다.
> 
> 반면 트리는 그래프의 한 종류이지만, 하나의 루트 노드를 기준으로 부모-자식 관계를 가지는 계층적 구조입니다. 사이클이 존재하지 않으며, 두 노드 사이의 경로는 항상 하나만 존재합니다. 또한 노드가 n개라면 간선은 항상 n-1개입니다.
> 
> 즉, 그래프는 일반적인 연결 구조이고, 트리는 그중에서 연결되어 있고 사이클이 없으며 계층 구조를 가지는 특수한 그래프라고 볼 수 있습니다.

### 그래프와 트리
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2F1ISus%2FbtreYnzVOxO%2FAAAAAAAAAAAAAAAAAAAAALMnJ0T2GJR-y_LUjxXQphUoBYW1AnHtn2HDjOg6E0ue%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DFRtz5a5rJzvH5j2WdeXBZFOn3wQ%253D)

### 그래프
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbAlVNh%2FbtreKxkxoN3%2FAAAAAAAAAAAAAAAAAAAAAKz_DLti7ySTJKssxLEGxxh4g4-yfOJFzJftDWngX6mW%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DwpoT6F6j72Xsd1j1%252B2n4IVm0Qaw%253D)
그래프는 노드(하나의 점)와 노드 간을 연결하는 간선으로 구성된 자료 구조

#### 그래프 특징
- 순환 혹은 비순환 구조
- 방향이 있는 그래프와 없는 그래프 존재
- 루트 노드 개념 X (부모 자식 관계 개념 X)
- 2개 이상의 경로 가능
- 네트워크 모델

### 트리
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FctR6nR%2FbtreYck5Goo%2FAAAAAAAAAAAAAAAAAAAAAIvATh2uTg0Po4zFfYqes-VHIi_25AUO-WxajwrgojHw%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DWZzUiq4w%252BoBm1CTD5G5Tbv7YbGQ%253D)
노드와 노드간을 연결하는 간선으로 구성된 자료 구조

-> 두 개의 노드 사이 반드시 1개의 경로를 가지며 사이틀 존재하지 않는 방향 그래프

#### 트리 특징
- 부모 자식 관계 존재해 레벨 존재
- 방향성 존재 O, 사이클 존재 X


| 구분    | 그래프(Graph)               | 트리(Tree)                      |
| ----- | ------------------------ | ----------------------------- |
| 개념    | 노드와 간선으로 이루어진 일반적인 자료구조  | 그래프의 한 종류로, 계층 구조를 표현하는 자료구조  |
| 관계    | 노드 간 자유로운 연결 관계          | 부모-자식 관계가 존재                  |
| 루트 노드 | 루트 노드 개념이 없음             | 하나의 루트 노드가 존재                 |
| 사이클   | 사이클이 존재할 수 있음            | 사이클이 존재하지 않음                  |
| 방향성   | 방향 그래프, 무방향 그래프 모두 가능    | 보통 부모 → 자식 방향의 계층 구조로 이해      |
| 경로 개수 | 두 노드 사이에 여러 경로가 존재할 수 있음 | 두 노드 사이의 경로는 반드시 하나           |
| 연결성   | 모든 노드가 연결되어 있지 않을 수도 있음  | 모든 노드가 연결되어 있음                |
| 간선 수  | 정해진 규칙 없음                | 노드가 n개라면 간선은 항상 n-1개          |
| 계층 구조 | 계층 구조가 아님                | 계층 구조를 가짐                     |
| 사용 예시 | 네트워크, 지도, SNS 관계, 최단 경로  | 파일 시스템, 조직도, DOM 구조, 이진 탐색 트리 |
