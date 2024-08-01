## 문제 정의

1. 트위터의 기능을 유사하게 일부 구현하는 클래스
2. 이러한 일반 기능 구현은 언제나 그렇듯이 예외처리를 얼마나 잘 고려하느냐가 관건이다
3. 릿코드 특성상 시간 제한은 없을 것으로 예상됨

## 예외사항

1. follow/unfollow 를 본인을 할 때
    1. 어차피 자신의 뉴스피드에는 자신의 포스트가 뜨니까 괜찮을 듯
2. follow/unfollow 를 두 번 할 때
    1. 일단 배제하고 돌려보자

## 1차 코드 작성

```kotlin
class Twitter() {
    val posts = mutableListOf<Pair<Int, Int>>()
    val followers = mutableMapOf<Int, MutableList<Int>>()

    fun postTweet(userId: Int, tweetId: Int) {
        posts.add(0,Pair(userId, tweetId))
    }

    fun getNewsFeed(userId: Int): List<Int> {
        val searchCount = 10
        val result = mutableListOf<Int>()
        val followees = followers.getOrDefault(userId, mutableListOf())
        val allPosts = posts.filter { it.first == userId || followees.contains(it.first) }

        allPosts.forEach {
            if (result.size < searchCount) {
                result.add(it.second)
            }
        }

        return result
    }

    fun follow(followerId: Int, followeeId: Int) {
        followers[followerId] = followers.getOrDefault(followerId, mutableListOf()).apply {
            add(followeeId)
        }
    }

    fun unfollow(followerId: Int, followeeId: Int) {
        followers[followerId]?.remove(followeeId)
    }

}

/**
 * Your Twitter object will be instantiated and called as such:
 * var obj = Twitter()
 * obj.postTweet(userId,tweetId)
 * var param_2 = obj.getNewsFeed(userId)
 * obj.follow(followerId,followeeId)
 * obj.unfollow(followerId,followeeId)
 */
```

![런타임 155ms, 메모리 36.24MB](https://prod-files-secure.s3.us-west-2.amazonaws.com/213ef69f-0d1e-4d21-9468-12fe04ca65b6/e45a5237-6ea6-4cd3-8d7b-4562b19c74b6/Untitled.png)

런타임 155ms, 메모리 36.24MB

## 풀이 복기

1. 복잡하게 생각할 거리는 딱히 없었고 예외처리도 별 게 없었던 것 같다.
2. 자바는 지원하는 함수가 너무 많다…
    1. 업무할땐 이런 기능 없어서 하나하나 내가 다 구현했는데
        1. 오버라이딩도 못하게 막아놓아서 더 힘든데..
3. 속도랑 메모리가 왜 잘나왔는진 모르겠다.
    1. 그나마 자바의 기본 기능이 최적화가 잘 된 편이다정도…?
4. 릿코드에서 다른 사람들의 예시를 보니 우선순위 큐를 사용한 사람들이 많다
    1. 굳이 쓸 필요는 없었던 것 같은데…?
