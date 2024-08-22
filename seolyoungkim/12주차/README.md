```java
/**
 * <a href="https://www.acmicpc.net/problem/5052">link</a>
 */
public class PhoneNumberList {
    public static void main(String[] args) throws IOException {
        try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out))) {
            int testCaseCount = Integer.parseInt(br.readLine());
            for (int i = 0; i < testCaseCount; i++) {
                List<String> phoneNumbers = new ArrayList<>();

                int phoneNumberCount = Integer.parseInt(br.readLine());
                for (int j = 0; j < phoneNumberCount; j++) {
                    phoneNumbers.add(br.readLine());
                }

                phoneNumbers.sort(Comparator.naturalOrder());

                if (isConsistent(phoneNumberCount, phoneNumbers)) {
                    bw.write("YES\n");
                } else {
                    bw.write("NO\n");
                }
            }
        }
    }

    private static boolean isConsistent(final int phoneNumberCount, final List<String> phoneNumbers) {
        for (int j = 0; j < phoneNumberCount - 1; j++) {
            if (phoneNumbers.get(j + 1).startsWith(phoneNumbers.get(j))) {
                return false;
            }
        }
        return true;
    }
}
```
