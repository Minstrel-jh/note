[java]: /note/java/README.md
[url:completablefuture_completionstage]: https://my.oschina.net/JackieRiver/blog/2054472
[url:future_completablefuture]: https://www.cnblogs.com/happyliu/archive/2018/08/12/9462703.html
[url:completablefuture]: https://segmentfault.com/a/1190000014479792

# CompletableFuture

[返回][java]

> [Java并发编程(六) CompletableFuture与CompletionStage使用][url:completablefuture_completionstage]  
> [Java并发编程系列一：Future和CompletableFuture解析与使用][url:future_completablefuture]  
> [Java 8 CompletableFuture 教程][url:completablefuture]

## Future接口的局限性

- Future不会通知你已经完成了，除了采用get()和轮询isDone()，无法获取异步执行结果。
- Future很难直接描述多个Future结果之间的依赖性
- 没有提供异常处理

jdk8中新增CompletableFuture实现了Future和CompletionStage接口，提供了许多关于创建，链式调用和组合多个Future的便利方法集，而且有广泛的异常处理支持。

## 创建

### 构造器

```java
// 创建一个未完成的CompletableFuture
CompletableFuture() {}

// 使用给出的参数作为结果创建一个新的CompletableFuture
CompletableFuture(Object r) {
    this.result = r;
}
```

### 创建一个已完成的CompletableFuture

```java
static <U> CompletableFuture<U> completedFuture(U value)
```

### 异步执行任务

```java
// 返回一个新的CompletableFuture，它在运行给定操作后由运行在 ForkJoinPool.commonPool()中的任务 异步完成。
static CompletableFuture<Void> runAsync(Runnable runnable)

// 返回一个新的CompletableFuture，它在运行给定操作之后由在给定执行程序中运行的任务异步完成。
static CompletableFuture<Void> runAsync(Runnable runnable, Executor executor)

// 返回一个新的CompletableFuture，它通过在 ForkJoinPool.commonPool()中运行的任务与通过调用给定的供应商获得的值 异步完成。
static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier)

// 返回一个新的CompletableFuture，由给定执行器中运行的任务异步完成，并通过调用给定的供应商获得的值。
static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier, Executor executor)
```

- runAsync()用于无返回值的异步执行，supplyAsync()用于可获得返回值的异步执行
- 如果不提供指定的executor，则默认使用ForkJoinPool.commonPool()线程池

## CompletetionStage接口提供的特性

### 转化

#### thenApply()

用于转化，消费当前CompletableFuture的结果，并产出一个新结果。

```java
// 返回一个新的CompletionStage，当此阶段正常完成时，将以该阶段的结果作为所提供函数的参数执行。
<U> CompletableFuture<U> thenApply(Function<? super T,? extends U> fn)

// 返回一个新的CompletionStage，当该阶段正常完成时，将使用此阶段的默认异步执行工具执行此阶段的结果作为所提供函数的参数。
<U> CompletableFuture<U> thenApplyAsync(Function<? super T,? extends U> fn)

// 返回一个新的CompletionStage，当此阶段正常完成时，将使用提供的执行程序执行此阶段的结果作为提供函数的参数。
<U> CompletableFuture<U> thenApplyAsync(Function<? super T,? extends U> fn, Executor executor)
```

#### thenCompose()

用于转化，与thenApply()的区别在于传入方法的会返回一个新的CompletionStage。

```java
// 返回一个新的CompletionStage，当这个阶段正常完成时，这个阶段将作为提供函数的参数执行。
<U> CompletableFuture<U> thenCompose(Function<? super T,? extends CompletionStage<U>> fn)

// 返回一个新的CompletionStage，当此阶段正常完成时，将使用此阶段的默认异步执行工具执行，此阶段作为提供的函数的参数。
<U> CompletableFuture<U> thenComposeAsync(Function<? super T,? extends CompletionStage<U>> fn)

// 返回一个新的CompletionStage，当此阶段正常完成时，将使用提供的执行程序执行此阶段的结果作为提供函数的参数。
<U> CompletableFuture<U> thenComposeAsync(Function<? super T,? extends CompletionStage<U>> fn, Executor executor)
```

#### thenCombine()

合并转化，消费两者结果，并产出一个新结果

