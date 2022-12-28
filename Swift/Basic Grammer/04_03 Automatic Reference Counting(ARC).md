# Automatic Reference Counting(ARC)

---

[1. Strong Reference](#1. Strong Reference)





---



- MRR(Manual Retain Release) 모델에서 지적되었던 여러 단점을 개선한 메모리 관리 모델
- 컴파일러가 코드를 분석하여 객체의 생명 주기에 적합한 메모리 관리 코드를 추가하는 방식을 사용
- ARC는 가비지컬렉션(GC: Garbage Collection)과 유사하게 개발자가 직접 메모리를 관리하지 않음. 그러나 GC가 런타임 중 주기적으로 메모리를 정리한다면, ARC는 컴파일 시점에 관리코드가 자동 추가되기에 런타임시 오버헤드가 발생하지 않음
- ARC는 객체를 생성할 때마다 객체에 대한 정보를 별도의 메모리 공간에 저장하고, 이 정보를 기반으로 메모리를 관리함

---

### 1. Strong Reference





### 2. Weak Reference





### 3. Unowned Reference







### 4. 클로저의 강한 참조







