```Java
class LRUCache {
    LinkedHashMap<Integer, Integer> cache = new LinkedHashMap<>();
    int capacity = 0;

    public LRUCache(int capacity) {
        this.capacity = capacity;
    }

    public int get(int key) {
        var v = cache.getOrDefault(key, -1);
        if (v != -1) {
            cache.remove(key);
            put(key, v);
        }

        return v;
    }

    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            cache.remove(key);
        }

        if (cache.size() == capacity) {
            cache.remove(cache.keySet().iterator().next());
        }

        cache.put(key, value);
    }
}
```