
```java
public class Main {
    public int[] twoSum(int[] nums, int target) {

        // 1. 배열을 복사한 뒤에 정렬하고, 이 정렬된 배열을 통해 어떤 값을 더해야 target이 되는지 알아낸다.
        // 2. 그리고 그 해답이 되는 두 정수를 알고있으니 원래 배열에서 그 정수의 index를 찾겠다.
        int[] copiedArray = nums.clone();
        Arrays.sort(copiedArray);
        int start = 0;
        int end = nums.length - 1;

        while (start < end) {
            int tmp = copiedArray[start] + copiedArray[end];
            if (tmp == target) {
                break;
            }
            if (tmp > target) {
                end--;
            } else {
                start++;
            }
        }

        // 3. first 는 내가 찾아야하는 첫번째 값이 된다, second 는 내가 찾아야하는 두 번째 값이 된다.
        // 이 둘을 더한다면 target이 될 것이다.
        int first = copiedArray[start];
        int second = copiedArray[end];

        int[] answer = new int[2];
        boolean firstAnswer = false;
        boolean secondAnswer = false;

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == first && !firstAnswer) {
                answer[0] = i;
                firstAnswer = true;
                // 4. continue를 넣은 이유는 현재 인덱스의 nums배열 값이 first, second와 같기 때문에 이것을 건너뛰기 위함이다.
                // 어차피 boolean으로 flag를 세우고 있으니 다시 첫번째 if문에 들어올 일은 없다.
                // 이 과정이 필요한 이유는 계속 같은 첫번째 if문이 계속 충족되면서 answer[0] = i ... 를 계속 반복하게된다
                // 예를 들어 [2, 5, 5, 11] 가 배열이고 target이 10이라면 [1, 2] 가 정답이 될텐데
                // answer[0] = 1, answer[0] = 2 ... 후 for문이 끝나게 되어 [2, 0] 과 같은 오답이 발생한다.
                continue;
            }
            if (nums[i] == second && !secondAnswer) {
                answer[1] = i;
                secondAnswer = true;
            }
        }
        return answer;
    }
}
```
