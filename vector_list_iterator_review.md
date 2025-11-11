# C++ STL Vector, List, Iterator 복습 정리

## 1. Vector 기초

### 선언 및 초기화
```cpp
std::vector<int> vec; // 빈 벡터
std::vector<int> vec2(5); // 0으로 초기화된 크기 5 벡터
std::vector<int> vec3(5, 10); // 10으로 초기화된 크기 5 벡터
```

### 학생 수 입력 후 점수 저장
```cpp
int stud;
std::cout << "학생 수 입력: ";
std::cin >> stud;

std::vector<int> score;
for (int i = 0; i < stud; ++i) {
    int soc;
    std::cin >> soc;
    score.push_back(soc); // push_back 사용: 동적 크기 관리, 맨 뒤에 추가
}
```
- 벡터는 **동적 크기**를 가지므로, 초기 학생 수를 모르더라도 push_back으로 추가 가능
- `score[i] = ...` 방식은 크기가 이미 정해져 있어야 가능 (즉, `vec.resize(stud)` 없이는 접근 불가)

### erase 사용 시 주의
```cpp
for (auto it = score.begin(); it != score.end();) {
    if (*it < 60)
        it = score.erase(it); // erase 반환값으로 it 갱신 필수
    else
        ++it; // 지우지 않았으면 다음 원소로 이동
}
```
- vector는 연속 메모리이므로 erase 후 iterator 무효화 발생
- erase 후 ++it 절대 금지

### 평균 이상 점수 출력 예시
```cpp
double sum = 0;
for (auto it = score.begin(); it != score.end(); ++it)
    sum += *it;
double avg = sum / score.size();

std::list<int> hilev;
for (auto it = score.begin(); it != score.end(); ++it) {
    if (*it >= avg)
        hilev.push_back(*it);
}
```
- auto 키워드: 반복자의 타입을 컴파일러가 자동 추론

## 2. List 기초

### 선언 및 초기화
```cpp
std::list<int> lst;
```
- 연결 리스트 구조
- 삽입, 삭제가 vector보다 빈번할 때 유리

### 삽입
```cpp
lst.push_back(value); // 뒤에 삽입
lst.push_front(value); // 앞에 삽입
```

### 삭제
```cpp
lst.pop_back(); // 마지막 원소 삭제
lst.pop_front(); // 첫 번째 원소 삭제
```
- vector와 달리 iterator 무효화 문제 덜 발생

### 순회
```cpp
for (auto it = lst.begin(); it != lst.end(); ++it)
    std::cout << *it << " ";
```
- range-based for 가능: `for (int val : lst)`

## 3. Iterator 사용법

### 선언
```cpp
std::vector<int>::iterator vit;
std::list<int>::iterator lit;
```
- 반복자는 포인터처럼 사용 가능
- *it : 현재 값 접근
- ++it / --it : 이동

### 범위 기반 for
```cpp
for (auto val : score)
    std::cout << val << " ";
```
- 내부적으로 iterator를 이용해 순회
- 코드 간결, 안전

## 4. 주의할 점 / 햇갈린 부분

1. **학생 수 입력 후 벡터에 넣기**
   - vector는 동적 크기라 push_back 사용 필요
   - `score[i] = ...` 형태는 크기 지정(resize) 없이는 오류 발생
2. **erase() 후 iterator 갱신 필수**
   - vector에서 erase 후 ++it 하면 오류 발생
3. **vector는 연속 메모리**
   - push_back 시 크기 초과하면 재할당 발생, 기존 iterator 무효화 가능
4. **list는 연결 구조**
   - 삽입/삭제 시 iterator 안전
5. **auto 사용 장점**
   - 반복자 타입 직접 작성 불필요, 코드 간결
6. **push_back vs [] 인덱스**
   - vector에 원소를 순차적으로 입력할 때 push_back 사용
   - []는 이미 존재하는 원소 접근용, 크기 미정이면 사용 불가

## 5. 요약
- vector: 가변 배열, 연속 메모리, push_back, erase 주의, 동적 입력 가능
- list: 연결 리스트, 삽입/삭제 유리, iterator 안전
- iterator: 포인터처럼 접근, erase 후 갱신 필수
- auto: 반복자 타입 자동 추론
- 원초적 직접 구현 경험은 내부 구조 이해에 도움, 실무에서는 STL 사용 위주

