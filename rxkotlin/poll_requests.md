### Poll requests

```kotlin
var count = 0
  Observable.fromPublisher<Int> {  
      count += 1       
      it.onNext(count)
      it.onComplete()               // pretend this is an api request
  }.repeatWhen {
      it.delay(3, TimeUnit.SECONDS) // poll after 3 seconds delay
  }.takeUntil {
      it == 3                       // condition to stop polling
  }.blockingSubscribeBy (
          onNext = { println(it) },             // 1 - (delay) - 2 - (delay) - 3
          onComplete = { println("Complete") }  // called when the condition is fulfilled 
  )
```