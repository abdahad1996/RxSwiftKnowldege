# What is an observable?
Observables are the heart of Rx. You’re going to spend some time discussing what
observables are, how to create them and how to use them.
You’ll see “observable”, “observable sequence” and “sequence” used interchangeably in
Rx. And, really, they’re all the same thing. You may even see an occasional “stream”
thrown around from time to time, especially from developers that come to RxSwift from
a different reactive programming environment. “Stream” also refers to the same thing,
but, in RxSwift, all the cool kids call it a sequence, not a stream.

Observable is just a sequence, with
some special powers. One of these powers — in fact the most important one — is that it
is asynchronous. Observables produce events, the process of which is referred to as
emitting, over a period of time. Events can contain values, such as numbers or
instances of a custom type, or they can be recognized gestures, such as taps.
One of the best ways to conceptualize this is by using marble diagrams, which are just
values plotted on a timeline.
 
 # Lifecycle of an observable
In the previous marble diagram, the observable emitted three elements. When an
observable emits an element, it does so in what’s known as a next event

• An observable emits next events that contain elements.

• It can continue to do this until a terminating event is emitted; e.g. either an error
event, or a completed event.

• Once an observable is terminated, it can no longer emit events.

# Creating observables

- just is aptly named, since all it does is create an observable sequence containing just a
single element. just is a type method on Observable. However, in Rx, methods are
referred to as “operators.”

- The from operator creates an observable of individual elements from an array of typed
elements. Option-click on observable4 and you’ll see that it is an Observable of Int,
not [Int]. The from operator only takes an array.

- If you want to create an observable array, you can simply pass an array to of.

you’ve created observables with specific .next event
elements. Using the create operator is another way to specify all events that an
observable will emit to subscribers.

The create operator takes a single parameter named subscribe. Its job is to provide the
implementation of calling subscribe on the observable. In other words, it defines all
the events that will be emitted to subscribers

The subscribe parameter is an escaping closure that takes an AnyObserver and returns
a Disposable. AnyObserver is a generic type that facilitates adding values onto an
observable sequence, which will then be emitted to subscribers.
Observable<String>.create { observer in
 // 1
 observer.onNext("1")

 // 2
 observer.onCompleted()

 // 3
 observer.onNext("?")

 // 4
 return Disposables.create()
}

Here’s the play-by-play:
1. Add a .next event onto the observer. onNext(_:) is a convenience method for
on(.next(_:)).

2. Add a .completed event onto the observer. Similarly, onCompleted is a convenience
method for on(.completed).

3. Add another .next event onto the observer.

5. Return a disposable.
Note: The last step, returning a Disposable, may seem strange but remember that
the subscribe operators return a disposable representing the subscription. Here,
Disposables.create() is an empty disposable, but some disposables have side
effects

# Creating observable factories
Rather than creating an observable that waits around for subscribers, it’s possible to
create observable factories that vend a new observable to each subscriber.
deferred used


# Using Traits
Traits are observables with a narrower set of behaviors than regular observables. Their
use is optional; you can use a regular observable anywhere you might use a trait
instead. Their purpose is to provide a way to more clearly convey your intent to readers
of your code or consumers of your API. The context implied by using a trait can help
make your code more intuitive.

There are three kinds of traits in RxSwift: Single, Maybe and Completable. Without
knowing anything more about them yet, can you guess how each one is specialized?

- Singles will emit either a .success(value) or .error event. .success(value) is actually
a combination of the .next and .completed events. This is useful for one-time processes
that will either succeed and yield a value or fail, such as downloading data or loading it
from disk.

- A Completable will only emit a .completed or .error event. It doesn't emit any values.
You could use a completable when you only care that an operation completed
successfully or failed, such as a file write.

- Finally, Maybe is a mashup of a Single and Completable. It can either emit
a .success(value), .completed or .error. If you need to implement an operation that
could either succeed or fail, and optionally return a value on success, then Maybe is your
ticket.

# Subscribing to observables
- you call observing an observable subscribing to it
- an observable won’t send events, or perform any work, until it has a
subscriber.
- Subscribing to observables is more streamlined than this, though. You can also add
handlers for each event type an observable can emit. Recall that an observable
emits .next, .error and .completed events. A .next event passes the element being
emitted to the handler, and an .error event contains an error instance.

The observable emits a .next event for each element, then emits a .completed event
and finally is terminated. When working with observables, you’ll usually be more
interested in the elements emitted by .next events, than you will be with the events
themselves.

observable has subscribe operator, and you’ll see that it takes an escaping closure
that takes an Event of type Int and doesn’t return anything, and subscribe returns a
Disposable

Event has an element property. It’s an optional value, because only .next events have an
element. So you use optional binding to unwrap the element if it’s not nil

 what use is an empty observable? Well, they’re handy when you want to return an
observable that immediately terminates or intentionally has zero values.
As opposed to the empty operator, the never operator creates an observable that doesn’t
emit anything and never terminates. 

# Disposing and terminating
Remember that an observable doesn’t do anything until it receives a subscription. It’s
the subscription that triggers an observable's work, which emits new events, up until it
emits an .error or .completed event and is terminated. You can manually cause an
observable to terminate by canceling a subscription to it.

To explicitly cancel a subscription, call dispose() on it. After you cancel the
subscription, or dispose of it, the observable in the current example will stop emitting
events

Managing each subscription individually would be tedious, so RxSwift includes a
DisposeBag type. A dispose bag holds disposables — typically added using
the .disposed(by:) method — and will call dispose() on each one when the dispose
bag is about to be deallocated

