# Palindrome Partitioning

![image](https://user-images.githubusercontent.com/46469385/103183882-837c9000-48f8-11eb-9c3e-b4e9dbaea049.png)

* * *

### IDEA

모든 substring 경우의 수를 백트래킹을 사용하여 구하면서 팰린드롬이면 리스트에 담는다.

이 때, 매번 팰린드롬인지 확인하는 방법도 있으나, DP를 사용해서 팰린드롬임을 확인해보도록 한다.

팰린드롬 s의 서브스트링 `s.substring(1, n-1)` 또한 팰린드롬임을 이용하자.

<img src="https://user-images.githubusercontent.com/46469385/103184045-5da3bb00-48f9-11eb-9a00-a792d80f839d.png" width="100">

* * *

### DP의 정의

DP[indexStart][indexEnd]: s.substring(indexStart, indexEnd)의 팰린드롬 여부로 정의한다.

* * *

### 맨 첫 글자와 끝 글자가 같은 경우 중에서 팰린드롬이 될 수 있는 경우

1. 맨첫글자 위치 == 끝글자 위치 ex) "a"
2. 두 글자가 연속인 경우 ex) "aa"
3. 중간에 한 글자가 껴있는 경우 ex) "aba"
4. s.substring(첫 글자의 다음 글자, 끝 글자의 이전 글자)가 팰린드롬인 경우

* * *

### 코드

```java
import java.util.*;
class Solution {
	public List<List<String>> partition(String s) {
		List<List<String>> answer = new ArrayList<List<String>>();

		int length = s.length();
		boolean[][] dp = new boolean[length][length];

		palindrome(s, 0, answer, new Stack<String>(), dp);

		return answer;
	}

	public void palindrome(String s, int indexStart, List<List<String>> answer, Stack<String> currentList, boolean[][] dp) {
		if (indexStart >= s.length()) {
			answer.add(new ArrayList<>(currentList));
			return;
		}

		for (int end = indexStart; end < s.length(); end++) {
			if (s.charAt(indexStart) == s.charAt(end)) {
				if (end - 1 <= indexStart + 1 || dp[indexStart + 1][end - 1]) {
					dp[indexStart][end] = true;
					currentList.add(s.substring(indexStart, end + 1));
					palindrome(s, end + 1, answer, currentList, dp);
					currentList.pop();
				}
			}
		}
	}
}
```
