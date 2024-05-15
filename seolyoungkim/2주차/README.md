[백준 15927](https://www.acmicpc.net/problem/15927)

```java
package stringprocess;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

/**
 * <a href="https://www.acmicpc.net/problem/15927">백준 15927번</a>
 */
public class NotPalindrome {
    public static void main(String[] args) throws IOException {
        try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
            String target = br.readLine();
            int targetLength = target.length();

            // 회문이 아니면 길이 반환
            if (isNotPalindrome(target)) {
                System.out.println(targetLength);
                return;
            }

            // 하나의 문자로만 이뤄진 경우 회문이 아닌 부분 문자열은 없음
            if (containsOnlyOneChar(target)) {
                System.out.println(-1);
                return;
            }

            // 회문구조가 하나의 문자로만 이뤄지지 않은 경우, 한 개의 문자만 제외하면 회문이 아니게 됨
            System.out.println(targetLength - 1);
        }
    }

    private static boolean isNotPalindrome(String target) {
        int leftIndex = 0;
        int rightIndex = target.length() - 1;

        while (leftIndex < rightIndex) {
            if (target.charAt(leftIndex) != target.charAt(rightIndex)) {
                return true;
            }
            leftIndex++;
            rightIndex--;
        }

        return false;
    }

    private static boolean containsOnlyOneChar(String target) {
        int length = target.length();
        char firstChar = target.charAt(0);
        for (int i = 1; i < length; i++) {
            if (firstChar != target.charAt(i)) {
                return false;
            }
        }

        return true;
    }
}

```

1. 회문 구조와 회문 구조가 아닌 문자열을 먼저 구분
  - 회문 구조가 아닐 경우, 문자열의 길이 출력
2. 회문 구조일 경우 2개의 케이스가 있으며, 각각 판단 기준이 다름 
  - 하나의 문자로만 구성된 경우
    - 절대 비회문 구조가 탄생할 수 없음 -> `-1` 출력 
  - 두 개 이상의 문자로 구성된 경우
    - 문자 1개만 빼도 대칭이 파괴되어 비회문 구조가 됨 -> `문자열 길이 - 1` 출력 
