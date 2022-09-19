# Group Channel

---

## 목차

---

[채널 생성](#채널 생성)

[Url로 채널 검색](#Url로 채널 검색)

[멤버 초대](#멤버 초대)

[초대 수락/거절](#초대 수락/거절)

[채널에 회원으로 참가](#채널에 회원으로 참가)

[채널 나가기](#채널 나가기)

[채널 정지/해제](#채널 정지/해제)

[채널 삭제](#채널 삭제)

[채널 리스트 검색](#채널 리스트 검색)

[채널 숨김/표시](#채널 숨김/표시)

[이전 메세지 로드](#이전 메세지 로드)

[타임스탬프로 메세지 로드](#타임스탬프로 메세지 로드)

[메세지 ID별로 메세지 로드](#메세지 ID별로 메세지 로드)

[타임스탬프/메세지ID별로 검색한 메세지 파라미터](#타임스탬프/메세지ID별로 검색한 메세지 파라미터)

[채팅 기록 기우기](#채팅 기록 기우기)

[채널 데이터 새로 고침](#채널 데이터 새로 고침)

[모든 구성원 목록 검색](#모든 구성원 목록 검색)

[회원의 온라인 상태 검색](#회원의 온라인 상태 검색)

[특정 순서로 구성원 및 운영자 목록 검색](#특정 순서로 구성원 및 운영자 목록 검색)

[운영자 목록 검색](#운영자 목록 검색)

[운영자 등록](#운영자 등록)

[운영자 등록 취소](#운영자 등록 취소)

[차단 또는 침묵된 사용자 목록 검색](#차단 또는 침묵된 사용자 목록 검색)

[사용자 차단/해제](#사용자 차단/해제)

[사용자 침묵/해제](#사용자 침묵/해제)

[메세지 / 유저 / 채널 신고](#메세지 / 유저 / 채널 신고)

---



## 채널 생성

```java
List<String> users = new ArrayList<>();
users.add("John");
users.add("Harry");
users.add("Jay");

List<String> operators = new ArrayList<>();
operators.add("Jeff");

GroupChannelParams params = new GroupChannelParams()
        .setPublic(false)
        .setEphemeral(false)
        .setDistinct(false)
        .setSuper(false)
        .addUserIds(users)
        .setOperatorUserIds(operators)  // Or .setOperators(List<User> operators)
        .setName(NAME)
        .setChannelUrl(UNIQUE_CHANNEL_URL)  // In a group channel, you can create a new channel by specifying its unique channel URL in a 'GroupChannelParams' object.
        .setCoverImage(FILE)            // Or .setCoverUrl(COVER_URL)
        .setData(DATA)
        .setCustomType(CUSTOM_TYPE);

GroupChannel.createChannel(params, new GroupChannel.GroupChannelCreateHandler() {
    @Override
    public void onResult(GroupChannel groupChannel, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A group channel with detailed configuration is successfully created.
        // By using groupChannel.getUrl(), groupChannel.getMembers(), groupChannel.getData(), groupChannel.getCustomType(), and so on,
        // you can access the result object from Sendbird server to check your GroupChannelParams configuration.
        String channelUrl = groupChannel.getUrl();
        ...
    }
});
```

- 파라미터
  - setPublic(false) - 공개 여부. true면 초대없이 참가 가능
  - setEphemeral(false) - 임시 채널 여부. true면 Sendbird데이터 베이스에 채널 정보 저장안됨.
  - setDistinct(false) - 이전 채널 재개 여부. true면 기존 멤버 조합의 채널이 존재하면 채널을 새로 생성하지 않고 기존 채널로 연결
  - setSuper(false) - 슈퍼 그룹으로 확장하여 생성할 것인지 선택
  - addUserIds(users) - 채널 생성시 초대할 멤버 리스트 추가
  - setOperatorUserIds(operators) - 채널 운영권한을 가진 멤버 리스트 세팅
  - setName(NAME) - 생성 채널 이름 세팅
  - setChannelUrl(UNIQUE_CHANNEL_URL) - 유니크한 채널 주소 세팅
  - setCoverImage(FILE) - 그룹 채널 정보에 포함될 커버 이미지 세팅
  - setData(DATA) - ??
  - setCustomType(CUSTOM_TYPE) - 그룹 채널 타입 세팅. 



## Url로 채널 검색

```java
GroupChannel.getChannel(CHANNEL_URL, new GroupChannel.GroupChannelGetHandler() {
    @Override
    public void onResult(GroupChannel groupChannel, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // Through the "groupChannel" parameter of the onResult() callback method,
        // the group channel object identified with the CHANNEL_URL is returned by Sendbird server,
        // and you can get the group channel's data from the result object.
        String channelName = groupChannel.getName();
        ...
    }
});
```



## 멤버 초대

```java
List<String> userIds = new ArrayList<>();
userIds.add("Tyler");
userIds.add("Young");

groupChannel.inviteWithUserIds(userIds, new GroupChannel.GroupChannelInviteHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});
```

- 초대된 새로운 멤버에게 기존 채팅기록의 표시 여부는 Sendbird 대시보드에서 설정 가능함. 

  (**Settings** > **Chat** > **Channels** > Group channels > Chat history on/off)


- 그룹 채널에 멤버 초대시, 기본값은 초대 자동수락. 초대할 멤버에게 승낙 여부를 결정하게 하려면 추가 세팅이 필요함

  (단, 해당 경우에 초대받은 유저는 초대 수락을 하기 전에는 본인이 참가한 채널리스트에서 검색되지 않음. 따로 해당 url의 채널연결과 초대 수락을 처리해야 채널리스트에 검색됨)

```java
boolean autoAccept = false; // The value of true (default) means that a user will automatically join a group channel with no choice of accepting and declining an invitation.

SendBird.setChannelInvitationPreference(autoAccept, new SendBird.SetChannelInvitationPreferenceHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});
```



## 초대 수락/거절

```java
// Accepting an invitation
groupChannel.acceptInvitation(new GroupChannel.GroupChannelAcceptInvitationHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});

// Declining an invitation
groupChannel.declineInvitation(new GroupChannel.GroupChannelDeclineInvitationHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});
```



## 채널에 회원으로 참가

- Public 그룹 채널에서만 사용

```java
if (groupChannel.isPublic()) {
    groupChannel.join(new GroupChannel.GroupChannelJoinHandler() {
        @Override
        public void onResult(SendBirdException e) {
            if (e != null) {
                // Handle error.
            }

            ...
        }
    });
}
```



## 채널 나가기

```java
groupChannel.leave(new GroupChannel.GroupChannelLeaveHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});
```



## 채널 정지/해제

- 정지시 일반 멤버들의 채팅 불가. 운영 멤버들(operators)만 메세지 전송 가능

```java
// Freezing a channel
groupChannel.freeze(new GroupChannel.GroupChannelFreezeHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // The channel successfully gets frozen.
        // You could display a message telling that chatting in the channel is unavailable, or do something in response to a successful operation.
        ...
    }
});

// Unfreezing a channel
groupChannel.unfreeze(new GroupChannel.GroupChannelUnfreezeHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // The channel successfully gets unfrozen.
        // You could display a message telling that chatting in the channel is available again, or do something in response to a successful operation.
        ...
    }
});
```



## 채널 삭제

- 운영 멤버들만 삭제 가능. 일반 멤버의 클라이언트에서 실행시 오류 반환

```java
groupChannel.delete(new GroupChannel.GroupChannelDeleteHandler() {
    @Override
    public void onResult(final SendBirdException e) {
        if (e != null) {
            // Handle error.
            return;
        }
        // The channel is successfully deleted.
    }
});
```



## 채널 리스트 검색

- 파라미터
  - setIncludeEmpty(true) - 메세지가 아직 존재하지 않는 빈 채널도 포함할지 선택
  - setOrder(GroupChannelListQuery.Order.LATEST_LAST_MESSAGE) - 리스트 정렬 방식 선택
  - setLimit(15) - 표시 개수

- 현재 유저의 private 그룹 채널 리스트를 검색.

```java
GroupChannelListQuery listQuery = GroupChannel.createMyGroupChannelListQuery();
listQuery.setIncludeEmpty(true);
listQuery.setMemberStateFilter(GroupChannelListQuery.MemberStateFilter.JOINED);
listQuery.setOrder(GroupChannelListQuery.Order.LATEST_LAST_MESSAGE);    // CHRONOLOGICAL, LATEST_LAST_MESSAGE, CHANNEL_NAME_ALPHABETICAL, and METADATA_VALUE_ALPHABETICAL
listQuery.setLimit(15);

listQuery.next(new GroupChannelListQuery.GroupChannelListQueryResultHandler() {
    @Override
    public void onResult(List<GroupChannel> list, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A list of matching group channels is successfully retrieved.
        // Through the "list" parameter of the onResult() callback method,
        // you can access the data of each group channel from the result list that Sendbird server has passed to the callback method.
        List<GroupChannel> channels = list;
        ...
    }
});
```

- 현재 유저의 public 그룹 채널 리스트 검색.

```java
PublicGroupChannelListQuery listQuery = GroupChannel.createPublicGroupChannelListQuery();
listQuery.setIncludeEmpty(true);
listQuery.setMembershipFilter(PublicGroupChannelListQuery.MembershipFilter.JOINED);
listQuery.setOrder(PublicGroupChannelListQuery.Order.CHANNEL_NAME_ALPHABETICAL);    // CHRONOLOGICAL, CHANNEL_NAME_ALPHABETICAL, and METADATA_VALUE_ALPHABETICAL
listQuery.setLimit(15);

listQuery.next(new PublicGroupChannelListQuery.PublicGroupChannelListQueryResultHandler() {
    @Override
    public void onResult(List<GroupChannel> list, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A list of public group channels matching search criteria is successfully retrieved.
        // Through the "list" parameter of the onResult() callback method,
        // you can access the data of each group channel from the result list that Sendbird server has passed to the callback method.
        List<GroupChannel> channels = list;
        ...
    }
});
```

- 필터 (group: 그룹 채널 / public: public그룹채널)

  - CustomTypesFilter - (group/public) 하나 이상의 지정된 타입으로 채널을 필터링함

  - CustomTypeStartsWithFilter - (group/public) 지정된 값으로 시작하는 타입들로 채널을 필터링함

  - ChannelNameContainsFilter - (group/public) 지정된 값이 채널이름에 포함된 채널을 필터링함

  - ChannelUrlsFilter - (group/public) 하나 이상의 지정된 url로 채널을 필터링함

  - SuperChannelFilter - (group/public) 슈퍼/비슈퍼 채널로 필터링함

  - PublicChannelFilter - (group/public) 공개/비공개 채널로 필터링함

  - UnreadChannelFilter - (group) 하나 이상의 읽지 않은 메세지가 있는 그룹 채널을 필터링함

  - HiddenChannelFilter - (group) 숨겨진 채널을 필터링함

    ```
    UNHIDDEN,
    HIDDEN_ALLOW_AUTO_UNHIDE,
    HIDDEN_PREVENT_AUTO_UNHIDE
    ```

  - MemberStateFilter - (group) 유저가 채널에 관여된 상태에 따라 필터링함 

    ```
    ALL,
    INVITED,
    INVITED_BY_FRIEND,
    INVITED_BY_NON_FRIEND,
    JOINED
    ```

  - MembershipFilter - (public) 유저가 채널에 관여된 상태에 따라 필터링함

    ```
    ALL,
    JOINED
    ```

  - UserIdsExactFilter - (group) 하나 이상의 지정된 값들과 일치된 유저ID를 포함하는 채널을 필터링함 

  - UserIdsIncludeFilter - (group) 하나 이상의 지정된 값들을 포함하는 유저ID를 포함하는 채널을 필터링함

  - NicknameContainsFilter - (group) 지정된 값을 포함하는 닉네임을 가진 유저를 포함하는 채널을 필터링함

  - MetaDataOrderKeyFilter - (group/public) 지정된 값을 키로 상용하는 항목을 포함하는 채널을 필터링함



## 채널 숨김/표시

```java
// Hiding (archiving) a group channel.
groupChannel.hide(IS_HIDE_PREVIOUS_MESSAGES, IS_ALLOW_AUTO_UNHIDE, new GroupChannel.GroupChannelHideHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // The channel successfully gets hidden from the list.
        // The current user's channel view should be refreshed to reflect the change.
        ...
    }
});

// Unhiding a group channel.
groupChannel.unhide(new GroupChannel.GroupChannelUnhideHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // The channel successfully gets unhidden from the list.
        // The current user's channel view should be refreshed to reflect the change.
        ...
    }
});
```

- 파라미터

  - IS_HIDE_PREVIOUS_MESSAGES - 채널이 리스트에 다시 표시될 때, 숨기기 전에 보내고 받은 메세지를 숨길지 선택. 기본값 false
  - IS_ALLOW_AUTO_UNHIDE - 숨겨놓은 채널이 다른 구성원의 새 메세지를 수신했을 때, 자동으로 숨김 해제가 될 것인지 선택. 기본값 true

- getHiddenState()으로 목록에 대한 채널 상태 확인 가능

  ```
  UNHIDDEN, // 표시된 상태
  HIDDEN_ALLOW_AUTO_UNHIDE, // 숨겨진 상태. 새 메세지 수신시 자동 숨김 해제
  HIDDEN_PREVENT_AUTO_UNHIDE // 숨겨진 상태. 새 메세지에 의한 자동 숨김 해제 안함.
  ```

  ```java
  if (groupChannel.getHiddenState() == GroupChannel.HiddenState.UNHIDDEN) {
      // Display the channel in the list.
  } else if (groupChannel.getHiddenState() == GroupChannel.HiddenState.HIDDEN_ALLOW_AUTO_UNHIDE) {
      // Hide the channel from the list, and get it appeared back on condition.
  } else if (groupChannel.getHiddenState() == GroupChannel.HiddenState.HIDDEN_PREVENT_AUTO_UNHIDE) {
      // Archive the channel, and get it appeared back in the list only when the unhide() is called.
  }
  ```

  

## 이전 메세지 로드

```java
// There should only be one single instance per channel view.
PreviousMessageListQuery listQuery = groupChannel.createPreviousMessageListQuery();
listQuery.setIncludeMetaArray(true);    // Retrieve a list of messages along with their metaarrays.
listQuery.setIncludeReactions(true);    // Retrieve a list of messages along with their reactions.
...

// Retrieving previous messages.
listQuery.load(LIMIT, REVERSE, new PreviousMessageListQuery.MessageListQueryResult() {
    @Override
    public void onResult(List<BaseMessage> messages, SendBirdException e) {
        if (e != null) {
                // Handle error.
        }

        ...
    }
});
```

- 파라미터

  | `LIMIT`                       | int     | Specifies the number of results to return per call. Acceptable values are 1 to 100, inclusive. The recommended value for this parameter is 30. |
  | ----------------------------- | ------- | ------------------------------------------------------------ |
  | `REVERSE`                     | boolean | Determines whether to sort the retrieved messages in reverse order. If **false**, the results are in ascending order. |
  | `INCLUDE_THREAD_INFO`         | boolean | Determines whether to include the information of the message thread in the results that contain parent messages. |
  | `REPLY_TYPE_FILTER`           | enum    | Specifies the type of message to include in the results. - **NONE (default):** All messages that are not replies. These message may or may not have replies in its thread. - **ALL:** All messages including threaded and non-threaded parent messages as well as its replies. - **ONLY_REPLY_TO_CHANNEL:** Messages that are not threaded. It only includes the parent messages and replies that were sent to the channel. |
  | `INCLUDE_PARENT_MESSAGE_INFO` | boolean | Determines whether to include the information of the parent messages in the result. (Default: **false**) |

- 이전 메세지를 계속 이어서 가져오기 위해서는 생성한 PreviousMessageListQuery의 인스턴스를 재사용해야 함. 새로운 인스턴스로 load()를 실행시, 가져오는 리스트가 초기화됨.

  

## 타임스탬프로 메세지 로드

```java
MessageListParams params = new MessageListParams();
params.setInclusive(IS_INCLUSIVE);
params.setPreviousResultSize(PREVIOUS_RESULT_SIZE);
params.setNextResultSize(NEXT_RESULT_SIZE);
params.setReverse(REVERSE);
params.setMessageType(MESSAGE_TYPE);
params.setCustomType(CUSTOM_TYPE);
...

groupChannel.getMessagesByTimestamp(TIMESTAMP, params, new BaseChannel.GetMessagesHandler() {
    @Override
    public void onResult(List<BaseMessage> messages, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A list of previous and next messages on both sides of a specified timestamp is successfully retrieved.
        // Through the "messages" parameter of the onResult() callback method,
        // you can access and display the data of each message from the result list that Sendbird server has passed to the callback method.
        List<BaseMessage> messages = messages;
        ...
    }
});
```



## 메세지 ID별로 메세지 로드

```java
MessageListParams params = new MessageListParams();
params.setInclusive(IS_INCLUSIVE);
params.setPreviousResultSize(PREVIOUS_RESULT_SIZE);
params.setNextResultSize(NEXT_RESULT_SIZE);
params.setReverse(REVERSE);
params.setMessageType(MESSAGE_TYPE);
params.setCustomType(CUSTOM_TYPE);
...

groupChannel.getMessagesByMessageId(MESSAGE_ID, params, new BaseChannel.GetMessagesHandler() {
    @Override
    public void onResult(List<BaseMessage> messages, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // A list of previous and next messages on both sides of a specified message ID is successfully retrieved.
        // Through the "messages" parameter of the onResult() callback method,
        // you can access and display the data of each message from the result list that Sendbird server has passed to the callback method.
        List<BaseMessage> messages = messages;
        ...
    }
});
```



## 타임스탬프/메세지ID별로 검색한 메세지 파라미터

| Argument                      | Type    | Description                                                  |
| :---------------------------- | :------ | :----------------------------------------------------------- |
| `TIMESTAMP`                   | long    | 검색할 메시지에 대한 참조 지점이 될 타임스탬프를 지정        |
| `MESSAGE_ID`                  | long    | 검색할 메시지의 참조 지점이 될 메시지 ID를 지정              |
| `PREV_RESULT_SIZE`            | int     | 지정된 타임스탬프 또는 메시지 ID 이전에 보낸 검색할 메시지 수를 지정 |
| `NEXT_RESULT_SIZE`            | int     | 지정된 타임스탬프 또는 메시지 ID 이후에 보낸 검색할 메시지 수를 지정 |
| `INCLUSIVE`                   | boolean | 결과에 일치하는 타임스탬프 또는 메시지 ID가 있는 메시지를 포함할지 여부를 결정 |
| `REVERSE`                     | boolean | 검색된 메시지를 역순으로 정렬할지 여부를 결정합니다. **false** 이면 결과가 오름차순 |
| `INCLUDE_THREAD_INFO`         | boolean | 부모 메시지가 포함된 결과에 메시지 스레드 정보를 포함할지 여부를 결정 |
| `REPLY_TYPE_FILTER`           | enum    | 결과에 포함할 메시지 유형을 지정합니다.<br/>\- **NONE(기본값):** 회신이 아닌 모든 메시지. 이 메시지는 스레드에 응답이 있을 수도 있고 없을 수도 있습니다.<br/>\- **ALL:** 스레드 및 스레드되지 않은 상위 메시지와 해당 응답을 포함한 모든 메시지.<br/>\- **ONLY_REPLY_TO_CHANNEL:** 스레드되지 않은 메시지입니다. 여기에는 채널에 보낸 상위 메시지와 응답만 포함 |
| `INCLUDE_PARENT_MESSAGE_INFO` | boolean | 결과에 상위 메시지의 정보를 포함할지 여부를 결정 (Default: **false**) |



## 채팅 기록 지우기

- 유저가 참가한 그룹 채널의 채팅기록을 삭제.
- 현재 사용자의 리스트에만 표시가 안될 뿐. Sendbird데이터베이스의 데이터가 삭제되는 것은 아님.

```java
groupChannel.resetMyHistory(new GroupChannel.GroupChannelResetMyHistoryHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});
```



## 채널 데이터 새로 고침

- 사용자의 그룹채널을 최신 정보로 업데이트함.
- Sendbird서버와 연결을 끊었다가 나중에 다시 연결하는 경우에 다음을 호출해야 함.

```java
groupChannel.refresh(new GroupChannel.GroupChannelRefreshHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});
```



## 모든 구성원 목록 검색

```java
List<Member> members = groupChannel.getMembers();
```



## 회원의 온라인 상태 검색

```java
for (User user : groupChannel.getMembers()) {
    if (user.getConnectionStatus() == User.ConnectionStatus.OFFLINE) {
        // ...
    }
}
```

| Value                         | Description   |
| :---------------------------- | :------------ |
| User.ConnectionStatus.OFFLINE | 오프라인 상태 |
| User.ConnectionStatus.ONLINE  | 온라인 상태   |



## 특정 순서로 구성원 및 운영자 목록 검색

```java
final GroupChannelMemberListQuery listQuery = groupChannel.createMemberListQuery();
listQuery.setLimit(10);
listQuery.setOrder(GroupChannelMemberListQuery.Order.OPERATOR_THEN_MEMBER_ALPHABETICAL);

while (listQuery.hasNext()) {
    listQuery.next(new GroupChannelMemberListQueryResultHandler() {
        @Override
        public void onResult(List<Member> list, SendBirdException e) {
            if (e != null) {
                // Handle error.
            }

            // A list of matching members and operators is successfully retrieved.
            // Through the "list" parameter of the callback method,
            // you can access the data of each item from the result list that Sendbird server has passed to the callback method.
            List<member> members = list;
            ...
        }
    }
}
```

| Value                             | Description                                      |
| :-------------------------------- | :----------------------------------------------- |
| MEMBER_NICKNAME_ALPHABETICAL      | 멤버는 알파벳 순서로 정렬됩니다(기본값)          |
| OPERATOR_THEN_MEMBER_ALPHABETICAL | 운영자는 알파벳 순서로 멤버보다 먼저 순서가 지정 |



```java
final GroupChannelMemberListQuery listQuery = groupChannel.createMemberListQuery();
listQuery.setLimit(10);
listQuery.setOperatorFilter(GroupChannelMemberListQuery.OperatorFilter.OPERATOR);    // ALL, OPERATOR, and NONOPERATOR

while (listQuery.hasNext()) {
    listQuery.next(new GroupChannelMemberListQuery.GroupChannelMemberListQueryResultHandler() {
        @Override
        public void onResult(List<Member> list, SendBirdException e) {
            if (e != null) {
                // Handle error.
            }

            // A list of matching members and operators is successfully retrieved.
            // Through the "list" parameter of the callback method,
            // you can access the data of each item from the result list that Sendbird server has passed to the callback method.
            List<member> members = list;
            ...
        }
    }
}
```

| Value       | Description                   |
| :---------- | :---------------------------- |
| ALL         | 모든 구성원 (기본값)          |
| OPERATOR    | 운영자만 검색                 |
| NONOPERATOR | 운영자를 제외한 구성원을 검색 |



## 운영자 목록 검색

```java
OperatorListQuery listQuery = groupChannel.createOperatorListQuery();
while (listQuery.hasNext()) {
    listQuery.next(new OperatorListQuery.OperatorListQueryResultHandler() {
        @Override
        public void onResult(List<User> list, SendBirdException e) {
            if (e != null) {
                // Handle error.
            }

            // A list of the operators of the channel is successfully retrieved.
            // Through the "list" parameter of the onResult() callback method,
            // you can access and display the data of each operator from the result list that Sendbird server has passed to the callback method.
            List<user> operators = list;
            ...
        }

        ...
    });
}
```



## 운영자 등록

```java
groupChannel.addOperators(USER_IDS, new AddOperatorsHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // The participants are successfully registered as operators of the channel.
        ...
    }
});
```



## 운영자 등록 취소

```java
groupChannel.removeOperators(USER_IDS, new RemoveOperatorsHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        // The cancel operation is succeeded,
        // and you could display some message to those who are not operators anymore.
        ...
    }
});
```

- 한꺼번에 삭제시

  ```java
  groupChannel.removeAllOperators(new RemoveAllOperatorsHandler() {
      @Override
      public void onResult(SendBirdException e) {
          if (e != null) {
              // Handle error.
          }
  
          // The cancel operation is succeeded,
          // and you could display some message to those who are not operators anymore.
          ...
      }
  });
  ```

  

## 차단 또는 침묵된 사용자 목록 검색

```java
// Retrieving banned users.
BannedUserListQuery listQuery = groupChannel.createBannedUserListQuery();
listQuery.next(new UserListQuery.UserListQueryResultHandler() {
    @Override
    public void onResult(List<User> list, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});

// Retrieving muted users.
GroupChannelMemberListQuery listQuery = groupChannel.createMemberListQuery();
listQuery.setMutedMemberListFilter(GroupChannelMemberListQuery.MutedMemberFilter.MUTED);
listQuery.next(new GroupChannelMemberListQuery.GroupChannelMemberListQueryResultHandler() {
    @Override
    public void onResult(List<Member> list, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});
```



## 사용자 차단/해제

```java
GroupChannel.getChannel(CHANNEL_URL, new GroupChannel.GroupChannelGetHandler() {
    @Override
    public void onResult(GroupChannel groupChannel, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        if (groupChannel.getMyRole() == Member.Role.OPERATOR) {
            // Ban a user.
            groupChannel.banUser(USER, DESCRIPTION, SECONDS, new GroupChannel.GroupChannelBanHandler() {
                @Override
                public void onResult(SendBirdException e) {
                    if (e != null) {
                        // Handle error.
                    }

                    // The user is successfully banned from the channel.
                    // You could notify the user of being banned by displaying a prompt.
                    ...
                }
            });

            // Unban a user.
            groupChannel.unbanUser(USER, new GroupChannel.GroupChannelUnbanHandler() {
                @Override
                public void onResult(SendBirdException e) {
                    if (e != null) {
                        // Handle error.
                    }

                    // The user is successfully unbanned from the channel.
                    // You could notify the user of being unbanned by displaying a prompt.
                    ...
                }
            });
        }
    }
});
```



## 사용자 침묵/해제

```java
GroupChannel.getChannel(CHANNEL_URL, new GroupChannel.GroupChannelGetHandler() {
    @Override
    public void onResult(GroupChannel groupChannel, SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        if (groupChannel.getMyRole() == Member.Role.OPERATOR) {
            // Mute a user.
            groupChannel.muteUser(USER, new groupChannel.GroupChannelMuteHandler() {
                @Override
                public void onResult(SendBirdException e) {
                    if (e != null) {
                        // Handle error.
                    }

                    // The user is successfully muted in the channel.
                    // You could notify the user of being muted by displaying a prompt.
                    ...
                }
            });

            // Unmute a user.
            groupChannel.unmuteUser(USER, new GroupChannel.GroupChannelUnmuteHandler() {
                @Override
                public void onResult(SendBirdException e) {
                    if (e != null) {
                        // Handle error.
                    }

                    // The user is successfully unmuted in the channel.
                    // You could notify the user of being unmuted by displaying a prompt.
                    ...
                }
            });
        }
    }
});
```



## 메세지 / 유저 / 채널 신고

```java
// Reporting a message.
groupChannel.reportMessage(MESSAGE_TO_REPORT, REPORT_CATEGORY, DESCRIPTION, new BaseChannel.ReportMessageHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});

// Reporting a user.
groupChannel.reportUser(OFFENDING_USER, REPORT_CATEGORY, DESCRIPTION, new BaseChannel.ReportUserHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});

// Reporting a channel.
groupChannel.report(REPORT_CATEGORY, DESCRIPTION, new BaseChannel.ReportHandler() {
    @Override
    public void onResult(SendBirdException e) {
        if (e != null) {
            // Handle error.
        }

        ...
    }
});
```

| Argument            | Type   | Description                                                  |
| :------------------ | :----- | :----------------------------------------------------------- |
| `MESSAGE_TO_REPORT` | object | 의심스럽거나 괴롭히거나 부적절한 콘텐츠에 대해 보고할 메시지를 지정 |
| `OFFENDING_USER`    | object | 노골적인 메시지나 부적절한 댓글을 보내는 등 모욕적이거나 모욕적인 언어를 사용하는 사용자를 지정 |
| `REPORT_CATEGORY`   | enum   | 보고 이유를 나타내는 보고서 범주를 지정합니다.  **ReportCategory.SUSPICIOUS** , **ReportCategory.HARASSING** , **ReportCategory.INAPPROPRIATE** 및 **ReportCategory.SPAM** |
| `DESCRIPTION`       | string | 보고서에 포함할 추가 정보를 지정                             |

