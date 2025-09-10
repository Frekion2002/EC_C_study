# 4주차

내용: 문자열, 포인터

# 포인터

---

## 1. 포인터란?

- 정의 : 포인터란 다른 변수의 메모리 주소를 저장하는 변수

```c
int myAge = 43;
int* ptr = &myAge; // 이 부분이 포인터 선언하는 부분. * <- 애스터리스크(asterisk)
```

<img width="511" height="220" alt="image" src="https://github.com/user-attachments/assets/5d88bf2c-6db2-4191-9680-21f2ea0428b6" />

- 왜 배워야 할까?
    1. 효율과 성능의 핵심
        1. **큰 데이터를 복사하지 않고, 주소만 넘겨주면 훨씬 빠르고 효율적임**
        2. 포인터의 크기는  변하지 않는다.(void*, int*, char* 등으로 타입이 다르지만)
            1. 32비트 시스템: 포인터는 4바이트(32비트)
            2. 64비트 시스템: 포인터는 8바이트(64비트)
    2. 진짜로 우리가 필요한 순간
        1. C언어는 함수에 값을 넘기면 복사본만 바뀌지만, 포인터를 사용하면 진짜 값을 바꿀 수 있다. (아래에 추가 설명)
        2. 배열에서도 포인터는 사용되며 추후 자료구조 배울 때 꼭 알고 있어야 하는 기술이다.
    3. 컴퓨터 작동 원리 이해
    4. 프로그래밍 실력 향상
        1. 메모리 관리, 효율적인 코드 작성, 버그 찾기 같은 기술을 익힐 수 있다. 

- 사용하는 이유(함수 part)
    
    아래의 코드는 화씨를 섭씨로 바꾸는 프로그램이다.
    
    ```c
    #include <stdio.h>
    // 화씨 온도를 입력받아 섭씨단위로 바꿔준다.
    void ToCelsius(double F)
    {
    	F = (F - 32)/1.8;
    }
    
    // 물론 이렇게 반환한 값을 호출부에서 활용해도 된다.
    // 하지만 본 글에서는 위의 구현에 대해서 다룬다.
    double ToCelsius2(double F)
    {
        return (F-32)/1.8;
    }
    
    int main()
    {
        double temperature = 32; // 화씨 32도
        ToCelsius(temperature); // 화씨 32도가 섭씨 0도로 바뀌었을 것이다.
        printf("%lf", temperature); // 하지만 그대로 32.000... 가 출력된다!
        return 0;
    }
    
    ```
    
    - Pass by value : C에서는 매개변수를 주소가 아닌 값으로 넘겨주기 때문에 32라는 값이 F에 복사되어 함수로 넘겨진다.  main의 temperature와 ToCelsius의 주소가 서로 다른 것을 알 수 있다.
    
    ```c
    #include <stdio.h>
    
    // 포인터(주소)를 매개변수로 받는다.
    void ToCelsius(double *F)
    {
        // *연산자는 포인터가 가리키는 변수의 값을 나타낸다.
        *F = (*F-32)/1.8;
    }
    
    	int main()
    {
        double temperature = 32; 
        
        // 1번 방법
        // 해당 변수의 주소를 넘겨준다.
        ToCelsius(&temperature); 
        printf("%lf", temperature); // 정상적으로 변경됨!
        
        // 2번 방법
        // 포인터에 변수의 주소를 할당하고
        double *p = &temperature;
        // 포인터를 넘겨준다.
        ToCelsius(p);
        printf("%lf", temperature);  // 정상적으로 변경됨!
        return 0;
    }
    ```
    
    - Pass by reference : 매개변수로 포인터를 넘겨주면 포인터가 원본을 변경할 수 있다.

## 2. 포인터의 선언과 사용법

- 선언 방법

```c
int* p;
int *p;
```

- 주소 저장

```c
int var = 30;
int *p = &var;
```

- 값 접근(역참조): *p를 사용해 해당 주소에 저장된 값을 읽거나 변경

```c
int var = 30;
int *p = &var;
printf("var의 값: %d, 주소: %p, *p의 값: %d\n", var, p, *p);
```

- 다양한 자료형의 포인터 선언

