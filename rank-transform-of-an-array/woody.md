# Problem

- [문제 링크](https://leetcode.com/problems/rank-transform-of-an-array/)

<br>

주어진 배열의 값을 비교하여 순위를 매긴 배열을 리턴하는 문제입니다.

<br><br>

# Solution

배열을 복사한 뒤에 정렬합니다.

배열의 값과 순위를 저장할 `HashMap` 을 선언합니다.

복사한 `copyed` 배열을 정렬한 다음에 순위를 넣습니다.

동점인 경우 값을 갱신하지 않기 위해 `putIfAbsent` 메소드를 사용합니다.

`arr` 배열을 다시 순회하면서 `copyed` 배열을 랭크값으로 갱신해주면 됩니다.

<br><br>

# Java Code

```java
class Solution {
    public int[] arrayRankTransform(int[] arr) {
        Map<Integer, Integer> map = new HashMap<>();    // value, rank
        int[] copyed = Arrays.copyOf(arr, arr.length);
        Arrays.sort(copyed);
        
        for (int value : copyed) {
            map.putIfAbsent(value, map.size());
        }
        
        for (int i = 0; i < arr.length; i++) {
            copyed[i] = map.get(arr[i]) + 1;
        }
        
        return copyed;
    }
}
```
