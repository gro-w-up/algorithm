## 문제 정의

1. matrix 의 크기 n 은 정사각형 크기이다.
2. n*n 보다 좋은 방법을 찾아야 한다.
3. matrix 는 row, column 별로 정렬되어 있다.
4. matrix의 형태를 보니 바로 퀵, 삽입 정렬이 생각난다.
5. 그래도 혹시 모르니 바로 인덱스로 변환해서 돌려 보자

## 1차 코드 작성

```jsx
class Solution {
    fun kthSmallest(matrix: Array<IntArray>, k: Int): Int {
        var inputIndex = k - 1
        var m = matrix[0].size
        var leftIndex  = (inputIndex / m)
        var rightIndex = (inputIndex % m)
        return matrix[leftIndex][rightIndex]
    }
}
```

![[[1,2],[1,3]] 에서 아웃이 1이 나와야 하나 2로 나옴](https://prod-files-secure.s3.us-west-2.amazonaws.com/213ef69f-0d1e-4d21-9468-12fe04ca65b6/a1424044-855b-44e9-b924-b2a0004135ed/Untitled.png)

[[1,2],[1,3]] 에서 아웃이 1이 나와야 하나 2로 나옴

## 풀이 복기

- 당연히 날먹은 안된다

## 풀이 논리

1. 먼저 입력받은 nxn matrix 의 정렬을 진행한다.
    1. 당연히 nlogn 의 효율을 보이는 퀵 정렬이 좋을 것이다.
        1. 퀵 정렬은 자바에서 지원하는 기능이 있었던 것 같다.
        2. 다만 같은 column 내에서는 정렬되어 있다는 점을 고려하면 삽입 정렬을 조금 수정하여 사용해도 그리 나쁘지 않을 것으로 예상된다.
        3. 이진 탐색을 포함하여 사용 시예상 시간 복잡도는 nlogn
2. 이후 정렬한 array 에서 바로 리턴한다.

## 1차 코드 작성

```jsx
class Solution {
    fun kthSmallest(matrix: Array<IntArray>, k: Int): Int {
        return convert2SortedFlatList(matrix)[k-1]
    }
    fun convert2SortedFlatList(matrix: Array<IntArray>): IntArray {
        val flatArray = matrix.flatMap { it.toList() }
        flatArray.sort()
        return flatArray
    }
}
```

![런타임 305ms, 메모리44.19MB](https://prod-files-secure.s3.us-west-2.amazonaws.com/213ef69f-0d1e-4d21-9468-12fe04ca65b6/9b717d7a-e83b-4143-a08a-45f21555e6c7/Untitled.png)

런타임 305ms, 메모리44.19MB

## 풀이 복기

1. flatten 메소드가 왜 에러가 나는지는 확인이 필요할 것 같다.
2. 속도를 개선하려면 .flatmap().toIntArray().sort() 의 수정이 이루어져야 할 것 같다.
    1. 메모리 사용량이 내가 매우 적은 편임을 감안하면 다른 사람들은 직접 해쉬맵이나 변환을 이용한 것으로 보인다.
3. 내가 생각한 것보다 재미없게 끝났다..

4. 조금씩 수정해서 계속 돌려보는데, 같은 코드로도 속도가 달라진다..