```java
// 返回一个新的CompletionStage，当这个和另一个给定的阶段都正常完成时，两个结果作为提供函数的参数执行。
<U,V> CompletableFuture<V> thenCombine(CompletionStage<? extends U> other, BiFunction<? super T,? super U,? extends V> fn)

// 返回一个新的CompletionStage，当这个和另一个给定阶段正常完成时，将使用此阶段的默认异步执行工具执行，其中两个结果作为提供函数的参数。
<U,V> CompletableFuture<V> thenCombineAsync(CompletionStage<? extends U> other, BiFunction<? super T,? super U,? extends V> fn)

// 返回一个新的CompletionStage，当这个和另一个给定阶段正常完成时，使用提供的执行器执行，其中两个结果作为提供的函数的参数。
<U,V> CompletableFuture<V> thenCombineAsync(CompletionStage<? extends U> other, BiFunction<? super T,? super U,? extends V> fn, Executor executor)
```

#### applyToEither()

采用最快结束的任务结果作转化

```java
<U> CompletableFuture<U> applyToEither(CompletionStage<? extends T> other, Function<? super T, U> fn)

<U> CompletableFuture<U> applyToEitherAsync(CompletionStage<? extends T> other, Function<? super T, U> fn)

<U> CompletableFuture<U> applyToEitherAsync(CompletionStage<? extends T> other, Function<? super T, U> fn, Executor executor)
```

### 消费

#### thenAccept()

会消费当前CompletableFuture的结果。

```java
// 返回一个新的CompletionStage，当此阶段正常完成时，将以该阶段的结果作为提供的操作的参数执行。
CompletableFuture<Void> thenAccept(Consumer<? super T> action)

// 返回一个新的CompletionStage，当此阶段正常完成时，将使用此阶段的默认异步执行工具执行，此阶段的结果作为提供的操作的参数。
CompletableFuture<Void> thenAcceptAsync(Consumer<? super T> action)

// 返回一个新的CompletionStage，当此阶段正常完成时，将使用提供的执行程序执行此阶段的结果作为提供的操作的参数。
CompletableFuture<Void> thenAcceptAsync(Consumer<? super T> action, Executor executor)
```

#### thenAcceptBoth()

消费两者的结果

```java
// 返回一个新的CompletionStage，当这个和另一个给定的阶段都正常完成时，两个结果作为提供的操作的参数被执行。
<U> CompletableFuture<Void> thenAcceptBoth(CompletionStage<? extends U> other, BiConsumer<? super T,? super U> action)

// 返回一个新的CompletionStage，当这个和另一个给定阶段正常完成时，将使用此阶段的默认异步执行工具执行，其中两个结果作为提供的操作的参数。
<U> CompletableFuture<Void> thenAcceptBothAsync(CompletionStage<? extends U> other, BiConsumer<? super T,? super U> action)

// 返回一个新的CompletionStage，当这个和另一个给定阶段正常完成时，使用提供的执行器执行，其中两个结果作为提供的函数的参数。
<U> CompletableFuture<Void> thenAcceptBothAsync(CompletionStage<? extends U> other, BiConsumer<? super T,? super U> action, Executor executor)
```

#### acceptEither()

采用最快结束的任务结果进行消费

```java
CompletableFuture<Void> acceptEither(CompletionStage<? extends T> other, Consumer<? super T> action)

CompletableFuture<Void> acceptEitherAsync(CompletionStage<? extends T> other, Consumer<? super T> action)

CompletableFuture<Void> acceptEitherAsync(CompletionStage<? extends T> other, Consumer<? super T> action, Executor executor)
```

### 串行执行

#### thenRun()

不依赖当前CompletableFuture的结果，在当前任务结束后执行。

```java
// 返回一个新的CompletionStage，当此阶段正常完成时，执行给定的操作。
CompletableFuture<Void> thenRun(Runnable action)

// 返回一个新的CompletionStage，当此阶段正常完成时，使用此阶段的默认异步执行工具执行给定的操作。
CompletableFuture<Void> thenRunAsync(Runnable action)

// 返回一个新的CompletionStage，当此阶段正常完成时，使用提供的执行程序执行给定的操作。
CompletableFuture<Void> thenRunAsync(Runnable action, Executor executor)
```

#### runAfterBoth()

在两个任务都完成后执行

