# Event Handler



- List of open channel events

| Method                               | Invoked when                                                 | Notified devices                                             |
| :----------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `onMessageReceived()`                | A message has been received in an open channel.              | 메시지를 보낸 장치를 제외하고 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMessageUpdated()`                 | A message has been updated in an open channel.               | 메시지가 업데이트된 장치를 제외하고 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMessageDeleted()`                 | A message has been deleted in an open channel.               | 메시지가 삭제된 장치를 ""포함"" 하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMentionReceived()`                | A user has been mentioned in a message sent in an open channel. | 다른 사용자를 언급하는 데 사용된 장치를 포함하여 채널에서 언급된 사용자의 모든 장치(최대 10개). |
| `onChannelChanged()`                 | One of the following [open channel properties](https://sendbird.com/docs/chat/v3/platform-api/guides/open-channel#2-resource-representation) has been changed: name, cover image, data, custom type, or operators. | 채널이 변경된 장치를 포함하여 변경된 채널에 연결된 모든 장치. |
| `onChannelDeleted()`                 | An open channel has been deleted.                            | 채널이 삭제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onChannelFrozen()`                  | An open channel has been frozen.                             | 채널이 고정된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onChannelUnfrozen()`                | An open channel has been unfrozen.                           | 채널이 고정 해제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaDataCreated()`                | A metadata for an open channel has been created.             | 메타데이터가 생성된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaDataUpdated()`                | A metadata for an open channel has been updated.             | 메타데이터가 업데이트된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaDataDeleted()`                | A metadata for an open channel has been deleted.             | 메타데이터가 삭제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaCounterCreated()`             | A metacounter for an open channel has been created.          | 메타 카운터가 생성된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaCounterUpdated()`             | A metacounter for an open channel has been updated.          | 메타카운터가 업데이트된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaCounterDeleted()`             | A metacounter for an open channel has been deleted.          | 메타 카운터가 삭제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onUserEntered()`                    | A user has entered an open channel.                          | 사용자가 해당 채널에 입력한 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onUserExited()`                     | A user has exited an open channel.                           | 사용자가 해당 채널을 종료한 장치를 제외하고 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onUserMuted()`                      | A user has been muted in an open channel.                    | 사용자가 음소거된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onUserUnmuted()`                    | A user has been unmuted in an open channel.                  | 사용자가 음소거 해제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onUserBanned()`                     | A user has been banned from an open channel.                 | 사용자가 차단된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onUserUnbanned()`                   | A user has been unbanned from an open channel.               | 사용자가 차단 해제된 기기를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 기기. |
| `onChannelParticipantCountChanged()` | One or more open channels' participant count has changed.    | 클라이언트 앱의 채널 목록에서 영향을 받는 열린 채널이 있는 연결된 모든 장치. |



- List of group channel events

| Method                          | Invoked when                                                 | Notified devices                                             |
| :------------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `onMessageReceived()`           | A message has been received in a group channel.              | 메시지를 보낸 장치를 제외하고 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMessageUpdated()`            | A message has been updated in a group channel.               | 메시지가 업데이트된 장치를 제외하고 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMessageDeleted()`            | A message has been deleted in a group channel.               | 메시지가 삭제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMentionReceived()`           | A user has been mentioned in a message sent in a group channel. | 다른 사용자를 언급하는 데 사용된 장치를 제외하고 채널에서 언급된 사용자의 모든 장치(최대 10개). |
| `onReactionUpdated()`           | A message reaction has been updated in a group channel.      | 메시지에 반응한 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onChannelChanged()`            | One of the following [group channel properties](https://sendbird.com/docs/chat/v3/platform-api/channel/channel-overview#4-list-of-properties-for-group-channels) has been changed: distinct, push notification preferences, last message (except when the silent [admin message](https://sendbird.com/docs/chat/v4/android/guides/group-channel-advanced#2-send-an-admin-message)), unread message count, name, cover image, data, or custom type. | 채널이 변경된 장치를 포함하여 변경된 채널에 연결된 모든 장치. |
| `onChannelDeleted()`            | A group channel has been deleted.                            | 채널이 삭제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onChannelFrozen()`             | A group channel has been frozen.                             | 채널이 고정된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onChannelUnfrozen()`           | A group channel has been unfrozen.                           | 채널이 고정 해제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaDataCreated()`           | A metadata for a group channel has been created.             | 메타데이터가 생성된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaDataUpdated()`           | A metadata for a group channel has been updated.             | 메타데이터가 업데이트된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaDataDeleted()`           | A metadata for a group channel has been deleted.             | 메타데이터가 삭제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaCounterCreated()`        | A metacounter for a group channel has been created.          | 메타 카운터가 생성된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaCounterUpdated()`        | A metacounter for a group channel has been updated.          | 메타카운터가 업데이트된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onMetaCounterDeleted()`        | A metacounter for a group channel has been deleted.          | 메타 카운터가 삭제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onChannelHidden()`             | A group channel has been hidden from the list.               | 채널을 숨긴 사용자의 모든 기기.                              |
| `onUserReceivedInvitation()`    | A user has been invited to a group channel.                  | 초대를 받은 사용자의 기기를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 기기. |
| `onUserDeclinedInvitation()`    | A user has declined an invitation to a group channel.        | 초대를 거부한 사용자의 기기를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 기기. |
| `onUserJoined()`                | A user has joined a group channel.                           | 채널에 가입한 사용자의 모든 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치와 초대를 수락하지 않은 사용자의 장치는 제외됩니다. |
| `onUserLeft()`                  | A user has left a group channel.                             | 채널을 떠난 사용자의 모든 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치와 초대를 수락하지 않은 사용자의 장치는 제외됩니다. |
| `onDeliveryReceiptUpdated()`    | A message has been delivered in a group channel.             | 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치와 메시지가 배달된 것으로 표시되고 이 이벤트를 호출한 장치는 제외됩니다. |
| `onReadReceiptUpdated()`        | A user has read a specific unread message in a group channel. | 채널에 초대된 사용자의 모든 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치와 읽지 않은 메시지를 읽은 사용자의 장치는 제외합니다. |
| `onTypingStatusUpdated()`       | A user starts typing a message to a group channel.           | 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치(채널에 초대된 사용자의 장치 및 메시지를 입력한 사용자의 모든 장치 제외). |
| `onThreadInfoUpdated()`         | A reply message has been created or deleted from a thread.   | 메시지 스레드에 있는 모든 사용자의 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치와 회신 메시지를 생성하거나 삭제한 사용자의 장치는 제외합니다. |
| `onUserMuted()`                 | A user has been muted in a group channel.                    | 사용자가 음소거된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onUserUnmuted()`               | A user has been unmuted in a group channel.                  | 사용자가 음소거 해제된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onUserBanned()`                | A user has been banned from a group channel.                 | 사용자가 차단된 장치를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 장치. |
| `onUserUnbanned()`              | A user has been unbanned from a group channel.               | 사용자가 차단 해제된 기기를 포함하여 채널이 있는 클라이언트 앱이 포그라운드에 있는 모든 기기. |
| `onChannelMemberCountChanged()` | One or more group channels' member count has changed.        | 클라이언트 앱의 채널 목록에 영향을 받는 그룹 채널이 있는 연결된 모든 장치. |
