# 문자열

문자열 = 연속된 문자 + `NUL`

- Byte string: char 배열. `<string.h>`
- Wide string: wchar_t(16/32bit) 배열. `<wchar.h>`

문자 상수 = '문자 하나 혹은 이스케이프 문자'. 정수 타입 4byte.

## 선언

- 문자열 상수: `"문자 배열"`
  - 목적: 초기화
  - 위치: 문자열 리터럴 풀(string literal pool)
- 문자 배열: `char array[SIZE]`
- 포인터: `char *array`

### 문자열 리터럴 풀

- 문자열 풀링 기능: 메모리 공간 절약
- 할당: 읽기 전용 메모리
- 범위: 없음. 전역, 정적, 로컬 상관 없음.

### 초기화

#### 배열 초기화

문자열 크기: 13byte = 12byte + NUL

```c
char header[] = "Media Player";
```

```c
char header[13];
strcpy(header, "Media Player");
```

```c
header[0] = 'M';
header[1] = 'e';
...
header[12] = '\0';
```

다음은 불가능하다

```c
char header[];
header = "Media Player";
```

#### 포인터 초기화

- strlen: 문자열 길이 반환
- sizeof: 배열/포인터 크기 반환

문자열이 힙과 리터럴 풀에 둘다 위치한다.

```c
char *header = (char*)malloc(strlen("Media Player") + 1);
char *header = (char*)malloc(13);
strcpy(header, "Media Player");
```

```c
*(header + 0) = 'M';
*(header + 1) = 'e';
...
*(header + 12) = '\0';
```

문자열이 리터럴 풀에만 위치한다.

```c
char *header = "Meida Player";
```

char 포인터에 문자 상수(정수형)를 할당할 수 없다.

```c
char* prefix = '+';
```

#### stdin 초기화

메모리 할당을 하지 않아 초기화 오류가 일어날 수 있다.

```c
char *command;
scanf("%s", command);
```

#### 문자열 위치 요약

```c
char *globalHeader = "Chapter"; // 전역 포인터(전역 메모리) → 문자열 리터럴 풀
char globalArrayHeader[] = "Chapter"; // 전역 문자열 배열(전역 메모리)

// 프로그램 스택
void displayHeader() {
    static char* staticHeader = "Chpater"; // 정적 문자열 포인터(전역 메모리) → 문자열 리터럴 풀
    char* localHeader = "Chpater"; // 로컬 문자열 포인터 → 문자열 리터럴 풀
    static char staticArrayHeader[] = "Chpater"; // 정적 문자열 배열(전역 메모리)
    char localArrayHeader[] = "Chpater"; // 로컬 문자열 배열(로컬 메모리)
    char* heapHeader = (char*)malloc(strlen("Chpater") + 1); // 로컬 문자열 포인터 → 힙 문자열
    strcpy(heapHeader, "Chpater"); // 힙 문자열
}
```

## 문자열 연산

### 비교

```c
int strcmp(const char *s1, const char *s2);
```

- 음수: s1 < s2
- 0: s1 == s2
- 양수: s1 > s2

### 복사

s2 문자열을 s1으로 복사한다.

```c
char* strcpy(char *s1, const char *s2);
```

### 연결

첫 번째 문자열 + 두 번째 문자열

```c
char *strcat(char *s1, const char *s2);
```

연결된 문자열은 새로 공간을 할당해서 저장한다.

## 문자열 전달

```c
method(simpleArray); // 문자열 주소
method(&simpleArray); // 주소의 포인터
method(&simpleArray[0]); // 첫 문자의 주소 = 문자열 주소
```

```c
void method(char* string);
void method(const char* string);
```

### 문자열 초기화

1. **반드시** 버퍼 주소, 크기 전달
   - 배열: sizeof(배열 주소)
   - 동적할당: 할당한 크기
2. 반환: 버퍼 포인터
3. 버퍼 해제 위치: 함수 호출한 곳

버퍼 주소 NULL 전달할 경우: 함수가 어떻게 사용될지 완전한 이해 필요

### 애플리케이션 인자

```c
int main(int argc, char** argv);
int main(int argc, char* argv[]);
```

- argc: 인자 수.
  - 최소 1: 실행 파일 이름
- argv: 문자열 포인터 배열

## 문자열 반환

### 리터럴 문자열 주소 반환

```c
return "문자열 상수";
```

정적 문자열 상수는 항상 덮어씌워진다.

```c
static char mystring[] = "정적 문자열 상수";
return mystring;
```

### 동적 할당 문자열 주소 반환

함수를 호출한 쪽에서 메모리를 해제해야 한다.

### 로컬 문자열 주소 반환

하면 안 된다.

로컬 문자열 배열로 할당을 하면 나중에 스택 프레임 안에서 내용이 변경된다.


