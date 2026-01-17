<div dir="rtl">

# الذاكرة: المكدس مقابل الكومة

## 1️⃣ نظرة عامة على المفهوم

**إدارة الذاكرة** في Java تتضمن فهم كيفية تخصيص JVM للذاكرة وإدارتها لأنواع البيانات المختلفة. المنطقتان الرئيسيتان هما **Stack** و **Heap**.

**ذاكرة Stack**:
- تخزن استدعاءات الدوال والمتغيرات المحلية
- بنية LIFO (آخر ما يدخل أول ما يخرج)
- خاصة بكل Thread (كل Thread له Stack خاص به)
- تُدار تلقائياً (تُحذف المتغيرات تلقائياً عند انتهاء الدالة)
- وصول أسرع من Heap
- حجم محدود (قد تسبب StackOverflowError)

**ذاكرة Heap**:
- تخزن الكائنات ومتغيرات الـ instance
- مشتركة بين جميع الـ Threads
- يديرها Garbage Collector
- أكبر من Stack
- وصول أبطأ من Stack
- قد تسبب OutOfMemoryError عند الامتلاء

**تخطيط الذاكرة**:
```
┌─────────────────────────────────────┐
│         HEAP MEMORY                 │
│  - Objects                          │
│  - Instance variables               │
│  - String Pool                      │
│  - Shared by all threads            │
│  - Garbage Collected                │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│         STACK MEMORY                │
│  Thread 1 Stack                     │
│  - Method calls                     │
│  - Local variables                  │
│  - References to heap objects       │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│  Thread 2 Stack                     │
│  - Method calls                     │
│  - Local variables                  │
└─────────────────────────────────────┘
```

**Method Area / Metaspace** (Java 8+):
- تخزن معلومات الـ class والمتغيرات الـ static ومعلومات الدوال
- كانت تسمى PermGen سابقاً (Java 7 وما قبلها)
- جزء من Heap في Java 8+ لكنها منفصلة منطقياً

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **مفهوم أساسي**: فهم الذاكرة ضروري لمطوري Java
- **تأثير على الأداء**: Stack مقابل Heap يؤثر على أداء التطبيق
- **الأخطاء الشائعة**: StackOverflowError و OutOfMemoryError
- **فهم GC**: معرفة Heap ضرورية لفهم جمع المخلفات
- **أمان الـ Threads**: فهم Stack يساعد في التعامل مع التزامن
- **تصحيح الأخطاء**: مشاكل الذاكرة شائعة في بيئة الإنتاج

---

## 3️⃣ القواعد والحقائق الأساسية

**مقارنة Stack مقابل Heap**:

| الميزة | Stack | Heap |
|---------|-------|------|
| **تخزن** | استدعاءات الدوال، المتغيرات المحلية، المراجع | الكائنات، متغيرات الـ instance، المصفوفات |
| **سرعة الوصول** | أسرع | أبطأ |
| **الحجم** | أصغر (عادة 1-2 MB) | أكبر (قابل للتخصيص، يمكن أن يكون GB) |
| **Thread** | كل Thread له Stack خاص | مشتركة بين جميع الـ Threads |
| **مدة البقاء** | أثناء تنفيذ الدالة | حتى يتم جمعها بواسطة GC |
| **الإدارة** | تلقائية (LIFO) | Garbage Collector |
| **الأخطاء** | StackOverflowError | OutOfMemoryError |
| **التخصيص** | متجاور | غير متجاور |
| **الترتيب** | LIFO (آخر ما يدخل أول ما يخرج) | لا ترتيب محدد |

**ماذا يذهب أين**:

**Stack**:
- المتغيرات المحلية البدائية (`int x = 5;`)
- متغيرات المراجع (`String str;` - المرجع على Stack)
- معلومات استدعاء الدوال (عنوان العودة، المعاملات)
- نتائج الحسابات الوسيطة

**Heap**:
- جميع الكائنات المنشأة بـ `new`
- متغيرات الـ instance (حقول الكائن)
- المصفوفات (حتى مصفوفات الأنواع البدائية)
- String Pool (منطقة خاصة في Heap)

