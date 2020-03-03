https://www.callicoder.com/java-concurrency-multithreading-basics/

# Thread
## 1. Extending Thread
The Thread class itself implements Runnable.
```
public class ThreadExample extends Thread {

    @Override
    public void run() { ... }

    public static void main(String[] args) {
        Thread thread = new ThreadExample();
        thread.start();
    }
}
```

## 2. Implementing Runnable
Preferred -> A class can only extend once but implement multiple interfaces.
```
public class RunnableExample {

    public static void main(String[] args) {
        Runnable runnable = new Runnable() {
            @Override
            public void run() { ... }
        };
        Thread thread = new Thread(runnable);
        thread.start();
    }    
}
```
Or with lambda: `Runnable runnable = () -> { ... };`

## Async with Thread
Inside run(): `Thread.sleep(2000);`

Wait for thread: `thread.join()` or `thread.join(1000)` to wait for max. 1 sec

# Executor and Thread Pools
### **Executor**: Interface with execute() to launch a task specified by a Runnable
### **ExecutorService**: Sub-interface of Executor to manage lifecycle of tasks, with submit() to accept both Runnable and Callable
### **ScheduledExecutorService**: Sub-interface of ExecutorService to schedule tasks

```
public class ExecutorsExample {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newSingleThreadExecutor();
        Runnable task1 = () -> { ... };
        executorService.submit(task1);

        // ... executorService.submit(task2);
    }
}
```
**newSingleThreadExecutor()**: uses a single worker thread  
Fixed sized Pool: `Executors.newFixedThreadPool(2)`

Wait for current tasks to execute and shut down: `executorService.shutdown();`  
Interrupt and shut down immediately: `executorService.shutdownNow();`

### Scheduled:
```
ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(1);
...
scheduledExecutorService.schedule(task, 5, TimeUnit.SECONDS);
```

# Callable and Future
## Callable
Similar to Runnable, with method call(), can return a result and throw a checked exception  
`Callable<String> callable = () -> { return "result"; };`

## Future
Submit Callable to an executor service -> returns a Future which can be used to fetch the result of the task  
(Future ~ Promise)

```
public class FutureAndCallableExample {
    public static void main(String[] args) throws InterruptedException, ExecutionException {
        ExecutorService executorService = Executors.newSingleThreadExecutor();
        Callable<String> callable = () -> { ... };
        Future<String> future = executorService.submit(callable);

        // Executes immediately
        doSomething();
        while (!future.isDone()) { ... }

        // Blocks until the result is available
        String result = future.get();
        executorService.shutdown();
    }
}
```

### Cancelling a Future
`future.cancel(true);`  
true: the thread executing the task will be interrupted  
false: in-progress tasks will be allowed to complete

Check if cancelled: `future.isCancelled()`  
After cancellation: `isDone()` is true and `future.get()` throws `CancellationException`

### Adding Timeouts
`future.get(1, TimeUnit.SECONDS);` -> wait max. 1 sec then throw `TimeoutException`

### Wait for multiple tasks
**invokeAll**: future.get() blocks until all Futures are complete  
**invokeAny**: returns the result of the fastest Callable (not return Future)
```
Callable<String> task1 = () -> { ... };
Callable<String> task1 = () -> { ... };
Callable<String> task1 = () -> { ... };

List<Callable<String>> taskList = Arrays.asList(task1, task2, task3);
List<Future<String>> futures = executorService.invokeAll(taskList);

for(Future<String> future: futures) {
    System.out.println(future.get());
}
```

```
String result = executorService.invokeAny(Arrays.asList(task1, task2, task3));
```

# CompletableFuture
https://www.callicoder.com/java-8-completablefuture-tutorial/


implements `Future` and `CompletionStage`, add functionalities to `Future`:  
manually complete, callback funtion, chained Futures, combine multiple Futures, Exception handling

```
CompletableFuture<String> completableFuture = new CompletableFuture<String>();
completableFuture.complete("Future's Result"); // manually complete
String result = completableFuture.get();
```

## runAsync() and supplyAsync()
Run async task without return value: `runAsync(runnable)`
```
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
    try { ... }
    catch (InterruptedException e) { ... }
});

future.get();
```

Run async task with return value: `supplyAsync(supplier)` -> takes `Supplier<T>` and returns `CompletableFuture<T>`  
`Supplier`: has single method get()
```
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> { 
    ...
    return "result";
});

String result = future.get();
```

runAsync() and supplyAsync() execute tasks in a thread obtained from `ForkJoinPool.commonPool()`. They also accept an `Executor` with customized thread pool as second argument: 
```
Executor executor = Executors.newFixedThreadPool(10);
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> { ... }, executor);
```

## Attach callback to CompletableFuture: thenApply(), thenAccept() and thenRun()
`thenApply()`: accepts a CompletableFuture, process it when it arrives and returns new CompletableFuture
```
CompletableFuture<String> whatsYourNameFuture = CompletableFuture.supplyAsync(() -> { 
    ...
    return "A name";
});

CompletableFuture<String> greetingFuture = whatsYourNameFuture.thenApply(name -> {
   return "Hello " + name;
});

// Block and get the result of the future.
System.out.println(greetingFuture.get());
```

