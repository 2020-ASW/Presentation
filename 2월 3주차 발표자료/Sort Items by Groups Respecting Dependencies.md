### Sort Items by Groups Respecting Dependencies

![image-20210223202937887](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210223202937887.png)

##### 사용한 알고리즘

- Topology Sort(O(n))



##### 풀이 로직

- 같은 그룹에 있는 아이템은 한번에 묶여서 나와야한다.

- 그룹이 -1인 아이템은 묶여서 나올 필요 없이 순서에 맞는다면 언제든지 나와도 상관없다.

- 순서를 이용하여 위상정렬을 하고 인접 리스트를 만들 때 각각의 그룹에 대한 인접 리스트도 같이 만들어 준다.

  ![image-20210224045754409](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210224045754409.png)

- beforeItems 를 순회하며 같은 그룹인 경우에는 그룹에서 사용할 진입차수를 계산해 주고 서로 다른 그룹이라면 전체 그룹간의 위상정렬을 위해 진입차수를 계산해준다.

- 순서를 그룹간의 위상정렬을 진행하고 한 그룹에 들어갔을 때 그룹 내에서 한번도 위상정렬을 해서 순서를 맞춰준다.

- 여기에서 -1인 그룹이 문제가 되었는데 -1은 묶여서 나올 필요가 없으므로 각각 m에서 하나씩 높은 수로 그룹을 지정해주었다.



##### Code

```javascript
/**
 * @param {number} n
 * @param {number} m
 * @param {number[]} group
 * @param {number[][]} beforeItems
 * @return {number[]}
 */
var sortItems = function(n, m, group, beforeItems) {
    const ans = []
    const narr = Array(n)
    const groupContain = {}
    const vis = Array(n).fill(0)
    for (let i=0; i<n; i++){
        narr[i] = []
        // 초기화를 하면서 -1인 그룹은 각각 서로 다른 그룹으로 지정!
        if (group[i] == -1){
            group[i] = m
            m += 1
        }
        // 그룹에 어떤 수가 있는지를 저장
        if (groupContain[group[i]]){
            groupContain[group[i]].push(i)
        } else{
            groupContain[group[i]] = []
            groupContain[group[i]].push(i)
        }
    }
    // 전체 그룹들 간의 진입차수
    const groupIndegree = Array(m).fill(0)
    // 그룹 내부에서 각각의 노드들간의 진입차수
    const indegree = Array(n).fill(0)
    
    
    beforeItems.forEach((arr,nxt)=>{
        let ngroup = group[nxt]
        arr.forEach(cur=>{
            let cgroup = group[cur]
            narr[cur].push(nxt)
            if (ngroup == cgroup){
                indegree[nxt] += 1
            } else{
                groupIndegree[ngroup] += 1
            }
        })
    })
    
    const groupQueue = []
    // 그룹 진입차수가 0인 그룹들을 모두 큐에 넣어준다.
    groupIndegree.forEach((val,idx)=>{
        if (val == 0){
            groupQueue.push(idx)
        }
    })
    
    // 그룹들을 순회하면서 그룹 내부에서의 위상정렬을 통해 순서를 맞춰준다.
    while (groupQueue.length){
        let curGroup = groupQueue.shift()
        let groupItems = groupContain[curGroup]
        console.log(curGroup, groupItems)
        // 그룹에 아무 값이 없는 경우가 존재해서 넘겨주었다.
        if (!groupItems) continue
        let queue = []
        // 그룹 내부에서 진입차수를 이용하여 위상정렬
        groupItems.forEach(val=>{
            if (indegree[val] == 0){
                queue.push(val)
            }
        })
        while (queue.length){
            let cur = queue.shift()
            if (vis[cur] == 0){
                ans.push(cur)
                vis[cur] = 1
            }
            let cgroup = group[cur]
            narr[cur].forEach(val=>{
                let ngroup = group[val]
                // 현재 그룹과 다음에 나올 그룹이 같다면 진입차수 확인 후 그룹 내부 큐에 넣어준다. 
                if (cgroup == ngroup){
                    if (indegree[val] == 1){
                        indegree[val] = 0
                        queue.push(val)
                    } else if(indegree[val] > 0){
                        indegree[val] -= 1
                    }
                } else{
                    // 현재 그룹과 다음 그룹이 다른경우 그룹 진입차수를 확인하고 그룹 큐에 넣어준다.
                    if (groupIndegree[ngroup] == 1){
                        groupIndegree[ngroup] = 0
                        groupQueue.push(ngroup)
                    } else if (groupIndegree[ngroup] > 0){
                        groupIndegree[ngroup] -= 1
                    }
                }
            })
        }
    }
    // 그룹을 순회하면서 순서가 올바르지 않을 경우 탐색할 수 없는 그룹이 생긴다.
    if (ans.length == n){
        return ans
    } else{
        return []
    }
    
};
```