**قواعد تخصيص الذاكرة**:
1. المتغيرات المحلية البدائية ← Stack
2. المتغيرات المحلية المرجعية ← المرجع على Stack، الكائن على Heap
3. متغيرات الـ instance ← Heap (جزء من الكائن)
4. المتغيرات الـ static ← Method Area/Metaspace
5. المصفوفات ← Heap (حتى مصفوفات الأنواع البدائية)
6. String literals ← String Pool (في Heap)

**Stack Frame**:
كل استدعاء دالة ينشئ stack frame يحتوي على:
- المتغيرات المحلية
- operand stack (للحسابات الوسيطة)
- بيانات الإطار (عنوان العودة، معلومات الاستثناءات)

**حقائق مهمة**:
- Java دائماً **pass-by-value** (حتى للكائنات، المرجع يُمرر بالقيمة)
- عند انتهاء الدالة، يُزال الـ stack frame الخاص بها
- الكائنات تبقى في Heap حتى يتم جمعها بواسطة GC
- مراجع متعددة يمكن أن تشير لنفس كائن Heap
- متغيرات Stack آمنة للـ threads (كل thread له stack خاص)
- كائنات Heap تحتاج مزامنة لأمان الـ threads

**أخطاء الذاكرة**:
- **StackOverflowError**: Stack ممتلئ (عادة تكرار لا نهائي)
- **OutOfMemoryError: Java heap space**: Heap ممتلئ (كائنات كثيرة جداً)
- **OutOfMemoryError: Metaspace**: Method area ممتلئة (classes كثيرة جداً)

**معاملات JVM**:
- `-Xss`: تحديد حجم Stack (مثل `-Xss512k`)
- `-Xms`: حجم Heap الابتدائي (مثل `-Xms512m`)
- `-Xmx`: حجم Heap الأقصى (مثل `-Xmx2g`)
- `-XX:MetaspaceSize`: حجم Metaspace

---

## 4️⃣ أمثلة برمجية (Java 8)

### مثال 1: تصور Stack مقابل Heap

```java
public class StackVsHeap {
    public static void main(String[] args) {
        // STACK: int 'age' with value 25
        int age = 25;

        // STACK: reference 'name' pointing to HEAP: String object "Alice"
        String name = "Alice";

        // STACK: reference 'person' pointing to HEAP: Person object
        Person person = new Person("Bob", 30);

        // Method call creates new stack frame
        processData(age, person);

        System.out.println("After method call:");
        System.out.println("Age: " + age);      // 25 (unchanged)
        System.out.println("Person: " + person.getName());  // Alice (changed!)
    }

    static void processData(int value, Person p) {
        // New stack frame created
        // STACK: 'value' = copy of 'age' = 25
        // STACK: 'p' = copy of reference to same Person object on HEAP

        value = 100;  // Changes local copy on stack
        p.setName("Alice");  // Changes object on heap

        // Stack frame removed when method returns
    }
}

class Person {
    // HEAP: instance variable (part of Person object)
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}

/*
MEMORY LAYOUT:

STACK (main thread):
┌──────────────────────┐
│ main() frame         │
│  - age: 25           │
│  - name: ref→"Alice" │
│  - person: ref→Obj   │
└──────────────────────┘

HEAP:
┌──────────────────────┐
│ String "Alice"       │
│ Person object:       │
│   - name: "Alice"    │
│   - age: 30          │
└──────────────────────┘
*/
```

### مثال 2: تخزين الأنواع البدائية مقابل الكائنات

```java
public class PrimitiveVsObject {
    public static void main(String[] args) {
        // Primitives - stored directly on STACK
        int num1 = 10;
        int num2 = 10;

        System.out.println(num1 == num2);  // true (comparing values)

        // Objects - reference on STACK, object on HEAP
        Integer obj1 = new Integer(10);
        Integer obj2 = new Integer(10);

        System.out.println(obj1 == obj2);  // false (different objects)
        System.out.println(obj1.equals(obj2));  // true (same value)

        // Visualization
        printMemoryLayout();
    }

    static void printMemoryLayout() {
        // STACK variables
        int x = 5;           // Stack: x = 5
        int y = 10;          // Stack: y = 10

        // HEAP objects
        String s1 = new String("Hello");  // Stack: s1 = ref1
                                          // Heap: String object "Hello"

        String s2 = new String("Hello");  // Stack: s2 = ref2
                                          // Heap: String object "Hello" (different)

        String s3 = s1;      // Stack: s3 = ref1 (same reference as s1)
                            // Points to same object on heap

        System.out.println("s1 == s2: " + (s1 == s2));  // false
        System.out.println("s1 == s3: " + (s1 == s3));  // true
    }
}
```