Chaining: 
```
CompletableFuture.supplyAsync(() -> { ... })
.thenApply(result -> { ... })
.thenApply(result -> { ... });
```

Without return value: (Run something at the end of a chain etc.)   
`thenAccept()`: takes a CompletableFuture and returns `CompletableFuture<Void>`  
`thenRun()`: takes a Runnable and returns `CompletableFuture<Void>` (doesn't have access to the Future's result)

### Async callback
`thenApplyAsync()`: Execute the following task in another thread from ForkJoinPool.commonPool()  
(`thenApply()` runs in same thread as `supplyAsync()`, or in main thread if `supplyAsync()` task completes immediately)

## Combining multiple CompletableFutures
`thenCompose()`: chain two futures when the `Supplier` itself inside supplyAsync() returns a CompletableFuture  
(In examples above suppliers always return a simple value, only wrapped automatically in a CompletableFuture)

```
CompletableFuture<User> getUsersDetail(String userId) {
    return CompletableFuture.supplyAsync(() -> { // supplier returns CompletableFuture
        return UserService.getUserDetails(userId); // API call returns a simple value
    });	
}

CompletableFuture<Double> getCreditRating(User user) {
	return CompletableFuture.supplyAsync(() -> {
		return CreditRatingService.getCreditRating(user);
	});
}

CompletableFuture<Double> result = getUserDetail(userId).thenCompose(user -> getCreditRating(user));
```

`thenCombine()`: run two futures independently, do something when both are completed
```
CompletableFuture<Double> future1 = CompletableFuture.supplyAsync(() -> { return 25.0; });
CompletableFuture<Double> future2 = CompletableFuture.supplyAsync(() -> { return 37.0; });

CompletableFuture<Double> futureSum = future1.thenCombine(future2, (result1, result2) -> {
    return result1 + result2;
});
```

`allOf()`: run multiple futures independently
```
CompletableFuture<Void> allFutures = CompletableFuture.allOf(futureArray);

CompletableFuture<List<String>> allResults = allFutures.thenApply(v -> {
    return futureArray.map(future -> future.join()).toList();
});
```
`join()` is similar to `get()` but throws exception if the future completes exceptionally.

`anyOf()`: returns the result of future which completes first (with unknown type)
```
CompletableFuture<Object> anyOfFuture = CompletableFuture.anyOf(future1, future2, future3);
```

## Exception Handling
Error in callback chain -> following callbacks will not be called and future will be resolved with exception.

`exceptionally()`: recover from error and return a default value
```
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    throw new IllegalArgumentException("Future with error");
}).exceptionally(e -> {
    System.out.println(e.getMessage());
    return "default";
});

future.get(); 
```

`handle()`: always called whether or not an exception occurs
```
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    throw new IllegalArgumentException("Future with error");
}).handle((res, e) -> {
    if (e != null) return "Error";
    return res;
});

future.get(); 
```
`res` is null when exception occurs, otherwise `e` is null.

# Thread Synchronazation
## synchronized (implicit lock)
`synchronized`: only one thread can enter the method at one time  
(bound to an object, or Class in case of static methods)
```
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count = count + 1;
    }

    public int getCount() {
        return count;
    }
}

for(int i = 0; i < 1000; i++) {
    executorService.submit(() -> counter.increment());
}
```

Or with `synchronized` block: must specify the object
```
public void increment() {
    synchronized (this) { 
        count = count + 1;
    }
}
```
The thread owning the lock can acquire it multiple times.

## volatile
`volatile`: tells the compiler to avoid doing any optimizations to the variable -> avoid memory consistency errors
```
public class VolatileKeywordExample {
    private static volatile boolean sayHello = false;

    public static void main(String[] args) throws InterruptedException {

        Thread thread = new Thread(() -> {
           while(!sayHello) {}

           System.out.println("Hello World!");

           while(sayHello) {}

           System.out.println("Good Bye!");
        });

        thread.start();

        Thread.sleep(1000);
        System.out.println("Say Hello..");
        sayHello = true;

        Thread.sleep(1000);
        System.out.println("Say Bye..");
        sayHello = false;
    }
}
```

## Lock
### ReentrantLock
```
class Counter {
    private final ReentrantLock lock = new ReentrantLock();
    private int count = 0;

    public void increment() {
        lock.lock();
        try {
            count = count + 1;
        } finally {
            lock.unlock();
        }
    }
}
```

### ReadWriteLock
Read lock: held by multiple threads simultaneously when write lock not held  
Write lock: held by only one thread
```
class ReadWriteCounter {
    ReadWriteLock lock = new ReentrantReadWriteLock();
    private int count = 0;

    public int incrementAndGetCount() {
        lock.writeLock().lock();
        try {
            count = count + 1;
            return count;
        } finally {
            lock.writeLock().unlock();
        }
    }

    public int getCount() {
        lock.readLock().lock();
        try {
            return count;
        } finally {
            lock.readLock().unlock();
        }
    }
}
```

## Atomic Variables -> faster than synchronized, preferred
```
class Counter {
    private AtomicInteger count = new AtomicInteger(0);

    public int incrementAndGet() {
        return count.incrementAndGet();
    }

    public int getCount() {
        return count.get();
    }
}
```
