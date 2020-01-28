# 동적 메모리 관리

Dynamic Memory Management

```c
#include <stdlib.h>

int *pi = (int*) malloc(sizeof(int));
*pi = 5;
free(pi);
pi = NULL;
```

## 메모리 누수

- 메모리 주소 잃어버리는 경우
- 메모리 해제하지 않은 경우

메모리 누수가 쌓이면 추가 메모리를 할당할 수 없어 Out Of Memory 오류로 프로그램이 종료된다.

## 할당 함수

**힙 관리자가 할당한 메모리 크기** = **할당할 메모리 크기 + 관리용 메모리 크기**

- `malloc`: 힙에서 메모리 할당
- `calloc`: 힙에서 메모리 할당 + 0으로 설정
- `realloc`: 기존 할당된 메모리 크기 변경
- `free`: 할당된 메모리를 힙으로 반환

## malloc

```c
void* malloc(size_t);
```

- 인자: 
  - `size_t` 타입: 할당될 byte 수
- 반환:
  - `void 포인터`: 명시적 캐스팅하는 것이 호환성에 좋음.
  - `NULL`

**다음 코드처럼 습관을 들인다.**

```c
int *pi = (int*) malloc(sizeof(int));

if (pi != NULL) {
    // 할당 성공
} else {
    // 할당 실패
    // 유효하지 않은 메모리 참조할 경우 Runtime Exception 발생
}
```

## 정적/전역 포인터 메모리 할당

정적/전역 변수에 malloc 함수로 초기화하면 컴파일 오류가 발생한다.

```c
static int *pi = malloc(sizeof(int)); // 컴파일 오류
```

하지만 정적 변수는 함수 안에서 할당문을 분리할 수 있다.

```c
static int *pi;
pi = malloc(sizeof(int));
```

초기화 연산자(=)과 할당 연산자(=)는 다르다.

## calloc

메모리 할당과 동시에 메모리 내용을 0으로 초기화한다.

```c
void* calloc(size_t numElements, size_t elementSize);
```

- 인자:
  - `numElements` 
  - `elementSize`
- 반환:
  - `void 포인터`
  - `NULL`

```c
int *pi = calloc(5, sizeof(int));
// 다음과 같다.
int *pi = malloc(5 * sizeof(int));
memset(pi, 0, 5 * sizeof(int));
```

## realloc

메모리를 재할당한다.

```c
void* realloc(void *ptr, size_t size);
```

- 인자:
  - `*ptr`: 기존 할당된 메모리에 대한 포인터
    - null인 경우: `malloc`과 같은 기능
  - `size`: 요청할 메모리 크기
    - 0인 경우: 기존 메모리 해제
- 반환:
  - `void 포인터`
  - `NULL`

## 참고: alloca

**사용하지 않는 것이 좋다.**

함수 내부에서 사용할 스택 프레임 안의 메모리를 할당한다.  
함수가 끝나면 메모리는 자동으로 해제된다.

## 참고: 가변 길이 배열

C99 표준에서 도입되었다.

```c
void compute(int size) {
    char buffer[size];
}
```

함수 내에서 변수 기반의 크기를 가지는 배열의 선언과 생성이 가능하다.  
스택 프레임의 일부에 메모리가 할당된다.  
함수가 종료될 때 자동으로 해제된다.  
이미 크기가 결정된 다음에는 크기를 변경할 수 없다.

## free

```c
void free(void *ptr);
```

이중 해제(double free)를 하면 런타임 예외가 발생한다.

의도치 않은 메모리 누수를 쉽게 찾을 수 있도록 메모리 해제를 하는 습관을 들인다.

## Dangling Pointers

해제된 메모리 영역을 가리키고 있는 포인터  
너무 빠른 해제(premature free)라고 부른다.

- 메모리 해제 후 다시 접근할 경우 에러
- 스택 프레임의 변수 주소를 블록 구문 밖에서 접근할 경우 에러

### 댕글링 포인터 처리 방법

- 메모리 해제 후 포인터를 NULL로 설정
- 사용자 정의 free 함수 작성
- 런타임/디버깅 시스템이 특별한 값으로 덮어쓰기
- 서드파티 도구 사용
