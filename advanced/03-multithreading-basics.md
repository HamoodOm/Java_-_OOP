# Multithreading Basics

## 1️⃣ Concept Overview

**Multithreading** is a Java feature that allows concurrent execution of two or more parts of a program for maximum CPU utilization. Each part is called a **thread**, which is a lightweight sub-process.

**Key Concepts**:
- **Thread**: Smallest unit of execution within a process
- **Process**: Program in execution with its own memory space
- **Multitasking**: Multiple processes running concurrently
- **Multithreading**: Multiple threads within a single process

**Thread vs Process**:

| Feature | Thread | Process |
|---------|--------|---------|
| **Memory** | Shared memory space | Separate memory space |
| **Communication** | Easy (shared variables) | Complex (IPC mechanisms) |
| **Creation Cost** | Lightweight | Heavyweight |
| **Context Switch** | Faster | Slower |
| **Independence** | Threads share process resources | Processes are independent |

**Why Multithreading?**:
- Better CPU utilization
- Improved application responsiveness
- Resource sharing within process
- Simplified program structure for concurrent tasks
- Better performance on multi-core systems

**Thread Lifecycle** (Thread States):
```
NEW → RUNNABLE → RUNNING → TERMINATED
        ↕          ↕
    BLOCKED    WAITING/TIMED_WAITING
```

**Thread States**:
1. **NEW**: Thread created but not started
2. **RUNNABLE**: Ready to run, waiting for CPU
3. **RUNNING**: Executing
4. **BLOCKED**: Waiting for monitor lock
5. **WAITING**: Waiting indefinitely for another thread
6. **TIMED_WAITING**: Waiting for specified time
7. **TERMINATED**: Completed execution

---

## 2️⃣ Why Interviewers Ask This

- **Fundamental concept**: Essential for Java developers
- **Real-world relevance**: Most applications use threads
- **Common issues**: Race conditions, deadlocks are production problems
- **Performance**: Threading affects application performance
- **Senior topic**: Shows advanced understanding
- **Common in interviews**: Very frequently asked

---

## 3️⃣ Key Rules / Facts

**Creating Threads** (Two Ways):

| Method | Description | Use Case |
|--------|-------------|----------|
| Extend Thread class | Inherit from Thread | Simple cases, when not extending other class |
| Implement Runnable | Implement Runnable interface | Preferred, allows extending other classes |

**Thread Methods**:

| Method | Description |
|--------|-------------|
| start() | Starts thread, calls run() |
| run() | Entry point for thread |
| sleep(ms) | Pause thread for specified time |
| join() | Wait for thread to complete |
| interrupt() | Interrupt waiting/sleeping thread |
| isAlive() | Check if thread is running |
| setName()/getName() | Set/get thread name |
| setPriority()/getPriority() | Set/get thread priority |
| currentThread() | Get reference to current thread |

**Synchronization**:
- **synchronized keyword**: Ensures only one thread executes a block at a time
- **Monitor lock**: Every object has an intrinsic lock
- **synchronized method**: Locks entire method
- **synchronized block**: Locks specific code section
- **static synchronized**: Locks on class object

**Thread Safety Issues**:

| Issue | Description | Solution |
|-------|-------------|----------|
| Race Condition | Multiple threads access shared data | Synchronization |
| Deadlock | Threads wait for each other indefinitely | Lock ordering, timeout |
| Starvation | Thread never gets CPU time | Fair scheduling |
| Livelock | Threads respond to each other, no progress | Randomization |

**volatile Keyword**:
- Ensures visibility of changes across threads
- Prevents caching of variable value
- No atomicity guarantee (unlike synchronized)
- Use for flags, state variables

**wait(), notify(), notifyAll()**:
- Must be called from synchronized context
- wait(): Release lock and wait
- notify(): Wake one waiting thread
- notifyAll(): Wake all waiting threads

**Thread Priority**:
- Range: MIN_PRIORITY (1) to MAX_PRIORITY (10)
- Default: NORM_PRIORITY (5)
- Just a hint to scheduler, not guaranteed