```java
// 返回一个新的CompletionStage，当这个和另一个给定的阶段都正常完成时，执行给定的动作。
CompletableFuture<Void> runAfterBoth(CompletionStage<?> other, Runnable action)

// 返回一个新的CompletionStage，当这个和另一个给定阶段正常完成时，使用此阶段的默认异步执行工具执行给定的操作。
CompletableFuture<Void> runAfterBothAsync(CompletionStage<?> other, Runnable action)

// 返回一个新CompletionStage，当这和其他特定阶段正常完成，使用附带的执行见执行给定的动作CompletionStage覆盖特殊的完成规则的文档。
CompletableFuture<Void> runAfterBothAsync(CompletionStage<?> other, Runnable action, Executor executor)
```

#### runAfterEither()

任意一个任务完成后开始执行

```java
// 返回一个新的CompletionStage，当这个或另一个给定阶段正常完成时，执行给定的操作。
CompletableFuture<Void> runAfterEither(CompletionStage<?> other, Runnable action)

// 返回一个新的CompletionStage，当这个或另一个给定阶段正常完成时，使用此阶段的默认异步执行工具执行给定的操作。
CompletableFuture<Void> runAfterEitherAsync(CompletionStage<?> other, Runnable action)

// 返回一个新的CompletionStage，当这个或另一个给定阶段正常完成时，使用提供的执行器执行给定的操作。
CompletableFuture<Void> runAfterEitherAsync(CompletionStage<?> other, Runnable action, Executor executor)
```

### 异常处理

#### handle()

带异常处理的转化。与thenApply()的区别在于，thenApply()只可以执行正常的任务，任务出现异常则不执行。

```java
// 返回一个新的CompletionStage，当此阶段正常或异常完成时，将使用此阶段的结果和异常作为所提供函数的参数执行。
<U> CompletableFuture<U> handle(BiFunction<? super T,Throwable,? extends U> fn)

// 返回一个新的CompletionStage，当该阶段完成正常或异常时，将使用此阶段的默认异步执行工具执行，此阶段的结果和异常作为提供函数的参数。
<U> CompletableFuture<U> handleAsync(BiFunction<? super T,Throwable,? extends U> fn)

// 返回一个新的CompletionStage，当此阶段完成正常或异常时，将使用提供的执行程序执行此阶段的结果和异常作为提供的函数的参数。
<U> CompletableFuture<U> handleAsync(BiFunction<? super T,Throwable,? extends U> fn, Executor executor)
```

#### whenComplete()

```java
// 返回与此阶段相同的结果或异常的新的CompletionStage，当此阶段完成时，使用结果（或 null如果没有））和此阶段的异常（或 null如果没有））执行给定的操作。  
CompletableFuture<T> whenComplete(BiConsumer<? super T,? super Throwable> action)

// 返回一个与此阶段相同结果或异常的新CompletionStage，当此阶段完成时，执行给定操作将使用此阶段的默认异步执行工具执行给定操作，结果（或 null如果没有））和异常（或 null如果没有）这个阶段作为参数。
CompletableFuture<T> whenCompleteAsync(BiConsumer<? super T,? super Throwable> action)

// 返回与此阶段相同的结果或异常的新的CompletionStage，当此阶段完成时，使用提供的执行者执行给定的操作，如果没有，则使用结果（或 null如果没有））和异常（或 null如果没有））作为论据。
CompletableFuture<T> whenCompleteAsync(BiConsumer<? super T,? super Throwable> action, Executor executor)
```

#### exceptionally()

```java
CompletableFuture<T> exceptionally(Function<Throwable, ? extends T> fn)
```

### 其他

#### toCompletableFuture()

可以把CompletionStage转化成CompletableFuture

## 自身的特性

### allOf()

```java
// 返回一个新的CompletableFuture，当所有给定的CompletableFutures完成时，完成。
static CompletableFuture<Void> allOf(CompletableFuture<?>... cfs)
```

### anyOf()

```java
// 返回一个新的CompletableFuture，当任何一个给定的CompletableFutures完成时，完成相同的结果。
static CompletableFuture<Object> anyOf(CompletableFuture<?>... cfs)
```

### join()

join()和get()方法都是用来获取CompletableFuture异步之后的返回值，区别在于：

- join()方法抛出的是uncheck异常（即未经检查的异常),不会强制开发者抛出
- get()方法抛出的是经过检查的异常，ExecutionException, InterruptedException 需要用户手动处理（抛出或者 try catch）
