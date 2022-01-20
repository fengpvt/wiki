# CompletableFuture小结

## 一、简单使用
- 运行耗时任务，并获取任务运行状态
```java
CompletableFuture future = CompletableFuture.runAsync(() -> {
    // todo time-consuming task, eg: initialize shared preference
});
try {
    future.get();
} catch (ExecutionException e) {
    e.printStackTrace();
} catch (InterruptedException e) {
    e.printStackTrace();
}
```
- 运行耗时任务，并在主线程处理运行结果
```java
CompletableFuture.supplyAsync(() -> {
    // todo time-consuming task, eg: get network configration
    return 1;
}).whenCompleteAsync((integer, throwable) -> {
    // todo handle final task result
}, ContextCompat.getMainExecutor(MyApp.getContext()));
```
- 运行存在依赖关系的耗时任务，并处理运行结果
```java
CompletableFuture.supplyAsync(() -> {
    // todo time-consuming task
    return 1;
}).thenApply(integer -> {
    // todo time-consuming task
    return integer * 2;
}).whenComplete((integer, throwable) -> {
    // todo handle final task result
});
```

## 2. CompletableFuture介绍
> A Future that may be explicitly completed (setting its value and status), and may be used as a CompletionStage, supporting dependent functions and actions that trigger upon its completion.</br>
参考Google说明：https://developer.android.com/reference/java/util/concurrent/CompletableFuture.html

```java
public class CompletableFuture<T> implements Future<T>, CompletionStage<T>
```

在Android24上新增的用于异步任务处理的工具，同时具有Future和CompletionStage的特性
- 作为Future，支持异步任务的状态判断以及设置
- 作为CompletionStage，可以链式的完成异步任务执行完成后的操作设置

### 2.1 CompletionStage
| id | method | param | async | alies | desc |
| --- | --- | --- | --- | --- | --- |
| 1 | thenApply | Function | Y | - | A -> C |
| 2 | thenAccept | Consumer | Y | - | A -> void |
| 3 | thenRun | Runnable | Y | - | void -> void |
| 4 | thenCombine | CompletionStage, BiFunction | Y | thenApplyBoth | A&B -> C |
| 5 | thenAcceptBoth | CompletionStage, BiConsumer | Y | - | A&B -> void |
| 6 | runAfterBoth | CompletionStage, Runable | Y | thenRunBoth | void -> void |
| 7 | applyToEither | CompletionStage, Function | Y | thenApplyEither | A\|B -> C |
| 8 | acceptEither | CompletionStage, Consumer | Y | thenAcceptEither | A\|B -> void |
| 9 | runAfterEither | CompletionStage, Runable |Y | thenRunEither | void -> void |
| 10 | thenCompose | Function | Y | - | A -> Stage |
| 11 | handle | BiFunction | Y | - | A\|E -> C |
| 12 | whenComplete | BiConsumer | Y | - | A\|E -> void |
| 13 | exceptionally | Function | N | - | E -> C |

### 2.2 ForkJoinPool
实现了Future和CompletionStage接口之后，已经算是实现了作为异步任务处理工具的主要功能了，为了便于开发，CompletableFuture还提供了默认的Executor：ForkJoinPool，针对async方法且不指定Executor的话，默认由ForkJoinPool作为异步任务运行线程池。



