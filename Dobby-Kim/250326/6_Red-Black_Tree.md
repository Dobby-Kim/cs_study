
## Red-Black Tree란?
- 이진 탐색 트리(Binary Search Tree)의 일종으로
- **자기 균형(Self-Balancing)** 속성을 갖는 자료구조
- 각 노드는 빨간색(Red) 또는 검은색(Black)의 색깔을 가지며, 특정 규칙을 만족해야 한다.
- 이 규칙들 덕분에 트리의 높이가 O(log n)으로 유지되어 삽입, 삭제, 탐색 연산을 효율적으로 수행할 수 있다.

## 주요 키워드와 간단 설명
- **이진 탐색 트리 (Binary Search Tree):** 모든 노드에 대해 왼쪽 서브트리의 값은 부모보다 작고, 오른쪽 서브트리의 값은 부모보다 큰 트리.
- **자기 균형 (Self-Balancing):** 삽입이나 삭제 후에도 트리의 높이가 일정 범위 내에 있도록 자동으로 균형을 맞추는 기능.
- **회전 (Rotation):** 트리의 구조를 변경하여 균형을 맞추는 연산. 좌회전(Left Rotation)과 우회전(Right Rotation)이 있다.
- **색상 속성 (Color Property):** 각 노드는 빨간색 또는 검은색이며, 다음과 같은 규칙을 만족한다:
    - 루트는 항상 검은색이다.
    - 모든 NIL(외부 리프)은 검은색이다.
    - 빨간색 노드의 자식은 반드시 검은색이다.
    - 모든 노드로부터 리프까지의 경로에는 동일한 수의 검은색 노드(black-height)가 존재한다.
- **Black-Height:** 루트에서 임의의 리프까지 경로에 존재하는 검은색 노드의 수.


## Red-Black Tree의 구조와 동작 원리
Red-Black Tree는 위의 색상 규칙들을 이용하여 삽입이나 삭제 후에 트리의 균형을 맞춘다.  
삽입 시에는 일반적인 이진 탐색 트리 삽입을 수행한 후, 색상 변경과 회전 연산을 통해 위배된 규칙을 복구한다.  
삭제 또한 복잡한 과정을 거쳐 삭제 후 발생할 수 있는 불균형을 수정한다.  
업데이트(특정 노드의 값 변경)의 경우, 값이 변경되어 트리의 순서가 어긋난다면 보통 삭제 후 삽입을 진행하여 트리의 성질을 유지한다.  
조회는 이진 탐색 트리의 기본 탐색과 동일하게 O(log n)의 시간 복잡도를 가진다.

