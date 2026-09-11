# Call-by-Value와 Call-by-reference

C언어는 기본적으로 `Call-by-value`이다. 왜냐하면 매개변수 전달하는 모든 값은 복사한 값이기 때문. 

## Call-by-value

- `Call-by-value`라고 외부 변수에 접근할 수 없는 것이 아니다. 매개변수를 통해 외부의 값을 직접 변경할 수 없다는 뜻.

```c
void change(int x) {
	x = 100;
}

int main() {
	int a = 10;
	
	change(a);
	
	printf("%d", a);
}
```

1. `int a` 의 값인 10을 복사
2. 그럼 매개변수에 int a = 10(a의 복사본) 전달
3. x = 100으로 바뀜
4. 함수가 끝나면 x는 사라지고 a는 여전히 10

## Call-by-reference

```c
void change(int *p) {
	*p = 100;
}

int main() {
	int a = 10;
	
	change(&a);
	
	print("%d", a);
}
```

- `int a` 의 주소값을 포인터 매개변수인 `int *p`에 전달.
- `*p`는 `int a`의 주소값을 가지고 있기 때문에 `a`값을 수정하는 것임.