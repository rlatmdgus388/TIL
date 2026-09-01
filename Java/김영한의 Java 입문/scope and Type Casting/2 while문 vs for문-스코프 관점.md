# 2. while문 vs for문-스코프 관점

날짜: 01. 스코프 형변환

## While

```java
package loop;

public class While2_3 {
	public static void main(String[] args) {
		int sum = 0;
		int i = 1;
		int endNum = 3;
		
		while (i <= endNum) {
			sum = sum + i;
			System.out.println("i=" + i + " sum=" + sum);
			i++;
		}
		//... 아래에 더 많은 코드들이 있다고 가정
	}
}
```

## For

```java
package loop;

public class For2 {
	public static void main(String[] args) {
		int sum = 0;
		int endNum = 3;
		
		for (int i = 1; i <= endNum; i++) {
			sum = sum + i;
			System.out.println("i=" + i + " sum=" + sum);
		}
		//... 아래에 더 많은 코드들이 있다고 가정
	}
}
```

변수의 스코프 관점에서 카운터 변수 `i` 를 비교

- `while`문 특성상 카운터 변수를 안에서 선언하기 힘들기 때문에 `i` 의 스코프가 `main()` 메서드 전체가 된다.
- `for` 문의 경우 변수 `i`의 스코프가 `for`문 안으로 한정.
- 따라서 `for` 문 안에서만 사용되는 카운터 변수가 있다면 `while`문 보다 `for`문을 사용해서 스코프의 범위를 제한하는 것이 메모리 사용과 유지보수 관점에서 좋음.