# 문자 입출력 함수(stdio.h)

## 문자 입출력 함수

- C언어의 표준 라이브러리에서 제공하는 함수
- `<stdio.h>`에 선언되어 있는 표준 입출력 함수

```c
#include <stdio.h>

int putchar(int c);
int fputc(int c, FILE *stream);

int getchar(void);
int fgetc(FILE *stream);
```

- `putchar(c)` : 문자 하나를 **표준 출력(stdout)** 에 출력
- `fputc(c, stream)`: 문자 하나를 **지정한 스트림**에 출력
- `getchar()` : 표준 입력(stdin)에 입력한 문자 하나 입력
- `fgetc(stream)` : 지정한 스트림에서 문자 하나 입력

```c
putchar('A');  // A
getchar();  // 키보드로부터 문자 하나를 입력받음
fputc('A', stdout);  // stdout은 표준 출력 스트림. putchar('A);와 같은 결과.
fputc('A', 파일);  // 파일에 A를 씀
```

## EOF

- End Of File의 악자로, 파일의 끝을 표현하기 위해서 정의해 놓은 상수이며 값은 `-1`이다.
- 파일을 대상으로 fgetc 함수가 호출됐을 때, 파일에 끝에 도달하면 EOF가 반환된다.
- 콘솔 대상의 fgetc, getchar 함수 호출로 EOF를 반환하는 경우
    - 함수 호출 실패
    - cmd + D 입력되는 경우

## 반환형이 int이고, int형 변수에 문자를 담는 이유

- 반환형이 char형이 아닌 Int형인 이유
    - `EOF`을 나타내는 `-1`을 안전하게 표현하기 위해서이다.
    - `char`타입의 불확실성:
        - C언어 표준에서 `char`는 컴파일러나 운영체제 환경에 따라 `signed char`(-128~127)로 동작할 수도 있고, `unsigned char`(0~255)로 동작할 수도 있다.
        - 만약 환경이 `unsigned char`라면, **음수인 `-1`을 표현할 수 없게 된다.** (`-1`을 저장하려고 하면 `255` 같은 전혀 다른 양수 값으로 변환되어 파일의 끝을 감지하지 못함)
    - `int` 타입의 안전성
        - `int`는 모든 컴파일러에서 음수를 포함하는 `signed int`로 동작
        - 따라서 일반 문자(ASCII 코드 0~127)와 파일의 끝을 알리는 신호인 `EOF`(-1)를 모두 유효하게 표현하고 구분할 수 있다.

```c
int ch; // 반환값을 받을 때도 char가 아닌 int 변수로 받아야 합니다.

while ((ch = getchar()) != EOF) {
    putchar(ch);
}
```

- 만약 여기서 `ch`를 `char`로 선언하면, `unsigned char` 환경에서 `EOF`(-1)를 만나도 `-1`로 비교되지 않아 **무한 루프**에 빠지는 문제가 발생한다.