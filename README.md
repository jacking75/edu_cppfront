# cppfront 학습 저장소
  
아래는 공식 문서에 있는 내용을 번역(기계번역) 정리한 것이다.  
  
# cppfront란 무엇인가요?

출처: [https://hsutter.github.io/cppfront/](https://hsutter.github.io/cppfront/)

[**Cppfront**](https://github.com/hsutter/cppfront)는 Cpp2 구문을 오늘날의 Cpp1 구문으로 컴파일하는 컴파일러입니다. 소스 파일의 이름을 `.cpp`에서 `.cpp2`로 바꾸고 [빌드 단계를 추가하기만 하면 모든 C++20 이상 컴파일러와 기존의 모든 C++ 도구(디버거, 빌드 시스템, 새니타이저 등)에서 Cpp2 문법을 시험할 수 있으며, 결과는 C++20 이상 컴파일러와 모든 C++ 도구에서 작동하기만 하면 됩니다.](https://hsutter.github.io/cppfront/#adding-cppfront-in-your-ide-build-system)

이는 의도적으로 최초의 C++ 컴파일러인 [**cfront**](https://en.wikipedia.org/wiki/Cfront)를 사용한 Bjarne Stroustrup의 현명한 접근 방식을 따르고 있습니다: 1980년대와 1990년대에 스트로스트럽은 C++를 순수 C로 변환하기 위해 cfront를 만들었고, 마찬가지로 동일한 소스 파일에서 C++를 C와 인터리브할 수 있도록 하고, C++가 래핑/마샬링/썽킹 없이도 항상 모든 C 코드를 호출할 수 있도록 보장했습니다. 스트로스트럽은 순수 C++ 컴파일러를 제공함으로써 이미 존재하는 C 에코시스템과의 완벽한 호환성을 보장했고, C++를 C로 먼저 변환하는 빌드 단계만 추가하면 기존 C 프로젝트에서 C++ 코드를 쉽게 사용해 볼 수 있도록 하여 사람들이 기존 C 도구와 함께 작동하는 결과물을 만들 수 있도록 했습니다.

# cppfront는 어떻게 구하고 빌드하나요?

cppfront의 전체 소스 코드는 [**Cppfront GitHub 리포지토리**](https://github.com/hsutter/cppfront)에서 확인할 수 있습니다.

Cppfront는 최신 C++ 컴파일러로 빌드합니다. `/cppfront/source` 디렉토리로 이동하여 다음 중 하나를 실행합니다:

**MSVC 빌드 지침(Visual Studio 2019 버전 16.11 이상)**  
cl cppfront.cpp \-std:c++20 \-EHsc

**GCC 빌드 지침(GCC 10 이상)**  
g++ cppfront.cpp std=c+++20 \-o cppfront

**클랑 빌드 지침(클랑 12 이상)**  
clang++ cppfront.cpp \-std=c+++20 \-o cppfront

끝입니다\!

Visual Studio 에 있는 콘솔 애플리케이션으로 빌드한다.  
![][image1]

# Hello, world\!

출처: [https://hsutter.github.io/cppfront/welcome/hello-world/](https://hsutter.github.io/cppfront/welcome/hello-world/) 

![][image2]

## `hello.cpp2` 프로그램

다음은 `Hello, world!`를 출력하는 일반적인 한 줄짜리 스타터 프로그램입니다. 이 프로그램은 완전한 프로그램이며 `#include` 가 필요하지 않습니다:

**hello.cpp2 \- 한 줄에**

| main: () \= std::cout \<\< "Hello, world\!\\n"; |
| :---- |

하지만 몇 가지를 보여주기 위해 조금 더 추가해 보겠습니다:

**hello.cpp2 \- 조금 더 흥미롭습니다.**

| main: () \= {    words: std::vector \= ( "Alice", "Bob" );    hello( words\[0\] );    hello( words\[1\] );}hello: (msg: std::string\_view) \=    std::cout \<\< "Hello, (msg)$\!\\n"; |
| :---- |

이 짧은 프로그램 코드는 이미 몇 가지 Cpp2 필수 요소를 보여줍니다.

**일관된 문맥 없는 구문** Cpp2는 주어진 사물의 철자를 표기하는 하나의 일반적인 방법이 모든 곳에서 일관되게 작동하도록 설계 되었습니다. 모든 Cpp2 타입/함수/객체/네임스페이스는 명확하고 컨텍스트가 없는 [선언 구문](https://hsutter.github.io/cppfront/cpp2/declarations/) **"*name : kind* `을 사용하여 작성됩니다:`** `:`는 **"is a,"**로 발음되고 `=` 는 **"정의됨."**으로 발음됩니다.

* `main` 은 인수를 받지 않고 아무것도 반환하지 않는 함수이며, 표시된 코드 본문이 정의되어 있습니다다.  
* `words` **는** `std::vector` 이며, `"Alice"` 및 `"Bob"`로 추기화 되어 있습니다.  
* `hello` 은 `std::string_view` 로 읽기만 하고 아무것도 반환하지 않는 함수이며, 일반적인 C++ 방식으로 문자열을 `cout` 에 인쇄하는 코드로 정의되어 있습니다.

모든 문법은 문맥에 구애받지 않습니다. 특히 코드를 읽는 사람과 컴파일러는 구문 분석 방법을 파악하기 위해 이름 조회를 할 필요가 없습니다. Cpp2에는 ["vexing 파싱"](https://en.wikipedia.org/wiki/Most_vexing_parse)이 전혀 없습니다. 자세한 내용은 [디자인 참고 사항: 모호하지 않은 구문 분석](https://github.com/hsutter/cppfront/wiki/Design-note%3A-Unambiguous-parsing) 을 참조하세요.

**기본적으로 간단하고 안전하며 효율적입니다.** Cpp2에는 컨트랙트(C++26 컨트랙트 초안 추적), 패턴 매칭, 문자열 보간, 마지막 사용에서 자동 이동 등의 기능이 있습니다.

* `words`를 선언하면 **"CTAD"**(C++의 일반적인 [생성자 템플릿 인수 추론](https://en.cppreference.com/w/cpp/language/class_template_argument_deduction))을 사용하여 `vector`에서 요소의 타입을 추론할 수 있습니다.  
* `words[0]` 및 `words[1]`는 **bounds-checked by default** 입니다. Cpp2 코드에서 일반 `std::vector` 첨자 액세스는 선호하는 표준 라이브러리로 업그레이드할 필요 없이 기본적으로 안전하게 경계가 검사되며, 이는 `std::size()` 및 `std::begin()`가 임의 액세스 반복기를 반환하는 경우, 이미 보유하고 있는 정수 인덱싱 컨테이너 타입을 포함하여 `std::size()` 및 `std::ssize((아직 제공되지 않은 경우)`를 쉽게 제공할 수 있는 모든 인하우스 정수로 인덱싱된 컨테이너 타입이 포함되어 있습니다.  
* `hello`는 **string 보간**을 사용하여 `"Hello, (msg)$!"를 쓸 수 있습니다.\n"` 대신 `"Hello, " << msg << "!\n"`. 문자열 보간은 [표준 C++ 형식 사양](https://en.cppreference.com/w/cpp/utility/format/spec)도 지원하므로 아이오스트림 매니퓰레이터가 필요하지 않습니다.

**일반성 \+ 기본값을 통한 단순성** Cpp2가 단순성을 제공하는 주요 방법은 주어진 것에 대해 하나의 강력한 일반 구문(예: 하나의 함수 정의 구문)만 제공하고, 현재 사용하지 않는 부분(예: 기본값에 만족하는 경우)은 생략할 수 있도록 설계하는 것입니다. 위의 기본값 중 일부는 이미 사용하고 있습니다:

* 이 두 함수처럼 아무것도 반환하지 않는 함수에 대해서는 `-> void` 반환 타입 작성을 생략할 수 있습니다.  
* `{` `}`는 단일 문 함수 본문에서 `hello`와 같이 생략할 수 있습니다.  
* `in` 매개변수에서 `msg`를 생략할 수 있습니다. Cpp2에는 매개변수를 전달하는 6가지 방법이 있습니다: 가장 일반적인 것은 읽기 전용의 `in`(기본값이므로 생략할 수 있음)와 읽기-쓰기용의 `inout` 입니다. 나머지는 `copy`, `out`, `move`, `forward` 입니다.

자세한 내용은 [디자인 참고: 기본값은 같은 말을 하는 한 가지 방법](https://github.com/hsutter/cppfront/wiki/Design-note%3A-Defaults-are-one-way-to-say-the-same-thing)를 참조하세요.

**기본적으로는 순서 독립적입니다.** `main` 이 나중에 정의된 `hello` 라는 것을 알아차리셨나요? Cpp2 코드는 기본적으로 순서에 독립적이며 정방향 선언이 없습니다.

**원활한 호환성 및 상호 운용.** `std::cout` 및 `std::operactor <<` 및 `std::string_view`를 평소처럼 직접 입력할 수 있습니다. Cpp2 코드는 래핑/마샬링/썽킹 없이 일반적인 직접 호출을 사용하여 표준 라이브러리를 포함한 모든 C++ 코드 또는 라이브러리와 함께 작동합니다.

**C++ 표준 라이브러리는 항상 사용할 수 있습니다.** `#include <iostream>` 또는 `import std; 이 필요하지 않습니다`. 소스 파일에 syntax-2 코드만 포함되어 있고 cppfront의 `-p`(`-pure-cpp2` 의 약어)를 사용하여 컴파일하거나 `-im`(-import-std의 줄임말)를 사용하는 경우. Cppfront는 ISO C++ 위원회에서 C++26 작업 초안에 투표하는 즉시 C++23 및 최신 C++26 라이브러리 추가 사항과 호환되도록 정기적으로 업데이트 되므로 새로운 표준(또는 최첨단 표준 초안\!) C++ 라이브러리 기능이 포함된 C++ 구현이 있는 경우 Cpp2 코드에서 이를 완전히 사용할 수 있습니다.

## 빌드 `hello.cpp2`

이제 `cppfront`를 사용하여 `hello.cpp2` 를 표준 C++ 파일인 `hello.cpp`로 컴파일합니다:

**cppfront를 호출하여 hello.cpp 생성**

| cppfront hello.cpp2 \-p |
| :---- |

결과는 다음과 같은 일반 C++ 파일입니다: 

| \#define CPP2\_IMPORT\_STD          Yes\#include "cpp2util.h"auto main() \-\> int;auto hello(cpp2::in\<std::string\_view\> msg) \-\> void;auto main() \-\> int{    std::vector words {"Alice", "Bob"};    hello(CPP2\_ASSERT\_IN\_BOUNDS\_LITERAL(words, 0));    hello(CPP2\_ASSERT\_IN\_BOUNDS\_LITERAL(std::move(words), 1));}auto hello(cpp2::in\<std::string\_view\> msg) \-\> void {    std::cout \<\< ("Hello, " \+ cpp2::to\_string(msg) \+ "\!\\n");  }  |
| :---- |

여기에서 Cpp2의 기능 작동 방식을 자세히 살펴볼 수 있습니다.

**방법: 일관된 컨텍스트 프리 구문**

* **컴파일된 모든 줄은 이식 가능한 C++20 코드**로 2019년 이후에 출시된 거의 모든 C++ 컴파일러로 빌드할 수 있습니다. Cpp2의 컨텍스트 프리 구문은 오늘날의 Cpp1 구문으로 바로 변환됩니다. 컨텍스트 민감성이나 모호성 때문에 씨름할 필요 없이 더 간단한 Cpp2 구문으로 C++ 타입/함수/객체를 작성하고 읽을 수 있으며, 여전히 모두 일반적인 타입/함수/객체일 뿐입니다.

**방법: 기본적으로 간단하고 안전하며 효율적입니다.**

* **라인 9: CTAD**는 이미 CTAD를 지원하는 일반 C++ 코드로 변환되기 때문에 작동합니다.  
* **10-11 라인: `words[0]` 및 `words[1]`는 기본적으로 call site에 비침입적으로 표시됩니다. 비침입적이기 때문에 안전한 Cpp2 코드에서 `std::size` 및 `std::ssize`를 인식하는 모든 기존 컨테이너 타입과 원활하게 작동합니다.**  
* **라인 11: 마지막 사용에서 자동 move**는 `words`의 마지막 사용이 r 값에 최적화된 것으로 전달되는 경우 자동으로 복사되지 않도록 보장합니다.  
* **라인 15: 문자열 보간**은 `msg`의 현재 값의 문자열 캡처를 `cpp2::to_string`를 통해 수행합니다. 이는 `std::to_string`를 사용하며, 추가 타입(예: `bool`)에 대해서도 작동합니다. `false` 및 `true` 대신 `0` 및 `1`를 인쇄하도록 설정할 수 있습니다. `std::boolalpha` 를 사용하는 것을 기억할 필요 없이).

**방법: 일반성 \+ 기본값을 통한 단순성**

* **라인 7: `in` 매개변수**는 `cpp2::in<> 를 사용하여 구현됩니다.` 안전하고 적절한 경우 `const` 값으로 전달하고, 그렇지 않으면 `const&`로 표시되므로 직접 선택할 필요가 없습니다.

**방법: 기본적으로 순서 독립적입니다.**

* **5라인과 7라인: 순서 독립성**은 cppfront가 모든 타입 및 함수 전달 선언을 생성하므로 사용자가 직접 생성할 필요가 없기 때문에 발생합니다. 그렇기 때문에 `main`는 `hello`를 호출하면 둘 다 포워드 선언이므로 서로를 볼 수 있습니다.

**방법: 원활한 호환성 및 상호 운용**

* **라인 9-11 및 15: 기존 C++ 코드에 대한 일반 직접 호출**이므로 래핑/마샬링/썽킹이 필요 없습니다.

**How: C++ 표준 라이브러리를 항상 사용할 수 있습니다.**

* **라인 1 및 3: `std::`가 사용 가능**합니다. 왜냐하면 cppfront가 `-p`로 호출되었기 때문입니다, (`-im`의 줄임말) 또는 `-in`(아직 모듈을 지원하지 않는 컴파일러의 경우 `-include-std`의 줄임말) 중 하나를 의미할 수 있습니다. 생성된 코드는 `cpp2util.h`에 전체 표준 라이브러리를 모듈로 가져오라고 지시합니다(또는 모듈을 사용할 수 없는 경우 헤더를 통해 동등한 작업을 수행합니다).

## 최신 C++ 컴파일러로 `hello.cpp` 빌드 및 실행하기

마지막으로, 자주 사용하는 C++20 컴파일러를 사용하여 `hello.cpp`를 빌드하면 됩니다. 여기서 `CPPFRONT_INCLUDE`는 `/cppfront/include`에 대한 경로가 됩니다:

**MSVC(Visual Studio 2019 버전 16.11 이상)**

| \> cl hello.cpp \-std:c++20 \-EHsc \-I CPPFRONT\_INCLUDE\> hello.exeHello, world\! |
| :---- |

**GCC(GCC 10 이상)**

| $ g++ hello.cpp \-std=c++20 \-ICPPFRONT\_INCLUDE \-o hello$ ./hello.exeHello, world\! |
| :---- |

**Clang(Clang 12 이상)**

| $ clang++ hello.cpp \-std=c++20 \-ICPPFRONT\_INCLUDE \-o hello$ ./hello.exeHello, world\! |
| :---- |

### 

# [기존 C++ 프로젝트에 cppfront 추가하기](https://hsutter.github.io/cppfront/welcome/integration/)

기존 C++ 프로젝트에서 Cpp2 구문을 사용해 보려면 빌드 단계를 추가하여 Cpp2를 Cpp1 구문으로 변환하기만 하면 됩니다:

* `.cpp` 파일을 같은 이름으로 복사하고 확장자를 `.cpp2`로 지정합니다.  
* `.cpp2` 파일을 프로젝트에 추가하고 `.cpp`가 C++20 모드인지 확인합니다.  
* 사용자 지정 빌드 도구를 사용하여 해당 파일을 빌드하도록 IDE에 지시하여 cppfront를 호출합니다.

그게 다입니다... 결과는 모든 C++20 이상 컴파일러와 기존의 모든 C++ 도구(디버거, 빌드 시스템, 새니타이저 등)에서 작동합니다. IDE 빌드는 오류 메시지가 있는 `.cpp2` 파일 소스 위치를 선택하면 되고, 디버거는 `.cpp2` 파일을 살펴보기만 하면 됩니다.

다음은 Visual Studio를 예로 들었지만 Xcode, Qt Creator, CMake 및 기타 IDE에서도 동일한 작업을 수행할 수 있습니다.

## 1\. 프로젝트에 .cpp2 파일을 추가하고, .cpp가 C++20 모드인지 확인합니다.

Visual Studio의 경우: 솔루션 탐색기에서 소스 파일을 마우스 오른쪽 버튼으로 클릭하고 추가를 선택하여 프로젝트에 파일을 추가합니다.

![][image3]

또한 솔루션 탐색기에서 `.cpp` 파일 속성을 마우스 오른쪽 버튼으로 클릭하고 C++20(또는 C++ 최신) 모드로 설정되어 있는지 확인합니다.

![][image4]

## 2\. 프로젝트 시스템에 사용자 지정 빌드 도구를 사용하여 해당 파일을 빌드하도록 지시하여 cppfront를 호출하고 포함 경로에 cppfront/include를 추가합니다.

Visual Studio의 경우: 솔루션 탐색기에서 `.cpp2` 파일을 마우스 오른쪽 버튼으로 클릭하고 속성을 선택한 다음 사용자 지정 빌드 툴을 추가합니다. 또한 사용자 지정 빌드 도구가 빌드 종속성에 대해 알 수 있도록 `.cpp` 파일을 생성한다고 알려야 한다는 점을 잊지 마세요:

![][image5]

마지막으로 `/cppfront/include` 디렉터리를 `INCLUDE` 경로에 넣습니다. 솔루션 탐색기에서 앱을 마우스 오른쪽 버튼으로 클릭하고 속성을 선택한 다음 VC++ 디렉터리 \> 포함 디렉터리에 추가합니다:

![][image6]

## 

## 이게 다입니다: 오류 메시지 출력, 디버거, 비주얼라이저 및 기타 도구가 작동해야 합니다.

이 정도면 빌드를 활성화하기에 충분하며, 나머지는 cppfront가 생성한 `.cpp` 파일에서 IDE가 선택하면 됩니다:

* **`파일명(줄, 색)` 형식의 cppfront 오류 메시지** 대부분의 C++ IDE는 이를 인식하며 일반적으로 컴파일러 오류 출력이 일반적으로 나타나는 곳에 진단 출력을 자동으로 병합합니다. 사용 중인 IDE가 `파일명:줄:콜론`를 선호하는 경우, cppfront `-format-colon-errors` 명령줄 옵션을 사용하면 됩니다.  
* **`#line` 지시어는 생성된 `.cpp` 파일에서 cppfront가 출력합니다.** 대부분의 C++ 디버거는 이를 인식하여 `.cpp2` 파일로 이동하는 것을 알 수 있습니다. `#line` 배출은 기본적으로 켜져 있지만 `-c`(`-clean-cpp1`의 줄임말)를 선택하면 이 기능이 억제되고 대신 생성된 C++ 코드를 디버거가 단계적으로 처리합니다. 디버거가 파일을 찾을 수 없는 경우 `-line-paths` 지시어에 상대 경로가 아닌 절대 경로를 `#line` 사용해야 할 수 있습니다  
* 구문과 관계없이 모든 타입/함수/객체/네임스페이스 등은 여전히 일반적인 C++ 타입/함수/객체/네임스페이스 등에 불과합니다. 대부분의 C++ 디버거 비주얼라이저는 프로그램에서 사용하는 모든 `std::` 타입에 대해 기본 제공 비주얼라이저(평소처럼 직접 사용되므로)와 자체 타입 또는 널리 사용되는 라이브러리 타입에 대해 이미 작성한 사용자 지정 비주얼라이저를 포함하여 작동하고 멋진 출력을 표시합니다.

# 

# Cpp2 reference

[https://hsutter.github.io/cppfront/cpp2/common/](https://hsutter.github.io/cppfront/cpp2/common/) 

## main

| //  Print out command line arguments, then invoke//  a Qt event loop for a non-UI Qt applicationmain: (args) \-\> int\= {    for args do (arg) {        std::cout \<\< arg \<\< "\\n";    }    app: QCoreApplication \= (args.argc, args.argv);    return app.exec();} |
| :---- |

메인이 반환할 수 있는 값은 

* void. 함수의 기본 반환값인 void입니다. 본문에는 반환 문을 사용할 수 없습니다. 이 경우 컴파일된 Cpp1 코드는 메인이 정수를 반환하는 것처럼 동작합니다.   
* int, 본문에 반환 문이 없는 경우 기본값은 함수 본문 끝에 0을 반환하는 것입니다.   
* Cpp1 컴파일러가 비표준 확장으로 지원하는 다른 타입입니다.

## 

## 예약 키워드

Cpp2에는 전역적으로 예약된 키워드가 거의 없으며, 거의 모든 키워드가 문맥에 따라 문법의 특정 위치에 나타날 때 특별한 의미를 갖습니다. 예를 들어

* `new`는 할당을 수행하는 일반 함수로 사용됩니다(예: `shared.new<widget>(1, 2, 3)`).  
* 메타함수 라이브러리에서 `struct` 및 `enum` 는 함수 이름으로 사용됩니다.  
* `type`는 일반 이름으로 사용할 수 있습니다(예: `std::common_type<T1,T2>::type`).

드물지만 일반적으로 다른 언어로 작성된 코드를 사용할 때 예약된 키워드인 이름을 작성해야 하는 경우가 있습니다. 이 경우 접두사 없이 일반 식별자로 취급하는 `__identifer__` 로 접두사를 붙이면 됩니다.

## 기본 데이터 타입

Cpp2는 현재의 Cpp1과 동일한 기본 타입을 지원하지만 네임스페이스 `cpp2`에서 다음과 같은 별칭을 추가로 제공합니다:

| Fixed-width types | Synonym for |
| :---- | :---- |
| i8 | std::int8\_t |
| i16 | std::int16\_t |
| i32 | std::int32\_t |
| i64 | std::int64\_t |
| u8 | std::uint8\_t |
| u16 | std::uint16\_t |
| u32 | std::uint32\_t |
| u64 | std::uint64\_t |

![][image7]

![][image8]

## Type 한정자

Type은 `const` 및 `*` 로 한정자를 지정할 수 있습니다. 타입은 왼쪽에서 오른쪽으로 작성되므로 한정자는 항상 바로 뒤에 오는 항목에 적용됩니다. 예를 들어 `const` 포인터를 선언하려면 `const` 객체에 대한 `const i32` 포인터를 가리킵니다, write:

**타입 한정자 사용하기**  
// const가 아닌 객체에 대한 const 포인터가 아닌 객체에 대한 const 포인터

| p: const \* \* const i32; |
| :---- |

## 리터럴

Cpp2는 대부분의 유니코드 인코딩 접두사와 원시 문자열 리터럴을 포함하여 Cpp1과 동일한 `'c'` 문자, `"문자열"`, 이진, 정수 및 부동소수점 리터럴을 지원합니다.

Cpp2는 기존 라이브러리를 원활하게 사용할 수 있도록 호환성을 위해 Cpp1 사용자 정의 리터럴 사용을 지원합니다. 그러나 Cpp2에는 [통합 함수 호출 구문(UFCS)](https://hsutter.github.io/cppfront/cpp2/expressions/#ufcs)이 있으므로 Cpp2에서 이와 동등한 것을 작성하는 가장 바람직한 방법은 함수 또는 타입 이름을 `.` 호출 접미사로 작성하는 것입니다. 예를 들어

* `u8` 값을 `u8(123)` 또는 **`123.u8()`**으로 작성하여 생성할 수 있습니다. [**1**](https://hsutter.github.io/cppfront/cpp2/common/#fn:u8using)  
* `'constexpr' 함수 처럼 작성하여 nm: (value: i64) -> my_nanometer_type == { /*...*/ }`는 정수를 받아 "나노미터" 타입으로 강력하게 입력된 값을 반환합니다, 그런 다음 `nm` 값을 `nm(123)` 또는 **`123으로 작성하여 생성합니다.nm()`**.

**`123.nm()`** 및 **`123.u8()`** 모두 사용자 정의 리터럴 구문과 매우 유사하며 더 일반적인 구문입니다.

## expressions

## \_ \- 명시적 삭제를 포함한 "상관 없음" 와일드카드입니다.

`_`는 **"상관없다"**로 발음되며 대부분의 문맥에서 와일드카드로 사용할 수 있습니다. 예를 들어

**와일드카드 \_ 사용**

| //  We don't care about the guard variable's name\_ : std::lock\_guard \= mut;//  If we don't care to write the variable's type, deduce itx : \_ \= 42;    // in cases like this, \_ can be omitted...    // this is equivalent to "x := 42;"return inspect v \-\> std::string {    is std::vector \= "v is a std::vector";    is \_ \= "unknown";   // don't care what else, match anything}; |
| :---- |

Cpp2는 모든 함수 출력(반환값, `inout` 및 `out` 매개변수를 통해 생성된 결과)을 중요한 것으로 취급하며 기본적으로 자동 폐기되지 않도록 설정되어 있습니다. 이러한 값을 명시적으로 삭제하려면 `_`에 값을 할당합니다. 예를 들어

**명시적 폐기를 위해 \_ 사용**

| \_ \= vec.emplace\_back(1,2,3);    // "\_ \=" is required to explicitly discard emplace\_back's    // return value (which is non-void since C++17){    x := my\_vector.begin();    std::advance(x, 2);    \_ \= x;  // required to explicitly discard x's new value,            // because std::advance modifies x's value} |
| :---- |

자세한 내용은 [설계 참고: 명시적 삭제](https://github.com/hsutter/cppfront/wiki/Design-note%3A-Explicit-discard)를 참조하세요. Cpp2에서는 데이터가 항상 초기화되고, 데이터가 자동으로 손실되지 않으며, 데이터 흐름을 항상 볼 수 있습니다. 데이터는 소중하며 항상 안전합니다.

## 함수에서 로컬 범위로, 그리고 다시 함수에서 로컬 범위로

함수 구문은 의도적으로 일반적이도록 설계되었으므로 일부를 생략할 수 있습니다. 즉, Cpp2에는 이름 없는 함수에 대한 특별한 "람다 함수" 구문이 없으며, 이름 없는 함수는 이름만 없는 일반 함수를 사용하여 작성된 이름 없는 함수입니다. 이는 이름이나 매개변수가 없는 함수와 동일하게 작성되는 일반 블록과 문으로까지 확장됩니다.

이를 두 가지 방향으로 설명할 수 있습니다. 먼저 전체 기능부터 시작하여 현재 사용하지 않는 옵션 부분을 순차적으로 생략해 보겠습니다:

**전체 함수부터 시작하고, 사용하지 않는 선택적 부분은 연속적으로 생략합니다.**

| // Full named functionf:(x: int \= init) \= { /\*...\*/ }     // x is a parameter to the functionf:(x: int \= init) \= statement;      // same, { } is implicit// Omit name \=\> anonymous function (aka 'lamba') :(x: int \= init) \= { /\*...\*/ }     // x is a parameter to the function :(x: int \= init) \= statement;      // same, { } is implicit// Omit declaration \=\> local and immediate (aka 'let' in other languages)  (x: int \= init)   { /\*...\*/ }     // x is a parameter to this  (x: int \= init)   statement;      //  compound or single-statement// Omit parameters \=\> ordinary block or statement                    { /\*...\*/ }     // ordinary compound statement                    statement;      // ordinary statement |
| :---- |

반대로, 평범한 블록이나 문으로 시작하여 이를 연속적으로 강화하여 더 강력하게 만들 수 있습니다:

**일반 블록 또는 문으로 시작하여 연속적으로 파트를 추가합니다.**

| // Ordinary block or statement                    { /\*...\*/ }     // ordinary compound statement                    statement;      // ordinary statement// Add parameters \=\> more RAII locally-scoped variables  (x: int \= init)   { /\*...\*/ }     // x is destroyed after this  (x: int \= init)   statement;      //  compound or single-statement// Add declaration \=\> treat the code as a callable object :(x: int \= init) \= { /\*...\*/ }     // x is a parameter to the function :(x: int \= init) \= statement;      // same, { } is implicit// Add name \=\> full named functionf:(x: int \= init) \= { /\*...\*/ }     // x is a parameter to the functionf:(x: int \= init) \= statement;      // same, { } is implicit |
| :---- |

## 템플릿 매개변수

템플릿 매개변수 목록은 [목록으로 묶인 `<` 와 쉼표로 구분된 매개변수로 이루어진 `목록입니다. 각 매개변수는 모든 타입 또는 객체와 동일한 구문인`](https://hsutter.github.io/cppfront/cpp2/common/#lists) `을 사용하여 선언됩니다. 매개 변수의 : kind가 지정되지 않으면 기본값은 : type 이 됩니다.`

예를 들어

**템플릿 매개변수 선언하기**

| array: \<T: type, size: i32\> type    // parameter T is a type    // parameter size is a 32-bit int\= {    // ...}tuple: \<Ts...: type\> type    // parameter Ts is variadic list of zero or more types\= {    // ...} |
| :---- |

## `requires` 제약 조건

필요 조건 제약 조건은 템플릿 선언의 종류 끝에 나타납니다. 조건이 거짓으로 평가되면 선언되지 않은 것처럼 템플릿의 해당 전문화가 무시됩니다. 

예: A는 변동 함수에 대한 제약 조건을 필요로 합니다.

| print: \<Args...: type\>       (inout out: std::ostream, args...: Args)       requires sizeof...(Args) \>= 1u\= {    (out \<\< ... \<\< args);} |
| :---- |

Examples

| //  n is a namespace defined as the following scopen: namespace\= {    //  shape is a templated type with one type parameter T    //  (equivalent to '\<T: type\>') defined as the following scope    shape: \<T\> type    \= {        //  point is a type defined as being always the same as        //  (i.e., an alias for) T        point\_type: type \== T;        //  points is an object of type std::vector\<point\_type\>,        //  defined as having an empty default value        //  (type-scope objects are private by default)        points: std::vector\<point\_type\> \= ();        //  draw is a function taking 'this' and 'canvas' parameters        //  and returning bool, defined as the following body        //  (type-scope functions are public by default)        //        //  this is an object of type shape (as if written 'this: shape')        //        //  where is an object of type canvas        draw: (this, where: canvas) \-\> bool        \= {            //  pen is an object of deduced (omitted) type 'color',            //  defined as having initial value 'color::red'            pen := color::red;            //  success is an object of deduced (omitted) type bool,            //  defined as having initial value 'false'            success := false;            // ...            return success;        }        //  count is a function taking 'this' and returning a type        //  deduced from its body, defined as a single-expression body        //  (equivalent to '= { return points.ssize(); }' but omitting        //  syntax where we're using the language defaults)        count: (this) \= points.ssize();        //  ...    }    //  color is an @enum type (see Note) defined as having these enumerators    color: @enum type \= { red; green; blue; }    //  calc\_next\_year is a function defined as always returning the same    //  value for the same input (i.e., 'constexpr', side effect-free)    calc\_next\_year: (year: i32) \-\> i32 \== year \+ 1;} |
| :---- |

참고: `@enum`는 메타함수로, 기본값, 제약 조건 및 생성된 함수 그룹을 쉽게 선택할 수 있는 방법을 제공합니다. 자세한 내용은 [`@enum`](https://hsutter.github.io/cppfront/cpp2/metafunctions/#enum)를 참조하세요.

[https://hsutter.github.io/cppfront/cpp2/declarations/](https://hsutter.github.io/cppfront/cpp2/declarations/) 

## 별칭

별칭은 **"동의어"**로 발음되며, Cpp2의 모든 것과 동일한 **이름 `:` kind `=` 값** 를 사용하여 작성합니다:

* **이름**은 **값**과 동의어로 선언됩니다.  
* **kind**는 모든 종류가 될 수 있습니다: `namespace`, `type`, 함수 서명 또는 type 입니다.  
* **`==`** 로 발음되는 **"의 동의어로 정의"는 항상 값 앞에 옵니다. `==` 구문은 컴파일 중에 이름의 모든 사용이 값으로 동등하게 대체될 수 있음을 강조합니다.**  
* **값**은 **이름**과 동의어인 표현식입니다.

네임스페이스 별칭

네임스페이스 별칭은 [네임스페이스](https://hsutter.github.io/cppfront/cpp2/namespaces/)와 같은 방식으로 작성되지만 `==`를 사용하고 다른 네임스페이스의 이름을 값으로 사용합니다. 예를 들어

**네임스페이스 별칭**

| //  'chr' is a namespace defined as a synonym for 'std::chrono'chr    : namespace \== std::chrono;//  'chrlit' is a namespace defined as a synonym for 'std::chrono\_literals'chrlit : namespace \== std::chrono\_literals;main: () \= {    using namespace chrlit;    //  The next two lines are equivalent    std::cout \<\< "1s is (std::chrono::nanoseconds(1s).count())$ns\\n";    std::cout \<\< "1s is (chr::nanoseconds(1s).count())$ns\\n";}//  Prints://      1s is 1000000000ns//      1s is 1000000000ns |
| :---- |

Type 별칭

Type 별칭은 Type과 같은 방식으로 작성되지만 \== 를 사용하고 다른 type의 이름을 값으로 사용합니다. 예를 들어

**별칭 입력**

| //  'imap\<T\>' is a type defined as a synonym for 'std::map\<i32, T\>'imap : \<T\> type \== std::map\<i32, T\>;main: () \= {    //  The next two lines declare two objects with identical type    map1: std::map\<i32, std::string\> \= ();    map2: imap\<std::string\> \= ();    //  Assertion they are the same type, using the same\_as concept    static\_assert( std::same\_as\< decltype(map1), decltype(map2) \> );} |
| :---- |

Function aliases

함수 별칭은 함수와 동일한 방식으로 작성되지만 \== 를 사용하고 부작용이 없는 본문을 값으로 사용하며, 본문은 항상 동일한 입력 인자에 대해 동일한 값을 반환해야 합니다. 

예를 들어

| //  'square' is a function defined as a synonym for the value of 'i \* i'square: (i: i32) \-\> \_ \== i \* i;main: () \= {    //  It can be used at compile time, with compile time values    ints: std::array\<i32, square(4)\> \= ();    //  Assertion that the size is the square of 4    static\_assert( ints.size() \== 16 );    //  And it can be used at run time, with run time values    std::cout \<\< "the square of 4 is (square(4))$\\n";}//  Prints://      the square of 4 is 16 |
| :---- |

Object aliases

객체 별칭은 [객체](https://hsutter.github.io/cppfront/cpp2/objects/)와 같은 방식으로 작성되지만 `==` 를 사용하여 부작용이 없는 값으로 작성됩니다. 예를 들어

| //  'BufferSize' is an object defined as a synonym for the value 1'000'000BufferSize: i32 \== 1'000'000;main: () \= {    buf: std::array\<std::byte, BufferSize\> \= ();    static\_assert( buf.size() \== BufferSize );} |
| :---- |

## Objects, initialization, and memory

[https://hsutter.github.io/cppfront/cpp2/objects/](https://hsutter.github.io/cppfront/cpp2/objects/) 

객체는 네임스페이스, `타입`, 함수, 표현식 등 모든 범위에서 선언할 수 있습니다.

선언은 Cpp2의 모든 것과 동일한 **name `:` kind `=` value** [선언 구문](https://hsutter.github.io/cppfront/cpp2/declarations/)을 사용하여 작성됩니다:

* **name**은 문자로 시작하고 그 뒤에 다른 문자, 숫자 또는 `_`가 이어집니다. 예시 `count`, `skat_game`, `Point2D` 가 유효한 이름입니다.  
* **kind**는 객체의 타입입니다. 타입 범위를 제외한 대부분의 위치에서 `_` 와일드카드를 타입으로 작성하거나 타입을 완전히 생략하여 타입을 추론하도록 요청할 수 있습니다. 타입이 템플릿인 경우 템플릿화된 인수는 생성자에서 유추할 수 있습니다([CTAD](https://hsutter.github.io/cppfront/welcome/hello-world/#ctad)).  
* **value**는 객체의 초기 값입니다. 기본으로 구성된 값을 사용하려면 `()`를 작성합니다.

예를 들어

**일부 객체 선언하기**

| //  numbers is an object of type std::vector\<point2d\>,//  defined as having the initial contents 1, 2, 3numbers: std::vector\<int\> \= (1, 2, 3);numbers: std::vector \= (1, 2, 3);       // same, deducing the vector's type//  count is an object of type int, defined as having initial value \-1count: int \= \-1;count: \_ \= \-1;      // same, deducing the object's type with the \_ wildcardcount := \-1;        // same, deducing the object's type by just omitting it//  pi is a variable template; \== signifies the value never changes (constexpr)pi: \<T: type\> T \== 3.14159'26535'89793'23846L;pi: \_ \== 3.14159'26535'89793'23846L;    // same, deducing the object's type |
| :---- |

매개변수 타입은 `_` (기본값이므로 생략 가능)를 작성하여 추론할 수 있습니다. 추론된 타입이 일치해야 하는 타입 제약 조건(예: concept)을 선언하려면 `is`를 사용할 수 있으며, 이 경우 `_` 가 필요합니다. 예를 들어

**제약된 추론된 타입의 객체 선언하기**

| //  number's type is deduced, but must match the std::regular conceptnumber: \_ is std::regular \= some\_factory\_function(); |
| :---- |

**보장된 초기화**

모든 객체는 사용하기 전에 `=` 를 사용하여 초기화해야 합니다.

모든 범위의 객체는 선언 시 초기화할 수 있습니다. 예를 들어

**예: 객체가 선언될 때 초기화하기**

| shape: type \= {    //  An object at type scope (data member)    //  initialized with its type's default value    points: std::vector\<point2d\> \= ();    draw: (this, where: canvas) \-\> bool    \= {        //  An object at function scope (local variable)        //  initialized with color::red        pen := color::red;        // ...    }    //  ...} |
| :---- |

또한 함수 로컬 범위에서 객체 `obj`는 선언과 별도로 초기화할 수 있습니다. 이는 프로그램에서 의미 있는 초기 값을 알기 전에 객체를 선언해야 하는 경우(잘못된 '더미' 값의 데드 쓰기를 방지하기 위해), 또는 다른 논리에 따라 객체가 여러 가지 방식으로 초기화될 수 있는 경우(예: 다른 경로에서 다른 생성자를 사용하여) 유용할 수 있습니다. 이를 위한 방법은 다음과 같습니다:

* 이니셜라이저 없이 `obj`(예: `obj: some_type;`)를 선언합니다. 이렇게 하면 객체에 대한 스택 공간이 할당되지만 객체를 구성하지는 않습니다.  
* `obj`는 모든 `if`/`else` 분기 경로에서 명확한 첫 번째 용도를 가져야 하며, 다음과 같이 정의해야 합니다.  
* 그 명확한 첫 번째 사용은 생성자 호출인 `obj = value;` 형식이어야 하며, 그렇지 않으면 `obj`를 `out` 인수로 (사실상 생성자 호출이며 호출자에서 구성을 수행) `out` 파라미터에 전달합니다.

예를 들어

**예: 로컬 객체가 선언된 후 초기화하기**

| f: () \= {    buf: std::array\<std::byte, 1024\>;   // uninitialized    //  ... calculate some things ...    //  ...  no uses of buf here  ...    buf \= some\_calculated\_value;        // constructs (not assigns) buf    //  ...    std::cout buf\[0\];                   // ok, a has been initialized}g: () \= {    buf: std::array\<std::byte, 1024\>;   // uninitialized    if flip\_coin\_is\_heads() {        if heads\_default\_is\_available {            buf \= copy\_heads\_default(); // constructs buf        }        else {            buf \= (other, constructor); // constructs buf        }    }    else {        load\_from\_disk( out buf );      // constructs buf (\*)    }    std::cout \<\< buf\[0\];                // ok, a has been initialized}load\_from\_disk: (out x) \= {    x \= /\* data read from disk \*/ ;     // when \`buffer\` is uninitialized,}                                       // constructs it; otherwise, assigns |
| :---- |

위의 예시에서 브랜치에 대한 간단한 규칙을 주목하세요: 로컬 변수는 `if` 및 `else` 브랜치 모두에서 초기화해야 하며, 두 브랜치 모두에서 초기화하지 않아야 합니다.

**Heap 개체**

여기서 `xxx.new <T> (/*initializer, arguments*/)` 는 메모리 할당자 역할을 하고 .new 함수 템플릿을 제공하는 모든 객체입니다. 네임스페이스 cpp2에는 두 개의 메모리 얼로케이터 객체가 제공됩니다:

* `unique.new<T>` 호출 `std::make_unique<T>`를 호출하고 `std::unique_ptr<T>`를 반환합니다.  
* `share.new<T>` 호출 `std::make_shared<T>`를 호출하고 `std::shared_ptr<T>`를 반환합니다.

할당자 객체를 지정하지 않으면 기본값은 `unique.new` 입니다.

예를 들어, 쓰기 타입에 대한 자세한 내용은 [types](https://hsutter.github.io/cppfront/cpp2/types/)를 참조하세요:

**힙 할당**

| f: () \-\> std::shared\_ptr\<widget\>\= {    //  Dynamically allocate an object owned by a std::unique\_ptr    //  'vec' is a unique\_ptr\<vector\<i32\>\> containing three values    vec := new\<std::vector\<i32\>\>(1, 2, 3);            // shorthand for 'unique.new\<...\>(...)'    std::cout \<\< vec\*.ssize();  // prints 3                    // note that \* dereference is a suffix operator    //  Dynamically allocate an object with shared ownership    wid := cpp2::shared.new\<widget\>();    store\_a\_copy( wid );        // store a copy of 'wid' somewhere    return wid;                 // and move-return a copy too} // as always in C++, vec is destroyed here automatically, which  // destroys the heap vector and deallocates its dynamic memory |
| :---- |

## Functions

함수는 : 뒤에 함수 서명을 작성하고 { } 복합문 뒤에 문(식 또는 \=)을 작성하여 정의됩니다. 모든 선언에 사용할 수 있는 선택적 템플릿 매개변수 다음에 함수 서명은 비어 있을 수 있는 파라미터 목록과 하나 이상의 선택적 리턴 값으로 구성됩니다.

예를 들어 매개 변수를 받지 않고 아무 것도 반환하지 않는 func라는 최소 함수(void)가 있습니다:

최소한의 함수

| func: ( /\* no parameters \*/ ) \= { /\* empty body \*/ } |
| :---- |

함수 서명: 매개변수, 반환 및 함수 타입 사용

**매개변수**  
매개변수 목록은 ( ) 괄호로 묶인 목록입니다. 각 매개변수는 모든 선언에 사용된 것과 동일한 통합 구문인 을 사용하여 선언됩니다. 예를 들어

매개변수 선언하기

| func: (    x: i32,                         // parameter x is a 32-bit int    y: std::string,                 // parameter y is a std::string    z: std::map\<i32, std::string\>   // parameter z is a std::map    )\= {    // ...} |
| :---- |

매개변수 타입은 \_ 를 작성하여 추론할 수 있습니다(기본값이므로 생략 가능). 추론된 타입이 일치해야 하는 타입 제약 조건(예: 개념)을 선언하려면 is를 사용할 수 있으며, 이 경우 \_가 필요합니다. 예를 들어

제약 추론된 타입의 매개변수 선언하기

| //  ordinary generic function, x's type is deducedprint: (x: \_) \= { std::cout \<\< x; }print: (x)    \= { std::cout \<\< x; } // same, using the \_ default//  number's type is deduced, but must match the std::integral conceptcalc: (number: \_ is std::integral) \= { /\*...\*/ } |
| :---- |

모든 사용 사례를 포괄하는 매개변수를 전달하는 방법에는 6가지가 있으며, 매개변수 이름 앞에 작성할 수 있습니다:

| Parameter *kind* | "Pass me an x I can \_\_\_\_\_\_" | Accepts arguments that are | Special semantics | *kind* x: X Compiles to Cpp1 as |
| :---- | :---- | :---- | :---- | :---- |
| in (default) | read from | anything | always constautomatically passes by value if cheaply copyable | X const x or X const& x |
| copy | take a copy of | anything | acts like a normal local variable initialized with the argument | X x |
| inout | read from and write to | lvalues |  | X& x |
| out | write to (including construct) | lvalues (including uninitialized) | must \= assign/construct before other uses | cpp2::impl::out\<X\> |
| move | move from (consume the value of) | rvalues | automatically moves from every definite last use | X&& |
| forward | forward | anything | automatically forwards from every definite last use | T&& constrained to type X |

참고: 로컬 변수를 제외한 Cpp2의 모든 매개변수 및 기타 개체는 기본적으로 const 입니다. 자세한 내용은 설계 참고: const 개체는 기본적으로를 참조하세요.

예를 들어  
매개변수 종류 선언하기

| append\_x\_to\_y: (    x       : i32,          // an i32 I can read from (i.e., const)    inout y : std::string   // a string I can read from and write to    )\= {    y \= y \+ to\_string(x);   // read x, read and write y}wrap\_f: (    forward x               // a generic value of deduced type I can forward)                           //  (omitting x's  type means the same as ': \_')\= {    global\_counter \+= x;    // ok to read x    f(x);                   // last use: automatically does 'std::forward\<T\>(x)'} |
| :---- |

**반환 값**  
함수는 다음 중 하나를 반환할 수 있습니다. 기본값은 \-\> void 입니다.

(1) \-\> X로 X 타입의 이름 없는 단일 값을 반환합니다, 이 함수에 반환값이 없음을 나타내는 void가 될 수 있습니다. X가 void가 아닌 경우, 함수 본문에는 return /\*value\*/가 포함되어야 합니다; 문은 함수를 종료하는 모든 경로에서 X 타입의 값을 반환합니다. 예를 들어

반환값이 명명되지 않은 함수

| //  A function returning no value (void)increment\_in\_place: (inout a: i32) \-\> void \= { a++; }//  Or, using syntactic defaults, the following has identical meaning:increment\_in\_place: (inout a: i32) \= a++;//  A function returning a single value of type i32add\_one: (a: i32) \-\> i32 \= { return a+1; }//  Or, using syntactic defaults, the following has identical meaning:add\_one: (a: i32) \-\> i32 \= a+1;//  A generic function returning a single value of deduced typeadd: \<T: type, U: type\> (a:T, b:U) \-\> decltype(a+b) \= { return a+b; }//  Or, using syntactic defaults, the following has identical meaning:add: (a, b) \-\> \_ \= a+b; |
| :---- |

(2) \-\> ( /\* 매개변수 목록 \*/ )을 사용하여 동일한 매개변수 구문을 사용하지만 전달 스타일만 아웃(기본값으로 가능한 경우 앞으로 이동) 또는 앞으로 이동하는 명명된 반환 매개변수 목록을 반환할 수 있습니다. 함수 본문은 다른 지역 변수와 동일한 방식으로 본문에 있는 각 반환 매개변수 ret의 값을 초기화해야 합니다. 명시적 반환 문은 그냥 반환하고 명명된 값을 반환하며, 함수의 마지막에는 암시적 반환이 있습니다. 목록에 반환 매개변수가 하나만 있는 경우 위 (1)과 같은 방식으로 하위 Cpp1 코드에서 출력되므로 해당 이름은 함수 본문 내에서만 확인할 수 있습니다.

For example:

Function with multiple/named return values

| divide: (dividend: int, divisor: int) \-\> (quotient: int, remainder: int) \= {    if divisor \== 0 {        quotient  \= 0;                      // constructs quotient        remainder \= 0;                      // constructs remainder    }    else {        quotient \= dividend / divisor;      // constructs quotient        remainder \= dividend % divisor;     // constructs remainder    }}main: () \= {    div := divide(11, 5);    std::cout \<\< "(div.quotient)$, (div.remainder)$\\n";}//  Prints://     2, 1 |
| :---- |

다음 예제에서는 집합이라는 타입에 여러 반환값을 가진 멤버 함수를 선언합니다:

Member function with multiple/named return values

| set: \<Key\> type \= {    container: std::set\<Key\>;    iterator : type \== std::set\<Key\>::iterator;    //  A std::set::insert-like function using named return values    //  instead of just a std::pair/tuple    insert: (inout this, value: Key) \-\> (where: iterator, inserted: bool) \= {        set\_returned := container.insert(value);        where    \= set\_returned.first;        inserted \= set\_returned.second;    }    ssize: (this) \-\> i64 \= std::ssize(container);    // ...}use\_inserted\_position: (\_) \= { }main: () \= {    m: set\<std::string\> \= ();    ret := m.insert("xyzzy");    if ret.inserted {        use\_inserted\_position( ret.where );    }    assert( m.ssize() \== 1 );} |
| :---- |

**함수 출력은 암시적으로 삭제할 수 없습니다.**

함수의 출력은 반환 값과 out 및 inout 매개 변수의 "out" 상태입니다.

함수 출력은 자동 삭제할 수 없습니다. 함수 출력을 명시적으로 삭제하려면 \_ 에 할당합니다. 예를 들어

자동 폐기 없음

| f: ()             \-\> void \= { }g: ()             \-\> int  \= { return 10; }h: (inout x: int) \-\> void \= { x \= 20; }main: ()\= {    f();                    // ok, no return value    std::cout \<\< g();       // ok, use return value    \_ \= g();                // ok, explicitly discard return value    g();                    // ERROR, return value is ignored    {        x := 0;        h( x );             // ok, x is referred to again...        std::cout \<\< x;     // ... here, so its new value is used    }    {        x := 0;        h( x );             // ok, x is referred to again...        \_ \= x;              // ... here where its value explicitly discarded    }    {        x := 0;        h( x );             // ERROR, this is a definite last use of x    }                       // so x is not referred to again, and its                            // 'out' value can't be implicitly discarded} |
| :---- |

Cpp2는 Cpp1 코드에 폐기할 수 없는 시맨틱을 부여하는 동시에 평소와 같이 완벽하게 호환됩니다:

Cpp2 구문으로 작성된 함수가 void가 아닌 다른 것을 반환하면 항상 \[\[nodiscard\]\]로 Cpp1에 컴파일됩니다.

Cpp2 x.f() 멤버 호출 구문으로 작성된 함수 호출은 void가 아닌 반환 타입을 항상 폐기 불가능한 것으로 취급하며, 이는 함수가 Cpp1 구문으로 작성된 경우라도 \[\[nodiscard\]\]가 없는 경우에도 마찬가지입니다.

**함수 타입 사용**  
동일한 함수 매개변수/리턴 구문을 함수 타입으로 사용할 수 있습니다(예: std::function를 인스턴스화하거나 함수 변수에 대한 포인터를 선언하는 데). 예를 들어

예: std::function 및 \*pfunc와 함께 함수 타입 사용

| decorate\_int: (i: i32) \-\> std::string \= "--\> (i)$ \<--";main: () \= {    pf1: std::function\< (i: i32) \-\> std::string \> \= decorate\_int&;    std::cout \<\< "pf1(123) returned \\"(pf1(123))$\\"\\n";    pf2: \* (i: i32) \-\> std::string \= decorate\_int&;    std::cout \<\< "pf2(456) returned \\"(pf2(456))$\\"\\n";}//  Prints://    pf1 returned "--\> 123 \<--"//    pf2 returned "--\> 456 \<--" |
| :---- |

## Control flow

### if, else — Branches

if 및 else는 C++에서 항상 그렇듯이 조건 주위에 ( ) 괄호가 필요하지 않다는 점을 제외하면 동일합니다. 대신 분기 본문 주위에 { } 중괄호를 사용해야 합니다. 예를 들어

Using if and else

| if vec.ssize() \> 100 {    do\_general\_algorithm( container );}else {    do\_linear\_scan( vec );} |
| :---- |

### for, while, do — Loops

do와 while은 C++에서 항상 그렇듯이 조건 주위에 ( ) 괄호가 필요하지 않다는 점을 제외하면 동일합니다. 대신 루프 본문 주위에 { } 중괄호를 사용해야 합니다.

for 범위 do (e) statement에서 "범위의 각 요소에 대해라고 합니다, e라고 호출하고 문을 수행합니다."  루프 매개변수 (e)는 모든 매개 변수 전달 스타일를 사용하여 전달할 수 있는 일반 매개변수이며, 항상 그렇듯이 기본값은 읽기 전용이며 읽기 전용 루프를 표현하는 in입니다. 이 문은 중괄호로 묶을 필요가 없습니다.

모든 루프에는 각 루프 본문 실행이 끝날 때 수행되는 next 절을 포함할 수 있습니다. 이렇게 하면 범위 for 루프를 포함하여 모든 루프에 대한 카운터를 쉽게 만들 수 있습니다.

참고: 공백은 문체 선택 사항일 뿐입니다. 이 문서의 스타일은 일반적으로 각 키워드를 고유한 줄에 배치하고 뒤에 오는 내용을 정렬합니다.

예를 들어

예: 루프 사용

| words: std::vector\<std::string\> \= ("Adam", "Betty");i := 0;while i \< words.ssize() // while this condition is truenext  i++               // and increment i after each loop body is run{                       // do this loop body    std::cout \<\< "word: (words\[i\])$\\n";}//  prints://      word: Adam//      word: Bettydo {                    // do this loop body    std::cout \<\< "\*\*\\n";}next  i--               // and decrement i after each loop body is runwhile i \> 0;            // while this condition is true//  prints://      \*\*//      \*\*for  words              // for each element in 'words'next i++                // and increment i after each loop body is rundo   (inout word)       // declare via 'inout' the loop can change the contents{                       // do this loop body    word \= "\[" \+ word \+ "\]";    std::cout \<\< "counter: (i)$, word: (word)$\\n";}//  prints://      counter: 0, word: \[Adam\]//      counter: 1, word: \[Betty\] |
| :---- |

일치하는 하위 집합에 대해서만 루프 본문을 수행하기 위해 특별한 "select" 또는 "where"가 필요하지 않으며, 이는 if로 자연스럽게 표현할 수 있기 때문입니다. 예를 들어

루프 \+ if 사용

| //  Continuing the previous examplei \= 0;for  wordsnext i++do   (word)if   i % 2 \== 1         // if i is odd{                       // do this loop body    std::cout \<\< "counter: (i)$, word: (word)$\\n";}//  prints://      counter: 1, word: \[Betty\] |
| :---- |

다음은 Cpp1 코드에 해당하는 for ( int i \= 0; i \< 10; \++i ){ std::cout \<\< i; } 입니다:

Cpp1 'for ( int i \= 0; i \< 10; \++i ){ std::cout \<\< i; }' 와 동등함

| (copy i := 0\)while i \< 10next  i++ {    std::cout \<\< i;} |
| :---- |

한 줄씩 살펴봅시다:

* (copy i := 0): 모든 문에는 statement-local 매개 변수가 있을 수 있으며, 이는 i를 루프에 로컬인 int로 선언하는 것입니다. 기본적으로 매개변수는 const 이며, 복사하기 어려운 타입의 경우 원래 값에 바인딩되므로 i를 수정하려면 copy라고 명시적으로 선언하여 이것이 루프 고유의 변경 가능한 스크래치 변수임을 알립니다.  
* while i \< 10:  종료 조건입니다.  
* next i++:  루프 반복 종료 문입니다. 참고 \++는 Cpp2에서 항상 포스트픽스입니다.

### 루프 이름, break 및 continue.

루프는 일반적인 이름 을 사용하여 이름을 지정할 수 있습니다: 구문을 사용하여 모든 이름을 지정할 수 있으며, break 및 continue는 이러한 이름을 참조할 수 있습니다. 예를 들어

명명된 중단 및 계속 사용

| outer: while i\<M next i++ {      // loop named "outer"    // ...    inner: while j\<N next j++ {  // loop named "inner"        // ...        if something() {            continue inner;      // continue the inner loop        }        // ...        if something\_else() {            break outer;         // break the outer loop        }        // ...    }    // ...} |
| :---- |

정의된 마지막 사용에서 Move/Forward  
함수 본문에서 로컬 이름의 정확한 마지막 사용은 루프가 아닌 문에서 해당 이름을 한 번 사용한 것으로, 해당 문 뒤에 다시 이름을 언급하는 제어 흐름 경로가 없는 경우입니다.

각 명확한 마지막 사용에 대해:

* 이름이 로컬 객체이거나 copy 또는 move 매개 변수인 경우 객체가 소멸되기 전에 다시 사용되지 않으므로 객체는 자동으로 rvalue(이동 후보)로 처리됩니다. 마지막 사용이 포함된 표현식이 rvalue에서 이동할 수 있는 경우 자동으로 이동이 수행됩니다.  
* 이름이 forward 매개변수인 경우, 객체의 상수 및 값 범주를 유지하기 위해 객체가 자동으로 전달됩니다(std::forward-ed).

예를 들어

확실한 마지막 사용

| f: (    copy    x: some\_type,    move    y: some\_type,    forward z: some\_type    )\= {    w: some\_type \= "y";    prepare(x);                     // NOT a definite last use    if something() {        process(y);        z.process(x);               // definite last uses of x and z    }    else {        cout \<\< z;                  // definite last use of z    }    transfer(y);                    // definite last use of y    offload(w);                     // definite last use of w} |
| :---- |

이 예제에서는

* x가 한 경로에는 명확한 마지막 용도가 있지만 다른 경로에는 없습니다. 13번째 줄은 x를 자동으로 r값으로 처리하는 명확한 마지막 사용입니다. 그러나 else가 사용되는 경우 x는 특별한 자동 처리를 받지 않습니다. 9번째 줄은 13번째 줄의 뒷부분에서 x가 다시 사용될 수 있으므로 확실한 마지막 사용은 아닙니다.

* y는 모든 경로에서 명확한 마지막 용도를 가지며, 이 경우 함수의 모든 실행에서 동일하게 사용됩니다. 19번째 줄은 x를 자동으로 r값으로 처리하는 명확한 마지막 사용입니다.

* z는 모든 경로에서 명확한 마지막 용도를 갖지만 y와 달리 함수의 다른 실행에서 다른 마지막 용도가 될 수 있습니다. 13줄과 16줄은 각각 z의 상수 및 값 범주를 자동으로 전달하는 확실한 마지막 사용입니다.

* w는 모든 경로에서 명확한 마지막 용도를 가지며, 이 경우 함수의 모든 실행에서 동일합니다. 21번째 줄은 w를 자동으로 r값으로 처리하는 명확한 마지막 사용입니다.

### 일반 참고 사항: 함수 기본값 요약

현재 사용하지 않는 부분은 생략할 수 있도록 설계된 단일 함수 구문이 있습니다.

예를 들어 equals가 두 개의 타입 매개변수 T와 U를 포함하는 함수 템플릿이라고 자세히 표현해 봅시다, 두 개의 일반 in 매개 변수 a와 b는 각각 T와 U 타입으로 구성됩니다, 및 추론된 반환 타입이며, 본문은 a \== b 결과를 반환합니다:

같음: (기본값을 사용하지 않고) 상세하게 작성된 일반 함수

| equals: \<T: type, U: type\> (in a: T, in b: U) \-\> \_ \= { return a \== b; } |
| :---- |

이 모든 것을 작성할 수 있지만 그럴 필요는 없습니다.  
먼저, : type는 템플릿 매개변수의 기본값이므로 생략해도 됩니다:

같음: 동일한 의미, 이제 템플릿 매개변수에 대해 :type 기본값을 사용합니다.

| equals: \<T, U\> (in a: T, in b: U) \-\> \_ \= { return a \== b; } |
| :---- |

지금까지 반환 타입은 이미 Cpp2 전체에서 사용할 수 있는 하나의 공통 기본값인 와일드카드 \_ ("don't care"로 발음)를 사용하고 있습니다. 이 함수의 본문은 실제로 매개변수 타입 이름인 T와 U를 사용하지 않으므로 매개변수 타입에도 와일드카드를 사용할 수 있습니다:

같음: 동일한 의미, 이제 매개변수 타입에도 \_ 와일드카드를 사용합니다.

| equals: (in a: \_, in b: \_) \-\> \_ \= { return a \== b; } |
| :---- |

다음으로, : \_도 기본 매개변수 타입이므로 이마저도 작성할 필요가 없습니다:  
같음: 동일한 의미, 이제 :\_ 기본 매개변수 타입 사용

| equals: (in a, in b) \-\> \_ \= { return a \== b; } |
| :---- |

다음으로, in는 기본 파라미터 전달 모드입니다. 따라서 이 기본값도 사용할 수 있습니다:

같음: 동일한 의미, 이제 'in' 기본 매개변수 전달 스타일 사용

| equals: (a, b) \-\> \_ \= { return a \== b; } |
| :---- |

{ }가 아무것도 반환하지 않는 한 줄 함수의 기본값이라는 것을 이미 확인했습니다. 마찬가지로, { return 및 }는 무언가를 반환하는 한 줄 함수에 대한 기본값입니다:

같음: 동일한 의미, 이제 { 반환 ... } 기본 본문 장식

| equals: (a, b) \-\> \_ \= a \== b; |
| :---- |

다음으로,  \-\> \_ \= (추론 반환 타입)은 무언가를 반환하는 단일 표현식 함수에 대한 기본값이므로 생략할 수 있습니다:

동일한 의미, 이제 무언가를 반환하는 함수에 대해 \-\> \_ \= 기본값 사용

| equals: (a, b) a \== b; |
| :---- |

마지막으로 표현식 범위(일명 "람바/임시")에서는 함수/개체의 이름이 지정되지 않으며, 뒤에 오는 ;는 선택 사항입니다:

(아닌) '같음': 동일한 의미이지만 표현식 범위에서 이름이 없는 함수로 이름이 없습니다.

| :(a, b) a \== b |
| :---- |

다음은 이름 없는 함수 표현식의 몇 가지 추가 예시입니다:

명명되지 않은 함수 표현식의 몇 가지 추가 예제

| std::ranges::for\_each( a, :(x) \= std::cout \<\< x );std::ranges::transform( a, b, :(x) x+1 );where\_is \= std::ranges::find\_if( a, :(x) x \== waldo$ ); |
| :---- |

참고: Cpp2에는 별도의 "람다" 구문이 없으며, 표현식 범위에서 일반 함수 구문을 사용하여 이름 없는 함수를 작성하고 이러한 함수 표현식을 편리하게 작성할 수 있도록 구문 기본값이 선택되어 있습니다. 그리고 Cpp2에서는 모든 지역 변수 capture(예: 위의 waldo$)가 본문에 작성되므로 함수 구문에 영향을 미치지 않습니다.

## Contracts

[https://hsutter.github.io/cppfront/cpp2/contracts/](https://hsutter.github.io/cppfront/cpp2/contracts/) 

## Types

[https://hsutter.github.io/cppfront/cpp2/types/](https://hsutter.github.io/cppfront/cpp2/types/)

사용자 정의 타입는 Cpp2의 모든 것과 동일한 **name : kind \= value** 선언 구문를 사용하여 작성됩니다. 타입의 "값"은 더 많은 선언을 포함하는 {} 로 묶인 본문입니다.

타입에서 데이터 멤버는 기본적으로 비공개이며, 함수와 중첩된 타입은 기본적으로 공개입니다. 타입 범위 선언을 명시적으로 선언하려면 public, protected 로 선언합니다, 또는 private인 경우 선언의 시작 부분에 해당 키워드를 작성합니다.

간단한 타입 작성

| mytype: type \={    // data members are private by default    x: std::string;    // functions are public by default    protected f: (this) \= { do\_something\_with(x); }    // ...} |
| :---- |

### this \- 매개변수 이름

이것은 현재 객체의 동의어입니다. member 라는 멤버가 있는 타입의 범위 내에서 member는 기본적으로 this.member 를 의미합니다.

참고: Cpp2에서 this는 포인터가 아닙니다.

this 라는 이름은 타입 범위 함수(일명 멤버 함수)의 첫 번째 매개변수에만 사용할 수 있습니다. 해당 타입은 항상 현재 타입이므로 명시적인 : its\_type 로 선언되지 않습니다.

this는 in(기본값), inout, out 또는 이동 매개변수일 수 있습니다. 어떤 것을 선택하느냐에 따라 선언되는 멤버 함수의 종류가 자연스럽게 결정됩니다:

* in this: myfunc 작성: (this /\*...\*/), 이는 myfunc의 축약어입니다: (in 이 /\*...\*/), Cpp1 const-적격 멤버 함수를 정의합니다, in 매개 변수는 const이기 때문입니다.

* inout this: myfunc 작성: (inout this /\*...\*/)는 Cpp1 비const 멤버 함수를 정의합니다.

* out this: myfunc 작성: (out this /\*...\*/)는 Cpp1 생성자... 등을 정의합니다. (아래 참조).

* move this: myfunc 작성: (move this /\*...\*/)는 Cpp1 &&-적격 멤버 함수를 정의하거나 추가 파라미터가 없는 경우 소멸자를 정의합니다.

예를 들어 다음은 읽기 전용 문자열 값을 받아 이 객체의 데이터 값과 문자열 메시지를 인쇄하는 print라는 읽기 전용 멤버 함수를 작성하는 방법입니다:

이 매개 변수

| mytype: type \= {    data: i32;   // some data member (private by default)    print: (this, msg: std::string) \= {        std::cout \<\< data \<\< msg;                 // "data" is shorthand for "this.data"    }    // ...} |
| :---- |

### this — Inheritance

기본 유형은 다음과 같은 이름의 멤버로 작성됩니다. 예를 들어, 타입이 데이터 멤버를 데이터로 작성할 수 있는 것처럼 문자열 \= "xyzzy"; , "데이터는 기본값 "xyzzy"를 갖는 것으로 정의된 문자열입니다."라고 발음되는 것처럼 기본 타입은 다음과 같이 작성됩니다: Shape \= (default, values); , "이것은 이러한 기본값을 갖는 것으로 정의된 모양입니다."라고 발음됩니다.

Cpp2 구문에는 별도의 베이스 목록이나 별도의 멤버 이니셜라이저 목록이 없습니다.

기본 및 멤버 서브 객체는 모두 같은 위치(타입 본문)에서 선언되고 같은 위치(함수 본문)에서 초기화되므로 인터리브를 포함하여 어떤 순서로 작성할 수 있으며 선언된 순서대로 안전하게 초기화되도록 보장할 수 있습니다. 즉, Cpp2에서는 데이터 멤버 객체를 기본 하위 객체보다 먼저 선언할 수 있으므로 자연스럽게 기본 하위 객체보다 오래 지속됩니다.

모든 동기 부여 예제를 직접 작성할 수 있기 때문에 Cpp2 코드에는 Boost의 base\_from\_member와 같은 해결 방법이 필요하지 않습니다. 자세한 내용은 이 [설명](https://github.com/hsutter/cppfront/issues/334#issuecomment-1500984173)를 참조하세요.

### virtual, override, 및 final \- 가상 함수

this 매개변수는 다음 중 하나로 추가적으로 선언할 수 있습니다:

* virtual: myfunc 작성: (virtual this /\*...\*/)는 새로운 가상 함수를 정의합니다.

* override: myfunc 작성: (override this /\*...\*/)는 기존 기본 클래스 가상 함수에 대한 재정의가 정의됩니다.

* final: myfunc 작성: (final this /\*...\*/)는 기존 베이스 클래스 가상 함수에 대한 최종 재정의가 정의됩니다.

순수 가상 함수는 virtual this 또는 override this 매개 변수와 본문이 없는 경우입니다.

예를 들어

예: 가상 함수

| abstract\_base: type\= {    //  A pure virtual function: virtual \+ no body    print: (virtual this, msg: std::string);    // ...}derived: type\= {    //  'this' is-an 'abstract\_base'    this: abstract\_base;    //  Explicit override    print: (override this, msg: std::string) \= { /\*...\*/ }    // ...} |
| :---- |

### implicit \- 변환 함수 제어

연산자= 함수의 this 매개변수는 다음과 같이 추가로 선언할 수 있습니다:

* implicit: operator=: (implicit out this, /\*...\*/) 는 Cpp1 구문으로 낮출 때 "명시적"으로 표시되지 않는 함수를 정의합니다.

참고: 이는 생성자가 기본적으로 "명시적"이 아니며 명시적으로 만들려면 "명시적"을 작성해야 하는 Cpp1 기본값을 뒤집는 것입니다.

### operator= \- 구성, 할당 및 소멸

모든 값 연산은 구성, 할당 및 소멸을 포함하여 연산자= 철자가 사용됩니다. operator=는 이 개체의 값을 설정합니다, 따라서 이 매개 변수는 in(즉, const가 아닌 어떤 것으로 전달될 수 있습니다) 이외의 값으로 전달될 수 있습니다:

* out this: 작성 operator=: (out this /\*...\*/ )는 초기화되지 않거나 초기화된 인수를 받을 수 있으므로 생성자이자 할당 연산자이기도 합니다. 보다 특수한 inout 이 할당 연산자를 작성하지 않는 경우, Cpp2는 out this 함수도 할당에 사용합니다.

* inout this: 작성 operator=: (inout this /\*...\*/ )는 할당 연산자입니다(inout 매개변수에는 초기화된 수정 가능한 인수가 필요하므로).

* move this: 작성 operator=: (move this)가 디스트럭터입니다. 다른 매개 변수는 허용되지 않으므로 "이를 아무 데도 이동하지 않습니다."라는 의미입니다.

operator=를 통합하면 컴포저블 보장 초기화에 필수적인 사용 가능한 out 매개 변수를 사용할 수 있습니다. 표현식 구문은 x \= 값으로 생성자나 할당 연산자를 호출할 수 있습니다, 따라서 둘 다 operator=로 명명하는 것이 일관성이 있습니다.

할당 연산자는 항상 이것과 동일한 타입을 반환하고 자동으로 return this;.

참고: \=를 작성하면 항상 연산자가 호출됩니다(실제로는 Cpp2 작성 타입의 경우, 의미론적으로는 Cpp1 작성 타입의 경우). 이렇게 하면 "=를 작성하면 연산자를 호출하지만 그렇지 않은 경우"(예: Cpp1 변수 초기화)를 제외하고는 Cpp1 모순을 피할 수 있습니다. 반대로 operator=는 항상 Cpp2에서 \=에 의해 호출됩니다.

### that \- 소스 매개변수

모든 타입 범위 함수는 복사/이동할 객체의 동의어인 두 번째 매개변수로 that을 사용할 수 있습니다. this와 마찬가지로 타입 범위에서는 타입이 항상 현재 타입이므로 명시적인 : its\_type로 선언되지 않습니다.

that는 in(기본값) 또는 move 매개 변수가 될 수 있습니다. 어떤 것을 선택하느냐에 따라 선언되는 멤버 함수의 종류가 자연스럽게 결정됩니다:

* in that:  myfunc: (/\*...\*/ 이, 그), myfunc의 축약어입니다: (/\*...\*/ 이, in that), 는 당연히 복사 및 이동 함수이며, l값 또는 r값 that 인수를 받을 수 있기 때문입니다. 좀 더 특수한 move that 이동 함수를 작성하지 않으면 Cpp2는 자동으로 in that 함수를 이동에도 사용합니다.

* move that: myfunc: (/\*...\*/ 이, move that) 이동 함수를 정의합니다.

this와 that를 함께 넣습니다: 가장 일반적인 형식의 operator=는 operator=입니다: (out 이, 그). 이는 통합된 일반 {복사, 이동} x {생성자, 할당} 연산자로 작동하며, 더 구체적인 코드를 직접 작성하지 않은 경우 아래 Cpp1 코드에서 이 네 가지를 모두 생성합니다.

### operator=는 생성에서 (A)ssignment를, 복사에서 (M)ove를 일반화할 수 있다

를 일반화할 수 있습니다: \- inout this 함수를 작성하지 않는 경우, Cpp2는 out this 함수를 그 자리에 사용합니다(작성하신 경우). \- move that 함수를 작성하지 않으면 Cpp2는 (작성했다면) 그 자리에 in that 함수를 사용합니다.

참고: Cpp1로 낮추는 경우, 이는 적절한 Cpp2 함수에서 해당 특수 멤버 함수를 생성하는 것을 의미합니다.

이 그래픽은 이러한 일반화를 요약한 것입니다. 편의상 (A)할당과 (M)오버 기본값에 번호를 매겼습니다.  
![][image9]

Cpp1 용어로는 다음과 같이 설명할 수 있습니다:

(M)ove, M1, M2: 복사 생성자 또는 할당 연산자를 작성하지만 해당 이동 생성자 또는 할당 연산자는 작성하지 않은 경우 후자가 생성됩니다.

(A)ssignment, A1, A2, A3: 복사 또는 이동 또는 변환 생성자를 작성했지만 해당 복사 또는 이동 또는 변환 할당 연산자를 작성하지 않은 경우 후자가 생성됩니다.

화살표는 전이적입니다. 예를 들어 복사 생성자만 작성하고 다른 생성자는 작성하지 않으면 이동 생성자, 복사 할당 연산자 및 이동 할당 연산자가 생성됩니다.

M2가 A2보다 선호됩니다. M2와 A2 모두 누락된 (in 이 move that) 함수입니다. 두 가지 옵션을 모두 사용할 수 있는 경우 Cpp2는 A2(이동 구성에서 이동 할당 생성)보다는 M2(복사 구성에서 이동 할당 생성)를 선호합니다. M2가 더 적합하기 때문입니다: 이동 할당은 구조적으로 기존 이 개체의 값을 설정하도록 설계되었기 때문에 이동 할당은 이동 구성보다는 복사 할당과 비슷합니다.

가장 일반적인 operator=와 그가 있는 는 (out 이이죠, that). Cpp1 용어로는 { 복사, 이동 }의 네 가지 조합을 모두 생성합니다. x { 생성자, 할당 }. 이 정도면 충분하므로 이러한 모든 값 설정 함수를 한 번만 작성해도 됩니다. 하지만 다른 작업을 수행하는 보다 구체적인 버전을 작성하고 싶다면 언제든지 작성할 수 있습니다.

참고: inout this (할당)에서 생성 out this도 변환 구조에서 변환 할당을 생성합니다, 새로운 기능입니다. 현재 Cpp1에서는 다른 X 타입에서 변환 생성자를 작성하면 X에서 해당 할당을 작성할 수도 있고 그렇지 않을 수도 있지만 Cpp2에서는 기본적으로 이를 가져와 X의 변환 생성자가 하는 것과 동일한 상태로 객체를 설정하게 됩니다.

기본적으로 생성되는 최소 함수  
언어가 타입에 대해 암시적으로 생성하는 기본값은 두 가지뿐입니다:

모든 타입이 가져야 하는 유일한 특수 함수는 소멸자입니다. 직접 작성하지 않으면 기본적으로 공용 비가상 소멸자가 생성됩니다.

소멸자 이외의 operator= 함수를 직접 작성하지 않으면 기본적으로 공용 기본 생성자가 생성됩니다.

다른 모든 operator= 함수는 직접 작성하거나 메타함수 적용을 선택해서 명시적으로 작성합니다(아래 참조).

참고: 생성 함수는 항상 선택 사항이므로 타입에 맞지 않는 함수가 생성될 수 없으므로 Cpp2에서는 원치 않는 생성 함수를 억제하기 위해 "=삭제"를 지원할 필요가 없습니다.

기본적으로 멤버별  
모든 복사/이동/비교 연산자= 함수는 Cpp2에서 기본적으로 멤버 방식으로 작동합니다. 여기에는 멤버별 구성과 할당을 직접 작성하는 경우도 포함됩니다.

직접 작성한 operator=에서 말이죠:

본문은 선언 순서대로 각 타입의 데이터 멤버(기본 클래스 포함)에 대해 하나씩 일련의 member \= value; 문으로 시작해야 합니다.

본문에 시작 섹션의 적절한 위치에 멤버가 언급되지 않은 경우 기본적으로 멤버의 기본 이니셜라이저가 사용됩니다.

할당 연산자(inout this)에서 member \= \_를 작성하여 멤버 설정을 명시적으로 건너뛸 수 있습니다; 대신 나중에 값을 설정해야 할 이유가 있거나 기존 값을 보존해야 하는 경우에만 설정할 수 있습니다. (이러한 경우는 드물며, 예를 들어 union 메타함수의 생성된 구현을 참조하세요).

예를 들어

멤버별 연산자 \= 의미론

| mytype: type\= {    //  data members (private by default)    name:          std::string;    social\_handle: std::string \= "(unknown)";    //  conversion from string (construction \+ assignment)    operator\=: (out this, who: std::string) \= {        name \= who;        //  if social\_handle is not mentioned, defaults to:        //      social\_handle \= "(unknown)";        //  now that the members have been set,        //  any other code can follow...        print();    }    //  copy/move constructor/assignment    operator\=: (out this, that) \= {        //  if neither data member is mentioned, defaults to:        //      name \= that.name;        //      social\_handle \= that.social\_handle;        print();    }    print: (this) \= std::cout \<\< "value is \[(name)$\] \[(social\_handle)$\]\\n";}//  The above definition of mytype allows all of the following...main: () \= {    x: mytype \= "Jim"; // construct from string    x \= "John";        // assign from string    y := x;            // copy construct    y \= x;             // copy assign    z := (move x);     // move construct    z \= (move y);      // move assign    x.print();         // "value is \[\] \[\]" \- moved from    y.print();         // "value is \[\] \[\]" \- moved from} |
| :---- |

참고: 이렇게 하면 멤버별 시맨틱이 구성과 할당에 대해 대칭을 이룹니다. Cpp1에서는 복사/이동이 아닌 생성자만 기본값이 있는데, 이는 기본 이니셜라이저로 멤버를 초기화하는 것입니다. Cpp2에서는 생성자와 할당 연산자 모두 변환 함수인 경우 기본 초기화자를 사용하고(비thatmember \= that.member;를 사용하는 것으로 기본값이 지정됩니다.

### operator\<=\> \- 통합된 비교 함수

타입에 대한 비교 함수를 작성하려면 일반적으로 operator\<=\>와 operator== 중 하나 또는 둘 모두를 첫 번째 파라미터로 this와 임의의 타입(일반적으로 같은 타입인 that를 두 번째 파라미터로 작성하기만 하면 됩니다.) 함수 본문을 생략하면 기본적으로 멤버별 비교가 생성됩니다.

operator\<=\>는 std::strong\_ordering, std::partial\_ordering 또는 std::weak\_ordering 중 하나를 반환해야 합니다. 타입에 따라 \<, \<=, \>, \>= 비교를 사용할 수 있게 합니다. 부분 정렬이나 약한 정렬을 사용해야 할 이유가 없다면 강한 정렬을 선호합니다. 사용자 지정 함수 본문 없이 operator\<=\>를 작성하면 operator==가 자동으로 생성됩니다.

operator==는 bool를 반환해야 합니다. 타입에 대해 \== 및 \!= 비교를 사용할 수 있습니다.

예를 들어

예: &&; 연산자 쓰기

| item: type \= {    x: i32         \= ();    y: std::string \= ();    operator\<=\>: (this, that) \-\> std::strong\_ordering;        //  memberwise by default: first compares x \<=\> that.x,        //  then if those are equal compares y \<=\> that.y    // ...}test: (x: item, y: item) \= {    if x \!= y {         //  ok        //  ...    }} |
| :---- |

Cpp2의 연산자\<=\> 기능 대부분이 이미 ISO C++(Cpp1)로 병합되었기 때문에 위는 Cpp1에서와 동일하게 적용되었습니다. 또한 Cpp2에서는 우선순위가 같은 비교를 안전하게 연결할 수 있으며, 항상 수학적으로 올바른 전이 의미를 갖거나 그렇지 않으면 컴파일 시 거부됩니다:

유효한 체인입니다: 모든 \</\<=, 모든 \>/\>= 또는 모든 \==. 각 용어에 대한 효율적인 단일 평가를 통해 a \<= b \< c와 같이 수학적으로 건전하고 안전한 모든 체인이 지원됩니다. 이러한 체인은 전이적이기 때문에 "건전"합니다. 이러한 체인은 a와 c 간의 관계를 의미합니다(이 경우 체인은 a \<= c도 참이라는 것을 암시합니다).  
참고: 이러한 유효한 체인은 Cpp1 구문으로 작성된 기존 비교 연산자를 호출할 때에도 항상 수학적으로 예상되는 결과를 제공합니다.

잘못된 체인: 다른 모든 것. a &;= b &;= c 및 a \!= b \!= c 같은 넌센스 체인은 컴파일 시간 오류입니다. 이러한 체인은 비전이적이기 때문에 "넌센스"이며, a와 c 사이에 어떠한 관계도 암시하지 않습니다.

비체인: 혼합 우선순위는 체인이 아닙니다. a\<b \== c\<d와 같은 표현식은 \==보다 우선순위가 낮기 때문에 체인이 아닙니다. 즉, (a\<b) \== (c\<d), true로 평가되는 식은 a\<b 및 c\<d가 둘 다 참이거나 둘 다 거짓이면 표현식은 false로 평가됩니다.

연쇄 비교

| //  If requested is in the range of values \[lo, hi)if lo \<= requested \< hi {    // ... do something ...} |
| :---- |

자세한 내용은 P0515R0 "일관된 비교" 섹션 3.3 및 P0893 "연쇄 비교"를 참조하세요.

## Metafunctions

[https://hsutter.github.io/cppfront/cpp2/metafunctions/](https://hsutter.github.io/cppfront/cpp2/metafunctions/)

## Namespaces

[https://hsutter.github.io/cppfront/cpp2/namespaces/](https://hsutter.github.io/cppfront/cpp2/namespaces/)

네임스페이스 N에는 선언을 포함할 수 있으며, 이 선언은 **N::** 또는 **using** 을 작성하여 액세스하는 네임스페이스 또는 선언을 포함할 수 있습니다. 예를 들어

네임스페이스에 몇 가지 선언하기

| //  A namespace to put all the names provided by a widget librarywidgetlib: namespace \= {    widget: type \= { /\*...\*/ }    // ... more things ...}main: () \= {    w: widgetlib::widget \= /\*...\*/;} |
| :---- |

### using

using 문은 다른 네임스페이스에서 선언된 이름을 마치 현재 범위에서 선언된 것처럼 현재 범위로 가져옵니다. 두 가지 형태가 있습니다:

* using    a\_namespace::a\_name; 단일 이름 a\_name을 범위로 가져옵니다.   
* using namespace   a\_namespace; 모든 네임스페이스의 이름을 범위로 가져옵니다.

예를 들어

| //  A namespace to put all the names provided by a widget librarywidgetlib: namespace \= {    widget: type \= { /\*...\*/ }    // ... more things ...}main: () \= {    //  Explicit name qualification    w: widgetlib::widget \= /\*...\*/;    {        //  Using the name, no qualification needed        using widgetlib::widget;        w2: widget \= /\*...\*/;        // ...    }    {        //  Using the whole namespace, no qualification needed        using namespace widgetlib;        w3: widget \= /\*...\*/;        // ...    }    // ...} |
| :---- |

## 문자열 보간으로 캡처하기

문자열 리터럴은 문자열 리터럴 안에 `(expr)$`를 작성하여 표현식 `expr`의 값을 캡처할 수 있습니다. `(` `)`는 필수이며 중첩할 수 없습니다. 문자열 리터럴은 캡처를 수행하는 경우 `std::string` 타입을 가지며, 그렇지 않으면 일반 C/C++ 문자열 리터럴(문자 배열)입니다.

문자열 리터럴의 모든 캡처는 문자열 리터럴이 기록되는 지점에서 평가됩니다. 문자열 리터럴은 나중에 반복해서 사용할 수 있으며 캡처된 값을 포함합니다.

예를 들어

**문자열 보간을 위한 캡처**

| x := 0;std::cout \<\< "x is (x)$\\n";        // Paste the value of \`x\`x \= 1;std::cout \<\< "now x+2 is (x+2)$\\n";        // Paste the value of \`x+2\`//  prints://      x is 0//      now x+2 is 3 |
| :---- |

문자열 리터럴 캡처에는 `:접미사`가 포함될 수 있으며, 접미사는 [표준 C++ 형식 사양](https://en.cppreference.com/w/cpp/utility/format/spec)입니다. 예를 들어, `(x.price(): <10.2f)$`는 `x.price()`를 평가하고 결과를 10자 폭, 2자리 정밀도, 왼쪽 맞춤으로 변환된 문자열로 변환합니다.

# 코드 스니펫

## 템플릿, range based for

| print: \<T,U,\> (t: T, u: U,) \= std::cout \<\< t \<\< u \<\< "\\n";main: () \= {    array: std::array \= ('A', 'B', 'C',);    for (0, 1, 2,) do (e) {        print( e, array\[e,\], );    }    //  Prints:    //      0A    //      1B    //      2C} |
| :---- |

# 동일한 소스 파일에 Cpp1(오늘날의 C++)과 Cpp2 혼합하기

Cpp1과 Cpp2 코드가 모두 포함된 소스 파일 컴파일하기

Cppfront는 `.cpp2` 파일을 컴파일하고 선호하는 C++20 이상 C++ 컴파일러에서 컴파일할 `.cpp` 파일을 생성합니다.

동일한 `.cpp2` 파일에는 Cpp2 구문과 오늘날의 "Cpp1" C++ 구문인 **이 나란히 있지만 중첩되지 않고 모두 포함될 수 있습니다.**

cppfront는 이렇게 혼합된 파일을 컴파일할 때 Cpp1 코드를 그대로 통과시키고 Cpp2 코드를 제자리에서 Cpp1로 변환합니다. 즉, 호출 사이트(이를 "호출자"라고 함)가 동일한 파일에 작성된 타입/함수/객체(이를 "수신자"라고 함)를 사용할 때 이를 그대로 전달합니다:

* **모든 Cpp2로 작성된 코드는 기본적으로 항상 순서와 무관합니다.** Cpp2 구문으로 작성된 호출자가 Cpp2 구문으로 작성된 호출자를 사용하는 경우 파일에서 어느 순서로든 나타날 수 있습니다.  
* **Cpp1로 작성된 코드는 평소와 같이 순서에 따라 달라집니다.** 호출자나 수신자(또는 둘 다)가 Cpp1 구문으로 작성된 경우 수신자는 호출자 앞에 선언되어야 합니다.

알겠습니다: Cpp1과 Cpp2가 나란히, 인터리브된 경우

예를 들어, 이 소스 파일은 Cpp2와 Cpp1 코드가 나란히 있고 평소처럼 서로를 원활하게 직접 호출하는 데 문제가 없습니다:

| \#include \<iostream\>                             // Cpp1\#include \<string\_view\>                          // Cpp1N: namespace \= {                                        // Cpp2    hello: (msg: std::string\_view) \=                    // Cpp2        std::cout \<\< "Hello, (msg)$\!\\n";                // Cpp2}                                                       // Cpp2int main() {                                    // Cpp1    auto words \= std::vector{ "Alice", "Bob" }; // Cpp1    N::hello( words\[0\] );                       // Cpp1    N::hello( words\[1\] );                       // Cpp1    std::cout \<\< "... and goodnight\\n";         // Cpp1}                                               // Cpp1 |
| :---- |

허용되지 않습니다: Cpp2 안에 Cpp1 중첩(또는 그 반대의 경우)

그러나 다음 소스 파일은 Cpp2 코드를 Cpp1 코드 안에 중첩하려고 시도하고 그 반대의 경우도 마찬가지이므로 유효하지 않습니다:

| \#include \<iostream\>                             // Cpp1\#include \<string\_view\>                          // Cpp1namespace N {                                  // Cpp1    hello: (msg: std::string\_view) \=           // Cpp2 (ERROR here)        std::cout \<\< "Hello, (msg)$\!\\n";       // Cpp2 (ERROR here)}                                              // Cpp1main: () \= {                                    // Cpp2    auto words \= std::vector{ "Alice", "Bob" }; // Cpp1 (ERROR here)    N::hello( words\[0\] );                       // ?    N::hello( words\[1\] );                       // ?    std::cout \<\< "... and goodnight\\n";         // ?}                                               // Cpp2 |
| :---- |

위의 중첩은 구문 분석 문제뿐만 아니라 의미상 모호함을 유발할 수 있으므로 지원되지 않습니다. 예를 들어 11\~13번째 줄은 구문론적으로 Cpp1 또는 Cpp2로 유효합니다, 하지만 Cpp2로 처리하면 `words[0]` 및 `words[1]` 표현식'`std::vector::operator[]` 호출은 기본적으로 경계 검사 및 경계 안전하지만 Cpp1로 취급되는 경우 경계 검사가 이루어지지 않습니다. 그리고 이것은 확실히 알아야 할 매우 중요한 차이점입니다\!

# Cppfront command line options

[https://hsutter.github.io/cppfront/cppfront/options/](https://hsutter.github.io/cppfront/cppfront/options/) 
  
