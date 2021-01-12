# baekjoon 네 개의 소수



### **사용한 알고리즘**

- 에라토스테네스의 체 (소수 찾기)

 

### **풀이 로직**

- 소수 = 2 3 5 7 11 13 17 19 23 29,,
- 1부터 직접 해본 결과 8부터 4개의 소수로 표현할 수 있다.
- 8 = 2 + 2 + 2 + 2 / 9 = 2 + 2 + 2 + 3 => 2 + 3 + 2+ 2
- n이 8이상이고 n % 2 == 0이면 '2 2'로 시작한다. -> n -= 4
- n이 8이상이고 n % 2 == 1이면 '2 3'으로 시작한다. -> n -= 5
- 새로 갱신된 n을 기준으로 에라토스테네스의 체를 이용해 n보다 작은 소수를 찾는다.



### **에라토스테네스의 체**

- 범위에서 합성수를 지우는 방식으로 소수를 찾는 방법
- 1 제거 
- 2 소수로 채택 -> 2 배수 지움
- 3, 5, 7 같은 방식으로 진행
- ![image-20210111000832673](img\에라토스테네스의 체.png)

- ```python
  def make_prime(n):
      a = [False,False] + [True]*(n-1)
  
      for i in range(2,n+1):
          if a[i]:
              for j in range(2*i, n+1, i):
                  a[j] = False
  
      return a
  ```

  

### **코드**

```python
def make_prime(n):
    a = [False,False] + [True]*(n-1)

    for i in range(2,n+1):
        if a[i]:
            for j in range(2*i, n+1, i):
                a[j] = False

    return a


n = int(input())
answer = ''  # answer를 초기화하자!!
if n < 8:
    print(-1)

if n >= 8 and n % 2 == 1:
    answer = '2 3 '
    n -= 5

elif n >= 8 and n % 2 == 0:
    answer = '2 2 '
    n -= 4

primes = make_prime(n)

for i in range(2, n+1):
    if primes[i] and primes[n - i]:
        answer += str(i) + ' ' + str(n-i)
        print(answer)
        break
```

