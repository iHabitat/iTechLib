> `CountDownLatch` is one of classes that was added to the Java 5 concurrency package. It allows one or more threads to wait util a set of operations being performed in other threads are completed.

# Use-case1: Achieving Maximum Parallelism

Sometimes we have a use-case where we want to start a number of threads at the same time to achieve maximum parallelism. For example, we want to test a class which creates a single instance of some class.

```java
@Testpublic void shouldCreateOnlySingleInstanceOfAClassWhenTestedWithParallelThreads() throws InterruptedException {    long start = System.currentTimeMillis();    final ObjectFactory factory = new ObjectFactory();    final CountDownLatch startSignal = new CountDownLatch(1);    class MyThread extends Thread {        MyObject instance;        @Override        public void run() {            try {                startSignal.await();                instance = factory.getInstance();                TimeUnit.SECONDS.sleep(1);            } catch (InterruptedException e) {                e.printStackTrace();            }        }    }    int threadCount = 1000;    MyThread[] threads = new MyThread[threadCount];    for (int i = 0; i < threadCount; i++) {        threads[i] = new MyThread();        threads[i].start();    }    startSignal.countDown();    for (MyThread myThread : threads) {        myThread.join();    }    MyObject instance = factory.getInstance();    for (MyThread myThread : threads) {        assertEquals(instance, myThread.instance);    }    System.out.println("spent: " + (System.currentTimeMillis() - start));}
```

# Use-case2: Waiting for several threads to complete

This scenario we will initialize the `CountDownLatch` with the number of threads we want to wait for and then each thread will call the `countDown()` method on finishing its work.

```java
@Testpublic void shouldCreateOnlySingleInstanceOfAClassWhenTestedWithParallelThreads2() throws InterruptedException {    long start = System.currentTimeMillis();    int threadCount = 1000;    final ObjectFactory factory = new ObjectFactory();    final CountDownLatch startSignal = new CountDownLatch(1);    final CountDownLatch stopSignal = new CountDownLatch(threadCount);    class MyThread extends Thread {        MyObject instance;        @Override        public void run() {            try {                startSignal.await();                instance = factory.getInstance();                TimeUnit.SECONDS.sleep(1);            } catch (InterruptedException e) {                e.printStackTrace();            } finally {                stopSignal.countDown();            }        }    }    MyThread[] threads = new MyThread[threadCount];    for (int i = 0; i < threadCount; i++) {        threads[i] = new MyThread();        threads[i].start();    }    startSignal.countDown();    stopSignal.await();    MyObject instance = factory.getInstance();    for (MyThread myThread : threads) {        assertEquals(instance, myThread.instance);    }    System.out.println("spent: " + (System.currentTimeMillis() - start));}
```
