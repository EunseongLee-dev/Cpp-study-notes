# C++ Map & Tree 구조 정리

## 1. Tree(트리) 기초
- **트리**: 계층적 구조, 부모-자식 관계, 배열/리스트와 달리 시작 위치(root)에서 자식으로 이어지는 구조.
- **이진트리(Binary Tree)**: 각 노드가 최대 2개의 자식 노드를 가짐.
- **완전이진트리(Complete Binary Tree)**: 모든 레벨이 최대한 채워진 상태, 일반적으로 배열로 구현.
- **BST(Binary Search Tree)**: 정렬된 트리를 기반으로 탐색/삽입 시 O(log N) 효율.
  - 왼쪽 자식: 부모보다 작은 값
  - 오른쪽 자식: 부모보다 큰 값
- **Self-Balancing**: AVL, Red/Black 트리 등, 균형이 깨지면 탐색 효율 O(N)까지 떨어지므로 균형 유지 필요.

## 2. Tree 순회
- **전위(preorder)**: 부모 → 왼쪽 → 오른쪽
- **중위(inorder)**: 왼쪽 → 부모 → 오른쪽 (BST에서 일반적, 정렬된 순서 출력)
- **후위(postorder)**: 왼쪽 → 오른쪽 → 부모

## 3. Map, Pair, Iterator 기본
- **std::map<Key, Value>**: Key를 기준으로 정렬된 BST 기반 자료구조.
  - Key는 유일(unique), 중복 허용 X → 중복 필요 시 `std::multimap` 사용
- **Key**: 비교 기준으로 정렬됨. 정수, 문자열 등 모두 가능.
- **Value**: Key와 연관된 데이터.
- **Pair**: map에 삽입할 때 Key와 Value를 묶어 전달.
  - 예: `std::make_pair(L"홍길동", info)`
- **Iterator**:
  - `.begin()`: map의 가장 처음 위치
  - `.end()`: map의 마지막 다음 위치, 찾는 Key가 없을 때 반환
  - `find(key)`: Key 기준 탐색, 존재하지 않으면 `.end()` 반환
- **Insert vs Emplace**:
  - `insert`: pair 객체를 만들어서 삽입
  - `emplace`: 객체 생성과 삽입을 동시에 수행, make_pair 생략 가능

## 4. 문자열 처리 관련 권장사항
- 문자열 타입이 많아 혼동 가능 (char[], wchar_t, std::string, std::wstring 등)
- **권장**: 프로젝트 단위로 한 가지 타입 사용
  - 일반적인 상황: `std::string`
  - 유니코드/멀티바이트 필요: `std::wstring`
- 비교 기준: key로 들어간 문자열은 **ASCII/유니코드 값 기준**으로 비교, 길이가 아니라 문자 코드 순으로 정렬
- 문자열 Key 예시:
  - `alice < mike` → ASCII 코드 기준 비교, 'a' < 'm' → left
  - 동일 문자열 길이 차이 시: 길이가 짧은 쪽이 더 작게 판단
- **중복 Key**: map에서는 삽입 X, multimap에서는 허용

## 5. 구조체 활용 예시
```cpp
struct Student {
    std::wstring name;
    unsigned char age;
    unsigned char gender;
};

std::map<std::wstring, Student> students;
students.insert(std::make_pair(L"홍길동", Student{L"홍길동", 20, 1}));
```
- 검색 예시:
```cpp
auto it = students.find(L"홍길동");
if(it != students.end()) {
    auto& key = it->first;   // L"홍길동"
    auto& value = it->second; // Student 구조체
}
```
- **핵심 이해**:
  - Key는 정렬 기준, find로 Key를 비교하여 탐색
  - Iterator의 `first` → Key, `second` → Value 반환

## 6. 권장 코딩 팁
- 문자열 Key 사용 시 프로젝트 전체에서 동일 타입 통일
- insert vs emplace 구분: 간단히 객체 생성 후 바로 넣을 때 emplace 추천
- 구조체 내 char/unsigned char 사용은 메모리 최적화 목적, 일반 정수는 int로 통일해도 무방
- map 트리 구조상 루트/좌우 위치를 매번 신경 쓸 필요 없음, 내부적으로 BST/Balancing 처리됨
- 동명이인/중복 Key는 map → 허용 X, multimap → 허용

---
**정리**: 오늘은 Tree 이론 + BST 개념 + C++ Map/Iterator/Pair 개념 이해 중심. 코딩 예시는 실습 시 강의 따라가면서 구조 이해 후 직접 작성 추천.

