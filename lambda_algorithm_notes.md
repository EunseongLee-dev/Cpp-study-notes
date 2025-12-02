# C++ 람다(Lambda) & Algorithm 정리

오늘 학습한 내용을 정리하고, 문제풀이 과정에서 중요하게 배운 포인트들을 깃허브 업로드용으로 정리했습니다.

---

## 1. 람다(Lambda) 개념

- 익명 함수라고 부르는 함수 객체(function object)의 간결한 표현식
- 사용 목적:
  - 간단한 함수 코드를 즉석에서 작성
  - 코드 가독성 향상 (짧고 한 번 쓰이는 연산용)
- 기본 구조:

```cpp
[캡처](매개변수) -> 반환형 { 실행문; }
```

- 캡처(Capture) 기능:
  - 외부 변수를 람다 내부에서 사용 가능
  - `[&]` : 외부 변수를 **참조**로 캡처
  - `[=]` : 외부 변수를 **값**으로 캡처
  - `[&sum]` : 특정 변수만 참조로 캡처

- 람다는 짧고 단순한 연산에 적합하며, 길거나 복잡한 함수는 기존 함수 사용 권장

### 예시
```cpp
int sum = 0;
std::vector<int> v = {1, 2, 3, 4, 5};

std::for_each(v.begin(), v.end(), [&sum](int x) {
    if (x >= 3) sum += x;
});
std::cout << sum; // 3+4+5 = 12
```

---

## 2. Algorithm 함수들

### 2-1. `find_if`
- 특정 조건을 만족하는 첫 번째 요소를 찾음
- 반환: 조건을 만족하는 요소의 **Iterator**
- 벡터 순회: `v.begin()` ~ `v.end()`

```cpp
auto it = find_if(v.begin(), v.end(), [](int x){ return x % 2 == 0 && x > 6; });
if(it != v.end()) std::cout << *it;
```
- 첫 번째로 조건 만족 시점에서 탐색 종료

### 2-2. `copy_if`
- 조건을 만족하는 요소만 다른 컨테이너에 복사
- 사용법:
  - `std::back_inserter(target_vector)`와 함께 사용하면 자동 `push_back` 효과

```cpp
std::vector<int> source = {3, 10, 5, 22, 8};
std::vector<int> result;
std::copy_if(source.begin(), source.end(), std::back_inserter(result),
             [](int x){ return x % 2 == 0; });
```

### 2-3. `for_each`
- 컨테이너 전체를 순회하며 지정한 함수를 실행
- 람다와 함께 쓰면 조건부 누적, 출력 등 간단한 연산 가능

```cpp
int sum = 0;
std::for_each(v.begin(), v.end(), [&sum](int x){ if(x>3) sum += x; });
```

---

## 3. `back_inserter`
- 기존 벡터의 끝에 새 요소를 추가할 수 있는 **Iterator** 반환
- 사용 이유:
  - `copy_if` 등 알고리즘에 대상 컨테이너 제공 시, `push_back` 자동 처리
- `<iterator>` 헤더에 정의되어 있음
- VS에서는 `<algorithm>` 포함만으로도 사용 가능하지만, 표준상 안전하게 쓰려면 `<iterator>` 포함 필요

---

## 4. 람다와 Algorithm 흐름 이해
1. 컨테이너 순회 (`begin` ~ `end`)
2. 람다 조건 검사
3. 조건 만족 시 `back_inserter`를 통해 결과 벡터에 삽입

즉, 순서: **순회 → 람다 판단 → 삽입**

---

## 5. 문제풀이 중 배운 포인트

- 람다 활용 시, 외부 변수 캡처 방법
- `find_if` / `copy_if` / `for_each` 기본 사용법 및 차이점
- `back_inserter`와 벡터 연결 방법
- 조건 복합 연산 시 괄호 사용 중요성
- 알고리즘 함수들은 람다 뿐만 아니라 일반 함수나 함수 객체도 가능

---

## 6. 주의 사항

- 람다는 단순하고 짧은 연산에 적합
- 긴 로직은 기존 함수 정의 유지 권장
- 알고리즘 함수들(`find_if`, `copy_if`, `for_each`)은 `<algorithm>`에 속함
- `back_inserter`는 `<iterator>`에 정의
- 조건 연산자 우선순위 주의 (괄호 사용 권장)

---

## 7. 참고 예제
```cpp
std::vector<int> nums = {5, 12, 7, 20, 3, 8};
std::vector<int> filtered;
int sum = 0;

std::copy_if(nums.begin(), nums.end(), std::back_inserter(filtered),
             [](int x){ return x > 10; });

std::for_each(nums.begin(), nums.end(), [&filtered, &sum](int x){
    if(x>10){ filtered.emplace_back(x); sum += x; }
});
```

