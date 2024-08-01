## 문제 정의

- (()), (()()) 는 올바른 괄호 문자열이다.
- 괄호 문자열은 결국 ( 와 ) 가 조건에 부합해야 하며, 이는 생각을 어렵게 만든다
    - n*n 이 사실 가장 먼저 드는 생각임
- 문제는 백준 특성상 시간 초과가 분명히 발생할 것이고 문제의 풀이는 그냥 전부 탐색하면 되니 시간을 줄이는 게 가장 중요한 관건이다.
    - nlogn 이 직관상 가장 부합할 것으로 보이나 생각이 나지 않음
- 가장 긴 팰린드롬과도 유사한 느낌이 있으나 세부 탐색 방식이 다르다.
    - 가장 긴 팰린드롬과 이 문제의 가장 큰 차이점은
    (())()
    이 가능하다는 것임
        - 이말인 즉 2n 만에 탐색하는 방식이 불가능하며, 다른 방식을 찾아보아야 함

## 풀이  논리

<aside>
💡 먼저 올바르지 않은 괄호 문자열을 열린 문자열로, 올바른 괄호 문자열을 닫힌 문자열로 칭하겠다.

</aside>

1. 먼저 문자열을 1차로 하나씩 찾으며 닫힌 문자열인지 확인한다.
    1. 확인하는 방법은 좌측에서 우측으로 1씩 움직이며 ( 일때 열고 ) 일때 닫으며 열린 후 닫혔는지 확인하는 것이다.
        1. 이는 빗물 트래핑과 유사한 방식이다.
            1. 열릴 때마다 높이를 1 높이고, 닫힐 때마다 높이를 1 낮춘다고 가정하면 완전히 동일한 접근도 가능하다.
            2. 쓰고 지웠는데
                1. 심지어 방법도 n*n 으로 만들었었는데..
2. 확인되었다면 그 길이를 담는 리스트를 만든다.
    1. ex: )4)(6(
    2. 여기에서 연속된 일부를 빼는 것이 관건이며, 어차피 (로 시작하여 열린 문자열은 ) 로 닫힐 수 없으니 배제하면 된다.
3. 4)(6
같이 더했을 때 가장 큰 숫자를 가진 닫힌 문자열들을 찾으면 끝난다.
- 위와 같이 반복 횟수를 줄이는 관건은 1번의 역할이 가장 크며, 다음은 N 의 반복만 시행하면 끝난다.
    - 이마저도 첫번째 반복 안에서 실행할 수 있음
        - 닫힌 문자열의 길이를 확정했을 때마다 1회씩
- 2-b 의 조건을 생각해보면 열린 문자열을 굳이 문자열에 포함할 필요 없이 닫힌 문자열의 길이만 리스트로 넣는 게 조금 더 빠를 것으로 보임
    - 문자를 추가해서 만들라면 포함해야겠지만 이는 필요 없는 가정이다.

## 1차 코드 작성

```jsx
import java.io.BufferedReader
import java.io.InputStreamReader

fun main() {
    // var br = BufferedReader(InputStreamReader(System.`in`))
    // var bw = System.out.bufferedWriter()
    // var input: String = br.readLine()

    var input = ")(()(())))(()()(()()("

    var ret = checkIsClosedBracket(input)

//    bw.write("$ret\n")
//    bw.flush()
}

fun checkIsClosedBracket(input: String): Int {
    var opened = 0
    var closed = 0
    var cutFlag = true
    var listOfClosed = mutableListOf<Int>()

    for (i in input.indices) {
        if (input[i] == ')') {
            if (opened > 0) {
                closed++

                if(cutFlag) {
                    listOfClosed.add(closed + )
                    cutFlag = false
                } else {
                    listOfClosed[listOfClosed.size - 1] = closed * 2
                }

            } else {
                opened = 0
                closed = 0
                cutFlag = true
            }

        } else {
            opened++
        }
        println("i: $i, text: ${input[i]}, opened: $opened, closed: $closed, listOfClosed: $listOfClosed")
    }

    return listOfClosed.size
}
```

to be continued….
