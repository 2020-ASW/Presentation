## [01 Matrix](https://leetcode.com/problems/01-matrix/)
![image](https://user-images.githubusercontent.com/46469385/104263054-23771380-54cc-11eb-808f-7bd6d29b4d5d.png)

## IDEA
bfs

## 구현
1. 각 cell에서 1인 부분을 큐에 넣는다.
2. 큐에 있는 위치를 대상으로 bfs를 통해 0까지의 최소 거리를 각각 찾는다

## 코드 => 시간복잡도 O(r^2 * c^2) ??
```java
import java.util.*;
class Solution {
	public static int dr[] = { 0, 0, 1, -1 };
	public static int dc[] = { 1, -1, 0, 0 };

	public int[][] updateMatrix(int[][] matrix) {
		Queue<Position> queue = new LinkedList<Position>();
		for (int i = 0; i < matrix.length; i++) {
			for (int j = 0; j < matrix[i].length; j++) {
				if (matrix[i][j] != 0) {
					queue.add(new Position(i, j, 0));
				}
			}
		}

		while (!queue.isEmpty()) {
			Position now = queue.poll();
			updateCell(now.row, now.col, matrix);
		}
		return matrix;
	}

	public void updateCell(int row, int col, int[][] matrix) {
		Queue<Position> queue = new LinkedList<Position>();
		queue.add(new Position(row, col, 0));

		while (!queue.isEmpty()) {
			Position now = queue.poll();

			if (matrix[now.row][now.col] == 0) {
				matrix[row][col] = now.value;
				return;
			}

			for (int i = 0; i < 4; i++) {
				int newRow = now.row + dr[i];
				int newCol = now.col + dc[i];

				if (isBoundary(newRow, newCol, matrix)) {
					queue.add(new Position(newRow, newCol, now.value + 1));
				}
			}
		}
	}

	public boolean isBoundary(int row, int col, int[][] matrix) {
		if (row < 0 || row >= matrix.length) {
			return false;
		}
		if (col < 0 || col >= matrix[row].length) {
			return false;
		}
		return true;
	}
}

class Position {
	int row, col, value;

	public Position(int row, int col, int value) {
		this.row = row;
		this.col = col;
		this.value = value;
	}
}
```

## IDEA
dp

## 구현
1. 왼쪽 위를 쭉 보면서 최소값으로 업데이트를 해 준다.
2. 오른쪽 아래를 보며 최소값을 업데이트 해 준다.

## 코드 => O(rc)
```java
import java.util.*;

class Solution {
	public static int INF = 10000;

	public int[][] updateMatrix(int[][] matrix) {
		int n = matrix.length;
		int m = matrix[0].length;
		int[][] dp = new int[n][m];

		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				if (matrix[i][j] != 0) {
					dp[i][j] = INF;
				}
			}
		}

		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				if (i > 0) {
					dp[i][j] = Math.min(dp[i][j], dp[i - 1][j] + 1);
				}
				if (j > 0) {
					dp[i][j] = Math.min(dp[i][j], dp[i][j - 1] + 1);
				}
			}
		}

		for (int i = n - 1; i >= 0; i--) {
			for (int j = m - 1; j >= 0; j--) {
				if (i < n - 1) {
					dp[i][j] = Math.min(dp[i][j], dp[i + 1][j] + 1);
				}
				if (j < m - 1) {
					dp[i][j] = Math.min(dp[i][j], dp[i][j + 1] + 1);
				}
			}
		}

		return dp;
	}
}
```
