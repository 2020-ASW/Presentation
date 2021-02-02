![image](https://user-images.githubusercontent.com/46469385/106633474-36a86b00-65c2-11eb-8865-4a5bb386504e.png)

## IDEA

```java
class Solution {
	public int answer = 0;

	public int findTargetSumWays(int[] nums, int S) {
		findAll(nums, 0, 0, S);
		return answer;
	}

	public void findAll(int[] nums, int index, int sum, int target) {
		if (index >= nums.length) {
			if (sum == target) {
				answer++;
			}
			return;
		}

		findAll(nums, index + 1, sum - nums[index], target);
		findAll(nums, index + 1, sum + nums[index], target);
	}
}
```

브루트 포스로 구현할 경우 중복 계산되는 부분이 있다.

문제 조건을 보면 num의 합은 1000을 넘지 않는다고 했다.

합이 있으면 차도 가능하니 num을 적절히 더하고 빼면 그 범위는 -1000 ~ 1000 일 것이다.

## 구현  => 시간복잡도 O(N)

`dp[i][sum+1000]: num 배열의 i번째 요소까지 더하거나 뺐을 때, sum을 만들 수 있는 가짓수로 정의(음수 인덱스는 불가능 하니까 sum+1000으로)`

## 코드

```java
class Solution {
	public int findTargetSumWays(int[] nums, int S) {
		int[][] memo = new int[nums.length][2001];
		for (int[] row : memo) {
			Arrays.fill(row, Integer.MIN_VALUE);
		}
		return calculate(nums, 0, 0, S, memo);
	}

	public int calculate(int[] nums, int i, int sum, int target, int[][] memo) {
		if (i == nums.length) { // nums의 i번째 요소까지 다 돌았을 때
			if (sum == target) { // 가능한 경우 가짓수 + 1
				return 1;
			}
			return 0;
		}

		if (memo[i][sum + 1000] != Integer.MIN_VALUE) { // 이미 dp가 채워졌으면 바로 리턴
			return memo[i][sum + 1000];
		}

		int add = calculate(nums, i + 1, sum + nums[i], target, memo); // 합을 실행했을 경우
		int subtract = calculate(nums, i + 1, sum - nums[i], target, memo); // 차를 실행했을 경우
		memo[i][sum + 1000] = add + subtract;
		return memo[i][sum + 1000];
	}
}
```
