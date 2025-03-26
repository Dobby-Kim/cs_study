## 트리
비선형 자료구조, 계층적 자료구조.

### 구성 요소
- 노드(Node): 트리 각각의 요소
- 간선(Edge): 노드 사이를 잇는 선
- 루트 노드(Root Node): 최상단 노드
- 리프 노드(Leaf Node): 하위 노드가 없는 노드
- 부모 노드(Parent Node): 해당 노드의 상위 노드
- 자식 노드(Child Node): 해당 노드의 하위 노드
- 형제 노드(Sibling Node): 해당 노드와 높이가 같은 노드

### 트리의 높이?
이거 ㄹㅇ 어떻게 계산함ㅁ;;; 말마다 다 다름

## 이진 트리
자식 노드의 갯수가 2가 넘지 않는 트리.   
이진 트리의 서브 트리도 이진 트리이다.

### 이진 트리의 종류
- 포화 이진 트리: 모든 레벨이 꽉 찬 이진 트리. 노드 갯수는 무조건 2^n-1개
- 완전 이진 트리: 트리의 노드가 왼쪽부터 꽉 찬 트리.
- 정 이진 트리: 자식 노드가 0개 혹은 2개인 트리

## 이진 탐색 트리(BST)
순서가 있는 값을 효율적으로 탐색하기 위한 트리.
탐색에 O(logN)의 시간 복잡도를 가진다.

### 이진 탐색 트리의 특징
- 이진 탐색 트리의 노드에 저장된 키는 유일하다.
- 부모의 키가 왼쪽 자식 노드의 키보다 크다.
- 부모의 키가 오른쪽 자식 노드의 키보다 작다.
- 왼쪽과 오른쪽 서브트리도 이진 탐색 트리이다.

```java
public class BinarySearchTree {
    Node[] tree;
    int rootIndex = 1;
    int size = 0;

    Node get(int value) {
        Node node = tree[rootIndex];
        while(node != null && node.value != value) {
            if(value < node.value) {
                node = tree[node.left];
            } else {
                node = tree[node.right];
            }
        }
        return node;
    }

    void put(int value) {
        Node parent = null;
        Node node = tree[rootIndex];
        boolean left = true;
        while(node != null) {
            if(value < node.value) {
                parent = node;
                node = tree[node.left];
                left = true;
            } else if(value > node.value) {
                parent = node;
                node = tree[node.right];
                left = false;
            } else {
                return;
            }
        }
        tree[++size] = new Node(value);
        if(parent != null) {
            if(left) {
                parent.left = size;
            } else {
                parent.right = size;
            }
        }
    }

    static class Node {
        int value;
        int left;
        int right;

        public Node(int value) {
            this.value = value;
            left = 0;
            right = 0;
        }
    }
}
```
