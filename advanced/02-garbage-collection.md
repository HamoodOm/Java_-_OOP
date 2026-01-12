# Garbage Collection

## 1️⃣ Concept Overview

**Garbage Collection (GC)** is the automatic memory management process in Java where the JVM identifies and removes objects that are no longer needed, freeing up heap memory.

**Key Characteristics**:
- **Automatic**: No manual memory deallocation (unlike C/C++)
- **Reclaims unused memory**: Frees objects with no references
- **Prevents memory leaks**: Removes unreachable objects
- **Stop-the-world**: Some GC phases pause application threads
- **Non-deterministic**: You cannot predict exactly when GC runs

**Why Garbage Collection?**:
- Eliminates manual memory management
- Prevents memory leaks (mostly)
- Prevents dangling pointers
- Simplifies programming

**GC Fundamentals**:
```
Object eligible for GC when:
1. No references point to it
2. All references are null
3. Object is unreachable from root
```

**Root Objects** (GC Roots):
- Local variables on stack
- Active threads
- Static variables
- JNI references

**Generational Hypothesis**:
- Most objects die young
- Few objects survive to old age
- Separate young and old generations optimize GC

**Heap Structure** (Generational GC):
```
┌─────────────────────────────────────────────────┐
│                  HEAP MEMORY                    │
├─────────────────────────────────────────────────┤
│         YOUNG GENERATION (1/3 of heap)          │
│  ┌──────────┬──────────────┬──────────────┐    │
│  │   Eden   │ Survivor S0  │ Survivor S1  │    │
│  │  (8/10)  │   (1/10)     │   (1/10)     │    │
│  └──────────┴──────────────┴──────────────┘    │
├─────────────────────────────────────────────────┤
│        OLD GENERATION (2/3 of heap)             │
│         (Long-lived objects)                    │
└─────────────────────────────────────────────────┘
```

**GC Types**:
- **Minor GC**: Cleans Young Generation
- **Major GC**: Cleans Old Generation
- **Full GC**: Cleans entire heap

---

## 2️⃣ Why Interviewers Ask This

- **Memory management**: Core Java concept
- **Performance**: GC affects application performance
- **Production issues**: GC tuning needed in real applications
- **Troubleshooting**: Understanding GC helps debug memory issues
- **Senior role indicator**: Advanced topic for experienced developers
- **Common problems**: Memory leaks, long pauses are production issues

---

## 3️⃣ Key Rules / Facts

**Making Objects Eligible for GC**:

| Method | Description | Example |
|--------|-------------|---------|
| Nullify reference | Set reference to null | `obj = null;` |
| Reassign reference | Point to different object | `obj = new Object();` |
| Local variable scope | Leave method scope | Method returns |
| Island of isolation | Circular references, no external refs | Objects reference each other |

**GC Algorithms** (Java 8):

| Algorithm | Description | Use Case |
|-----------|-------------|----------|
| Serial GC | Single-threaded | Small applications, single CPU |
| Parallel GC | Multiple threads (default Java 8) | Multi-core, throughput priority |
| CMS (Concurrent Mark Sweep) | Low pause time | Interactive applications |
| G1 GC (Garbage First) | Balanced (default Java 9+) | Large heaps, predictable pauses |

**Generational GC Process**:

1. **New objects** → Created in Eden space
2. **Minor GC** → Eden full → Live objects → Survivor S0
3. **Next Minor GC** → Live objects → Move to S1 (age++)
4. **Aging** → Objects alternate S0 ↔ S1, age increases
5. **Promotion** → Age threshold reached → Move to Old Gen
6. **Major GC** → Old Gen full → Collect old generation
7. **Full GC** → Both Young and Old collected (expensive!)

**GC Phases**:
- **Mark**: Identify live objects
- **Sweep**: Remove dead objects
- **Compact**: Defragment memory (optional)

**Object Reachability**:
- **Strongly Reachable**: Regular references
- **Softly Reachable**: SoftReference (cleared before OutOfMemoryError)
- **Weakly Reachable**: WeakReference (cleared at next GC)
- **Phantom Reachable**: PhantomReference (for cleanup)

