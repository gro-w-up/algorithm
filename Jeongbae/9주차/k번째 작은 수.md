```java
class Main {
    // 2차원 배열이 주어지며, 행 열에 대해 각각 오름차순이다. k 번째로 작은 수를 구해라.
    // 조건에 맞는 풀이에는 실패했지만 일단 봐주시면 감사하겠습니다 . . .
    public int kthSmallest(int[][] matrix, int k) {
        // 접근 : 어떤 자료구조에 값을 처음부터 끝까지 넣으면서 정렬한다. 그리고 k개 이상일때를 처리해주면 된다.
        // reverseOrder를 하지 않으면 정수형에대해 오름차순 정렬입니다.
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());

        for (int[] ints : matrix) {
            for (int num : ints) {
                // 우선순위 큐에 숫자를 넣으며 정렬합니다.
                pq.offer(num);

                // 어차피 정렬은 끝났고, k보다 큐의 크기가 커지면 *큰 수*를 버리면 됩니다.
                if (pq.size() > k) {
                    pq.poll();
                }
            }
        }
        // 이제 가장 큰 수를 반환하면, k 번째로 작은 수가 됩니다.
        return pq.peek();
    }
    // [[1,5,9],[10,11,13],[12,13,15]] 를 순서대로 넣으면 아래와 같이 변합니다.
    // ex) 1,5,9,10,11,12,13,13 (8번째, k번째 숫자인 13이 정답)
    // 시간복잡도는 O(logK * N^2) 가 됩니다.

}
```