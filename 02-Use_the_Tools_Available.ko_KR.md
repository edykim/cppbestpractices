# 쓸 수 있는 도구 사용하기

이런 도구를 실행하는 자동화 프레임워크는 개발 프로세스 초기부터 사용해야 합니다. 2-3개 정도 명령어를 사용해서 소스 코드를 받아, 빌드하고 테스트를 실행할 수 있어야 합니다. 테스트 실행이 한번 실행된 후로는 거의 완벽한 상태와 품질의 코드를 가질 수 있어야 합니다.

## 소스 관리

소스 관리는 어떤 소프트웨어 개발 프로젝트든 반드시 필요합니다. 아직 사용하지 않고 있다면 하나를 골라 사용하세요.

 * [GitHub](https://github.com/) - 무제한 공개 리포지터리를 허용하며 비공개 리포지터리는 비용을 지불합니다.
 * [Bitbucket](https://bitbucket.org/) - 5명까지 무료로 사용할 수 있는 비공개 리포지터리를 무제한으로 제공합니다.
 * [SourceForge](http://sourceforge.net/) - 오픈소스 호스팅만 제공합니다.
 * [GitLab](https://gitlab.com/) - 무제한 공개/비공개 리포지터리를 제공하며 제한 없는 CI 러너를 포함하여 무료입니다.
 * [Visual Studio Online](https://visualstudio.com) (http://www.visualstudio.com/what-is-visual-studio-online-vs) - 무제한 공개 리포지터리를 허용하며 비공개 리포지터리는 비용을 지불합니다. 리포지터리는 git과 TFVC를 사용할 수 있습니다. 추가적으로 이슈 추적과 프로젝트 계획하기 (SCRUM과 같은 여러 애자일 템플릿을 제공), 호스트 빌드 통합, Microsoft Visual Studio와의 통합 등을 제공합니다. Windows에서만 사용할 수 있습니다.

## 빌드 도구

산업 표준으로 널리 적용된 빌드 도구를 사용하세요. 이런 도구를 사용하면 이미 존재하는 것을 다시 발명하는 일을 막아줍니다. 스스로 발견한 것이든, 새 라이브러리를 연결하거나 제품을 패키징 하는 등의 작업이든 말입니다. 다음 도구를 확인해보세요.

 * [CMake](http://www.cmake.org/)
   * 고려: https://github.com/sakra/cotire/ 빌드 성능
   * 고려: https://github.com/toeb/cmakepp 사용성 증대
   * 활용: https://cmake.org/cmake/help/v3.6/command/target_compile_features.html C++ 표준 플래그
   * 고려: https://github.com/cheshirekow/cmake_format CMakeLists.txt 자동 양식
   * [더 읽기](10-Further_Reading.ko_KR.md)에서 CMake 모범 사례를 참고하세요
 * [Waf](https://waf.io/)
 * [FASTBuild](http://www.fastbuild.org/)
 * [Ninja](https://ninja-build.org/) - 대형 프로젝트에서 증분 빌드 시간을 향상하는데 큰 도움이 됩니다. CMake을 대상으로 사용할 수 있습니다.
 * [Bazel](http://bazel.io/) - 노트: MacOS와 Linux만 사용 가능
 * [gyp](https://chromium.googlesource.com/external/gyp/) - Chromium을 위한 Google의 빌드 도구
 * [maiken](https://github.com/Dekken/maiken) - Maven-esque 설정 스타일의 다중 플랫폼 빌드 도구
 * [Qt Build Suite](http://doc.qt.io/qbs/) - Qt에서 제공하는 다중 플랫폼 빌드 도구
 * [meson](http://mesonbuild.com/index.html) - 정말 빠르고 사용자 친화적인 오픈소스 빌드 시스템
 * [premake](https://premake.github.io/)

단순히 빌드 도구가 아니라 프로그래밍 언어인 점을 기억하세요. 좋고 깔끔한 빌드 스크립트를 유지할 수 있도록 노력하고 사용하는 도구에서 추천하는 사례를 따르기 바랍니다.

## 패키지 관리자

패키지 관리는 C++에서 중요한 주제입니다. 현재로는 명확한 승자가 없습니다. 패키지 관리자 사용을 고려하세요. 프로젝트의 의존성을 추적하는데 도움되며 프로젝트를 새로 시작하는 사람이 편리하게 사용할 수 있습니다.

 * [Conan](https://www.conan.io/) - C++용 다중 플랫폼 의존성 관리자
 * [hunter](https://github.com/ruslo/hunter) - CMake을 기반으로 한, C/C++용 다중 플랫폼 의존성 관리자
 * [C++ Archive Network (CPPAN)](https://cppan.org/) - C++용 다중 플랫폼 의존성 관리자
 * [qpm](https://www.qpm.io/) - Qt용 패키지 매니저 관리자
 * [build2](https://build2.org/) - cargo 방식 C++용 패키지 관리

## 지속적인 통합 (Continuous Integration)

빌드 도구를 골랐다면 지속적인 통합 환경을 구축해야 합니다.

지속적인 통합 (CI) 도구는 리포지터리에 변경점이 전달되면 소스 코드를 자동으로 빌드합니다. 비공개 호스트를 사용해서 빌드를 하거나 CI 호스트에서 빌드하는 것도 가능합니다.

 * [Travis CI](http://travis-ci.org)
   * C++와 함께 잘 동작
   * GitHub에 사용하는 것을 전제로 구성
   * GitHub 공개 리포지터리는 무료
 * [AppVeyor](http://www.appveyor.com/)
   * Windows, MSVC와 MinGW 지원
   * GitHub 공개 리포지터리는 무료
 * [Hudson CI](http://hudson-ci.org/) / [Jenkins CI](https://jenkins-ci.org/)
   * Java 어플리케이션 서버 필요
   * Windows, OS X, Linux 지원
   * 많은 플러그인으로 확장 가능
 * [TeamCity](https://www.jetbrains.com/teamcity) 
   * 오픈 소스 프로젝트용 무료 옵션 제공
 * [Decent CI](https://github.com/lefticus/decent_ci)
   * 간단한 ad-hoc 방식의 지속적인 통합을 제공
   * Windows, OS X, Linux 지원
   * [ChaiScript](http://chaiscript.com/ChaiScript-BuildResults/full_dashboard.html)에서 사용
 * [Visual Studio Online](https://visualstudio.com) (http://www.visualstudio.com/what-is-visual-studio-online-vs)
   * Visual Studio Online의 소스 리포지터리와 긴밀하게 동합되어 있음
   * MSBuild 사용 (Visual Studio의 빌드 엔진), Windows, OS X, Linux에서 사용 가능한 엔진
   * 호스트 빌드 에이전트 제공, 사용자 제공 빌드 에이전트도 사용 가능
   * Microsoft Visual Studio에서 제어 및 모니터링 가능
   * Microsoft Team Foundation Server로 온프로미스 설치 가능
 * [GitLab](https://gitlab.com)
   * 커스텀 Docker 이미지를 사용할 수 있기 때문에 C++에도 사용 가능
   * 공유된 CI 러너를 무료로 제공
   * 커버리지 분석 결과를 세세하게 처리 가능

만약 오픈소스며 GitHub에서 공개적으로 호스팅하는 프로젝트라면,

 * Travis Ci와 AppVeyor 통합을 지금 당장 켜고 오세요. 다녀올 때까지 여기서 기다리겠습니다. CMake 기반 C++ 어플리케이션에 활성화하는 간단한 예시는 이 링크에서 확인하세요. https://github.com/ChaiScript/ChaiScript/blob/master/.travis.yml
 * 아래 목록서 사루는 커버리지 도구 중 하나를 활성화합니다. (Codecov 또는 Coveralls)
 * [Coverity 스캔](https://scan.coverity.com)을 활성화합니다.

이 도구는 모두 무료며 상대적으로 쉽게 설정할 수 있습니다. 프로젝트를 지속적으로 빌드하고 테스트, 분석, 보고 받기까지 단 한 번의 설정으로 가능합니다. 게다가 무료입니다.

## 컴파일러

사용 가능하며 합당한 경고 옵션이라면 모두 사용하세요. 어떤 경고 옵션은 최적화가 켜졌을 때만 동작합니다. 또는 선택한 최적화 단계보다 높은 상황에서 더 잘 동작하기도 합니다. [`-Wnull-dereference`](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html#index-Wnull-dereference-367)를 GCC에서 사용하는 경우를 예로 들 수 있습니다.

플랫폼에서 사용할 수 있는, 가능한 한 많은 수의 컴파일러를 사용해야 합니다. 각 컴파일러가 구현한 표준은 조금씩 다릅니다. 여러 컴파일러에서 테스트하면 가장 이식 가능하며(portable) 안정적인 코드를 확보하는데 도움이 됩니다.

### GCC / Clang

`-Wall -Wextra -Wshadow -Wnon-virtual-dtor -pedantic`

 * `-Wall -Wextra` 합리적이고 표준
 * `-Wshadow` 변수 선언이 부모 컨텍스트를 따르는 경우에 사용자에게 경고
 * `-Wnon-virtual-dtor` 가상 함수가 있는 클래스에 가상 함수가 아닌 소멸자가 있는 경우 사용자에게 경고. 이 경고는 찾기 어려운 메모리 오류를 찾는 것을 도움.
 * `-Wold-style-cast` c 방식의 캐스트(cast)를 경고
 * `-Wcast-align` 잠재적 성능 문제가 있는 캐스트를 경고
 * `-Wunused` 사용되지 않은 모든 부분을 경고
 * `-Woverloaded-virtual` 가상 함수를 오버로드 (오버라이드 말고) 한 경우에 경고
 * `-Wpedantic` 비표준 C++를 사용하면 경고
 * `-Wconversion` 형식 변환(type conversion) 데이터를 유실하는 경우 경고
 * `-Wsign-conversion` sign 변환 경고
 * `-Wmisleading-indentation` 블럭이 존재하지 않는 곳에 블럭을 의미하는 들여쓰기가 있는 경우 경고
 * `-Wduplicated-cond` `if` / `else`에 중복되는 조건이 있으면 경고
 * `-Wduplicated-branches` `if` / `else`에 중복되는 코드가 있으면 경고
 * `-Wlogical-op` bitwise를 필요로 하는 곳에 논리 연산이 사용된 경우에 경고
 * `-Wnull-dereference` null 역참조가 감지된 경우 경고
 * `-Wuseless-cast` 동일한 형식으로 캐스팅을 수행한 경우 경고
 * `-Wdouble-promotion` `float` 형식이 암묵적으로 `double` 형식으로 사용된 경우 경고
 * `-Wformat=2` 형식 출력(예로 `printf`)에 보안 문제가 있는 경우 경고


`-Weverything`을 사용하고 Clang에 비활성화 하고 싶은 경고만 비활성화 해서 사용하는 방식도 고려하세요.

`-Weffc++` 경고 모드는 잡음이 지나치게 많을 수도 있지만 프로젝트에서 동작한다면 사용하길 바랍니다.

### MSVC

`/permissive-` - [강제 표준 규칙 적용](https://docs.microsoft.com/ko-kr/cpp/build/reference/permissive-standards-conformance).

`/W4 /W14640` - 이 플래그와 함께 다음 내용을 사용하는 것을 권합니다. (설명을 확인하세요.)

 * `/W4` 합당한 모든 경고
 * `/w14242` '식별자': 'type1'에서 'type1', 데이터 손실으로 변환
 * `/w14254` 'operator': 'type1:field_bits'에서 'type2:field_bits', 데이터 손실으로 변환
 * `/w14263` 'function': 멤버 함수가 기본 클래스 가상 멤버 함수를 재정의하지 않습니다.
 * `/w14265` 'classname': 클래스에 가상 함수가 있지만 소멸자는 가상이 아닙니다
 * `/w14287` 'operator': 서명 되지 않은 또는 음의 상수가 일치 하지 않습니다
 * `/we4289` 비표준 확장이 사용됨 : 'var' : for 루프에서 선언된 루프 제어 변수가 for 루프 범위 외부에서 사용되었습니다.
 * `/w14296` 'operator': 식이 항상 'boolean_value'입니다.
 * `/w14311` 'variable' : 'type1'에서 'type2'(으)로 포인터가 잘립니다.
 * `/w14545` 쉼표 앞의 식이 인수 목록이 없는 함수로 계산됩니다.
 * `/w14546` 쉼표 앞의 함수에 인수 목록이 없습니다.
 * `/w14547` 'operator': 쉼표 효과가 없습니다; 앞의 연산자 파생 작업이 있는 연산자 여야 합니다.
 * `/w14549` 'operator': 쉼표 효과가 없습니다; 앞의 연산자 'operator' 사용 하려고 했습니까?
 * `/w14555` 식이 효과가 없습니다. 파생 작업이 있는 식이어야 합니다.
 * `/w14619` pragma 경고: 없는 경고 번호 'number'
 * `/w14640` 'instance': 지역 정적 개체를 생성할 스레드로부터 안전하게 보호 되지 않습니다.
 * `/w14826` 'Type1'에서 'type_2'으로 변환의 부호가 확장 됩니다. 이렇게 하면 예기치 않은 런타임 동작이 발생할 수 있습니다.
 * `/w14905` 와이드 문자열 리터럴을 'LPSTR'로 캐스팅했습니다.
 * `/w14906` 문자열 리터럴을 'LPWSTR'로 캐스팅했습니다.
 * `/w14928` 복사 초기화가 잘못되었습니다. 사용자 정의 변환이 암시적으로 두 번 이상 적용되었습니다.

다음은 추천하지 않습니다.

 * `/Wall` - 표준 라이브러리도 포함해, 모든 불러온 파일의 경고를 표시합니다. 너무 많은 경고를 만들어내기 때문에 그다지 유용하지 않습니다.

### 일반

처음부터 매우 엄격한 경고 설정을 두고 시작합니다. 프로젝트를 진행한 후에 경고 수준을 높이는 방식은 매우 고통스러운 방식입니다.

*경고를 오류로 처리하기* 설정을 사용하는 것을 고려하세요. MSVC에서는 `/Wx`, GCC / Clang에서는 `-Werror`로 설정할 수 있습니다.

## LLVM 기반 도구

LLVM 기반 도구는 컴파일 명령 데이터베이스를 출력할 수 있는 빌드 시스템(예로 cmake)과 함께 잘 동작합니다. 예를 들면 다음과 같습니다.

```
$ cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON .
```

만약 이런 빌드 시스템을 사용하고 있지 않다면 [Build EAR](https://github.com/rizsotto/Bear)을 고려할 수 있습니다. 이 도구를 사용하면 빌드 시스템에 연결해서 컴파일 명령어 데이터베이스를 생성할 수 있습니다.

또한 CMake은 [일반 컴파일 과정](https://cmake.org/cmake/help/latest/prop_tgt/LANG_CLANG_TIDY.html)에서 `clang-tidy`라는 내장 기능을 지원합니다.

 * [include-what-you-use](https://github.com/include-what-you-use), [예제 결과](https://github.com/ChaiScript/ChaiScript/commit/c0bf6ee99dac14a19530179874f6c95255fde173)
 * [clang-modernize](http://clang.llvm.org/extra/clang-modernize.html), [예제 결과](https://github.com/ChaiScript/ChaiScript/commit/6eab8ddfe154a4ebbe956a5165b390ee700fae1b)
 * [clang-check](http://clang.llvm.org/docs/ClangCheck.html)
 * [clang-tidy](http://clang.llvm.org/extra/clang-tidy.html)

## 정적 분석기

정적 분석기의 가장 큰 장점은 자동 빌드 시스템의 일부로 구동할 수 있다는 점입니다. Cppcheck과 clang은 이 용도로 무료로 사용할 수 있습니다.

### Coverity 스캔 


[Coverity](https://scan.coverity.com/)은 (오픈소스 대상으로) 무료 정적 분석 툴킷을 제공합니다. [Travis CI](http://travis-ci.org)와 [AppVeyor](http://www.appveyor.com/)에 통합하면 매번 커밋에 구동할 수 있습니다.

### PVS-Studio

[PVS-Studio](http://www.viva64.com/en/pvs-studio/)은 프로그램의 소스 코드에서 버그를 찾는 도구로 C, C++와 C#으로 작성된 프로그램에 사용할 수 있습니다. 개인적 학술 프로젝트나 오픈소스 비상용 프로젝트, 개인 개발자의 독립 프로젝트에 무료로 사용할 수 있습니다. Windows와 Linux 환경을 지원합니다.

### Cppcheck

[Cppcheck](http://cppcheck.sourceforge.net/)은 자유 소프트웨어이며 오픈 소스입니다. 감지를 제대로 못하는 경우가 없으며 좋은 결과를 냅니다. 그러므로 `--enable=all` 플래그로 모든 오류를 켜고 사용합니다.

노트:

 * 제대로 동작하기 위해서는 헤더를 위해 제대로 작성된 경로가 필요합니다. 사용하기 전에 `--check-config`을 통과하는지 확인하기 바랍니다.
 * 사용하지 않는 헤더를 찾는 기능은 `-j`가 1 이상인 경우에는 동작하지 않습니다.
 * 코드에 `#ifdef`가 많고 모든 경우를 확인해야 한다면 `--force` 플래그를 추가해서 사용합니다.
 
### cppclean

[cppclean](https://github.com/myint/cppclean)은 오픈소스 정적 분석기로 대규모의 코드로 느리게 개발하고 있는 C++ 소스에서 문제 찾는 것에 집중한 도구입니다.

### CppDepend
 
[CppDepend](https://www.cppdepend.com/)는 복잡한 C/C++ 코드 기반 관리를 단순화할 수 있도록 코드 의존성을 분석하고 시각화합니다. 그 과정은 디자인 규칙을 정하고 영향을 분석하고 코드의 여러 버전을 분석하는 방식으로 진행합니다. OSS 기여자는 무료로 사용할 수 있습니다.

### Clang 정적 분석기

Clang에 포함된 분석기는 기본 설정이 각 플랫폼에 적절하게 잘 설정되어 있습니다. 이 도구는 [CMake에서 직접](http://garykramlich.blogspot.com/2011/10/using-scan-build-from-clang-with-cmake.html) 사용하는 것도 가능합니다. 이 도구는 [LLVM 기반 도구](#llvm-기반-도구)에서 clang-check이나 clang-tidy를 통해 호출하는 것도 가능합니다.

또한 clang의 정적 분석의 프론트엔드로 [CodeChecker](https://github.com/Ericsson/CodeChecker)를 사용할 수 있습니다.

`clang-tidy`는 Visual Studio에서도 [Clang 파워 도구](https://caphyon.github.io/clang-power-tools/) 확장을 통해 사용할 수 있습니다.

### MSVC 정적 분석기

이 정적 분석기는 `/analyze` [명령행 옵션](http://msdn.microsoft.com/en-us/library/ms173498.aspx)으로 활성화 할 수 있습니다. 지금은 기본 설정에 따라 사용하기로 합니다.

### Flint / Flint++

[Flint](https://github.com/facebook/flint)와 [Flint++](https://github.com/L2Program/FlintPlusPlus)는 페이스북의 코딩 표준에 맞춰 C++ 코드를 분석하는 린터(linter)입니다.

### OCLint

[OCLint](http://oclint.org/)는 무료, 자유, 오픈소스 정적 코드 분석 도구로 C++ 코드의 품질을 다양한 방식으로 향상하도록 돕습니다.

### ReSharper C++ / CLion

둘 다 [JetBrains](https://www.jetbrains.com/cpp/)에서 제공하는 도구입니다. 어느 정도의 정적 분석과 함께 더 나은 방식으로 작성할 수 있는 일반적인 요소를 자동으로 고쳐주는 기능을 제공합니다. 오픈소스 프로젝트를 위한 무료 라이센스도 제공하고 있습니다.

### Cevelop 

[Cevelop](https://www.cevelop.com/) IDE는 이클립스 기반으로 다양한 정적 분석과 리팩토링 / 코드 수정 도구를 제공합니다. 예를 들면 C++의 `constexprs`로 매크로를 대체하거나 네임스페이스를 리팩토링 (추출/인라인 `using`, 이름 적합성), C++ 11의 유니폼 초기화 문법으로 리팩토링 하는 등의 기능을 제공합니다. Cevelop은 무료로 사용할 수 있습니다.

### Qt Creator

Qt Creator는 clang 정적 분석기에 연결할 수 있습니다.

### clazy

[clazy](https://github.com/KDE/clazy)는 clang 기반 도구로 Qt 사용을 분석할 때 사용할 수 있습니다.

## 런타임 확인 도구

### 코드 커버리지 분석

커버리지 분석 도구는 테스트를 구동할 때 어플리케이션 전체가 테스트 되었는지 확인하는 용도로 사용할 수 있습니다. 아쉽게도 커버리지 분석은 컴파일 최적화를 비활성화 했을 때만 사용할 수 있습니다. 이 설정을 끄게 되면 테스트 실행 시간이 극단적으로 길어질 가능성이 있습니다.

 * [Codecov](https://codecov.io/)
   * Travis CI와 AppVeyor 통합 가능
   * 오픈소스 프로젝트에 무료
 * [Coveralls](https://coveralls.io/)
   * Travis CI와 AppVeyor 통합 가능
   * 오픈소스 프로젝트에 무료
 * [LCOV](http://ltp.sourceforge.net/coverage/lcov.php)
   * 설정 폭이 넓음
 * [Gcovr](http://gcovr.com/)
 * [kcov](http://simonkagstrom.github.io/kcov/index.html)
   * codecov와 coveralls에 통합 가능
   * 디버그 심볼과 함께 동작하는 방식으로 별도의 컴파일 플래그 없이 코드 커버리지 보고를 수행
 * [OpenCppCoverage](https://github.com/OpenCppCoverage/OpenCppCoverage) - 오픈소스 커버리지 보고 도구 (Windows)


### Valgrind

[Valgrind](http://www.valgrind.org/)는 런타임 코드 분석기로 메모리 누수, 경쟁 상태(race conditions), 그 외 비슷한 문제를 검사하는데 사용할 수 있습니다. 다양한 Unix 플랫폼에서 사용할 수 있습니다.

### Dr Memory

Valgrind와 유사한 도구입니다. http://www.drmemory.org

### GCC / Clang Sanitizers

Valgrind와 같은 기능을 많이 제공하고 있지만 이 도구는 컴파일러에 내장되어 있습니다. 쉽게 사용할 수 있으며 무엇이 문제인지 리포트를 제공합니다.

 * AddressSanitizer
 * MemorySanitizer
 * ThreadSanitizer
 * UndefinedBehaviorSanitizer

이 도구에서 제공하는 옵션(런타임 옵션 포함)에 [유의합니다](https://kristerw.blogspot.com/2018/06/useful-gcc-address-sanitizer-checks-not.html).

### 퍼지 분석기 (Fuzzy Analyzers)


프로젝트가 사용자 정의 입력을 받는다면 퍼지 입력 실험 도구를 사용하는 것을 고려하기 바랍니다.

이 도구는 커버리지 리포트를 사용해 새로운 코드 실행 경로를 찾고 코드에 다른 방식의 입력을 시도합니다. 충돌, 응답 없음을 찾고 참으로 고려하지 않은 값이 입력으로 허용되는지 찾아냅니다.

 * [american fuzzy lop](http://lcamtuf.coredump.cx/afl/)
 * [LibFuzzer](http://llvm.org/docs/LibFuzzer.html)
 * [KLEE](http://klee.github.io/) - 퍼지 개별 함수를 사용할 수 있음

### 제어 흐름 가드

MSVC의 [제어 흐름 가드(Control Flow Guard)](https://msdn.microsoft.com/ko-kr/library/windows/desktop/mt637065%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)는 고성능의 런타임 보안 검사를 추가합니다.

### STL 구현 확인

 * `_GLIBCXX_DEBUG`에 대한 GCC의 구현과 libstdc++ 구현을 확인합니다. [Krister의 블로그 글](https://kristerw.blogspot.se/2018/03/detecting-incorrect-c-stl-usage.html)을 참고하세요.

### 힙 프로파일링

 * [Memoro](https://epfl-vlsc.github.io/memoro/) - 상세한 힙 프로파일 도구

## 경고 무시하기

컴파일러나 분석 도구의 경고가 잘못되거나 피할 수 없는 경우로 팀에서 논의되었다면 코드의 해당 지역에 특정 오류 검사를 끄는 방법을 선택할 수 있습니다.

해당 영역의 코드 경고를 끄면서 다른 부분에 영향을 주지 않도록 주의합니다. 코드 경고를 끄는 것이 [다른 코드에 흘러가는 일](http://www.forwardscattering.org/post/48)을 바라지는 않을 것입니다.

## 테스트하기

위에서 언급한 CMake는 테스트를 실행하기 위한 내장 프레임워크가 존재합니다. 어떤 빌드 시스템을 사용하든 내장된 테스트를 구동하는 방법을 확인하기 바랍니다.

테스트를 실행하는데 추가 지원이 필요하다면 다음 라이브러리를 고려합니다. 테스트를 조직하는데 도움이 됩니다.

 * [Google Test](https://github.com/google/googletest)
 * [Catch](https://github.com/philsquared/Catch)
 * [CppUTest](https://github.com/cpputest/cpputest)
 * [Boost.Test](http://www.boost.org/doc/libs/release/libs/test/)

### 단위 테스트 (Unit Tests)

Unit tests are for small chunks of code, individual functions which can be tested standalone.

단위 테스트는 작은 양의 코드와 개별적인 함수를 대상으로 하며 단독으로 수행하는 테스트입니다.

### 통합 테스트 (Integration Tests)

모든 기능 및 버그 픽스에 대한 테스트도 가능해야 합니다. [코드 커버리지 분석](#코드-커버리지-분석)을 참고하세요. 이런 테스트는 단위 테스트보다 상위 수준에서 이뤄집니다. 하지만 여전히 각 범위는 개별 기능에 한정해야 합니다.

### 부정(Negative) 테스트 하기

오류 처리가 제대로 발생하고 의도대로 동작하는지 확인하는 것을 잊지 말기 바랍니다. 100% 코드 커버리지를 목표로 한다면 이런 테스트의 용도는 더 명확해집니다.

## 디버깅

### uftrace

[uftrace](https://github.com/namhyung/uftrace)는 프로그램 실행에 대한 함수 호출 그래프를 생성하는데 사용할 수 있습니다.

### rr

[rr](http://rr-project.org/)은 무료 (오픈소스) 리버스 디버거로 C++를 지원합니다.

## 그 외 도구

### Lizard

[Lizard](http://www.lizard.ws/)는 C++ 코드를 대상으로 복잡도 분석을 수행하며 아주 간단한 인터페이스로 기능을 제공합니다.

### Metrix++

[Metrix++](http://metrixplusplus.sourceforge.net/)는 코드에서 가장 복잡한 부분이 어디인지 분석하고 보고합니다. 컴파일러가 잘 이해하고 최적화 할 수 있도록 코드 복잡도를 낮추는데 도움을 줍니다.

### ABI Compliance Checker

[ABI Compliance Checker](http://ispras.linuxbase.org/index.php/ABI_compliance_checker) (ACC)는 두 라이브러리 버전을 분석해서 API와 C++ ABI 변경점에 따라 세부 호환성 보고서를 생성합니다. 이 도구는 라이브러리 개발자가 하위 호환성을 확보하고 의도하지 않은 호환 불가 코드를 생성하지 않도록 진단할 수 있습니다.

### CNCC 

[Customizable Naming Convention Checker](https://github.com/mapbox/cncc)는 코드 내에서 명명 규칙을 따르지 않은 식별자를 찾아 보고합니다.

### ClangFormat

[ClangFormat](http://clang.llvm.org/docs/ClangFormat.html)은 조직의 규칙에 따라 코드를 제대로 작성했는지 자동으로 확인하고 고치는 도구입니다. clang-format을 활용하기 위한 [연재글](https://engineering.mongodb.com/post/succeeding-with-clangformat-part-1-pitfalls-and-planning/)을 확인하세요.

### SourceMeter 

[SourceMeter](https://www.sourcemeter.com/)는 코드의 다양한 척도를 확인할 수 있는 무료 버전을 제공하며 cppcheck 내에서도 호출할 수 있는 기능을 제공합니다.

### Bloaty McBloatface

[Bloaty McBloatface](https://github.com/google/bloaty)는 바이너리 크기 분석기/프로파일러로 unix-like 플랫폼에서 동작합니다.
