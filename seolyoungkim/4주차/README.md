[개인정보 수집 유효기간](https://school.programmers.co.kr/learn/courses/30/lessons/150370)
```java
public int[] solution(String today, String[] terms, String[] privacies) {
    DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy.MM.dd");
    LocalDate currentDate = LocalDate.parse(today, dateTimeFormatter);

    // 약관 Map
    Map<String, Integer> termsMap = Arrays.stream(terms)
            .map(term -> term.split(" "))
            .collect(Collectors.toUnmodifiableMap(
                    term -> term[0],
                    term -> Integer.parseInt(term[1])
            ));

    List<Integer> results = new ArrayList<>();
    for (int i = 0; i < privacies.length; i++) {
        String privacy = privacies[i];
        String[] split = privacy.split(" ");
        LocalDate collectionDate = LocalDate.parse(split[0], dateTimeFormatter);
        Integer expirationPeriod = termsMap.get(split[1]);

        LocalDate expirationDate = collectionDate.plusMonths(expirationPeriod);
        if (currentDate.isEqual(expirationDate) || currentDate.isAfter(expirationDate)) {
            results.add(i + 1);
        }
    }

    return results.stream()
            .mapToInt(i -> i)
            .toArray();
}
```
