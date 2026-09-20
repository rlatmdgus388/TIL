# C로 객체지향 표현하기

## 1. 객체 → `struct`

C++에서는

```cpp
class Button {
    int id;
    char label[24];
};
```

C에서는

```c
typedef struct {
    int id;
    char label[24];
} Button;
```

즉, 

```c
Button 객체
   ↓
struct Button 변수
```

## 2. 메서드 → 함수 + 포인터

C++에서는

```cpp
button.render();
button.click();
```

처럼 객체작 자기 함수를 가지고 있는 것처럼 보인다.

C의 `struct`에는 함수를 직접 넣을 수 없으니까 보통 함수 포인터를 넣는다.

```cpp
typedef struct {
    void (*render)(Button *self);
    void (*click)(Button *self);
} Button;
```

그러면 `button.render(&button);` 같은 식으로 사용할 수 있다.

## 3. 상속 → 구조체를 포함시키기

C++에서는

```cpp
class Button : public Widget {
    ...
};
```

라고 할 수 있는데 C에는 `:`를 이용한 상속이 없다.

그래서 구조체를 다른 구조체 안에 넣는 방식으로 비슷하게 표현할 수 있다.

```cpp
typedef struct {
    Widget base;
    int color;
} Button;
```

```cpp
Button
┌──────────────────┐
│ Widget base      │  ← 부모 역할
│ int color        │  ← Button 고유 데이터
└──────────────────┘
```

## 4. 다형성→ 함수 포인터 + VTable

다형성이란 같은 방식으로 요청해도, 실제 객체의 종류에 따라 서로 다른 동작을 수행할 수 있는 성질을 말한다.

#### 예시

`동물`이라는 개념이 있다고 해보자.

```cpp
Animal
 ├── Dog
 ├── Cat
 └── Bird
```

그리고 모등 동물에게 `speak()` 이란 행동이 있다고 해보자.

하지만 실제 동작은 아래처럼 다 다르다.

```cpp
Dog  → 멍멍
Cat  → 야옹
Bird → 짹짹
```

그런데 사용하는 쪽에서는 `animal.speak()` 이라고만 호출한다. 이렇게 Dog인지 Cat인지 일일히 확인하지 않는 것이 다형성의 핵심이다.

#### C코드로 보기

```c
typedef struct {
    void (*render)(Widget *self);
    void (*on_event)(Widget *self, int code);
} VTable;
```

```c
static const VTable BUTTON_VT = {
    button_render,
    widget_noop_event
};

static const VTable LABEL_VT = {
    label_render,
    widget_noop_event
};
```

Widget에는 VTable의 주소를 가지고 있다.

```c
typedef struct Widget Widget;

struct Widget {
    const VTable *vtbl;
    int id;
    int closed;
    char label[24];
};
```

그러면 `w->vtbl->render(w);` 이 한 줄로 Widget의 실제 종류에 따라 다른 함수가 호출된다. 즉, 호출하는 코드는 똑같은데 실제 실행되는 함수가 달라진다.

따라서 이게 Button인지 Label인지 Dialog인지 일일이 검사할 필요가 없다.