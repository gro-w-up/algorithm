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
        if (deleteCount >= 2) {  // deleteCount가 2 이상이면 아무것도아님 
            return 2;
        }

        while (leftIdx < rightIdx) {
            if (str.charAt(leftIdx) == str.charAt(rightIdx)) {
                leftIdx++;
                rightIdx--;
                continue;
            }

            int firstResult = palindrome(str, leftIdx + 1, rightIdx, deleteCount + 1);  // 왼쪽부터 이동시켜서 확인 
            int secondResult = palindrome(str, leftIdx, rightIdx - 1, deleteCount + 1);  // 오른쪽부터 이동시켜서 확인 
            return Math.min(firstResult, secondResult);  // 둘 중 숫자가 더 작은것 반환 (ex: 유사회문 결과 vs 아무것도 아닌 결과)
        }

        return deleteCount;  // deleteCount = 0 -> 회문 , deleteCount = 1 -> 유사 회문 
    }
}

```
