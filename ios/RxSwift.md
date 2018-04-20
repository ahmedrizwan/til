# RxSwift

https://github.com/ReactiveX/RxSwift

### Model
```swift
class Repository: Mappable {
    var identifier: Int!
    var language: String!
    var url: String!
    var name: String!
    
    required init?(map: Map) { }
    
    func mapping(map: Map) {
        identifier <- map["id"]
        language <- map["language"]
        url <- map["url"]
        name <- map["name"]
    }
}
```

### Observables

```swift
var nameObservable: Observable<String> {
    return searchBar.rx.text
        .filter { $0 != nil }
        .map { $0! }
        .filter { $0.count > 0 }
        .debounce(1, scheduler: MainScheduler.instance)
        .distinctUntilChanged()
}
```

```swift
func fetchRepositories() -> Observable<[Repository]> {
   return nameObservable
            .do(onNext: { response in
                UIApplication.shared.isNetworkActivityIndicatorVisible = true
            })
            .flatMapLatest { text in
                return RxAlamofire
                    .requestJSON(.get, "https://api.github.com/users/\(text)/repos")
                    .catchError({ (error) -> Observable<(HTTPURLResponse, Any)> in
                        // Handle error here!
                        UIApplication.shared.isNetworkActivityIndicatorVisible = false
                        return Observable.empty()
                    })
            }
            .map { (response, json) -> [Repository] in // again back to .background, map objects
                if let repos = Mapper<Repository>().mapArray(JSONObject: json) {
                    return repos
                } else {
                    return []
                }
            }
            .do(onNext: { response in
                UIApplication.shared.isNetworkActivityIndicatorVisible = false
            })
            .subscribeOn(ConcurrentDispatchQueueScheduler(qos: .background))
            .observeOn(MainScheduler.instance)
}
```

### Subscribers
```swift 
repositoryObservable
    .subscribe(onNext: { (repos) in
        if repos.count == 0 {
            let alert = UIAlertController(title: "Oh!", message: "No repositories for this user.", preferredStyle: .alert)
            alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
            if self.navigationController?.visibleViewController is UIAlertController != true {
                self.present(alert, animated: true, completion: nil)
            }
        }
    })
    .addDisposableTo(disposeBag)
```

```swift
repositoryObservable
    .bindTo(tableView.rx.items(cellIdentifier: "repositoryCell")) { _, repository, cell in
        cell.textLabel?.text = repository.name
    }
    .addDisposableTo(disposeBag)
```