```c
#include <stdio.h>

int main() {
    int num = 10;
    int *p_int = &num;              // int형 포인터

    float f = 3.14f;
    float *p_float = &f;            // float형 포인터

    double d = 2.71828;
    double *p_double = &d;          // double형 포인터

    char c = 'A';
    char *p_char = &c;              // char형 포인터

    long l = 123456789L;
    long *p_long = &l;              // long형 포인터

    long long ll = 9876543210LL;
    long long *p_llong = &ll;       // long long형 포인터

    void *p_void = &num;            // void형 포인터(범용 포인터, 어떤 타입이든 주소 저장 가능)

    // 포인터 값(주소)와 역참조 결과 출력
    printf("p_int: %p, *p_int: %d\n", (void*)p_int, *p_int);
    printf("p_float: %p, *p_float: %f\n", (void*)p_float, *p_float);
    printf("p_double: %p, *p_double: %lf\n", (void*)p_double, *p_double);
    printf("p_char: %p, *p_char: %c\n", (void*)p_char, *p_char);
    printf("p_long: %p, *p_long: %ld\n", (void*)p_long, *p_long);
    printf("p_llong: %p, *p_llong: %lld\n", (void*)p_llong, *p_llong);
    printf("p_void: %p\n", p_void);

    return 0;
}

```

- void* : 타입이 정해지지 않은 포인터로 어떤 자료형의 주소든 저장할 수 있는 범용 포인터다
    - 특징
        - 모든 타입의 주소를 저장할 수 있다.
        - 역참조(값 읽기/쓰기)는 불가능 하다.
            - void*는 ‘타입 정보’가 없으므로, 직접 값을 읽거나 쓸 수 없다.
            - 반드시 원래 타입으로 캐스팅해야 한다.
            
            ```c
            int a = 100;
            void *vp = &a;
            printf("%d\n", *(int*)vp); // (int*)로 캐스팅 후 역참조
            ```
            
- 왜 출력에서 (void*)로 캐스팅 해주나요!?
    - C언어 표준(ISO C99, C11 등)에서, %p는 void* 타입의 포인터만 정확하게 출력하도록 보장되어 있다.
    - 만약 int*, float*, char* 등 다른 타입의 포인터를 %p로 바로 출력하면, 컴파일러에 따라 경고가 나올 수 있고, 일부 시스템에서는 잘못된 값이 출력될 수도 있다.
    
    - 그치만 Visual Studio 2022에서 해보니까 잘 나오던데요?
        - 현대의 컴파일러(특히 x86, x64 환경의 Visual Studio, GCC 등)는 int*, float*, char* 등 어떤 포인터 타입이든 내부적으로 모두 같은 크기(예: 8바이트)로 처리한다.
        - 그래서 %p에 int나 float를 그냥 넣어도, 실제로는 void*와 메모리 구조가 같으므로 주소가 정상적으로 출력된다.
        - 즉, **대부분의 컴파일러에서는 잘 동작하지만, 표준적으로는 안전하지 않다.**
        
        - 잘만 나오는데 왜 표준을 지켜가면서 귀찮게 다 캐스팅 해야해요?
            - **이식성(Portability)**
                - 지금은 Visual Studio에서 잘 되더라도, 다른 컴파일러(예: 임베디드 시스템, 특수한 하드웨어)에서는 잘못된 값이 나올 수 있다.
            - **경고/오류**
                - 일부 컴파일러에서는 경고 메시지를 출력할 수 있다.
            - **미래의 코드 유지보수**
                - 표준을 지키면, 코드가 더 안전하고 유지보수가 쉬워진다.
                

## 3. 포인터 실습 예제

<기본 포인터 선언과 사용>

```c
#include <stdio.h>
int main() {
    int num = 15;
    int *p = &num;

    printf("num의 값: %d\n", num);
    printf("num의 주소: %p\n", &num);
    printf("포인터 p의 값(=num의 주소): %p\n", p);
    printf("포인터 p가 가리키는 값: %d\n", *p);
    return 0;
}
// 출력 어떻게 될까요!? 예상해보고 직접 돌려서 결과를 확인해보세요 ㅎㅎ

```

- p는 num의 주소를 저장하는 포인터
- *p를 사용하면 num의 값을 읽을 수 있다.

<img width="192" height="145" alt="image 1" src="https://github.com/user-attachments/assets/010f7a4a-326d-4fba-a3c2-9ace95746c9c" />

<위와 비슷한 포인터 기본 예제>

```c
#include<stdio.h>

int main(void){
	int i = 1234;
	int *p = &i;     // 포인터에 i의 주소를 저장. 즉, p는 2000이라는 값을 가르킴

	int temp; 
	temp = *p;       // 변수에 p가 가르키는 오브젝트를 저장한다.
									 // p는 i를 가르키고 있고, i의 값은 1234
									 // 따라서 1234가 저장된다.
	
	printf("%d", temp);
	return 0;
}
```

