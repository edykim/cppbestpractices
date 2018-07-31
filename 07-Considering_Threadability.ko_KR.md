# 스레드 가능성 고려하기

## 전역 데이터 피하기

전역 데이터는 함수가 호출되는 사이에 의도하지 않은 파생 작업(side effect)으로 흘러갈 수 있으며 이런 코드를 병렬로 만드는 일을 어렵거나 불가능에 가깝습니다. 오늘 당장에는 병렬로 구동할 필요가 없는 코드라고 하더라도 미래에도 불가능하도록 작성할 이유는 전혀 없습니다.

### 정적

Besides being global data, statics are not always constructed and deconstructed as you would expect. This is particularly true in cross-platform environments. See for example, [this g++ bug](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=66830) regarding the order of destruction of shared static data loaded from dynamic modules.

전역 데이터로 만드는 것 외에도 정적(static) 또한 기대처럼 항상 생성되고 소멸되지 않습니다. 특히 크로스 플랫폼 환경에서는 그렇습니다. 예를 들면 [이 g++ 버그](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=66830)는 동적 모듈로 불려온 공유 정적 데이터의 소멸 순서에 인해 발생한 버그입니다.

### 공유 포인터

`std::shared_ptr` is "as good as a global" () because it allows multiple pieces of code to interact with the same data.

`std::shared_ptr`은 ["전역 만큼 좋은데"](http://stackoverflow.com/a/18803611/29975) 동일한 자료를 여러 코드 조각에서 접근할 수 있기 때문입니다.

### 싱글턴

싱글턴은 종족 정적과 `shared_ptr`로 함께 구현되거나 각각 구현되기도 합니다.

## 힙(heap) 조작 피하기

스레드 환경에서는 더 느립니다. 대부분의 경우에서 데이터를 복사하는 일이 더 빠릅니다. (무브와 같은 작업도 포함)

## Mutex and mutable go together (M&M rule, C++11)

## Mutex와 가변성은 함께 간다 (M&M 규칙, C++11)

멤버 변수에서는 뮤텍스(mutex)와 가변성을 함께 사용하는 것이 좋은 방식입니다. 다음 두 가지가 모두 적용됩니다.
* 가변적인 멤버 변수는 공유 변수로 추정되어 뮤텍스와 동기화 되어야 합니다. (또는 원자적으로 처리)
* 만약 멤버 변수 자체가 뮤텍스라면 가변적이어야 합니다. 내부적으로 상수 멤버 함수를 사용하는 것이 요구됩니다.

더 많은 정보는 [Herb Sutter의 다음 글](http://herbsutter.com/2013/05/24/gotw-6a-const-correctness-part-1-3/)에서 찾을 수 있습니다.

또한 반환값 `const &`와 [관련된 안정성 토론](04-Considering_Safety.ko_KR.md#주의깊게-반환-형식을-고려하기)을 확인합니다.
