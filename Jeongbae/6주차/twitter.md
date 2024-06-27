```java
public class Twitter {

    private List<Integer[]> posts;

    // 1. key 는 사용자, value 는 사용자가 follow 하는 사람들 입니다.
    private Map<Integer, Set<Integer>> map;
    
    public Twitter() {
        this.posts = new ArrayList<>();
        this.map = new HashMap<>();
    }

    // 2. postTweet 은 쉽습니다.
    public void postTweet(int userId, int tweetId) {
        this.posts.add(new Integer[]{userId, tweetId});
    }

    // 5. 여기가 조금 복잡했습니다.
    public List<Integer> getNewsFeed(int userId) {
        // 5-1. 먼저 return 할 뉴스피드의 리스트를 정의하고
        List<Integer> newsFeed = new ArrayList<>();

        // 5-2. 여기서 getOrDefault 를 안나면 NPE 가 터집니다.
        // 5-3. getNewsFeed 를 할 때 정배(userId = 1)이 호출했다고 하면 정배가 follow 한 사람들의 userId가(Set) 필요합니다.
        // 그런데 오답이 발생했던 이유는 정배가 직접 작성한 포스트도 가져와야하기 Set 에 자기 자신을 추가해줘야합니다.
        Set<Integer> follower = map.getOrDefault(userId, new HashSet<>());
        // user id 에 해당하는 Set이 나옴 (팔로우 하는 사람들)
        follower.add(userId);

        int cnt = 0;
        for (int i = posts.size() - 1; i >= 0 && cnt < 10; i--) {
            Integer[] post = posts.get(i);
            // 모든 post 를 순회하면서, 포스트를 남긴 사람이 Set에 들어있는지 확인하면 됩니다.
            // * 유저가 팔로우하는 사람들의 글이 나와야 함 + 내가 직접 쓴 글이 나와야함
            if (follower.contains(post[0])) {
                newsFeed.add(post[1]);
                cnt++;
            }
        }
        return newsFeed;
    }

    // 3. follow 도 쉽습니다. follower 는 하는사람, followee 는 당하는 사람입니다.
    public void follow(int followerId, int followeeId) {
        map.putIfAbsent(followerId, new HashSet<>());
        map.get(followerId).add(followeeId);
    }

    // 4. 언팔도 쉽습니다. map 에서 사용자의 id로 Set 을 가져온 뒤, 삭제할 id를 입력하면됩니다.
    public void unfollow(int followerId, int followeeId) {
        if (map.containsKey(followerId)) {
            map.get(followerId).remove(followeeId);
        }
    }


}

```