- *p를 이용해서 새로운 변수에 값을 할당할 수 있다.

<함수로 두 변수의 값을 서로 변환하기>

```c
#include <stdio.h>

void swap(int *x, int *y)
{
	int temp;
	temp = *x; // x의 값을 temp에 넣기
	*x = *y; // x가 가리키는 값을 y가 가리키는 값으로 바꿈
	*y = temp; // y가 가리키는 값에 temp를 넣어주기
}

int main(void)
{
	int x = 1;
	int y = 2;
	swap(&x,&y); // 매개변수로 x의 주소와 y의 주소를 넣어주기
	printf("x = %d\ny = %d\n", x, y);

	return 0;
}

/*
x = 2
y = 1
*/
```

- 함수에 변수의 주소를 넘겨, 실제 값을 바꿀 수 있다.

위의 코드를 조금 더 분석해보면,

```c
#include <stdio.h>

void swap(int *x, int *y)
{
	int temp;
	temp = *x; // x의 값을 temp에 넣기
	*x = *y; // x가 가리키는 값을 y가 가리키는 값으로 바꿈
	*y = temp; // y가 가리키는 값에 temp를 넣어주기
	printf("*x : %d\n", *x);
	printf("*y : %d\n", *y);
	printf("temp : %d\n", temp);
	printf("&x : %d\n", &x);
	printf("&y : %d\n", &y);
	printf("&temp : %d\n", &temp);
}

int main(void)
{
	int x = 1;
	int y = 2;
	swap(&x,&y); // 매개변수로 x의 주소와 y의 주소를 넣어주기
	printf("x : %d\ny : %d\n", x, y);

	return 0;
}

/* 결과를 예측해보세요. 이거 잘하셔야해요.
*x : 
*y : 
temp : 
&x :
&y : 
&temp :
x : 
y :  
*/
```

<배열과 포인터>

```c
#include <stdio.h>
int main() {
    int arr[3] = {10, 20, 30};
    int *p = arr;
    printf("첫 번째 요소: %d\n", *p);      // 10
    printf("두 번째 요소: %d\n", *(p+1));  // 20
    printf("세 번째 요소: %d\n", *(p+2));  // 30
    return 0;
}

```

- 배열 이름은 첫 번째 요소의 주소와 같다.
- 포인터 연산으로 각 요소에 접근할 수 있다.

<aside>
💡

!!실습 예제: 주소 출력!!

1. arr 이름의 정수형 배열을 선언하고, 배열에 10, 20, 30, 40, 50의 값을 할당
2. 각 요소의 주소를 출력
</aside>

- 실습 후 질문
    - 아니..int는 4바이트인데, 그럼 arr 배열에서 다음 요소로 갈 때, 4씩 더 해야 하는 거 아닌가요?
        - 포인터 연산은 자동으로 자료형 크기를 고려하므로 p+4가 아니라 p+1을 해주면 된다.
    

## 4. 포인터 사용 시 주의사항

- 포인터는 강력한 기능을 제공하지만, 잘못 사용하면 프로그램 오류, 예기치 않은 동작, 심지어 보안 취약점까지 발생할 수 있다.

### 1. 포인터 초기화

- 문제점: 포인터를 선언만 하고 초기화하지 않으면, 포인터는 임의의(쓰레기) 메모리 주소를 가르킨다.
- 이런 포인터를 역참조하면 프로그램이 예기치 않게 종료되거나 데이터가 손상될 수 있다.

```c
int *p;   // 초기화하지 않음
*p = 10;  // 어디를 가르키는지 모름
```

- 해결책: 포인터를 선언할 때 항상 NULL이나 올바른 주소로 초기화

```c
int val = 30;
int *p1 = NULL;
int *p2 = &val;
```

### 2. NULL 포인터 역참조

- 문제점: NULL 포인터(아무것도 가르키지 않는 포인터)를 역참조하면 세그먼테이션 폴트(segmentation fault)가 발생한다.(a.k.a segfault)
- Segmentation Fault: 허용되지 않는 메모리 영역에 접근할 때 발생하는 런타임 에러

```c
int *p = NULL;
*p = 10; // 위험! NULL 주소에 접근
```

- 해결책: 포인터 사용 전 NULL인지 아닌지 확인하기

```c
if (p != NULL) {
    *p = 10;
}
```

### 3. 포인터 타입 불일치(Type Mismatch)

