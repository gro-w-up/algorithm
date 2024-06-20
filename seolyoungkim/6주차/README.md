```java
/**
 * 사용자가 트윗을 게시할 수 있음
 * 다른 사용자를 팔로우/언팔로우 할 수 있음
 * 사용자의 뉴스피드에서 가장 최근 트윗 10개를 볼 수 있음
 */
class Twitter {
    /**
     * key = userId, value = 팔로우한 사람들의 userId
     */
    private final Map<Integer, Set<Integer>> followMap = new HashMap<>();

    /**
     * key = userId, value = userId가 작성한 tweets
     */
    private final Map<Integer, List<Tweet>> tweetMap = new HashMap<>();

    private int order;

    public Twitter() {

    }

    static class Tweet {
        private final int id;
        private final int order;

        Tweet(int id, int order) {
            this.id = id;
            this.order = order;
        }

        @Override
        public String toString() {
            return "Tweet{" +
                    "id=" + id +
                    ", autoIncrement=" + order +
                    '}';
        }
    }

    /**
     * userId의 tweetId로 새 트윗 작성
     * 고유한 tweetId로 호출
     */
    public void postTweet(int userId, int tweetId) {
        tweetMap.computeIfAbsent(userId, key -> new ArrayList<>())
                .add(new Tweet(tweetId, order++));
    }

    /**
     * userId 뉴스피드에서 가장 최근 트윗ID 10개를 검색
     * 뉴스피드 각 항목은 사용자가 팔로우한 사용자 또는 사용자 자신이 게시한 것임 (팔로워 or 내꺼)
     * 최신순 정렬
     */
    public List<Integer> getNewsFeed(int userId) {
        List<Tweet> myFeeds = tweetMap.getOrDefault(userId, Collections.emptyList());

        List<Tweet> followeeFeeds = followMap.getOrDefault(userId, Collections.emptySet())
                .stream()
                .flatMap(followee -> tweetMap.getOrDefault(followee, Collections.emptyList()).stream())
                .toList();

        return Stream.concat(myFeeds.stream(), followeeFeeds.stream())
                .sorted(Comparator.comparing(tweet -> tweet.order, Comparator.reverseOrder()))
                .map(tweet -> tweet.id)
                .limit(10)
                .toList();
    }

    /**
     * followerId가 followeeId 사용자를 팔로우
     */
    public void follow(int followerId, int followeeId) {
        followMap.computeIfAbsent(followerId, key -> new HashSet<>())
                .add(followeeId);
    }

    /**
     * followerId가 followeeId를 언팔로우
     */
    public void unfollow(int followerId, int followeeId) {
        followMap.getOrDefault(followerId, Collections.emptySet())
                .remove(followeeId);
    }
}
```
