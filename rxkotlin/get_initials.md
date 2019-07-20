### Get initials

```kotlin
Observable.fromArray("Some Name", "Some Other Name")
        .map { it.split(' ') }
        .flatMap { names ->
            Observable.fromIterable(names)
                    .filter { it.isNotEmpty() }
                    .takeLast(2)
                    .reduce("", { acc: String, element: String ->
                        "$acc${element[0]}"
                    })
                    .map { it.toUpperCase() }
                    .filter { it.isNotEmpty() }
                    .toObservable()
        }
        .subscribeBy(
                onNext = {
                    println(it) //SN, ON
                }
        )
```