### مثال 3: المصفوفات والـ Heap

```java
public class ArraysInMemory {
    public static void main(String[] args) {
        // Primitive array
        // STACK: reference 'numbers'
        // HEAP: int array with 5 elements
        int[] numbers = {1, 2, 3, 4, 5};

        // Object array
        // STACK: reference 'strings'
        // HEAP: String array (holds references)
        // HEAP: Individual String objects
        String[] strings = {"A", "B", "C"};

        // Multi-dimensional array
        // STACK: reference 'matrix'
        // HEAP: 2D array (array of arrays)
        int[][] matrix = new int[3][3];

        demonstrateArrayMemory();
    }

    static void demonstrateArrayMemory() {
        // Even primitive arrays go to heap!
        int[] arr1 = {1, 2, 3};
        int[] arr2 = {1, 2, 3};

        System.out.println(arr1 == arr2);  // false (different objects)

        int[] arr3 = arr1;  // Same reference
        System.out.println(arr1 == arr3);  // true (same object)

        arr3[0] = 100;  // Modifies heap object
        System.out.println(arr1[0]);  // 100 (same object!)
    }
}

/*
MEMORY LAYOUT for int[] numbers = {1, 2, 3, 4, 5}:

STACK:
┌──────────────────┐
│ numbers: ref→arr │
└──────────────────┘

HEAP:
┌──────────────────┐
│ int[] array:     │
│  [0]: 1          │
│  [1]: 2          │
│  [2]: 3          │
│  [3]: 4          │
│  [4]: 5          │
└──────────────────┘
*/
```

### مثال 4: استدعاءات الدوال وإطارات الـ Stack

```java
public class StackFrameDemo {
    public static void main(String[] args) {
        System.out.println("Starting main");
        method1();
        System.out.println("Back to main");
    }

    static void method1() {
        System.out.println("In method1");
        int x = 10;
        method2(x);
        System.out.println("Back to method1");
    }

    static void method2(int value) {
        System.out.println("In method2");
        int y = value * 2;
        System.out.println("y = " + y);
        // method2 stack frame removed here
    }
}

/*
STACK EVOLUTION:

Step 1 - main() starts:
┌──────────────────┐
│ main() frame     │
└──────────────────┘

Step 2 - method1() called:
┌──────────────────┐
│ method1() frame  │
│  - x: 10         │
├──────────────────┤
│ main() frame     │
└──────────────────┘

Step 3 - method2() called:
┌──────────────────┐
│ method2() frame  │
│  - value: 10     │
│  - y: 20         │
├──────────────────┤
│ method1() frame  │
│  - x: 10         │
├──────────────────┤
│ main() frame     │
└──────────────────┘

Step 4 - method2() returns (frame removed):
┌──────────────────┐
│ method1() frame  │
│  - x: 10         │
├──────────────────┤
│ main() frame     │
└──────────────────┘

Step 5 - method1() returns:
┌──────────────────┐
│ main() frame     │
└──────────────────┘
*/
```

### مثال 5: خطأ StackOverflowError

```java
public class StackOverflowDemo {
    public static void main(String[] args) {
        try {
            recursiveMethod(1);
        } catch (StackOverflowError e) {
            System.out.println("StackOverflowError caught!");
            System.out.println("Stack depth matters!");
        }
    }

    // Infinite recursion - causes StackOverflowError
    static void recursiveMethod(int depth) {
        System.out.println("Depth: " + depth);
        recursiveMethod(depth + 1);  // Each call adds stack frame
    }

    // Proper recursion with base case
    static int factorial(int n) {
        if (n <= 1) {
            return 1;  // Base case - stops recursion
        }
        return n * factorial(n - 1);
    }
}

/*
STACK OVERFLOW:

Each recursiveMethod() call creates new stack frame:

┌──────────────────────┐
│ recursiveMethod(N)   │  ← Stack limit reached!
├──────────────────────┤
│ recursiveMethod(...)  │
├──────────────────────┤
│ ...                  │
├──────────────────────┤
│ recursiveMethod(3)   │
├──────────────────────┤
│ recursiveMethod(2)   │
├──────────────────────┤
│ recursiveMethod(1)   │
├──────────────────────┤
│ main()               │
└──────────────────────┘

Stack has limited size → StackOverflowError
*/
```

