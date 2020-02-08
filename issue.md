# 오남용 문제

> 애플리케이션의 보안에 대한 책임은 그 대부분이 개발자에게 있다.

- 주소 영역 배치 랜덤화. Address Space Layout Randomization. ASLR.
  - 메모리 내 애플리케이션의 데이터 영역을 랜덤하게 배치
- 데이터 실행 방지. Data Execution Prevention. DEP.
  - 메모리의 실행 불가능한 영역에 있는 코드 실행 차단

## 선언과 초기화

각 변수는 다른 줄에 선언하는 습관을 들여야 한다.

포인터 변수 하나와 정수 변수 하나로 선언된다.

```c
int* ptr1, ptr2;

#define PINT int*;
PINT ptr1, ptr2;
```

다음 선언이 올바르다.

```c
int *ptr1, *ptr2;

typedef int* PINT;
PINT ptr1, ptr2;
```

- 포인터는 언제나 NULL로 초기화한다.
- 디버그 과정에서 assert 함수를 사용한다.
- 서드파티 도구를 사용한다.

## 포인터 사용 이슈

버퍼 오버플로: 객체의 영역을 벗어난 영역의 메모리가 덮어 쓰일 때 발생

운영체제는 segmentation fault를 발생시키고 프로그램을 강제 종료한다.

- 배열 요소 접근 시 인덱스 값 확인하지 않음
- 배열 포인터 연산을 주의하지 않음
- 표준 입력을 받을 때 gets 같은 함수 사용함
- strcpy, strcat 같은 함수 부적절하게 사용함

- 애플리케이션 주소 공간 내 버퍼 오버플로: 허가되지 않은 데이터 접근. 다른 세그먼트로 제어 넘어가 시스템 파괴. 관리자 권한으로 실행된 애플리케이션이면 매우 위험.
- 스택 프레임 내 버퍼 오버플로: 복귀 주소 부분을 악성 코드 주소로 덮어 씀.

### 확인 목록

- NULL 확인
- 선언과 역참조 연산자 차이 확인
  - 선언: `int *pi = &num`
  - 역참조: `*pi = &num`. 잘못됨.
- 댕글링 포인터 확인
- 배열 인덱스 범위 확인
- 배열 크기 확인: 항상 배열과 배열 크기를 함께 전달.
- sizeof 연산자 확인
  - `sizeof(배열 포인터)`: 배열 전체 크기
  - `sizeof(배열 포인터) / sizeof(배열 데이터 타입)`: 배열 크기
- 포인터 타입 일치 확인
- 유계 포인터. Bounded pointers 확인: 유효한 영역 내에서만 사용 가능한 포인터인지 확인
  - 유효성 검증 함수 생성. pointer validation function.
  - Bounded Model Checking for ANSI-C and C++(CBMC)
- 문자열 보안 이슈 확인
  - 버퍼 오버플로 가능한 문자열 함수 주의
  - 포맷 문자열 공격 주의
  - 사용자가 입력한 포맷 문자열 사용 금지
- 구조체에서 포인터 연산 사용 금지
- 함수 포인터 사용 확인
  - 함수 이름 = 함수 포인터 주소 = 항상 0보다 크다 = true
  - 함수 포인터 매개변수 확인

## 메모리 해제 이슈

1. 메모리 이중 해제
2. 메모리 해제 후 남아있는 데이터 보호 필요

```c
char name[32];
char *securityQuestion;

memset(name, 0, sizeof(name));
memset(securityQuestion, 0, strlen(securityQuestion));
```

## 정적 분석 도구 사용

Static Analysis Tool



