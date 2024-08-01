``` java

package org.example;

import java.util.*;

/**
 * leetcode
 * https://leetcode.com/problems/implement-trie-prefix-tree/description/
 */
public class ImplementTrie {

    /// Fields
    private final Map<String, Boolean> wordMap;
    private final Map<String, Boolean> prefixMap;

    /// Constructor
    public ImplementTrie() {
        this.wordMap = new HashMap<>();
        this.prefixMap = new HashMap<>();
    }

    /// Method
    public void insert(String word) {
        wordMap.put(word, true);
        for (int i = 1; i <= word.length(); i++) {
            prefixMap.put(word.substring(0, i), true);
        }
    }

    public boolean search(String word) {
        return wordMap.getOrDefault(word, false);
    }

    public boolean startsWith(String prefix) {
        return prefixMap.getOrDefault(prefix, false);
    }

}

```
