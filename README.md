 # SUBJECTS
 You’ve gotten a handle on what an observable is, how to create one, how to subscribe to
it and how to dispose of things when you’re done. Observables are a fundamental part
of RxSwift, but a common need when developing apps is to manually add new values
onto an observable at runtime that will then be emitted to subscribers. What you want
is something that can act as both an observable and as an observer. And that
something is called a Subject.

# What are subjects?
Subjects act as both an observable and an observer. You saw earlier how they can
receive events and also be subscribed to. The subject received .next events, and every
time it receives an event, it turns around and emits it to its subscribers.
There are four subject types in RxSwift, and two relay types that wrap subjects:

• PublishSubject: Starts empty and only emits new elements to subscribers.

• BehaviorSubject: Starts with an initial value and replays it or the latest element to
new subscribers.

• ReplaySubject: Initialized with a buffer size and will maintain a buffer of elements
up to that size and replay it to new subscribers.

• AsyncSubject: Emits only the last .next event in the sequence, and only when the
subject receives a .completed event. This is a seldom used kind of subject, and you
won't use it in this book. It's listed here for the sake of completeness.

• PublishRelay and BehaviorRelay: These wrap their relative subjects, but only
accept .next events. You cannot add a .completed or .error event onto relays at all,
so they're great for non-terminating sequences.

# Working with publish subjects
Publish subjects come in handy when you simply want subscribers to be notified of new
events from the point at which they subscribed, until they either unsubscribe, or the
subject has terminated with a .completed or .error event.

subjects, once terminated, will re-emit its stop event to future subscribers. So
it’s a good idea to include handlers for stop events in your code, not just to be notified
when it terminates, but also in case it is already terminated when you subscribe to it.
This can sometimes be the cause of subtle bugs, so watch out!
You might use a publish subject when you’re modeling time-sensitive data, such as in
an online bidding app. It wouldn’t make sense to alert the user who joined at 10:01 am
that at 9:59 am there was only 1 minute left in the auction. That is, of course, unless
you like 1-star reviews to your bidding app.

# Working with behavior subjects
Behavior subjects work similarly to publish subjects, except they will replay the
latest .next event to new subscribers

Since BehaviorSubject always emits the latest element, you can’t create one
without providing an initial value. If you can’t provide an initial value at creation
time, that probably means you need to use a PublishSubject instead

# Working with replay subjects
Replay subjects will temporarily cache, or buffer, the latest elements they emit, up to a
specified size of your choosing. They will then replay that buffer to new subscribers.
The replay subject is terminated with an error, which it will reemit to new subscribers as you’ve already seen subjects do. But the buffer is also still
hanging around, so it gets replayed to new subscribers as well, before the stop event is
re-emitted.
By explicitly calling dispose() on the replay subject beforehand, new subscribers will
only receive an error event indicating that the subject was already disposed.

# Working with relays
As mentioned earlier, a relay wraps a subject while maintaining its replay behavior.cannot add completion and error events to relays
Unlike other subjects (and observables in general), you add a value onto a relay by using
the accept(_:) method. In other words, you don’t use onNext(_:). This is due to the
fact relays can only accept values, and you cannot add a .error or .completed event
onto them.
A PublishRelay wraps a PublishSubject and a BehaviorRelay wraps a BehaviorSubject.
What sets relays apart from their wrapped subjects is that they are guaranteed to never
terminate.

Remember that publish relays simply wrap a publish subject and work just like them,
except the accept part and that they will not terminate. How about something a little
more interesting

Behavior relays also will not terminate with a .completed or .error event. Because it
wraps a behavior subject, a behavior relay is created with an initial value, and it will
replay its latest or initial value to new subscribers. A behavior relay's special power,
though, is that you can ask it for its current value. This feature bridges the imperative
and reactive worlds in a useful way
As mentioned earlier, behavior relays let you directly access their current value.
