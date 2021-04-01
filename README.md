# Sharing subscriptions
* The problem is that each time you call subscribe(...), this creates a new Observable for that subscription — and each copy is not guaranteed to be the same as the previous. And even when the Observable does produce the same sequence of elements, it’s overkill to produce those same duplicate elements for each subscription. There’s no point in doing that.
To share a subscription, you can use the share() operator. A common pattern in Rx code is to create several sequences from the same source Observable by filtering out different elements in each of the results.

share (and its specializations via parameters) create a subscription only when the number of subscribers goes from 0 to 1 (i.e., when there isn't a shared subscription already). When a second, third and so on subscribers start observing the sequence, share uses the already created subscription to share with them. If all subscriptions to the shared sequence get disposed (e.g. there are no more subscribers), share will dispose the shared sequence as well. If another subscriber starts observing, share will create a new subscription for it just like described above.

The rule of thumb about sharing operators is that it's safe to use share() with observables that do not complete, or if you guarantee no new subscriptions will be made after completion. If you want piece of mind, use share(replay: 1)
# Completing a subscription after given time interval
It’s a common pattern for messages that don’t necessarily require user input to disappear on their own after a while. In this section, you are going to alter your code so that you show the alert box for a maximum of 5 seconds. If the user doesn’t tap Close themselves within that time limit, you will automatically hide the alert and dispose of the subscription.
take(_:scheduler:) is a filtering operator much like take(1) or takeWhile(...). take(_:scheduler:) takes elements from the source sequence for the given time period. Once the time interval has passed, the resulting sequence completes.

# Using throttle to reduce work on subscriptions with high load
You will be surprised how often you will find yourself in the situation where you need to solve this exact problem: “if there are many incoming elements one after the other, take only the last one.” Since it’s such a common pattern of asynchronous programming, there is a special Rx operator for it.
throttle(_:scheduler:) filters any elements followed by another element within the specified time interval.

So if the user selects a photo and taps another one after 0.2 seconds, throttle will filter the first element out and only let the second one through. This will save you the work to build the first intermediate collage, which will be immediately outdated by the second one.

throttle also works for more than one element that comes in close succession. If the user selects five photos, tapping them quickly one after the other, throttle will filter the first four and let only the 5th element through, as long as there isn’t another element following it in less than 0.5 seconds.

 
