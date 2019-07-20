### Parallel Observables
```kotlin
val networkObservable1 = Observable
        .fromPublisher<String> {
            println("Starting Observable 1")
            Thread.sleep(4000) 
            it.onNext("Response 1")
        }.map {
            // process response (if needed)
            println(it)
            it
        }.subscribeOn(Schedulers.io()) // specify thread

val networkObservable2 = Observable
        .fromPublisher<String> {
            println("Starting Observable 2")
            Thread.sleep(1000)
            it.onNext("Response 2")
        }.map {
            println(it)
            it
        }
        .subscribeOn(Schedulers.io())

val networkObservable3 = Observable
        .fromPublisher<String> {
            println("Starting Observable 3")
            Thread.sleep(2000)
            it.onNext("Response 3")
        }.map {
            println(it)
            it
        }
        .subscribeOn(Schedulers.io())

val observables = arrayListOf(networkObservable1, networkObservable2, networkObservable3)

Observable.zip(observables) { responsesArray ->
    responsesArray.toCollection(ArrayList())
}.blockingSubscribe {
    // this block executes when all observables are done
    println(it)
}
```