### مثال 6: خطأ OutOfMemoryError

```java
import java.util.*;

public class OutOfMemoryDemo {
    public static void main(String[] args) {
        // This will eventually cause OutOfMemoryError
        try {
            List<byte[]> list = new ArrayList<>();
            while (true) {
                // Each iteration creates 1MB byte array on heap
                byte[] array = new byte[1024 * 1024];  // 1 MB
                list.add(array);
                System.out.println("Arrays created: " + list.size());
            }
        } catch (OutOfMemoryError e) {
            System.out.println("OutOfMemoryError: Java heap space");
            System.out.println("Too many objects on heap!");
        }
    }
}

/*
HEAP OVERFLOW:

HEAP (before error):
┌──────────────────────┐
│ ArrayList object     │
│ byte[] 1 (1MB)       │
│ byte[] 2 (1MB)       │
│ byte[] 3 (1MB)       │
│ ...                  │
│ byte[] N (1MB)       │  ← Heap limit reached!
└──────────────────────┘

Heap has finite size → OutOfMemoryError
*/
```

### مثال 7: تمرير القيمة والذاكرة

```java
public class PassByValueDemo {
    public static void main(String[] args) {
        // Primitive - pass by value
        int num = 10;
        modifyPrimitive(num);
        System.out.println("After modifyPrimitive: " + num);  // 10 (unchanged)

        // Object - reference passed by value
        Person person = new Person("Alice");
        modifyObject(person);
        System.out.println("After modifyObject: " + person.getName());  // Bob (changed!)

        // Reference reassignment
        reassignReference(person);
        System.out.println("After reassignReference: " + person.getName());  // Bob (unchanged!)
    }

    static void modifyPrimitive(int value) {
        // STACK: new variable 'value' (copy of original)
        value = 100;  // Modifies local copy
    }

    static void modifyObject(Person p) {
        // STACK: new reference 'p' (copy of original reference)
        // Both references point to SAME object on HEAP
        p.setName("Bob");  // Modifies heap object
    }

    static void reassignReference(Person p) {
        // STACK: 'p' initially points to original Person
        p = new Person("Charlie");  // 'p' now points to NEW object
        // Original object unchanged!
        // New object eligible for GC when method returns
    }
}

class Person {
    private String name;

    public Person(String name) { this.name = name; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}

/*
MEMORY VISUALIZATION - modifyObject():

BEFORE method call:
STACK (main):                    HEAP:
┌──────────────────┐            ┌──────────────┐
│ person: ref1 ────┼───────────→│ Person       │
└──────────────────┘            │  name:"Alice"│
                                └──────────────┘

DURING method call:
STACK (main):                    HEAP:
┌──────────────────┐            ┌──────────────┐
│ person: ref1 ────┼───────────→│ Person       │
└──────────────────┘            │  name:"Bob"  │
STACK (modifyObject):           └──────────────┘
┌──────────────────┐                    ↑
│ p: ref1 (copy) ──┼────────────────────┘
└──────────────────┘

Both references point to SAME object!
*/
```

### مثال 8: String Pool في Heap

