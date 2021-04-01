# Transforming elements
* Observables emit elements individually, but you will frequently want to work with collections, such as when you’re binding an observable to a table or collection view, which you’ll learn how to do later in the book. A convenient way to transform an observable of individual elements into an array of all those elements is by using toArray.

# Transforming inner observables
struct Student {
  let score: BehaviorSubject<Int>
}
 
 
Student is structure that has a score property that is a BehaviorSubject<Int>. RxSwift includes a few operators in the flatMap family that allow you to reach into an observable and work with its observable properties. You’re going to learn how to use the two most common ones here.
 
 flatMap projects and transforms an observable value of an observable, and then flattens it down to a target observable
 -creates a new observale for every property of observable and the flattens it down to target obervabe which is sused by subscriiers
 
 This is because flatMap keeps up with each and every observable it creates, one for each element added onto the source observable. Now change charlotte’s score by adding the following code, just to verify that both observables are being monitored and changes projected
 
 To recap, flatMap keeps projecting changes from each observable. There will be times when you want this behavior. And there will be times when you only want to keep up with the latest element in the source observable. So what do you think the name of the flatMap operator would be that only keeps up with the latest element
 
 
 flatMapLatest works just like flatMap to reach into an observable element to access its observable property and project it onto a new sequence for each element of the source observable. Those elements are flattened down into a target observable that will provide elements to the subscriber. What makes flatMapLatest different is that it will automatically switch to the latest observable and unsubscribe from the the previous one
 
