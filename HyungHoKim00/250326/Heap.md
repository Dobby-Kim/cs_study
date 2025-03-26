## 힙
완전 이진 트리 형식을 하고 있다.   
순서가 있는 값에서 최대/최소 값을 찾고 싶을 때 활용.   
최댓값을 찾기 위해 O(1)의 시간 복잡도를 가지고 있다.   
삽입/삭제는 O(logN)의 시간 복잡도를 가지고 있다.   
자바에는 최소 힙이 PriorityQueue로 구현되어 있다.   

### 최대 힙 구현
```java
public class MaxHeap {
    int[] tree;
    int rootIndex = 1;
    int size = 0;

    int getMax(int value) {
        return tree[rootIndex];
    }

    void put(int value) {
        tree[++size] = value;
        int index = size;
        while(index > 1) {
            int parent = index / 2;
            if (tree[parent] >= tree[index]) {
                break;
            }
            int temp = tree[index];
            tree[index] = tree[parent];
            tree[parent] = temp;
            index = parent;
        }
    }

    void deleteMax() {
        tree[rootIndex] = tree[size];
        int index = rootIndex;
        while(index * 2 + 1 <= size) {
            int lChild = index * 2;
            int rChild = index * 2 + 1;
            if(lChild > rChild) {
                if(lChild <= tree[index]) {
                    break;
                }
                int temp = tree[index];
                tree[index] = tree[lChild];
                tree[lChild] = temp;
                index = lChild;
            } else {
                if(rChild <= tree[index]) {
                    break;
                }
                int temp = tree[index];
                tree[index] = tree[rChild];
                tree[rChild] = temp;
                index = rChild;
            }
        }
        size--;
    }
}
```
