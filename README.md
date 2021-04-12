 https://ali-akhtar.medium.com/rxswift-part-3-cad06fb1adf8
https://ali-akhtar.medium.com/rxswift-part-4-d4693ba732c5

# Zip
Another combination operator is the zip family of operators. Like the combineLatest family, it comes in several variants:

To get started, create a Weather enum and a couple of observables:
example(of: "zip") {
  enum Weather {
case cloudy
case sunny }
  let left: Observable<Weather> = Observable.of(.sunny, .cloudy, .cloudy,
.sunny)
  let right = Observable.of("Lisbon", "Copenhagen", "London", "Madrid",
"Vienna")
Then create a zipped observable of both sources. Note that you’re using the zip(_:_:resultSelector:) variant. Use the shorter form as shown below, with the closure after the last parenthesis, for improved readability.
let observable = Observable.zip(left, right) { weather, city in
    return "It's \(weather) in \(city)"
  }
  _ = observable.subscribe(onNext: { value in
    print(value)
}) }
  
Here’s what zip(_:_:resultSelector:) did for you:
• Subscribed to the observables you provided.
• Waited for each to emit a new value.
• Called your closure with both new values.
Did you notice how Vienna didn’t show up in the output? Why is that?
The explanation lies in the way zip operators work. They pair each next value of each observable at the same logical position (1st with 1st, 2nd with 2nd, etc.). This implies that if no next value from one of the inner observables is available at the next logical position (i.e. because it completed, like in the example above), zip won‘t emit anything anymore. This is called indexed sequencing, which is a way to walk sequences in lockstep. But while zip may stop emitting values early, it won‘t itself complete until all its inner observables complete, making sure each can complete its work.

# Triggers

withLatestFrom(_:)
withLatestFrom(_:) is useful in all situations where you want the current (latest) value emitted from an observable, but only when a particular trigger occurs.

sample(_:)
It does nearly the same thing with just one variation: each time the trigger observable emits a value, sample(_:) emits the latest value from the “other” observable, but only if it arrived since the last “tick”. If no new data arrived, sample(_:) won’t emit anything

# Switches
RxSwift comes with two main so-called “switching” operators: amb(_:) and switchLatest(). They both allow you to produce an observable sequence by switching between the events of the combined or source sequences. This allows you to decide which sequence’s events will the subscriber receive at runtime.

amb(_:)

The amb(_:) operator subscribes to the left and right observables. It waits for any of them to emit an element, then unsubscribes from the other one. After that, it only relays elements from the first active observable. It really does draw its name from the term ambiguous: at first, you don’t know which sequence you’re interested in, and want to decide only when one fires.
This operator is often overlooked. It has a few select practical applications, such as connecting to redundant servers and sticking with the one that responds first.
Each time the source observable emits an element, scan(_:accumulator:) invokes your closure. It passes the running value along with the new element, and the closure returns the new accumulated value.
Like reduce(_:_:), the resulting observable type is the closure return type.
switchLatest().
Your subscription only prints items from the latest sequence pushed to the source observable. This is the purpose of switchLatest().

# Combining elements within a sequence
Each time the source observable emits an element, scan(_:accumulator:) invokes your closure. It passes the running value along with the new element, and the closure returns the new accumulated value.
Like reduce(_:_:), the resulting observable type is the closure return type.
