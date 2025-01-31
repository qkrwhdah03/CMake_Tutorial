# CMAKE_TUTORIAL
## Environment
- Windows 11
- Visial Studio 2022
- CMake 3.30.3 

## Tutorials
<details>
<summary>Step1 - Basic Structure of CMake </summary>

### Step1 - Basic Structure of CMake

**Commands**
- `add_executable()` : 실행파일 생성 
- `cmake_minimum_required()` : 최소 요구 CMake 버전을 명시
- `project()` : 프로젝트 명 설정
    - `<PROJECT_NAME>` 변수에 프로젝트명을 저장
    - 이외에도 다른 변수들을 set하게 됨 (공식문서 참고)
    - `<PROJECT_NAME>_VERSION_MAJOR`, `<PROJECT_NAME>_VERSION_MINOR`도 설정 가능
- `set()` : 변수 값 설정
- `configure_file()` : 템플릿 파일을 이용해 CMake 변수들로 치환하여 파일 생성
- `target_include_directories()` : 타겟을 컴파일 시, include할 때 검색하는 directory를 명시

**Variables**
- `CMAKE_CXX_STANDARD` : 11은 c++ 2011 표준을 의미 (공식문서 참고)
- `CAMKE_CXX_STANDART_REQUIRED` : TRUE / FALSE
- `<PROJECT_NAME>_VERSION_MAJOR` : 프로젝트의 First Version Number
- `<PROJECT_NAME>_VERSION_MINOR` : 프로젝트의 Second Version Number
- `PROJECT_BINARY_DIR` : 현재 프로젝트의 빌드 디렉토리의 경로

**Understanding**
1. cmake_minimum_required로 최소 CMake 버전 3.10을 명시
2. 프로젝트 명을 step1으로 설정하고, 프로젝트 버전 1.0 명시
3. c++ 표준 설정
4. main.cpp 파일을 이용해 main 실행파일을 생성
5. config.h.in 파일의 CMake 변수들을 치환하여 config.h 파일을 생성. 생성된 헤더파일은 `$PROJECT_BINARY_DIR`에 생김
6. main을 컴파일할 때, `$PROJECT_BINARY_DIR`를 include 경로에 추가함.

**Questions**

Q1: 3.30.3 버전의 CMake를 설치했는데, 왜 `cmake_minimum_required()`의 버전은 이보다 낮게 설정해야하는가?

A: `cmake_minimum_required()`의 공식문서를 보면 최대 지정가능한 버전이 명시되어 있음.

Q2: 왜 표준 라이브러리는 CMake에서 따로 링크해줄 필요가 없는가?

A: iostream과 같은 표준 라이브러리는 컴파일러가 이를 자동으로 찾고 링크해준다.

Q3: `configure_file()`이 무슨 역할인가?

A: config.h.in 템플릿 파일에서 정의된 플레이스홀더(예: @PROJECT_NAME@, @VERSION@)를 CMake 변수를 사용해 실제 값으로 치환한다. 이를 통해 프로젝트 버전이나 옵션 등을 소스 코드에서 쉽게 참조할 수 있다.
</details>

<details>
<summary> Step2 - Adding a Library </summary>

### Step2 - Adding a Library
**Commands**
- `add_library()` : 라이브러리를 프로젝트에 추가함
- `add_subdirectory()` : 여기에 들어가는 디렉토리에는 CMakeLists.txt가 포함되어야 함. 해당 디렉토리를 로드하고, 그 디렉토리의 CMakeLists.txt를 실행해줌.
- `target_link_libraries()` : 라이브러리를 실행파일에 링크해줌

**Variables**
- `PROJECT_SOURCE_DIR` : 최상위 소스 트리 디렉토리

**Understanding**
1. step1과 동일하게 CMake 버전, 프로젝트 이름 및 버전, C++ 표준을 명시함
2. add_subdirectory를 통해 MyMath 디렉토리를 로드함. 이때, MyMath의 CMakeLists.txt가 실행됨. 
3. add_library를 통해 MyMath.cpp를 통해 mymath 라이브러리가 만들어짐.
4. 다시 돌아와서 main.cpp를 통해 main 실행파일을 생성.
5. 3에서 만들어진 mymath 라이브러리를 main에 링크
6. main에서 MyMath 헤더를 쓸 수 있도록 include 경로에 추가해줌.

**Questions** 

Q1: 5,6번 과정에서 라이브러리를 링크하는 것과 include해서 쓰는 것은 어떤 차이인가?

