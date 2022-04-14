# 개요

- 이론적으로 숙지할 내용 기록



## 채널

- 인원수에 따라, 기능에 따라 구분됨
- 타입
  - 그룹 채널 - 개인적인 상담, 소규모 회의 등의 인가된 사람들만을 위한 공간
  - 슈퍼그룹 채널 - 그룹채널의 확장형, 중간규모의 회의에 주로 사용됨.
  - 오픈 채널 - 스트리밍과 같은 라이브 이벤트, 누구나 참여할 수 있는 대규모 토론방..
- 타입별 권한은 아래 링크 참조
  - https://sendbird.com/docs/chat/v3/android/guides/channel-types#2-open-channel-vs-group-channel-vs-supergroup-channel-3-features
- 그룹 채널 옵션
  - Public - 인원 제한수까지 초대없이 참가 가능. 기본값은 false, 초대없이 참가 불가
  - Distinct - 새로운 그룹채널 생성시, 동일한 멤버 조합의 채널이 있을 경우, 이전 채널을 재개할지 새로운 채널을 생성할지 결정. 기본값은 false, 새로운 채널 생성함.
  - Supergroup - 채널 생성시, 슈퍼그룹으로 확장할 것인지 선택. 선택할 경우, Distinct옵션 사용 불가. 기본값 false
  - Ephemeral - 임시 채널로 생성할 지 선택. 선택할 경우, Sendbird의 데이터베이스에 저장되지 않음. 새로운 메세지가 오래된 메세지를 밀어낸 경우, 다시 확인 불가. 기본값 false. 
