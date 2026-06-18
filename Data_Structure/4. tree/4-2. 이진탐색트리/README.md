## 이진 탐색 트리(BST) 의 평균 탐색 시간 복잡도는 어떻게 되고 최악의 경우 발생 가능한 성능 저하 문제는 무엇인가요?
> 이진 탐색 트리의 평균 탐색 시간 복잡도는 O(log n) 입니다.
> 
> 하지만 삽입 순서에 따라 트리가 한쪽으로 치우치면 높이가 n에 가까워져 최악의 경우 O(n) 이 됩니다
> 
> 이때는 연결 리스트처럼 순차 탐색하게 되어 성능이 크게 저하됩니다.
> 
> 또한 노드당 자식이 최대 2개라 대규모 데이터에서는 트리 깊이가 커지고, 디스크나 캐시 관점에서도 블록 활용 효율이 낮아 I/O와 캐시 미스가 증가할 수 있습니다.

### 이진 탐색 트리 탐색 
#### 검색
1. 찾고자하는 값과 현재 루트 노드 비교
2. 작으면 왼쪽 서브트리
3. 크다면 오른쪽 서브트리

=> 재귀적으로 반복 수행하여 타겟 데이터일 때까지 탐색

#### 트리가 균형 잡힌 경우
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fce84Q7%2FbtsBQlOT1sd%2FAAAAAAAAAAAAAAAAAAAAAFmZFWxdz037wtOIMUPRg3T9q_u3wNCOLndRnisr_Qqn%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3Dp3XIkSUxBIvbwmMtxyIEbZSutes%253D)
노드 개수를 n이라고 했을 때 시간 복잡도 **O(logn)**

#### 트리가 한쪽으로 치우쳐져 있을 경우
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FyZodk%2FbtsBUKApa4Z%2FAAAAAAAAAAAAAAAAAAAAAM9b9_af9wfo3WMd-JqZFghnpW2yArPco8EdVRlnjHjC%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DT9X4leMwfnNEOvTvmgl5%252B%252Fahye0%253D)
노드 개수를 n이라고 했을 때 시간 복잡도 **O(n)**

-> 선형 탐색과 다를 바가 없어짐

#### 성능 저하 문제
1. 불균형 문제 -> 선형 탐색과 동일한 탐색
2. 노드 활용 비효율성
   
    각 노드는 최대 2개의 자식 노드만 관리 -> 데이터가 많아질수록 트리 깊이 증가

    이는 탐색 경로의 증가로 이어져 검색, 삽입, 삭제 작업이 전반적으로 느려지게 됨
    
    ![](https://blog.kakaocdn.net/dna/dvLIgo/btsNBxh4DEC/AAAAAAAAAAAAAAAAAAAAAE_Ot0wU0x5M0iKZJU7GLKr8Q4J9autl80x3yFQOgFc7/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1782831599&allow_ip=&allow_referer=&signature=iEqYnIj2jDRarjXiE2m3L%2Fy7a7s%3D)

    또한, 대규모 데이터 처리하는 DB와 파일 시스템은 디스크 블록 단위로 데이터 관리

    디스크 블록에는 더 많은 키와 자식 노드 포이터 저장 구조 / 이진 탐색 트리는 노드당 두개 자식만 허용 (디스크 블록당 데이터 활용 효율 낮아 디스트 I/O 작업 증가)

    마지막으로, 이진 탐색 트리는 캐시에 로드되는 데이터 크기가 작아 메모리 활용도 떨어지며 캐시 미스가 자주 발생 가능

## 이를 해결하기 위한 자가 균형 이진 탐색 트리(Red-Black Tree)에 대해 간략히만 설명해주세요.
> 자가 균형 이진 탐색 트리는 일반 BST가 한쪽으로 치우쳐 최악의 경우 O(n)이 되는 문제를 해결하기 위한 자료구조입니다.
> 
> 삽입이나 삭제 이후 트리의 균형이 깨지면 회전이나 색상 변경 같은 재조정 작업을 통해 트리의 높이가 지나치게 커지지 않도록 유지합니다. 대표적으로 AVL Tree와 Red-Black Tree가 있으며, 이를 통해 탐색, 삽입, 삭제 연산을 최악의 경우에도 O(log n)으로 보장할 수 있습니다.
> 
> 다만 균형을 유지하기 위한 추가 연산이 필요하기 때문에 구현이 복잡하고, 삽입·삭제 시 약간의 오버헤드가 발생할 수 있습니다.

### 자가 균형 이진 탐색 트리
삽입, 삭제 등의 연산 후에도 항상 균형을 유지하는 이진 탐색 트리

=> 자가 균형 트리는 트리의 높이가 지나치게 커지는 것을 방지

#### 장점
1. 항상 O(log n) 성능 보장
2. 효율적 탐색

#### 단점
1. 복잡한 구현 및 디버깅
2. 추가적 메모리 공간 요구
3. 삽입/삭제 연산 성능 저하

### Red-Black Tree
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbMAlpy%2FbtrZxI6OBCh%2FAAAAAAAAAAAAAAAAAAAAAEAsHzNgO5D6bmPxQinex7RbOTgI5jvjkgB3Ve8fl6SK%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3Duv4Fx2pSUhazyymjt%252F%252F0xMa%252F2zg%253D)

