## 多线程调用不同的服务接口, 同时聚合结果

### 前提
>开发过程中有一个接口依次调用了五个业务逻辑代码, 导致该接口响应时间在8s以上, 所以考虑优化并行进行
> 了解多线程的各种创建方式, 并最后决定使用CompletableFuture方法, 因为该方法是java1.8后引入的更方便的工具类

### 了解多线程的创建方式 :

#### 实现Runnable接口
#### 继承Thread类
#### 实现Callable接口
#### 使用CompletableFuture

### 样例代码
```java

            // asyncPromiseExecutor是定义好的线程池对象类, 并且引入后
            CompletableFuture<List<T>> future1 = CompletableFuture.supplyAsync(() -> {
						return getDemo1List(batcheList);
					}, asyncPromiseExecutor
			);

			CompletableFuture<Map<String, List<T>>> future2 = CompletableFuture.supplyAsync(() -> {
						return getDemo2List(batcheList);
					}, asyncPromiseExecutor

			);
			CompletableFuture<Map<String, Map>> future3 = CompletableFuture.supplyAsync(() -> {
						return getDemo3Map(batcheList);
					}, asyncPromiseExecutor
			);
			CompletableFuture<Map<String, List<T>>> future4 = CompletableFuture.supplyAsync(() -> {
						return getDemo4Map(batcheList);
					}, asyncPromiseExecutor
			);

			CompletableFuture<Map<String, List<T>>> future5 = CompletableFuture.supplyAsync(() -> {
						return getDemo5Map(batcheList);
					}, asyncPromiseExecutor
			);

        // 调用join方法并进行等待所有线程执行完毕
        CompletableFuture.allOf(future1, future2, future3, future4, future5).join();
        // 获取所有接口的返回结果
        List<T> result1 = future1.get();
        Map<String, List<T>> result2 = future2.get();
        Map<String, Map> result3 = future3.get();
        Map<String, List<T>> result4 = future4.get();
        Map<String, List<T>> result5 = future5.get();


        //对5个接口的数据进行处理
        for (String ajbh : batcheList) {
            //数据处理
        }
    
        private List<T> getDemo1List(List<String> batcheList) {
            //业务逻辑
        }
        
        private Map<String, List<T>> getDemo2List(List<String> batcheList) {
            //业务逻辑
        }
        
        ......

```

### 底层原理

CompletableFuture 是 Java 8 新增的一个类，它是实现异步编程的一种方式。它的底层原理确实涉及到 Callable 和 Future。

具体来说，CompletableFuture 类继承了 Future 接口，因此它也具有 Future 接口的特性。同时，CompletableFuture 类也实现了 CompletionStage 接口，这个接口定义了很多用于处理异步计算结果的方法，例如 thenApply、thenAccept、thenCompose 等等。

在 CompletableFuture 中，我们可以使用 supplyAsync 和 runAsync 方法来创建一个 CompletableFuture 实例，并在其中传入一个 Callable 或 Runnable 对象，这些对象会被异步执行。在执行完毕后，CompletableFuture 实例中就会保存着异步执行的结果或异常。

通过使用 CompletableFuture 类提供的一系列方法，我们可以方便地对异步计算结果进行处理。例如，我们可以使用 thenApply 方法来对异步计算结果进行转换，或者使用 thenAccept 方法来处理计算结果，还可以使用 thenCompose 方法将多个 CompletableFuture 实例组合起来进行串行计算等等。这些方法的底层实现就是基于 Callable 和 Future 等接口来完成的。
