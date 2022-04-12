# Push Notifications

- 안드로이드 클라이언트 앱에 대한 푸시알림은 FCM(Firebase Cloud Messaging) 또는 HMS(Huawei Mobile Service)를 사용하여 전송함.
- 기본적으로 사용자의 장치와 Sendbird 서버와의 연결이 끊어지면 푸시알림 발송. 기본적으로는 사용자의 모든 장치에서 연결이 해제되어야 푸시알림 발송함.



### FCM 푸시 알림

- 파이어베이스에서 일반적인 FCM 세팅 과정은 생략, Sendbird 서버에 연결하는 파트만 설명.

1. 서버키 생성

   - 해당 프로젝트를 위한 계정의 파이어베이스 콘솔로 이동 (https://console.firebase.google.com/)
   - 프로젝트 개요의 환경설정 아이콘-> 2번째 탭인 클라우드 메시징 선택 -> Cloud Messaging API을 "사용 설정됨"으로 변경해주고 해당 영역의 서버키 복사
   - 해당 프로젝트의 설정에서 google-services.json을 받아 안드로이드 앱 프로젝트에 추가

2. Sendbird 대시보드에 서버키 등록

   - 대시보드 -> Settings -> Notifications -> Notifications 상태 on으로 변경
   - Push notifications for multi-device users -> "Send to devices when all offline" 설정
   - Push notification credentials -> 안드로이드 FCM의 "Add credential"을 이용하여 복사한 서버키 세팅

3. Android Project에 FCM 클라이언트 앱 설정

   - FirebaseMessagingServices 상속 클래스 생성
   - Menifest파일 세팅
   - FirebaseApp 초기화

4. Sendbird 서버에 등록 토큰 등록

   ```java
   SendBird.connect(getSendbirdUserID(), new SendBird.ConnectHandler() {
               @Override
               public void onConnected(User user, SendBirdException e) {
   
                   if (user != null) {
                       if (e != null) {
                           // offline mode. connection will be made automatically.
                           // The connection will be notified by ConnectionHandler.onReconnectSucceeded()
                           callback.onFailed(e);
                       } else {
                           // online mode
                           mUser = user;
   
                           // Register a registration token to Sendbird server.
                           String token = getFCM_Token();
                           SendBird.registerPushTokenForCurrentUser(token, new SendBird.RegisterPushTokenWithStatusHandler() {
                               @Override
                               public void onRegistered(SendBird.PushTokenRegistrationStatus ptrs, SendBirdException e) {
                                   if (e != null) {
                                       // Handle error.
                                   }
   
                                   if (ptrs == SendBird.PushTokenRegistrationStatus.PENDING) {
                                       // A token registration is pending.
                                       // Retry the registration after a connection has been successfully established.
                                   }
                               }
                           });
   
                           callback.onSucceed();
                       }
                   } else {
                       // Handle error.
                       callback.onFailed(e);
                   }
               }
           });
   ```

   

5. FCM 메시지 페이로드 처리

   ```java
   @Override
       public void onMessageReceived(RemoteMessage remoteMessage) {
   
           //Log.d(NSTag, "Func(onMessageReceived) Start");
   
           
   
           if (remoteMessage.getData() != null) {
               //sendNotification(remoteMessage.getNotification().getTitle(), remoteMessage.getNotification().getBody());
               // 노티 전송
               sendNotification(remoteMessage.getNotification(), remoteMessage.getData());
           }
   
       }
   ```

   





### HMS 푸시알림

- 생략, 필요한 사람은 아래 링크를 참조
- https://sendbird.com/docs/chat/v3/android/guides/push-notifications#2-push-notifications-for-hms

# 상수



```swift
let 상수명1: 자료형 = 초기값
```

```swift
let a: Int 100
```

- 선언과 동시에 초기값 설정.

  

```swift
let 상수명2: 자료형
상수명2 = 초기값
```

```swift
let b: Int
b = 100
```

- 최초 한 번에 한해, 초기값 할당 가능.

- 상수의 값을 읽기 전까지는 반드시 초기화가 이루어져야 함.

  ```swift
  let a: Int
  a = 0
  let b = a
  // OK
  ```

    ```swift
  let a: Int
  let b = a // Error
  a = 0
    ```

