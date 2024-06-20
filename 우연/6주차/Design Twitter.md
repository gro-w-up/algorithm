[Design Twitter - LeetCode](https://leetcode.com/problems/design-twitter/description/)

### 문제 설명

트위터의 단순화된 버전을 설계하세요. 사용자는 트윗을 게시할 수 있고, 다른 사용자를 팔로우/언팔로우할 수 있으며, 사용자의 뉴스 피드에서 가장 최근의 10개 트윗을 볼 수 있습니다.

`Twitter` 클래스를 구현하세요:

- `Twitter()`: 트위터 객체를 초기화합니다.
- `void postTweet(int userId, int tweetId)`: 사용자가 트윗 ID `tweetId`로 새로운 트윗을 작성합니다. 이 함수가 호출될 때마다 고유한 트윗 ID가 제공됩니다.
- `List<Integer> getNewsFeed(int userId)`: 사용자의 뉴스 피드에서 가장 최근의 10개 트윗 ID를 가져옵니다. 뉴스 피드의 각 항목은 사용자가 팔로우한 사용자 또는 사용자가 직접 게시한 트윗이어야 합니다. 트윗은 가장 최근 순서대로 정렬되어야 합니다.
- `void follow(int followerId, int followeeId)`: 사용자 `followerId`가 사용자 `followeeId`를 팔로우합니다.
- `void unfollow(int followerId, int followeeId)`: 사용자 `followerId`가 사용자 `followeeId`를 언팔로우합니다.

### 예제 1:

- 입력
    
    ``` text
    ["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
    [[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
    ```
    
- 출력
    
    ``` text
    [null, null, [5], null, null, [6, 5], null, [5]]
    ```
    
- 설명
    
    ``` text
    Twitter twitter = new Twitter();
    twitter.postTweet(1, 5); // 사용자 1이 새로운 트윗 (id = 5)을 게시합니다.
    twitter.getNewsFeed(1);  // 사용자 1의 뉴스 피드는 트윗 ID [5]를 반환해야 합니다. [5]
    twitter.follow(1, 2);    // 사용자 1이 사용자 2를 팔로우합니다.
    twitter.postTweet(2, 6); // 사용자 2가 새로운 트윗 (id = 6)을 게시합니다.
    twitter.getNewsFeed(1);  // 사용자 1의 뉴스 피드는 트윗 ID [6, 5]를 반환해야 합니다. [6, 5]
    twitter.unfollow(1, 2);  // 사용자 1이 사용자 2를 언팔로우합니다.
    twitter.getNewsFeed(1);  // 사용자 1의 뉴스 피드는 트윗 ID [5]를 반환해야 합니다. [5]
    ```
    
### 제약 사항:

- `userId`, `followerId`, `followeeId`는 1 이상 500 이하입니다.
- `tweetId`는 0 이상 104 이하입니다.
- 모든 트윗은 고유한 ID를 가집니다.
- `postTweet`, `getNewsFeed`, `follow`, `unfollow`에 대해 최대 30,000번의 호출이 이루어집니다.

### 풀이 코드

```java
public class Twitter {

    // constant
    private static final int MAX_TWEETS_COUNT = 10;

    // fields
    private Map<Integer, Set<Integer>> followees; // 유저, Set<유저>
    private Map<Integer, List<Tweet>> tweets; // 유저, List<트윗>
    
    // constructor
    public Twitter() {
        followees = new HashMap<>();
        tweets = new HashMap<>();
    }
    
    // Method

    /**
     * 트윗 등록
     * @param userId
     * @param tweetId
     */
    public void postTweet(int userId, int tweetId) {
        // 존재하지 않는 사용자인 경우 사용자 정보 추가
        if (NotExistUser(userId)) {
            tweets.put(userId, new ArrayList<>());
        }
        // 사용자 정보에 트윗 아이디 추가
        addTweet(userId, tweetId);
    }

    /**
     * 뉴스 피드 조회
     * @param userId
     * @return
     */
    public List<Integer> getNewsFeed(int userId) {
        // 시간기준 내림차순 정렬
        PriorityQueue<Tweet> pq = getTweetOrderByTimeDesc();
        // 팔로우 정보 조회 및 본인 정보 추가
        Set<Integer> users = getFolloweeByUserIdAndAddSelf(userId);
        // 사용자별 트윗 큐(리스트 트윗) 생성
        addUsersTweetsToQueue(users, pq);
        // 트윗 큐에서 상위 N개 뽑기
        return getTopTweetsFromQueue(pq, MAX_TWEETS_COUNT);
    }

    /**
     * 팔로우
     * @param followerId
     * @param followeeId
     */
    public void follow(int followerId, int followeeId) {
        // 자신을 팔로우할 수 없음
        if (isMe(followerId, followeeId)) return;

        // 팔로우 하고 있지 않으면 팔로우 함
        if (!isFollowee(followerId)) {
            followees.put(followerId, new HashSet<>());
        }

        // 팔로워 아이디에 팔로우
        followees.get(followerId).add(followeeId);
    }

    /**
     * 언팔로우
     * @param followerId
     * @param followeeId
     */
    public void unfollow(int followerId, int followeeId) {
        if (isFollowee(followerId)) {
            followees.get(followerId).remove(followeeId);
        }
    }

    private void addTweet(int userId, int tweetId) {
        tweets.get(userId).add(new Tweet(tweetId, LocalDateTime.now()));
    }

    private boolean NotExistUser(int userId) {
        return !tweets.containsKey(userId);
    }

    private static PriorityQueue<Tweet> getTweetOrderByTimeDesc() {
        return new PriorityQueue<>((a, b) ->
                b.time.compareTo(a.time)
        );
    }

    private boolean isFollowee(int followerId) {
        return followees.containsKey(followerId);
    }

    private static boolean isMe(int followerId, int followeeId) {
        return followerId == followeeId;
    }

    private List<Integer> getTopTweetsFromQueue(PriorityQueue<Tweet> pq, int maxTweets) {
        List<Integer> newsFeed = new ArrayList<>();
        int count = 0;
        while (!pq.isEmpty() && count < maxTweets) {
            newsFeed.add(pq.poll().getId());
            count++;
        }
        return newsFeed;
    }

    private void addUsersTweetsToQueue(Set<Integer> users, PriorityQueue<Tweet> pq) {
        for (int user : users) {
            List<Tweet> userTweets = tweets.getOrDefault(user, new ArrayList<>());
            pq.addAll(userTweets);
        }
    }

    private Set<Integer> getFolloweeByUserIdAndAddSelf(int userId) {
        // 팔로우들 조회
        Set<Integer> users = followees.getOrDefault(userId, new HashSet<>());
        // 본인 추가
        users.add(userId);
        return users;
    }

    public static void main(String[] args) {
        Twitter twitter = new Twitter();

        twitter.postTweet(1, 5);
        System.out.println(twitter.getNewsFeed(1));  // [5]

        twitter.follow(1, 2);
        twitter.postTweet(2, 6);
        System.out.println(twitter.getNewsFeed(1));  // [6, 5]

        twitter.unfollow(1, 2);
        System.out.println(twitter.getNewsFeed(1));  // [5]
    }

    // inner class
    private static class Tweet {

        // fields
        int id;
        LocalDateTime time;

        // constructor
        Tweet(int id, LocalDateTime time) {
            this.id = id;
            this.time = time;
        }

        // Method
        public int getId() {
            return id;
        }

        public LocalDateTime getTime() {
            return time;
        }
    }
}
```