**Important Rules**:
- start() can be called only once per thread
- Cannot restart terminated thread
- Synchronization on null throws NullPointerException
- wait()/notify() must be in synchronized context
- sleep() doesn't release lock
- wait() releases lock
- join() waits for thread to die

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Creating Threads - Two Ways

```java
// Method 1: Extending Thread class
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// Method 2: Implementing Runnable interface (Preferred)
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class ThreadCreation {
    public static void main(String[] args) {
        System.out.println("=== Method 1: Extending Thread ===");
        MyThread t1 = new MyThread();
        t1.setName("Thread-1");
        t1.start();  // Starts new thread, calls run()
        
        System.out.println("\n=== Method 2: Implementing Runnable ===");
        MyRunnable runnable = new MyRunnable();
        Thread t2 = new Thread(runnable, "Thread-2");
        t2.start();
        
        System.out.println("\n=== Using Lambda (Java 8) ===");
        Thread t3 = new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println(Thread.currentThread().getName() + ": " + i);
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "Thread-3");
        t3.start();
        
        System.out.println("Main thread: " + Thread.currentThread().getName());
    }
}

/*
OUTPUT (interleaved):
Main thread: main
Thread-1: 1
Thread-2: 1
Thread-3: 1
Thread-1: 2
Thread-2: 2
Thread-3: 2
...
*/
```

### Example 2: Thread States and Lifecycle

```java
public class ThreadStates {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(() -> {
            System.out.println("Thread running");
            
            synchronized (ThreadStates.class) {
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            
            System.out.println("Thread completed");
        });
        
        // State 1: NEW
        System.out.println("State after creation: " + thread.getState());  // NEW
        
        // State 2: RUNNABLE
        thread.start();
        Thread.sleep(100);
        System.out.println("State after start: " + thread.getState());  // RUNNABLE or TIMED_WAITING
        
        // State 3: TERMINATED
        thread.join();  // Wait for thread to complete
        System.out.println("State after completion: " + thread.getState());  // TERMINATED
        
        demonstrateAllStates();
    }
    
    static void demonstrateAllStates() throws InterruptedException {
        System.out.println("\n=== All Thread States ===");
        
        Object lock = new Object();
        
        Thread t = new Thread(() -> {
            synchronized (lock) {
                try {
                    lock.wait();  // WAITING state
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        System.out.println("NEW: " + t.getState());
        
        t.start();
        Thread.sleep(100);
        System.out.println("WAITING: " + t.getState());
        
        synchronized (lock) {
            lock.notify();
        }
        
        t.join();
        System.out.println("TERMINATED: " + t.getState());
    }
}

/*
THREAD LIFECYCLE:

NEW: Thread created but not started
  ↓ start()
RUNNABLE: Ready to run, may be running
  ↓ sleep(), wait(), I/O
WAITING/TIMED_WAITING: Waiting for condition
  ↓ notify(), timeout
RUNNABLE: Back to runnable state
  ↓ run() completes
TERMINATED: Thread finished
*/
```

### Example 3: Race Condition Problem

```java
// PROBLEM: Race condition without synchronization
class Counter {
    private int count = 0;
    
    // NOT thread-safe
    public void increment() {
        count++;  // Three operations: read, increment, write
    }
    
    public int getCount() {
        return count;
    }
}

public class RaceConditionDemo {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        
        // Create 1000 threads, each increments 1000 times
        Thread[] threads = new Thread[1000];
        
        for (int i = 0; i < 1000; i++) {
            threads[i] = new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    counter.increment();
                }
            });
            threads[i].start();
        }
        
        // Wait for all threads to complete
        for (Thread t : threads) {
            t.join();
        }
        
        // Expected: 1000 * 1000 = 1,000,000
        // Actual: Less than 1,000,000 due to race condition
        System.out.println("Final count: " + counter.getCount());
        System.out.println("Expected: 1000000");
        System.out.println("Lost updates due to race condition!");
    }
}

/*
RACE CONDITION EXPLANATION:

Thread 1: Read count (100) → Increment (101) → Write (101)
Thread 2: Read count (100) → Increment (101) → Write (101)

Both threads read 100, both write 101
One increment is lost!

SOLUTION: Use synchronization (next example)
*/
```

