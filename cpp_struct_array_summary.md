# C++ 학습 정리 (2025-10-13)

## 1. 오늘 강의 진도 요약

### 1.1 구조체 (Struct)
- C 스타일 구조체
```cpp
typedef struct _tagMyst {
    int a;
    float f;
} MYST;
```
- 구조체 내부 멤버 접근: `.` 연산자 사용
```cpp
MYST t;
t.a = 10;
t.f = 10.23f;
```
- 중첩 구조체 가능
```cpp
typedef struct _tagBig {
    MYST k;
    int i;
    char c;
} BIG;
```

### 1.2 배열
- 기본 배열 선언 및 초기화
```cpp
int iArray[10] = {0}; // 0으로 초기화
```
- 배열은 연속적인 메모리 공간을 가짐.
- 배열 인덱스는 0부터 시작.

### 1.3 변수 종류
| 종류 | 위치 | 특징 |
|------|------|------|
| 지역변수 | 함수 내부 (Stack) | 함수 호출 시 생성, 종료 시 소멸 |
| 전역변수 | 함수 외부 (Data 영역) | 프로그램 시작 시 생성, 종료 시 해제 |
| 정적변수(static) | Data 영역 | 프로그램 종료 전까지 유지 |
| 외부변수(extern) | Data 영역 | 다른 파일에서 참조 가능 |

- 추가 영역: 힙 영역, 읽기 전용(ROM)

---

## 2. 오늘 응용문제 정리: 파티 멤버 관리

### 2.1 구조체 기반 멤버 정의
```cpp
struct Name {
    std::string name;
    int level = 0;
    int hp = 0;
    int maxHp = 0;
};
```

### 2.2 파티 전역 변수
```cpp
int PartyMember[5]; // 멤버 최대 5명
int partyCount = 0; // 현재 멤버 수
```

### 2.3 AddMember() 함수 핵심 포인트
- 전역 변수 `partyCount`를 통해 현재 인원 확인.
- 인원이 가득 찼으면 `return`으로 함수 종료.
- 입력 후 `partyCount++` 증가.
- 예시 구조:
```cpp
void AddMember() {
    if (partyCount >= 5) {
        std::cout << "파티 인원이 가득 찼습니다.\n";
        return;
    }
    Name a;
    std::cin >> a.name >> a.level >> a.maxHp;
    PartyMember[partyCount] = a;
    partyCount++;
}
```
- 핵심 개념: **조기 종료(early return) 패턴**.

### 2.4 PrintParty() 함수 포인트
- 반복문으로 배열 인덱스 접근
- 사람용 출력 번호는 `i + 1` 사용 (배열 인덱스 0부터 시작하므로)
```cpp
for (int i = 0; i < partyCount; i++) {
    std::cout << i+1 << ". 이름: " << PartyMember[i].name << "\n";
}
```

### 2.5 UpdateHP() 함수 포인트
- 사용자 입력 번호(num)는 1부터 시작
- 배열 인덱스와 맞추기 위해 `idx = num - 1`
- 구조체 멤버 접근
```cpp
int idx = num - 1;
PartyMember[idx].hp += changeValue;
```

### 2.6 오늘 문제에서 막혔던 이유
1. 구조체 배열과 인덱스 접근 방식이 낯설었음.
2. 함수, 조건문, 배열, 구조체를 한꺼번에 사용해야 하는 통합 문제였음.
3. 전역 변수 `partyCount`와 사용자 입력 번호를 배열 인덱스와 맞추는 연산(`num - 1`) 이해 필요.

---

## 3. 오늘 배운 핵심 개념
- **조기 종료(early return)**: 조건을 먼저 확인하고 함수 종료
- **배열 인덱스 0 시작 vs 출력 번호 1 시작**: `i + 1`
- **사용자 입력 번호 → 배열 인덱스**: `num - 1`
- **구조체 배열 접근**: `PartyMember[idx].멤버`
- **전역변수 + 배열 + 구조체**를 한꺼번에 활용하는 통합 사고

