# 오늘 학습 정리 - Vector & List 활용 및 반복자

## 1. 입력과 벡터 저장 원리
- 사용자 입력을 for문으로 하나씩 받고 `push_back`으로 벡터에 저장.
- `std::cin`은 띄어쓰기(space) 또는 엔터(\n)로 구분하여 값을 읽음.
- for문이 반복될 때마다 한 값씩 `int` 변수에 저장한 후 벡터에 추가.
- 입력된 값들은 순차적으로 처리되며, 벡터의 `push_back`을 통해 끝에 삽입됨.

```cpp
std::vector<int> score;
for(int i=0; i<stud; ++i) {
    int val;
    std::cin >> val;
    score.push_back(val);
}
```

---

## 2. 반복자(Iterator) 이해
- 벡터, 리스트 순회 시 반복자 사용:
  - 벡터: `auto it = v.begin(); it != v.end(); ++it`
  - 리스트: `auto it = l.begin(); it != l.end(); ++it`
- 증감 연산자(`++it`)를 통해 다음 원소로 이동 가능.
- 리스트는 인덱스 접근 불가하므로 `advance(it, n)`으로 이동.
- 벡터는 `it + n` 또는 `it[i]`로 특정 위치 접근 가능.

> 반복자는 본체 벡터/리스트의 위치를 직접 이동시키지만, 객체 자체를 옮기지 않음.

---

## 3. erase와 순회
- 조건에 따라 원소 삭제 시, `erase` 후 반환값으로 새로운 반복자 받음.
```cpp
for(auto it = v.begin(); it != v.end(); ) {
    if(*it < 60)
        it = v.erase(it); // 삭제 후 새로운 위치 반환
    else
        ++it;             // 조건 미충족 시 다음 원소로 이동
}
```
- `for(auto it : v)`처럼 값 복사본 사용 시 erase 불가.
- **핵심:** erase 후 반복자를 재설정하지 않으면 순회 중 데이터 접근 오류 발생.

---

## 4. 벡터와 리스트 부가 기능
### 4-1. insert
- 리스트/벡터 특정 위치에 값 삽입 가능.
```cpp
std::list<int> l;
l.insert(l.begin(), 50); // 첫 위치 삽입
```
- 조건부 삽입: 반복자 순회 중 if 조건 만족 시 insert.
- iterator 기반으로 위치 지정 가능.

### 4-2. assign
- 벡터/리스트에 range 또는 값으로 전체 교체 가능.
```cpp
v.assign(score.begin(), score.end());
```

### 4-3. sort
- `<algorithm>` 헤더 필요.
- 오름차순 정렬 기본:
```cpp
std::sort(v.begin(), v.end());
```
- 벡터에서만 직접 지원, 리스트는 `l.sort()` 사용.

### 4-4. unique
- 연속 중복 원소 제거.
- 반드시 정렬 후 사용해야 연속된 중복만 제거됨.
```cpp
auto it = std::unique(v.begin(), v.end());
v.erase(it, v.end());
```
- unique 자체는 원소 제거가 아닌, **중복을 뒤쪽으로 몰아서 새로운 끝 반복자 반환**.
- erase와 함께 사용해야 실제 벡터에서 삭제됨.

### 4-5. reverse
- 리스트/벡터 뒤집기 가능.
```cpp
l.reverse(); // 리스트 전용
```
- 벡터는 std::reverse(v.begin(), v.end()) 사용.

---

## 5. for-each vs iterator
| 용도 | 사용 예 |
|------|---------|
| 값 읽기 / 출력 | for(auto it : v) |
| 값 수정 | for(auto& it : v) |
| 구조 변경(erase/insert) | for(auto it = v.begin(); it != v.end(); /* 조건 내 증감 */) |

- for(auto it : v)는 **읽기 전용**이므로 삭제/삽입 불가.
- erase/insert는 항상 **iterator 기반 순회**가 안전.

---

## 6. 연습하며 주의할 점
1. 리스트는 인덱스 접근 불가, advance로 이동.
2. erase 후 iterator 재설정 필수.
3. insert 위치 지정 시 iterator 필요.
4. for(auto : container)는 출력/읽기 전용.
5. unique는 연속 중복 제거, 정렬 필요.
6. 정렬/정렬 후 unique/erase 패턴은 표준 라이브러리에서 중복 제거 시 기본 패턴.

---

## 7. 오늘 새롭게 배운 함수/개념
- `std::vector::erase`, `std::list::erase`
- `std::vector::insert`, `std::list::insert`
- `std::vector::assign`, `std::list::assign`
- `std::sort`, `std::unique`, `std::reverse`
- `std::advance(it, n)`
- for-each (`for(auto it : container)`), reference (`auto& it`) 차이 이해

