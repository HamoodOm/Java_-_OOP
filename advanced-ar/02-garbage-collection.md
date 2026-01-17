<div dir="rtl">

# جمع المخلفات (Garbage Collection)

## 1️⃣ نظرة عامة على المفهوم

**Garbage Collection (GC)** هي عملية إدارة الذاكرة التلقائية في Java حيث يحدد JVM الكائنات التي لم تعد مطلوبة ويزيلها، محرراً ذاكرة Heap.

**الخصائص الرئيسية**:
- **تلقائية**: لا حاجة لتحرير الذاكرة يدوياً (على عكس C/C++)
- **تستعيد الذاكرة غير المستخدمة**: تحرر الكائنات بدون مراجع
- **تمنع تسرب الذاكرة**: تزيل الكائنات غير القابلة للوصول
- **توقف العالم**: بعض مراحل GC توقف threads التطبيق
- **غير حتمية**: لا يمكنك التنبؤ بالضبط متى يعمل GC

**لماذا Garbage Collection؟**:
- تلغي إدارة الذاكرة اليدوية
- تمنع تسرب الذاكرة (غالباً)
- تمنع المؤشرات المعلقة
- تبسط البرمجة

**أساسيات GC**:
```
Object eligible for GC when:
1. No references point to it
2. All references are null
3. Object is unreachable from root
```

**الكائنات الجذرية** (GC Roots):
- المتغيرات المحلية على Stack
- الـ Threads النشطة
- المتغيرات الـ static
- مراجع JNI

**فرضية الأجيال**:
- معظم الكائنات تموت صغيرة
- قليل من الكائنات تعيش لسن كبير
- فصل الأجيال الصغيرة والكبيرة يحسن GC

**بنية Heap** (Generational GC):
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

**أنواع GC**:
- **Minor GC**: ينظف الجيل الصغير
- **Major GC**: ينظف الجيل الكبير
- **Full GC**: ينظف كل الـ Heap

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **إدارة الذاكرة**: مفهوم Java أساسي
- **الأداء**: GC يؤثر على أداء التطبيق
- **مشاكل الإنتاج**: ضبط GC مطلوب في التطبيقات الحقيقية
- **استكشاف الأخطاء**: فهم GC يساعد في تصحيح مشاكل الذاكرة
- **مؤشر للمستوى المتقدم**: موضوع متقدم للمطورين ذوي الخبرة
- **مشاكل شائعة**: تسرب الذاكرة والتوقفات الطويلة مشاكل إنتاج

---

## 3️⃣ القواعد والحقائق الأساسية

**جعل الكائنات مؤهلة لـ GC**:

| الطريقة | الوصف | مثال |
|--------|-------|-------|
| إلغاء المرجع | جعل المرجع null | `obj = null;` |
| إعادة تعيين المرجع | الإشارة لكائن مختلف | `obj = new Object();` |
| نطاق المتغير المحلي | مغادرة نطاق الدالة | انتهاء الدالة |
| جزيرة العزل | مراجع دائرية بدون مراجع خارجية | كائنات تشير لبعضها |

**خوارزميات GC** (Java 8):

| الخوارزمية | الوصف | حالة الاستخدام |
|-----------|-------|------------|
| Serial GC | Thread واحد | تطبيقات صغيرة، CPU واحد |
| Parallel GC | threads متعددة (افتراضي Java 8) | متعدد النوى، أولوية الإنتاجية |
| CMS (Concurrent Mark Sweep) | وقت توقف منخفض | تطبيقات تفاعلية |
| G1 GC (Garbage First) | متوازن (افتراضي Java 9+) | heaps كبيرة، توقفات متوقعة |

**عملية Generational GC**:

1. **كائنات جديدة** ← تُنشأ في Eden
2. **Minor GC** ← Eden ممتلئ ← الكائنات الحية ← Survivor S0
3. **Minor GC التالي** ← الكائنات الحية ← تنتقل إلى S1 (العمر++)
4. **التقدم بالعمر** ← الكائنات تتبادل S0 ↔ S1، العمر يزداد
5. **الترقية** ← الوصول لعتبة العمر ← الانتقال للجيل الكبير
6. **Major GC** ← الجيل الكبير ممتلئ ← تنظيف الجيل الكبير
7. **Full GC** ← الصغير والكبير يُنظفان (مكلف!)

