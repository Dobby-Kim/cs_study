# 2. Stack & Queue


## Stack과 Queue란?

Stack은 Last in first-out(LIFO) 순서로 자료들을 삽입과 제거하는 선형 자료구조이다.

반면 Queue는 First in first-out(FIFO) 순서로 자료의 삽입과 제거가 이루어지는 선형 자료구조이다. 주변에서 흔히 편의점에서 볼 수 있다. 냉장창고에서 음료수 냉장고 뒤에서 음료수를 채워주면 손님은 유리문을 열고 음료수를 가져가는데, 이때 가장 먼저 채워진 음료수를 가져가게 된다. (하지만 가끔 뒤에 있는게 더 시원해보여서 뒤에거 가져가는 행위는 Queue를 규칙을 위배한다. 유념하자.)

## 구현 코드
Stack과 Queue는 기본적으로 ArrayList와 같이 내부에 데이터를 저장하는 Array와 현재 데이터 개수를 뜻하는 size를 가지고 있다.
차이점은 데이터의 삽입과 제거에서 그 순서가 다르다는 것이다.
```java
public class Stack<E> { // Queue도 동일. 다만 Java에서는 구현체가 다르다. Stack은 Vector을 구현하며, Queue는 Collection을 구현한다.
	E[] items;
	int size;
}
```

흔히 알고리즘 풀이에서 자주 사용되는데, Stack은 DFS, Queue는 BFS에서 whlie문과 함께 사용된다.
