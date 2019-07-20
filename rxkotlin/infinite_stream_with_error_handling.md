Handle errors and resume an observable in case of any errors.

```kotlin
sealed class Error : Throwable() {
    object UserNotFound : Error()
    object AuthorizationError : Error()
    object UnknownError : Error()
}

typealias UserNotFound = Error.UserNotFound
typealias AuthorizationError = Error.AuthorizationError
typealias UnknownError = Error.UnknownError

// data stream (assume it's an infinite stream)
val usersStream = Observable.fromArray("Deadpool", "Cable", "Domino", "Juggernaut")

// error handling stream 
val errorHandler = ObservableTransformer<String, String> {
    it.flatMap {
        Observable.just(it).map {
            if (it == "Cable") throw AuthorizationError
            if (it == "Domino") throw UserNotFound
            if (it != "Deadpool") throw UnknownError

            it
        }.onErrorResumeNext { error: Throwable ->
            // handle errors here 
            when (error) {
                is UserNotFound -> Observable.just("Who dis?")
                is AuthorizationError -> Observable.just("You shall not pass! :@")
                else -> Observable.just("¯\\_(ツ)_/¯")
            }
        }
    }
}

usersStream.compose(errorHandler)
        .blockingSubscribe {
            println(it)
        }
```