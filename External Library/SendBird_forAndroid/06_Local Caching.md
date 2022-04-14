# Authentication 



### Sendbird SDK 초기화

```java
SendBird.init(String appId, Context context, boolean useCaching, @NonNull final InitResultHandler handler)
```

- 해당 메소드는 Application클래스에서 선언할 것. OnCreate()에서 선언.
- appId : 앱 ID
- Context : getApplicationContext()를 사용
- useCashing : 로컬 캐시를 사용할 지 여부
- InitResultHandler : 로컬 캐시 사용하지 않으면, onInitSucceed()만 호출, 사용시에는 onMigrationStarted()와 onInitFailed()가 호출될 수 있음. 실패시 기존의 캐시를 모두 삭제하고 초기화 다시 시도.



### 로그인

```java
SendBird.connect(String userId, SendBird.ConnectHandler handler)
```

- 변수명은 소문자로 시작이 관례.

- 변수명은 영문자나 "_"로 시작할 수 있다. 숫자나 다른 특수문자로 시작 불가.

- 유니코드로 표현할 수 있는 한글이나 이모티콘도 변수명으로 설정 가능. 가독성은 떨어짐

- 두 개 이상의 단어로 구성된 이름은 <span style="color:red">lowerCamelCase</span> 규칙을 사용.

- 헝가리안 표기법을 사용하지 않음.

- 예약어는 변수명으로 사용할 수 없음.

  - 예약어를 '로 감싸주면 식별자 이름으로 사용할 수 있음

    ```swift
    var 'if' = 123;
    print('if')
    // 123
    ```

    