**مراحل GC**:
- **Mark**: تحديد الكائنات الحية
- **Sweep**: إزالة الكائنات الميتة
- **Compact**: إلغاء تجزئة الذاكرة (اختياري)

**قابلية وصول الكائنات**:
- **Strongly Reachable**: مراجع عادية
- **Softly Reachable**: SoftReference (تُنظف قبل OutOfMemoryError)
- **Weakly Reachable**: WeakReference (تُنظف في GC التالي)
- **Phantom Reachable**: PhantomReference (للتنظيف)

**حقائق GC المهمة**:
- لا يمكنك إجبار GC، فقط اقتراحه بـ `System.gc()`
- `finalize()` مهمل وغير موثوق (لا تستخدمه)
- GC يعمل في threads منفصلة منخفضة الأولوية
- GC غير حتمي (توقيت غير متوقع)
- توقفات stop-the-world تؤثر على كل threads التطبيق
- GC الجيل الصغير سريع، full GC بطيء

**معاملات JVM لـ GC**:
- `-Xms`: حجم Heap الابتدائي
- `-Xmx`: حجم Heap الأقصى
- `-Xmn`: حجم الجيل الصغير
- `-XX:+UseSerialGC`: استخدام Serial GC
- `-XX:+UseParallelGC`: استخدام Parallel GC (افتراضي Java 8)
- `-XX:+UseConcMarkSweepGC`: استخدام CMS GC
- `-XX:+UseG1GC`: استخدام G1 GC
- `-XX:+PrintGCDetails`: طباعة سجلات GC
- `-verbose:gc`: طباعة معلومات GC

**متى تصبح الكائنات مؤهلة لـ GC**:
- لا مراجع من الكائنات الجذرية
- كل المراجع أصبحت null
- كائن أُنشئ داخل دالة وانتهت الدالة
- الكائن الأب مؤهل لـ GC (والأبناء ليس لهم مراجع أخرى)

---

## 4️⃣ أمثلة برمجية (Java 8)

### مثال 1: جعل الكائنات مؤهلة لـ GC

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

### مثال 2: فهم GC مع finalize() (مهمل)

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

### مثال 3: تسرب الذاكرة

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

### مثال 4: المراجع الضعيفة

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

### مثال 5: مراقبة نشاط GC

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

### مثال 6: فهم الأجيال

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

### مثال 7: String Pool و GC

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

### مثال 8: سيناريوهات GC الشائعة

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

### مثال 9: منع تسرب الذاكرة

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

### مثال 10: أفضل ممارسات GC

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

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما هو Garbage Collection في Java؟**
2. **كيف يعمل Garbage Collection؟**
3. **متى يصبح الكائن مؤهلاً لـ Garbage Collection؟**
4. **ما الفرق بين Minor GC و Major GC؟**
5. **هل يمكنك إجبار Garbage Collection في Java؟**
6. **ما هو الغرض من دالة finalize()؟**
7. **ما هي أنواع المراجع المختلفة في Java؟**
8. **ما هو تسرب الذاكرة في Java؟**
9. **ما هي خوارزميات GC المختلفة في Java؟**
10. **ما هي فرضية الأجيال في GC؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما هو Garbage Collection في Java؟**

"Garbage Collection هي عملية إدارة الذاكرة التلقائية حيث يحدد JVM الكائنات التي لم تعد مطلوبة ويزيلها، محرراً ذاكرة Heap. على عكس لغات مثل C/C++، مطورو Java لا يحررون الذاكرة يدوياً. الـ GC يحدد الكائنات غير القابلة للوصول - تلك التي ليس لها مراجع من GC roots مثل المتغيرات المحلية، والمتغيرات الـ static، أو الـ threads النشطة - ويستعيد ذاكرتها. هذا يمنع تسرب الذاكرة ويبسط البرمجة. GC يعمل في threads خلفية وهو غير حتمي، يعني لا يمكنك التنبؤ بالضبط متى يعمل. رغم أنه تلقائي، GC يسبب توقفات stop-the-world التي توقف مؤقتاً threads التطبيق."

**س2: كيف يعمل Garbage Collection؟**

