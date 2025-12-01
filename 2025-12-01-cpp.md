# C++ 스마트포인터 정리

## 1. 스마트포인터 종류와 특징

### 1.1 unique_ptr
- **소유권 단독**: 한 객체를 단 하나의 unique_ptr만 소유할 수 있음
- **복사 불가**, **이동(move) 가능**
- 사용법 예시:
```cpp
std::unique_ptr<Test> a = std::make_unique<Test>(10);
std::unique_ptr<Test> b;
b = std::move(a); // 소유권 이전
```
- reset() 사용:
```cpp
b.reset(); // b가 소유한 객체 삭제, b는 nullptr
b.reset(new Test(6)); // 새로운 객체 소유
```
- **게임 비유**: 한 명이 단독으로 소유하는 장비나 무기

### 1.2 shared_ptr
- **공유 소유**: 여러 포인터가 하나의 객체를 공유할 수 있음
- use_count()로 현재 참조 수 확인 가능
- 스코프 종료 시 자동 해제
- 사용법 예시:
```cpp
std::shared_ptr<Test> a = std::make_shared<Test>(20);
{
    std::shared_ptr<Test> b = a;
    std::cout << a.use_count(); // 2
}
std::cout << a.use_count(); // 1
```
- **게임 비유**: 팀원 모두가 공유하는 자원(예: 공용 버프, 길드 창고)

### 1.3 weak_ptr
- **소유권 없음**: 객체를 관찰만 가능, 직접 삭제 권한 없음
- shared_ptr가 소멸되면 expired() == true
- lock()을 통해 안전하게 shared_ptr 접근 가능
- 사용법 예시:
```cpp
std::weak_ptr<Test> w = a;
if(!w.expired())
    w.lock()->SomeFunction();
```
- **게임 비유**: 남이 가진 장비를 확인만 가능, 직접 소유는 못함

---

## 2. 주요 메서드와 기능

### 2.1 unique_ptr
- `std::move()`: 소유권 이전
- `release()`: 소유권 포기, 포인터 반환
- `reset()`: 소유 객체 삭제, 새 객체 소유 가능
- `get()`: 내부 포인터 주소 확인

### 2.2 shared_ptr
- `use_count()`: 현재 객체를 참조 중인 포인터 수
- `reset()`: 소유 객체 해제 또는 새 객체 소유

### 2.3 weak_ptr
- `expired()`: 객체 존재 여부 확인
- `lock()`: shared_ptr 얻어 안전하게 접근

---

## 3. 학습 시 유의점

1. **unique_ptr**: 단독 소유이므로 복사 불가, move 사용
2. **shared_ptr**: 공유 소유, 스코프 관리 중요, 순환 참조 주의
3. **weak_ptr**: 관찰자, shared_ptr 없으면 접근 불가
4. **소멸자**는 대부분 자동 호출되므로 특별한 리소스 해제 필요 없으면 구현 생략 가능
5. 스마트포인터는 동적 메모리 관리의 편리함 + 안전성을 제공, 실무에서는 필요할 때 활용

---

## 4. 실습 및 문제 메모

1. unique_ptr 소유권 이동 문제
2. shared_ptr use_count 확인 및 스코프 종료 후 변화 확인
3. weak_ptr expired()와 lock() 사용 실습
4. reset() 사용 시 nullptr 상태와 새 객체 할당

---

## 5. 핵심 요약

- **unique_ptr**: 단독 소유, move 가능
- **shared_ptr**: 공유 소유, use_count 관리
- **weak_ptr**: 관찰자, expired()로 안전 확인
- **스마트포인터 사용 이유**: 메모리 안전 + 자동 해제 + 소유권 명확화
- **학습 포인트**: 사용법과 동작 원리 이해, 실제 프로젝트에서는 필요 시 적용