**Important GC Facts**:
- You cannot force GC, only suggest with `System.gc()`
- `finalize()` is deprecated and unreliable (don't use)
- GC runs in separate low-priority threads
- GC is non-deterministic (unpredictable timing)
- Stop-the-world pauses affect all application threads
- Young generation GCs are fast, full GCs are slow

**JVM GC Parameters**:
- `-Xms`: Initial heap size
- `-Xmx`: Maximum heap size
- `-Xmn`: Young generation size
- `-XX:+UseSerialGC`: Use Serial GC
- `-XX:+UseParallelGC`: Use Parallel GC (default Java 8)
- `-XX:+UseConcMarkSweepGC`: Use CMS GC
- `-XX:+UseG1GC`: Use G1 GC
- `-XX:+PrintGCDetails`: Print GC logs
- `-verbose:gc`: Print GC information

**When Objects Become Eligible for GC**:
- No more references from root objects
- All references set to null
- Object created inside method and method returns
- Parent object eligible for GC (and children have no other refs)

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Making Objects Eligible for GC

```java
public class GCEligibility {
    public static void main(String[] args) {
        // Method 1: Nullify reference
        String str1 = new String("Hello");
        str1 = null;  // Object eligible for GC
        
        // Method 2: Reassign reference
        String str2 = new String("World");
        str2 = new String("Java");  // "World" eligible for GC
        
        // Method 3: Object out of scope
        {
            String str3 = new String("Temporary");
            // str3 eligible for GC when this block ends
        }
        
        // Method 4: Method call
        createAndForget();  // Object created inside eligible after return
        
        // Method 5: Island of isolation
        createIslandOfIsolation();
        
        // Suggest GC (not guaranteed to run)
        System.gc();
        
        // Give GC time to run
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("Main method end");
    }
    
    static void createAndForget() {
        String local = new String("I will be GC'd");
        // When method returns, 'local' is removed from stack
        // Object becomes eligible for GC
    }
    
    static void createIslandOfIsolation() {
        // Two objects reference each other but no external reference
        Node node1 = new Node();
        Node node2 = new Node();
        
        node1.next = node2;
        node2.next = node1;
        
        // When method returns, both become eligible for GC
        // Even though they reference each other (island of isolation)
    }
    
    static class Node {
        Node next;
    }
}
```

### Example 2: Understanding GC with finalize() (Deprecated)

```java
public class FinalizeExample {
    private String name;
    
    public FinalizeExample(String name) {
        this.name = name;
    }
    
    // finalize() is DEPRECATED - don't use in production!
    // Shown here only for educational purposes
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Finalizing: " + name);
        super.finalize();
    }
    
    public static void main(String[] args) {
        FinalizeExample obj1 = new FinalizeExample("Object 1");
        FinalizeExample obj2 = new FinalizeExample("Object 2");
        
        obj1 = null;
        obj2 = null;
        
        System.out.println("Requesting GC...");
        System.gc();  // Request GC (not guaranteed)
        
        // Give time for finalize to run
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("Main end");
    }
}

/*
IMPORTANT NOTES:
1. finalize() is DEPRECATED and unreliable
2. GC timing is non-deterministic
3. finalize() may never be called
4. Use try-with-resources or explicit cleanup instead
5. System.gc() only suggests GC, doesn't force it
*/
```

### Example 3: Memory Leak Example

```java
import java.util.*;

public class MemoryLeakDemo {
    // Static collection - references never released
    private static List<byte[]> leakyList = new ArrayList<>();
    
    public static void main(String[] args) {
        demonstrateMemoryLeak();
        demonstrateProperMemoryManagement();
    }
    
    // BAD: Memory leak
    static void demonstrateMemoryLeak() {
        System.out.println("Creating memory leak...");
        
        // Adding objects to static list
        // Objects cannot be GC'd even when not needed
        for (int i = 0; i < 100; i++) {
            byte[] array = new byte[1024 * 1024];  // 1 MB
            leakyList.add(array);
        }
        
        // leakyList is static, so references persist
        // Memory cannot be reclaimed by GC!
        System.out.println("Leak created. Memory held: " + leakyList.size() + " MB");
    }
    
    // GOOD: Proper memory management
    static void demonstrateProperMemoryManagement() {
        System.out.println("Proper memory management...");
        
        List<byte[]> localList = new ArrayList<>();
        
        for (int i = 0; i < 100; i++) {
            byte[] array = new byte[1024 * 1024];  // 1 MB
            localList.add(array);
        }
        
        // When method returns, localList goes out of scope
        // Arrays eligible for GC
        System.out.println("Method end. Memory will be GC'd.");
    }
    
    // Common memory leak: Event listeners
    static class MemoryLeakListener {
        private List<Listener> listeners = new ArrayList<>();
        
        public void addListener(Listener listener) {
            listeners.add(listener);
            // If listeners are never removed, memory leak!
        }
        
        public void removeListener(Listener listener) {
            listeners.remove(listener);  // Important for preventing leaks
        }
        
        interface Listener {
            void onEvent();
        }
    }
    
    // Common memory leak: HashMap with no cleanup
    static class Cache {
        private static Map<String, Object> cache = new HashMap<>();
        
        public static void put(String key, Object value) {
            cache.put(key, value);
            // If entries are never removed, memory leak!
        }
        
        public static void clear() {
            cache.clear();  // Important for preventing leaks
        }
    }
}
```

### Example 4: Weak Reference Example

```java
import java.lang.ref.WeakReference;

public class WeakReferenceDemo {
    public static void main(String[] args) {
        // Strong reference - object won't be GC'd
        Object strongRef = new Object();
        
        // Weak reference - object can be GC'd
        WeakReference<Object> weakRef = new WeakReference<>(strongRef);
        
        System.out.println("Before GC:");
        System.out.println("Strong ref: " + strongRef);
        System.out.println("Weak ref: " + weakRef.get());
        
        // Remove strong reference
        strongRef = null;
        
        // Request GC
        System.gc();
        
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("\nAfter GC:");
        System.out.println("Weak ref: " + weakRef.get());  // Likely null
        
        // Practical example: Cache with weak references
        demonstrateWeakCache();
    }
    
    static void demonstrateWeakCache() {
        System.out.println("\n=== Weak Cache Demo ===");
        
        // Cache using weak references
        Map<String, WeakReference<byte[]>> cache = new HashMap<>();
        
        // Add large object
        byte[] data = new byte[1024 * 1024];  // 1 MB
        cache.put("data", new WeakReference<>(data));
        
        System.out.println("Before GC: " + cache.get("data").get());
        
        // Remove strong reference
        data = null;
        
        // Request GC
        System.gc();
        
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("After GC: " + cache.get("data").get());  // Likely null
    }
}
```

### Example 5: Monitoring GC Activity

```java
public class GCMonitoring {
    public static void main(String[] args) {
        Runtime runtime = Runtime.getRuntime();
        
        System.out.println("=== Memory Before Object Creation ===");
        printMemoryStats(runtime);
        
        // Create many objects
        List<byte[]> list = new ArrayList<>();
        for (int i = 0; i < 100; i++) {
            byte[] array = new byte[1024 * 1024];  // 1 MB each
            list.add(array);
        }
        
        System.out.println("\n=== Memory After Object Creation ===");
        printMemoryStats(runtime);
        
        // Clear references
        list.clear();
        list = null;
        
        System.out.println("\n=== Memory After Clearing References ===");
        printMemoryStats(runtime);
        
        // Request GC
        System.gc();
        
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("\n=== Memory After GC ===");
        printMemoryStats(runtime);
    }
    
    static void printMemoryStats(Runtime runtime) {
        long totalMemory = runtime.totalMemory();
        long freeMemory = runtime.freeMemory();
        long usedMemory = totalMemory - freeMemory;
        long maxMemory = runtime.maxMemory();
        
        System.out.println("Total Memory: " + (totalMemory / 1024 / 1024) + " MB");
        System.out.println("Free Memory:  " + (freeMemory / 1024 / 1024) + " MB");
        System.out.println("Used Memory:  " + (usedMemory / 1024 / 1024) + " MB");
        System.out.println("Max Memory:   " + (maxMemory / 1024 / 1024) + " MB");
    }
}

/*
To see GC logs, run with:
java -verbose:gc GCMonitoring
or
java -XX:+PrintGCDetails GCMonitoring
*/
```

### Example 6: Understanding Generations

```java
public class GenerationalGCDemo {
    public static void main(String[] args) {
        demonstrateYoungGeneration();
        demonstrateOldGeneration();
    }
    
    // Most objects die young - collected quickly
    static void demonstrateYoungGeneration() {
        System.out.println("=== Young Generation Demo ===");
        
        for (int i = 0; i < 1000; i++) {
            // Create temporary objects
            String temp = new String("Temporary " + i);
            // temp immediately eligible for GC after each iteration
        }
        
        System.out.println("Temporary objects created and eligible for GC");
        // These will be collected in Minor GC (Young Generation)
    }
    
    // Few objects survive to old generation
    static void demonstrateOldGeneration() {
        System.out.println("\n=== Old Generation Demo ===");
        
        // Long-lived objects
        List<String> longLived = new ArrayList<>();
        
        for (int i = 0; i < 1000; i++) {
            longLived.add("Long lived " + i);
            // Objects referenced by list survive Minor GCs
            // Eventually promoted to Old Generation
        }
        
        System.out.println("Long-lived objects: " + longLived.size());
        // These survive multiple Minor GCs and move to Old Gen
    }
}

/*
GENERATIONAL GC FLOW:

1. New objects created in Eden (Young Gen)
2. Eden fills up → Minor GC triggered
3. Live objects → Survivor S0
4. Next Minor GC → Live objects → Survivor S1
5. Objects survive multiple GCs → Promoted to Old Gen
6. Old Gen fills up → Major GC triggered
7. Full heap full → Full GC (expensive!)

Minor GC: Fast, frequent
Major GC: Slow, less frequent
Full GC: Very slow, rare
*/
```

### Example 7: String Pool and GC

```java
public class StringPoolGC {
    public static void main(String[] args) {
        // String literals - go to String Pool
        String s1 = "Hello";
        String s2 = "Hello";  // Same reference as s1
        
        System.out.println(s1 == s2);  // true
        
        // String with new - not in pool initially
        String s3 = new String("Hello");
        System.out.println(s1 == s3);  // false
        
        // intern() - add to pool or get from pool
        String s4 = s3.intern();
        System.out.println(s1 == s4);  // true
        
        demonstrateStringGC();
    }
    
    static void demonstrateStringGC() {
        System.out.println("\n=== String GC Demo ===");
        
        // String literals in pool - rarely GC'd
        String literal = "Pooled String";
        
        // String objects - can be GC'd
        String object1 = new String("Object String 1");
        String object2 = new String("Object String 2");
        
        object1 = null;  // Eligible for GC
        object2 = null;  // Eligible for GC
        
        // String Pool strings persist (unless no references and pool is collected)
        System.out.println("String objects eligible for GC");
        System.out.println("Pooled string persists: " + literal);
    }
}

/*
STRING POOL NOTES:
- String Pool in heap (Java 7+)
- String literals added to pool automatically
- Strings in pool are not immediately GC'd
- Pool can be GC'd if strings unreachable
- Use intern() carefully - can cause memory issues
*/
```

### Example 8: Common GC Scenarios

```java
import java.util.*;

public class CommonGCScenarios {
    public static void main(String[] args) {
        scenario1_NullifyReference();
        scenario2_ReassignReference();
        scenario3_IslandOfIsolation();
        scenario4_AnonymousObject();
    }
    
    // Scenario 1: Nullify reference
    static void scenario1_NullifyReference() {
        System.out.println("=== Scenario 1: Nullify Reference ===");
        
        Object obj = new Object();
        System.out.println("Object created: " + obj);
        
        obj = null;  // Object eligible for GC
        System.out.println("Reference nullified - object eligible for GC\n");
    }
    
    // Scenario 2: Reassign reference
    static void scenario2_ReassignReference() {
        System.out.println("=== Scenario 2: Reassign Reference ===");
        
        String str = new String("First");
        System.out.println("First string: " + str);
        
        str = new String("Second");  // "First" eligible for GC
        System.out.println("Second string: " + str);
        System.out.println("First string eligible for GC\n");
    }
    
    // Scenario 3: Island of isolation
    static void scenario3_IslandOfIsolation() {
        System.out.println("=== Scenario 3: Island of Isolation ===");
        
        Node n1 = new Node("Node 1");
        Node n2 = new Node("Node 2");
        
        n1.next = n2;
        n2.next = n1;  // Circular reference
        
        System.out.println("Circular reference created");
        
        n1 = null;
        n2 = null;  // Both eligible for GC despite circular reference
        
        System.out.println("External references removed - both eligible for GC\n");
    }
    
    // Scenario 4: Anonymous object
    static void scenario4_AnonymousObject() {
        System.out.println("=== Scenario 4: Anonymous Object ===");
        
        // Anonymous object - immediately eligible for GC
        new String("Anonymous").length();
        
        System.out.println("Anonymous object created and used");
        System.out.println("Immediately eligible for GC\n");
    }
    
    static class Node {
        String data;
        Node next;
        
        Node(String data) {
            this.data = data;
        }
    }
}
```

### Example 9: Preventing Memory Leaks

```java
import java.util.*;

public class PreventMemoryLeaks {
    public static void main(String[] args) {
        demonstrateCollectionLeak();
        demonstrateListenerLeak();
        demonstrateThreadLocalLeak();
    }
    
    // Leak 1: Collections not cleared
    static void demonstrateCollectionLeak() {
        System.out.println("=== Collection Leak Prevention ===");
        
        List<Object> list = new ArrayList<>();
        
        for (int i = 0; i < 1000; i++) {
            list.add(new Object());
        }
        
        // BAD: Not clearing when done
        // list.clear();  // GOOD: Clear when done
        
        System.out.println("Collection size: " + list.size());
        
        // SOLUTION: Clear collection
        list.clear();
        System.out.println("Collection cleared: " + list.size() + "\n");
    }
    
    // Leak 2: Event listeners not removed
    static void demonstrateListenerLeak() {
        System.out.println("=== Listener Leak Prevention ===");
        
        EventManager manager = new EventManager();
        Listener listener = new Listener();
        
        // Register listener
        manager.addListener(listener);
        System.out.println("Listener added");
        
        // BAD: Not removing listener
        // manager.removeListener(listener);  // GOOD: Remove when done
        
        // SOLUTION: Always remove listeners
        manager.removeListener(listener);
        System.out.println("Listener removed\n");
    }
    
    // Leak 3: ThreadLocal not cleaned
    static void demonstrateThreadLocalLeak() {
        System.out.println("=== ThreadLocal Leak Prevention ===");
        
        ThreadLocal<Object> threadLocal = new ThreadLocal<>();
        threadLocal.set(new Object());
        
        System.out.println("ThreadLocal set");
        
        // BAD: Not clearing ThreadLocal
        // threadLocal.remove();  // GOOD: Remove when done
        
        // SOLUTION: Always remove ThreadLocal
        threadLocal.remove();
        System.out.println("ThreadLocal cleared\n");
    }
    
    static class EventManager {
        private List<Listener> listeners = new ArrayList<>();
        
        void addListener(Listener listener) {
            listeners.add(listener);
        }
        
        void removeListener(Listener listener) {
            listeners.remove(listener);
        }
    }
    
    static class Listener {
        // Listener implementation
    }
}
```

### Example 10: GC Best Practices

```java
import java.util.*;

public class GCBestPractices {
    public static void main(String[] args) {
        practice1_MinimizeObjectCreation();
        practice2_ReuseObjects();
        practice3_UseStringBuilder();
        practice4_ClearReferences();
        practice5_UseAppropriateCollections();
    }
    
    // Practice 1: Minimize object creation
    static void practice1_MinimizeObjectCreation() {
        // BAD: Creating unnecessary objects
        for (int i = 0; i < 1000; i++) {
            String temp = new String("Temp");  // Unnecessary
        }
        
        // GOOD: Reuse or use literals
        String reused = "Reused";
        for (int i = 0; i < 1000; i++) {
            // Use reused string
        }
    }
    
    // Practice 2: Reuse objects when possible
    static void practice2_ReuseObjects() {
        // BAD: Creating new objects in loop
        for (int i = 0; i < 1000; i++) {
            StringBuilder sb = new StringBuilder();
            sb.append("Value: ").append(i);
        }
        
        // GOOD: Reuse object
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 1000; i++) {
            sb.setLength(0);  // Clear
            sb.append("Value: ").append(i);
        }
    }
    
    // Practice 3: Use StringBuilder for string concatenation
    static void practice3_UseStringBuilder() {
        // BAD: String concatenation in loop
        String result = "";
        for (int i = 0; i < 100; i++) {
            result += i;  // Creates new String each iteration
        }
        
        // GOOD: Use StringBuilder
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 100; i++) {
            sb.append(i);
        }
        String betterResult = sb.toString();
    }
    
    // Practice 4: Clear references when done
    static void practice4_ClearReferences() {
        List<Object> list = new ArrayList<>();
        
        // Add objects
        for (int i = 0; i < 1000; i++) {
            list.add(new Object());
        }
        
        // GOOD: Clear when done
        list.clear();
        
        // Or set to null if not needed
        list = null;
    }
    
    // Practice 5: Use appropriate collections
    static void practice5_UseAppropriateCollections() {
        // BAD: ArrayList for frequent removals
        List<Integer> badList = new ArrayList<>();
        for (int i = 0; i < 1000; i++) {
            badList.add(i);
        }
        badList.remove(0);  // Slow for ArrayList
        
        // GOOD: LinkedList for frequent removals
        List<Integer> goodList = new LinkedList<>();
        for (int i = 0; i < 1000; i++) {
            goodList.add(i);
        }
        goodList.remove(0);  // Fast for LinkedList
        
        // GOOD: Use initial capacity if size known
        List<Integer> optimized = new ArrayList<>(1000);
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is Garbage Collection in Java?**
2. **How does Garbage Collection work?**
3. **When is an object eligible for Garbage Collection?**
4. **What is the difference between Minor GC and Major GC?**
5. **Can you force Garbage Collection in Java?**
6. **What is the purpose of the finalize() method?**
7. **What are the different types of references in Java?**
8. **What is a memory leak in Java?**
9. **What are the different GC algorithms in Java?**
10. **What is the generational hypothesis in GC?**

---

## 6️⃣ Model Answers

**Q1: What is Garbage Collection in Java?**

"Garbage Collection is the automatic memory management process where the JVM identifies and removes objects that are no longer needed, freeing heap memory. Unlike languages like C/C++, Java developers don't manually free memory. The GC identifies unreachable objects—those with no references from GC roots like local variables, static variables, or active threads—and reclaims their memory. This prevents memory leaks and simplifies programming. GC runs in background threads and is non-deterministic, meaning you can't predict exactly when it runs. While automatic, GC does cause stop-the-world pauses that temporarily halt application threads."

**Q2: How does Garbage Collection work?**

"GC works in phases: Mark, Sweep, and optionally Compact. In the Mark phase, the GC identifies live objects by traversing from GC roots—local variables on stack, static variables, active threads. It marks all reachable objects. In the Sweep phase, unmarked objects are considered dead and their memory is reclaimed. In the Compact phase (optional), memory is defragmented by moving live objects together. Java uses generational GC, dividing heap into Young and Old generations. New objects start in Eden in Young generation. When Eden fills, Minor GC moves live objects to Survivor spaces. Objects that survive multiple Minor GCs are promoted to Old generation. When Old generation fills, Major GC occurs, which is more expensive."

**Q3: When is an object eligible for Garbage Collection?**

"An object is eligible for GC when it's unreachable from any GC root. This happens when: all references to it are set to null, the reference is reassigned to another object, the object was created in a method and the method returns, or it's part of an island of isolation—objects that reference each other but have no external references. For example, if you create an object in a method as a local variable and don't return it, it becomes eligible when the method ends. The key is that there must be no path from any GC root to the object. Even if objects reference each other circularly, they're eligible if no external reference exists."

**Q4: What is the difference between Minor GC and Major GC?**

"Minor GC cleans the Young Generation and is fast and frequent. It occurs when Eden space fills up. Live objects are moved to Survivor spaces or promoted to Old Generation. Minor GC is quick because most objects die young. Major GC, also called Full GC, cleans the Old Generation and is slow and less frequent. It occurs when Old Generation fills up. Major GC takes longer because it processes long-lived objects and often involves compacting memory. Full GC can cause noticeable application pauses. The generational approach is based on the observation that most objects die young, so Minor GC efficiently handles the common case while Major GC handles the exceptional case of long-lived objects."

**Q5: Can you force Garbage Collection in Java?**

"No, you cannot force GC. You can only suggest it using System.gc() or Runtime.getRuntime().gc(), but the JVM may ignore these requests. GC timing is non-deterministic and controlled by the JVM. The JVM decides when to run GC based on memory pressure and GC algorithm. Calling System.gc() is generally discouraged because it can trigger Full GC at inappropriate times, causing unnecessary pauses. The JVM's automatic GC typically makes better decisions about when to collect. In production code, you should never rely on System.gc(). The only valid uses are testing, debugging, or very specific scenarios where you know a large number of objects just became unreachable."

**Q6: What is the purpose of the finalize() method?**

"The finalize() method was designed for cleanup before an object is garbage collected, but it's deprecated as of Java 9 and should not be used. It has major problems: it's non-deterministic—you can't predict when or if it runs; it can prevent objects from being collected if finalize() creates new references; it's slow—objects with finalize() take at least two GC cycles to collect; and it's unreliable—finalize() might never be called. Instead, use try-with-resources for resources like files and connections, explicit close() methods, or Cleaner class in Java 9+. The finalize() method is a historical relic that causes more problems than it solves."

**Q7: What are the different types of references in Java?**

"Java has four types of references with different GC behavior. Strong references are normal references—objects with strong references are never collected. Soft references, created with SoftReference class, are cleared before OutOfMemoryError, useful for memory-sensitive caches. Weak references, created with WeakReference, are cleared at the next GC regardless of memory, useful for canonical mapping. Phantom references, created with PhantomReference, are the weakest—get() always returns null, used for cleanup actions. The GC respects this hierarchy: strong references prevent collection, soft references allow collection under memory pressure, weak references allow collection at any GC, and phantom references are collected but allow post-mortem cleanup."

**Q8: What is a memory leak in Java?**

"A memory leak occurs when objects that are no longer needed cannot be garbage collected because references to them still exist. Even though Java has automatic GC, you can still leak memory by holding unnecessary references. Common causes include: static collections that grow unbounded and never clear, listeners or callbacks that aren't removed, ThreadLocal variables not cleaned up, and caches without expiration policies. For example, adding objects to a static ArrayList without ever removing them prevents those objects from being collected. The solution is to null out references, remove listeners, clear collections, use weak references for caches, and properly clean ThreadLocal variables. Memory profilers help identify leaks."

**Q9: What are the different GC algorithms in Java?**

"Java has several GC algorithms optimized for different scenarios. Serial GC is single-threaded, suitable for small applications with single CPU, low memory footprint but causes long pauses. Parallel GC uses multiple threads for Young and Old generation collection, good for throughput but still has pauses—it's the default in Java 8. CMS (Concurrent Mark Sweep) performs most work concurrently with application threads for low pause times, good for interactive applications but uses more CPU and can fragment memory. G1 GC (Garbage First) divides heap into regions, offers predictable pause times, and handles large heaps well—it's the default from Java 9+. You select algorithms with -XX flags like -XX:+UseG1GC."

**Q10: What is the generational hypothesis in GC?**

"The generational hypothesis is the observation that most objects die young—they're used briefly and become unreachable quickly. Java's generational GC exploits this by dividing heap into Young and Old generations. New objects are created in Young Generation's Eden space. Since most objects die young, Eden fills frequently but most objects are dead by the time Minor GC runs, so collection is fast. The few survivors move to Survivor spaces and age. Objects that survive multiple Minor GCs prove they're long-lived and get promoted to Old Generation, which is collected less frequently. This approach optimizes the common case—short-lived objects—with fast Minor GCs while handling the exceptional case—long-lived objects—with less frequent Major GCs."

---

## 7️⃣ Common Mistakes

1. **Calling System.gc() in production code**:
   ```java
   // BAD
   System.gc();  // Don't force GC!
   
   // GOOD
   // Let JVM decide when to run GC
   ```

2. **Relying on finalize() for cleanup**:
   ```java
   // BAD
   protected void finalize() {
       // Close resources - unreliable!
   }
   
   // GOOD
   try (Resource r = new Resource()) {
       // Use try-with-resources
   }
   ```

3. **Holding references in static collections**:
   ```java
   // BAD
   static List<Object> list = new ArrayList<>();
   list.add(obj);  // Never removed - memory leak!
   
   // GOOD
   list.clear();  // Clear when done
   ```

4. **Not removing listeners**:
   ```java
   // BAD
   button.addListener(listener);
   // Never removed - memory leak!
   
   // GOOD
   button.removeListener(listener);
   ```

5. **Creating unnecessary objects in loops**:
   ```java
   // BAD
   for (int i = 0; i < 1000; i++) {
       String s = new String("Value");  // Unnecessary
   }
   
   // GOOD
   String s = "Value";  // Reuse
   ```

6. **Not clearing ThreadLocal**:
   ```java
   // BAD
   threadLocal.set(value);
   // Never removed - leak in thread pools!
   
   // GOOD
   threadLocal.remove();
   ```

7. **Assuming GC timing**:
   ```java
   // BAD
   obj = null;
   // Assume it's collected now - wrong!
   
   // GOOD
   obj = null;
   // Object eligible, collected when JVM decides
   ```

8. **Over-using intern()**:
   ```java
   // BAD
   for (String s : manyStrings) {
       s.intern();  // Can fill String Pool!
   }
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- GC is automatic memory management
- Objects eligible when unreachable from GC roots
- Generational: Young (Minor GC) and Old (Major GC)
- Cannot force GC, only suggest with System.gc()
- finalize() is deprecated, don't use
- Memory leaks possible from holding unnecessary references

**What to avoid**:
- Don't claim GC is deterministic (it's not)
- Don't say you can force GC (you can only suggest)
- Don't recommend using finalize()
- Don't forget that memory leaks are possible in Java

**Under pressure**:
- Explain: "Automatic process that reclaims memory from unreachable objects"
- State: "Object eligible when no references from GC roots"
- Mention: "Young Generation (fast, frequent) vs Old Generation (slow, rare)"
- Reference: "System.gc() only suggests, doesn't force"

**Red flags to avoid**:
- "GC happens immediately when object is null" (non-deterministic)
- "System.gc() forces garbage collection" (only suggests)
- "Use finalize() for cleanup" (deprecated, unreliable)
- "Java has no memory leaks" (can leak by holding references)

**Bonus points**:
- Mention GC roots: local vars, static vars, active threads
- Discuss stop-the-world pauses and their impact
- Reference different GC algorithms (Serial, Parallel, CMS, G1)
- Mention weak/soft references for caches
- Discuss memory leak prevention (clear collections, remove listeners)
- Reference JVM flags: -Xms, -Xmx, -verbose:gc
- Mention island of isolation concept
- Discuss generational hypothesis and why it works
