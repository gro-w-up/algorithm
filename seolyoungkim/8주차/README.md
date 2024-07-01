```java
import static org.assertj.core.api.Assertions.assertThat;

import java.util.PriorityQueue;
import org.junit.jupiter.api.Test;

/**
 * <a href="https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/submissions/1305916087/">378</a>
 */
public class KthSmallestElementTest {
    /**
     * @param matrix 오름차순으로 정렬된 n x n 행렬
     * @param k 1 <= k <= n^2
     * @return 행렬에서 k번째로 작은 요소 반환
     */
    public int kthSmallest(int[][] matrix, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int[] nums : matrix) {
            for (int num : nums) {
                pq.add(num);
            }
        }

        for (int i = 0; i < k - 1; i++) {
            pq.poll();
        }

        return pq.poll();
    }

    @Test
    void test() {
        assertThat(kthSmallest(new int[][]{{1, 5, 9}, {10, 11, 13}, {12, 13, 15}}, 8)).isEqualTo(13);
    }

    @Test
    void test2() {
        assertThat(kthSmallest(new int[][]{{-1}}, 1)).isEqualTo(-1);
    }
}
```
