# C++ Vector, List, Iterator 정리

## 1. Vector (가변 배열)

### 개념
- 내부적으로 연속된 메모리 공간을 사용하여 데이터를 저장
- 일반 배열과 달리 동적 크기 변경 가능 (push_back, pop_back 등)
- 메모리가 부족하면 내부적으로 새 메모리를 할당하고 기존 데이터를 복사

### 주요 함수
- `push_back(value)` : 현재 마지막 위치에 새 데이터를 추가
- `pop_back()` : 마지막 요소 제거
- `size()` : 현재 데이터 개수 반환
- `capacity()` : 현재 확보된 최대 저장 공간 반환
- `erase(iterator)` : 특정 위치의 데이터를 제거 (뒤쪽 요소들은 한 칸씩 이동)
- `at(index)` : 인덱스로 접근 (범위 체크 포함)
- `[]` : 인덱스로 접근 (범위 체크 없음)
- `data()` : 내부 배열 포인터 반환

### 특징/유의점
- 중간 요소 삭제 시 뒤쪽 요소를 한 칸씩 이동해야 함 → 비효율적
- front() 함수 없음 → 맨 앞 요소 제거하려면 erase(begin()) 사용
- 가변 배열 구조이므로 동적할당 이해 필요

### 사용 예시
```cpp
std::vector<int> vec = {50, 70, 90};
vec.push_back(80);
vec.erase(vec.begin()); // 첫 번째 요소 제거
for (size_t i = 0; i < vec.size(); ++i)
    std::cout << vec[i] << " ";
```

---

## 2. List (연결 리스트)

### 개념
- 노드 단위로 데이터를 저장, 각 노드는 이전/다음 노드 주소를 가짐
- 배열처럼 연속 메모리 사용 X → 삽입/삭제가 빠름
- 양방향 연결리스트 (prev, next)로 구현 가능

### 주요 함수
- `push_back(value)` : 마지막 노드 뒤에 추가
- `push_front(value)` : 맨 앞에 새 노드 추가
- `pop_back()` : 마지막 노드 제거
- `pop_front()` : 맨 앞 노드 제거
- `size()` : 현재 데이터 개수 반환
- `begin()`, `end()` : 이터레이터 반환 (범위 기반 for 사용 가능)

### 특징/유의점
- 배열 인덱스 접근 불가 → `[]` 사용 불가
- 삽입/삭제 시 노드 연결만 수정하면 됨 → 효율적
- 데이터를 검색하려면 순회 필요 (0~n-1까지 노드 확인)

### 사용 예시
```cpp
std::list<int> lst = {50, 70, 90};
lst.push_back(80);
lst.pop_front();
for (int value : lst)
    std::cout << value << " ";
```

---

## 3. Iterator (반복자)

### 개념
- 컨테이너 내부 요소를 순회할 때 사용하는 포인터와 유사한 객체
- 포인터처럼 `*` 연산자를 통해 데이터 접근 가능
- vector, list 등 STL 컨테이너는 begin()/end()를 통해 iterator 제공

### 특징
- range-based for 문에서 자동 생성됨
- 반복문 내 변수는 iterator가 가리키는 값을 임시로 담는 변수일 뿐
- 일반 변수에 `:` 붙인다고 범위 기반 for 되는 건 아님 (배열/컨테이너 지원 필요)

### 사용 예시
```cpp
std::list<int>::iterator it = lst.begin();
int value = *it;
++it;
```

### range-based for 내부 동작
```cpp
for (int value : lst) {
    // 내부적으로 iterator 사용
    // for(auto it = lst.begin(); it != lst.end(); ++it) value = *it;
}
```

---

## 4. 정리 포인트

1. **Vector vs List**
   - Vector: 연속 메모리, 인덱스 접근 가능, 중간 삽입/삭제 비효율적
   - List: 노드 단위 연결, 삽입/삭제 효율적, 인덱스 접근 불가

2. **Iterator 역할**
   - 컨테이너 순회용 객체
   - 포인터처럼 * 연산 가능, range-based for 내부에서 자동 사용

3. **복습 체크포인트**
   - push_back, push_front, pop_back, pop_front 사용법과 차이 이해
   - erase와 pop_back/pop_front의 차이 이해
   - vector는 중간 삭제 시 요소 이동, list는 단순 연결 수정
   - range-based for는 iterator 내부 사용 원리 이해
   - dynamic allocation 이해: vector가 가변 배열로 동작하는 이유

