<div dir="rtl">

# أساسيات تعدد الخيوط

## 1️⃣ نظرة عامة على المفهوم

**تعدد الخيوط (Multithreading)** هو ميزة في Java تسمح بالتنفيذ المتزامن لجزأين أو أكثر من البرنامج لتحقيق أقصى استفادة من المعالج. كل جزء يسمى **Thread**، وهو عملية فرعية خفيفة الوزن.

**المفاهيم الأساسية**:
- **Thread**: أصغر وحدة تنفيذ داخل العملية
- **Process**: برنامج قيد التنفيذ مع مساحة ذاكرة خاصة به
- **تعدد المهام (Multitasking)**: تشغيل عمليات متعددة بشكل متزامن
- **تعدد الخيوط (Multithreading)**: خيوط متعددة داخل عملية واحدة

**الفرق بين Thread و Process**:

| الميزة | Thread | Process |
|--------|--------|---------|
| **الذاكرة** | مساحة ذاكرة مشتركة | مساحة ذاكرة منفصلة |
| **التواصل** | سهل (متغيرات مشتركة) | معقد (آليات IPC) |
| **تكلفة الإنشاء** | خفيف الوزن | ثقيل الوزن |
| **تبديل السياق** | أسرع | أبطأ |
| **الاستقلالية** | الخيوط تشترك في موارد العملية | العمليات مستقلة |

**لماذا نستخدم تعدد الخيوط؟**:
- استخدام أفضل للمعالج
- تحسين استجابة التطبيق
- مشاركة الموارد داخل العملية
- تبسيط هيكل البرنامج للمهام المتزامنة
- أداء أفضل على الأنظمة متعددة النوى

**دورة حياة Thread** (حالات Thread):
```
NEW → RUNNABLE → RUNNING → TERMINATED
        ↕          ↕
    BLOCKED    WAITING/TIMED_WAITING
```

**حالات Thread**:
1. **NEW**: تم إنشاء Thread لكن لم يبدأ بعد
2. **RUNNABLE**: جاهز للتشغيل، ينتظر المعالج
3. **RUNNING**: قيد التنفيذ
4. **BLOCKED**: ينتظر قفل monitor
5. **WAITING**: ينتظر بشكل غير محدد لـ thread آخر
6. **TIMED_WAITING**: ينتظر لوقت محدد
7. **TERMINATED**: اكتمل التنفيذ

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **مفهوم أساسي**: ضروري لمطوري Java
- **صلة بالواقع**: معظم التطبيقات تستخدم الخيوط
- **مشاكل شائعة**: Race conditions و deadlocks مشاكل إنتاجية حقيقية
- **الأداء**: الخيوط تؤثر على أداء التطبيق
- **موضوع متقدم**: يظهر فهماً عميقاً
- **شائع في المقابلات**: يُسأل بشكل متكرر جداً

---

## 3️⃣ القواعد والحقائق الأساسية

**إنشاء الخيوط** (طريقتان):

| الطريقة | الوصف | حالة الاستخدام |
|---------|-------|----------------|
| وراثة Thread class | الوراثة من Thread | حالات بسيطة، عندما لا ترث من class آخر |
| تنفيذ Runnable | تنفيذ Runnable interface | مفضلة، تسمح بالوراثة من classes أخرى |

**دوال Thread**:

| الدالة | الوصف |
|--------|-------|
| start() | تبدأ thread، تستدعي run() |
| run() | نقطة دخول thread |
| sleep(ms) | إيقاف مؤقت لـ thread لوقت محدد |
| join() | انتظار اكتمال thread |
| interrupt() | مقاطعة thread في حالة انتظار/نوم |
| isAlive() | التحقق مما إذا كان thread يعمل |
| setName()/getName() | تعيين/الحصول على اسم thread |
| setPriority()/getPriority() | تعيين/الحصول على أولوية thread |
| currentThread() | الحصول على مرجع thread الحالي |

