# 지속가능성 고려하기

## 컴파일러 매크로 회피하기

컴파일러 정의와 매크로는 컴파일러가 실제로 구동되기 전에 전처리기에 의해 대체됩니다. 이 동작은 디버깅 과정을 어렵게 만들 수 있습니다. 어느 코드가 어디서 왔는지 디버거가 알 수 없기 때문입니다.

```cpp
// 나쁜 아이디어
#define PI 3.14159;

// 좋은 아이디어
namespace my_project {
  class Constants {
  public:
    // 만약 위 매크로를 확장하면 다음 행처럼 됩니다.
    //   static const double 3.14159 = 3.14159;
    // 하지만 이 행은 컴파일 시 오류가 발생합니다. 이런 오류는 오류만으로 이해하기 쉽지 않습니다.
    static constexpr double PI = 3.14159;
  };
}
```

## 불린(boolean) 매개변수는 피하는 것을 고려하기

불린 매개변수는 코드를 읽기 전까지는 어떤 의미도 제공하지 않습니다. 좀 더 의미있는 이름으로 별도의 함수를 생성하거나 좀 더 명확하게 의미를 알 수 있는 열거형(enumeration)으로 전달할 수 있습니다.

더 자세한 내용은 [이 글](http://mortoray.com/2015/06/15/get-rid-of-those-boolean-function-parameters/)에서 확인하세요.

## 날 것의 반복문 피하기

반복문을 위한 C++ 표준 알고리즘을 이해하고 사용하세요. 자세한 내용은 [C++ Seasoning](https://www.youtube.com/watch?v=qH6sSOr-yk8)에서 확인합니다.

## `assert`를 파생 작업과 함께 사용하지 않기

```cpp
// 나쁜 아이디어
assert(set_value(something));

// 나은 아이디어
[[maybe_unused]] const auto success = set_value(something);
assert(success);
```

`assert()`는 릴리즈 빌드에서 제거되기 때문에 전자의 코드에서 `set_value`는 호출되지 않게 됩니다.

그러므로 두 번째 코드는 보기에는 덜 이쁘더라도 첫 코드처럼 잘못된 코드는 아닙니다.

## `override`와 `final` 적절히 활용하기

이 키워드는 다른 개발자가 어떻게 가상 함수를 활용할지 명확하게 표현하고 가상함수의 시그니처가 변경되었을 때 어떻게 잠재적 오류를 잡아낼 수 있게 합니다. 또한 [컴파일러가 최적화를 수행할 때](http://stackoverflow.com/questions/7538820/how-does-the-compiler-benefit-from-cs-new-final-keyword) 도움이 될 수 있습니다.
