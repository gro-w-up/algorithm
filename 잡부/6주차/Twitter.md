# Twitter  모델

1. UserID를 통해서 TwitterID를 발행할 수 있습니다.
2. UserID는 Follower / Followeed 관계가 될 수 있습니다.
3. 가장 최근에 발행된 순서로 출력이 되어야합니다.

## 제약 조건
1. 1 <= userId, followerId, followeeId <= 500
2. 0 <= tweetId <= 104
3. All the tweets have unique IDs.
4. At most 3 * 10^4 calls will be made to postTweet, getNewsFeed, follow, and unfollow.

상당히 많은 호출량이 발생한다.

### 1차 버전
```Java
class Twitter {
    LinkedList<Tweet> tweets = new LinkedList<>();
    HashMap<Integer, List<Integer>> followers = new HashMap<>();

    class Tweet {
        int userId;
        int tweetId;

        public Tweet(int userId, int tweetId) {
            this.userId = userId;
            this.tweetId = tweetId;
        }
    }

    public Twitter() {
    }

    public void postTweet(int userId, int tweetId) {
        tweets.addFirst(new Tweet(userId, tweetId));
    }

    public List<Integer> getNewsFeed(int userId) {
        var followerIds = followers.getOrDefault(userId, new ArrayList<>());
        followerIds.add(userId);
        var result = tweets.stream()
            .filter(tweet -> followerIds.contains(tweet.userId))
            .map(tweet -> tweet.tweetId)
            .toList();

        return result.subList(0, Math.min(10, result.size()));
    }

    public void follow(int followerId, int followeeId) {
        if (!followers.containsKey(followerId)) {
            followers.put(followerId, new ArrayList<>());
        }

        followers.get(followerId).add(followeeId);
    }

    public void unfollow(int followerId, int followeeId) {
        if (followers.containsKey(followerId)) {
            followers.get(followerId).removeIf(id -> id == followeeId);
        }
    }
}
```

### 속도 개선 버전
```Java
class Twitter {
    ListNode tweets;
    HashMap<Integer, ListNode> followers = new HashMap<>();

    class Tweet {
        int userId;
        int tweetId;

        public Tweet(int userId, int tweetId) {
            this.userId = userId;
            this.tweetId = tweetId;
        }
    }

    class ListNode {
        int value;
        Tweet tweet;
        ListNode next;

        public ListNode() {
            this.tweet = null;
            this.next = null;
        }

        public ListNode(Tweet tweet) {
            this.tweet = tweet;
            this.next = null;
        }

        public ListNode(int value) {
            this.value = value;
            this.next = null;
        }

    }

    public Twitter() {
        tweets = new ListNode();
    }

    public void postTweet(int userId, int tweetId) {
        var node = new ListNode(new Tweet(userId, tweetId));
        node.next = tweets.next;
        tweets.next = node;
    }

    public List<Integer> getNewsFeed(int userId) {
        var followerIds = new ArrayList<Integer>();
        followerIds.add(userId);
        if (followers.containsKey(userId)) {
            var head = followers.get(userId).next;
            while (head != null) {
                followerIds.add(head.value);
                head = head.next;
            }
        }

        var idx = 0;
        var head = tweets.next;

        List<Integer> result = new ArrayList<>();
        while (idx < 10 && head != null) {
            if (followerIds.contains(head.tweet.userId)) {
                result.add(head.tweet.tweetId);
                idx++;
            }

            head = head.next;
        }

        return result;
    }

    public void follow(int followerId, int followeeId) {
        if (!followers.containsKey(followerId)) {
            followers.put(followerId, new ListNode(followeeId));
        }

        var head = followers.get(followerId);
        var node = new ListNode(followeeId);
        node.next = head.next;
        head.next = node;
    }

    public void unfollow(int followerId, int followeeId) {
        if (followers.containsKey(followerId)) {
            var head = followers.get(followerId);
            while (head.next != null) {
                if (head.next.value == followeeId) {
                    head.next = head.next.next;
                    break;
                }

                head = head.next;
            }
        }
    }
}
```

# 왜 ListNode인가?
ArrayList의 Add 함수
 ```
     /**
     * Default initial capacity.
     */
    private static final int DEFAULT_CAPACITY = 10;


     /**
     * The array buffer into which the elements of the ArrayList are stored.
     * The capacity of the ArrayList is the length of this array buffer. Any
     * empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
     * will be expanded to DEFAULT_CAPACITY when the first element is added.
     */
    transient Object[] elementData; // non-private to simplify nested class access


     private void add(E e, Object[] elementData, int s) {
        if (s == elementData.length)
            elementData = grow();
        elementData[s] = e;
        size = s + 1;
    }
    
    private Object[] grow(int minCapacity) {
        int oldCapacity = elementData.length;
        if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            int newCapacity = ArraysSupport.newLength(oldCapacity,
                    minCapacity - oldCapacity, /* minimum growth */
                    oldCapacity >> 1           /* preferred growth */);
            return elementData = Arrays.copyOf(elementData, newCapacity);
        } else {
            return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
        }
    }
 ```

Object 일정 크기의 배열로 선언해두고, 크기를 넘어가면 확장을 이뤄지게 된다.

이때, 새로운 배열을 만들고 복사가 이뤄지기때문에 데이터 양이 크다면 그만큼 소비되는 시간도 무시하지 못하게 된다.

### LinkedList는 대안이 될 수 없는가?
 ```Java
 public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
    transient int size = 0;

    /**
     * Pointer to first node.
     */
    transient Node<E> first;

    /**
     * Pointer to last node.
     */
    transient Node<E> last;

 ```

LinkedList를 구현체를 보면, Node를 활용하여 구성이 되어있다.

그러나, 아쉽게도 탐색을 하기 위해서 Node 자체를 가져와야하는데 그러하지 못하기에, LinkedList에서 제공하고 있는 indexOf 를 활용해야한다.

indexOf를 살펴보면 다음과 같다.
 
```Java
    public int indexOf(Object o) {
        int index = 0;
        if (o == null) {
            for (Node<E> x = first; x != null; x = x.next) {
                if (x.item == null)
                    return index;
                index++;
            }
        } else {
            for (Node<E> x = first; x != null; x = x.next) {
                if (o.equals(x.item))
                    return index;
                index++;
            }
        }
        return -1;
    }
```

1 -> 2 -> 3 -> 4번째로 순회를하게 되면, 2중 for문의 효과를 가져오게 된다.