**التزامن (Synchronization)**:
- **كلمة synchronized**: تضمن أن thread واحد فقط ينفذ الكود في وقت واحد
- **قفل Monitor**: كل كائن له قفل جوهري
- **synchronized method**: قفل الدالة بالكامل
- **synchronized block**: قفل جزء محدد من الكود
- **static synchronized**: قفل على كائن الـ class

**مشاكل أمان الخيوط**:

| المشكلة | الوصف | الحل |
|---------|-------|------|
| Race Condition | خيوط متعددة تصل لبيانات مشتركة | التزامن |
| Deadlock | الخيوط تنتظر بعضها للأبد | ترتيب الأقفال، مهلة زمنية |
| Starvation | thread لا يحصل على وقت معالج أبداً | جدولة عادلة |
| Livelock | الخيوط تستجيب لبعضها، بدون تقدم | العشوائية |

**كلمة volatile**:
- تضمن رؤية التغييرات عبر الخيوط
- تمنع تخزين قيمة المتغير مؤقتاً
- لا تضمن الذرية (على عكس synchronized)
- تستخدم للرايات ومتغيرات الحالة

**wait()، notify()، notifyAll()**:
- يجب استدعاؤها من سياق synchronized
- wait(): تحرر القفل وتنتظر
- notify(): توقظ thread واحد منتظر
- notifyAll(): توقظ جميع الخيوط المنتظرة

**أولوية Thread**:
- النطاق: MIN_PRIORITY (1) إلى MAX_PRIORITY (10)
- الافتراضي: NORM_PRIORITY (5)
- مجرد تلميح للمجدول، غير مضمون

**قواعد مهمة**:
- start() يمكن استدعاؤها مرة واحدة فقط لكل thread
- لا يمكن إعادة تشغيل thread منتهي
- التزامن على null يرمي NullPointerException
- wait()/notify() يجب أن تكون في سياق synchronized
- sleep() لا تحرر القفل
- wait() تحرر القفل
- join() تنتظر موت thread

---

## 4️⃣ أمثلة الكود (Java 8)

### المثال 1: إنشاء الخيوط - طريقتان

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

### المثال 2: حالات Thread ودورة الحياة

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

### المثال 3: مشكلة Race Condition

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

### المثال 4: التزامن - حل مشكلة Race Condition

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

### المثال 5: wait()، notify()، notifyAll()

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

### المثال 6: مثال Deadlock

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

### المثال 7: منع Deadlock

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

### المثال 8: كلمة volatile

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

### المثال 9: أولوية Thread

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

### المثال 10: مثال واقعي - نمط المنتج والمستهلك

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

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما هو تعدد الخيوط في Java؟**
2. **ما هي طرق إنشاء thread؟**
3. **ما الفرق بين start() و run()؟**
4. **ما هو التزامن (synchronization) في Java؟**
5. **ما الفرق بين synchronized method و synchronized block؟**
6. **ما هي race condition؟**
7. **ما هو deadlock وكيف نمنعه؟**
8. **ما الفرق بين wait() و sleep()؟**
9. **ما هو الغرض من كلمة volatile؟**
10. **ما هي حالات thread في Java؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما هو تعدد الخيوط في Java؟**

"تعدد الخيوط هو ميزة في Java تسمح بالتنفيذ المتزامن لخيوط متعددة داخل عملية واحدة. Thread هو عملية فرعية خفيفة، أصغر وحدة تنفيذ. الخيوط المتعددة تشترك في نفس مساحة الذاكرة لكنها تنفذ بشكل مستقل، مما يسمح للبرامج بأداء مهام متعددة في وقت واحد. على سبيل المثال، تطبيق GUI يمكنه تحديث الواجهة على thread واحد أثناء معالجة البيانات على آخر. تعدد الخيوط يحسن استخدام المعالج واستجابة التطبيق وهو ضروري للعمليات المتزامنة. كل thread له call stack خاص به لكنه يشترك في ذاكرة heap مع الخيوط الأخرى في العملية."

**س2: ما هي طرق إنشاء thread؟**

