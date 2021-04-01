# flatMap 
is similar to map, but it transforms element of observable to an observable of sequences and flattens them by merging all individual observable sequences into one observale sequence that can be observed by subscribers whenever the value changes.

public struct Person {
  public var car: BehaviorSubject<String>
  
  public init(car: BehaviorSubject<String>) {
    self.car = car
  }
}

let disposeBag = DisposeBag()

let persons = PublishSubject<Person>()

persons
  .flatMap {
    $0.car
  }
  .subscribe(onNext: {
    print($0)
  })
  .disposed(by: disposeBag)

let john = Person(car: BehaviorSubject(value: "Toyota Corolla"))

persons.onNext(john)

let alice = Person(car: BehaviorSubject(value: "Honda Accord"))

persons.onNext(alice)

john.car.onNext("Ford Model T")

// Output:
// Toyota Corolla
// Honda Accord
// Ford Model T
//

//explanation
when we add onnext think of it like a loop . flat map  goes inside the first next and takes its element and makes it an observerable sequence that can be observed . wehn another next comes same happen , the element is made a sequence. both element sequences are merged to make one observable sequence that is montored 

 # flatmaplatest
 flatmaplatest is similar to flatmapp except it  switches its monitoring to the newest observable and ignores emissions from previous observables. rest of the working for the elements of observable remains same 

let disposeBag = DisposeBag()

let persons = PublishSubject<Person>()

persons
  .flatMapLatest {
    $0.car
  }
  .subscribe(onNext: {
    print($0)
  })
  .disposed(by: disposeBag)

let john = Person(car: BehaviorSubject(value: "Toyota Corolla"))

persons.onNext(john)

let alice = Person(car: BehaviorSubject(value: "Honda Accord"))

persons.onNext(alice)

john.car.onNext("Ford Model T")

// Output:
// Toyota Corolla
// Honda Accord
