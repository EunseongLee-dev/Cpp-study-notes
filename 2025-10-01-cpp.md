# C++ 자료형 정리 (2025-10-01)

## 1. 자료형 개요
- 1byte = 8bit, 1024byte = 1KB, 1024KB = 1MB, 1024MB = 1GB
- 정수형: char(1), short(2), int(4), long(4), long long(8)
- 실수형: float(4), double(8)
- unsigned: 양수만 표현 가능

## 2. 정수형
### char
- 1byte, -128 ~ 127 혹은 0 ~ 255 (unsigned)
- 내부적으로 숫자지만 `cout`에서 출력 시 문자처럼 보이게 처리 가능
- 예제:
```cpp
char c = 'A';
std::cout << c << std::endl;       // 문자 A 출력
std::cout << (int)c << std::endl;  // 숫자 65 출력
std::cout << c + 3 << std::endl;   // 숫자 68 출력
std::cout << static_cast<char>(c + 3) << std::endl; // 문자 D 출력
```

### int, long, long long
- 정수형 변수
- 내부 표현은 숫자, 출력도 숫자만
- char와 달리 문자처럼 출력되지 않음

## 3. 실수형
- float: 4바이트, 소수점 정밀도 제한
- double: 8바이트, 더 세밀한 소수점까지 표현 가능
- 정수와 실수 혼합 연산 시 명시적 형변환 필요
```cpp
float f = 10.2415f + (float)20;
```

## 4. 출력 관련
- `#include <iostream>`: `cout`, `cin`, `endl` 등 기본 입출력 사용 시 반드시 포함
- `std::cout`, `std::endl` 처럼 네임스페이스 명시 권장
- `using namespace std;`는 학습/실습에서는 가능하지만 실무에서는 피하는 것이 좋음

## 5. char의 본질 정리
- char = 1바이트 정수형
- 문자처럼 보이는 이유: 출력 연산자가 char 타입을 문자로 처리
- 내부값 = 정수, 출력 시 문맥에 따라 문자로도 표시됨
- int, long 등은 문자 출력 규칙이 없어서 숫자로만 표시됨

---

### 🔑 학습 포인트
- 자료형의 크기와 범위를 이해하고, 출력 시 문맥에 따라 결과가 달라짐을 눈으로 확인
- char의 문자/숫자 특성을 이해하고 필요 시 타입 캐스팅 사용
- iostream과 std:: 사용 습관을 초반부터 익히기

