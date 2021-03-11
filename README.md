## Filtering
 # Ignoring operators
- ignoreElements will do : ignore .next event elements. It will,
however, allow stop events through, such as .completed or .error events
- There may be times when you only want to handle the nth (ordinal) element emitted by
an observable, such as the third strike. For that you can use elementAt, which takes the
index of the element you want to receive, and it ignores everything else
An interesting fact about element(at:) is, that as soon as an element is emitted at the
provided index, the subscription will be terminated.
-filter. It takes a predicate closure,
which it applies to every element emitted, allowing through only those elements for
which the predicate resolves to true.
 # Skipping operators
- It might be that you need to skip a certain number of elements. The skip operator
allows you to ignore from the 1st to the number you pass as its parameter
- skipWhile, returning true will cause the element to be skipped, and returning
false will let it through.and after that wverything else will fallthrough and not skip as predicate already failed once
-  skipUntil will keep skipping elements from the source observable
(the one you’re subscribing to) until some other trigger observable emits. In this marble
diagram, skipUntil ignores elements emitted by the source observable (the top line)
until the trigger observable (second line) emits a .next event. Then it stops skipping
and lets everything through from that point on.
# Taking operators
-Taking is the opposite of skipping. When you want to take elements, RxSwift has you
covered. The first taking operator you’ll learn about is take which will take elemnets till the paramter sepcified and will
skipp the rest
-takeWhile operator that works similarly to skipWhile, except you’re
taking instead of skipping . you only receive elements as long as the conditions is true 
-takeUntil operator, shown in this marble diagram, taking
from the source observable until the trigger observable emits an element.
 trigger  will cause takeUntil to stop taking once it
emits
# Distinct operators
-distinctUntilChanged only prevents duplicates that are right
next to each other.use distinctUntilChanged to prevent sequential duplicates from getting through.
-distinctUntilChanged(_:), which takes a closure that receives each sequential
pair of elements and print out elements that are considered distinct based on the
comparing logic you provided.
