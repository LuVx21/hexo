<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [Semaphore](#semaphore)
- [Usage](#usage)
- [实现原理](#实现原理)
- [阅读](#阅读)

<!-- /TOC -->
</details>

## Semaphore

**作用**

一般用于控制对某组资源的访问权限

**使用场景**

m个银行客户到n个窗口的银行办理业务的例子

## Usage

```Java
@Slf4j
public class SemaphoreCase {
    /**
     * 客户数
     */
    private static final int       num       = 10;
    /**
     * 银行窗口数
     */
    private static final int       n1        = 3;
    private static final Semaphore semaphore = new Semaphore(n1);

    private static final ExecutorService executor = Executors.newFixedThreadPool(n1 + 1);

    @AllArgsConstructor
    static class A implements Runnable {
        private final int i;

        @SneakyThrows
        @Override
        public void run() {
            try {
                semaphore.acquire();
                log.info("第{}个人开始办理业务", i);
                TimeUnit.SECONDS.sleep(2);
                log.info("第{}个人业务办理结束", i);
            } finally {
                semaphore.release();
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        for (int i = 1; i <= num; i++) {
            executor.execute(new A(i));
            TimeUnit.MILLISECONDS.sleep(500);
        }
        executor.shutdown();
    }
}
```

## 实现原理

```Java
public Semaphore(int permits) {
    sync = new NonfairSync(permits);
}
public Semaphore(int permits, boolean fair) {
    sync = fair ? new FairSync(permits) : new NonfairSync(permits);
}
public void acquire() throws InterruptedException {
    sync.acquireSharedInterruptibly(1);
}
public void release() {
    sync.releaseShared(1);
}
```
还有`acquire(int)` 和 `release(int)`方法, 参数的意思可以理解为1个人占用几个窗口办业务, 但内部调用的方法都是一样

`AbstractQueuedSynchronizer`

## 阅读


