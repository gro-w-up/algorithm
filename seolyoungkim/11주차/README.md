```java
class Trie {
    static class Node{
        HashMap<Character, Node> child = new HashMap<>();  //노드의 자식 노드들을 저장
        boolean endOfWord;  //이 노드가 단어의 끝인지 저장
    }

    Node root;

    public Trie() {
        this.root = new Node();
    }
    
    public void insert(String word) {
        Node node = this.root;

        for (char c : word.toCharArray()) {
            node.child.putIfAbsent(c, new Node());
            node = node.child.get(c);
        }

        node.endOfWord = true;
    }
    
    public boolean search(String word) {
        Node node = this.root;

        for (char c : word.toCharArray()) {
            if (node.child.containsKey(c)) {
                node = node.child.get(c);
            } else {
                return false;
            }
        }

        return node.endOfWord;
    }
    
    public boolean startsWith(String prefix) {
        Node node = this.root;

        for (char c : prefix.toCharArray()) {
            if (!node.child.containsKey(c)) {
                return false;
            }
            node = node.child.get(c);
        }

        return true;
    }
}
```
