```java

public class Main{

    // 1. 배열의 길이가 10만... 이러면 N^2 이상의 알고리즘은 쓸 수 없다.. brute force 쓸 수 없다...
    // 배열의 요소를 한번씩 고정한 뒤에, 나머지 두 개를 더해서 0이 된다면 풀이가 two sum과 비슷하다 투포인터로 가자.
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        // 2. 배열을 정렬한 이유는 중복된 요소를 쉽게 건너뛰기 위함
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {
            // 3. 중복되는 값이 있다면 스킵
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            // 4. 고정포인터 다음부터 left, 배열의 끝에서 시작하는 right로 투 포인터 설정
            int left = i + 1;
            int right = nums.length - 1;
            int target = -nums[i];

            while (left < right) {
                int sum = nums[left] + nums[right];

                // 5. 만약 0이 된다면? 리스트에 정답을 추가하고 포인터 이동
                if (sum == target) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    left++;
                    right--;

                    // 5-1. 리스트에 정답을 추가했는데, 또 중복을 제거해 주지 않으면 삼중배열이 중복으로 result에 add되는 현상 발생
                    // 반례 [-1, -1, 0, 0, 1, 1]
                    // index 1 인 -1이 시작점일때 바로 다음의 0, 맨 마지막의 1이 조건에 만족해 -1, 0, 1 이 정답에 추가된다
                    // 그리고 또 중복된 값을 건너뛰어주지 않으면 index 3의 0, index 4의 1이 추가로 더해진다
                    // 그리고 이미 기준점이 정해졌는데, 왼쪽포인터 오른쪽 포인터의 값 중 하나의 값만 달라진다면 정답이 될리가 없으므로 양쪽 다 같은수(중복)을 제거해준다.
                    while (left < right && nums[left] == nums[left - 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right + 1]) {
                        right--;
                    }
                } else if (sum < target) {
                    // 6. target보다 못미치면 포인터 오른쪽으로 밀어 -> 합을 늘려준다
                    left++;
                } else {
                    // 7. target보다 넘치면 포인터 왼쪽으로 밀어 -> 합을 줄여준다
                    right--;
                }
            }
        }
        return result;
    }
}
```