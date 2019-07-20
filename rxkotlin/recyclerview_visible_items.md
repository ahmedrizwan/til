### Get initials

```kotlin
RxRecyclerView.scrollEvents(recyclerView)
        .debounce(1, TimeUnit.SECONDS)
        .map {
            (layoutManager.findFirstVisibleItemPosition()..layoutManager.findLastVisibleItemPosition()).map { index ->
               items[index]
            } 
        }.flatMap {
            Observable.fromIterable(it) // list to single item
        }.subscribeBy(onNext = {
                // Item1, Item2... 
        })
```