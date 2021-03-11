## Exposing subjects
You’d like to add a PublishSubject to expose the properties of a view controller , but you don’t want
the subject publicly accessible, as that would allow other classes to call onNext(_) and
make the subject emit values. You might want to do that elsewhere, but not in this case.

private let selectedPhotosSubject = PublishSubject<UIImage>()
var selectedPhotos: Observable<UIImage> {
 return selectedPhotosSubject.asObservable()
}
Here, you define both a private PublishSubject that will emit the selected photos and a
public property named selectedPhotos that exposes the subject’s observable.
Subscribing to this property is how the a controller can observe the photo sequence,
without being able to interfere with it.
 
## Disposing 
- root view controller will not dispose when observing since it will always be on stack so we have to manually complete it form some other view controller when view did disspaoer valled
- otherwise just adding in dispose bag and and as soon as view controller disspaoers from stack that will dispose it 

## TRaits
-You can convert an observable sequence to a completable by using the
ignoreElements() operator, in which case all next events will be ignored, with only a
completed or error event emitted, just as required for a Completable.

-you can either create a Maybe directly by using Maybe.create({ ...
}) or by converting any observable sequence via .asMaybe().

- you can subscribe to any observable and use .asSingle() to convert it
to a Single
