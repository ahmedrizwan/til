# RxAndroid

https://github.com/ReactiveX/RxAndroid

### Model
```kotlin
class Repository(val identifier: Int,
                 val language: String,
                 val url: String,
                 val name: String)
```

### Observables

```kotlin
val nameObservable = RxTextView.afterTextChangeEvents(editTextUser)
                .skipInitialValue()
                .map {
                    it.view().text.toString()
                }
                .filter { it.isNotEmpty() }
                .debounce(1, TimeUnit.SECONDS, AndroidSchedulers.mainThread())
                .doOnNext { progressBar.visibility = View.VISIBLE }
```

```kotlin
fun fetchRepositories(): Observable<List<Repository>> {
    return nameObservable
            .flatMap { name ->
                githubService.getRepos(name)
                        .onErrorResumeNext { throwable: Throwable ->
                            // Handle error here!
                            Observable.just(listOf())
                        }
                        .subscribeOn(Schedulers.newThread())
            }
            .observeOn(AndroidSchedulers.mainThread())
}
```

### Subscribers

```kotlin
repositoriesObservable
                .subscribeBy(onNext = { repositories ->
                    progressBar.visibility = View.GONE
                    reposAdapter.updateDataSet(repositories)
                    if (it.isEmpty()) {
                        errorDialog.show()
                    }
                })
```