```java
public class StringPoolDemo {
    public static void main(String[] args) {
        // String literals - go to String Pool (in heap)
        String s1 = "Hello";  // Stack: s1 → String Pool: "Hello"
        String s2 = "Hello";  // Stack: s2 → Same "Hello" in pool

        System.out.println(s1 == s2);  // true (same reference)

        // String with new - goes to heap (not pool)
        String s3 = new String("Hello");  // Stack: s3 → Heap: new String
        System.out.println(s1 == s3);  // false (different locations)

        // intern() - add to pool
        String s4 = s3.intern();  // Returns reference from pool
        System.out.println(s1 == s4);  // true (both from pool)

        visualizeStringMemory();
    }

    static void visualizeStringMemory() {
        String literal1 = "Java";
        String literal2 = "Java";
        String object1 = new String("Java");
        String object2 = new String("Java");

        System.out.println("literal1 == literal2: " + (literal1 == literal2));  // true
        System.out.println("object1 == object2: " + (object1 == object2));      // false
        System.out.println("literal1 == object1: " + (literal1 == object1));    // false
    }
}

/*
STRING POOL MEMORY LAYOUT:

STACK:
┌──────────────────────┐
│ s1: ref1 ────┐       │
│ s2: ref1 ────┤       │
│ s3: ref2     │       │
│ s4: ref1 ────┤       │
└──────────────│───────┘
               │
               ↓
HEAP - String Pool:
┌──────────────────────┐
│ "Hello" ←────────────┘
└──────────────────────┘

HEAP - Regular:
┌──────────────────────┐
│ String("Hello") ←────── s3
└──────────────────────┘
*/
```

### مثال 9: متغيرات الـ Instance مقابل Static مقابل المحلية

```java
public class VariableMemoryLocation {
    // STATIC variables - Method Area/Metaspace
    static int staticCounter = 0;
    static String staticName = "Global";

    // INSTANCE variables - Heap (part of object)
    int instanceId;
    String instanceName;

    public VariableMemoryLocation(int id, String name) {
        // LOCAL variables - Stack (during constructor execution)
        int localTemp = id * 2;

        // Instance variables assigned
        this.instanceId = id;
        this.instanceName = name;

        staticCounter++;
    }

    public void processData() {
        // LOCAL variables - Stack (during method execution)
        int localVar1 = 10;
        String localVar2 = "Local";

        // Can access instance and static variables
        System.out.println("Instance: " + instanceId);
        System.out.println("Static: " + staticCounter);
        System.out.println("Local: " + localVar1);
    }

    public static void main(String[] args) {
        // Creating objects
        VariableMemoryLocation obj1 = new VariableMemoryLocation(1, "First");
        VariableMemoryLocation obj2 = new VariableMemoryLocation(2, "Second");

        obj1.processData();
        obj2.processData();

        System.out.println("Static counter: " + staticCounter);  // 2
    }
}

/*
MEMORY LAYOUT:

METHOD AREA / METASPACE:
┌────────────────────────────┐
│ VariableMemoryLocation     │
│  - staticCounter: 2        │
│  - staticName: "Global"    │
└────────────────────────────┘

STACK (main thread):
┌────────────────────────────┐
│ main() frame               │
│  - obj1: ref1              │
│  - obj2: ref2              │
└────────────────────────────┘

HEAP:
┌────────────────────────────┐
│ Object 1:                  │
│  - instanceId: 1           │
│  - instanceName: "First"   │
├────────────────────────────┤
│ Object 2:                  │
│  - instanceId: 2           │
│  - instanceName: "Second"  │
└────────────────────────────┘
*/
```

### مثال 10: تسرب الذاكرة

```java
import java.util.*;

public class MemoryLeakExample {
    // Static collection holds references - prevents GC
    private static List<Object> leakyList = new ArrayList<>();

    public static void main(String[] args) {
        // This creates a memory leak
        for (int i = 0; i < 1000; i++) {
            Object obj = new Object();
            leakyList.add(obj);  // Reference held forever in static list
            // Objects cannot be garbage collected!
        }

        // Proper way - allow GC
        demonstrateProperCleanup();
    }

    static void demonstrateProperCleanup() {
        List<Object> localList = new ArrayList<>();

        for (int i = 0; i < 1000; i++) {
            localList.add(new Object());
        }

        // When method returns, localList reference is gone
        // Objects eligible for garbage collection
    }

    static void properResourceManagement() {
        // Local scope - objects eligible for GC after method
        {
            List<Object> tempList = new ArrayList<>();
            for (int i = 0; i < 100; i++) {
                tempList.add(new Object());
            }
            // tempList goes out of scope here
        }
        // Objects from tempList eligible for GC
    }
}

/*
MEMORY LEAK:

STATIC leakyList holds references forever:
┌────────────────────────────┐
│ METHOD AREA:               │
│  leakyList: ref ───┐       │
└────────────────────│───────┘
                     │
                     ↓
HEAP:
┌────────────────────────────┐
│ ArrayList object           │
│  → Object 1                │
│  → Object 2                │
│  → Object 3                │
│  → ... (never GC'd!)       │
└────────────────────────────┘

Objects cannot be collected even if no longer needed!
*/
```

