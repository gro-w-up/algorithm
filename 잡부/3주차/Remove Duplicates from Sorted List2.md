# 정렬된 배열에서 조건에 맞춰 숫자 지우기.

배열은 정렬된 숫자가 전달된다.

같은 숫자가 최대 2번 중복되도록 재정렬을 한다.

재정렬된 배열의 길이를 return 한다.

삭제되어 불필요한 배열의 값은 아무값이나 채워도 무관하다.

```Java
    if (nums.length <= 2) {
        return nums.length;
    }

    Map<Integer, Integer> numCounter = new HashMap<>();

    for (var n : nums) {
        var count = numCounter.getOrDefault(n, -1);
        count++;
        if (count < 2) {
            numCounter.put(n, count);
        }
    }

    int idx = 0;
    for (var k : numCounter.keySet().stream().sorted().toList()) {
        for (var j=0; j<=numCounter.get(k); ++j) {
            nums[idx] = k;
            idx++;
        }
    }

    return idx
```

# 접근법

주어진 배열에서 나타난 숫자의 개수를 체크한다.

단, 2개 이상일 경우에는 체크하지 않는다.

체크가 끝났다면, 배열에 덮어쓴 후 사용된 길이를 return 한다.