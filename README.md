# CMAKE_TUTORIAL
## Environment
- Windows 11
- Visial Studio 2022
- CMake 3.30.3 

## Tutorials
### Step1 - Basic Structure of CMake
우선, 다음을 찾아 이해하자.

**Commands**
- add_executable()
- cmake_minimum_required() 
- project()
    - \<PROJECT_NAME\> 변수에 프로젝트명을 저장
    - 이외에도 다른 변수들을 set하게 됨 (공식문서 참고)
    - \<PROJECT_NAME\>_VERSION_MAJOR, \<PROJECT_NAME\>_VERSION_MINOR도 설정 가능
- set()
- configure_file()
- target_include_directories()

**Variables**
- CMAKE_CXX_STANDARD
- CAMKE_CXX_STANDART_REQUIRED
- \<PROJECT_NAME\>_VERSION_MAJOR : First Version Number
- \<PROJECT_NAME\>_VERSION_MINOR : Second Version Number

**Questions**

Q1: 3.30.3 버전의 CMake를 설치했는데, 왜 cmake_minimum_required의 버전은 이보다 낮게 설정해야하는가?

A: cmake_minimum_required의 공식문서를 보면 최대 지정가능한 버전이 명시되어 있음.

Q2: 왜 표준 라이브러리는 CMake에서 따로 링크해줄 필요가 없는가?

A: iostream과 같은 표준 라이브러리는 컴파일러가 이를 자동으로 찾고 링크해준다.

Q3: configure_file()이 무슨 역할인가?

A: config.h.in 템플릿 파일에서 정의된 플레이스홀더(예: @PROJECT_NAME@, @VERSION@)를 CMake 변수를 사용해 실제 값으로 치환한다. 이를 통해 프로젝트 버전이나 옵션 등을 소스 코드에서 쉽게 참조할 수 있다.

## References
[1] https://cmake.org/cmake/help/v3.30/guide/tutorial/index.html

