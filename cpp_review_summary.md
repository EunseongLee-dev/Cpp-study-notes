# C++ 복기 정리 (템플릿 / 클래스 / 상속 / 포인터 / 컨테이너)

## 1. ItemBox 템플릿 핵심

-   `vector` 기반 템플릿 클래스
-   add / remove / get / size / clear 구현

``` cpp
template<typename T>
class ItemBox {
private:
    std::vector<T> items;
public:
    void addItem(const T& item) { items.emplace_back(item); }

    bool removeItem(size_t index) {
        if (index < items.size()) {
            items.erase(items.begin() + index);
            return true;
        }
        return false;
    }

    T getItem(size_t index) const {
        if (index < items.size()) return items[index];
        std::cerr << "잘못된 인덱스 접근!
";
        exit(1);
    }

    size_t size() const { return items.size(); }
    void clear() { items.clear(); }
};
```

### 핵심 요약

-   `index < size()` → 벡터의 정상 인덱스 범위 확인
-   `begin()+index` == items\[index\] 위치
-   remove를 bool로 만드는 이유 → 삭제 성공 여부 반환
-   `std::cerr` → 에러 출력 전용 스트림
-   `exit(1)` → 즉시 프로그램 종료
-   `std::to_string()` → 숫자 → 문자열 변환

------------------------------------------------------------------------

## 2. 상속 기반 Item 구조

``` cpp
class Item {
protected:
    std::string name;
    double weight;
public:
    virtual ~Item() {} // 부모 포인터로 자식 delete할 때 필수

    std::string GetName() const { return name; }
    double GetWeight() const { return weight; }

    virtual std::string ToString() const {
        return "[Item] Name=" + name + ", Weight=" + std::to_string(weight);
    }

    virtual std::string GetType() const { return "Item"; }
};
```

### 왜 virtual 소멸자가 필요한가?

부모 포인터(`Item*`) 로 자식 객체(`Weapon`, `Armor`) 를 delete 할 때\
**자식 소멸자가 호출되도록 보장하기 위해서**.

------------------------------------------------------------------------

## 3. Weapon / Armor 오버라이드

``` cpp
class Weapon : public Item {
private:
    int damage;
public:
    Weapon(std::string n, double w, int d) : Item(n,w), damage(d) {}
    std::string ToString() const override {
        return "[Weapon] Name=" + name + ", Weight=" + std::to_string(weight)
            + ", Damage=" + std::to_string(damage);
    }
    std::string GetType() const override { return "Weapon"; }
};
```

-   오버라이딩으로 Item 버전 대신 Weapon 버전 실행됨

------------------------------------------------------------------------

## 4. Player 클래스 주요 개념

### 인벤토리 관리 (vector\<Item\*\>)

``` cpp
std::vector<Item*> inventory;
```

#### AddItem

-   단순 push

#### RemoveItem

``` cpp
delete inventory[i];     // 실제 객체 메모리 해제
inventory.erase(...);    // 포인터 제거
```

왜 delete 먼저? - **erase로 포인터를 제거하면 객체 주소를 잃어버림 →
delete 불가능**\
즉 메모리 누수 발생\
그래서 delete → erase 순서.

------------------------------------------------------------------------

## 5. list로 로그(최근 5개만 유지)

``` cpp
equipHistory.emplace_back(msg);
if (equipHistory.size() > 5)
    equipHistory.pop_front();
```

------------------------------------------------------------------------

## 6. map을 이용한 장비 슬롯

``` cpp
equipment["Weapon"] = nullptr;
equipment["Armor"] = nullptr;
```

------------------------------------------------------------------------

## 전반적인 키워드

-   템플릿 기본 작성법
-   vector로 저장/삭제
-   begin()+index, size(), clear()
-   클래스/상속/오버라이드
-   virtual 소멸자
-   new/delete 메모리 소유권
-   vector / list / map 컨테이너 목적 비교
