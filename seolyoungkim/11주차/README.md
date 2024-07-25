```java
public int majorityElement(int[] nums) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int num : nums) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }

    return map.entrySet()
            .stream()
            .filter(entry -> entry.getValue() > nums.length / 2)
            .map(Entry::getKey)
            .findAny()
            .get();
}
```