```java

public class RedBlackTree {
    private static final boolean RED = true;
    private static final boolean BLACK = false;

    class Node {
        int key;
        boolean color;
        Node left, right, parent;

        Node(int key) {
            this.key = key;
            this.color = RED; // 새로운 노드는 기본적으로 빨간색
            left = right = parent = null;
        }
    }

    private Node root;
    private Node NIL;

    public RedBlackTree() {
        NIL = new Node(0);
        NIL.color = BLACK;
        NIL.left = NIL.right = NIL.parent = null;
        root = NIL;
    }

    // 좌회전 연산
    private void leftRotate(Node x) {
        Node y = x.right;
        x.right = y.left;
        if (y.left != NIL) {
            y.left.parent = x;
        }
        y.parent = x.parent;
        if (x.parent == NIL) {
            root = y;
        } else if (x == x.parent.left) {
            x.parent.left = y;
        } else {
            x.parent.right = y;
        }
        y.left = x;
        x.parent = y;
    }

    // 우회전 연산
    private void rightRotate(Node y) {
        Node x = y.left;
        y.left = x.right;
        if (x.right != NIL) {
            x.right.parent = y;
        }
        x.parent = y.parent;
        if (y.parent == NIL) {
            root = x;
        } else if (y == y.parent.left) {
            y.parent.left = x;
        } else {
            y.parent.right = x;
        }
        x.right = y;
        y.parent = x;
    }

    // 삽입 후 Red-Black 규칙 복구
    private void insertFixup(Node z) {
        while (z.parent.color == RED) {
            if (z.parent == z.parent.parent.left) {
                Node y = z.parent.parent.right;
                if (y.color == RED) {
                    // Case 1: 부모와 삼촌이 빨간색인 경우
                    z.parent.color = BLACK;
                    y.color = BLACK;
                    z.parent.parent.color = RED;
                    z = z.parent.parent;
                } else {
                    if (z == z.parent.right) {
                        // Case 2: z가 오른쪽 자식인 경우
                        z = z.parent;
                        leftRotate(z);
                    }
                    // Case 3: z가 왼쪽 자식인 경우
                    z.parent.color = BLACK;
                    z.parent.parent.color = RED;
                    rightRotate(z.parent.parent);
                }
            } else {
                // 대칭인 경우
                Node y = z.parent.parent.left;
                if (y.color == RED) {
                    z.parent.color = BLACK;
                    y.color = BLACK;
                    z.parent.parent.color = RED;
                    z = z.parent.parent;
                } else {
                    if (z == z.parent.left) {
                        z = z.parent;
                        rightRotate(z);
                    }
                    z.parent.color = BLACK;
                    z.parent.parent.color = RED;
                    leftRotate(z.parent.parent);
                }
            }
        }
        root.color = BLACK;
    }

    // Red-Black Tree에 삽입
    public void insert(int key) {
        Node z = new Node(key);
        Node y = NIL;
        Node x = root;

        while (x != NIL) {
            y = x;
            if (z.key < x.key) {
                x = x.left;
            } else {
                x = x.right;
            }
        }
        z.parent = y;
        if (y == NIL) {
            root = z;
        } else if (z.key < y.key) {
            y.left = z;
        } else {
            y.right = z;
        }

        z.left = NIL;
        z.right = NIL;
        z.color = RED;

        insertFixup(z);
    }

    // 이진 탐색 트리 방식으로 값 검색
    public Node search(Node node, int key) {
        if (node == NIL || key == node.key) {
            return node;
        }
        if (key < node.key) {
            return search(node.left, key);
        } else {
            return search(node.right, key);
        }
    }

    public Node search(int key) {
        return search(root, key);
    }

    // 삭제 연산 (Red-Black Tree 삭제의 복잡한 과정을 단순화하여 구현)
    public void delete(int key) {
        Node z = search(root, key);
        if (z == NIL) {
            System.out.println("Key not found in the tree");
            return;
        }
        Node y = z;
        Node x;
        boolean yOriginalColor = y.color;

        if (z.left == NIL) {
            x = z.right;
            transplant(z, z.right);
        } else if (z.right == NIL) {
            x = z.left;
            transplant(z, z.left);
        } else {
            y = minimum(z.right);
            yOriginalColor = y.color;
            x = y.right;
            if (y.parent == z) {
                x.parent = y;
            } else {
                transplant(y, y.right);
                y.right = z.right;
                y.right.parent = y;
            }
            transplant(z, y);
            y.left = z.left;
            y.left.parent = y;
            y.color = z.color;
        }
        if (yOriginalColor == BLACK) {
            deleteFixup(x);
        }
    }

    // transplant 메서드: 두 서브트리를 교체하는 도우미 메서드
    private void transplant(Node u, Node v) {
        if (u.parent == NIL) {
            root = v;
        } else if (u == u.parent.left) {
            u.parent.left = v;
        } else {
            u.parent.right = v;
        }
        v.parent = u.parent;
    }

    // 최소값 노드 찾기
    private Node minimum(Node node) {
        while (node.left != NIL) {
            node = node.left;
        }
        return node;
    }

    // 삭제 후 Red-Black 규칙 복구
    private void deleteFixup(Node x) {
        while (x != root && x.color == BLACK) {
            if (x == x.parent.left) {
                Node w = x.parent.right;
                if (w.color == RED) {
                    w.color = BLACK;
                    x.parent.color = RED;
                    leftRotate(x.parent);
                    w = x.parent.right;
                }
                if (w.left.color == BLACK && w.right.color == BLACK) {
                    w.color = RED;
                    x = x.parent;
                } else {
                    if (w.right.color == BLACK) {
                        w.left.color = BLACK;
                        w.color = RED;
                        rightRotate(w);
                        w = x.parent.right;
                    }
                    w.color = x.parent.color;
                    x.parent.color = BLACK;
                    w.right.color = BLACK;
                    leftRotate(x.parent);
                    x = root;
                }
            } else {
                Node w = x.parent.left;
                if (w.color == RED) {
                    w.color = BLACK;
                    x.parent.color = RED;
                    rightRotate(x.parent);
                    w = x.parent.left;
                }
                if (w.right.color == BLACK && w.left.color == BLACK) {
                    w.color = RED;
                    x = x.parent;
                } else {
                    if (w.left.color == BLACK) {
                        w.right.color = BLACK;
                        w.color = RED;
                        leftRotate(w);
                        w = x.parent.left;
                    }
                    w.color = x.parent.color;
                    x.parent.color = BLACK;
                    w.left.color = BLACK;
                    rightRotate(x.parent);
                    x = root;
                }
            }
        }
        x.color = BLACK;
    }

    // 중위 순회로 트리의 키 출력 (정렬된 순서로 출력)
    public void inorderPrint(Node node) {
        if (node != NIL) {
            inorderPrint(node.left);
            System.out.print(node.key + " ");
            inorderPrint(node.right);
        }
    }

    public void printTree() {
        inorderPrint(root);
        System.out.println();
    }

    // 간단한 테스트를 위한 메인 메서드
    public static void main(String[] args) {
        RedBlackTree tree = new RedBlackTree();
        tree.insert(10);
        tree.insert(5);
        tree.insert(15);
        tree.insert(1);
        tree.insert(7);
        tree.insert(12);
        tree.insert(18);

        System.out.println("Red-Black Tree in-order traversal:");
        tree.printTree();  // 출력: 1 5 7 10 12 15 18

        System.out.println("Searching for 7:");
        Node result = tree.search(7);
        if (result != tree.NIL) {
            System.out.println("Found: " + result.key);
        } else {
            System.out.println("Not found");
        }

        System.out.println("Deleting 5...");
        tree.delete(5);
        tree.printTree();
    }
}

```
