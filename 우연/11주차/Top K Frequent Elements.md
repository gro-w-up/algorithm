https://leetcode.com/problems/top-k-frequent-elements/description/

---

Given an integer array `nums` and an integer `k`, return *the* `k` *most frequent elements*. You may return the answer in **any order**.

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]

```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]

```

**Constraints:**

- `1 <= nums.length <= 105`
- `104 <= nums[i] <= 104`
- `k` is in the range `[1, the number of unique elements in the array]`.
- It is **guaranteed** that the answer is **unique**.

**Follow up:** Your algorithm's time complexity must be better than `O(n log n)`, where n is the array's size.

---

### HashMap 사용 연산 풀이

```java
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;

/**
 * leetcode
 * https://leetcode.com/problems/top-k-frequent-elements
 */
public class TopKFrequentElements {

    /// fields
    private Map<Integer/*엘리먼트*/, Integer/*빈도수*/> frequencyMap;

    /// Constructor
    public TopKFrequentElements() {
        this.frequencyMap = new HashMap<>();
    }

    /// Method
    public int[] solution(int[] nums, int k) {
        makeFrequencyMap(nums);
        return getTopKFrequentElements(makeBuckets(), k);
    }

    // 주어진 배열을 통해 빈도수 맵을 구축
    private void makeFrequencyMap(int[] nums) {
        frequencyMap = Arrays.stream(nums)
                .boxed()  // int -> Integer
                .collect(Collectors.toMap(
                        Function.identity(),  // 키: 엘리먼트
                        val -> 1,             // 각 빈도수의 초기 빈도수는 1
                        Integer::sum          // {1, 1, 1, 2, 2, 3, 4} -> {1=3, 2=2, 3=1, 4=1}
                ));
    }

    // 빈도수를 기반으로 요소를 분류하는 버킷 생성
    private Map<Integer, List<Integer>> makeBuckets() {
        return frequencyMap.entrySet()
                .stream()
                .collect(Collectors.groupingBy(
                        Map.Entry::getValue, // 키: 빈도수
                        Collectors.mapping(Map.Entry::getKey, Collectors.toList())
                ));
    }

    // 가장 빈도가 높은 K개의 요소를 가져옴
    private int[] getTopKFrequentElements(Map<Integer/*빈도수*/, List<Integer>/*엘리먼트*/> buckets, int k) {
        return buckets.entrySet()
                .stream()
                .sorted((e1, e2) -> Integer.compare(e2.getKey(), e1.getKey())) // 빈도수를 기준으로 내림차순 정렬
                .flatMap(e -> e.getValue().stream()) // 엘리먼트 리스트를 하나의 스트림으로 만듦
                .limit(k) // 상위 k개의 요소만 선택
                .mapToInt(Integer::intValue) // Integer -> int
                .toArray(); // int 배열로 반환하여 리턴
    }

}
```

### 우선 순위 큐 풀이

```java
public int[] solution(int[] nums, int k) {
    // 요소의 빈도를 계산
    Map<Integer, Integer> frequencyMap = Arrays.stream(nums)
            .boxed()
            .collect(Collectors.toMap(
                    num -> num,
                    num -> 1,
                    Integer::sum
            ));

    // 빈도수를 기준으로 우선순위 큐(최소 힙) 사용
    PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>(
            Map.Entry.comparingByValue()
    );

    frequencyMap.entrySet().stream()
            .forEach(entry -> {
                pq.add(entry);
                if (pq.size() > k) {
                    pq.poll();
                }
            });

    // 상위 k개의 빈도수를 가지는 요소들을 결과 배열로 변환
    int[] result = pq.stream()
            .map(Map.Entry::getKey)
            .mapToInt(Integer::intValue)
            .toArray();

    return result;
}
```
