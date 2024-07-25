```Java
import java.util.Map;
import java.util.Map.Entry;

class Solution {
    
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> counter = new HashMap<>();

        for (int num : nums) {
            counter.put(num, counter.getOrDefault(num, 0) + 1);
        }

        for (Entry<Integer, Integer> entry : counter.entrySet()) {
            if (entry.getValue() > nums.length / 2) {
                return entry.getKey();
            }
        }
        
        return 0;
    }
}
```