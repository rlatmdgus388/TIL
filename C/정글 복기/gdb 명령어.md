# gdb 명령어

| 명령어 | 의미 |
| --- | --- |
| `gdb ./main` | GDB 시작 |
| `break main` | main에 breakpoint |
| `break 10` | 10번 줄에 breakpoint |
| `run` / `r` | 실행 |
| `next` / `n` | 다음 줄 |
| `step` / `s` | 함수 내부로 들어가기 |
| `continue` / `c` | 다음 breakpoint까지 |
| `print x` / `p x` | 변수 x 확인 |
| `info registers` | 레지스터 확인 |
| `p $rax` | RAX 확인 |
| `x/...` | 메모리 확인 |
| `backtrace` / `bt` | 함수 호출 스택 |
| `list` / `l` | 소스 코드 |
| `finish` | 현재 함수 끝까지 |
| `info breakpoints` | breakpoint 확인 |
| `delete` | breakpoint 삭제 |
| `quit` / `q` | 종료 |

## 예시

- n을 눌러서 나오는 코드 줄은 이미 실행한 줄이 아니라 이제 실행할 줄이라는 의미

```c
#include <stdio.h>

void change(int *p)
{
    *p = 20;
}

int main()
{
    int a = 10;
    int *p = &a;

    change(p);

    printf("%d\n", a);

    return 0;
}
```

main에 breakpoint를 만든 후 run 실행

```c
(gdb) break main
(gdb) run
```

그 다음 `next`로 `a = 10` 실행

```c
(gdb) n
```

print a로 변수 a의 값 확인

```c
(gdb) p a
$1 = 10
```

포인터 변수 `p`에 담겨있는 주소 확인

```c
(gdb) n
(gdb) p p
$2 = (int *) 0x7ffffff...
```

포인터 변수 p에 들어있는 주소로 가 그 주소에 담겨있는 값 출력

```c
(gdb) p *p
$3 = 10
```

그리고 change(p)에서 `step` 하면 `change()` 내부로 들어감

```c
(gdb) s
```

이때 `print p`를 하면 똑같은 주소가 나오고

```c
(gdb) p p
```

주소로 들어가 값을 확인하면 똑같이 10이다.

```c
(gdb) p *p
```

이제 다음 줄을 실행하면

```c
(gdb) n
```

값이 20으로 바뀐 것을 확인할 수 있다.

```c
(gdb) p *p
$4 = 20
```