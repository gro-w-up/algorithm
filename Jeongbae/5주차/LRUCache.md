```java
public class Main {
    // 발표 걸린다면 뭔가 죄송할 것 같습니다.. 풀이가 얻어걸린 느낌이에요
    public static void main(String[] args) {
        // input
        String[] input = new String[]{"LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"};

        // 원활한 테스트를 위해 가능 용량은 5로 하겠습니다.
        LRUCache lruCache = new LRUCache(5);
    }
}


class LRUCache {
    // 1. 가장 먼저 생각해야할 부분은 자료구조입니다. 무슨 자료구조를 이용해야 key를 삭제하는 것과 value 를 꺼내는 것이 용이할까..? (Map 이 바로 떠오름)
    // 2. 근데 LinkedList 는 중간의 노드를 삭제하거나, 특정 값을 찾을때도(순회하니까) 그렇게 적절한 자료구조는 아닌 것 같은데..? 라는 생각을 했습니다.
    // 3. 키 값과 쌍을 저장하고 싶다는 생각 + 순서를 유지하고 싶다는 생각을 하니까..? Java 의 LinkedHashMap 자료구조밖에 생각이 안났습니다. 일단 해보고 시간초과나면 다른 자료구조 생각해야지!
    private final int capacity;
    private final LinkedHashMap<Integer, Integer> map;

    // 4. 생성자로 초기화해주고
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new LinkedHashMap(capacity, 0.75f, true);
        // 6. 이 3번째 파라미터 true로 지정해주면, 순서가 유지된 HashMap 의 순서를 변경할 수 있고
        // 두 번째 파라미터인 0.75f 는 해시맵이 75% 차면 크기를 늘리겠다는 의미입니다.
        // 해시 자료구조에 대해서는 다같이 공부해봐요! 충돌 최소화를 위한 여러 확장 방법이 있습니다.
    }

    public int get(int key) {
        if (map.get(key) != null) {
            return map.get(key);
        }
        return -1;
    }

    public void put(int key, int value) {
        // 5. 우선 put 부터 고려해봤습니다. 이때 고민은 어떻게 key가 최근에 사용되었다는걸 검증하지..? 입니다. 저의 답은 그냥 맨 처음으로 옮기자 입니다.
        // 맨 처음으로 옮기고, 뒤에 있는 것 부터 값을 삭제하면 가장 나중에 참조된 key 가 삭제되지 않을까 라고 생각했습니다.
        // 근데 순서를 유지하는 LinkedMap 에서 순서를 옮기는 기능은 사용해본적이 없었습니다. 그래서 LinkedHashMap 의 기능에 대해 검색을 하는 도중에
        // 이러한 생성자를 봤고 이때 LRU 로 동작한다는 것을 봐버렸습니다...? 가장 최근에 접근한 값이 연결리스트의 마지막으로 이동하게 됩니다... capacity 를 넘어가는 순간 맨 앞의 값을 삭제하면 되겠죠..?
        map.put(key, value);

        // put 을 했으면, 용량이 다 찼는지 확인합니다
        if (map.size() > capacity) {
            int least = map.keySet().iterator().next();
            map.remove(least);
        }
    }
}
```

