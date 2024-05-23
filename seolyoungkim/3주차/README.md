[Remove Duplicates from Sorted Array II - LeetCode](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/description/?envType=study-plan-v2&envId=top-interview-150)

```java
public int removeDuplicates(int[] nums) {
    int length = nums.length;
    int number = nums[0];
    int count = 1;
    for (int i = 1; i < length; i++) {
        if (number == nums[i]) {
            count++;
            if (count >= 3) {
                nums[i] = Integer.MAX_VALUE;
            }
        } else {
            number = nums[i];
            count = 1;
        }
    }

    Arrays.sort(nums);
    return (int) Arrays.stream(nums)
            .filter(num -> num != Integer.MAX_VALUE)
            .count();
}
```
