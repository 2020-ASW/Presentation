# X와 K



### 문제

![image-20210204035214688](D:\2000_study\Presentation\1월 5주차 발표자료\img\image-20210204035214688.png)



### 접근 방식

주어진 식 `X + Y = X | Y` 을 만족하려면,

<u>X를 2진수로 바꿨을 때 0인 부분에 0 또는 1의 값을 넣어 Y값을 만들어야 한다</u>

예시)

X가 5일 때, 2진수로 변환해보면 00101(2)

![image-20210204042653889](D:\2000_study\Presentation\1월 5주차 발표자료\img\image-20210204042653889.png)



2진수 X의 0인 부분에 작은 수부터 차례로 0 또는 1을 배치를 하게되면

문제에서 요구하는 K번째 작은 Y를 구할 수가 있는데

<u>여기서 차례로 배치해본 Y 값이 K 값과 같다는 규칙을 찾을 수 있다</u>

<br>

<br>

#### 정리

1. X를 2진수로 변환한다
2. 2진수 X의 비트를 보며 0인 부분에서 K를 뒷자리부터 배치한다( K/2 )
3. K가 1이 배치된다면 Y값에 해당 자리수의 2 지수승을 더해 준다
4. 2~3번 과정을 K가 끝날때 까지 반복한다

※ X와 K는 20억 이하의 자연수이지만 Y의 경우 int의 범위를 초과할 수있기 때문에 long으로 처리 필요



### 소스 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class X와K {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int X = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        // X의 2진수를 담을 배열
        int[] binaryX = new int[65];
        
        for (int i = 64; X > 0; X /= 2) {
            binaryX[i--] = X % 2;
        }
        // X가 5일 경우
        // binaryX = [0, 0, 0, ... , 0, 0, 1, 0, 1]


        long Y = 0, p = 1;
        for (int i = 64; i > 0; i--) {
			// K가 끝나면
            if (K <= 0) break;

            // X가 0인 부분 == 0 또는 1을 배치하여 Y를 만들 수 있음
            if (binaryX[i] == 0) {
                // 1을 배치했을 때 해당 자리에 맞는 2의 지수승 더해줌
                if (K % 2 == 1) {
                    Y += p;
                }
                K /= 2;
            }
            p *= 2;
        }
        System.out.println(Y);
    }
}

```

<br>

<br>

<br>

### 어려웠던 점

- K 규칙을 알아내는데 시간이 좀 걸렸다.
- 처음엔 String으로 접근했는데 뭐가 문젠지 계속 틀렸다고 나왔다..(반례 모르겠음)



#### Failed Code (처음 시도 코드)

````java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int X = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        String binaryX = Integer.toBinaryString(X);
        String binaryK = Integer.toBinaryString(K);

        int[] Y = new int[65];
        int xIdx = binaryX.length() - 1;
        for (int i = 64; i >= 0; i--) {
            if (xIdx < 0) break;
            Y[i] = Integer.parseInt(binaryX.substring(xIdx, xIdx + 1));
            xIdx--;
        }

        int kIdx = binaryK.length() - 1;
        for (int i = 64; i >= 0; i--) {
            if (Y[i] == 1) {
                Y[i] = 0;
                continue;
            }
            if (kIdx >= 0) {
                Y[i] = Integer.parseInt(binaryK.substring(kIdx, kIdx + 1));
                kIdx--;
            }
        }

        long result = 0;
        for (int i = 64; i > 0; i--) {
            if (Y[i] == 1) result += Math.pow(2, 64 - i);
        }
        System.out.println(result);
    }
}
````





