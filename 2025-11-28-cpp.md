# C++ 기초 1단계 리뷰 - 텍스트 RPG 전투 시스템 구현

## 1. 오늘 학습 목표
- 상속과 다형성을 활용한 부모/자식 클래스 구조 설계
- std::list를 이용한 리스트 관리
- 템플릿 클래스를 통한 범용 매니저 구조
- Unit 클래스 기반 RPG 전투 로직 작성


## 2. 구현 클래스 구조

### Unit (부모 클래스)
```cpp
class Unit
{
protected:
    std::string name;
    int hp;
    int attackPower;

public:
    Unit(std::string _name, int _hp, int _power)
        : name(_name), hp(_hp), attackPower(_power) {}

    int GetHp() const { return hp; }
    std::string Getname() const { return name; }

    void TakeDamage(int dmg) { hp -= dmg; }

    virtual void Attack() = 0; // 순수 가상 함수로 다형성 구현
};
```
- `Attack()`는 자식 클래스에서 구체화
- `hp`와 `name`은 protected로 자식 클래스 접근 가능
- `TakeDamage()`로 피해 처리


### Hero (자식 클래스)
```cpp
class Hero : public Unit
{
private:
    GameManager<Monster>* monstermanager = nullptr;

public:
    Hero(std::string _name, int _hp, int _power)
        : Unit(_name, _hp, _power) {}

    void setmonster(GameManager<Monster>* m) { monstermanager = m; }

    void Attack() override
    {
        if (!monstermanager) return;
        for (auto it : monstermanager->Getunits())
        {
            it->TakeDamage(attackPower);
            std::cout << name << "가 " << it->Getname() << "을 공격했다! ("
                      << attackPower << " 데미지)\n";
            break; // 첫 번째 몬스터만 공격
        }
    }
};
```
- `monstermanager` 포인터를 통해 몬스터 리스트 접근
- 리스트 순회 후 첫 번째 몬스터만 공격하도록 구현
- Attack() 오버라이딩으로 다형성 구현


### Monster (자식 클래스)
```cpp
class Monster : public Unit
{
private:
    Hero* hero = nullptr;
public:
    Monster(std::string _name, int _hp, int _power)
        : Unit(_name, _hp, _power) {}

    void sethero(Hero* h) { hero = h; }

    void Attack() override
    {
        if (!hero) return;
        hero->TakeDamage(attackPower);
        std::cout << name << "가 " << hero->Getname() << "을 공격했다! (" << attackPower << " 데미지)\n";
    }
};
```
- `hero` 포인터를 통해 영웅 객체 접근
- 순수 가상함수 Attack() 오버라이딩


### GameManager (템플릿 클래스)
```cpp
template <typename T>
class GameManager
{
private:
    std::list<T*> units;

public:
    void Add(T* unit) { units.emplace_back(unit); }
    std::list<T*>& Getunits() { return units; }

    void Update(Hero& hero)
    {
        for (auto it = units.begin(); it != units.end();)
        {
            T* u = *it;
            u->Attack(); // 다형성 호출

            if (u->GetHp() <= 0)
            {
                std::cout << u->Getname() << "가 사망. 리스트에서 제거.\n";
                it = units.erase(it);
            }
            else
            {
                ++it;
            }
        }
    }
};
```
- 리스트를 순회하며 `Attack()` 호출
- HP가 0 이하인 경우 erase로 제거
- 템플릿으로 범용 관리 가능 (Monster뿐 아니라 다른 Unit 타입도 관리 가능)


## 3. main() 예시
```cpp
int main()
{
    Hero hiro("영웅", 100, 15);

    Monster most1("몬스터1", 20, 5);
    Monster most2("몬스터2", 15, 5);
    Monster most3("몬스터3", 30, 5);

    GameManager<Monster> manager;

    most1.sethero(&hiro);
    most2.sethero(&hiro);
    most3.sethero(&hiro);

    hiro.setmonster(&manager);

    manager.Add(&most1);
    manager.Add(&most2);
    manager.Add(&most3);

    hiro.Attack();
    manager.Update(hiro);

    std::cout << "영웅의 남은 HP: " << hiro.GetHp() << "\n";
    std::cout << "남아있는 몬스터 수: " << manager.Getunits().size() << "\n";

    return 0;
}
```

## 4. 오늘 학습 핵심 정리
- 상속과 가상함수(다형성)를 통해 Hero와 Monster가 같은 인터페이스(`Attack()`)를 공유
- `std::list` 사용으로 중간 삭제(erase) 실습
- 템플릿으로 GameManager를 범용적으로 설계 가능
- Hero와 Monster가 서로를 참조하기 위해 포인터(sethero, setmonster) 사용
- `TakeDamage()`로 HP 감소를 안전하게 처리

## 5. 참고 사항 (오류 발생 부분)
- 처음에는 Hero가 GameManager<Monster>*를 사용하려고 했지만 Monster 클래스 전방 선언 문제로 컴파일 오류 발생
- 순환 참조 문제 때문에 Hero와 Monster, GameManager 순서를 조정하거나 포인터/전방 선언 활용 필요
- Attack()에서 매개변수를 사용할 수 없기 때문에, 포인터로 다른 객체 접근 후 공격 처리

---

오늘 정리한 내용과 코드를 깃허브에 올리면 C++ 상속, 다형성, 리스트, 템플릿 학습 결과를 명확히 기록할 수 있음.

