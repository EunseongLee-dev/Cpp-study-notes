# C++ 후반부 정리 - unordered_map, 문자열, 예외처리 등

## 1. unordered_map
- **정의:** 키(key)와 값(value)을 쌍으로 저장하며, 해시 테이블 기반으로 빠른 검색 가능
- **특징:**
  - `map`과 달리 키가 자동 정렬되지 않음
  - 순회 시 순서는 불규칙적
  - 검색, 삽입, 삭제가 평균 O(1)
- **사용 예:**
```cpp
#include <unordered_map>
#include <string>
#include <iostream>

std::unordered_map<std::string, int> m;
m.emplace("Alice", 90);
int score = m["Alice"]; // 값 접근
```
- **find vs operator[]:**
  - `find(key)` : 키 존재 여부 확인 후 접근 가능
  - `operator[](key)` : 키가 없으면 새로 생성, 기본값 0
- **reserve(N)** : 버킷을 미리 할당해 성능 향상 가능
- **속도 개선:**
```cpp
std::ios::sync_with_stdio(false);
std::cin.tie(nullptr); // cin/cout 속도 향상
```

---

## 2. 문자열 관련 기능
- **getline(cin, str)** : 공백 포함 한 줄 입력
- **find(substr)** : 문자열 내 위치 검색
  - `npos` : 찾지 못했을 때 반환 값
- **substr(pos, len)** : 문자열 잘라서 반환
- **length() / size()** : 문자열 길이 반환
- **c_str()** : C 스타일 문자열(const char*) 반환

**사용 예:**
```cpp
std::string line;
std::getline(std::cin, line);
size_t pos = line.find("C++");
if(pos != std::string::npos){
    std::string after = line.substr(pos + 3);
    const char* ptr = after.c_str();
}
```

---

## 3. nullptr
- **정의:** 포인터가 아무 것도 가리키지 않음을 명시
- **예시:**
```cpp
const char* ptr = nullptr;
if(ptr) {
    std::cout << *ptr;
}
```
- 스마트포인터(TUniquePtr, TSharedPtr) 사용 시 직접 nullptr 사용 빈도 줄어듦

---

## 4. 예외 처리 (Exception Handling)
- **try / catch** : 예외 발생 가능 코드와 처리 코드 구분
- **throw** : 예외 발생 시 던짐
- **std::runtime_error** : 표준 예외 클래스

**사용 예:**
```cpp
try {
    if(pos == std::string::npos) {
        throw std::runtime_error("단어를 찾을 수 없음");
    }
} catch(const std::runtime_error& e) {
    std::cout << e.what() << std::endl;
}
```
- 흐름: try 안에서 문제가 발생하면 catch로 이동, 없으면 정상 수행

---

## 5. 오늘 문제풀이에서 핵심 포인트
1. `unordered_map` 사용법 및 find / operator[] 차이 이해
2. 문자열 검색 및 부분 문자열 추출, C 스타일 문자열 변환
3. nullptr 사용과 포인터 안전성 이해
4. 예외 처리 구조와 throw/catch 흐름 이해
5. 백인서터(back_inserter) + copy_if / for_each 등 알고리즘과 람다 활용

---

> **Tip:** 게임 개발에서는 unordered_map, string, 예외 처리 등은 기초 이해 정도로 충분하며, 실제 구현에서는 엔진 제공 API를 활용하는 경우가 많음.

