![image](https://user-images.githubusercontent.com/46469385/105170547-6ede9100-5b60-11eb-99d7-3ef3a63048e2.png)

## IDEA

0에 도달 할 때까지 위쪽으로 반복하여 직사각형의 최대 높이 찾기

직사각형의 최대 높이를 수용하지 않는 높이까지 왼쪽과 오른쪽으로 바깥쪽으로 반복하여 직사각형의 최대 너비 찾기

예를 들어 노란색 점으로 정의 된 직사각형 찾기 :

<img src="https://user-images.githubusercontent.com/46469385/105203168-28039200-5b86-11eb-9838-c2ed739796ea.png" width="300">

최대 직사각형은 이러한 방식으로 구성된 직사각형 중 하나이다.

* 높이 h, 왼쪽 index = l 및 오른쪽 index = r을 갖는 최대 직사각형이 주어지면, [l, r]에 `연속적인 점(즉 높이)의 수가 <= h` 인 직사각형의 밑면의 점이 있어야한다. 
    * 이 점이 존재하는 경우, 위의 방식으로 점에 의해 정의 된 직사각형은 최대 직사각형이 될 것이다. 이는 높이 h에 도달 한 다음 해당 경계 [l, r]의 경계까지 확장된다.
    * 이 점이 없으면 [l, r] 간격의 모든 높이가 h보다 크므로 원래 사각형의 높이를 높이면 더 큰 사각형을 만들 수 있다. 따라서 사각형은 최대 값이 될 수 없다.

결과적으로 각 점에 대해 정의하는 사각형의 높이, 왼쪽 경계 및 오른쪽 경계인 h, l 및 r 만 계산하면 된다.

동적 계획법을 사용하면 이전 행의 각 점의 h, l 및 r을 사용하여 선형 시간에서 다음 행의 모든 점에 대한 h, l 및 r을 계산할 수 있다.

## 구현 => O(r*c)

* height[j]: matrix [i][j]의 연속된 1의 수, 즉 높이

* 직사각형의 왼쪽 경계 l이 변경되는 원인 - 현재 행에서 0이 발생하는 경우 
    * 왜냐하면 현재 행 위의 행에서 발생하는 모든 0 인스턴스가 이미 현재 버전의 왼쪽에 포함되었으므로 왼쪽에 영향을 미치는 유일한 것은 현재 행에서 0이 발생하는 경우이다.
    * newLeft[j] = max(oldLeft[j], currentLeft)
    * 직사각형을 왼쪽으로 확장하면 currentLeft을 지나서 확장 할 수 없다는 것을 알 수 있다. 그렇지 않으면 0이 된다.
    
* 마찬가지로 오른쪽은 newRight[j] = min(oldRight[j], currentRight)로 정의할 수 있다.

## 풀이

```java
class Solution {

	public int maximalRectangle(char[][] matrix) {
		if (matrix.length == 0) {
			return 0;
		}

		int row = matrix.length; // 행 길이
		int col = matrix[0].length; // 열 길이

		int[] left = new int[col]; // initialize left as the leftmost boundary possible
		int[] right = new int[col];
		int[] height = new int[col];

		Arrays.fill(right, col); // initialize right as the rightmost boundary possible

		int maxArea = 0;

		for (int i = 0; i < row; i++) {
			int currentLeft = 0;
			int currentRight = col;

			// update height
			for (int j = 0; j < col; j++) {
				height[j] = matrix[i][j] == '1' ? height[j] + 1 : 0;
			}

			// update left
			for (int j = 0; j < col; j++) {
				if (matrix[i][j] == '1') {
					left[j] = Math.max(left[j], currentLeft);
				} else {
					left[j] = 0;
					currentLeft = j + 1;
				}
			}

			// update right
			for (int j = col - 1; j >= 0; j--) {
				if (matrix[i][j] == '1') {
					right[j] = Math.min(right[j], currentRight);
				} else {
					right[j] = col;
					currentRight = j;
				}
			}

			// update area
			for (int j = 0; j < col; j++) {
				maxArea = Math.max(maxArea, (right[j] - left[j]) * height[j]);
			}
		}
		return maxArea;
	}
}
```