"GC يعمل في مراحل: Mark و Sweep واختيارياً Compact. في مرحلة Mark، يحدد GC الكائنات الحية بالتنقل من GC roots - المتغيرات المحلية على stack، والمتغيرات الـ static، والـ threads النشطة. يُعلّم كل الكائنات القابلة للوصول. في مرحلة Sweep، الكائنات غير المُعلّمة تُعتبر ميتة وتُستعاد ذاكرتها. في مرحلة Compact (اختيارية)، يُلغى تجزئة الذاكرة بنقل الكائنات الحية معاً. Java تستخدم generational GC، تقسم Heap إلى جيل صغير وكبير. الكائنات الجديدة تبدأ في Eden في الجيل الصغير. عندما يمتلئ Eden، Minor GC ينقل الكائنات الحية إلى Survivor spaces. الكائنات التي تنجو من Minor GCs متعددة تُرقى للجيل الكبير. عندما يمتلئ الجيل الكبير، يحدث Major GC، وهو أكثر تكلفة."

**س3: متى يصبح الكائن مؤهلاً لـ Garbage Collection؟**

"الكائن يصبح مؤهلاً لـ GC عندما يكون غير قابل للوصول من أي GC root. هذا يحدث عندما: كل المراجع إليه تصبح null، أو المرجع يُعاد تعيينه لكائن آخر، أو الكائن أُنشئ في دالة وانتهت الدالة، أو هو جزء من جزيرة عزل - كائنات تشير لبعضها لكن ليس لها مراجع خارجية. مثلاً، إذا أنشأت كائناً في دالة كمتغير محلي ولم ترجعه، يصبح مؤهلاً عند انتهاء الدالة. المفتاح هو ألا يوجد مسار من أي GC root للكائن. حتى لو الكائنات تشير لبعضها دائرياً، تكون مؤهلة إذا لم يوجد مرجع خارجي."

**س4: ما الفرق بين Minor GC و Major GC؟**

"Minor GC ينظف الجيل الصغير وهو سريع ومتكرر. يحدث عندما يمتلئ Eden space. الكائنات الحية تُنقل إلى Survivor spaces أو تُرقى للجيل الكبير. Minor GC سريع لأن معظم الكائنات تموت صغيرة. Major GC، يُسمى أيضاً Full GC، ينظف الجيل الكبير وهو بطيء وأقل تكراراً. يحدث عندما يمتلئ الجيل الكبير. Major GC يأخذ وقتاً أطول لأنه يعالج كائنات طويلة العمر وغالباً يتضمن ضغط الذاكرة. Full GC يمكن أن يسبب توقفات ملحوظة في التطبيق. النهج الجيلي مبني على ملاحظة أن معظم الكائنات تموت صغيرة، فـ Minor GC يعالج الحالة الشائعة بكفاءة بينما Major GC يعالج الحالة الاستثنائية للكائنات طويلة العمر."

**س5: هل يمكنك إجبار Garbage Collection في Java؟**

"لا، لا يمكنك إجبار GC. يمكنك فقط اقتراحه باستخدام System.gc() أو Runtime.getRuntime().gc()، لكن JVM قد يتجاهل هذه الطلبات. توقيت GC غير حتمي ويتحكم به JVM. الـ JVM يقرر متى يشغل GC بناءً على ضغط الذاكرة وخوارزمية GC. استدعاء System.gc() غير مستحسن عموماً لأنه قد يُشغّل Full GC في أوقات غير مناسبة، مسبباً توقفات غير ضرورية. GC التلقائي لـ JVM عادة يتخذ قرارات أفضل حول متى يجمع. في كود الإنتاج، لا يجب أن تعتمد أبداً على System.gc(). الاستخدامات الصحيحة الوحيدة هي الاختبار، وتصحيح الأخطاء، أو سيناريوهات محددة جداً حيث تعرف أن عدداً كبيراً من الكائنات أصبحت غير قابلة للوصول."

**س6: ما هو الغرض من دالة finalize()؟**

