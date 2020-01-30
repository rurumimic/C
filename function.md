# 함수

- 함수 표기법이 곧 함수 포인터 표기법이다.
- 함수의 이름을 함수의 주소로 사용한다.

1. 함수에 포인터를 전달해 효과적으로 데이터를 관리한다.
2. 함수 포인터로 프로그램의 실행 흐름을 제어한다.

## 프로그램 스택

- 함수의 실행을 지원하기 위한 메모리 영역.
- 스택과 힙이 사용하는 메모리 영역은 같다.
- 힙: 메모리 높은 곳 위치. 어디에나 할당 가능. 단편화 발생.
- 스택: 메모리 낮은 곳 위치. 
- 스레드마다 프로그램 스택이 할당된다.

### 스택 프레임

- 스택 포인터: 제일 높은 위치를 가리킨다. 런타임 시스템이 사용한다.
- 로컬 변수 저장소: 나열된 순서 반대로 추가된다.
- 기반 포인터 base pointer: 보통 반환 주소를 가리킨다. 스택 프레임 요소 접근을 돕는다. 런타임 시스템이 사용한다.
- 반환 주소: 함수 종료 후 돌아가야 할 프로그램 내의 주소
- 매개변수 저장소: 선언과 반대 순서로 프레임에 추가된다.

## 포인터 전달

- 전역으로 선언할 필요없다.
- 데이터 수정: 포인터 전달
- 데이터 보호: 상수 포인터 전달
- 포인터 수정: 포인터의 포인터 전달

## 포인터 반환

### 함수 종료 시 반환

```c
int* allocateArray(int size, int value) {
    int* arr = (int*)malloc(size * sizeof(int));
    for (int i = 0; i < size; i++) {
        arr[i] = value;
    }
    return arr;
}

int* vector = allocateArray(5, 45);
free(vector);
```

- 초기화되지 않은 포인터 반환 가능
- 잘못된 주소 가리키는 포인터 반환 가능
- 로컬 변수를 가리키는 포인터 반환 가능
- 반환된 포인터 메모리 해제 실패 가능

### 로컬 데이터 포인터

반환되는 즉시 유효하지 않다.

#### static

- 스택 프레임 바깥에 할당
- 고정된 크기의 정적 배열
- 함수가 호출될 때마다 변수 재사용
- 수정되지 않을 제한된 목록 중 값 하나를 반환하는 기능으로 사용하기 좋다.

```c
int* allocateArray(int size, int value) {
    static int arr[5];
    return arr;
}
```

### Null 포인터 전달

```c
int* allocateArray(int* arr, int size, int value) {
    if (arr != NULL) {
        for (int i = 0; i < size; i++) {
            arr[i] = value;
        }
    }
    return arr;
}

int* vector = (int*)malloc(5 * sizeof(int));
allocateArray(vector, 5, 45);
```

#### 포인터의 포인터 전달

```c
void allocateArray(int** arr, int size, int value) {
    *arr = (int*)malloc(size * sizeof(int));
    if (*arr != NULL) {
        for (int i = 0; i < size; i++) {
            *(*arr+i) = value;
        }
    }
}

int* vector = NULL;
allocateArray(&vector, 5, 45);
```

다음은 동작하지 않는다. 포인터 값이 복사만 되기 때문이다.  
새로 할당받은 메모리 영역이 복사된 변수에 할당되어 쓸 수 없다.  
메모리 누수가 일어난다.

```c
void allocateArray(int* arr, int size, int value) {
    arr = (int*)malloc(size * sizeof(int));
    if (*arr != NULL) {
        for (int i = 0; i < size; i++) {
            arr[i] = value;
        }
    }
}

int* vector = NULL;
allocateArray(&vector, 5, 45);
```

### 사용자 정의 free 함수

```c
void saferFree(void **pp) { // 포인터의 포인터
    if (pp != NULL && *pp != NULL) { // 이미 NULL 값인지, 포인터가 가리키는 포인터가 NULL 포인터인지
        free(*pp); // 포인터 해제: 메모리를 반환한다.
        *pp = NULL; // 포인터 값에 NULL 저장
    }
}

#define safeFree(p) saferFree((void**))&(p))

int* pi = (int*)malloc(sizeof(int));
safeFree(pi);
safeFree(pi); // 이미 NULL 값이어도 오류 없음
```

1. 포인터의 포인터: 포인터 수정 가능
1. void: 모든 타입 가능
1. 명시적 캐스팅 필요

## 함수 포인터

함수가 할당되는 세그먼트 영역은 프로그램 스택과 다르다.

### 프로세서

1. 다음 실행할 분기 처리
2. 정확한 분기 예측 후 파이프라인 명령 유지
3. 성능 향상

- 분기 처리(branch prediction): 명령 실행 순서 예측 및 실행
- 파이프라이닝(pipelining): 하드웨어 기술. 명령 중첩 실행.

함수 포인터를 사용하면 프로그램이 느려질 수도 있다.

### 선언

```c
void (*foo)();
```

```c
double* (*foo)(int, int*);
```

### 사용

```c
int (*fptr)(int);
int square(int num) {
    return num * num;
}
fptr = square;
fptr(5);
```

```c
typedef int (*funcptr)(int);

funcptr fptr;
fptr = square;
fptr(5);
```

### 전달

```c
typedef int (*fptrOperation)(int, int);
int compute(fptrOperation operation, int a, int b) {
    return operation(a, b);
}
compute(add, 2, 3);
```

### 반환

```c
fptrOperation select(char opcode) {
    switch(opcode) {
        case '+': return add;
        case '-': return sub;
    }
}
int evaluate(char opcode, int a, int b) {
  fptrOperation operation = select(opcode);
  return operation(a, b);
}
evaluate('+', 2, 3);
```

### 함수 포인터 배열

```c
typedef int (*operation)(int, int);
operation operations[128] = { NULL };
// or
int (*operations[128])(int, int) = { NULL };

void init() {
    operation['+'] = add;
    operation['-'] = sub;
}

int evaluateArray(char opcode, int a, int b) {
    fptrOperation operation = operations[opcode];
    return operation(a, b);
}

init();
evaluateArray('+', 2, 3);
```

### 비교

```c
fptr == add
```

### 캐스팅

- 캐스팅이 가능하다.
- 함수 포인터와 데이터 포인터 사이 변환은 실패할 수도 있다.
- 함수 포인터를 void*에 할당할 수 없다.

```c
typedef int (*fptrA)(int);
typedef int (*fptrB)(int, int);
fptrB fptr1 = add;
fptrA fptr2 = (fptrA)fptr1;
fptr1 = (fptrB)fptr2;
fptr1(2, 3);
```

#### 기본 함수 포인터 타입

```bash
typedef void (*fptrBase)();
```

```c
fptrB fptr1 = add;
fptrBase basePointer = (fptrBase)fptr1;
fptr1 = (fptrB)basePointer;
fptr1(2, 3);
```
