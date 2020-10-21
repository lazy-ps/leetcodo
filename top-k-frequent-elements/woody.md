# Problem

- [문제 링크](https://leetcode.com/problems/top-k-frequent-elements/)

<br>

난이도는 **_Medium_**

`Integer` 배열이 주어졌을 때 가장 많이 나타나는 `k` 개의 요소를 리턴하는 문제입니다.

단, 시간복잡도가 `O(n log n)` 보다 빨라야 합니다.

<br><br>

# Solution

풀이 방법은 두 가지가 있습니다.

<br>

## 1. Bucket Sort

첫 번째 풀이 방법은 Bucket Sort 를 활용하여 `O(n)` 에 구하는 방법입니다.

먼저 `HashMap (number, count)` 을 이용해서 각 숫자의 갯수를 전부 구합니다.

그리고 이번에는 2차원 배열 (코드에서는 `List[]`) 에 각 카운트 별로 해당되는 숫자를 전부 집어넣습니다.

예를 들면 [1, 1, 1, 1, 2, 2, 3, 4] 가 주어지면 아래와 같이 값이 들어갑니다.

```java
list[0] = null
list[1] = [3, 4] // 3, 4 는 한번만 나옴
list[2] = [2]
list[3] = null
list[4] = [1]
```

카운트가 없는 숫자의 인덱스는 `null` 이 되므로 `list` 를 거꾸로 순회하면서 `null` 이 아닌 숫자들을 `k` 개 만큼 집어넣으면 답을 구할 수 있습니다.

<br>

## 2. Priority Queue

두 번째 풀이 방법은 우선순위 큐를 활용하여 `O(n log k)` 에 구하는 방법입니다.

1번 풀이와 마찬가지로 `HashMap` 에 각 숫자의 갯수를 전부 구해둡니다.

그 뒤 `value` 가 작은 순서대로 나오도록 우선순위 큐에 조건을 걸고, `map` 에 있는 값들을 전부 넣어줍니다.

대신 이 때 단순히 넣어주기만 하면 시간복잡도가 `O(log n)` 이지만, `pq` 의 사이즈가 `k` 를 넘지않도록 유지한다면 우선순위 큐에 삽입하는 작업은 `O(log k)` 가 됩니다.

어차피 `k` 개의 숫자 외엔 필요 없으므로 `pq` 의 사이즈가 `k` 를 초과할 때마다 가장 작은 값을 하나씩 빼주면서 진행하면 됩니다.

<br><br>

# Java Code

```java
// 1. Use Bucket Sort
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        
        List<Integer>[] bucket = new List[nums.length + 1];
        for (Integer key : map.keySet()) {
            int count = map.get(key);
            
            if (bucket[count] == null) {
                bucket[count] = new ArrayList<>();
            }
            
            bucket[count].add(key);
        }
        
        List<Integer> list = new ArrayList<>();
        for (int i = bucket.length - 1; i >= 0 && list.size() < k; i--) {
            if (bucket[i] == null) continue;
            list.addAll(bucket[i]);
        }
        
        // list to array
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = list.get(i);
        }
        
        return res;
    }
}


// 2. Use Priority Queue
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        
        Queue<Integer> pq = new PriorityQueue<>(
            (a, b) -> map.get(b) - map.get(a)
        );
        
        for (Integer num : map.keySet()) {
            pq.add(num);
            
            if (pq.size() > k) {
                pq.poll();
            }
        }
        
        // list to array
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = pq.poll();
        }
        
        return res;
    }
}
```
