# Webhooks



- 오픈 채널 이벤트

| Event                                                        | Invoked when       |
| :----------------------------------------------------------- | :----------------- |
| [open_channel:create](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/open-channel#2-open_channel-create) | 채널 생성          |
| [open_channel:remove](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/open-channel#2-open_channel-remove) | 채널 제거          |
| [open_channel:enter](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/open-channel#2-open_channel-enter) | 채널에 유저 입장   |
| [open_channel:exit](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/open-channel#2-open_channel-exit) | 채널에서 유저 나감 |
| [open_channel:message_send](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/open-channel#2-open_channel-message_send) | 메세지 전송        |
| [open_channel:message_update](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/open-channel#2-open_channel-message_update) | 메세지 업데이트    |
| [open_channel:message_delete](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/open-channel#2-open_channel-message_delete) | 메세지 삭제        |

- 그룹 채널 이벤트

| Event                                                        | Invoked when                                   |
| :----------------------------------------------------------- | :--------------------------------------------- |
| [group_channel:create](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-create) | 채널 생성                                      |
| [group_channel:changed](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-changed) | 채널 정보 변경                                 |
| [group_channel:remove](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-remove) | 채널 제거                                      |
| [group_channel:invite](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-invite) | 다른 유저 초대                                 |
| [group_channel:decline_invite](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-decline_invite) | 유저가 초대를 거부                             |
| [group_channel:join](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-join) | 유저가 채널에 참여                             |
| [group_channel:leave](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-leave) | 유저가 채널에서 나감                           |
| [group_channel:message_send](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-message_send) | 메세지 전송                                    |
| [group_channel:message_read](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-message_read) | 유저가 채널에서 더이상 읽지 않은 메세지가 없음 |
| [group_channel:message_update](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-message_update) | 메세지 업데이트                                |
| [group_channel:message_delete](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-message_delete) | 메세지 삭제                                    |
| [group_channel:freeze_unfreeze](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-freeze_unfreeze) | 채널 고정/해제                                 |
| [group_channel:reaction_add](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-reaction_add) | 유저가 메세지에 반응 추가                      |
| [group_channel:reaction_delete](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/group-channel#2-group_channel-reaction_delete) | 유저가 메세지에 추가한 반응 삭제               |

- 유저 차단/해제 이벤트

| Event                                                        | Invoked when               |
| :----------------------------------------------------------- | :------------------------- |
| [user:block](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/user#2-user-block) | 유저가 다른 유저 차단      |
| [user:unblock](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/user#2-user-unblock) | 유저가 다른 유저 차단 해제 |

- 운영자 등록/해제 이벤트

| Event                                                        | Invoked when                                          |
| :----------------------------------------------------------- | :---------------------------------------------------- |
| [operators:register_by_operator](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/operator#2-operators-register_by_operator) | 기존 운영자의 앱에서 한 명이상의 운영자가 등록됨      |
| [operators:unregister_by_operator](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/operator#2-operators-unregister_by_operator) | 기존 운영자의 앱에서 한 명이상의 운영자가 등록 해제됨 |

- 신고 기능 이벤트

| Event                                                        | Invoked when               |
| :----------------------------------------------------------- | :------------------------- |
| [message:report](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/report#2-message-report) | 유저에 의한 메세지 신고    |
| [user:report](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/report#2-user-report) | 유저에 의한 유저 신고      |
| [open_channel:report](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/report#2-open_channel-report) | 유저에 의한 오픈 채널 신고 |
| [group_channel:report](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/report#2-group_channel-report) | 유저에 의한 그룹 채널 신고 |

- 경고 이벤트

| Event                                                        | Invoked when                         |
| :----------------------------------------------------------- | :----------------------------------- |
| [alert:user_message_rate_limit_exceeded](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/alert#2-alert-user_message_rate_limit_exceeded) | 유저가 보낼 수 있는 메세지 수를 초과 |

- 메세지 필터링 이벤트

| Event                                                        | Invoked when                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [profanity_filter:replace](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/profanity-filter#2-profanity_filter-replace) | 메세지 내에 명시된 단어가 (*)로 대체됨                       |
| [profanity_filter:block](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/profanity-filter#2-profanity_filter-block) | 명시된 단어를 포함한 메세지가 차단됨                         |
| [profanity_filter:moderate](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/profanity-filter#2-profanity_filter-moderate) | 유저에게 **mute**, **kick**, and **ban** 중 적절한 패널티 하나가 부여됨 |

- 이미지 필터링 이벤트

| Event                                                        | Invoked when                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [image_moderation:block](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/image-moderation#2-image_moderation-block) | 명시된 이미지나 부적절한 이미지 url이 포함된 메세지는 차단됨 |

- 알림 발송 이벤트

| Event                                                        | Invoked when                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [announcement:create_channels](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/announcement#2-announcement-create_channels) | 알림 발신자와 아직 채널이 없는 유저에게 알림을 보내기 위해 채널이 생성 |
| [announcement:send_messages](https://sendbird.com/docs/chat/v3/platform-api/webhook/events/announcement#2-announcement-send_messages) | 알림 메시지가 대상 채널 및 유저에게 전송                     |
