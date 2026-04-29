## Q. 트위터의 팔로우 시스템을 옵저버 패턴으로 어떻게 구현할 수 있나요?

### 핵심 답변
트위터 사용자가 Subject, 팔로워가 Observer 역할을 합니다. 팔로우(register), 언팔로우(unregister), 트윗 게시(notifyObservers)를 옵저버 패턴의 핵심 연산으로 구현합니다.

### 상세 설명

```javascript
// Subject: 트위터 사용자 (트윗 작성자)
class TwitterUser {
    constructor(username) {
        this.username = username;
        this.followers = new Set();
        this.tweets = [];
    }

    // register
    follow(follower) {
        this.followers.add(follower);
        console.log(`@${follower.username}이 @${this.username}을 팔로우`);
    }

    // unregister
    unfollow(follower) {
        this.followers.delete(follower);
        console.log(`@${follower.username}이 @${this.username}을 언팔로우`);
    }

    // 트윗 발행 = notifyObservers
    tweet(content) {
        const post = { author: this.username, content, time: new Date() };
        this.tweets.push(post);
        this.notifyFollowers(post);
    }

    notifyFollowers(post) {
        this.followers.forEach(follower => follower.receiveTweet(post));
    }
}

// Observer: 팔로워
class Follower {
    constructor(username) {
        this.username = username;
        this.timeline = [];
    }

    // update
    receiveTweet(tweet) {
        this.timeline.push(tweet);
        console.log(`[@${this.username} 타임라인] @${tweet.author}: ${tweet.content}`);
    }
}

// 사용 예
const elonMusk = new TwitterUser('elonMusk');
const userA = new Follower('userA');
const userB = new Follower('userB');

elonMusk.follow(userA);
elonMusk.follow(userB);

elonMusk.tweet('화성 갑니다 🚀');
// → [@userA 타임라인] @elonMusk: 화성 갑니다 🚀
// → [@userB 타임라인] @elonMusk: 화성 갑니다 🚀

elonMusk.unfollow(userA);
elonMusk.tweet('떠납니다');
// → [@userB 타임라인] @elonMusk: 떠납니다
// userA는 수신 안 함
```

### 꼬리 질문
- 팔로워가 수백만 명일 때 알림 성능을 어떻게 최적화하나요?
- 실제 트위터는 이 방식을 어떻게 변형하여 구현할까요? (Fan-out 전략)

### 실무 사례
실제 SNS에서는 "Fan-out on write"(쓸 때 팔로워 피드에 직접 저장) vs "Fan-out on read"(읽을 때 계산) 전략을 인플루언서 여부에 따라 혼용합니다.
