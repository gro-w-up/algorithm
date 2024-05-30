## 문제 정의

1. 이미 정렬된 배열에서 확인하는 문제임
2. 중복되는 문자열은 2개까지로 고정되며, 1 개 이하라면 넘겨야 함

## 예상되는 문제 사항

1. 감으로는 O(n) 이 가능해 보이며, 다른 리스트를 할당하지 말라는 조건이 있음
2. 코틀린은 매개변수의 수정이 불가능하댔는데…?
3. 시간 제한이 걸릴 가능성이 마찬가지로 높아 보임

## 1차 풀이

```kotlin
class Solution {
    fun removeDuplicates(nums: IntArray): Int {
        var twiceFlag = false
        var deleteCount = 0
        for (i in 0..<nums.size - 1) {
            var ii = i - deleteCount
            if (nums[ii] == nums[ii + 1]) {
                if (twiceFlag) {
                    for (j in ii + 1..<nums.size - 1) {
                        nums[j] = nums[j + 1]
                    }
                    deleteCount++
                } else {
                    twiceFlag = true
                }
            } else {
                twiceFlag = false
            }
        }
        return nums.size - deleteCount
    }
}
```

풀이 자체는 특별한 것이 없음

![스크린샷 2024-05-27 004747.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/213ef69f-0d1e-4d21-9468-12fe04ca65b6/371e6a3b-ec2a-4209-8a49-273ae2d6c4bd/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2024-05-27_004747.png)

문제는 한 번에 풀렸다는 점임

아마 시간 제한을 염두에 두지 않았거나 내가 잘한 게 있을 것 같은데

## 풀이 복기

1. 탐색 자체는 n 번만에 끝나긴 함
2. 배열 수정 시 최대 n 번의 반복이 곱해져 결국 Big O 는 n^n 의 풀이가 맞음
3. 결국 시간 제한을 염두에 둔 문제가 아니어서 발생한 통과로 보임