"دالة finalize() صُممت للتنظيف قبل جمع الكائن، لكنها مهملة اعتباراً من Java 9 ولا يجب استخدامها. لها مشاكل كبيرة: غير حتمية - لا يمكنك التنبؤ متى أو إذا ستعمل؛ يمكن أن تمنع جمع الكائنات إذا أنشأت finalize() مراجع جديدة؛ بطيئة - كائنات مع finalize() تأخذ على الأقل دورتين GC للجمع؛ وغير موثوقة - finalize() قد لا تُستدعى أبداً. بدلاً من ذلك، استخدم try-with-resources للموارد مثل الملفات والاتصالات، ودوال close() صريحة، أو Cleaner class في Java 9+. دالة finalize() هي بقية تاريخية تسبب مشاكل أكثر مما تحل."

**س7: ما هي أنواع المراجع المختلفة في Java؟**

"Java لديها أربعة أنواع من المراجع بسلوكيات GC مختلفة. المراجع القوية هي المراجع العادية - الكائنات بمراجع قوية لا تُجمع أبداً. المراجع الناعمة، تُنشأ بـ SoftReference class، تُنظف قبل OutOfMemoryError، مفيدة للـ caches الحساسة للذاكرة. المراجع الضعيفة، تُنشأ بـ WeakReference، تُنظف في GC التالي بغض النظر عن الذاكرة، مفيدة للـ canonical mapping. المراجع الوهمية، تُنشأ بـ PhantomReference، هي الأضعف - get() دائماً ترجع null، تُستخدم لإجراءات التنظيف. الـ GC يحترم هذا التسلسل: المراجع القوية تمنع الجمع، المراجع الناعمة تسمح بالجمع تحت ضغط الذاكرة، المراجع الضعيفة تسمح بالجمع في أي GC، والمراجع الوهمية تُجمع لكن تسمح بتنظيف بعد الموت."

**س8: ما هو تسرب الذاكرة في Java؟**

"تسرب الذاكرة يحدث عندما الكائنات التي لم تعد مطلوبة لا يمكن جمعها لأن مراجع إليها لا تزال موجودة. رغم أن Java لديها GC تلقائي، يمكنك تسريب الذاكرة بالاحتفاظ بمراجع غير ضرورية. الأسباب الشائعة تشمل: مجموعات static تنمو بلا حدود ولا تُنظف أبداً، مستمعين أو callbacks لا تُزال، متغيرات ThreadLocal لا تُنظف، وcaches بدون سياسات انتهاء. مثلاً، إضافة كائنات لـ ArrayList static بدون إزالتها أبداً تمنع جمع تلك الكائنات. الحل هو إلغاء المراجع، وإزالة المستمعين، وتنظيف المجموعات، واستخدام المراجع الضعيفة للـ caches، وتنظيف متغيرات ThreadLocal بشكل صحيح. أدوات تحليل الذاكرة تساعد في تحديد التسربات."

**س9: ما هي خوارزميات GC المختلفة في Java؟**

"Java لديها عدة خوارزميات GC محسنة لسيناريوهات مختلفة. Serial GC هو thread واحد، مناسب لتطبيقات صغيرة مع CPU واحد، استهلاك ذاكرة منخفض لكن توقفات طويلة. Parallel GC يستخدم threads متعددة لجمع الجيل الصغير والكبير، جيد للإنتاجية لكن لا يزال له توقفات - هو الافتراضي في Java 8. CMS (Concurrent Mark Sweep) يؤدي معظم العمل بشكل متزامن مع threads التطبيق لتوقفات منخفضة، جيد للتطبيقات التفاعلية لكن يستخدم CPU أكثر ويمكن أن يجزئ الذاكرة. G1 GC (Garbage First) يقسم Heap إلى مناطق، يقدم توقفات متوقعة، ويعالج heaps كبيرة بشكل جيد - هو الافتراضي من Java 9+. تختار الخوارزميات بأعلام -XX مثل -XX:+UseG1GC."

**س10: ما هي فرضية الأجيال في GC؟**

"فرضية الأجيال هي الملاحظة أن معظم الكائنات تموت صغيرة - تُستخدم لفترة قصيرة وتصبح غير قابلة للوصول بسرعة. GC الجيلي في Java يستغل هذا بتقسيم Heap إلى جيل صغير وكبير. الكائنات الجديدة تُنشأ في Eden space بالجيل الصغير. بما أن معظم الكائنات تموت صغيرة، Eden يمتلئ كثيراً لكن معظم الكائنات تكون ميتة بوقت عمل Minor GC، فالجمع يكون سريعاً. الناجون القليلون ينتقلون لـ Survivor spaces ويكبرون بالعمر. الكائنات التي تنجو من Minor GCs متعددة تُثبت أنها طويلة العمر وتُرقى للجيل الكبير، الذي يُجمع أقل. هذا النهج يُحسّن الحالة الشائعة - الكائنات قصيرة العمر - بـ Minor GCs سريعة بينما يعالج الحالة الاستثنائية - الكائنات طويلة العمر - بـ Major GCs أقل تكراراً."

