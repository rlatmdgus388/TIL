# malloc과 free에 전달되는 인자 값

## free( )는 포인터 값을 받는다.

예를 들어:

```c
Widget *w = malloc(sizeof(Widget));
```

메모리 상태를 단순화하면:

```c
w
│
│  주소값 0x1000
▼
┌──────────────┐
│ Widget       │
│ ...          │
└──────────────┘
```

여기서:

```c
free(w);
```

라고 하면 `free()` 에게 `w`에 들어 있는 주소값 0x1000을 전달하는 것

즉:

```c
free(w);
     ↑
     포인터 값
```

## 인자가 포인터 배열의 원소일 경우도 똑같다

```c
typedef struct {
    Widget *items[MAX_WIDGETS];
    int count;
} Screen;
```

```c
s.items[i]
```

의 타입은 `Widget *` 

따라서:

```c
free(s.items[2]);
```

는 `s.items[2]`에 들어있는 주소를 `free`에게 전달해서, 그 주소의 힙 메모리를 해제한다는 의미.