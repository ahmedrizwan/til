### Retry network call

```kotlin
Observable.fromPublisher<String> {
    it.onNext("Doing a network call!")
    Thread.sleep(1000)      // Long running process
    it.onError(Exception()) // Some error thrown
}.retryWhen { errors ->
    errors.zipWith(Observable.range(1, 3)   // Zip error observable with a range one
            .concatMap { retryCount ->
                Observable.timer(retryCount.toLong() * 10, TimeUnit.SECONDS)
            }
    )
}.blockingSubscribeBy(
        onNext = { println(it) },
        onError = { println(it) },
        onComplete = { println("Complete") }
```