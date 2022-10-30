

# Spring响应式





## Reactor

* Flux：一个响应式流，产生0个、一个、多个元素
* Mono：一个响应式流，产生0个、一个元素

### 创建
* `<stream> = Flux.just(<str>...)`     生成字符串流
* `<stream> = Flux.fromArray(<array>)`
* `<stream> = Flux.fromIterable(<iter>)`
* `<stream> = Flux.range(start, num)`  生成整数流
* `<strea> = Flux.empty()`             生成空流

* `<strea> = Mono.error(<exception>)`  生成错误流
* `<strea> = Mono.fromCallable()`
* `<strea> = Mono.fromRunnable()`

* `<dispose> = <stream>.subscribe(onNext, onError, onComplete)`  订阅
    * `onNext: data -> { ... }`  处理值
    * `onError: err -> { ... }`  处理错误
    * `onComplete: () -> { ... }` 处理完成信号