"هناك طريقتان رئيسيتان لإنشاء الخيوط في Java. الأولى، وراثة Thread class وتجاوز دالة run(). هذا أبسط لكنك لا تستطيع الوراثة من class آخر لأن Java لا تدعم الوراثة المتعددة. الثانية، تنفيذ Runnable interface وتمريرها لكائن Thread. هذه مفضلة لأنها تفصل المهمة عن آلية الخيط وتسمح لـ class الخاص بك بالوراثة من class آخر. في Java 8+، يمكنك أيضاً استخدام تعبيرات lambda مع Runnable. كلا الطريقتين تتطلبان استدعاء start() لبدء التنفيذ، والتي تنشئ thread جديد وتستدعي run(). استدعاء run() مباشرة ينفذها فقط على الـ thread الحالي."

**س3: ما الفرق بين start() و run()؟**

"start() تنشئ thread جديد وتستدعي run() على ذلك الـ thread، مما يمكّن التنفيذ المتزامن. عندما تستدعي start()، JVM تنشئ call stack جديد للـ thread ودالة run() تنفذ في ذلك الـ thread الجديد. run() هي مجرد دالة عادية - إذا استدعيتها مباشرة، تنفذ على الـ thread الحالي مثل أي استدعاء دالة آخر، بدون تعدد خيوط. أيضاً، start() يمكن استدعاؤها مرة واحدة فقط لكل كائن thread - استدعاؤها مرة أخرى يرمي IllegalThreadStateException. لكن يمكنك استدعاء run() عدة مرات مباشرة. الفرق الرئيسي هو start() تمكّن التزامن بينما run() لا تفعل."

**س4: ما هو التزامن في Java؟**

"التزامن هو القدرة على التحكم في الوصول للموارد المشتركة من قبل خيوط متعددة. إنه يمنع race conditions حيث تعدل خيوط متعددة البيانات المشتركة بشكل متزامن، مما يؤدي إلى حالة غير متسقة. Java توفر كلمة synchronized التي تستخدم monitors - كل كائن له قفل جوهري. عندما يدخل thread دالة synchronized أو block، يحصل على القفل. الخيوط الأخرى التي تحاول الدخول لكود synchronized على نفس الكائن يجب أن تنتظر حتى يتم تحرير القفل. هذا يضمن أن thread واحد فقط ينفذ القسم الحرج في وقت واحد. التزامن يوفر الاستبعاد المتبادل ويضمن رؤية التغييرات عبر الخيوط. ومع ذلك، يمكن أن يقلل الأداء بسبب حظر الخيوط."

**س5: ما الفرق بين synchronized method و synchronized block؟**

"synchronized method تقفل الدالة بالكامل - لدوال instance، تقفل على 'this'؛ لدوال static، تقفل على كائن الـ Class. synchronized block يسمح لك بالقفل على أي كائن وجزء محدد فقط من الكود. الفروق الرئيسية هي الدقة والأداء. synchronized blocks أكثر مرونة - يمكنك اختيار أي كائن للقفل عليه وتقليل القسم الحرج، مما يحسن الأداء. على سبيل المثال، إذا كانت الدالة تحتوي على 100 سطر لكن 10 فقط تحتاج التزامن، synchronized block أفضل. synchronized methods أبسط في الكتابة لكنها تقفل أكثر مما هو ضروري. استخدم synchronized blocks للتحكم والأداء الأفضل، synchronized methods للبساطة عندما تحتاج الدالة بالكامل للقفل."

**س6: ما هي race condition؟**

