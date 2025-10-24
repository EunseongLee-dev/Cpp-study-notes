# C++ 문자열 함수 정리 및 과제 풀이

## 1. 오늘 학습 내용 요약

### 1.1 GetLength 함수
- 기능: wchar_t 문자열의 길이 반환
- 구조:
```cpp
unsigned int GetLength(const wchar_t* _pStr)
{
    int i = 0;
    while (true)
    {
        wchar_t c = *(_pStr + i);
        if (c == '\0') break;
        ++i;
    }
    return i;
}
```
- 핵심 포인트:
  - `_pStr[i]`와 `*(_pStr + i)`는 같은 의미.
  - 포인터를 이용해 문자열 끝(`\0`)까지 순회.

### 1.2 StrCat 함수 (문자열 이어붙이기)
- 기능: 두 문자열을 하나의 버퍼에 이어붙임
- 구조:
```cpp
void StrCat(wchar_t* _pstr, unsigned int _buffer, const wchar_t* _psrc)
{
    int idest = GetLength(_pstr);
    int istc = GetLength(_psrc);

    if (_buffer < idest + istc + 1) assert(nullptr);

    for (int i = 0; i < istc + 1; ++i)
    {
        _pstr[idest + i] = _psrc[i];
    }
}
```
- 핵심 포인트:
  - `_pstr[idest + i] = _psrc[i];`
    - `_pstr[idest]`는 이어붙일 시작 위치.
    - `_psrc[i]`는 복사할 문자열의 각 문자.
    - `i`는 `_psrc`의 인덱스를 의미.
  - 포인터를 사용하면 +=가 필요없고 =만으로 이어붙이기 가능.
  - 버퍼 크기 체크 필수.

### 1.3 wcscmp 직접 구현 (과제)
- 요구사항:
  1. 두 문자열이 같은지 확인
  2. 첫 번째 문자열이 사전순으로 앞서면 -1 반환
  3. 두 번째 문자열이 사전순으로 앞서면 1 반환
  4. 같으면 0 반환

- 구현 포인트:
```cpp
int wcstest(const wchar_t* _one, const wchar_t* _two)
{
    int lef = GetLength(_one);
    int rlt = GetLength(_two);
    int iLoop = lef < rlt ? lef : rlt;

    for (int i = 0; i < iLoop; ++i)
    {
        if (_one[i] < _two[i]) return -1;
        else if (_one[i] > _two[i]) return 1;
    }

    if (lef == rlt) return 0;
    else if (lef < rlt) return -1;
    else return 1;
}
```
- 핵심 포인트:
  - `_one[i]`와 `_two[i]`를 비교하면 내부적으로 아스키 값 비교가 됨.
  - 짧은 문자열만큼만 for문을 돌려서 인덱스 초과 방지.
  - 마지막에 길이 체크로 남은 글자 처리.
  - 삼항연산자 `? :`는 조건문 단축 형태, 참이면 왼쪽, 거짓이면 오른쪽 값 사용.

## 2. 오늘 학습 중 조심해야 될 부분

1. **포인터 vs 배열**
   - 포인터를 사용하면 `++ptr`로 다음 문자로 이동 가능.
   - 배열을 값으로 받으면 `++` 불가능, 주소가 아니므로.

2. **버퍼 크기 체크**
   - 문자열 이어붙일 때 `_buffer` 크기 확인 필수.

3. **while문 조건**
   - 문자열 끝(`\0`) 확인 조건 중요.
   - 두 문자열 모두 끝나지 않았을 때 반복하도록 설정.

4. **문자 비교**
   - 문자는 내부적으로 정수값으로 비교됨.
   - 직접 아스키 코드로 변환하지 않아도 `_one[i] < _two[i]`로 비교 가능.

5. **반환값 설계**
   - -1, 0, 1 반환 기준 명확히 이해 후 구현.
   - 직관과 실제 비교 조건이 일치하는지 확인.

## 3. 오늘 학습 피드백

- StrCat, GetLength 함수 구현 이해 잘함.
- wcscmp 직접 구현 시, 아스키 코드보다 포인터/인덱스 기반 비교로 단순화 가능.
- GPT 도움은 길잡이 수준으로 활용, 직접 로직을 설계해보는 것이 학습 효율 높음.
- 검색이 막힐 때 GPT 또는 자료 참조로 아이디어를 얻고, 직접 구현해보는 습관이 중요.

