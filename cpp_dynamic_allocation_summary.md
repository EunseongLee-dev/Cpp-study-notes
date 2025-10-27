# C++ 동적할당(Dynamic Allocation) 정리

## 1. 개념
- 동적할당: 런타임(프로그램 실행 중)에 메모리를 유동적으로 할당받는 것
- 일반 변수는 스택(stack)에 생성, 동적할당은 힙(heap)에 생성
- 필요 없으면 해제해야 메모리 누수 방지

## 2. C 스타일 vs C++ 스타일

### C 스타일
```cpp
int* a = (int*)malloc(10); // 10바이트 할당
free(a);                   // 해제
```
- malloc: 바이트 단위 할당
- 반환형은 void*, 따라서 강제 캐스팅 필요
- free: 힙 메모리 해제

### C++ 스타일
```cpp
int* a = new int;       // int형 1개 동적할당
delete a;               // 해제

int* arr = new int[10]; // int형 배열 동적할당
delete[] arr;           // 배열 해제
```
- 자료형 선언만으로 크기 결정 (타입에 맞는 바이트 자동 계산)
- 강제 캐스팅 필요 없음
- delete / delete[] 사용

## 3. 포인터와 구조체 활용 예시
```cpp
struct Player {
    char name[20];
    int hrt;
    int atk;
};

int main() {
    int num = 3;
    Player* plays = new Player[num];

    for (int i = 0; i < num; ++i) {
        std::cin >> plays[i].name >> plays[i].hrt >> plays[i].atk;
    }

    for (int i = 0; i < num; ++i) {
        std::cout << plays[i].name << " HP: " << plays[i].hrt << " ATK: " << plays[i].atk << '\n';
    }

    delete[] plays;
    plays = nullptr;
    return 0;
}
```
- `new`로 구조체 배열 생성
- `delete[]`로 해제
- 해제 후 포인터는 `nullptr`로 초기화 권장

## 4. 주의점
1. `malloc` vs `new` 차이
   - `malloc`: C 스타일, 바이트 단위, 캐스팅 필요
   - `new`: C++ 스타일, 자료형 단위, 자동 크기
2. delete / delete[] 구분
   - 배열이면 `delete[]`, 단일 변수면 `delete`
3. 전역 포인터 활용 시
   - 전역 포인터는 초기에는 nullptr로 선언하고, 필요할 때 동적할당
   - delete 후 nullptr 초기화 권장
   - 여러 함수에서 이어 쓰려면 STL 컨테이너 사용이 안전
4. wchar_t / char 혼용 문제
   - 한 함수 안에서 cout/wcout 혼용하면 출력/입력 꼬일 수 있음
   - 한 타입으로 통일하는 것이 안전함

