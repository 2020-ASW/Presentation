## [Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/)

![image](https://user-images.githubusercontent.com/46469385/103511857-1b571c80-4eab-11eb-8d79-c79b43e961d4.png)

## IDEA

이진탐색 트리란?

<img src="https://user-images.githubusercontent.com/46469385/103512418-fdd68280-4eab-11eb-9427-8234c8cea57c.png" width="300">

* 각 노드의 왼쪽 서브트리에는 해당 노드의 값보다 작은 값을 지닌 노드들로 이루어져 있다.
* 각 노드의 오른쪽 서브트리에는 해당 노드의 값보다 큰 값을 지닌 노드들로 이루어져 있다.
* 중복된 노드가 없어야 한다.
* 왼쪽 서브트리, 오른쪽 서브트리 또한 이진탐색트리이다.

-> in-order traversal을 하게 되면 노드가 오름차순으로 진행된다.

## 구현

1. in-order traversal을 진행하며 배열에 담는다. -> O(N)
2. 배열에서 오름차순이 아닌 두 값을 찾는다. -> O(N)
    * 예시
      * [1, 3, 2, 4] : 연속된 값이 swap된 경우
      * [1, 2, 6, 4, 5, 3, 7] : 연속되지 않은 값이 swap된 경우
3. 트리를 다시 순회하면서 두 값을 바꾼다. -> O(N)

## 코드

```java
class Solution {
	public void recoverTree(TreeNode root) {
		LinkedList<Integer> inOrderList = new LinkedList<Integer>();
		inOrderTraversal(root, inOrderList);
		System.out.println(inOrderList);
		int swap[] = findTwoElements(inOrderList);
		swapTwoElements(swap[0], swap[1], root);
	}

	public void swapTwoElements(int value1, int value2, TreeNode root) {
		if (root.val == value1) {
			root.val = value2;
		} else if (root.val == value2) {
			root.val = value1;
		}

		if (root.right != null) {
			swapTwoElements(value1, value2, root.right);
		}
		if (root.left != null) {
			swapTwoElements(value1, value2, root.left);
		}
	}

	public int[] findTwoElements(LinkedList<Integer> list) {
		int length = list.size();
		int value1 = 0;
		int value2 = 0;

		for (int i = 0; i < length - 1; i++) {
			if (list.get(i) > list.get(i + 1)) {
				if (value1 == 0) {
					value1 = list.get(i);
				} else {
					value2 = list.get(i + 1);
					break;
				}
			}
		}
		return new int[] { value1, value2 };
	}

	public void inOrderTraversal(TreeNode root, LinkedList<Integer> list) {
		if (root == null) {
			return;
		}
		inOrderTraversal(root.left, list);
		list.add(root.val);
		inOrderTraversal(root.right, list);
	}
}
```

## 질문
inOrderTraversal의 list을 지역변수로 선언해야 하는 이유가 뭘까요?


