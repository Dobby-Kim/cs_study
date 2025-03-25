### 스택
후입선출(LIFO) 구조를 가진 자료구조.   
DFS를 구현할 때 쓰인다.   

```java
public class Stack {
    int[] stack;
    int top;
    
    void push(int value) {
        stack[++top] = value;
    }
    
    int pop() {
        return stack[top--];
    }
}
```

### 큐
선입선출(FIFO) 구조를 가진 자료구조.   
BFS를 구현할 때 쓰인다.

```java
public class Queue {
    LinkedList<Integer> queue = new LinkedList<>();
    
    void offer(int value) {
        queue.addLast(value);
    }
    
    int poll() {
        return queue.removeFirst();
    }
}
```
