# Automatic Reference Counting(ARC)

---

[1. Strong Reference](#1. Strong Reference)

[2. Weak Reference](#2. Weak Reference)

[3. Unowned Reference](#3. Unowned Reference)

[4. 클로저의 강한 참조](#4. 클로저의 강한 참조)

---

- MRR(Manual Retain Release) 모델에서 지적되었던 여러 단점을 개선한 메모리 관리 모델

- 컴파일러가 코드를 분석하여 객체의 생명 주기에 적합한 메모리 관리 코드를 추가하는 방식을 사용
- ARC는 가비지컬렉션(GC: Garbage Collection)과 유사하게 개발자가 직접 메모리를 관리하지 않음. 그러나 GC가 런타임 중 주기적으로 메모리를 정리한다면, ARC는 컴파일 시점에 관리코드가 자동 추가되기에 런타임시 오버헤드가 발생하지 않음
- ARC는 객체를 생성할 때마다 객체에 대한 정보를 별도의 메모리 공간에 저장하고, 이 정보를 기반으로 메모리를 관리함

---

### 1. Strong Reference

- 참조 카운트를 증가시키는 형태의 reference

  ```swift
  class Person {
    	var name = "John Doe"
    
    	deinit {
        	println("\(name) is being deinitialized")
      }
  }
  
  var person1: Person?
  var person2: Person?
  var person3: Person?
  
  person1 = Person()
  ```

  - person1변수는 생성된 Person객체에 강한 참조 유지. Person 객체의 참조 카운트는 1

  ```swift
  person2 = person1
  person3 = person1
  ```

  - person2, person3 변수에 person1 변수를 할당하면, 두 변수도 강한 참조 유지. Person 객체의 참조 카운트는 3

  ```swift
  person1 = nil
  person2 = nil
  ```

  - nil을 할당하면 객체에 대한 강한 참조 해제. MRR모델에서 release메시지 보내는 것과 동일. Person 객체의 참조 카운트는 1

  ```swift
  person3 = nil
  ```

  - Person 객체에 대한 모든 강한 참조가 해제되어 참조 카운트가 0이 되었기에 메모리에서 해제됨



- Strong Reference Cycle

  ```swift
  class Person {
    	var name = "John Doe"
    
    	deinit {
        	println("\(name) is being deinitialized")
      }
  }
  
  class Car {
    	var model: String
    	var lessee: Person?
    
    	init(model: String) {
        	self.model = model
      }
    
    	deinit {
        	println("\(model) is being deinitialized")
      }
  }
  
  var person: Person? = Person()
  var rentedCar: Car? = Car(model: "Porsche 911")
  ```

  - person변수와 rentedCar변수에 의해 강한 참조 유지. Person객체와 Car객체의 참조 카운트는 1

  ```swift
  person!.car = rentedCar
  rentedCar!.lessee = person
  ```

  - person의 car속성에 rentedCar를 할당, rentedCar의 lessee속성에 person을 할당함으로써 강한 참조 유지. 
  - Person객체와 Car객체의 참조 카운트는 2

  ```swift
  person = nil
  rentedCar = nil
  ```

  - 각각 nil을 할당하더라도 person의 car속성과 rentedCar의 lessee속성의 강한 참조가 남아 Person객체와 Car객체의 참조 카운트는 1이 되어 메모리가 해제되지 않음
  - 해당 변수는 이미 nil이 할당되어 속성에 접근 불가
  - 강한 참조 해제가 불가능하여 두 객체는 불필요한 메모리를 유지하게 됨
  - 이처럼 객체 사이의 강한 참조로 인해 메모리가 정상적으로 해제되지 않는 문제를 강한 참조 사이클(Strong Reference Cycle), 또는 참조 사이클이라 함
  - ARC는 CG와 달리 참조 사이클 문제를 스스로 처리하지 못함



### 2. Weak Reference

- 참조 카운트가 증가하지 않는 참조

- 초기에는 자신이 참조하는 객체가 해제되는 경우 댕글링 포인터(Dangling Pointer)가 되는 문제 발생

- Zeroing Weak Reference 도입 - 해당 참조는 참조하는 객체가 해제될 때 nil로 초기화함

- weak 선언시, 반드시 옵셔널로 선언해야 함 (참조 객체 해재시, nil이 될 수 있음)

  ```swift
  class Car {
    	var model: String
    	weak var lessee: Person?
    	// ...
  }
  
  var person: Person? = Person()
  var rentedCar: Car? = Car(model: "Porsche 911")
  
  person!.car = rentedCar
  rentedCar!.lessee = person
  ```

  - Person 객체에 대한 강한 참조는 person변수. 참조 카운트는 1
  - Car 객체에 대한 강한 참조는 rentedCar 변수와 Person객체의 car속성. 참조 카운트 1

  ```swift
  person = nil
  // John Doe is being deinitialized
  ```

  - Person 객체의 강한 참조가 모두 해제되어 메모리도 해제됨
  - Person객체의 car속성의 소유권 포기로 Car 객체의 참조 카운트 1

  ```swift
  rentedCar = nil
  // Porsche 911 is being deinitialized
  ```

  - Car 객체의 강한 참조 모두 해제되어 메모리도 해제



### 3. Unowned Reference

- 참조 대상에 대한 강한 참조를 유지하지 않지만 항상 유효한 대상을 참조

- 약한 참조와 달리 옵셔널로 선언되지 않고 nil값을 가질 수 없음

- 참조 대상이 해제되는 경우 자동으로 nil로 변경되지 않으므로 런타임 오류 발생 가능

- 비소유 참조(Unowned Reference)는 참조 대상이 항상 존재한다고 확신할 수 있는 경우에만 제한적으로 사용

- 서로 참조해야 하는 경우, 한쪽을 비소유 참조로 선언

  ```swift
  class Person {
    	var name = "John Doe"
    	var fitnessMembership: Membership?
    
    	deinit {
        	println("\(name) is being deinitialized")
      }
  }
  
  class Membership {
    	let membershipId: String
    	unowned var owner: Person
    
    	init(owner: Person) {
        	self.owner = owner
        	self.membershipId = "20160001"
      }
    
    	deinit {
        	println("\(membershipId) membership is being deinitialized")
      }
  }
  ```

  - 사람은 멤버십의 보유 여부를 결정할 수 있으므로 fitnessMembership속성은 옵셔널로 선언
  - 모든 멤버십은 한 명의 회원과 연관되어야 하기 때문에 owner속성을 unowned 키워드를 통해 비소유 참조로 선언하여 참조 사이클 문제 해결

  ```swift
  var p: Person? = Person()
  p!.fitnessMembership = Membership(owner: p!)
  ```

  - p변수에 Person객체의 강한 참조 유지
  - p변수의 fitnessMembership속성에 Membership 객체의 강한 참조 유지

  ```swift
  p = nil
  // John Doe is being deinitialized
  // 20160001 membership is being deinitialized
  ```

  - p변수에 nil을 할당하여 Person객체애 대한 강한 참조를 해제하여 소멸자 호출
  - Person객체가 해제됨에 따라 fitnessMembership속성의 강한 참조도 해제되어 membership객체도 소멸자 호출





### 4. 클로저의 강한 참조







