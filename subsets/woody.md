# Problem

- [문제 링크](https://leetcode.com/problems/subsets/)

<br>

난이도는 **_Medium_**

배열이 주어졌을 때, 중복되지 않는 모든 Subset, 즉 PowerSet 을 구하는 문제입니다.

<br><br>

# Solution

Bit 를 사용해도 되고 재귀로 해도 되고 백트래킹으로 해도 되고 방법은 많습니다.

가장 구현하기 편한 백트래킹으로 했습니다.

<br><br>

# Java Code

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        dfs(nums, res, new ArrayList<>(), 0);
        return res;
    }
    
    private void dfs(int[] nums, List<List<Integer>> res, List<Integer> list, int start) {
        res.add(new ArrayList<>(list));
        
        for (int i = start; i < nums.length; i++) {
            list.add(nums[i]);
            dfs(nums, res, list, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```