### Example 4: Synchronization - Solution to Race Condition

```java
// SOLUTION: Synchronized method
class SynchronizedCounter {
    private int count = 0;
    
    // Thread-safe with synchronized
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}

// Alternative: Synchronized block
class BlockSynchronizedCounter {
    private int count = 0;
    private Object lock = new Object();
    
    public void increment() {
        synchronized (lock) {  // Synchronized block
            count++;
        }
    }
    
    public int getCount() {
        synchronized (lock) {
            return count;
        }
    }
}

public class SynchronizationDemo {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("=== Synchronized Counter ===");
        SynchronizedCounter counter = new SynchronizedCounter();
        
        Thread[] threads = new Thread[1000];
        
        for (int i = 0; i < 1000; i++) {
            threads[i] = new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    counter.increment();
                }
            });
            threads[i].start();
        }
        
        for (Thread t : threads) {
            t.join();
        }
        
        System.out.println("Final count: " + counter.getCount());
        System.out.println("Expected: 1000000");
        System.out.println("No race condition!");
        
        demonstrateSynchronizedBlock();
    }
    
    static void demonstrateSynchronizedBlock() {
        System.out.println("\n=== Synchronized Block ===");
        
        Object lock = new Object();
        
        Thread t1 = new Thread(() -> {
            synchronized (lock) {
                System.out.println("Thread 1: Inside synchronized block");
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Thread 1: Exiting synchronized block");
            }
        });
        
        Thread t2 = new Thread(() -> {
            System.out.println("Thread 2: Waiting for lock...");
            synchronized (lock) {
                System.out.println("Thread 2: Got lock, inside synchronized block");
            }
        });
        
        t1.start();
        try { Thread.sleep(100); } catch (InterruptedException e) {}
        t2.start();
    }
}

/*
SYNCHRONIZED KEYWORD:

synchronized method:
- Locks on 'this' for instance methods
- Locks on Class object for static methods
- Only one thread can execute at a time

synchronized block:
- Locks on specified object
- More granular control
- Better performance (smaller critical section)
*/
```

### Example 5: wait(), notify(), notifyAll()

```java
class Message {
    private String message;
    private boolean hasMessage = false;
    
    // Producer writes message
    public synchronized void write(String message) {
        while (hasMessage) {
            try {
                wait();  // Wait until message is read
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        
        this.message = message;
        hasMessage = true;
        System.out.println("Written: " + message);
        notify();  // Notify waiting reader
    }
    
    // Consumer reads message
    public synchronized String read() {
        while (!hasMessage) {
            try {
                wait();  // Wait until message is written
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        
        hasMessage = false;
        System.out.println("Read: " + message);
        notify();  // Notify waiting writer
        return message;
    }
}

public class WaitNotifyDemo {
    public static void main(String[] args) {
        Message message = new Message();
        
        // Producer thread
        Thread producer = new Thread(() -> {
            String[] messages = {"Message 1", "Message 2", "Message 3"};
            for (String msg : messages) {
                message.write(msg);
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        // Consumer thread
        Thread consumer = new Thread(() -> {
            for (int i = 0; i < 3; i++) {
                message.read();
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        producer.start();
        consumer.start();
    }
}

/*
wait() / notify() / notifyAll():

wait():
- Must be called from synchronized context
- Releases lock and waits
- Thread goes to WAITING state

notify():
- Wakes up one waiting thread
- Must be called from synchronized context

notifyAll():
- Wakes up all waiting threads
- Better than notify() to avoid missed signals
*/
```

### Example 6: Deadlock Example

