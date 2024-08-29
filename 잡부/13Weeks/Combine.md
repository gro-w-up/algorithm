# Combine

```Kotlin
class Solution {
    fun dfs(elements: List<Int>, start: Int, depth: Int, result: MutableList<Int>, results: MutableList<List<Int>>) {
        if (result.size == depth) {
            results.add(result.toList())
            return
        }

        for (i in start until elements.size) {
            result.add(elements[i])
            dfs(elements, i + 1, depth, result, results)
            result.removeAt(result.size - 1)
        }
    }

    fun combine(n: Int, k: Int): List<List<Int>> {
        val elements = mutableListOf<Int>()
        for (i in 1..n) {
            elements.add(i)
        }

        val results = mutableListOf<List<Int>>()
        dfs(elements, 0, k, mutableListOf(), results)
        return results;
    }
}

fun main() {
    val solution = Solution()
    println(solution.combine(4, 2))
}


```

## Solution
n 개의 요소가 담긴 List 가 필요하다. 이는 순회를 위한 데이터이다.

k 는 요소가 가질 수 있는 개수이다. 즉 Depth와 동일하게 볼 수 있다.

하나를 선택한 이후, 자신을 제외한 요소를 선택하면서 Depth를 채워나가면 된다. 