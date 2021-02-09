![image](https://user-images.githubusercontent.com/46469385/107243743-ab6f1f80-6a70-11eb-99cb-ecd91b77d8d1.png)

## IDEA
merge 2 ArrayList라면?
![image](https://user-images.githubusercontent.com/46469385/107302712-2102dc00-6ac1-11eb-84cb-a4e97c041d91.png)

## 풀이 O(NlogK)
분할 정복 => NlogK만에 문제를 풀 수 있다.
![image](https://user-images.githubusercontent.com/46469385/107302533-de410400-6ac0-11eb-8fa2-0193c939bba0.png)

## 코드
```java
class Solution {
	public ListNode mergeKLists(ListNode[] lists) {
		return merge(lists, 0, lists.length - 1);
	}

	private ListNode merge(ListNode[] lists, int s, int e) {
		if (e < s) {
			return null;
		}
		if (e == s) {
			return lists[s];
		}

		int m = (s + e) / 2;
		
		ListNode l = merge(lists, s, m);
		ListNode r = merge(lists, m + 1, e);
		ListNode dummy = new ListNode();
		ListNode pre = dummy;
		
		while (l != null && r != null) {
			if (l.val <= r.val) {
				pre.next = l;
				l = l.next;
			} else {
				pre.next = r;
				r = r.next;
			}
			pre = pre.next;
		}

		while (l != null) {
			pre.next = l;
			pre = l;
			l = l.next;
		}

		while (r != null) {
			pre.next = r;
			pre = r;
			r = r.next;
		}
		return dummy.next;
	}

}
```
