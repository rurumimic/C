# C

씨-언어

## 내용

[책](#책) 내용을 간단하게 정리했다.

1. [포인터](pointer.md)
2. [동적 메모리](dynamic.md)
3. [함수](function.md)
4. [배열](array.md)
5. [문자열](strings.md)
6. [구조체](struct.md)
7. [오남용 문제](issue.md)
8. [기타](etc.md)

## 책

Understanding and Using C Pointers / Richard Reese  
씨 포인터의 이해와 활용 / 조인중, 강성용

> 포인터를 얼마나 확실하게 이해하고 효율적으로 사용할 수 있는가로 초보와 전문 C 프로그래머를 구분할 수 있다.

## 역사

[Wikipedia: C History](https://en.wikipedia.org/wiki/C_(programming_language)#History)

### 초기 개발

1972년 데니스 리치는 PDP-11의 Unix에 사용할 C 언어를 만들었다. Unix 버전 2에는 C 컴파일러와 유틸리티가 포함됐다. Unix 버전 4부터 C로 만든 커널을 사용했다. 이때부터 `struct`처럼 강력한 기능들이 추가됐다. 1977년 경, 리치와 스티븐 C. 존슨이 Unix의 이식성을 향상시키기 위해 C 언어를 추가적으로 변경했다. 존슨의 Portable C Compiler는 새로운 플랫폼에서 C를 사용하기 위한 기초가 되었다.

### K&R C

1978년, 브라이언 커니핸과 데니스 리치가 *The C Programming Language*라는 책을 출간했다. 이 책을 "K&R"이라고 부른다.

K&R 기능
- 표준 입출력 라이브러리
- `long int` 자료형
- `unsigned int` 자료형
- 복합 대입 연산자를 `=op`에서 `op=` 형태로 변경
- 함수 리턴값 기본으로 `int`이다.

몇년 후 기능들이 추가됐다.
- `void` 함수
- `struct`, `union` 자료형을 함수 반환값으로 사용 가능
- `struct` 자료형 선언
- `enum` 자료형

### ANSI C

- C89: 미국 국립 표준 협회에서는 C의 표준을 만들었다. ANSI C, 표준 C, C89라고 부른다.
- C90: 국제 표준화 기구에서는 C89를 표준으로 등록했다. C90이라고 부른다. C89와 똑같다.
- C95: 개정판. 확장문자 지원 등 기능 추가.
- C99: 인라인 함수, 새로운 자료형, 가변길이 배열, 가변 배열 멤버, `int` 선언 생략 불가.
- C11: 타입 제너릭 매크로, 익명 구조체, 유니코드 지원, 멀티스레드 등.
- C18: 새로운 기능 없음.
- Embedded C
