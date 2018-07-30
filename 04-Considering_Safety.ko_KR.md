# 안전 고려하기

## 가능한 한 상수(Const) 쓰기

`const` 키워드는 컴파일러에 변수 또는 메소드가 불변하다고 알려주기 위해 사용합니다. 이 키워드는 컴파일러가 코드를 최적화 하는데 도움되며 개발자에게도 함수에 파생 작업이 있는지 이해하는데 도움이 됩니다. 또한 `const &`은 컴파일러가 불필요한 데이터를 복사하는 일을 방지합니다. [존 카맥의 `const`에 대한 덧글](http://kotaku.com/454293019)도 유익합니다.

```cpp
// 나쁜 아이디어
class MyClass
{
public:
  void do_something(int i);
  void do_something(std::string str);
};


// 좋은 아이디어
class MyClass
{
public:
  void do_something(const int i);
  void do_something(const std::string &str);
};

```

### 주의깊게 반환 형식을 고려하기

 * 게터(Getters)
   * 값을 확인하기 위해 반환하는 값에 `&` 또는 `const &`을 반환하면 상당한 성능 향상을 만들 수 있습니다.
   * 값으로 반환하기는 스레드 안전성에 더 낫습니다. 그리고 반환된 값을 어떻게든 복사 하더라도 성능 하락이 발생하지 않습니다.
   * 만약 API의 반환 형식이 공변적이라면 꼭 `&` 또는 `*`로 반환해야 합니다.
 * 임시, 지역 변수
   * 항상 값으로 반환합니다.

참고자료: https://github.com/lefticus/cppbestpractices/issues/21 https://twitter.com/lefticus/status/635943577328095232 

### 단순한 형식을 상수 참조로 전달하거나 반환하지 않기

```cpp
// 아주 나쁜 아이디어
class MyClass
{
public:
  explicit MyClass(const int& t_int_value)
    : m_int_value(t_int_value)
  {
  }
  
  const int& get_int_value() const
  {
    return m_int_value;
  }

private:
  int m_int_value;
}
```

이런 방식 대신에 단순한 형식을 값으로 반환합니다. 전달한 값을 변경하지 않을 예정이라면 `const` 참조가 아닌 `const`로 선언합니다.

```cpp
// 좋은 아이디어
class MyClass
{
public:
  explicit MyClass(const int t_int_value)
    : m_int_value(t_int_value)
  {
  }
  
  int get_int_value() const
  {
    return m_int_value;
  }

private:
  int m_int_value;
}
```

왜냐고요? 참조로 값을 전달하거나 반환하면 참조는 포인터로 동작하는데 값으로 전달하면 프로세서의 레지스터로 동작하므로 훨씬 빠르기 때문입니다.

## 직접적인 메모리 접근 피하기

C++에서 메모리에 직접 접근해 할당과 할당 해제를 하는 일은 [메모리 오류와 누수](http://blog2.emptycrate.com/content/nobody-understands-c-part-6-are-you-still-using-pointers) 없이 잘하기 쉽지 않습니다. C++11은 이 문제를 피하기 위한 도구를 제공합니다.

```cpp
// 나쁜 아이디어
MyClass *myobj = new MyClass;

// ...
delete myobj;


// 좋은 아이디어
auto myobj = std::make_unique<MyClass>(constructor_param1, constructor_param2); // C++14
auto myobj = std::unique_ptr<MyClass>(new MyClass(constructor_param1, constructor_param2)); // C++11
auto mybuffer = std::make_unique<char[]>(length); // C++14
auto mybuffer = std::unique_ptr<char[]>(new char[length]); // C++11

// 또는 참조 횟수 계산 방식 (reference counting) 객체
auto myobj = std::make_shared<MyClass>(); 

// ...
// myobj는 더이상 사용되지 않을 때 자동으로 할당 해제가 됩니다.
```

## C 방식 배열 대신에 `std::array` 또는 `std::vector`를 사용하기

이 방식은 객체의 인접 메모리 레이아웃을 보장하며 날 포인터를 사용하지 않고도 C 방식의 배열 조작을 대체할 수 있습니다.

또한 `std::shard_ptr`을 배열을 위해 사용하는 일은 [피하세요]((http://stackoverflow.com/questions/3266443/can-you-use-a-shared-ptr-for-raii-of-c-style-arrays)).

## 예외 사용하기

예외는 무시될 수 있습니다. 반환 값은 무시될 수 있으며 확인하지 않으면 충돌이나 메모리 오류의 원인이 될 수 있습니다. `boost::optional`을 사용하는 경우를 예로 볼 수 있습니다. 반면에 예외가 발생하면 그 순간을 찾아 처리할 수 있습니다. 잠재적으로 어플리케이션의 최상위 수준까지 올라가서 로그를 남기고 자동으로 어플리케이션을 재시작 할 수 있습니다.

C++의 원 저작자인 Stroustrup은 [더 명확하게 설명](http://www.stroustrup.com/bs_faq2.html#exceptions-why)합니다.

## C 방식의 캐스팅 대신 C++ 방식 사용하기

C 방식의 캐스팅 대신 C++ 방식의 캐스팅 (`static_cast<>`, `dynamic_cast<>` ...)을 사용합니다. C++ 방식의 캐스팅은 컴파일러가 더 많은 확인을 하며 상당히 안전합니다.

```cpp
// 나쁜 아이디어
double x = getX();
int i = (int) x;

// 그렇게 나쁘지 않은 아이디어
int i = static_cast<int>(x);
```

덧붙여 C++ 캐스팅 방식이 더 시각적이며 검색에도 용이합니다.

하지만 `double`을 `int`로 캐스팅해야 할 필요가 있다면 프로그램 로직을 리팩토링 할 것을 고려하기 바랍니다. (예를 들면 오버플로우나 언더플로우를 추가적으로 확인하는 방식으로.) 3배를 측정하고 0.9999999999981 배를 잘라냅니다.

## 가변 함수를 정의하지 말 것

가변 함수는 다양한 수의 매개변수를 사용할 수 있습니다. 아마 가장 잘 알려진 예제는 `printf()` 함수일 겁니다. 아마 이런 형식의 함수를 정의해서 사용할 가능성이 있지만 이런 함수는 보안 위협 문제가 있습니다. 가변 함수의 용도는 형식에 안전하지 않으며 매개변수를 잘못 입력하면 정의하지 않은 동작으로 프로그램이 종료될 수 있습니다. 이렇게 정의되지 않은 동작은 보안 문제를 야기할 수 있습니다. C++11을 지원하는 컴파일러를 사용한다면 이런 가변 함수 대신 가변 템플릿을 사용할 수 있습니다.

[몇 컴파일러에서는 타입에 안전한 C 방식의 가변함수를 만들 수 있습니다](https://github.com/lefticus/cppbestpractices/issues/53).

## 추가 리소스

David Wheeler의 [다음 하트블리드를 어떻게 방지하나](http://www.dwheeler.com/essays/heartbleed.html)에서 코드 안전에 대한 현재 상황을 잘 분석하며 어떻게 안전하게 코드를 작성하는지 잘 설명하고 있습니다.