---

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما الفرق بين ذاكرة Stack و Heap؟**
2. **أين تُخزن المتغيرات المحلية؟**
3. **أين تُخزن الكائنات في الذاكرة؟**
4. **ما هو StackOverflowError ومتى يحدث؟**
5. **ما هو OutOfMemoryError وما أسبابه؟**
6. **هل Java تمرر بالقيمة أم بالمرجع؟**
7. **أين تُخزن المتغيرات الـ static؟**
8. **ماذا يحدث للذاكرة عند استدعاء دالة؟**
9. **لماذا تُخزن المصفوفات في Heap حتى لو كانت تحتوي على أنواع بدائية؟**
10. **أين يقع String Pool؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما الفرق بين ذاكرة Stack و Heap؟**

"ذاكرة Stack تخزن استدعاءات الدوال والمتغيرات المحلية في بنية LIFO، حيث كل Thread له Stack خاص به. هي أسرع لكن أصغر وتُدار تلقائياً - عند انتهاء الدالة، يُزال الـ stack frame الخاص بها. ذاكرة Heap تخزن جميع الكائنات ومتغيرات الـ instance، مشتركة بين جميع الـ Threads ويديرها Garbage Collector. هي أكبر لكن أبطأ من Stack. Stack تخزن المتغيرات المحلية البدائية والمراجع لكائنات Heap. Heap تخزن الكائنات الفعلية. Stack overflow يحدث من التكرار العميق جداً، بينما heap overflow يحدث من كثرة الكائنات. Stack آمنة للـ threads بطبيعتها، Heap تحتاج مزامنة."

**س2: أين تُخزن المتغيرات المحلية؟**

"المتغيرات المحلية تُخزن على Stack. للأنواع البدائية، القيمة الفعلية تُخزن على Stack. للأنواع المرجعية، المرجع نفسه على Stack لكن الكائن الذي يشير إليه على Heap. عند استدعاء دالة، يُنشأ stack frame يحتوي على المتغيرات المحلية. عند انتهاء الدالة، يُزال الـ stack frame وتُدمر جميع المتغيرات المحلية. هذه إدارة ذاكرة تلقائية - لا تحتاج تحرير ذاكرة Stack يدوياً. كل Thread له Stack خاص، لذا المتغيرات المحلية آمنة للـ threads بطبيعتها."

**س3: أين تُخزن الكائنات في الذاكرة؟**

"جميع الكائنات تُخزن في ذاكرة Heap، بغض النظر عن مكان تعريفها. حتى لو عرفت كائناً داخل دالة كمتغير محلي، المرجع فقط على Stack - الكائن نفسه على Heap. هذا يشمل جميع الكائنات المنشأة بكلمة new، والمصفوفات، وكائنات String. متغيرات الـ instance جزء من الكائن، لذا هي أيضاً على Heap. الـ Heap مشتركة بين جميع الـ Threads ويديرها Garbage Collector. الكائنات تبقى على Heap حتى لا يكون لها مراجع ويتم جمعها."

**س4: ما هو StackOverflowError ومتى يحدث؟**

"StackOverflowError يحدث عند نفاد ذاكرة Stack، عادة من تكرار لا نهائي أو عميق جداً. كل استدعاء دالة ينشئ stack frame، وإذا كان لديك تكرار غير محدود، في النهاية سينفد مكان Stack. مثلاً، دالة تكرارية بدون حالة أساسية صحيحة. يمكن أن يحدث أيضاً من سلاسل استدعاءات دوال عميقة جداً، لكن هذا أقل شيوعاً. حجم Stack محدود ويمكن تخصيصه بمعامل JVM هو -Xss. لإصلاحه، إما أضف حالات أساسية صحيحة للتكرار، أو حوّل إلى تكرار بالحلقات، أو زد حجم Stack إذا كان عمق التكرار مطلوباً فعلاً."

