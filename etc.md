# 기타

## 포인터 캐스팅

- 특수 목적 주소 접근 가능
- Port 대응 주소 할당 가능
- 시스템 엔디안 종류 확인

### Handle

- handle: 시스템 리소스 간접 접근
- pointer: 리소스 주소 직접 가리킴

### 특수 목적 주소 접근

```c
#define VIDEO_BASE 0xB8000
int *video = (int*) VIDEO_BASE;
*video = 'A';
```

메모리 주소 0 접근 방법

- 포인터를 0으로 설정
- 정수 변수 0 → 정수 포인터로 캐스팅
- 유니언 사용
- `memset((void*)&ptr, 0, sizeof(ptr))`

### 포트 접근

```c
#define PORT 0xB0000000
unsigned int volatile * const port = (unsinged int *)PORT;

*port = 0x0BF4;
value = *port;
```

- 포트 주소: 16진수 값
- volatile 키워드 제한자: 변수가 프로그램 밖에서 변경될 수 있음.
  - 외부 장치가 포트에 데이터 기록 가능
  - 런타임 시스템이 캐시/레지스터에 임시(컴파일러 최적화 기능)로 포트 값을 저장하지 못하게 함
  - 포트에서 직접 데이터 읽기/쓰기

### DMA로 메모리 접근

직접 메모리 접근. Direct Memory Access

- 메인 메모리와 특정 장치 간 데이터 전송 보조하는 저수준 동작
- CPU와 병행으로 이루어진다.
- CPU는 장치와 데이터 전송에 관여하지 않아 성능이 좋아진다.
- 프로그램: DMA 함수 호출 후 완료 대기 → 운영체제: 콜백 함수 호출

### 시스템 엔디안 종류

메모리 단위에서 바이트 배열 순서

- little endian: 정수의 최하위 비트 least significant bit는 4byte 중 가장 낮은 주소에 저장.
- big endian

## Aliasing

- 두 포인터가 같은 메모리 위치 참조: A 포인터는 B 포인터에 대한 alias

### Strict Aliasing

한 데이터 타입에 대한 포인터가 타입이 다른 데이터에 대해 에일리어싱을 할 수 없다.

예: `int*` → `float*`. 규칙 위반

기호나 한정자만 다르게 선언된 포인터에 대해서는 적용되지 않는다.

```c
int num;
const int *ptr1 = &num;
int *ptr2 = &num;
int volatile *ptr3 = &num;
```

에일리어싱 문제 회피 방법

- 유니언 사용
- 엄격한 에일리어싱 비활성화: 컴파일러 옵션 설정
- char* 사용: 문자 포인터는 항상 잠재적인 에일리어스라고 가정됨. 안정하게 사용 가능.

## 유니언

```c
typedef union _conversion {
    float fNum;
    unsigned int uiNum;
} Conversion;

int isPositive1(float numer) {
    Conversion conversion = { .fNum = number };
    return (conversion.uiNum & 0x80000000) == 0; 
}
```

## 엄격한 에일리어싱

컴파일러는 경고만 표시한다. 컴파일러는 다른 타입 포인터들이 같은 객체를 절대 참조하지 않을 것이라고 가정한다.

각각의 구조체에 대한 포인터는 절대 같은 객체를 참조해서는 안 된다.

구조체 정의가 같고 이름만 다른 경우에는 다수 포인터가 하나의 객체를 참조할 수 있다.

## restrict 키워드

C 컴파일러는 포인터가 기본적으로 에일리어스되었다고 가정한다.

restrict 키워드로 컴파일러에게 포인터가 에일리어스되지 않음을 알린다.

최적화가 가능하다.

## 스레드

생략

## 객체 지향 기법과 다형성

생략