---

## 7️⃣ الأخطاء الشائعة

1. **استدعاء System.gc() في كود الإنتاج**:
   ```java
   // BAD
   System.gc();  // Don't force GC!

   // GOOD
   // Let JVM decide when to run GC
   ```

2. **الاعتماد على finalize() للتنظيف**:
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

3. **الاحتفاظ بمراجع في مجموعات static**:
   ```java
   // BAD
   static List<Object> list = new ArrayList<>();
   list.add(obj);  // Never removed - memory leak!

   // GOOD
   list.clear();  // Clear when done
   ```

4. **عدم إزالة المستمعين**:
   ```java
   // BAD
   button.addListener(listener);
   // Never removed - memory leak!

   // GOOD
   button.removeListener(listener);
   ```

5. **إنشاء كائنات غير ضرورية في الحلقات**:
   ```java
   // BAD
   for (int i = 0; i < 1000; i++) {
       String s = new String("Value");  // Unnecessary
   }

   // GOOD
   String s = "Value";  // Reuse
   ```

6. **عدم تنظيف ThreadLocal**:
   ```java
   // BAD
   threadLocal.set(value);
   // Never removed - leak in thread pools!

   // GOOD
   threadLocal.remove();
   ```

7. **افتراض توقيت GC**:
   ```java
   // BAD
   obj = null;
   // Assume it's collected now - wrong!

   // GOOD
   obj = null;
   // Object eligible, collected when JVM decides
   ```

8. **الإفراط في استخدام intern()**:
   ```java
   // BAD
   for (String s : manyStrings) {
       s.intern();  // Can fill String Pool!
   }
   ```

---

## 8️⃣ نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- GC هو إدارة ذاكرة تلقائية
- الكائنات مؤهلة عندما تكون غير قابلة للوصول من GC roots
- جيلي: صغير (Minor GC) وكبير (Major GC)
- لا يمكن إجبار GC، فقط اقتراحه بـ System.gc()
- finalize() مهمل، لا تستخدمه
- تسرب الذاكرة ممكن من الاحتفاظ بمراجع غير ضرورية

**ما يجب تجنبه**:
- لا تدعي أن GC حتمي (ليس كذلك)
- لا تقل أنك تستطيع إجبار GC (يمكنك فقط الاقتراح)
- لا توصي باستخدام finalize()
- لا تنسَ أن تسرب الذاكرة ممكن في Java

**تحت الضغط**:
- اشرح: "عملية تلقائية تستعيد الذاكرة من الكائنات غير القابلة للوصول"
- قل: "الكائن مؤهل عندما لا توجد مراجع من GC roots"
- اذكر: "الجيل الصغير (سريع، متكرر) مقابل الجيل الكبير (بطيء، نادر)"
- أشر: "System.gc() يقترح فقط، لا يُجبر"

**علامات تحذيرية يجب تجنبها**:
- "GC يحدث فوراً عندما يكون الكائن null" (غير حتمي)
- "System.gc() يُجبر garbage collection" (يقترح فقط)
- "استخدم finalize() للتنظيف" (مهمل، غير موثوق)
- "Java ليس لها تسرب ذاكرة" (يمكن التسرب بالاحتفاظ بمراجع)

**نقاط إضافية**:
- اذكر GC roots: المتغيرات المحلية، المتغيرات الـ static، الـ threads النشطة
- ناقش توقفات stop-the-world وتأثيرها
- أشر لخوارزميات GC المختلفة (Serial, Parallel, CMS, G1)
- اذكر المراجع الضعيفة/الناعمة للـ caches
- ناقش منع تسرب الذاكرة (تنظيف المجموعات، إزالة المستمعين)
- أشر لأعلام JVM: -Xms, -Xmx, -verbose:gc
- اذكر مفهوم جزيرة العزل
- ناقش فرضية الأجيال ولماذا تعمل

</div>
