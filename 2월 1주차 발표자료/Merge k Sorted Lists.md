![image](https://user-images.githubusercontent.com/46469385/107243743-ab6f1f80-6a70-11eb-99cb-ecd91b77d8d1.png)

## IDEA - 1 O(NlogK)
일단 모든 노드 값을 priority queue에 넣고 하나씩 빼면서 리스트를 머지하는 방식

## 코드
```java
class Solution {
	public ListNode mergeKLists(ListNode[] lists) {
		if (lists == null || lists.length == 0) {
			return null;
		}
		PriorityQueue<Integer> queue = new PriorityQueue<>();
		for (ListNode node : lists) {
			while (node != null) {
				queue.add(node.val);
				node = node.next;
			}
		}
		return getNode(queue);
	}

	private ListNode getNode(PriorityQueue<Integer> queue) {
		if (!queue.isEmpty()) {
			ListNode node = new ListNode(queue.poll(), null);
			node.next = getNode(queue);
			return node;
		}
		return null;
	}
}

```

## IDEA - 2 