```java
public class DeadlockDemo {
    private static Object lock1 = new Object();
    private static Object lock2 = new Object();
    
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Thread 1: Holding lock1...");
                
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                
                System.out.println("Thread 1: Waiting for lock2...");
                synchronized (lock2) {
                    System.out.println("Thread 1: Holding lock1 & lock2");
                }
            }
        });
        
        Thread t2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("Thread 2: Holding lock2...");
                
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                
                System.out.println("Thread 2: Waiting for lock1...");
                synchronized (lock1) {
                    System.out.println("Thread 2: Holding lock1 & lock2");
                }
            }
        });
        
        t1.start();
        t2.start();
        
        // Deadlock occurs!
        // Thread 1 waits for lock2, Thread 2 waits for lock1
    }
}

/*
DEADLOCK CONDITIONS:
1. Mutual Exclusion: Only one thread can hold lock
2. Hold and Wait: Thread holds lock and waits for another
3. No Preemption: Lock cannot be forcibly taken
4. Circular Wait: T1 waits for T2, T2 waits for T1

DEADLOCK PREVENTION:
1. Lock ordering: Always acquire locks in same order
2. Lock timeout: Use tryLock() with timeout
3. Deadlock detection: Monitor and break deadlock
*/
```

### Example 7: Deadlock Prevention

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class DeadlockPrevention {
    private static Lock lock1 = new ReentrantLock();
    private static Lock lock2 = new ReentrantLock();
    
    public static void main(String[] args) {
        // Solution 1: Lock ordering
        lockOrdering();
        
        // Solution 2: tryLock with timeout
        tryLockWithTimeout();
    }
    
    // Solution 1: Always acquire locks in same order
    static void lockOrdering() {
        System.out.println("=== Lock Ordering Solution ===");
        
        Thread t1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Thread 1: Holding lock1");
                synchronized (lock2) {  // Same order as Thread 2
                    System.out.println("Thread 1: Holding lock1 & lock2");
                }
            }
        });
        
        Thread t2 = new Thread(() -> {
            synchronized (lock1) {  // Same order as Thread 1
                System.out.println("Thread 2: Holding lock1");
                synchronized (lock2) {
                    System.out.println("Thread 2: Holding lock1 & lock2");
                }
            }
        });
        
        t1.start();
        t2.start();
    }
    
    // Solution 2: Use tryLock with timeout
    static void tryLockWithTimeout() {
        System.out.println("\n=== tryLock with Timeout ===");
        
        Thread t1 = new Thread(() -> {
            try {
                if (lock1.tryLock()) {
                    try {
                        System.out.println("Thread 1: Got lock1");
                        Thread.sleep(100);
                        
                        if (lock2.tryLock()) {
                            try {
                                System.out.println("Thread 1: Got both locks");
                            } finally {
                                lock2.unlock();
                            }
                        } else {
                            System.out.println("Thread 1: Couldn't get lock2");
                        }
                    } finally {
                        lock1.unlock();
                    }
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        t1.start();
    }
}
```

### Example 8: volatile Keyword

```java
public class VolatileDemo {
    // Without volatile - thread may cache value
    private static boolean flag = false;
    
    // With volatile - ensures visibility across threads
    private static volatile boolean volatileFlag = false;
    
    public static void main(String[] args) throws InterruptedException {
        demonstrateVolatile();
    }
    
    static void demonstrateVolatile() throws InterruptedException {
        Thread writer = new Thread(() -> {
            try {
                Thread.sleep(1000);
                volatileFlag = true;
                System.out.println("Flag set to true");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        Thread reader = new Thread(() -> {
            while (!volatileFlag) {
                // Waiting for flag to become true
                // Without volatile, thread might cache old value
            }
            System.out.println("Flag is now true!");
        });
        
        reader.start();
        writer.start();
        
        reader.join();
        writer.join();
    }
}

/*
VOLATILE KEYWORD:

Purpose:
- Ensures visibility of changes across threads
- Prevents caching of variable value
- Variable read from/written to main memory

Use Cases:
- Flags or state variables
- When only one thread writes, others read

Limitations:
- No atomicity (count++ still needs synchronization)
- Only for visibility, not mutual exclusion

When to use volatile vs synchronized:
- volatile: Single variable, visibility only
- synchronized: Multiple operations, atomicity + visibility
*/
```

### Example 9: Thread Priority

```java
public class ThreadPriorityDemo {
    public static void main(String[] args) {
        Thread low = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Low priority: " + i);
            }
        });
        
        Thread normal = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Normal priority: " + i);
            }
        });
        
        Thread high = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("High priority: " + i);
            }
        });
        
        // Set priorities
        low.setPriority(Thread.MIN_PRIORITY);      // 1
        normal.setPriority(Thread.NORM_PRIORITY);  // 5
        high.setPriority(Thread.MAX_PRIORITY);     // 10
        
        System.out.println("Low priority: " + low.getPriority());
        System.out.println("Normal priority: " + normal.getPriority());
        System.out.println("High priority: " + high.getPriority());
        
        // Start threads
        low.start();
        normal.start();
        high.start();
    }
}

