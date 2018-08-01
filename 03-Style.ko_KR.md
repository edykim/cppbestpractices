# 스타일

스타일에서 가장 중요한 면은 일관성입니다. 두번째로 중요한 점은 평균적인 C++ 프로그래머가 익숙하게 생각하는 스타일을 따르는 것입니다.

C++는 식별자명에 길이를 제멋대로 입력할 수 있도록 허용하므로 이름을 지정할 때 간결하게 정할 필요가 없습니다. 서술적인 명칭을 사용하고 일관적인 스타일을 유지하기 바랍니다.

 * 카멜 표기법: `CamelCase`
 * 스네이크 표기법: `snake_case`

이 두 방식이 일반적입니다. *snake_case*를 사용한다면 원하는 경우 문법 검사를 사용할 수 있다는 장점도 있습니다.

## 스타일 가이드라인 발행하기

어떤 스타일 가이드라인을 발행하든지 `.clang-format` 파일을 작성해서 어떤 스타일을 적용할지 명확하게 명세를 작성합니다. 이 파일로는 명명 규칙을 정할 수 없지만 오픈 소스 프로젝트에서 명명 규칙에 일관적인 스타일을 유지하는 일은 특히 중요합니다.

모든 IDE와 대부분의 편집기는 clang-format 지원이 내장되어 있거나 손쉽게 확장을 설치할 수 있습니다.

 * VSCode https://marketplace.visualstudio.com/items?itemName=xaver.clang-format
 * Visual Studio https://marketplace.visualstudio.com/items?itemName=LLVMExtensions.ClangFormat#review-details
 * Resharper++: https://www.jetbrains.com/help/resharper/2017.2/Using_Clang_Format.html
 * Vim
     * https://github.com/rhysd/vim-clang-format
     * https://github.com/chiel92/vim-autoformat
 * XCode: https://github.com/travisjeffery/ClangFormat-Xcode

## 일반 C++ 명명 규칙

 * 형식은 대문자로 시작합니다: `MyClass`.
 * 함수와 변수는 소문자로 시작합니다: `myMethod`.
 * 상수는 모두 대문자입니다: `const double PI=3.14159265358979323;`.

