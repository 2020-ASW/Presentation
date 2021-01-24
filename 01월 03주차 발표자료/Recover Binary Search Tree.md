# Recover Binary Search Tree



### 문제 

- Binary Search Tree가 주어졌을 때, 2개의 노드가 swap이 되는 실수가 생겼다.
- BST를 복구하라!

[문제 보기](https://leetcode.com/problems/recover-binary-search-tree/)





### 풀이

---

#### BST(Binary Search Tree)?

![이진탐색트리](img/RecoverBST1.png)



- Root를 기준으로 좌측엔 작은 값들로 구성된 서브 트리, 우측엔 큰 값들로 구성되어 있는 서브트리

  (각 서브트리에도 동일하게 적용)

- 자식 노드를 최대 2개 까지 가진다

- 중복된 노드가 없다

- 중위 순회 방식(in-order traversal)으로 탐색 시, 노드 값들의 오름차순
- 후위 순회 방식(post-order traversal)으로 탐색 시, 노드 값들의 내림차순



#### 접근 방식

정확히 <u>2개의 노드가 순서가 바뀌어있는 상태</u>이므로 

<u>재귀를 통한 중위 순회 방식으로 이상하게 배치되어있는 2개의 노드</u>를 찾은 뒤 바꿔 준다





### 소스 코드

```java
public class RecoverBinarySearchTree {
    private TreeNode prev;
    private TreeNode node1;
    private TreeNode node2;

    public void recoverTree(TreeNode root) {
        prev = null;		// 이전 값을 저장해 둘 노드
        node1 = null;		// 이상한 노드 1
        node2 = null;		// 이상한 노드 2

        discover(root);

        int tmp = node1.val;
        node1.val = node2.val;
        node2.val = tmp;
    }

    // 중위 순회 방식으로 노드 탐색
    private void discover(TreeNode node) {
        if (node == null) return;

        discover(node.left);

        // 이전 노드가 null이 아니고
        // 이전 값이 현재값보다 크다면
        if (prev != null && prev.val > node.val) {
            if(node1 == null) node1 = prev;
            node2 = node;
        }
        prev = node;

        discover(node.right);
    }
}
```

