# 4. Tree

## Tree란?
- 노드로 이루어진 비선형 자료구조
- 계층적 관계(**hierarchical** relationship)를 가진 노드 집합
- 노드간 부모 - 자식 관계를 맺고 있다. 하나의 루트에서 자식 노드로 뻗어나간다.

주변에서 가장 흔하게 볼 수 있는 트리 구조는 컴퓨터에서 사용되는 디렉토리(혹은 폴더) 구조이다. 바탕화면 - 내문서 - ... 이런식

## 주요 키워드
- **노드(Node)**: 트리를 이루는 기본 구성 요소. 데이터를 직접 담고 있거나, 데이터의 주소를 담고 있다.
- **간선(Edge)**: 노드와 노드를 잇는 연결.
위 두 키워드는 트리에서만 쓰이는 것이 아니라 자료구조를 설명할 때 범용적으로 쓰인다.
- **루트(Root)**: 트리에서 가장 최상단 부모 노드를 루트라고 부른다. 나무가 뿌리에서 한 기둥이 시작되어 여러 가지로 뻗어나가는 것을 생각하면 된다.
- **리프(Leaf)**: 자식 노드가 없는 노드. 나무에서 뿌리의 가장 반대에 있는 잎사귀를 뜻한다. 가장 최하단 자식 노드를 리프 노드라고 부른다.
- **높이(Height)**: 루트에서 리프까지의 최대 거리


## 이진 트리(Binary Tree)란?
이진 트리는 각 노드가 최대 두 개의 자식 노드를 가질 수 있는 트리이다. 각각을 왼쪽 자식(Left Child)과 오른쪽 자식(Right Child)으로 구분한다.


## 이진 트리의 종류

### 1. Full Binary Tree (정 이진 트리)
모든 노드가 0개 또는 정확히 2개의 자식 노드를 가진 이진 트리입니다. 즉, 하나의 자식만 가진 노드는 존재하지 않는다.

### 2. Complete Binary Tree (완전 이진 트리)
모든 레벨이 완전히 채워져 있고, 마지막 레벨을 제외하면 왼쪽에서부터 빈 공간 없이 채워진 이진 트리

### 3. Binary Search Tree (이진 탐색 트리, BST)
이진 탐색 트리는 노드가 특정 규칙을 따르는 이진 트리로, 데이터를 빠르게 찾는 데 유리
- 왼쪽 자식 노드는 부모 노드보다 작은 값
- 오른쪽 자식 노드는 부모 노드보다 큰 값


## 이진 트리 구현 Java 예제

```java
class Node {
	int key;
	Node left;
	Node right;

	public Node(int item) {
		key = item;
		left = null;
		right = null;
	}
}

public class BST {
	Node root;

	public BST() {
		root = null;
	}

	public void insert(int key) {
		root = insertRec(root, key);
	}

	private Node insertRec(Node root, int key) {
		if (root == null) {
			root = new Node(key);
			return root;
		}

		if (key < root.key) {
			root.left = insertRec(root.left, key);
		} else if (key < root.key) {
			root.right = insertRec(root.right, key);
		}

		return root;
	}

	public boolean search(int key) {
        return searchRec(root, key);
    }

    private boolean searchRec(Node root, int key) {
        if (root == null) return false;

        if (root.key == key) return true;

        return key < root.key ? searchRec(root.left, key) : searchRec(root.right, key);
    }
}

```