- 포인터 타입이 변수 타입과 다르면, 잘못된 데이터 해석이나 메모리 손상이 발생할 수 있다.

```c
float f = 3.14f;
int *p = &f;

// 1. 오류가 날 수 있다(컴파일 오류, 런타임 오류 등)
// 2. 쓰레기값이 나온다(정의되지 않은 동작, Undefined Behavior)
```

---

# 2. 문자열

---

## 1. C언어에서 문자열이란?

- 문자열(String)은 메모리에 저장된 연속된 문자(char)들의 집합을 의미한다.
- 큰따옴표(””)를 사용하여 표현되며, 아래의 예시처럼 선언하면 된다.
- 문자열은 반드시 \0 (널 문자)로 끝나야 한다.

```c
char str1[] = "Hello";  // C는 자동으로 마지막에 \0을 추가한다.
char str2[] = {'H', 'e', 'l', 'l', 'o', '\0'}; // 직접 문자 할당할 때는 반드시 \0
```

<자기 이름으로 초기화 해보기>

```c
#include <stdio.h>

int main(void)
{
	char str1[] = "YongJin";
    // []안에는 문자열의 크기가 들어갑니다
    // 문자열의 크기를 자동으로 지정되게 하고싶으면 아래처럼 크기 적는 칸을 비워주면 됩니다.
	char str2[] = "My name is YongJin";

    printf("%s\n",str1);     // YongJin
    printf("%s\n",str2);     // My name is YongJin
    printf("%c\n",str1[0]);  // Y
}
```

<문자열과 포인터>

- 문자열은 첫 번째 문자의 주소(포인터)로도 표현할 수 있다.

```c
char *str = "Hello";
while (*str != '\0'){
	printf("%c ", *str);
	str++;
}
```

- str은 Hello의 첫 글자 H의 주소를 가르키는 포인터이다.
- 포인터 연산을 통해 문자열을 탐색할 수 있다.
    - 수정 하려면 문자열 상수가 아닌 배열로 선언!! 아님 런타임 에러나 이상 동작이 발생.
- 포인터를 증가시키며 각 문자를 출력

## 2. 문자열 입력

<문자열 입력>

<img width="1104" height="606" alt="image 2" src="https://github.com/user-attachments/assets/e2614d67-2b0a-4d2c-a291-bc4d870ddcf5" />

<사용자에게 문자를 입력받아서 그 문자열의 글자수 세기>

```c
#include <stdio.h>

int main(void)
{
    char input[1001];
    scanf_s("%s", input, 1001); // 또는 1001

    int count = 0;
    while (input[count] != '\0')
    {
        count++;
    }

    printf("입력한 문자열의 길이는 %d입니다.\n", count);
    printf("입력한 문자열은 %s입니다.\n", input);
    return 0;
}

// scanf("%1001s", input); // 크기 제한 없음

```

- 문자열 입력 시 널 문자까지 고려해서 크기 설정

```c
char name[11];       // 10글자 + 널 문자('\0')
scanf("%s", name);
```

→ 만약에 10글자보다 더 많이 입력하면 런타임 에러 혹은 이상 동작 발생

## 3. string.h 라이브러리

<string.h 라이브러리>

string 헤더 파일에는 문자열 관련하여 유용한 함수들을 많이 제공한다.

“#include <string.h>”로 선언하고, 관련 함수들을 사용하면 된다.

<img width="1053" height="368" alt="image 3" src="https://github.com/user-attachments/assets/e1f42c2c-ecd1-4fed-97b3-9b5cbc3d28bb" />

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char arr[10] = "hello";

    // 문자열의 길이를 알려주는 strlen() 함수
    printf("문자열의 길이 : %zu\n", strlen(arr));
    // %zu : size_t 타입 출력 (Visual Studio에서 경고 방지)

    // 두 문자열을 비교해주는 strcmp() 함수
    char arr1[10] = "A";
    char arr2[10] = "C";
    printf("문자열 비교 : %d\n", strcmp(arr1, arr2));
    // arr1이 사전순으로 앞에 있으면 음수, 뒤에 있으면 양수, 같으면 0

    // 문자열을 복사해주는 strcpy_s() 함수 (Visual Studio 권장)
    char s1[10] = "hello";
    char s2[10] = "world";
    strcpy_s(s2, sizeof(s2), s1); // s1을 s2에 복사
    printf("문자열 복사 : %s\n", s2);

    return 0;
}

/*
문자열의 길이 : 5
문자열 비교 : -2
문자열 복사 : hello
*/
```
