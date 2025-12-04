# C++ 문자열 처리 & 예외 처리 정리

## 1. 문자열 길이

-   `size()` / `length()` : 문자열 길이 반환 (동일한 기능)

``` cpp
string s = "Hello";
s.length(); // 5
```

------------------------------------------------------------------------

## 2. 찾기 (find)

-   `find("문자열")` : 처음 등장한 위치 반환. 못 찾으면 `npos`.

``` cpp
string s = "Name=Arthur;Level=27;";
size_t pos = s.find("Name="); // 0
```

------------------------------------------------------------------------

## 3. 부분 문자열 (substr)

-   `substr(start, length)`
-   start = 시작 인덱스
-   length = 가져올 문자 개수

``` cpp
string s = "Name=Arthur;";
size_t start = 5;       // 'A'
size_t end = 11;        // ';'
string name = s.substr(start, end - start); // "Arthur"
```

------------------------------------------------------------------------

## 4. 문자열 삽입 (insert)

-   `insert(pos, str)`\
    → pos 위치 "앞에" str이 들어감

``` cpp
string s = "HelloWorld";
s.insert(5, " "); // "Hello World"
```

------------------------------------------------------------------------

## 5. 문자열 삭제 (erase)

-   `erase(pos, count)`
-   pos = "지울 문자 위치"
-   count = "그 위치부터 몇 개 지울지"

``` cpp
string s = "Hello, C++ World";
s.erase(5, 2);   // ", " 제거 → "HelloC++ World"
```

------------------------------------------------------------------------

## 6. 문자열 치환 (replace)

-   `replace(pos, count, str)`
-   pos부터 count개를 str로 교체

``` cpp
string s = "Hello C++ World";
s.replace(6, 3, "Python"); // "Hello Python World"
```

------------------------------------------------------------------------

## 7. 문자열 이어붙이기

### 3가지 방식

-   `+`
-   `+=`
-   `.append()`

``` cpp
string s = "Hello";
s += " World";
s.append("!");
```

------------------------------------------------------------------------

## 8. 문자 접근

``` cpp
string s = "Hello";
char c = s[1]; // 'e'
```

------------------------------------------------------------------------

## 9. 양쪽 공백 제거 (Trim)

C++은 직접 만들어야 함.

``` cpp
string trim(const string& s) {
    size_t start = s.find_first_not_of("    
");
    size_t end = s.find_last_not_of("   
");
    return s.substr(start, end - start + 1);
}
```

------------------------------------------------------------------------

## 10. 대소문자 변환

``` cpp
char upper = std::toupper('a'); // 'A'
char lower = std::tolower('Z'); // 'z'
```

------------------------------------------------------------------------

# 예외 처리 정리

## try-catch 기본 구조

``` cpp
try {
    int n = stoi("123a"); // 예외 발생
}
catch (const std::exception& e) {
    cout << "Error: " << e.what();
}
```

### 왜 `const std::exception& e` 를 쓰는가?

1.  **복사 비용 줄이기 (참조 사용)**
2.  **원본 예외 객체 유지**
3.  **상속 구조에서 다형성 유지**
4.  **수정 불가 의미로 const 사용**

------------------------------------------------------------------------

# stoi() 예외

-   문자가 섞여 있으면 `invalid_argument`
-   숫자가 너무 크면 `out_of_range`

``` cpp
try {
    int n = stoi("99999999999999999999");
}
catch (...) {}
```

------------------------------------------------------------------------

# 전체 파싱 예제

``` cpp
string s = "Name=Arthur;Level=27;HP=150;";

// Name 파싱
size_t namePos = s.find("Name=");
size_t nameStart = namePos + 5;
size_t nameEnd = s.find(";", nameStart);
string name = s.substr(nameStart, nameEnd - nameStart);

// Level 파싱
size_t lvlPos = s.find("Level=");
size_t lvlStart = lvlPos + 6;
size_t lvlEnd = s.find(";", lvlStart);
int level = stoi(s.substr(lvlStart, lvlEnd - lvlStart));

// HP 파싱
size_t hpPos = s.find("HP=");
size_t hpStart = hpPos + 3;
size_t hpEnd = s.find(";", hpStart);
int hp = stoi(s.substr(hpStart, hpEnd - hpStart));
```

------------------------------------------------------------------------

# Summary

-   문자열 조작은 인덱스 기반이지만, 함수마다 pos 의미가 다름
-   insert → pos 앞에 넣기\
-   erase → pos부터 삭제\
-   replace → pos부터 교체\
-   substr → pos부터 length만큼\
-   find → 위치 검색\
-   stoi → 예외 발생 가능\
-   catch → `const std::exception&` 로 받는 것이 표준