**س5: ما هو OutOfMemoryError وما أسبابه؟**

"OutOfMemoryError يحدث عندما لا يستطيع JVM تخصيص ذاكرة، الأكثر شيوعاً 'Java heap space' عندما تمتلئ Heap. هذا يحدث عندما تنشئ كائنات كثيرة جداً لا يمكن جمعها، مثل إضافة كائنات لمجموعة لا تُنظف أبداً. أنواع أخرى تشمل 'Metaspace' عند تحميل classes كثيرة جداً، أو 'unable to create new native thread' عند وجود threads كثيرة جداً. الأسباب الشائعة هي تسرب الذاكرة، وتحميل بيانات كبيرة، أو حجم heap غير كافٍ. يمكنك زيادة Heap بـ -Xmx أو إصلاح تسربات الذاكرة بإزالة مراجع الكائنات غير الضرورية."

**س6: هل Java تمرر بالقيمة أم بالمرجع؟**

"Java دائماً pass-by-value، دائماً. للأنواع البدائية، القيمة نفسها تُنسخ. للكائنات، المرجع يُمرر بالقيمة - يعني نسخة من المرجع تُمرر، ليس نسخة من الكائن. لهذا يمكنك تعديل حالة كائن في دالة وترى التغييرات، لكن لا يمكنك إعادة تعيين المرجع وتؤثر على الأصلي. مثلاً، إذا فعلت 'p = new Person()' في دالة، يؤثر فقط على النسخة المحلية من المرجع. الالتباس يأتي لأن المراجع تُمرر، لكنها تُمرر بالقيمة، ليس بالمرجع."

**س7: أين تُخزن المتغيرات الـ static؟**

"المتغيرات الـ static تُخزن في Method Area، وتسمى أيضاً Metaspace في Java 8+. قبل Java 8، كانت تسمى PermGen. الـ Method Area تخزن بيانات مستوى الـ class بما فيها المتغيرات الـ static، ومعلومات الـ class، وconstant pool. المتغيرات الـ static مشتركة بين جميع instances من class وتوجد طوال عمر التطبيق. تُهيأ عند تحميل الـ class لأول مرة وتبقى حتى ينتهي JVM. على عكس متغيرات الـ instance التي هي جزء من كائنات على Heap، المتغيرات الـ static تنتمي للـ class نفسه."

**س8: ماذا يحدث للذاكرة عند استدعاء دالة؟**

"عند استدعاء دالة، يُنشأ stack frame جديد ويُدفع على stack الـ thread. هذا الإطار يحتوي على المتغيرات المحلية للدالة، والمعاملات، وعنوان العودة، والنتائج الجزئية. إذا أنشأت الدالة كائنات، تذهب إلى Heap مع تخزين المراجع على Stack. عند انتهاء الدالة، يُسحب الـ stack frame ويُزال، محرراً تلقائياً جميع المتغيرات المحلية. لكن الكائنات المنشأة على Heap تبقى حتى يتم جمعها. لهذا يمكن للدوال إرجاع كائنات - المرجع يُرجع بينما الكائن يبقى على Heap."

**س9: لماذا تُخزن المصفوفات في Heap حتى لو كانت تحتوي على أنواع بدائية؟**

"المصفوفات كائنات في Java، لذا تُخزن على Heap بغض النظر عن نوع عناصرها. حتى 'int[] arr = new int[10]' تنشئ كائن مصفوفة على Heap. هذا لأن المصفوفات لها حجم ديناميكي يُحدد وقت التشغيل، وهذا لا يناسب نموذج إطار Stack ثابت الحجم. مرجع المصفوفة على Stack، لكن المصفوفة نفسها على Heap. هذا يسمح للمصفوفات أن تعيش أكثر من الدالة التي أنشأتها وأن تكون بأي حجم. كلمة 'new' مؤشر واضح أن الذاكرة تُخصص على Heap."

**س10: أين يقع String Pool؟**

