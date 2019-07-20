### Chain requests

```kotlin
Observable.fromPublisher<String> {
    Thread.sleep(1000)           // Long running process
    it.onNext("First Response!") // Response
}.flatMap { response ->
    Observable.fromPublisher<String> {
        println(response)        // Process first response here
        Thread.sleep(1000)       // Long running process
        it.onNext("Final Response!")
    }
}.subscribeBy(
        onNext = {
            println(it)          // Process final response
        }
)
```