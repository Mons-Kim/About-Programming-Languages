# Authentication 



## Sendbird SDK 초기화

```java
boolean useCaching = true;
// When the useLocalCaching is set to true.
SendBird.init(APP_ID, getApplicationContext(), useCaching, new InitResultHandler() {
    @Override
    public void onMigrationStarted() {
        Log.i("Application", "Called when there's an update in Sendbird server.");
    }

    @Override
    public void onInitFailed(SendBirdException e) {
        Log.i("Application", "Called when initialize failed. SDK will still operate properly as if useLocalCaching is set to false.");
    }

    @Override
    public void onInitSucceed() {
        Log.i("Application", "Called when initialization is completed.");
    }
});
```

- 해당 메소드는 Application클래스에서 선언할 것. OnCreate()에서 선언.
- appId : 앱 ID
- Context : getApplicationContext()를 사용
- useCashing : 로컬 캐시를 사용할 지 여부
- InitResultHandler : 로컬 캐시 사용하지 않으면, onInitSucceed()만 호출, 사용시에는 onMigrationStarted()와 onInitFailed()가 호출될 수 있음. 실패시 기존의 캐시를 모두 삭제하고 초기화 다시 시도.



## 로그인

```java
// When the useLocalCaching is set to true.
SendBird.connect(userId, new SendBird.ConnectHandler() {
    @Override
    public void onConnected(User user, SendBirdException e) {
        if (user != null) {
            if (e != null) {
                // Proceed in offline mode with the data stored in the local database.
                // Later, connection will be made automatically
                // and can be notified through the ConnectionHandler.onReconnectSucceeded().
            } else {
                // Proceed in online mode.
            }
        } else {
            // Handle error.
        }
    }
});
```

- 로그인시, 엑세스 토큰이나 세션 토큰을 이용하는 방법이 있지만, 토큰의 발행은 Android SDK에서 실행 불가. 토큰 발행을 하려면 Platform API를 이용해야 하기 때문에, 여기서는 해당 방법은 생략함. 사용을 원하면 아래를 참조할 것.

  (https://sendbird.com/docs/chat/v3/android/guides/authentication#2-connect-to-sendbird-server-with-a-user-id-and-a-token)



## 로그아웃

- 이벤트 핸들러를 통한 수신 중지, 내부 캐시 데이터 삭제됨

```java
SendBird.disconnect(new SendBird.DisconnectHandler() {
    @Override
    public void onDisconnected() {
        // The current user is disconnected from Sendbird server.
        ...
    }
});
```



## 프로필 수정

```java
SendBird.updateCurrentUserInfo(NICKNAME, PROFILE_URL, new UserInfoUpdateHandler() {
    @Override
    public void onUpdated(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // The current user's profile is successfully updated.
        // You could redraw the profile in a view in response to this operation.
        ...
    }
});
```

- 프로필 사진 직접 업로드

```java
SendBird.updateCurrentUserInfoWithProfileImage(NICKNAME, PROFILE_FILE, new UserInfoUpdateHandler() {
    @Override
    public void onUpdated(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A new profile images is successfully uploaded to Sendbird server.
        // You could redraw the profile in a view in response to this operation.
        ...
    }
});
```