"String Pool يقع في ذاكرة Heap اعتباراً من Java 7. قبل Java 7، كان في PermGen. الـ String Pool منطقة خاصة حيث يخزن JVM string literals لتحسين الذاكرة. عندما تنشئ string literal مثل 'String s = "Hello"'، Java تتحقق إذا كان 'Hello' موجوداً في الـ pool. إذا نعم، تُرجع ذلك المرجع. إذا لا، تنشئ String جديد وتضيفه للـ pool. لهذا string literals بنفس القيمة يتشاركون نفس المرجع. Strings المنشأة بـ 'new' لا تذهب للـ pool إلا إذا استدعيت intern()."

---

## 7️⃣ الأخطاء الشائعة

1. **الاعتقاد أن الكائنات تُخزن على Stack**:
   ```java
   void method() {
       Person p = new Person();  // p is on stack, Person object is on heap!
   }
   ```

2. **الخلط بين pass-by-value و pass-by-reference**:
   ```java
   void modify(Person p) {
       p = new Person();  // Doesn't affect original reference!
   }
   ```

3. **افتراض أن مصفوفات الأنواع البدائية على Stack**:
   ```java
   int[] arr = new int[10];  // Array is on heap, not stack!
   ```

4. **عدم فهم نطاق المتغيرات الـ static**:
   ```java
   class MyClass {
       static int count;  // In Method Area, not stack or heap instances
   }
   ```

5. **التكرار اللانهائي بدون إدراك حدود Stack**:
   ```java
   void recursive() {
       recursive();  // StackOverflowError!
   }
   ```

6. **الاحتفاظ بمراجع غير ضرورية (تسرب ذاكرة)**:
   ```java
   static List<Object> list = new ArrayList<>();
   // Adding to static list prevents GC
   ```

7. **الاعتقاد أن Stack و Heap لكل تطبيق**:
   ```java
   // Wrong: Each thread has its own stack
   // Heap is shared by all threads
   ```

8. **عدم فهم موقع String Pool**:
   ```java
   String s1 = "Hello";  // String Pool in heap (Java 7+)
   ```

---

## 8️⃣ نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- Stack: استدعاءات الدوال، المتغيرات المحلية، المراجع (LIFO، خاصة بكل thread، تلقائية)
- Heap: الكائنات، متغيرات الـ instance، المصفوفات (مشتركة، يتم جمعها، أكبر)
- Java دائماً pass-by-value (حتى للكائنات، المرجع يُمرر بالقيمة)
- StackOverflowError من التكرار العميق، OutOfMemoryError من كثرة الكائنات
- String Pool في Heap (Java 7+)

**ما يجب تجنبه**:
- لا تدعي أن Java هي pass-by-reference (هي pass-by-value)
- لا تقل أن الكائنات يمكن أن تكون على Stack (دائماً على Heap)
- لا تخلط بين PermGen (Java 7-) و Metaspace (Java 8+)
- لا تنسَ أن المصفوفات دائماً على Heap

**تحت الضغط**:
- ارسم رسماً بسيطاً: Stack (استدعاءات الدوال) مقابل Heap (الكائنات)
- مثال: `Person p = new Person();` → p على stack، الكائن على heap
- قل: "Stack = متغيرات محلية، Heap = كائنات، Java = pass-by-value"
- اذكر أمان الـ threads: stack لكل thread، heap مشتركة

**علامات تحذيرية يجب تجنبها**:
- "الكائنات على Stack" (أبداً)
- "Java هي pass-by-reference" (هي pass-by-value)
- "مصفوفات الأنواع البدائية على Stack" (كل المصفوفات على Heap)
- "String Pool في PermGen" (انتقل إلى Heap في Java 7)

**نقاط إضافية**:
- اذكر معاملات JVM: -Xss (stack)، -Xms/-Xmx (heap)
- ناقش escape analysis وتخصيص Stack (تحسين JVM متقدم)
- أشر إلى method area/metaspace للمتغيرات الـ static ومعلومات الـ class
- اذكر أن تحسين String Pool يقلل الذاكرة لـ string literals
- ناقش تسربات الذاكرة من المجموعات الـ static أو المستمعين
- أشر إلى generational heap (Young/Old) لمناقشة GC
- اذكر أن stack frames تحتوي على المتغيرات المحلية، operand stack، بيانات الإطار

</div>
