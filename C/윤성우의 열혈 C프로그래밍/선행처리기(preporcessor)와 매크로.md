# 선행처리기(preporcessor)와 매크로

## 선행처리기의 역할

- 선행처리기는 컴파일을 하기 전 `#include`, `#define`, 조건부 컴파일 지시를 처리해서 컴파일러가 받을 소스 코드를 만들어준다.
- 선행처리기에게 무엇인가를 명령하는 문장은 `#`으로 시작한다.
- 매크로
    - 이 이름이 나오면 이것으로 바꿔줘라고 미리 정해놓은 치환 규칙

## #define: Object-like macro

- `# define PI 3.1415`
    - `#define` : 지시자
    - `PI` : 매크로(object-like macro)
    - `3.1415` : 매크로 몸체

```c
#define NAME "홍길동"
#deinfe AGE 20
#define PI 3.14

printf("%s\n", NAME);
printf("%s\n", AGE);
```

코드가 위와 같이 있을 때 선행처리기가 아래와 같이 치환함

```c
printf("%s\n", "홍길동");
printf("%s\n", 20);
```

이때 선행처리기는 NAME이라는 변수를 만드는 것이 아니라, `“NAME”` 이 나오면 `“홍길동”` 이란 리터럴 값으로 치환해서 컴파일 전에 단순히 코드가 바뀌는 것이다.

그 후 그 치환된 것을 컴파일러가 컴파일함.

## #define: Funtion-like macro

- `# define SQUARE(X) X*X`
    - SQUARE(123); → 123*123
    - SQUARE(NUM); → NUM * NUM
        - 이러한 변환 과정을 `매크로 확장`(macro expansion)이라 한다.

```c
// 매크로 확장의 예
#define SQUARE(X) X*X
int main(void)
{
	int num=20;
	
	/* 정상적 결과 출력 */
	printf("Square of num: %d \n", SQUARE(num));
	printf("Square of num: %d \n", SQUARE(-5));
	printf("Square of num: %d \n", SQUARE(2.5));
  /* 비정상적 결과 출력 */
	printf("Square of num: %d \n", SQUARE(3+2));
	return 0;
}
	
```

```c
square of num: 400
square of num: 25
square of num: 6.25
square of num: 11
```

#### 잘못된 매크로의 정의와 소괄호 해결책

```c
#define SQUARE(X) X*X
SQUARE(3+2) -> 3+2*3+2 -> 11
SQUARE((3+2)) -> (3+2)*(3+2) -> 25

#define SQUARE(X) (X)*(X)
SQUARE(3+2) -> (3+2)*(3+2) -> 25
num=120/SQUARE(2) -> 120/(2)*(2)

#define SQUARE(X) ((X)*(X))
num=120/SQUARE(2) -> 120/((2)*(2))
```

## 매크로를 두 줄에 걸쳐서 정의하는 방법

```c
// 에러 발생
#define SQUARE(X)
((X)*(X))

// 첫 번쨰 줄의 끝에 \를 삽입
#define SQUARE(X) \
((X)*(X))
```

## 먼저 정의된 매크로의 사용

- 아래 예제와 같이, 앞 줄에서 먼저 정의된 매크로는 새로운 매크로를 정의하는데 있어서 사용할 수 있다.

```c
#define PI 3.14
#define PRODUCT(X, Y) ((X)*(Y))
#define CIRCLE_AREA(R) (PRODUCT((R), (R))*PI)

int main(void)
{
	double rad=2.1;
	printf("반지름 %g인 원의 넓이: %g \n", rad, CIRCLE_AREA(rad));
	return 0;
}		
```