C++ 표준 라이브러리(그리고 [Boost](http://www.boost.org/)와 같이 잘 알려진 다른 C++ 라이브러리)에서는 다음 규칙을 따릅니다.

 * 매크로 이름은 대문자와 밑줄(underscore)를 사용합니다: `INT_MAX`.
 * 템플릿 매개변수명은 카멜 표기법을 사용합니다: `InputIterator`.
 * 다른 명칭은 스네이크 표기법을 사용합니다: `unordered_map`.

## 비공개 객체 데이터 구분하기

비공개(private) 데이터의 명칭은 `m_`을 접두어로 붙여 공개(public) 데이터와 구분합니다. `m_`은 "멤버(member)" 데이터를 뜻합니다.

## 함수 매개변수 구분하기

코드 기반의 일관성 유지는 가장 중요합니다. 이 방식이 일관성을 유지하는데 도움이 될 수 있습니다.

함수 매개변수의 명칭에는 `t_` 접두어를 사용합니다. `t_`는 "the"로 생각할 수 있겠지만 그건 임의로 붙인 의미입니다. 함수 파라미터를 범위 내에서 다른 변수와 구분하는 것은 일관적인 명명 전략을 제공합니다.

어떤 접두어, 접미어든 조직에서 선택할 수도 있고 사용하지 않을 수도 있습니다. 이는 한 예일 뿐입니다. *이 제안은 논란이 많은 주제로 추가적인 논의는 이슈 [#11](https://github.com/lefticus/cppbestpractices/issues/11)을 참고하세요.*

```cpp
struct Size
{
  int width;
  int height;

  Size(int t_width, int t_height) : width(t_width), height(t_height) {}
};

// 이 버전은 아마도 스레드 안전이나 다른 어떤 경우에는 말이 될 수 있습니다만
// 좀더 요점을 생각하면 때때로 데이터를 숨겨야 할 필요도 있고 때로는 그러지 않은
// 경우도 있다는 뜻입니다.
class PrivateSize
{
  public:
    int width() const { return m_width; }
    int height() const { return m_height; }
    PrivateSize(int t_width, int t_height) : m_width(t_width), m_height(t_height) {}

  private:
    int m_width;
    int m_height;
};
```

## 어떤 명칭도 `_`로 시작하지 않는다

만약 이런 명칭을 사용한다면 컴파일러와 표준 라이브러리 구현에서 사용하는 예약어와 충돌할 가능성이 있습니다.

http://stackoverflow.com/questions/228783/what-are-the-rules-about-using-an-underscore-in-a-c-identifier

## 잘 구성된 예제

```cpp
class MyClass
{
public:
  MyClass(int t_data)
    : m_data(t_data)
  {
  }

  int getData() const
  {
    return m_data;
  }

private:
  int m_data;
};
```

## Out-of-Source-Directory 빌드 활성화하기

생성된 결과물은 소스 폴더와 분리된 공간에 생성되도록 설정합니다.


## `nullptr` 사용하기

C++11에서 널(null) 포인터를 표현하는 특별한 값으로 `nullptr`가 도입되었습니다. 이 값은 널 포인터를 나타낼 때 `0` 또는 `NULL` 대신 사용해야 합니다.

## 주석

주석 블럭은 `/* */` 대신 `//`를 사용해야 합니다. `//`를 사용하면 디버그를 하는 동안 코드 블럭을 주석 처리하는데 편리합니다.

```cpp
// 이 함수는 무슨 무슨 일을 함
int myFunc()
{
}
```

디버깅 할 때는 함수 블럭을 다음과 같이 주석 처리 할 수 있습니다.

```cpp
/*
// 이 함수는 무슨 무슨 일을 함
int myFunc()
{
}
*/
```

함수 주석 헤더로 `/* */`를 사용했다면 이런 방식의 주석 처리를 할 수 없습니다.

## 헤더 파일에 `using namespace` 절대 사용하지 말 것

이런 코드를 작성하면 `using`으로 불러온 네임스페이스의 모든 파일이 헤더에 포함되게 됩니다.

이런 방식은 네임스페이스를 오염시키며 이름 충돌을 야기할 수도 있습니다.

`using namespace`를 사용하는 것은 구현 파일 내에서면 충분합니다.

## 가드(Guards) 포함하기

헤더 파일은 구분되는 이름의 가드를 추가하는 방식으로 동일한 헤더를 여러번 불러오는 일을 피하고 다른 프로젝트 헤더와 충돌하는 일을 막아야 합니다.

```cpp
#ifndef MYPROJECT_MYCLASS_HPP
#define MYPROJECT_MYCLASS_HPP

namespace MyProject {
  class MyClass {
  };
}

#endif
```

`#pragma once` 디렉티브를 사용하는 방법도 고려할 수 있습니다. 이 디렉티브는 여러 컴파일러에서 지원하는 비표준 방식입니다.

이 디렉티브는 짧고 의도가 명확합니다.

## 블럭에는 `{}`가 필요합니다.

블럭을 명확하게 하지 않고 두면 맥락 오류를 만들기 쉽습니다.

```cpp
// 나쁜 아이디어
// 이 코드는 컴파일되고 원하는 대로 동작하지만 미래에 코드를 수정하거나
// 주의를 기울이지 않고 코드를 바꾼 경우에 혼란스러운 문제를 만들 수 있습니다.
for (int i = 0; i < 15; ++i)
  std::cout << i << std::endl;

// 나쁜 아이디어
// 여기서 cout은 반복 내에 포함되어 있지 않았는데 놓치기 쉽습니다.
int sum = 0;
for (int i = 0; i < 15; ++i)
  ++sum;
  std::cout << i << std::endl;


// 좋은 아이디어
// 반복이 어디까지인지 명확하게 표시되었습니다. (if든, 어떤 상황에서든 명확해집니다)
int sum = 0;
for (int i = 0; i < 15; ++i) {
  ++sum;
  std::cout << i << std::endl;
}
```

## 행 적절 길이 유지하기

```cpp
// 나쁜 아이디어
// 길어서 읽기 힘듭니다.
if (x && y && myFunctionThatReturnsBool() && caseNumber3 && (15 > 12 || 2 < 3)) {
}

// 좋은 아이디어
// 논리적으로 구분되어 있으며 쉽게 읽을 수 있습니다.
if (x && y && myFunctionThatReturnsBool()
    && caseNumber3
    && (15 > 12 || 2 < 3)) {
}
```

많은 프로젝트와 코딩 표준은 행 당 80자 또는 100자보다 적게 사용하자는 부드러운 지침이 있습니다.

그렇게 작성한 코드는 일반적으로 읽기 쉽습니다.

또한 이렇게 코드를 작성하면 다른 두 파일을 좌우에 놓고도 작지 않은 글자로 양쪽 내용을 함께 확인할 수 있는 장점이 있습니다.


## 지역 파일을 포함할 때 "" 사용하기

`<>` 표기는 [시스템 인클루드(include)에 예약](http://blog2.emptycrate.com/content/when-use-include-verses-include)되어 있습니다.

```cpp
// 나쁜 아이디어. 컴파일에 -I 디렉티브를 추가해야 합니다.
// 그리고 표준에 반대됩니다.
#include <string>
#include <includes/MyHeader.hpp>

// 더 나쁜 아이디어
// 이런 경우에는 더 명확한 -I 디렉티브를 사용해야 하며
// 코드를 패키징하고 배포하는데 더 어렵습니다.
#include <string>
#include <MyHeader.hpp>


// 좋은 아이디어
// 추가적인 디렉티브가 필요 없으며 사용자에게 로컬 파일 임을
// 명확하게 보여줍니다.
#include <string>
#include "MyHeader.hpp"
```

## 멤버 변수 초기화하기

멤버 초기화 목록을 함께 사용합니다.

POD 형식에서는 수동 초기화와 초기화 목록이 동일한 성능을 냅니다. 하지만 다른 형식에는 명확하게 성능 향상이 있습니다.

```cpp
// 나쁜 아이디어
class MyClass
{
public:
  MyClass(int t_value)
  {
    m_value = t_value;
  }

private:
  int m_value;
};

// 나쁜 아이디어
// 이 코드는 대입 전 m_myOtherClass를 위해
// 생성자에서 추가적인 호출이 필요합니다.
class MyClass
{
public:
  MyClass(MyOtherClass t_myOtherClass)
  {
    m_myOtherClass = t_myOtherClass;
  }

private:
  MyOtherClass m_myOtherClass;
};

// 좋은 아이디어
// 성능 향상은 없지만 코드는 깔끔해졌습니다.
class MyClass
{
public:
  MyClass(int t_value)
    : m_value(t_value)
  {
  }

private:
  int m_value;
};

// 좋은 아이디어
// 이 코드에서는 기본 생성자를 위한 m_myOtherClass가 전혀 호출되지 않았습니다.
// 이처럼 MyOtherClass가 is_trivially_default_constructible이 아니라면
// 성능 향상이 있습니다.
class MyClass
{
public:
  MyClass(MyOtherClass t_myOtherClass)
    : m_myOtherClass(t_myOtherClass)
  {
  }

private:
  MyOtherClass m_myOtherClass;
};
```

C++11에서는 각 멤버에 기본 값을 지정할 수 있습니다. (`=`나 `{}`를 사용합니다.)

### 기본 값을 `=`로 할당하기

```cpp
// ... //
private:
  int m_value = 0; // 허용
  unsigned m_value_2 = -1; // signed에서 unsigned로 축소(narrowing) 할당 허용
// ... //
```

이 방식은 생성자가 멤버 객체를 초기화하는 일을 절대 "잊지" 않도록 합니다.

### 기본 값 중괄호 초기화(brace initialization)로 할당하기

중괄호 초기화를 사용하면 축소(narrowing) 할당을 하지 못하도록 컴파일 시간에 체크합니다.

```cpp
// 좋은 아이디어

// ... //
private:
  int m_value{ 0 }; // 허용
  unsigned m_value_2 { -1 }; // signed를 unsigned로 축소 할당은 할 수 없으며 컴파일 시 오류가 발생합니다.
// ... //
```

꼭 사용해야 할 이유가 없다면 `=` 초기화보다 `{}`를 사용합니다.

멤버 초기화를 잊으면 소스 내에서 정의하지 않은 동작을 하는 버그가 발생하는데 때로 정말 찾아내기 어렵습니다.

멤버 변수가 초기화 이후 변경되지 않아야 한다면 `const`로 표시해둡니다.

```cpp
class MyClass
{
public:
  MyClass(int t_value)
    : m_value{t_value}
  {
  }

private:
  const int m_value{0};
};
```

상수 멤버 변수는 새 값을 할당할 수 없기 때문에 클래스 같은 경우에는 의미있는 복사 할당 연산자를 못가질 수도 있습니다.

## 항상 네임스페이스 사용하기

전역 네임스페이스에 식별자를 선언해야 할 일은 거의 전무합니다. 대신에 함수와 클래스는 적절한 이름의 네임스페이스나 네임스페이스 내에 있는 클래스에 존재해야 합니다. 전역 네임스페이스에 식별자를 선언하면 다른 라이브러리의 식별자와 충돌할 가능성이 높습니다. (대부분 C인데 C에는 네임스페이스가 없기 때문입니다.)

## 표준 라이브러리 기능에 올바른 Integer 형식 사용하기

표준 라이브러리에서는 크기와 관련된 모든 것에 `std::size_t`를 사용합니다. 크기 `size_t`는 구현에 정의되어 있습니다.

일반적으로 `auto`를 사용하면 대부분의 문제를 해결하지만 전부 해결하진 않습니다.

올바른 integer 형식을 사용하고 C++ 표준 라이브러리와 함께 일관성을 유지하기 바랍니다. 현재 사용하는 플랫폼에는 큰 문제가 없을 수 있겠지만 플랫폼이 변경되었을 때 문제가 발생할 수 있습니다.

*참고로 unsigned 값으로 조작을 수행할 때 integer 언더플로우가 발생할 수 있습니다. 다음은 그 예시입니다.*

```cpp
std::vector<int> v1{2,3,4,5,6,7,8,9};
std::vector<int> v2{9,8,7,6,5,4,3,2,1};
const auto s1 = v1.size();
const auto s2 = v2.size();
const auto diff = s1 - s2; // diff는 아주 큰 숫자로 underflow가 발생합니다
```

## .hpp와 .cpp를 파일 확장자로 사용하기

사실 선호도의 문제에 가깝지만 .hpp와 .cpp는 다양한 편집기와 도구에서 널리 인식되는 확장자입니다. 그래서 이 확장자를 사용하는 것이 실용적입니다. 특히 Visual Studio는 .cpp와 .cxx를 C++ 파일로 자동 인식하지만 Vim은 .cc를 C++ 파일로 인식하지 않습니다.

어느 대형 프로젝트 ([OpenStudio](https://github.com/NREL/OpenStudio))는 .hpp와 .cpp를 사용자 생성 파일에 사용하고 .hxx와 .cxx를 도구가 생성한 파일에 사용하고 있습니다. 이 확장자 모두 잘 인식되며 구분해서 보는데 도움이 됩니다.

## 탭과 스페이스 섞지 않기

어떤 편집기는 들여쓰기에 탭과 스페이스를 섞어 사용하는 방식이 기본값입니다. 이 설정은 탭 들여쓰기 설정이 동일하지 않은 사람이라면 읽기 어려운 코드가 되고 맙니다. 이런 문제가 발생하지 않도록 에디터를 설정하세요.

## 파생 작업(side effect)가 있는 코드를 assert() 내에 쓰지 않기

```cpp
assert(registerSomeThing()); // registerSomeThing()이 true를 반환
```

위 코드는 컴파일러가 디버그 빌드를 수행했을 때는 실행되고 릴리즈 빌드를 했을 때는 제거됩니다. 즉, 위와 같은 코드는 디버그와 릴리즈 빌드에서 동작의 차이를 만듭니다. `assert()`는 매크로로 릴리즈 모드에서는 아무 일도 하지 않기 때문입니다.

## 템플릿 두려워하지 않기

템플릿은 [DRY 원칙](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself)을 따르는데 도움됩니다.

매크로는 네임스페이스 아래 존재하지 않는 등의 이유로 매크로보다 템플릿을 선호해야 합니다.

## 연산자(Operator) 오버로딩 사용 유의하기

연산자 오버로딩은 표현 구문을 활용하기 위해 발명되었습니다. 여기서 표현은 두 정수를 더할 때 `a.add(b)` 대신에 `a + b` 방식으로 작성하는 것을 의미합니다. 일반적인 다른 예는 `std::string` 입니다. 두 문자열을 합칠 때 `string1 + string2`으로 붙이는 것이 일반적입니다.

이런 편리함이 있지만 너무 많이 사용하거나 잘못된 연산자 오버로딩을 만들어 읽기 힘든 코드를 생성하는 일도 쉽게 일어납니다. 연산자를 오버로딩 할 때 다음 [스택오버플로우의 글](http://stackoverflow.com/questions/4421706/operator-overloading/4421708#4421708)에서 설명하는 기본 규칙 세 가지를 따르기 바랍니다.

세부적으로는 다음 항목을 염두하세요.

* 리소스를 다룰 때 `operator=()`를 오버로딩하는 것은 필수입니다. 아래 나오는 [0번 규칙 고려하기](03-Style.ko_KR.md#0번-규칙-고려하기)를 참고하세요.
* 다른 연산자는 각 연산자의 맥락에 맞는 상황에서만 오버로딩을 합니다. 일반적인 시나리오는 무언가 붙여야 하는 상황에 +를 사용하는 것, 부정 연산자(negating expressions, 역주: 서두에 ! 붙이는 것)를 참 또는 거짓으로 처리하기 등이 있습니다.
* 항상 [연산자 우선 순위](http://en.cppreference.com/w/cpp/language/operator_precedence)를 유의해서 직관적이지 않는 구성은 피하도록 합니다.
* 숫자(numeric) 형식 또는 특정 분야에서 이해할 수 있는 기호인 경우가 아니라면 ~, % 같은 별난 연산자(exotic operators)는 오버로드하지 않습니다.
* [절대](http://stackoverflow.com/questions/5602112/when-to-overload-the-comma-operator?answertab=votes#tab-top) `operator,()` (쉼표 연산자)는 오버로드 하지 않습니다.
* `operator>>()`와 `operator<<()` 연산자를 스트림과 함께 적용할 때는 멤버가 아닌 함수를 사용합니다. 예를 들면 `operator<<(std::ostream &, MyClass const &)`을 오버로드하는 것으로 클래스를 스트림에 "쓰는" 것이 가능합니다. `std::cout`, `std::fstream`, `std::stringstream`이 그런 방식으로 작성되었습니다. 마지막은 값의 문자열 표현을 생성하기 위해 종종 사용합니다.
* 오버로드하는 일반적인 연산자는 [여기에 정리](http://stackoverflow.com/questions/4421706/operator-overloading?answertab=votes#tab-top)되어 있습니다.

임의 생성자를 어떻게 구현하는지 세부적인 팁은 [C++ 연산자 오버로딩 가이드라인](http://courses.cms.caltech.edu/cs11/material/cpp/donnie/cpp-ops.html) ([번역](https://www.haruair.com/blog/4582))에서 찾을 수 있습니다.

## 암시적 변환 피하기

### 단일 매개변수 생성자

단일 매개변수 생성자는 컴파일 타임에 적용되어 형식 간 자동 변환에 사용됩니다. `std::string(const char *)`와 같은 방식은 유용하지만 이런 코드는 일반적으로 피해야 하는데 런타임 과부하가 발생할 수 있기 때문입니다.

대신에 단일 매개변수 생성자를 `explicit` 키워드를 사용해서 명시적으로 선언합니다.

### 변환 연산자

단일 매개변수 생성자와 비슷하게 변환 연산자는 컴파일러에서 호출될 수 있으며 의도치 않은 과부하를 만들 수 있습니다. 이런 연산자도 `explicit`로 표시되어야 합니다.

```cpp
// 나쁜 아이디어
struct S {
  operator int() {
    return 2;
  }
};
```

```cpp
// 좋은 아이디어
struct S {
  explicit operator int() {
    return 2;
  }
};
```

## 0번 규칙 고려하기

0번 규칙 (The Rule of Zero)는 새로운 형태의 소유권으로 생성하는 클래스가 아니고는 컴파일러가 제공할 수 있는 어떤 함수도 작성하지 않도록 명시하고 있습니다. (복사 생성자, 복사 할당 연산자, 무브 생성자, 무브 할당 연산자, 소멸자)

이 규칙의 목표는 컴파일러가 알아서 하도록 하는데 있습니다. 멤버 변수를 추가하더라도 최적의 버전을 컴파일러에서 자동으로 제공하기 때문입니다.

[원본 글](https://rmf.io/cxx11/rule-of-zero)이 그 배경을 설명하고 구현 기술에 대한 설명은 [이 글](http://www.nirfriedman.com/2015/06/27/cpp-rule-of-zero/)에서 대부분 다루고 있습니다.
