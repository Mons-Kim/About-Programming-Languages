# Push Notifications

- 안드로이드 클라이언트 앱에 대한 푸시알림은 FCM(Firebase Cloud Messaging) 또는 HMS(Huawei Mobile Service)를 사용하여 전송함.
- 기본적으로 사용자의 장치와 Sendbird 서버와의 연결이 끊어지면 푸시알림 발송. 기본적으로는 사용자의 모든 장치에서 연결이 해제되어야 푸시알림 발송함.



## FCM 푸시 알림

### 1. 기본 세팅

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




### 2. 알림 활성화 설정

```java
public void setPushNotification(boolean enable) {
  
    if (enable) { // 알림 활성화
        SendBird.registerPushTokenForCurrentUser(pushToken, new SendBird.RegisterPushTokenWithStatusHandler() {
            @Override
            public void onRegistered(SendBird.PushTokenRegistrationStatus status, SendBirdException e) {
                if (e != null) {
                    // Handle error.
                }

                ...
            }
        });
    }
    else { // 알림 해제
      
        // If you want to unregister the current device only, call this method.
        SendBird.unregisterPushTokenForCurrentUser(pushToken, new SendBird.UnregisterPushTokenHandler() {
            @Override
            public void onUnregistered(SendBirdException e) {
                if (e != null) {
                    // Handle error.
                }

                ...
            }
        });

        // If you want to unregister all devices of the user, call this method.
        SendBird.unregisterPushTokenAllForCurrentUser(new SendBird.UnregisterPushTokenHandler() {
            @Override
            public void onUnregistered(SendBirdException e) {
                if (e != null) {
                    // Handle error.
                }

                ...
            }
        });
    }
}
```



### 3. 알림 상황 설정

- 트리거 옵션
  - ALL - 모든 메세지에 대해 알림
  - MENTION_ONLY - 해당 유저가 언급된 메세지만 알림
  - OFF - 알림하지 않음

```java
// If you want to trigger notification messages only when the current user is mentioned in messages.
SendBird.setPushTriggerOption(SendBird.PushTriggerOption.MENTION_ONLY, new SendBird.SetPushTriggerOptionHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
}
```





## HMS 푸시알림

- 생략, 필요한 사람은 아래 링크를 참조
- https://sendbird.com/docs/chat/v3/android/guides/push-notifications#2-push-notifications-for-hms



## 다중 기기 지원

- 동시에 여러개의 기기에서 푸시알림을 받을 수 있도록 지원함.
- 해당 세팅을 앱에 추가하려면 샌드버드 프로젝트의 기능외 푸시알림 설정을 지원할 수 없음. 다중 기기 지원을 위한 코드세팅에서 충돌이 남.
- 해당 기능을 사용하려면 아래 주소 참조
- https://sendbird.com/docs/chat/v3/android/guides/push-notifications-multi-device-support#2-push-notifications-for-fcm-3-step-6-enable-multi-device-support-in-the-dashboard
