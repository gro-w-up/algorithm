```java
// 문제 설명을 읽고 생성자를 통해 Trie를 생성한 다음 인스턴스 메서드로 접근한다는 것을 알았음
// 그럼 문자열을 저장할 자료구조가 필요할 것이고, startwith 를 통해 문자열을 검색할 수도 있어야함.
// 만약 apple 이 들어있는데, app으로 startWith()를 호출했을때 true 가 나오려면?...
// 고민하다 답지를 사알짝 알파벳 하나하나를 노드로 여기는 방법을 사용함
class Trie {

    private TrieNode root;

    private class TrieNode {
        // 1. 인스턴스변수는 해시테이블로 자식노드를 값으로 갖는다
        // 2. endOfWord를 검사해야 prefix() 인지 startWith() 인지 검증할 수 있다.
        Map<Character, TrieNode> children;
        boolean isEndOfWord;

        public TrieNode() {
            this.children = new HashMap<>();
            this.isEndOfWord = false; // false로 초기화
        }
    }

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {
        TrieNode current = root;

        for (char ch : word.toCharArray()) {
            // 없으면 노드를 생성, 있으면 기존노드 반환
            current = current.children.computeIfAbsent(ch, c -> new TrieNode());
        }
        current.isEndOfWord = true;
        // 이러면 apple 에서
        // a -> p -> p -> l -> e 라는 노드가 저장되었음 e라는 노드는 endOfWord = true
    }

    public boolean search(String word) {
        TrieNode node = searchPrefix(word);
        // 완벽한 검색 메서드는, 접두사가 존재하면서 endOfWord까지 true 여야 한다. applew -> apple 일때 false
        if (node != null && node.isEndOfWord) {
            return true;
        }
        return false;
    }

    public boolean startsWith(String prefix) {
        // 그러나 접두사만 검색하는 메서드는 endOfWord를 검증할 필요가없음 searchPrefix 메서드만 통과하면 됨
        return searchPrefix(prefix) != null;
    }

    // 가장 중요한 prefix 를 찾는 메서드
    private TrieNode searchPrefix(String prefix) {
        TrieNode current = root;
        for (char ch : prefix.toCharArray()) {
            TrieNode node = current.children.get(ch);

            if (node == null) {
                // 해당 문자열에대한 노드가 없다면 null , 해당접두사가 트라이에 없다.
                return null;
            }
            current = node; // 이동
        }
        // 모든 문자를 순회했는데, 전부 존재한다면? 접두사가 있다.
        return current;
    }
}


```