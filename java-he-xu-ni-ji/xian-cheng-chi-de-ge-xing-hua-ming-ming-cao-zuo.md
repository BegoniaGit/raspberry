# 线程池的个性化命名操作

```java
private ExecutorService executor = new ThreadPoolExecutor(2, 2,
            60L, TimeUnit.SECONDS,
            new ArrayBlockingQueue(200000),
            r -> {
                Thread thread = new Thread(r);
                thread.setName("http-server-thread");
                return thread;
            });
```

