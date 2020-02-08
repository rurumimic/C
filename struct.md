# 구조체

구조체 크기 = 변수 크기 합 + 패딩

## 선언

```c
struct _person {
    char* firstName;
    char* lastName;
    char* title;
    unsigned int age;
};
```

```c
typedef struct _person {
    char* firstName;
    char* lastName;
    char* title;
    unsigned int age;
} Person;
```

### 사용

```c
Person person;

// 1. dot
person.firstName = (char*)malloc(strlen("Emily") + 1);
strcpy(person.firstName, "Emily");
person.age = 23;
```

```c
Person *ptrPerson;
ptrPerson = (Person*)malloc(sizeof(Person));

// 2. points-to
ptrPerson->firstName = (char*)malloc(strlen("Emily") + 1);
strcpy(person->firstName, "Emily");
person->age = 23;

// 3. 역참조 dereference 후 dot
(*ptrPerson).firstName = (char*)malloc(strlen("Emily") + 1);
strcpy((*ptrPerson).firstName, "Emily");
(*ptrPerson).age = 23;
```

## 할당과 해제

- 구조체 내부 포인터: 직접 할당/해제 필요
- 전역/정적 구조체 멤버: 모두 0/NULL로 초기화

## 구조체 목록

구조체 개체 풀을 만들어 할당/해제 오버헤드를 줄인다.

동적으로 리스트 크기를 조절하면 더 좋다.

```c
#define LIST_SIZE 10
Person *list[LIST_SIZE];

// 리스트 사용 전 초기화
void initList() {
    for(int i = 0; i < LIST_SIZE; i++) {
        list[i] = NULL;
    }
}

Person *getPerson() {
    for(int i = 0; i < LIST_SIZE; i++) {
        if(list[i] != NULL) { // 구조체를 사용할 수 있으면 == 풀에 사용가능한 구조체가 있으면
            Person *ptr = list[i]; // 하나를 꺼낸다.
            list[i] = NULL;
            return ptr;
        }
    }
    // 사용가능한 구조체가 없으면
    Person *person = (Person*)malloc(sizeof(Person));
    return person; // 하나를 생성한다.
}

Person *returnPerson(Person *person) {
    for(int i = 0; i < LIST_SIZE; i++) {
        if(list[i] == NULL) { // 구조체를 다시 채울 수 있으면 == 풀에 사용가능한 구조체가 없으면
            list[i] = person; // 하나를 채운다.
            return person; // 포인터를 반환한다.
        }
    }
    deallocatePerson(person); // 풀이 이미 사용할 수 있는 구조체들이 꽉차 있으면, 메모리를 해제한다.
    free(person);
    return NULL;
}
```

```c
initList();
Person *ptrPerson;

ptrPerson = getPerson();
initPerson(ptrPerson, "Ralph", "Fitsgerald", "Mr.", 35);
displayPerson(*ptrPerson);
returnPerson(ptrPerson);
```

## 자료구조

생략
