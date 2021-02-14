![image](https://user-images.githubusercontent.com/46469385/107881646-9db21200-6f28-11eb-9331-46ab21c9b4cc.png)

## IDEA

괄호 중 `]` 을 만나는 순간, 뒤에서부터 가장 먼저 만나게 되는 `[` 의 위치를 찾아 그 부분을 반복하는 문제이다.

=> 뒤에서 부터 꺼내는 것이니까 스택을 사용하자

## 풀이 `3[a2[c]]`

1. 일단 닫는 괄호를 만나기 전까지는 스택에 push
![image](https://user-images.githubusercontent.com/46469385/107881709-ecf84280-6f28-11eb-9843-6b75f2233261.png)

2. 여는 괄호를 만나기 전까지 pop을 하며 stringbuilder에 더해주고, 여는 괄호까지 pop한 뒤에 top이 숫자가 될 수 있는지 확인한다. 숫자이면 pop해주고, stringbuilder를 그 숫자만큼 반복해준다.
![image](https://user-images.githubusercontent.com/46469385/107881769-11ecb580-6f29-11eb-9781-e9106aaeefe6.png)

3. 2의 결과를 스택에 다시 넣어주고
![image](https://user-images.githubusercontent.com/46469385/107881790-2af56680-6f29-11eb-843b-7e95d6894121.png)

4. 위 규칙을 반복한다
![image](https://user-images.githubusercontent.com/46469385/107881792-2e88ed80-6f29-11eb-842f-452c7421f07d.png)

## 코드

```java
import java.util.*;
import java.util.regex.*;

class Solution {
	public String decodeString(String s) {
		Stack<String> stack = new Stack<String>();
		Queue<String> queue = new LinkedList<String>();

		Pattern p = Pattern.compile("(\\D)+|(\\d)+");
		Matcher m = p.matcher(s);

		while (m.find()) {
			StringTokenizer st = new StringTokenizer(m.group(), "[]", true);
			while (st.hasMoreElements()) {
				queue.add(st.nextToken());
			}
		}
		
		StringBuilder sb = new StringBuilder("");
		while (!queue.isEmpty()) {
			String str = queue.poll();

			if (str.equals("]")) {
				while (true) {
					String buffer = stack.pop();

					if (buffer.equals("[")) {
						break;
					}
					sb.insert(0, buffer);
				}

				if (!stack.isEmpty()) {
					int k = isNumber(stack.peek());
					if (k != -1) {
						stack.pop();
						for (int i = 0; i < k; i++) {
							stack.add(sb.toString());
						}
					}
				}
				
				sb.setLength(0);
			} else {
				stack.add(str);
			}
		}
		
		while(!stack.isEmpty()) {
			sb.insert(0, stack.pop());
		}
		return sb.toString();
	}

	private int isNumber(String str) {
		try {
			int n = Integer.parseInt(str);
			return n;
		} catch (Exception e) {
			return -1;
		}
	}
}
```
