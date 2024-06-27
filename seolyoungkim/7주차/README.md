```java
package stack;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.Deque;

/**
 * <a href="https://www.acmicpc.net/problem/15926">15926</a>
 */
public class ParenthesesKing {
    public static void main(String[] args) throws IOException {
        try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
            int length = Integer.parseInt(br.readLine());
            String parentheses = br.readLine();

            Deque<Integer> stack = new ArrayDeque<>();
            stack.push(-1);  // 길이를 잴 시작점 역할

            int answer = 0;
            for (int i = 0; i < length; i++) {
                char c = parentheses.charAt(i);
                if (c == '(') {
                    stack.push(i);
                } else {  // 닫힌 괄호인 경우
                    stack.pop();  // 인덱스를 하나 제거한다
                    if (stack.isEmpty()) {  // 완전히 비었을 경우, 길이를 잴 시작 지점으로 만든다
                        stack.push(i);  // 닫힌 괄호 인덱스를 하나 넣는다. 이는 길이를 잴 시작 기준점이 된다.
                        continue;
                    }

                    Integer idx = stack.peek();
                    answer = Math.max(answer, i - idx);
                }
            }
            System.out.println(answer);
        }
    }
}
```
