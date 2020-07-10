[java]: /note/java/README.md
[url:callable_future_futuretask]: https://www.cnblogs.com/dolphin0520/p/3949310.html
[url:future_completablefuture]: https://www.cnblogs.com/happyliu/archive/2018/08/12/9462703.html
[img:future_mode]: /pic/Future.png

# Callable、Future和FutureTask

[返回][java]

> [Java并发编程：Callable、Future和FutureTask][url:callable_future_futuretask]  
> [Java并发编程系列一：Future和CompletableFuture解析与使用][url:future_completablefuture]

传统的Thread或Runnable的缺陷：在执行任务之后无法获取执行结果。

如果需要获取执行结果，就必须通过共享变量或者使用线程通信的方式，比较麻烦。

jdk5中，提供了Callable和Future，用于获取任务执行完毕后的结果。

## Callable 与 Runnable

Runnable接口中的run()没有返回值，而Callable接口中声明的call()有一个泛型的返回值。

与ExecutorService接口结合使用，ExecutorService接口中声明了若干种submit():

```java
<T> Future<T> submit(Callable<T> task);
<T> Future<T> submit(Runnable task, T result);
Future<?> submit(Runnable task);
```

## Future

Future模式用于构建异步应用，是多线程开发中常见的设计模式。

![avatar][img:future_mode]

Future接口对Runnable或Callable任务进行：取消执行、查询是否完成、获取结果等操作。提供了5个方法：

```java
public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning);
    boolean isCancelled();
    boolean isDone();
    V get() throws InterruptedException, ExecutionException;
    V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException;
}
```

## FutureTask

FutureTask类实现了RunnableFuture接口，RunnableFuture继承了Runnable接口和Future接口。

FutureTask提供了2个构造器：

```java
public FutureTask(Callable<V> callable) {
    if (callable == null)
        throw new NullPointerException();
    this.callable = callable;
    this.state = NEW;       // ensure visibility of callable
}

public FutureTask(Runnable runnable, V result) {
    this.callable = Executors.callable(runnable, result);
    this.state = NEW;       // ensure visibility of callable
}
```
