```Java
public class Trie {
    private TrieNode root = new TrieNode();

    private static class TrieNode {
        private TrieNode[] links;
        private final int R = 26;
        private boolean isEnd;

        public TrieNode() {
            links = new TrieNode[R];
        }

        public boolean containsKey(char ch) {
            return links[ch - 'a'] != null;
        }

        public TrieNode get(char ch) {
            return links[ch - 'a'];
        }

        public void put(char ch, TrieNode node) {
            links[ch - 'a'] = node;
        }

        public void setEnd() {
            isEnd = true;
        }

        public boolean isEnd() {
            return isEnd;
        }
    }

    public Trie() {
    }

    public void insert(String word) {
        TrieNode node = root;
        for (char currentChar : word.toCharArray()) {
            if (!node.containsKey(currentChar)) {
                node.put(currentChar, new TrieNode());
            }
            node = node.get(currentChar);
        }
        node.setEnd();
    }

    public boolean search(String word) {
        TrieNode node = searchPrefix(word);
        return node != null && node.isEnd();
    }

    public boolean searchWith(String prefix) {
        return searchPrefix(prefix) != null;
    }

    private TrieNode searchPrefix(String prefix) {
        TrieNode node = root;
        for (char currentChar : prefix.toCharArray()) {
            if (node.containsKey(currentChar)) {
                node = node.get(currentChar);
            } else {
                return null;
            }
        }
        return node;
    }
}
```

# 접근법
searchWith는 문자열이 포함되어있는지 검토를 해야한다.

contain 의 개념이고, 다음과 같을 때 메모리와 속도를 둘 다 잡고 싶어서 트리를 개념을 도입했다.

```
a - b - c - d 
        | - f
```

만약, abd 가 추가가 된다면 어떻게 해야할까?
```
a - b - c - d 
    |   | - f
    
    | - d

```
와 같이 구성을 하게 된다면, 중복된 데이터의 저장 공간을 활용할 수 있으며 탐색 또한 좋아지게 될 것이라 판단되었다.

그리고, 현재 사용하는 문자열은 26개로 제한이 되어있기때문에 26 으로 linked List 를 구성하였다.


