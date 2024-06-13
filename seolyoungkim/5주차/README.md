```java
static class LRUCache {
    private final int capacity;

    private final Map<Integer, Node> cache = new HashMap<>();
    private int size;

    private Node head;
    private Node tail;

    /**
     * 양수 capacity로 LRU 캐시를 초기화
     * @param capacity LRU 캐시의 용량 (양수)
     */
    public LRUCache(int capacity) {
        this.capacity = capacity;

        this.head = new Node();
        this.tail = new Node();

        head.next = tail;
        tail.prev = head;
    }

    private static class Node {
        int key;
        int value;
        Node prev;
        Node next;

        public Node() {
        }

        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    /**
     * key에 해당하는 value를 리턴
     * @param key LRU 캐시의 키
     * @return key가 존재하면 키의 값을 반환, 없을 경우 -1 반환
     */
    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }

        Node node = cache.get(key);
        moveToHead(node);  // 노드를 호출하면 캐시의 head 뒤로 이동시킨다

        return node.value;
    }

    private void moveToHead(Node node) {
        removeNode(node);  // 본래 위치에서 제거한다
        addNodeNextToHead(node);  // 노드를 헤드 바로 뒤에 위치시킨다
    }

    private void removeNode(Node node) {
        if (cache.containsKey(node.key)) {
            Node prev = node.prev;
            Node next = node.next;

            prev.next = next;
            next.prev = prev;
        }
    }

    /**
     * head와 head.next 사이에 넣으면 최근에 참조된 값일수록 head.next쪽에, 가장 늦게 참조된 값일수록 tail.prev쪽에 위치한다.
     */
    private void addNodeNextToHead(Node node) {
        // 노드를 head와 head.next 사이에 넣는다
        node.prev = head;
        node.next = head.next;

        head.next.prev = node;
        head.next = node;
    }

    /**
     * <li>key가 존재하면 key 값을 업데이트, 없으면 key-value 쌍을 캐시에 추가</li>
     * <li>key 수가 capacity를 초과하는 경우 가장 적게 사용된 키를 제거한다</li>
     * @param key LRU 캐시의 키
     * @param value key에 해당하는 value
     */
    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            node.value = value;
            moveToHead(node);
            return;
        }

        // 새로운 노드 추가
        Node newNode = new Node(key, value);
        cache.put(key, newNode);
        addNodeNextToHead(newNode);
        size++;

        // 사이즈가 capacity보다 크면 노드 제거
        if (size > capacity) {
            // tail 제거
            Node tail = popTail();
            cache.remove(tail.key);
            size--;
        }
    }

    private Node popTail() {
        Node prev = tail.prev;
        removeNode(prev);
        return prev;
    }
}
```
