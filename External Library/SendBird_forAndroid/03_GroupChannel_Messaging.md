# GroupChannel Messaging



## 메세지 보내기

| Message type                                                 | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| UserMessage                                                  | 유저가 보낸 메세지                                           |
| FileMessage                                                  | 유저가 보낸 파일 메세지                                      |
| [AdminMessage](https://sendbird.com/docs/chat/v3/android/guides/group-channel-advanced#2-send-an-admin-message) | [Chat API](https://sendbird.com/docs/chat/v3/platform-api/message/messaging-basics/send-a-message)를 이용해 보낸 어드민 메세지 |

- 유저 메세지 보내기

```java
List<String> userIDsToMention = new ArrayList<>();
userIDsToMention.add("Jeff");
userIDsToMention.add("Julia");

List<String> translationTargetLanguages = new ArrayList<>();
translationTargetLanguages.add("fr");       // French
translationTargetLanguages.add("de");       // German

UserMessageParams params = new UserMessageParams()
        .setMessage(TEXT_MESSAGE)
        .setCustomType(CUSTOM_TYPE)
        .setData(DATA)
        .setMentionType(MentionType.USERS)      // Either USERS or CHANNEL
        .setMentionedUserIds(userIDsToMention)      // Or .setMentionedUsers(LIST_OF_USERS_TO_MENTION)
        .setMetaArrays(Arrays.asList(new MessageMetaArray("itemType", Arrays.asList("tablet")), new MessageMetaArray("quality", Arrays.asList("best", "good")))
        .setTranslationTargetLanguages(translationTargetLanguages)
        .setPushNotificationDeliveryOption(PushNotificationDeliveryOption.DEFAULT); // Either DEFAULT or SUPPRESS

groupChannel.sendUserMessage(params, new BaseChannel.SendUserMessageHandler() {
    @Override
    public void onSent(UserMessage userMessage, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A text message with detailed configuration is successfully sent to the channel.
        // By using userMessage.getMessageId(), userMessage.getMessage(), userMessage.getCustomType(), and so on,
        // you can access the result object from Sendbird server to check your UserMessageParams configuration.
        // The current user can receive messages from other users through the onMessageReceived() method of an event handler.
        long messageId = userMessage.getMessageId();
        ...
    }
});
```

- 파일 메세지 보내기

```java
// Sending a file message with a raw file
List<FileMessage.ThumbnailSize> thumbnailSizes = new ArrayList<>(); // Allowed number of thumbnail images: 3
thumbnailSizes.add(new ThumbnailSize(100,100));
thumbnailSizes.add(new ThumbnailSize(200,200));

List<String> userIDsToMention = new ArrayList<>();
userIDsToMention.add("Jeff");
userIDsToMention.add("Julia");

FileMessageParams params = new FileMessageParams()
        .setFile(FILE)              // Or .setFileURL(FILE_URL). You can also send a file message with a file URL.
        .setFileName(FILE_NAME)
        .setFileSize(FILE_SIZE)
        .setMimeType(MIME_TYPE)
        .setThumbnailSizes(thumbnailSizes)
        .setCustomType(CUSTOM_TYPE)
        .setMentionedType(MentionType.USERS)        // Either USERS or CHANNEL
        .setMentionedUserIds(userIDsToMention)      // Or .setMentionedUsers(LIST_OF_USERS_TO_MENTION)
        .setPushNotificationDeliveryOption(PushNotificationDeliveryOption.DEFAULT); // Either DEFAULT or SUPPRESS

groupChannel.sendFileMessage(params, new BaseChannel.SendFileMessageHandler() {
    @Override
    public void onSent(FileMessage fileMessage, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A file message with detailed configuration is successfully sent to the channel.
        // By using fileMessage.getMessageId(), fileMessage.getFileName(), fileMessage.getCustomType(), and so on,
        // you can access the result object from Sendbird server to check your FileMessageParams configuration.
        // The current user can receive messages from other users through the onMessageReceived() method of an event handler.
        long messageId = fileMessage.getMessageId();
        ...
    }
});
```



## 메세지 파라미터

- 유저 메세지

| Params                         | Description                                                  |
| :----------------------------- | :----------------------------------------------------------- |
| Message                        | 발송할 메세지 내용                                           |
| CustomType                     | 임의의 타입 세팅                                             |
| Data                           | 추가로 포함될 데이터                                         |
| MentionType                    | MentionType.USERS / MentionType.CHANNEL을 통해 타겟을 정함   |
| MentionedUserIds               | MentionType.USERS일 떄 멘션할 유저ID 리스트                  |
| MetaArrays                     | 메타데이터 리스트                                            |
| TranslationTargetLanguages     | 번역을 요청할 타겟 언어 리스트                               |
| PushNotificationDeliveryOption | 푸시 알림 옵션 (DEFAULT: 푸시 발송 / SUPPRESS: 푸시 발송 안함) |

- 파일 메세지

| Params                         | Description                                                  |
| :----------------------------- | :----------------------------------------------------------- |
| File                           | 보낼 파일                                                    |
| FileName                       | 보낼 파일 이름                                               |
| FileSize                       | 파일 크기                                                    |
| MimeType                       | 전송을 위한 변환 타입                                        |
| ThumbnailSizes                 | 세팅한 썸네일 크기들                                         |
| CustomType                     | 임의의 타입 세팅                                             |
| MentionType                    | MentionType.USERS / MentionType.CHANNEL을 통해 타겟을 정함   |
| MentionedUserIds               | MentionType.USERS일 떄 멘션할 유저ID 리스트                  |
| PushNotificationDeliveryOption | 푸시 알림 옵션 (DEFAULT: 푸시 발송 / SUPPRESS: 푸시 발송 안함) |



## IOS기기에 중요한 경고 메세지 보내기

```java
// Send a critical alert user message.
final UserMessageParams userMessageParams = new UserMessageParams();
final AppleCriticalAlertOptions options = new AppleCriticalAlertOptions("name", 0.7); // Acceptable values for `volume` range from 0 to 1.0, inclusive.
userMessageParams.setAppleCriticalAlertOptions(options);

groupChannel.sendUserMessage(userMessageParams, new BaseChannel.SendUserMessageHandler() {
    @Override
    public void onSent(UserMessage message, SendBirdException e) {
        // Handle error.
    }
});
```



## 채널 이벤트 핸들러를 통해 메세지 수신

- 메세지 타입
  - UserMessage
  - FileMessage
  - AdminMessage
- UNIQUE_HANDLER_ID - 핸들러를 등록하는 고유 식별자

```java
SendBird.addChannelHandler(UNIQUE_HANDLER_ID, new SendBird.ChannelHandler() {
    @Override
    public void onMessageReceived(BaseChannel channel, BaseMessage message) {
        // You can customize how to display the different types of messages with the result object in the "message" parameter.
        if (message instanceof UserMessage) {
            ...
        } else if (message instanceof FileMessage) {
            ...
        } else if (message instanceof AdminMessage) {
            ...
        }
    }
});
```

- 더이상 유효하지 않은 경우 핸들러 제거

```java
SendBird.removeChannelHandler(UNIQUE_HANDLER_ID);
```



## 텍스트 메세지로 답장

```java
// Create a `UserMessageParams` object.
UserMessageParams params = new UserMessageParams();
params.setParentMessageId(PARENT_MESSAGE_ID);
...

// Pass the params to the parameter of the `sendUserMessage()` method.
groupChannel.sendUserMessage(params, new BaseChannel.SendUserMessageHandler() {
    @Override
    public void onSent(UserMessage userMessage, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A reply to a specific message in the form of a text message is successfully sent.
        ...
    }
});
```

- PARENT_MESSAGE_ID - 답장할 상위 메세지의 고유 ID를 지정



## 파일 메세지로 답장

```java
// Create a `FileMessageParams` object.
FileMessageParams params = new FileMessageParams();
params.setParentMessageId(PARENT_MESSAGE_ID);
...

// Pass the params to the parameter in the `sendFileMessage()` method.
groupChannel.sendFileMessage(params, new BaseChannel.SendFileMessageHandler() {
    @Override
    public void onSent(FileMessage userMessage, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A reply to a specific message in the form of a file message is successfully sent.
        ...
    }
});
```

- PARENT_MESSAGE_ID - 답장할 상위 메세지의 고유 ID를 지정



## 메세지에 리액션 추가

```java
String emojiKey = "smile";

// The BASE_MESSAGE below indicates a BaseMessage object to add a reaction to.
groupChannel.addReaction(BASE_MESSAGE, emojiKey, new BaseChannel.ReactionHandler() {
    @Override
    public void onResult(ReactionEvent reactionEvent, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});

// The BASE_MESSAGE below indicates a BaseMessage object to delete a reaction from.
groupChannel.deleteReaction(BASE_MESSAGE, emojiKey, new BaseChannel.ReactionHandler() {
    @Override
    public void onResult(ReactionEvent reactionEvent, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});

// Note: To add or remove the emoji of which key matches 'smile' below the message on the current user's chat view,
// the applyReactionEvent() method should be called in the channel event handler's onReactionUpdated() method.
```

- 리액션 기능은 현재 그룹채널에서만 가능함

  

## 상위 메세지의 답장 나열

```java

```



## 메세지 검색

```java

```



## 메세지 업데이트

```java

```



## 메세지 삭제

```java

```



## 메세지 복사

```java

```



## 다른 회원에게 입력중 표시 보내기

```java

```



## 메세지 전달 완료 표시

```java

```



## 메세지 읽음 표시

```java

```



## 메세지를 받지 못한 회원 수 검색

```java

```



## 메세지를 읽은 회원 검색

```java

```



## 메세지를 읽지 않은 회원수 검색

```java

```



## 채널의 마지막 메세지 검색

```java

```



## 채널에세 읽지 않은 메세지 수 검색

```java

```



## 모든 채널에서 읽지 않은 메세지 수 검색

```java

```



## 읽지 않은 메세지가 있는 채널 수 검색

```java

```



## 관리자 메세지 보내기

```java

```



## 메세지에 메타데이터 추가

```java

```



## 메세지에 url링크 표시

```java

```



## 키워드로 메세지 검색

```java

```



## 파일 메세지의 썸네일 생성

```java

```



## 핸들러를 사용하여 파일 업로드 진행 상황 추적

```java

```



## 진행 중인 파일 업로드 취소

```java

```



## 암호화된 파일을 다른 회원과 공유

```java

```



## 메세지 Flood 방지

```java

```



## 메세지 자동 번역

```java

```



## 메세지 번역 요청

```java

```



## 
