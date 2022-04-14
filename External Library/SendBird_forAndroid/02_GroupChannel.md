# Group Channel



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

- 파라미터
  - setIncludeEmpty(true) - 메세지가 아직 존재하지 않는 빈 채널도 포함할지 선택
  - setMemberStateFilter(GroupChannelListQuery.MemberStateFilter.JOINED)
  - setOrder(GroupChannelListQuery.Order.LATEST_LAST_MESSAGE)
  - setLimit(15)



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



## 채널에 회원으로 참가

```java

```

