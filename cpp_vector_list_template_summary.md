# C++ 벡터, 리스트, 템플릿 문제풀이 정리

## 1. 벡터 입력과 for문 순회

```cpp
std::vector<int> score;
for (int i = 0; i < stud; ++i) {
    int tin;
    std::cin >> tin;
    score.push_back(tin);
}
```

- 사용자가 입력한 값은 공백이나 엔터로 구분되어 연속적으로 입력됨.
- `std::cin`은 공백, 엔터를 기준으로 하나씩 읽어서 변수(`tin`)에 저장.
- `push_back()`으로 벡터에 순서대로 추가.
- 입력과 벡터 저장은 **for문 내부에서 한 번에 하나씩 처리**됨.

---

## 2. 중복 제거 (`unique` + `erase`)

```cpp
std::sort(score.begin(), score.end());
auto it = std::unique(score.begin(), score.end());
score.erase(it, score.end());
```

- `unique`는 **연속된 중복만 제거**. 따라서 정렬이 선행되어야 모든 중복 제거 가능.
- `unique`는 새로운 컨테이너를 만들지 않고 **중복을 뒤쪽으로 밀어낸 후 반복자를 반환**.
- 실제로 제거하려면 `erase`로 범위를 잘라야 함.
- **포인트**: `for(auto i : score)`에서는 읽기만 가능, 삭제나 수정 불가.

---

## 3. 반복문 순회와 삭제

```cpp
for (auto it = score.begin(); it != score.end();) {
    if (*it < 60) it = score.erase(it);
    else ++it;
}
```

- **이터레이터를 사용해야 안전하게 삭제 가능**.
- `for(auto i : score)`처럼 range-based for는 값 복사(`int`)만 받아서 삭제 불가.
- 삭제 후 iterator는 erase가 반환한 다음 위치를 사용해야 함.
- 출력용이나 값 수정용(range-based for with reference)은 가능: `for(auto &it : score) it += 5;`

---

## 4. 리스트에 조건 삽입

```cpp
std::list<int> top;
for (auto it = score.begin(); it != score.end(); ++it) {
    if (*it >= 90) top.push_back(*it);
}
top.reverse();
```

- `insert` 사용 시 `top.begin()`을 지정하면 항상 첫 위치에 삽입됨.
- 조건별 삽입에서 순서를 유지하려면 iterator 위치를 적절히 조절해야 함.
- `push_back`은 단순히 맨 뒤에 추가하므로 간단.
- 핵심: 출력 목적 외에는 range-based for 보다는 **이터레이터 순회**가 안전.

---

## 5. 템플릿 함수 작성 시 포인트

### 5-1. Container 템플릿
```cpp
template <typename Container>
void remove(Container& c);
```
- main에서 벡터나 리스트를 그대로 전달 가능.
- `&` 레퍼런스 사용으로 **실제 컨테이너를 함수 내에서 수정 가능**.

### 5-2. 여러 타입, 여러 컨테이너 처리
```cpp
template <typename ContainerIn, typename ContainerOut, typename T>
void exrta(const ContainerIn& c, ContainerOut& out, T limit);
```
- 한 함수에서 서로 다른 컨테이너 타입을 처리하려면 **템플릿 이름을 분리**.
- `ContainerIn` = 입력 컨테이너, `ContainerOut` = 출력 컨테이너.
- `T` = 값 타입(int, double 등).
- 이 구조가 아니면 한 매개변수의 타입이 다른 컨테이너와 충돌 시 컴파일 오류 발생.

### 5-3. 구조체 템플릿
```cpp
template <typename T>
struct mand { T data; };
```
- `T`는 구조체 멤버 `data`의 타입을 의미.
- `mand<int>` → data는 int, `mand<double>` → data는 double.
- 단순히 구조체 안에서 자료형을 다양하게 사용하고자 할 때 활용.
- 주의: `mand<int> a;`라고 선언 후 `std::vector<a>`는 불가. 정확히는 `std::vector<mand<int>>`여야 함.

---

## 6. 오늘 문제풀이에서 햇갈린 부분 정리

1. **unique + erase의 동작 원리**
   - unique는 연속된 중복만 제거
   - erase가 실제 컨테이너에서 요소를 삭제
2. **range-based for (`for(auto i : score)`) vs iterator**
   - 읽기/출력 용도: range-based for 사용 가능
   - 삭제/삽입/수정: 반드시 iterator 사용, 레퍼런스로 받는 것도 가능(`auto&`)
3. **템플릿 함수에서 매개변수 타입 문제**
   - 같은 템플릿 이름 사용 시 타입 고정됨 → 다른 타입 사용하려면 별도의 템플릿 이름 필요
   - ContainerIn/ContainerOut 분리 필요
4. **구조체 템플릿 사용**
   - `mand<T>`로 멤버 타입 유연하게 지정 가능
   - main에서 벡터/리스트 생성 시 반드시 정확한 템플릿 타입 지정 필요
5. **리스트 insert vs push_back**
   - insert(it 위치, 값) → 지정 위치에 삽입
   - push_back → 항상 뒤에 추가
   - 조건부 삽입 시 순서와 위치 주의

---

## 7. 오늘 배운 함수/문법

| 기능 | 예제 | 설명 |
|------|------|------|
| 벡터 push_back | `score.push_back(in);` | 입력값을 벡터 끝에 추가 |
| unique | `std::unique(score.begin(), score.end());` | 연속 중복 제거, 반복자 반환 |
| erase | `score.erase(it, score.end());` | 실제 요소 삭제 |
| sort | `std::sort(score.begin(), score.end());` | 오름차순 정렬 |
| list push_back | `top.push_back(*it);` | 리스트 끝에 요소 추가 |
| list insert | `top.insert(itlist, *it);` | 지정 위치에 요소 삽입 |
| range-based for | `for(auto i : score)` | 값 읽기/출력 용도 |
| range-based for ref | `for(auto &it : score)` | 값 수정 가능 |
| template struct | `template <typename T> struct mand { T data; };` | 자료형 유연한 구조체 생성 |
| template function | `template <typename Container> void remove(Container& c)` | 벡터/리스트 등 컨테이너에 대한 일반화 함수 |
| template function 다중 | `template <typename ContainerIn, typename ContainerOut, typename T>` | 서로 다른 컨테이너/자료형 처리 가능 |

