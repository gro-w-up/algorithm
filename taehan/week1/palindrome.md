### 1차 풀이 접근

1. 팰린드롬 문자열은 코틀린에서 *.reversed() 와 비교하여 확인 가능하다.
    1. 연산의 속도는 문제가 될 수 있어 보이나, 코틀린도, 알고리즘 풀이도 처음이니 일단 패스
2. 유사 회문 까지도 검사가 필요하므로, 각 문자열을 1 회 씩 제하며 반복문을 처리한다.
    1. 회문 및 유사 회문이 확인된다면 반복문을 나가야 유리하다.
3. 최대 문자열의 개수 및 문자열 길이가 주어져 있으며 그 길이가 큰 편이다.
    1. c/t 문제 발생 가능성이 있으므로 상대적으로 신중한 접근이 필요하다.

### 1차 코드 작성

```jsx
import java.util.*

fun main(args: Array<String>) = with(Scanner(System.`in`)) {
    var countOfInput: Int = nextInt()

    for (i in 0..<countOfInput) {
        var input: String = next()

        if (checkIsPalindrome(input)) {
            println(0)
            continue
        }
        for (j in input.indices) {
            if (checkIsPalindrome(input.substring(0,j) + input.substring(j+1))) {
                println(1)
                break
                continue
            }
        }
        println(2)
    }
}

fun checkIsPalindrome(input: String): Boolean {
    return input == input.reversed()
}

```

<aside>
💡 1차 코드로 예제 실행 시 입력 및 결과

```jsx
7
abba
summuus
xabba
xabbay
comcom
comwwmoc
comwwtmoc
0
1
2
1
2
2
2
0
1
2
```

리턴이 10개인 것으로 보아 

```jsx
break
continue
```

문이 원하는 동작대로 움직이지 않은 것으로 예상되어 해당 부분 수정

</aside>

### 2차 코드 작성

```jsx
import java.util.*

fun main(args: Array<String>) = with(Scanner(System.`in`)) {
    var countOfInput: Int = nextInt()

    for (i in 0..<countOfInput) {
        var input: String = next()

        if (checkIsPalindrome(input)) {
            println(0)
            continue
        }
        var outFlag: Boolean = false
        for (j in input.indices) {
            outFlag = false
            if (checkIsPalindrome(input.substring(0,j) + input.substring(j+1))) {
                println(1)
                outFlag = true
                break

            }
        }
        if (outFlag) {
            continue
        }
        println(2)
    }
}

fun checkIsPalindrome(input: String): Boolean {
    return input == input.reversed()
}
```

<aside>
💡 예제 코드 정상 동작 확인

</aside>

### 문제 제출

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/213ef69f-0d1e-4d21-9468-12fe04ca65b6/8bf4d849-1c4d-479a-95aa-da7ccac39309/Untitled.png)

- 시간 초과 발생
    - 예산대로 시간 초과 발생하여 추가적인 개선 방안 도출 필요
1. Scanner 함수를 BufferedReader 로 수정한다.
2. String.reversed() 메소드의 속도를 for 문을 이용한 직접 뒤집기와 비교하여 더 빠른 코드 사용

### 3차 코드 작성

```jsx
    val start = System.currentTimeMillis()
    var testStr = "ab" + "a".repeat(1000) + "b"
    for (i in 0..1000) {
        testStr = testStr.reversed()
    }
    val end = System.currentTimeMillis()
    println("Time1: ${end - start} ms")

    val start2 = System.currentTimeMillis()
    var testStr2 = "ab" + "a".repeat(1000) + "b"
    var testStr3: String = ""
    for (i in 0..1000) {
        testStr3 = ""
        for (j in testStr2.indices) {
            testStr3 += testStr2[testStr2.length - j - 1]
        }
    }
    val end2 = System.currentTimeMillis()
    println("Time2: ${end2 - start2} ms")

```

```jsx
import java.io.BufferedReader
import java.io.InputStreamReader

fun main() {
    var br = BufferedReader(InputStreamReader(System.`in`))
    var bw = System.out.bufferedWriter()
    var countOfInput: Int = br.readLine().toInt()
    
    for (i in 0..<countOfInput) {
        var input: String = br.readLine()

        if (checkIsPalindrome(input)) {
            bw.write("0\n")
            continue
        }
        for (j in input.indices) {
            if (checkIsPalindrome(input.substring(0,j) + input.substring(j+1))) {
                bw.write("1\n")
                break
                continue
            }
        }
        bw.write("2\n")
    }
    br.close()
    bw.flush()
    bw.close()

    return
}

fun checkIsPalindrome(input: String): Boolean {
    return input == input.reversed()
}

```

### 2차 문제 제출

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/213ef69f-0d1e-4d21-9468-12fe04ca65b6/e263122d-780b-44a0-8314-a1bc5a06b117/Untitled.png)

<aside>
💡 위 내용 중 더이상 문제가 될만한 내용은 
checkIsPalindrome(input.substring(0,j) + input.substring(j+1))
외에 없다고 판단.

</aside>

1. substring 메소드 대신 StringBuilder 사용 시도

### 4차 코드 작성

```jsx
import java.io.BufferedReader
import java.io.InputStreamReader

fun main() {
    var br = BufferedReader(InputStreamReader(System.`in`))
    var bw = System.out.bufferedWriter()
    var countOfInput: Int = br.readLine().toInt()

    for (i in 0..<countOfInput) {
        var input: String = br.readLine()

        if (checkIsPalindrome(input)) {
            bw.write("0\n")
            continue
        }
        for (j in input.indices) {
            if (checkIsSimilarPalindrome(input)) {
                bw.write("1\n")
                break
                continue
            }
        }
        bw.write("2\n")
    }
    br.close()
    bw.flush()
    bw.close()

    return
}

fun checkIsPalindrome(input: String): Boolean {
    return input == input.reversed()
}

fun checkIsSimilarPalindrome(input: String): Boolean {
    var sb = StringBuilder(input)
    for (i in 0..<sb.length / 2) {
        if (sb[i] != sb[sb.length - 1 - i]) {
            sb.deleteCharAt(i)
            if (sb.toString() == sb.reversed().toString()) {
                return true
            }
        }
    }
    return false
}
```

- 참고자료
    
    책 코틀린 기초 내용 일부만 그 때 그 때
    
    intelliJ 코드 자동 수정 기능
    
    [[코틀린] 코틀린에서의 입력값 처리 방법 feat. 알고리즘 문제풀이 꿀팁](https://velog.io/@blucky8649/코틀린-코틀린에서의-입력값-처리-방법-feat.-알고리즘-문제풀이-꿀팁)
    
    [코틀린으로 알고리즘 풀 때 기본 팁](https://wonnyhouse.tistory.com/246)
    
    [[코틀린, kotlin / 기초] StringBuilder에 대해 알아보자(관련함수, 사용예시)](https://www.writingaday.com/2023/12/kotlin-stringbuilder-basics.html?m=1)
