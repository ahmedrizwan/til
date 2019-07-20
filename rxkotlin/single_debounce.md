Scenario → There’s a need to add a small delay between the time response is returned and the time it’s displayed on screen. For example, showing a loader for at least 500ms, if the response is retrieved before 500ms, hold it, and if not then immediately show it.

```kotlin
// 500ms time window
val timer = Observable.timer(500, TimeUnit.MILLISECONDS)

// network operation observable 
val networkObservable = Observable
        .fromPublisher<String> {
            Thread.sleep(100)
            it.onNext("Some Response!")
        }

// using the zipWith operator, debounce networkObservable response for the first 500ms
timer.zipWith(networkObservable, BiFunction { _: Long, networkResponse: String -> networkResponse })
     .blockingSubscribeBy {
        println(it)
     }
```