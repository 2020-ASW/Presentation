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
3. 트리를 다시 순회하면서 두 값을 바꾼다. -> O(N)

## 코드

```java

```