/*
THREAD PRIORITY:

Range: 1 (MIN_PRIORITY) to 10 (MAX_PRIORITY)
Default: 5 (NORM_PRIORITY)

Important Notes:
- Priority is just a HINT to scheduler
- Not guaranteed execution order
- Platform-dependent behavior
- Don't rely on priority for correctness
- Use for optimization suggestions only
*/
```

### Example 10: Real-World Example - Producer-Consumer

```java
import java.util.LinkedList;
import java.util.Queue;

class SharedQueue {
    private Queue<Integer> queue = new LinkedList<>();
    private int capacity = 5;
    
    // Producer adds items
    public synchronized void produce(int item) throws InterruptedException {
        while (queue.size() == capacity) {
            System.out.println("Queue full, producer waiting...");
            wait();  // Wait until space available
        }
        
        queue.add(item);
        System.out.println("Produced: " + item + " (Queue size: " + queue.size() + ")");
        notifyAll();  // Notify waiting consumers
    }
    
    // Consumer removes items
    public synchronized int consume() throws InterruptedException {
        while (queue.isEmpty()) {
            System.out.println("Queue empty, consumer waiting...");
            wait();  // Wait until item available
        }
        
        int item = queue.poll();
        System.out.println("Consumed: " + item + " (Queue size: " + queue.size() + ")");
        notifyAll();  // Notify waiting producers
        return item;
    }
}

public class ProducerConsumerDemo {
    public static void main(String[] args) {
        SharedQueue queue = new SharedQueue();
        
        // Producer thread
        Thread producer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    queue.produce(i);
                    Thread.sleep(200);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        // Consumer thread
        Thread consumer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    queue.consume();
                    Thread.sleep(500);  // Slower than producer
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        producer.start();
        consumer.start();
    }
}

