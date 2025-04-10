## 해시 테이블
해시 함수을 통해 생성한 고유한 숫자를 인덱스로 사용하는 배열

### 해시 함수
값들을 특정 로직을 사용하여 작은 범위의 수로 치환한다.   
Collision을 최소화하는 방향으로 생성해야 하나, 무조건 1:1로 만들면 메모리 낭비가 심하다.   
데이터 분산이 균등하게 이루어져야 한다. kmodM 연산에서 M을 소수로 하는 것을 권장.

### 충돌 해결 방식
1. 개방 주소법   
해시 충돌이 발생하면, 다른 해시 버킷에 삽입하는 방식

3. 분리 연결법   
각각의 버킷을 연결 리스트 혹은 트리 구조로 만들어 충돌이 발생하면 해당 버킷에 값을 추가하는 방식

### 해시를 이용한 자료구조
Java에는 HashMap, HashSet이 있다. HashSet은 HashMap의 Key만 사용하는 형태다.

아래는 Linked List를 활용한 분리 연결법으로 구현한 HashMap이다.

```java
public class HashMap<K, V> {
    private LinkedList<Node<K, V>>[] nodes;
    private int M = nodes.length;

    V get(K key) {
        int hash = key.hashCode() % M;
        for(Node<K, V> node : nodes[hash]) {
            if(node.hash == hash && (node.key == key || node.key.equals(key))) {
                return node.value;
            }
        }
        return null;
    }

    void put(K key, V value) {
        int hash = key.hashCode() % M;
        if(nodes[hash] == null) {
            nodes[hash] = new LinkedList<>();
        }
        nodes[hash].add(new Node<>(key, value, hash));
    }

    static class Node<K, V> {
        K key;
        V value;
        int hash;

        public Node(K key, V value, int hash) {
            this.key = key;
            this.value = value;
            this.hash = hash;
        }
    }
}

```
