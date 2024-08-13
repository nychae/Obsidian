# 트리(Tree)

![](https://i.imgur.com/omvR4hf.png)

- **특징**
	\- 계층적인 구조
	\- 루트 노드에서 시작해 자식 노드로 확장

- **접근 시간**
	\- 이진트리 : O(log n) -> 평균적인 경우 `최악의 경우는 O(n)`
	\- 일반트리 : O(n) 

- **장점**
	\- 데이터 검색, 삽입, 삭제가 효율적

- **단점**
	\- 균형이 맞지 않는 트리는 성능이 저하될 수 있음

- 종류
	\- 이진트리
	\- 이진 탐색 트리(BST)
	\- AVL 트리
	\- B-트리
	\- 힙(Heap)

- **사용 예**
	\- 계층적 데이터 저장, 데이터베이스 인덱스, 파일 시스템

``` java
class TreeNode {
	int data;
	TreeNode left, right;

	TreeNode(int data) {
		this.data = data;
		left = right = null;
	}
}

public class TreeExample {
	TreeNode root;

	void inorderTraversal(TressNode node) {
		if (node == null) {
			return;
		}

		inorderTraversal(node.left);
		System.out.print(node.data + " ");
		inorderTraversal(node.right);
	}

	public static void main(String[] args) {
		TreeExample tree = new TreeExample();

		tree.root = new TreeNode(1);
		tree.root.left = new TreeNode(2);
		tree.root.right = new TreeNode(3);
		tree.root.left.left = new TreeNode(4);
		tree.root.left.right = new TreeNode(5);

		tree.inorderTraversal(tree.root);
	}
}
```

##### 🏷️ 이진트리 (Binary Tree)
- 각 노드가 최대 두 개의 자식 노드를 가지는 기본적인 트리 구조
- 자식 노드의 수와 위치에 대한 제약이 없음

- **유형**
![](https://i.imgur.com/dHVW6Ih.png)
	1. 완전 이진트리
		\- 모든 레벨은 완전히 채워져 있으며, 마지막 레벨은 왼쪽부터 오른쪽으로 노드가 채워짐
		\- 노드의 위치를 계산하는데 유용하며, 배열로 구현하기에 적합
		\- 힙(Heap)과 같은 자료구조에서 사용됨	
	2. 포화 이진트리
		\- 모든 노드는 0개 또는 2개의 자식노드를 가지며, 1개만 가지거나 3개 이상을 가지지않음
		\- 노드의 수와 깊이에 따라 특정한 구조를 가짐
	3. 비정렬 이진트리
		\- 정해진 정렬 규칙없이 노드가 배치된 이진트리
		\- 일반적인 이진트리이며, 노드의 배치나 구조에 제약이 없음
		\- 자식 노드가 부모 노드보다 크거나 작을 필요가 없음
		

- **순회(traversal)**
	\- 트리의 모든 노드를 방문하여 순서대로 처리하는 방법

1) 전위 순회 (Pre-order Traversal)
	\- 현재 노드 -> 왼쪽 서브트리 -> 오른쪽 서브트리 순서로 노드 방문
``` java
void preorder() {
	preorderRec(root);
}

private void preorderRec(TreeNode root) {
	if(root != null) {
		System.out.print(root.value + " "); // 현재 노드 방문
		preorderRec(root.left); // 왼쪽 서브트리 전위 순회
		preorderRec(root.right); // 오른쪽 서브트리 전위 순회
	}
}
```

2) 중위 순회 (In-order Traversal)
	\- 왼쪽 서브트리 -> 현재 노드 -> 오른쪽 서브트리 순서로 노드 방문
``` java
void inorder() {
	inorderRec(root);
}

private void inorderRec(TreeNode root) {
	if(root != null) {
		inorderRec(root.left); // 왼쪽 서브트리 중위 순회
		System.out.print(root.value + " "); // 현재 노드 방문
		inorderRec(root.right); // 오른쪽 서브트리 중위 순회
	}
}
```

3) 후위 순회 (Post-order Traversal)
	\- 왼쪽 서브트리 -> 오른쪽 서브트리 -> 현재 노드 순서로 노드 방문
``` java
void postorder() {
	postorderRec(root);
}

private void postorderRec(TreeNode root) {
	if (root != null) {
		postorderRec(root.left); // 왼쪽 서브트리 후위 순회
		postorderRec(root.right); // 오른쪽 서브트리 후위 순회
		System.out.print(root.value + " "); // 현재 노드 방문
	}
}
```


