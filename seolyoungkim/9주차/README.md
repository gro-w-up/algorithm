```java
public static void main(String[] args) throws IOException {
    try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
        int cardNumber = Integer.parseInt(br.readLine());

        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int i = 0; i < cardNumber; i++) {
            pq.offer(Integer.parseInt(br.readLine()));
        }

        int result = 0;
        while (pq.size() > 1) {
            int sum = pq.poll() + pq.poll();
            pq.offer(sum);

            result += sum;
        }

        System.out.println(result);
    }
}
```
