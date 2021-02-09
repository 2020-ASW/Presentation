# Task Scheduler



### 문제

- Task가 주어지면 한 시간 단위로 Task를 처리하게되고, 이때 순서는 맘대로 실행할 수 있다
- 같은 Task를 처리하기 위해선 n 시간만큼의 쿨타임이 필요하다



### 제약 조건

- 1 <= task.length <= 10^4
- Task는 항상 대문자 알파벳
- n은 0 ~ 100



### 접근 방식

- Task 의  빈도수 체크 후, 우선순위 큐에 저장 (우선순위는 빈도수의 내림차순)

```java
int[] frequency = new int[26];
for (char c : tasks) frequency[c - 'A']++;

Queue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
for (int f : frequency)
    if (f != 0) pq.add(f);
```

- Map에다 <time, frequency> 형식으로 스케쥴을 저장하고
- 하나씩 꺼내어 다음 시작되야될 타임에 스케쥴 등록

```java
// 시간초
int time = 0;
// 잔여 Task 수
int tasksRemaining = tasks.length;

while (tasksRemaining > 0) {
    // 스케쥴에 등록된 Task 확인
    Integer coolTimeTask = schedule.get(time);
    // Task가 있다면
    if (coolTimeTask != null)
        pq.add(coolTimeTask);

    Integer cur = pq.poll();
    // 현재 작업할 Task가 있다면
    if (cur != null) {
        tasksRemaining--;
        cur--;
        if (cur > 0)
            // 해당 Task가 끝나고 쿨타임 n초 뒤 스케쥴에 등록
            schedule.put(time + n + 1, cur);
    }
    // if문 두개에 모두 걸리지 않으면 idle 상태
    time++;
}
```





### 소스 코드

```java
public class TaskScheduler {
    public static int leastInterval(char[] tasks, int n) {

        int[] frequency = new int[26];
        for (char c : tasks) frequency[c - 'A']++;

        Queue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        for (int f : frequency)
            if (f != 0) pq.add(f);

        // <time, frequency>
        Map<Integer, Integer> schedule = new HashMap<>();

        int time = 0, tasksRemaining = tasks.length;

        while (tasksRemaining > 0) {
            Integer coolTimeTask = schedule.get(time);
            if (coolTimeTask != null)
                pq.add(coolTimeTask);

            Integer cur = pq.poll();
            if (cur != null) {
                tasksRemaining--;
                cur--;
                if (cur > 0)
                    schedule.put(time + n + 1, cur);
            }
            time++;
        }

        return time;
    }
}
```



### 어려웠던 점

- 문제 이해가 잘 되지 않았다..