A:

라이브러리 링크: 링크 단계에서 실행파일이 라이브러리의 코드에 접근할 수 있도록 결합하는 작업.

include 경로 추가: 컴파일 단계에서 헤더 파일의 선언을 찾을 수 있도록 경로를 지정하는 작업.

</details>

<details>
<summary> Step3 - Adding an Option </summary>

### Step3 - Adding a Library
**Commands**
- `if()` / `endif()`- 명령어의 조건부 실행
- `option()` - 사용자가 설정할 수 있는 불린 옵션 (ON / OFF)을 제공
- `target_compile_definitions()` - 지정된 타깃을 컴파일 할때, 컴파일 정의(definition)을 지정 (즉, #ifdef에 사용될 수 있음)
    - `add_compile_definitions()` 함수도 위와 컴파일 정의(definition)를 추가하는데 사용

**Variables**

None

**Understanding**
1. 우선 step2의 과정은 생략한다.
2. option을 통해서 USE_MYMATH를 ON으로 정의한다. 
3. if문에서 USE_MYMATH가 ON이므로 참이다.
4. if문 안에서 target_compile_definition이 실행되고, "USE_MYMATH"가 mymath 컴파일 시에 정의된다.

**Questions** 

None
</details>

<details>
<summary> Step4 - Adding Usage Requirements for Library </summary>

### Step4 - Adding Usage Requirements for Library 
**Commands**
- `target_compile_options()` - `COMPILE_OPTIONS`나 `INTERFACE_COMPILE_OPTIONS`에 타깃이 컴파일 될때 사용되는 옵션을 추가 
- `target_link_options()` - 링크 과정에서의 옵션을 추가
- `target_link_directories()` - 타깃에 링크 리렉토리를 추가. 즉, 링커가 해당 타깃을 링킹할 때, 라이브러리를 탐색할 경로를 추가
- `target_precompile_headers()` - 미리 컴파일된 헤더(Precompiled Header)를 추가 - 이를 통해 컴파일 속도를 향상시킬 수 있음
- `target_sources()` - 타깃에 소스를 추가.
- `target_compile_featuers()` - Add expected compiler features to a target.

**Variables**
- `CMAKE_CURRENT_SOURCE_DIR` :  현재 처리되고 있는 소스 트리의 디렉토리

**Understanding**
1. 우선 `target_include_directories()`를 MyMath 안으로 옮겨서 `CMAKE_CURRENT_SOURCE_DIR`을 추가해줌. 즉, top-level 디렉토리에서 include 디렉토리를 지정해주지 않아도 됨.
2. 인터페이스 라이브러리를 통해 타깃의 요구사항을 지정해줄 수 있다. 이번 예제에서는 C++ 11 요구사항을 지정해주었다.
- 우선 `add_libraary()`를 통해 인터페이스 라이브러리를 생성한다
- `target_compile_feature()`를 통해 c++ 11 컴파일 요구사항을 지정
- `target_link_libraries()`를 통해 해당 요구사항을 만족해야하는 타깃을 지정

With this, all of our code still requires C++ 11 to build. Notice though that with this method, it gives us the ability to be specific about which targets get specific requirements. In addition, we create a single source of truth in our interface library.

**Questions** 

Q1: INTERFACE 라이브러리란?

A: 헤더 전용 라이브러리 또는 구현체가 없는 라이브러리를 정의하는 데 사용된다. 주로 빌드에 필요한 컴파일러 옵션, include 디렉토리, 링크 대상과 같은 사용자 정의 정보를 의존하는 대상에 전달하는 역할을 한다.

</details>

<details>
<summary> Step5 - Installing and Testing </summary>

### Step5 - Installing and Testing
**Commands**
- `install()` - generates installation rules for a project

**Variables**
- `CMAKE_INSTALL_PREFIX`- install directory used by `install()`
    - Default value `C:/Program Files/${PROJECT_NAME}` on Windows


None

**Understanding**
- 빌드 후에 빌드 폴더 내에서 다음 명령어를 통해 install이 가능하다
- `cmake --install . --prefix [INSTALLATION PATH]`
- `--prefix`는 `CMAKE_INSTALL_PREFIX`를 지정
- CMakeLists.txt 파일에 정의된 installation 규칙에 따라 프로젝트가 설치됨

**Questions** 

</details>


## References
[1] https://cmake.org/cmake/help/v3.30/guide/tutorial/index.html

