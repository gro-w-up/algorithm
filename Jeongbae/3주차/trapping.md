```java
// 문제풀이 실패 보지마세요

public class Main{
    public static int trap(int[] height) {
        Deque<Integer> deque = new LinkedList<>();
        int sum = 0;

        // 세번째 조건식에서 막혀 풀이 실패.. 세번째 조건식이 있거나 없어도 문제의 조건을 만족하지 않음
        for (int i = 0; i < height.length; i++) {

            if (!deque.isEmpty() && height[deque.getLast()] <= height[i] && height[deque.getFirst()] <= height[i]) {
                int min = Math.min(height[deque.getFirst()], height[i]);
                while (!deque.isEmpty()) {
                    int water = min - height[deque.pollLast()];
                    if (water > 0) {
                        sum += water;
                    }
                }
            }
            deque.add(i);
        }
        return sum;
    }
}

```