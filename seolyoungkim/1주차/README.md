[백준 문제 링크](https://www.acmicpc.net/problem/17609)

```java
package stringprocess;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

/**
 * <a href="https://www.acmicpc.net/problem/17609">백준 17609번</a>
 */
public class Palindrome {
    public static void main(String[] args) throws IOException {
        try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out))) {
            int count = Integer.parseInt(br.readLine());

            for (int i = 0; i < count; i++) {
                String str = br.readLine();
                int length = str.length();
                int leftIdx = 0;
                int rightIdx = length - 1;

                bw.write(palindrome(str, leftIdx, rightIdx, 0) + "\n");
            }
        }
    }

    public static int palindrome(String str, int leftIdx, int rightIdx, int deleteCount) {
        if (deleteCount >= 2) {
            return 2;
        }

        while (leftIdx < rightIdx) {
            if (str.charAt(leftIdx) == str.charAt(rightIdx)) {
                leftIdx++;
                rightIdx--;
                continue;
            }

            int firstResult = palindrome(str, leftIdx + 1, rightIdx, deleteCount + 1);
            int secondResult = palindrome(str, leftIdx, rightIdx - 1, deleteCount + 1);
            return Math.min(firstResult, secondResult);
        }

        return deleteCount;
    }
}

```
