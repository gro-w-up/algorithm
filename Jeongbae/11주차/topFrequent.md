```java
class main {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> countHashMap = new HashMap<>();
        for (int num : nums) {
            countHashMap.put(num, countHashMap.getOrDefault(num, 0) + 1);
        }

        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[1] - a[1]);

        for (Integer integer : countHashMap.keySet()) {
            pq.add(new int[]{integer, countHashMap.get(integer)});
        }

        int[] results = new int[k];

        for (int i = 0; i < k; i++) {
            results[i] = pq.poll()[0];
        }

        return results;
    }

}
```