"race condition تحدث عندما تصل خيوط متعددة للبيانات المشتركة بشكل متزامن، والنتيجة النهائية تعتمد على توقيت تنفيذها. على سبيل المثال، إذا كان هناك خيطان يزيدان عداداً مشتركاً، عملية count++ تتضمن ثلاث خطوات: قراءة، زيادة، كتابة. إذا قرأ كلا الخيطين نفس القيمة قبل أن يكتب أي منهما، تُفقد زيادة واحدة. اعتبر count = 5: Thread 1 يقرأ 5، Thread 2 يقرأ 5، كلاهما يحسب 6، كلاهما يكتب 6 - النتيجة 6 بدلاً من 7. هذا غير حتمي - أحياناً يعمل، أحياناً يفشل، مما يجعله صعب التصحيح. الحل هو التزامن - استخدام كلمة synchronized أو الأقفال لضمان أن thread واحد فقط ينفذ القسم الحرج في وقت واحد."

**س7: ما هو deadlock وكيف نمنعه؟**

"Deadlock يحدث عندما يتم حظر خيطين أو أكثر للأبد، ينتظر كل منهما الآخر لتحرير الأقفال. مثال كلاسيكي: Thread 1 يحمل Lock A وينتظر Lock B؛ Thread 2 يحمل Lock B وينتظر Lock A. لا يمكن لأي منهما المتابعة. يجب أن تتحقق أربعة شروط لـ deadlock: الاستبعاد المتبادل، الاحتفاظ والانتظار، عدم الاستباق، والانتظار الدائري. استراتيجيات المنع: أولاً، ترتيب الأقفال - دائماً الحصول على الأقفال بنفس الترتيب عبر جميع الخيوط. ثانياً، استخدام tryLock() مع مهلة زمنية بدلاً من الحظر للأبد. ثالثاً، تجنب الأقفال المتداخلة عندما يكون ذلك ممكناً. رابعاً، استخدام أدوات كشف deadlock لتحديده وكسره. في الإنتاج، ترتيب الأقفال هو الأكثر عملية."

**س8: ما الفرق بين wait() و sleep()؟**

"wait() و sleep() كلاهما يوقفان تنفيذ thread لكنهما يختلفان جوهرياً. wait() تحرر قفل monitor ويجب استدعاؤها من سياق synchronized - تُستخدم للتواصل بين الخيوط حيث ينتظر thread واحد لآخر لأداء إجراء. sleep() لا تحرر أي أقفال ويمكن استدعاؤها من أي مكان - تُستخدم لإيقاف التنفيذ لوقت محدد. wait() تُستدعى على كائن والـ thread ينتظر حتى يتم استدعاء notify() أو notifyAll() على ذلك الكائن. sleep() هي دالة static تُستدعى على Thread class مع معامل وقت. wait() يمكن مقاطعتها واستئنافها بواسطة notify()، بينما sleep() تستيقظ فقط بعد انتهاء الوقت أو المقاطعة."

**س9: ما هو الغرض من كلمة volatile؟**

"volatile تضمن رؤية تغييرات المتغير عبر الخيوط. بدون volatile، قد تخزن الخيوط المتغيرات في سجلات المعالج أو الذاكرة المؤقتة المحلية، لذا قد لا تكون التغييرات التي يجريها thread مرئية للآخرين. volatile تفرض أن تمر جميع القراءات والكتابات عبر الذاكرة الرئيسية، مما يضمن أن جميع الخيوط ترى أحدث قيمة. على سبيل المثال، راية boolean المستخدمة لإيقاف thread يجب أن تكون volatile. ومع ذلك، volatile لا توفر الذرية - عمليات مثل count++ لا تزال تحتاج للتزامن لأنها عمليات متعددة (قراءة، زيادة، كتابة). استخدم volatile لرايات بسيطة أو متغيرات الحالة حيث يكتب thread واحد فقط والآخرون يقرأون. للعمليات المركبة أو المتغيرات المتعددة، استخدم synchronized."

**س10: ما هي حالات thread في Java؟**

