# RxSwiftKnowldege
* RxSwift, in its essence, simplifies developing asynchronous programs by allowing your
   code to react to new data and process it in a sequential, isolated manner
   
   The three building blocks of Rx code are observables, operators and schedulers. The
    sections below cover each of these in detail.

# Observables
The Observable<T> class provides the foundation of Rx code: the ability to
asynchronously produce a sequence of events that can “carry” an immutable snapshot
of generic data of type T. In the simplest words, it allows other objects or consumers to
subscribe for events, or values, emitted by another object over time.
The Observable<T> class allows one or more observers to react to any events in real
time and update the app's UI, or otherwise process and utilize new and incoming data.
The ObservableType protocol (to which Observable<T> conforms) is extremely simple.
An Observable can emit (and observers can receive) only three types of events:
  
• A next event: An event that “carries” the latest (or "next") data value. This is the way
observers “receive” values. An Observable may emit an indefinite amount of these
values, until a terminating event is emitted.

• A completed event: This event terminates the event sequence with success. It means
the Observable completed its life cycle successfully and won’t emit additional events.

• An error event: The Observable terminates with an error and will not emit
additional events.

# Finite observable sequences
Some observable sequences emit zero, one or more values, and, at a later point, either
terminate successfully or terminate with an error.

In an iOS app, consider code that downloads a file from the internet:
• First, you start the download and start observing for incoming data.

• Then you repeatedly receive chunks of data as parts of the file come in.

• In the event the network connection goes down, the download will stop and the
connection will time out with an error.

• Alternatively, if the code downloads all the file’s data, it will complete with success

# Infinite observable sequences
Unlike file downloads or similar activities, which are supposed to terminate either
naturally or forcefully, there are other sequences which are simply infinite. Often, UI
events are such infinite observable sequences.
For example, consider the code you need to react to device orientation changes in your
app:

• You add your class as an observer to UIDeviceOrientationDidChange notifications
from NotificationCenter.

• You then need to provide a method callback to handle orientation changes. It needs
to grab the current orientation from UIDevice and react accordingly to the latest
value.

This sequence of orientation changes does not have a natural end. As long as there is
device, there is a possible sequence of orientation changes. Further, since the sequence
is virtually infinite, you always have an initial value at the time you start observing it.

# Operators
For example, take the mathematical expression: (5 + 6) * 10 - 2.
In a clear, deterministic way, you can apply the operators *, ( ), + and - in their
predefined order to the pieces of data that are their input, take their output and keep
processing the expression until it’s resolved.

In a somewhat similar manner, you can apply Rx operators to the events emitted by an
Observable to deterministically process inputs and outputs until the expression has
been resolved to a final value, which you can then use to cause side effects


# Schedulers
Schedulers are the Rx equivalent of dispatch queues — just on steroids and much easier
to use.
RxSwift comes with a number of predefined schedulers, which cover 99% of use cases
and hopefully means you will never have to go about creating your own scheduler.
In fact, most of the examples in the first half of this book are quite simple and generally
deal with observing data and updating the UI, so you won’t look into schedulers at all
until you’ve covered the basics.
That being said, schedulers are very powerful.
For example, you can specify that you’d like to observe next events on a
SerialDispatchQueueScheduler, which uses Grand Central Dispatch run your code
serially on a given queue.
ConcurrentDispatchQueueScheduler will run your code concurrently, while
OperationQueueScheduler will allow you to schedule your subscriptions on a given
OperationQueue

# App architecture
It’s worth mentioning that RxSwift doesn’t alter your app’s architecture in any way; it
mostly deals with events, asynchronous data sequences and a universal communication
contract.
You can create apps with Rx by implementing MVC (Model-View-Controller)
architecture as defined in the Apple developer documentation. You can also choose to
implement MVP (Model-View-Presenter) architecture or MVVM (Model-ViewViewModel) if that’s what you prefe

The reason MVVM and RxSwift go great together is that a ViewModel allows you to
expose Observable<T> properties, which you can bind directly to UIKit controls in your
View controller's glue code. This makes binding model data to the UI very simple to
represent and to code:
