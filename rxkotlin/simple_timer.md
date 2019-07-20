### Timer for 5 seconds

```kotlin
Observable.interval(1, TimeUnit.SECONDS, Schedulers.newThread())
        .take(5)
        .subscribeBy (
                onNext = { //1,2,3,4,5 },
                onComplete = { //Complete }
        )
```