/*
PRODUCER-CONSUMER PATTERN:

Producer:
1. Check if queue is full
2. If full, wait()
3. If not, add item
4. notifyAll() consumers

Consumer:
1. Check if queue is empty
2. If empty, wait()
3. If not, remove item
4. notifyAll() producers

Benefits:
- Decouples production and consumption
- Handles different processing speeds
- Thread-safe data exchange
*/
```

---

## 5️⃣ Common Interview Questions

1. **What is multithreading in Java?**
2. **What are the ways to create a thread?**
3. **What is the difference between start() and run()?**
4. **What is synchronization in Java?**
5. **What is the difference between synchronized method and synchronized block?**
6. **What is a race condition?**
7. **What is deadlock and how to prevent it?**
8. **What is the difference between wait() and sleep()?**
9. **What is the purpose of volatile keyword?**
10. **What are thread states in Java?**

---

## 6️⃣ Model Answers

**Q1: What is multithreading in Java?**

"Multithreading is a Java feature that allows concurrent execution of multiple threads within a single process. A thread is a lightweight sub-process, the smallest unit of execution. Multiple threads share the same memory space but execute independently, allowing programs to perform multiple tasks simultaneously. For example, a GUI application can update the interface on one thread while processing data on another. Multithreading improves CPU utilization, application responsiveness, and is essential for concurrent operations. Each thread has its own call stack but shares heap memory with other threads in the process."

**Q2: What are the ways to create a thread?**

"There are two main ways to create threads in Java. First, extend the Thread class and override the run() method. This is simpler but you can't extend another class since Java doesn't support multiple inheritance. Second, implement the Runnable interface and pass it to a Thread object. This is preferred because it separates the task from the thread mechanism and allows your class to extend another class. In Java 8+, you can also use lambda expressions with Runnable. Both approaches require calling start() to begin execution, which creates a new thread and calls run(). Calling run() directly just executes it on the current thread."

**Q3: What is the difference between start() and run()?**

"start() creates a new thread and calls run() on that thread, enabling concurrent execution. When you call start(), the JVM creates a new call stack for the thread and the run() method executes in that new thread. run() is just a normal method—if you call it directly, it executes on the current thread like any other method call, with no multithreading. Also, start() can only be called once per thread object—calling it again throws IllegalThreadStateException. But you can call run() multiple times directly. The key difference is start() enables concurrency while run() doesn't."

**Q4: What is synchronization in Java?**

"Synchronization is the capability to control access to shared resources by multiple threads. It prevents race conditions where multiple threads simultaneously modify shared data, leading to inconsistent state. Java provides the synchronized keyword which uses monitors—every object has an intrinsic lock. When a thread enters a synchronized method or block, it acquires the lock. Other threads trying to enter synchronized code on the same object must wait until the lock is released. This ensures only one thread executes the critical section at a time. Synchronization provides mutual exclusion and ensures visibility of changes across threads. However, it can reduce performance due to thread blocking."

**Q5: What is the difference between synchronized method and synchronized block?**

"Synchronized method locks the entire method—for instance methods, it locks on 'this'; for static methods, it locks on the Class object. Synchronized block allows you to lock on any object and only a specific section of code. The key differences are granularity and performance. Synchronized blocks are more flexible—you can choose what object to lock on and minimize the critical section, improving performance. For example, if a method has 100 lines but only 10 need synchronization, a synchronized block is better. Synchronized methods are simpler to write but lock more than necessary. Use synchronized blocks for better control and performance, synchronized methods for simplicity when entire method needs locking."

**Q6: What is a race condition?**

"A race condition occurs when multiple threads access shared data concurrently, and the final result depends on the timing of their execution. For example, if two threads increment a shared counter, the operation count++ involves three steps: read, increment, write. If both threads read the same value before either writes, one increment is lost. Consider count = 5: Thread 1 reads 5, Thread 2 reads 5, both calculate 6, both write 6—result is 6 instead of 7. This is non-deterministic—sometimes it works, sometimes it fails, making it hard to debug. The solution is synchronization—using synchronized keyword or locks to ensure only one thread executes the critical section at a time."

**Q7: What is deadlock and how to prevent it?**

"Deadlock occurs when two or more threads are blocked forever, waiting for each other to release locks. Classic example: Thread 1 holds Lock A and waits for Lock B; Thread 2 holds Lock B and waits for Lock A. Neither can proceed. Four conditions must hold for deadlock: mutual exclusion, hold and wait, no preemption, and circular wait. Prevention strategies: First, lock ordering—always acquire locks in the same order across all threads. Second, use tryLock() with timeout instead of blocking indefinitely. Third, avoid nested locks when possible. Fourth, use deadlock detection tools to identify and break deadlocks. In production, lock ordering is most practical."

**Q8: What is the difference between wait() and sleep()?**

"wait() and sleep() both pause thread execution but differ fundamentally. wait() releases the monitor lock and must be called from a synchronized context—it's used for inter-thread communication where one thread waits for another to perform an action. sleep() doesn't release any locks and can be called anywhere—it's used to pause execution for a specific time. wait() is called on an object and the thread waits until notify() or notifyAll() is called on that object. sleep() is a static method called on Thread class with a time parameter. wait() can be interrupted and resumed by notify(), while sleep() only wakes after the time expires or interruption."

**Q9: What is the purpose of volatile keyword?**

"volatile ensures visibility of variable changes across threads. Without volatile, threads may cache variables in CPU registers or thread-local cache, so changes made by one thread might not be visible to others. volatile forces all reads and writes to go through main memory, ensuring all threads see the latest value. For example, a boolean flag used to stop a thread should be volatile. However, volatile doesn't provide atomicity—operations like count++ still need synchronization because they're multiple operations (read, increment, write). Use volatile for simple flags or status variables where only one thread writes and others read. For compound operations or multiple variables, use synchronized."

**Q10: What are thread states in Java?**

"A thread in Java has six states defined in Thread.State enum. NEW: thread created but not started. RUNNABLE: ready to run or currently running—the JVM doesn't distinguish between ready and running. BLOCKED: waiting to acquire a monitor lock for synchronized code. WAITING: waiting indefinitely for another thread, via wait(), join(), or LockSupport.park(). TIMED_WAITING: waiting for a specified time, via sleep(), wait(timeout), or join(timeout). TERMINATED: thread completed execution. A thread starts in NEW, moves to RUNNABLE when start() is called, may transition between RUNNABLE, BLOCKED, WAITING, and TIMED_WAITING during execution, and ends in TERMINATED. You can check state with getState() method."

---

## 7️⃣ Common Mistakes

1. **Calling run() instead of start()**:
   ```java
   Thread t = new Thread(() -> System.out.println("Hello"));
   t.run();  // BAD: Executes on current thread!
   
   // GOOD:
   t.start();  // Creates new thread
   ```

2. **Not handling InterruptedException**:
   ```java
   // BAD: Empty catch
   try {
       Thread.sleep(1000);
   } catch (InterruptedException e) {
       // Ignoring exception
   }
   
   // GOOD:
   catch (InterruptedException e) {
       Thread.currentThread().interrupt();  // Restore interrupt status
       // Or handle appropriately
   }
   ```

3. **Synchronizing on null**:
   ```java
   Object lock = null;
   synchronized (lock) {  // NullPointerException!
       // ...
   }
   ```

4. **Not using wait() in loop**:
   ```java
   // BAD:
   if (condition) {
       wait();
   }
   
   // GOOD: Always use while loop
   while (condition) {
       wait();  // Protects against spurious wakeups
   }
   ```

5. **Calling wait/notify without synchronization**:
   ```java
   // BAD:
   obj.wait();  // IllegalMonitorStateException!
   
   // GOOD:
   synchronized (obj) {
       obj.wait();
   }
   ```

6. **Starting thread multiple times**:
   ```java
   Thread t = new Thread(() -> {});
   t.start();
   t.start();  // IllegalThreadStateException!
   ```

7. **Assuming thread priority guarantees order**:
   ```java
   // BAD: Relying on priority for correctness
   t1.setPriority(10);
   t2.setPriority(1);
   // No guarantee t1 runs first!
   ```

8. **Not making shared variables volatile**:
   ```java
   // BAD:
   private boolean flag = false;
   
   // GOOD:
   private volatile boolean flag = false;
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Two ways to create threads: extend Thread or implement Runnable
- start() creates new thread, run() doesn't
- Synchronization prevents race conditions
- wait() releases lock, sleep() doesn't
- volatile ensures visibility, synchronized ensures atomicity
- Deadlock prevention: lock ordering, timeout

**What to avoid**:
- Don't confuse start() with run()
- Don't forget that synchronization has performance cost
- Don't claim volatile provides atomicity
- Don't say you can restart a terminated thread

**Under pressure**:
- Explain: "Multithreading = multiple threads in one process"
- State: "Create via Thread class or Runnable interface"
- Mention: "Synchronization prevents race conditions"
- Reference: "wait() vs sleep(): lock release is key difference"

**Red flags to avoid**:
- "Call run() to start thread" (use start()!)
- "volatile makes count++ thread-safe" (needs synchronized)
- "Priority guarantees execution order" (just a hint)
- "wait() and sleep() are same" (very different!)

**Bonus points**:
- Mention thread states (NEW, RUNNABLE, BLOCKED, etc.)
- Discuss ThreadLocal for thread-confined data
- Reference ExecutorService and thread pools
- Mention java.util.concurrent package
- Discuss happens-before relationship
- Reference ReentrantLock as alternative to synchronized
- Mention CountDownLatch, CyclicBarrier, Semaphore
- Discuss atomic variables (AtomicInteger, etc.)
