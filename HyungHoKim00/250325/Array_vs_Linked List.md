### 배열
논리적 저장 순서와 물리적 저장 순서가 일치함.   
따라서, 접근 시간 복잡도가 O(1)이다.   
하지만, 삽입/삭제 연산의 경우 다른 원소들의 저장 순서를 변경해야 하기 때문에 O(N)의 시간 복잡도를 가짐.   

```java
public class Array {
    int[] array;
    
    int get(int index) {
        return array[index];
    }

    void insert(int index, int value) {
        for(int i = arrays.length - 1; i > index; i--) {
            array[i] = array[i - 1];
        }
        array[index] = value;
    }
    
    void delete(int index) {
        for(int i = index; i < arrays.length - 1; i++) {
            array[i] = array[i + 1];
        }
    }
}

```

### 연결 리스트
논리적 저장 순서와 물리적 저장 순서가 일치하지 않는다.   
앞에 있는 원소가 다음 원소의 주소값을 가지고 있는 형태.   
따라서, 접근 시간 복잡도는 O(N)이다.   
이 때문에, 삽입, 삭제 연산도 O(N)의 시간 복잡도를 가짐.   

```java
public class LinkedList {
    int[] pointers;
    int[] values;
    int head = 0;
    int size;

    int get(int index) {
        int cur = getPhysicalIndex(index);
        return values[cur];
    }

    void insert(int index, int value) {
        int before = getPhysicalIndex(index - 1);
        int after = pointers[before];
        int cur = size++;
        pointers[cur] = after;
        pointers[before] = cur;
        values[cur] = value;
    }
    
    void delete(int index) {
        int before = getPhysicalIndex(index - 1);
        int cur = pointers[before];
        int after = pointers[cur];
        pointers[before] = after;
    }

    private int getPhysicalIndex(int index) {
        int cur = head;
        for(int i = 0; i < index; i++) {
            cur = pointers[cur];
        }
        return cur;
    }
}
```

### 이중 연결 리스트
연결 리스트와 동일하지만, 앞에 있는 원소가 다음 원소 뿐만 아니라 이전 원소의 주소값도 가진다.   
시간 복잡도는 연결 리스트와 동일하다.   

### 시간 복잡도
| 자료구조 | 접근 | 삽입 | 삭제 |
| --- | --- | --- | --- |
| 배열 | O(1) | O(N) | O(N) |
| 연결 리스트 | O(N) | O(N) | O(N) |
