# Combination Sum

```Kotlin
class Solution {
    val memo = HashMap<List<Int>, Boolean>();

    private fun backtracking(target: Int, candidates: IntArray, tempStore: MutableList<Int>, result: MutableList<List<Int>>) {
        if (tempStore.sum() == target) {
            var copy = tempStore.toList()
            copy = copy.sorted()

            if (!memo.containsKey(copy)) {
                memo[copy] = true
                result.add(tempStore.toList())
                return
            }

            return
        } else if (tempStore.sum() > target) {
            return
        }

        for (i in candidates.indices) {
            tempStore.add(candidates[i])
            backtracking(target, candidates, tempStore, result)
            tempStore.removeLast()
        }
    }

    fun combinationSum(candidates: IntArray, target: Int): List<List<Int>> {
        val result = mutableListOf<List<Int>>()
        val tempStore = mutableListOf<Int>()
        backtracking( target, candidates, tempStore, result)
        return result
    }
}
```