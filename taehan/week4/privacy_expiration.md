## 문제 정의

1. expiration 에서 terms 타입을 매칭하여 오늘보다 작은 날짜를 찾아 리턴해야 함
2. 모든 달은 28일까지가 끝임
    1. 굳이 날짜 계산할 필요가 없음
3. terms 는 정렬되어 있지 않음

## 예상되는 문제 사항

1. 코틀린 숙련도 이슈로 terms 의 타입 매칭이 못생겨질 가능성 높음
    1. 코틀린은 JSON 을 직접 다루는 게 용이하지 않음
        1. 자바스크립트는 되는데..
2. 시간 초과 발생 가능성 있음
    1. 다만 O(2n) 으로 풀 수 있을 게 뻔한 상황이어서 시간 초과가 발생한다면 이는 type 매칭 중 발생한 타임 로스일 가능성이 높음

## 1차 코드 작성

```kotlin
class Solution {
    fun solution(today: String, terms: Array<String>, privacies: Array<String>): IntArray {
        var termsMap = mutableMapOf<String, Int>()
        for (term in terms) {
            val (type, month) = term.split(" ")
            termsMap[type] = month.toInt()
        }

        var answer = intArrayOf()
        var i = 0
        for (privacy in privacies) {
            i++
            val (date, type) = privacy.split(" ")
            if (isExpired(today, date, termsMap[type]!!)) {
                answer += i
            }
        }

        return answer
    }

    fun isExpired(today: String, date: String, term: Int): Boolean {
        val (todayYear, todayMonth, todayDay) = splitDate(today)
        var (dateYear, dateMonth, dateDay) = splitDate(date)

        dateMonth += term
        if (dateMonth > 12) {
            dateYear += 1
            dateMonth -= 12
        }
        if (todayYear > dateYear) return true
        if (todayYear == dateYear && todayMonth > dateMonth) return true
        if (todayYear == dateYear && todayMonth == dateMonth && todayDay >= dateDay) return true

        return false

    }

    fun splitDate(date: String): Triple<Int, Int, Int> {
        val (year, month, day) = date.split(".")
        return Triple(year.toInt(), month.toInt(), day.toInt())
    }

}
```

문제 틀림

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/213ef69f-0d1e-4d21-9468-12fe04ca65b6/3e816144-a71a-409b-8ed4-86ae2df587c8/Untitled.png)

## 풀이 복기

1. 개월수가 12를 넘어갈 가능성을 고려하지 않았음

## 2차 코드 작성

```kotlin

class Solution {
    fun solution(today: String, terms: Array<String>, privacies: Array<String>): IntArray {
        println("today: $today")
        println("terms: ${terms.joinToString(", ")}")
        println("privacies: ${privacies.joinToString(", ")}")

        var termsMap = mutableMapOf<String, Int>()
        for (term in terms) {
            val (type, month) = term.split(" ")
            println("type: $type, month: $month")
            termsMap[type] = month.toInt()
        }

        var answer = intArrayOf()
        var i = 0
        for (privacy in privacies) {
            i++
            val (date, type) = privacy.split(" ")
            println("date: $date, type: $type, term: ${termsMap[type]}")
            if (isExpired(today, date, termsMap[type]!!)) {
                println("expired : $i")
                answer += i
            }
        }

        return answer
    }

    fun isExpired(today: String, date: String, term: Int): Boolean {
        val (todayYear, todayMonth, todayDay) = splitDate(today)
        var (dateYear, dateMonth, dateDay) = splitDate(date)

        dateMonth += term
        if (dateMonth > 12) {
            if(dateMonth % 12 == 0) {
                dateYear += dateMonth / 12 - 1
                dateMonth = 12
            } else {
                dateYear += dateMonth / 12
                dateMonth %= 12
            }
        }
        if (todayYear > dateYear) return true
        if (todayYear == dateYear && todayMonth > dateMonth) return true
        if (todayYear == dateYear && todayMonth == dateMonth && todayDay >= dateDay) return true

        return false

    }

    fun splitDate(date: String): Triple<Int, Int, Int> {
        val (year, month, day) = date.split(".")
        return Triple(year.toInt(), month.toInt(), day.toInt())
    }

}
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/213ef69f-0d1e-4d21-9468-12fe04ca65b6/d937ccdd-c9f0-4821-8d49-d8a957305155/Untitled.png)

## 풀이 복기

1. 시간 초과가 없어 저번 주와 마찬가지로 상대적으로 쉬웠음
2. mutableMap 이 javascript 의 object 와 유사한 동작을 해서 풀이가 더러워지지 않고 끝났음
3. 당연히 만료가 12월이 넘는다는 것이 문제 설명에 나와 있었으나 유의깊게 보지 않고 넘어감
