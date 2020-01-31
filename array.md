# 배열

- 배열은 연속적으로 저장된 같은 종류의 데이터들이다.
- 인덱스로 접근한다.
- 선언할 때 크기가 결정된다.

## 선언

```c
int vector[5];
int vector[5] = { 1, 2, 3, 4, 5 };
int vector[2][3] = { {1, 2, 3}, {4, 5, 6} };
```

## 포인터와 차이점

```c
int vector[5] = { 1, 2, 3, 4, 5 };
int* pv = vector;
int* pv = &vector[0];
```

결과는 같지만, 기계어는 다르다.

```c
vector + i
&vector[i]
pv + i
&pv[i]
```

```c
vector[i]
*(vector + i)
pv[i]
*(pv + i)
```

```c
pv = pv + 1;            // 정상
pv = vector + 1;        // 정상
vector = vector + 1;    // 구문 오류
```

## malloc

```c
int* pv = (int*)malloc(5 * sizeof(int));
for (int i = 0; i < 5; i++) {
    pv[i] = i + 1;
    *(pv + i) = i + 1;
}
```

## 전달

배열의 크기를 명시한다.

```c
void displayArray(int arr[], int size); // 배열 주소, 배열 크기(요소의 수)
displayArray(vector, 5);
displayArray(vector, sizeof(vector)); // 잘못됨.
displayArray(vector, sizeof(vector) / sizeof(int)); // 배열 메모리 크기 / 요소 메모리 크기 = 요소의 수.
```

포인터 표기법 사용과 배열 표기법 둘다 가능하다.

```c
void displayArray(int arr[], int size);
void displayArray(int* arr, int size);

arr[i]
*(arr + i)
```

## 1차원 포인터 배열 할당

```c
int* arr[5];
for (int i = 0; i < 5; i++) {
    arr[i] = (int*)malloc(sizeof(int));
    *arr[i] = i; // 포인터 배열의 i번의 값(= 주소값)으로 접근해 i를 저장.
}

for (int i = 0; i < 5; i++) {
    *(arr + i) = (int*)malloc(sizeof(int)); // 포인터 배열의 i번의 값으로 접근해 정수 크기 메모리 할당
    **(arr + i) = i; // 포인터 배열의 i번의 값(= 주소값)으로 접근해 i를 저장.
}
```

## 2차원 포인터 배열

```c
int matrix[2][5];
int (*pmatrix)[5] = matrix;
```

메모리 주소가 4 * 5 = 20byte 차이난다.

```c
matrix + 1
```

다른 요소에 접근하는 방법이다.

```c
*(matrix[i] + j)
```

## 다차원 배열 전달

```c
void display2DArray(int arr[][5], int rows);
void display2DArray(int (*arr)[5], int rows);
void display2DArray(int *arr[5], int rows); // 1차원 포인터 배열
```

배열의 크기를 모를 때:

```c
void display2DArray(int *arr, int rows, int cols) {
    *(arr + (i * cols) + j)
    (arr + i * cols)[j]
}

display2DArray(&matrix[0][0], 2, 5);
```

## 2차원 배열 할당

불연속 메모리 할당

```c
int **matrix = (int **)malloc(rows * sizeof(int *));
for (int i = 0; i < rows; i++) {
    matrix[i] = (int*)malloc(columns * sizeof(int));
}
```

연속 메모리 할당 1

```c
int **matrix = (int **)malloc(rows * sizeof(int *));
matrix[0] = (int*)malloc(rows * columns * sizeof(int));
for (int i = 1; i < rows; i++) {
    matrix[i] = matrix[0] + i * columns;
}
```

연속 메모리 할당 2

```c
int *matrix = (int *)malloc(rows * columns * sizeof(int *));
for (int i = 0; i < rows; i++) {
    for (int i = i; i < columns; j++) {
      *(matrix + (i * columns) + j) = i * j;
    }
}
```

## 복합 리터럴

**compound literals**

```c
(const int) { 100 }
(int[3]) { 10, 20, 30 }
```

## 가변 배열

```c
int (*(arr[])) = {
    (int[]) {0, 1, 2, 3},
    (int[]) {4, 5},
    (int[]) {6, 7, 8},
}
```