#### 조건
- 모든 노드는 Red or Black
- 루트 노드는 Black
- 모든 단말노드 Black
- 노드가 Red면 자식은 Black
- 루트노드에서 모든 리프노드까지 경로에서 만나는 black 노드 수 동일

#### 삽입 연산
- 새로 삽입되는 노드 색은 Red
- Recoloring
  - 새로 삽입된 노드의 삼촌 노드가 Red인 경우
    - 새로 삽입된 노드의 부모 노드, 삼촌 노드를 black으로 하고 조부모 노드를 red로 함
    - 조부모 노드가 루트 도드가 아니라면 double red가 발생할 수 있음
    - 발생하면 다시 recoloring 또는 rotation

- Rotation
  - 새로 삽입된 노드의 삼촌 노드가 black이거나 null인 경우
    - 새로 삽입된 노드, 부모 노드, 조부모 노드를 오름차순으로 정렬
    - 중간에 위치한 노드를 부모 노드로 하고 나머지 두 노드를 자식 노드로
    - 부모 노드를 black으로 자식들을 red로

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbbuW6v%2FbtrZryEvZFv%2FAAAAAAAAAAAAAAAAAAAAAB0jbbans4rOy_CaJyAYC_9Xr8Lb0-D5juhgl3MihwiA%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DXttgBWV8z3rMr660YGzKsST9a6g%253D)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fcx1gkp%2FbtrZznBlGPZ%2FAAAAAAAAAAAAAAAAAAAAAEHIPSMOIOF24wAX5DNPjZGd-PUXAMW-85rCUPiQITfZ%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DSc36x8tbTa2fHz2VELfV456%252BsaY%253D)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2F21EVn%2FbtrZzahLVaZ%2FAAAAAAAAAAAAAAAAAAAAAJvrMiYHKUm9CSE1QHY2I6N6fFgONRAmYmm-WpvaRNaQ%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DCb5SvRy6XKjxnXwZo9w7DOkSxJY%253D)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FlMPZx%2FbtrZrVzkL7o%2FAAAAAAAAAAAAAAAAAAAAAEombYw64GiUfMbX16--6yqcdxXHgKV67jUfjKYGeQv-%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DG3p0l%252BQcu8mpL%252F7DeZWHl5JRdsQ%253D)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FVgaeV%2FbtrZqTbfi4N%2FAAAAAAAAAAAAAAAAAAAAAO06-t2Bdk5GBRgEglz_yDN3DtbQP9kec-bBSRUCJ2Hb%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DB3CBjjg7DU6gxKrTpqVfkmf2s2Q%253D)

#### 삭제 연산
- Red 노드는 그냥 삭제
- Black은  rotation 통해 삭제

#### 시간 복잡도
O(log n)

#### 장점
- 최악의 경우 일정 실행 시간 보장
- 실시간 처리와 같은 실행 시간 중요한 경우 유용

#### 단점
- 이해 어렵고 구현 복잡

### AVL Tree
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FblxsRD%2Fbtq21CW9Fw3%2FAAAAAAAAAAAAAAAAAAAAAIENqeJjUQgY2tFEmyAYUfAfueFivqIBepMxryz8W0rQ%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DLoiKIxgKkdv7gokVHE3NtXFSSJE%253D)
#### 조건
- 왼쪽, 오른쪽 서브 트리의 높이 차이 최대 1

#### 회전
AVL 트리는 삽입 삭제에서 rotation을 통해 트리 균형 맞춤

##### LL case
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbiiR2r%2FbtrJUY5baM4%2FAAAAAAAAAAAAAAAAAAAAABUgONrGBCC9Oc80eKzykuNlGkN6eS0Z4E-ubzcdR7JE%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3Dg2OMtnhVxrDdCp%252BG9wUN%252Fd4tGvQ%253D)
1. y 노드의 오른쪽 자식 노드를 z 노드로 변경
2. z 노드의 왼쪽 자식 노드를 y 노드 오른쪽 서브 트리(T2)로 변경

##### RR case
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FMgydF%2Fbtq2ZpcT9dF%2FAAAAAAAAAAAAAAAAAAAAADsAE3lEt4aq53oxva6dsUWlBNM3bFFzHho2vbXg7W_k%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DQL5Z3X8RM2hO1CWBuT57JE9ZwcE%253D)
1. y 노드의 왼쪽 자식 노드를 z노드로 변경
2. z노드 오른쪽 자식 노드를 y노드 왼쪽 서브트리(T2)로 변경

##### LR case
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbqqYJK%2FbtrTfOfWWBd%2FAAAAAAAAAAAAAAAAAAAAABGaEIXsZIoH4L19fRjP4mkq5XYjXVcewETSjV9tpIlw%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3DCLaq7WBNtRta0Pkd%252BlWBceinsp0%253D)

##### RL case
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbrTQV1%2Fbtq2TcMbXA3%2FAAAAAAAAAAAAAAAAAAAAAOy-lk2-CUCp6pjQ1U__jO6unEeS7Js4xltJ3McryFlR%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1782831599%26allow_ip%3D%26allow_referer%3D%26signature%3D%252BqpEkpImb1q5jFszfzaKnldM464%253D)

#### 시간 복잡도
O(log n)

#### 장점
- 모든 연산이 O(log n) 시간 복잡도 보장
- 완벽한 균형으로 최악의 경우에도 효율적

#### 단점
- 잦은 재조정으로 오버헤드 발생 가능
- 추가 공간 필요