"thread في Java له ست حالات معرفة في Thread.State enum. NEW: تم إنشاء thread لكن لم يبدأ. RUNNABLE: جاهز للتشغيل أو قيد التشغيل حالياً - JVM لا تفرق بين الجاهز والتشغيل. BLOCKED: ينتظر الحصول على قفل monitor لكود synchronized. WAITING: ينتظر بشكل غير محدد لـ thread آخر، عبر wait() أو join() أو LockSupport.park(). TIMED_WAITING: ينتظر لوقت محدد، عبر sleep() أو wait(timeout) أو join(timeout). TERMINATED: thread اكتمل التنفيذ. يبدأ thread في NEW، ينتقل إلى RUNNABLE عند استدعاء start()، قد ينتقل بين RUNNABLE و BLOCKED و WAITING و TIMED_WAITING أثناء التنفيذ، وينتهي في TERMINATED. يمكنك التحقق من الحالة بدالة getState()."

---

## 7️⃣ الأخطاء الشائعة

1. **استدعاء run() بدلاً من start()**:
   ```java
   Thread t = new Thread(() -> System.out.println("Hello"));
   t.run();  // BAD: Executes on current thread!

   // GOOD:
   t.start();  // Creates new thread
   ```

2. **عدم معالجة InterruptedException**:
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

3. **التزامن على null**:
   ```java
   Object lock = null;
   synchronized (lock) {  // NullPointerException!
       // ...
   }
   ```

4. **عدم استخدام wait() في حلقة**:
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

5. **استدعاء wait/notify بدون تزامن**:
   ```java
   // BAD:
   obj.wait();  // IllegalMonitorStateException!

   // GOOD:
   synchronized (obj) {
       obj.wait();
   }
   ```

6. **بدء thread عدة مرات**:
   ```java
   Thread t = new Thread(() -> {});
   t.start();
   t.start();  // IllegalThreadStateException!
   ```

7. **الافتراض بأن أولوية thread تضمن الترتيب**:
   ```java
   // BAD: Relying on priority for correctness
   t1.setPriority(10);
   t2.setPriority(1);
   // No guarantee t1 runs first!
   ```

8. **عدم جعل المتغيرات المشتركة volatile**:
   ```java
   // BAD:
   private boolean flag = false;

   // GOOD:
   private volatile boolean flag = false;
   ```

---

## 8️⃣ نصائح المقابلات الحقيقية

**ما يجب التأكيد عليه**:
- طريقتان لإنشاء الخيوط: وراثة Thread أو تنفيذ Runnable
- start() تنشئ thread جديد، run() لا تفعل
- التزامن يمنع race conditions
- wait() تحرر القفل، sleep() لا تفعل
- volatile تضمن الرؤية، synchronized تضمن الذرية
- منع Deadlock: ترتيب الأقفال، المهلة الزمنية

**ما يجب تجنبه**:
- لا تخلط بين start() و run()
- لا تنسَ أن التزامن له تكلفة أداء
- لا تدّعِ أن volatile توفر الذرية
- لا تقل أنه يمكن إعادة تشغيل thread منتهي

**تحت الضغط**:
- اشرح: "تعدد الخيوط = خيوط متعددة في عملية واحدة"
- اذكر: "الإنشاء عبر Thread class أو Runnable interface"
- أشر إلى: "التزامن يمنع race conditions"
- ارجع إلى: "wait() مقابل sleep(): تحرير القفل هو الفرق الرئيسي"

**علامات التحذير لتجنبها**:
- "استدعاء run() لبدء thread" (استخدم start()!)
- "volatile تجعل count++ آمنة للخيوط" (تحتاج synchronized)
- "الأولوية تضمن ترتيب التنفيذ" (مجرد تلميح)
- "wait() و sleep() متشابهتان" (مختلفتان جداً!)

**نقاط إضافية**:
- اذكر حالات thread (NEW، RUNNABLE، BLOCKED، إلخ)
- ناقش ThreadLocal للبيانات المحصورة بالـ thread
- ارجع إلى ExecutorService ومجموعات الخيوط
- اذكر حزمة java.util.concurrent
- ناقش علاقة happens-before
- ارجع إلى ReentrantLock كبديل لـ synchronized
- اذكر CountDownLatch، CyclicBarrier، Semaphore
- ناقش المتغيرات الذرية (AtomicInteger، إلخ)

</div>
