[LRU Cache - LeetCode](https://leetcode.com/problems/lru-cache/)

```java
import java.util.LinkedHashMap;

public class LRUCache {

    public Integer capacity;
    public LinkedHashMap<Integer, Integer> cache;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new LinkedHashMap(); // 순서대로 저장 해주는 Map
    }

    public int get(int key) {
        if (cache.containsKey(key)) {
            Integer value = cache.get(key);
            cache.remove(key);
            cache.put(key, value); // 사용했으니 맨 뒤로 보내기
            return value;
        }
        return -1; // 없는 경우 -1
    }

    public void put(int key, int value) {

        removeDuplecateKey(key);

        cache.put(key, value);

        removeLeastRecentlyUsed();

    }

    private void removeLeastRecentlyUsed() {
        if (cache.size() > capacity) {
            // 가장 오래된(먼저 들어온) 키값이 삭제 대상
            Integer removeKey = cache.keySet().iterator().next();
            cache.remove(removeKey);
        }
    }

    private void removeDuplecateKey(int key) {
        if (cache.containsKey(key)) {
            cache.remove(key);
        }
    }
}
```
