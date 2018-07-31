# 성능 고려하기

## 빌드 시간

### 가능하면 전달 선언하기

이렇게 작성합니다.

```cpp
// 헤더 파일 등등
class MyClass;

void doSomething(const MyClass &);
```

이 방식 대신에 말입니다.

```cpp
// 헤더 파일 등등
#include "MyClass.hpp"

void doSomething(const MyClass &);
```

이 방법은 템플릿에도 적용됩니다.

```cpp
template<typename T> class MyTemplatedType;
```

이 방법은 컴파일 시간과 의존성을 다시 만드는 시간을 줄이는 적극적인 접근법입니다.

### 필요 없는 템플릿 인스턴스화 피하기

템플릿을 인스턴스로 만드는 일은 공짜가 아닙니다. 템플릿을 많이 만들거나 템플릿으로 더 많은 코드를 만들면 컴파일 코드 크기와 빌드 시간을 늘리게 됩니다.

더 상세한 예시는 [이 포스트](http://blog2.emptycrate.com/content/template-code-bloat-revisited-smaller-makeshared)에서 확인할 수 있습니다.

### 재귀 템플릿 인스턴스화 피하기

재귀 템플릿 인스턴스화는 컴파일러에 엄청난 부하를 만들 수 있는 데다 코드를 이해하기 어렵게 만듭니다.

[가변 확장을 사용해서 접을 수 있는 곳은 접는 방식을 고려하길 바랍니다.]((http://articles.emptycrate.com/2016/05/14/folds_in_cpp11_ish.html)

### 빌드 분석하기

[Templight](https://github.com/mikael-s-persson/templight)와 같은 도구를 사용하면 프로젝트의 빌드 시간을 분석할 수 있습니다. 구성하는데 노력이 좀 필요하지만 한번 구축하고 나서는 clang++을 대체할 수 있습니다.

Templight을 빌드에 사용한 후에는 그 결과를 분석해야 합니다. [templight-tools](https://github.com/mikael-s-persson/templight-tools) 프로젝트는 다양한 방법을 지원합니다. (저자 노트: callgrind convert를 사용하고 kcachegrind로 결과를 시각화하는 방법을 추천합니다.)

### 방화벽이 헤더 파일을 자주 변경함

#### 필요 없이 헤더를 추가하지 않음

컴파일러는 각 include 디렉티브를 보면 각각 동작을 하게 됩니다. `#ifndef` 인클루드 가드를 보더라도 컴파일러는 파일을 열어서 처리를 시작합니다.

[include-what-you-use](https://github.com/include-what-you-use/include-what-you-use)는 필요한 헤더가 무엇인지 판단하는데 도움 주는 도구입니다.

#### 전처리기 부하 줄이기

이 항목은 "자주 변경하는 헤더 파일에 방화벽 적용하기(역주: Compiler Firewall 또는 Pimpl)"과 "불필요한 헤더 포함하지 않기"의 일반적인 형태입니다. BOOST_PP와 같은 도구가 매우 유용할 수 있지만 이런 도구도 전처리기에 큰 짐을 얹습니다.

## 미리 컴파일 된 헤더 사용하기

미리 컴파일된 헤더는 대형 프로젝트의 컴파일 시간을 상당히 줄일 수 있습니다. 선택한 헤더를 중간 형태 (PCH 파일)로 컴파일해두는 것으로 컴파일러가 더 빠르게 처리할 수 있습니다. 이렇게 미리 컴파일하는 헤더는 자주 사용하지만 변경은 자주 일어나지 않는 헤더(예로 시스템과 라이브러리 헤더)에 적용하면 컴파일 속도 감소라는 목표를 달성할 수 있습니다.

하지만 헤더를 미리 컴파일 해두는 일에 몇 가지 단점이 있으며 이 점을 계속 염두해야 합니다.

* 미리 컴파일한 헤더는 이식 가능하지 않습니다. (not portable.)
* 생성된 PCH 파일은 기기에 의존적입니다.
* 생성된 PCH 파일은 크기가 꽤 클 수 있습니다.
* 헤더 의존성을 망가뜨릴 수 있습니다. 모든 파일이 미리 컴파일된 헤더를 포함할 가능성이 있기 때문입니다. 그런 일이 정말 일어나면 미리 컴파일된 헤더를 껐을 때 빌드가 깨지게 됩니다. 배포 형태가 라이브러리라면 이런 방식은 문제가 될 수 있습니다. 이런 이유에서 처음은 미리 컴파일된 헤더와 함께 빌드하고 두번째는 이 기능을 끄고 빌드하기를 매우 추천합니다.

미리 컴파일된 헤더는 일반 컴파일러 대부분이 지원합니다. [GCC](https://gcc.gnu.org/onlinedocs/gcc/Precompiled-Headers.html), [Clang](http://clang.llvm.org/docs/PCHInternals.html),  [Visual Studio](https://msdn.microsoft.com/en-us/library/szfdksca.aspx)도 지원합니다. cmake 플러그인인  [cotire](https://github.com/sakra/cotire/)은 빌드 시스템에 미리 컴파일된 헤더를 추가할 수 있도록 돕는 도구입니다.

### 도구 사용 고려하기

물론 이런 도구를 사용한다고 좋은 디자인을 대체할 수는 없습니다.

 * [ccache](https://ccache.samba.org/)
 * [warp](https://github.com/facebook/warp), Facebook의 전처리기

### 임시 디렉토리를 램디스크에 넣기

[이 유튜브 영상](https://www.youtube.com/watch?v=t4M3yG1dWho)에서 자세한 내용을 확인하세요.

### gold 링커 사용하기

리눅스라면 GCC에서 gold 링커 사용을 고려해보세요.

## 런타임

### 코드를 분석하자!

코드를 분석하지 않고는 어디가 병목구간인지 실제로 알아낼 방법이 없습니다.

 * http://developer.amd.com/tools-and-sdks/opencl-zone/codexl/
 * http://www.codersnotes.com/sleepy

### 코드 단순화하기

깨끗하고 단순하고 읽기 쉬운 코드는 컴파일러 또한 제대로 구현할 가능성이 높습니다.

### 초기화 목록 사용하기

```cpp
// 이렇게
std::vector<ModelObject> mos{mo1, mo2};

// 또는
auto mos = std::vector<ModelObject>{mo1, mo2};
```

```cpp
// 이렇게 하지 말 것
std::vector<ModelObject> mos;
mos.push_back(mo1);
mos.push_back(mo2);
```

초기화 목록은 훨씬 더 효율적입니다. 객체 카피를 줄이고 컨테이너의 크기를 다시 설정하게 됩니다.

### 임시 객체 줄이기

```cpp
// 이 방법 대신에
auto mo1 = getSomeModelObject();
auto mo2 = getAnotherModelObject();

doSomething(mo1, mo2);
```

```cpp
// 이런 방식을 고려하기

doSomething(getSomeModelObject(), getAnotherModelObject());
```

코드를 이런 방식으로 작성하면 컴파일러가 무브 연산을 수행하는 것을 방지합니다.

### 무브 연산(Move operations) 활성화하기

무브 연산은 C++11에서 가장 많이 언급된 기능 중 하나입니다. 이 기능은 컴파일러가 몇 상황에서 임시 객체를 옮길 때 추가적인 복사 작업을 하지 않고 임시 객체를 옮길 수 있도록 하는 기능입니다.

우리가 작성한 몇 코딩 방법(직접 소멸자를 선언하거나, 할당 연산자나 복사 생성자를 선언하는 등)은 컴파일러가 무브 생성자를 생성하는 것을 방지합니다.

대부분 코드에서는 간단하며 다음 정도면 충분합니다.

```cpp
ModelObject(ModelObject &&) = default;
```

하지만 MSVC2013에서는 아직 이런 방식으로 사용할 수 없습니다.

### `shared_ptr` 복사 죽이기

`shared_ptr` 객체는 복사하는데 생각보다 더 많은 비용이 듭니다. 왜냐하면 참조 횟수 계산 방식은 원자성과 스레드 안전성이 지켜져야 하기 때문입니다. 위에서 언급을 다시 강조하자면 임시적인 것과 지나치게 많은 객체 복사를 피해야 합니다. 단순히 Pimpl을 사용한다고 해서 이런 복사로부터 자유롭다고 할 수 없습니다.

### 가능한 한 복사와 재할당 줄이기

간단한 경우에는 삼항 연산자를 사용할 수 있습니다.

```cpp
// 나쁜 아이디어
std::string somevalue;

if (caseA) {
  somevalue = "Value A";
} else {
  somevalue = "Value B";
}
```

```cpp
// 나은 아이디어
const std::string somevalue = caseA ? "Value A" : "Value B";
```

복잡한 경우에는 [즉시 실행 람다함수](http://blog2.emptycrate.com/content/complex-object-initialization-optimization-iife-c11)를 유용하게 쓸 수 있습니다. 

```cpp
// 나쁜 아이디어
std::string somevalue;

if (caseA) {
  somevalue = "Value A";
} else if(caseB) {
  somevalue = "Value B";
} else {
  somevalue = "Value C";
}
```

```cpp
// 좋은 아이디어
const std::string somevalue = [&](){
    if (caseA) {
      return "Value A";
    } else if (caseB) {
      return "Value B";
    } else {
      return "Value C";
    }
  }();
```


### 초과 예외 피하기

일반 처리 과정 중 내부적으로 던지고 받는 예외는 어플리케이션의 실행을 느리게 합니다. 또한 디버거와 함께 사용자 경험을 해치는데 디버거가 모든 예외 이벤트를 관찰하고 보고하기 때문입니다. 가능하다면 내부 예외 처리를 회피하는 것이 가장 좋습니다.

### `new` 키워드 피하기

이미 다뤘던 내용처럼 메모리에 직접 접근하지 않는 대신 `unique_ptr`와 `shared_ptr`를 사용해야 한다고 했었습니다.

힙 할당은 스택 할당보다 훨씬 비싸지만 사용해야 하는 경우가 있습니다. 설상가상으로 `shared_ptr`이 생성하는 크기는 2 힙 할당을 필요로 합니다.

하지만 `make_shared` 함수는 이 할당을 1개로 줄입니다.

```cpp
std::shared_ptr<ModelObject_Impl>(new ModelObject_Impl());

// 이렇게 작성해야 합니다
std::make_shared<ModelObject_Impl>(); // (이 코드가 더 읽기 쉽고 간결합니다)
```

### `shared_ptr`보다 `unique_ptr` 선호하기

If possible use `unique_ptr` instead of `shared_ptr`. The `unique_ptr` does not need to keep track of its copies because it is not copyable. Because of this it is more efficient than the `shared_ptr`. Equivalent to `shared_ptr` and `make_shared` you should use `make_unique` (C++14 or greater) to create the `unique_ptr`:

만약 `unique_ptr`을 `shared_ptr` 대신 사용하면 `unique_ptr`은 복사할 수 없기 때문에 복사본을 계속 추적할 필요가 없습니다. 그런 덕분에 `shared_ptr`보다 더 효과적입니다. `shared_ptr`를 `make_shared`로 사용하는데 `unique_ptr`에서는 동일한 기능을 `make_unique`로 사용할 수 있습니다 (C++14 또는 그 이상).

```cpp
std::make_unique<ModelObject_Impl>();
```

현재 모범 사례로는 팩토리 함수에서도 `unique_ptr`을 반환하는 방식을 추천하며 그 이후에 필요에 따라 `unique_ptr`을 `shared_ptr`로 변환해서 사용하기도 합니다.

```cpp
std::unique_ptr<ModelObject_Impl> factory();

auto shared = std::shared_ptr<ModelObject_Impl>(factory());
```

### std::endl 제거하기

`std::endl`는 암시적으로 비우기 연산(flush operation)을 수행합니다. 이 동작은 `"\n" << std::flush`와 동일합니다.

### 변수 범위(scope) 제한하기

변수는 가능한 한 나중에 선언되어야 하며 이상적으로는 객체가 초기화 될 때 선언 되는 방식이 좋습니다. 변수 범위를 줄이면 그 결과로 메모리를 더 적게 사용하고 코드는 더 효율적이며 컴파일러가 코드를 더 최적화하는데 도움이 됩니다.

```cpp
// 좋은 아이디어
for (int i = 0; i < 15; ++i)
{
  MyObject obj(i);
  // obj로 뭔가 합니다!

// 나쁜 아이디어
MyObject obj; // 의미 없는 객체 초기화
for (int i = 0; i < 15; ++i)
{
  obj = MyObject(i); // 불필요한 할당 코드
  // obj로 뭔가 합니다!
}
// obj는 여전히 메모리에 자리 잡고 있는데 이유가 없습니다.
```

[This topic has an associated discussion thread](https://github.com/lefticus/cppbestpractices/issues/52).

### `double`을 `float`로 바꾸기 선호하기, 물론 먼저 테스트 할 것

아마도 상황과 컴파일러의 최적화 능력에 따라서 둘 중 하나가 다른 하나에 비해 빠를 것입니다. `float`을 선택하면 정밀도가 낮아지고 변환하는 과정이 더 느려질 겁니다. 백터 연산을 수행하는데 정밀도를 조금 희생해도 되는 경우라면 `float`이 좀 더 빠릅니다.

C++에서 소수점이 있는 값이라면 `double`을 기본 형식으로 사용하도록 권합니다.

더 자세한 정보는 [stackoverflow](http://stackoverflow.com/questions/4584637/double-or-float-which-is-faster)에서 확인할 수 있습니다.

### `++i`보다 `i++` 선호하기

... when it is semantically correct. Pre-increment is [faster](http://blog2.emptycrate.com/content/why-i-faster-i-c) than post-increment because it does not require a copy of the object to be made.

... 물론 문법적으로 맞을 때 씁니다. 전위증가(pre-increment)가 후위증가(post-increment)보다 [빠른데](http://blog2.emptycrate.com/content/why-i-faster-i-c) 복사할 객체를 생성할 필요가 없기 때문입니다.

```cpp
// 나쁜 아이디어
for (int i = 0; i < 15; i++)
{
  std::cout << i << '\n';
}

// 좋은 아이디어
for (int i = 0; i < 15; ++i)
{
  std::cout << i << '\n';
}
```

현대적인 컴파일러 대부분은 이 두 반복문을 동일한 기계어 코드로 최적화하지만 여전히 `++i`를 사용하는 쪽이 모범 사례입니다. 사용하지 않을 이유가 전혀 없으며 컴파일러가 최적화하지 않아도 문제가 되지 않기 때문입니다.

또한 컴파일러가 상수 타입일 때는 최적화하지 못하며 모든 반복자(iterator)나 사용자 정의 형식에는 필요하지 않습니다.

결론적으로 전위증가나 후위증가를 둘 다 써도 의미상 맞을 때에는 전위증가 연산자를 사용하기를 추천합니다.

### Char는 char고 string은 string

```cpp
// 나쁜 아이디어
std::cout << someThing() << "\n";

// 좋은 아이디어
std::cout << someThing() << '\n';
```

아주 사소한 부분이긴 하지만 `"\n"`은 컴파일러에서 `const char *`로 번역되며 스트림에 작성할 때에 `\0`를 위해서 범위 검사를 수행하게 됩니다. (또는 문자열을 더할 때) `'\n'`은 문자 하나로 처리되고 많은 CPU 명령을 피할 수 있습니다.

만약 이런 효과적이지 않은 코드를 많이 사용해서 성능에 영향을 줄 수 있는 것도 사실입니다. 하지만 이 예시에서 생각해봐야 할 더 중요한 점은 컴파일러와 런타임에서 코드가 실행될 때 어떻게 수행되는가 하는 점입니다.

### `std::bind` 절대 사용하지 않기

`std:bind`는 항상 필요 이상의 과부하를 많이 만들어내므로 (컴파일 시간, 런타임 시간 모두) 이 방법 대신 간단하게 람다식을 사용합니다.

```cpp
// 나쁜 아이디어
auto f = std::bind(&my_function, "hello", std::placeholders::_1);
f("world");

// 좋은 아이디어
auto f = [](const std::string &s) { return my_function("hello", s); };
f("world");
```

### 표준 라이브러리 알기

공급 업체(vendor)가 제공한 표준 라이브러리를 사용하면 이미 고도로 최적화된 컴포넌트를 사용할 수 있습니다. 적절하게 사용하세요.

#### `in_place_t`와 관련된 기능

`in_place_t`와 객체를 효과적으로 생성할 수 있는 연관 태그인 `std::tuple`, `std::any`, `std::variant` 사용법에 유의하세요.