##### 🏷️ 이진 탐색 트리 (BST: Binary Search Tree)
- 정렬된 트리구조로, 각 노드가 특정 규칙에 따라 정렬된 이진트리
- 다양한 데이터 검색, 삽입, 삭제 작업을 효율적으로 수행할 수 있도록 설계됨

- **구조**
	\- 각 노드 : key(값)과 두개의 자식 노드(left, right)를 가짐
	\- 정렬 규칙
		1) 왼쪽 서브트리 : 현재 노드의 왼쪽 자식 및 그 자식들 모두는 현재 노드의 값보다 작은 값을 가짐
		2) 오른쪽 서브트리 : 현재 노드의 오른쪽 자식 및 그 자식들 모두는 현재 노드의 값보다 큰 값을 가짐

- **기본 연산**
	1) 검색(Search)
		\- 검색 연산은 루트 노드에서 시작하여, 검색할 값이 현재 노드의 값보다 작으면 왼쪽 서브트리로, 크면 오른쪽 서브트리로 이동하여 반복
		\- 시간 복잡도: 평균적으로 O(log n), 최악의 경우`(트리가 한쪽으로 치우칠 경우)` O(n)
		
	2) 삽입(Insert)
		\- 새 노드를 삽입할때 검색과 유사하게 올바른 위치를 찾기 위해 노드의 값과 비교하며 트리를 탐색
		\- 시간 복잡도: 평균적으로 O(log n), 최악의 경우 O(n)
		
	3) 삭제(Delete)
		\- 삭제할 노드를 찾은 후, 노드의 자식 수에 따라 다르게 처리
			**자식이 없는 경우**: 노드를 단순히 제거
			**자식이 하나인 경우**: 노드를 제거하고, 자식 노드를 해당 노드의 위치로 이동
			**자식이 둘인 경우**: 일반적으로 노드의 값을 오른쪽 서브트리에서 가장 작은 값(혹은 왼쪽 서브트리에서 가장 큰 값)으로 교체하고, 해당 값을 삭제
		\- 시간 복잡도: 평균적으로 O(log n), 최악의 경우 O(n)

- **장점**
	1) **정렬된 데이터 유지**: BST는 노드의 값이 정렬된 상태로 유지되기 때문에, 이진 탐색 알고리즘 사용 가능
	2) **효율적인 탐색**: 평균적으로 O(log n)의 시간 복잡도로 효율적인 탐색 가능
	3) **동적 데이터**: 데이터가 동적으로 추가되거나 삭제될 수 있는 경우에도 효과적

- **단점**
	\- 불균형: 자가 균형을 유지하지 않기 때문에, 삽입과 삭제 연산이 트리를 비균형하게 만들 수 있음. 이 경우, 검색/삽입/삭제 연산의 성능 저하 가능성 있음
		=> 대안으로 균형 이진 탐색 트리`(ex. AVL 트리, 레드-블랙 트리)`와 같은 자가 균형 이진 탐색 트리가 있음

``` java
class TreeNode {
	int value;
	TreeNode left, right;

	public TreeNode(int item) {
		value = item;
		left = right = null;
	}
}

class BinarySearchTree {
	TreeNode root;

	public BinarySearchTree() {
		root = null;
	}

	void insert(int value) {
		root = insertRec(root, value);
	}

	private TreeNode insertRec(TreeNode root, int value) {
		if (root == null) {
			root = new TreeNode(value);
			return root;
		}

		if (value < root.value) {
			root.left = insertRec(root.left, value);
		} else if (value > root.value) {
			root.right = insertRec(root.right, value);
		}

		return root;
	}

	boolean search(int value) {
		return searchRec(root, value);
	}

	private TreeNode searchRec(TreeNode root, int value) {
		if (root == null) {
			return false;
		}

		if(root.value == value) {
			return true;
		}

		if(value < root.value) {
			return searchRec(root.left, value);
		} else {
			return searchRec(root.right, value);
		}
	}
	
	void inorder() {
		inorderRec(root);
	}

	private void inorderRec(TreeNode root) {
		if(root != null) {
			inorderRec(root.left);
			System.out.print(root.value + " ");
			inorderRec(root.right);
		}
	}
}

public class Main {
	public static void main(String[] args) {
		BinarySearchTree bst = new BinarySearchTree();

		bst.insert(50);
		bst.insert(30):
		bst.insert(20);
		bst.insert(40);
		bst.insert(70);
		bst.insert(60);
		bst.insert(80);

		System.out.println("중위 순회 결과: ");
		bst.inorder(); // 20 30 40 50 60 70 80

		System.out.println("값 40 검색 결과: " + bst.search(40)); // 출력: true 
		System.out.println("값 25 검색 결과: " + bst.search(25)); // 출력: false
	}
}
```