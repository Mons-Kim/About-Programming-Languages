# Autorelease Pool

---

- 해당 공간에 저장된 객체들은 오토릴리즈 풀이 해제될 때 release 메시지를 전달받음. 객체가 추가된 횟수만큼 release 메시지를 받음

- 오토릴리즈 풀이 생성되지 않은 상태로 객체에 autorelease 메시지를 보내면, release 메시지를 받지 못해 메모리 누수의 원인이 됨

- Xcode로 생성한 모든 프로젝트는 메인 스레드에서 동작하는 기본 오토릴리즈 풀을 제공

- 모든 스레드는 오토릴리즈 풀 스택을 가지고 있고, 새롭게 생성된 오토릴리즈 풀은 스택 최상위에 추가됨

- autorelease 메시지를 받은 객체는 스레드에 존재하는 오토릴리즈 풀 중 최상위의 풀에 추가됨

- 오토릴리즈 풀이 해제되는 경우 스택에서 제거

- 스레드 자체가 종료되는 경우에는 스택에 있는 모든 풀이 자동으로 해제

  ```swift
  autoreleasepool {
    	// ...
  }
  ```

- 개발자가 오토릴리즈 풀을 직접 선언해야 하는 경우

  - 명령줄 도구(Command Line Tool)와 같이 UI 프레임워크를 사용하지 않는 프로그램을 개발할 때
  - 반복문에서 다수의 임시 객체를 생성할 때
  - 스레드를 직접 생성할 때

