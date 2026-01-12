# Memory: Stack vs Heap

## 1️⃣ Concept Overview

**Memory Management** in Java involves understanding how the JVM allocates and manages memory for different types of data. The two main areas are **Stack** and **Heap**.

**Stack Memory**:
- Stores method calls and local variables
- LIFO (Last In First Out) structure
- Thread-specific (each thread has its own stack)
- Automatically managed (variables automatically deleted when method returns)
- Faster access than heap
- Limited size (can cause StackOverflowError)

**Heap Memory**:
- Stores objects and instance variables
- Shared among all threads
- Managed by Garbage Collector
- Larger than stack
- Slower access than stack
- Can cause OutOfMemoryError when full

**Memory Layout**:
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
- Stores class metadata, static variables, method information
- Formerly PermGen (Java 7 and earlier)
- Part of heap in Java 8+ but logically separate

---

## 2️⃣ Why Interviewers Ask This

- **Fundamental concept**: Understanding memory is crucial for Java developers
- **Performance implications**: Stack vs heap affects application performance
- **Common errors**: StackOverflowError and OutOfMemoryError
- **GC understanding**: Heap knowledge essential for garbage collection
- **Thread safety**: Understanding stack helps with concurrency
- **Debugging**: Memory issues are common in production

---

## 3️⃣ Key Rules / Facts

**Stack vs Heap Comparison**:

| Feature | Stack | Heap |
|---------|-------|------|
| **Stores** | Method calls, local variables, references | Objects, instance variables, arrays |
| **Access Speed** | Faster | Slower |
| **Size** | Smaller (typically 1-2 MB) | Larger (configurable, can be GBs) |
| **Thread** | Each thread has own stack | Shared by all threads |
| **Lifetime** | Exists during method execution | Exists until garbage collected |
| **Management** | Automatic (LIFO) | Garbage Collector |
| **Errors** | StackOverflowError | OutOfMemoryError |
| **Allocation** | Contiguous | Non-contiguous |
| **Order** | LIFO (Last In First Out) | No specific order |

**What Goes Where**:

**Stack**:
- Primitive local variables (`int x = 5;`)
- Reference variables (`String str;` - reference is on stack)
- Method call information (return address, parameters)
- Intermediate computation results

**Heap**:
- All objects created with `new`
- Instance variables (object fields)
- Arrays (even primitive arrays)
- String Pool (special area in heap)

**Memory Allocation Rules**:
1. Local primitive variables → Stack
2. Local reference variables → Reference on stack, object on heap
3. Instance variables → Heap (part of object)
4. Static variables → Method Area/Metaspace
5. Arrays → Heap (even primitive arrays)
6. String literals → String Pool (in heap)

**Stack Frame**:
Each method call creates a stack frame containing:
- Local variables
- Operand stack (for intermediate calculations)
- Frame data (return address, exception info)

**Important Facts**:
- Java is always **pass-by-value** (even for objects, the reference is passed by value)
- When method returns, its stack frame is removed
- Objects remain in heap until garbage collected
- Multiple references can point to same heap object
- Stack variables are thread-safe (each thread has own stack)
- Heap objects need synchronization for thread safety

**Memory Errors**:
- **StackOverflowError**: Stack full (usually infinite recursion)
- **OutOfMemoryError: Java heap space**: Heap full (too many objects)
- **OutOfMemoryError: Metaspace**: Method area full (too many classes)

**JVM Arguments**:
- `-Xss`: Set stack size (e.g., `-Xss512k`)
- `-Xms`: Initial heap size (e.g., `-Xms512m`)
- `-Xmx`: Maximum heap size (e.g., `-Xmx2g`)
- `-XX:MetaspaceSize`: Metaspace size

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Stack vs Heap Visualization

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

### Example 2: Primitive vs Object Storage

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

### Example 3: Arrays and Heap

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

### Example 4: Method Calls and Stack Frames

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

### Example 5: StackOverflowError Example

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

### Example 6: OutOfMemoryError Example

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

### Example 7: Pass-by-Value and Memory

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

### Example 8: String Pool in Heap

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

### Example 9: Instance vs Static vs Local Variables

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

### Example 10: Memory Leak Example

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

## 5️⃣ Common Interview Questions

1. **What is the difference between stack and heap memory?**
2. **Where are local variables stored?**
3. **Where are objects stored in memory?**
4. **What is StackOverflowError and when does it occur?**
5. **What is OutOfMemoryError and what causes it?**
6. **Is Java pass-by-value or pass-by-reference?**
7. **Where are static variables stored?**
8. **What happens to memory when a method is called?**
9. **Why are arrays stored in heap even if they contain primitives?**
10. **Where is the String Pool located?**

---

## 6️⃣ Model Answers

**Q1: What is the difference between stack and heap memory?**

"Stack memory stores method calls and local variables in a LIFO structure, with each thread having its own stack. It's faster but smaller and automatically managed—when a method returns, its stack frame is removed. Heap memory stores all objects and instance variables, shared by all threads and managed by the garbage collector. It's larger but slower than stack. Stack stores primitive local variables and references to heap objects. Heap stores the actual objects. Stack overflow occurs from too deep recursion, while heap overflow occurs from too many objects. Stack is thread-safe by nature, heap requires synchronization."

**Q2: Where are local variables stored?**

"Local variables are stored on the stack. For primitive types, the actual value is stored on the stack. For reference types, the reference itself is on the stack but the object it points to is on the heap. When a method is called, a stack frame is created containing its local variables. When the method returns, the stack frame is removed and all local variables are destroyed. This is automatic memory management—you don't need to manually free stack memory. Each thread has its own stack, so local variables are inherently thread-safe."

**Q3: Where are objects stored in memory?**

"All objects are stored in heap memory, regardless of where they're declared. Even if you declare an object inside a method as a local variable, only the reference is on the stack—the object itself is on the heap. This includes all instances created with new keyword, arrays, and String objects. Instance variables are part of the object, so they're also on the heap. The heap is shared among all threads and managed by the garbage collector. Objects remain on heap until they have no more references and are garbage collected."

**Q4: What is StackOverflowError and when does it occur?**

"StackOverflowError occurs when the stack memory is exhausted, typically from infinite or very deep recursion. Each method call creates a stack frame, and if you have unlimited recursion, eventually the stack runs out of space. For example, a recursive method without a proper base case. It can also occur from extremely deep method call chains, though this is less common. Stack size is limited and can be configured with -Xss JVM argument. To fix it, either add proper base cases to recursion, convert to iteration, or increase stack size if the recursion depth is legitimately needed."

**Q5: What is OutOfMemoryError and what causes it?**

"OutOfMemoryError occurs when the JVM cannot allocate memory, most commonly 'Java heap space' when the heap is full. This happens when you create too many objects that can't be garbage collected, like adding objects to a collection that's never cleared. Other variants include 'Metaspace' when you load too many classes, or 'unable to create new native thread' when you have too many threads. Common causes are memory leaks, loading large datasets, or insufficient heap size. You can increase heap with -Xmx or fix memory leaks by removing unnecessary object references."

**Q6: Is Java pass-by-value or pass-by-reference?**

"Java is strictly pass-by-value, always. For primitives, the value itself is copied. For objects, the reference is passed by value—meaning a copy of the reference is passed, not a copy of the object. This is why you can modify an object's state in a method and see the changes, but you cannot reassign the reference and have it affect the original. For example, if you do 'p = new Person()' in a method, it only affects the local copy of the reference. The confusion arises because references are passed, but they're passed by value, not by reference."

**Q7: Where are static variables stored?**

"Static variables are stored in the Method Area, also called Metaspace in Java 8+. Before Java 8, this was called PermGen. The Method Area stores class-level data including static variables, class metadata, and constant pool. Static variables are shared across all instances of a class and exist for the lifetime of the application. They're initialized when the class is first loaded and remain until the JVM terminates. Unlike instance variables which are part of objects on heap, static variables belong to the class itself."

**Q8: What happens to memory when a method is called?**

"When a method is called, a new stack frame is created and pushed onto the thread's stack. This frame contains the method's local variables, parameters, return address, and partial results. If the method creates objects, those go to the heap with references stored on the stack. When the method returns, its stack frame is popped and removed, automatically freeing all local variables. However, objects created on the heap remain until garbage collected. This is why methods can return objects—the reference is returned while the object stays on heap."

**Q9: Why are arrays stored in heap even if they contain primitives?**

"Arrays are objects in Java, so they're stored on the heap regardless of their element type. Even 'int[] arr = new int[10]' creates an array object on the heap. This is because arrays have dynamic size determined at runtime, which doesn't fit the stack's fixed-size frame model. The array reference is on the stack, but the array itself is on the heap. This allows arrays to outlive the method that created them and to be of arbitrary size. The 'new' keyword is a clear indicator that memory is allocated on the heap."

**Q10: Where is the String Pool located?**

"The String Pool is located in the heap memory as of Java 7. Before Java 7, it was in PermGen. The String Pool is a special area where the JVM stores string literals to optimize memory. When you create a string literal like 'String s = "Hello"', Java checks if 'Hello' exists in the pool. If yes, it returns that reference. If no, it creates a new String and adds it to the pool. This is why two string literals with the same value share the same reference. Strings created with 'new' don't go to the pool unless you call intern()."

---

## 7️⃣ Common Mistakes

1. **Thinking objects are stored on stack**:
   ```java
   void method() {
       Person p = new Person();  // p is on stack, Person object is on heap!
   }
   ```

2. **Confusing pass-by-value with pass-by-reference**:
   ```java
   void modify(Person p) {
       p = new Person();  // Doesn't affect original reference!
   }
   ```

3. **Assuming primitive arrays are on stack**:
   ```java
   int[] arr = new int[10];  // Array is on heap, not stack!
   ```

4. **Not understanding static variable scope**:
   ```java
   class MyClass {
       static int count;  // In Method Area, not stack or heap instances
   }
   ```

5. **Infinite recursion without realizing stack limits**:
   ```java
   void recursive() {
       recursive();  // StackOverflowError!
   }
   ```

6. **Holding unnecessary references (memory leak)**:
   ```java
   static List<Object> list = new ArrayList<>();
   // Adding to static list prevents GC
   ```

7. **Thinking stack and heap are per-application**:
   ```java
   // Wrong: Each thread has its own stack
   // Heap is shared by all threads
   ```

8. **Not understanding string pool location**:
   ```java
   String s1 = "Hello";  // String Pool in heap (Java 7+)
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Stack: method calls, local variables, references (LIFO, thread-specific, automatic)
- Heap: objects, instance variables, arrays (shared, garbage collected, larger)
- Java is pass-by-value (even for objects, reference passed by value)
- StackOverflowError from deep recursion, OutOfMemoryError from too many objects
- String Pool is in heap (Java 7+)

**What to avoid**:
- Don't claim Java is pass-by-reference (it's pass-by-value)
- Don't say objects can be on stack (they're always on heap)
- Don't confuse PermGen (Java 7-) with Metaspace (Java 8+)
- Don't forget that arrays are always on heap

**Under pressure**:
- Draw simple diagram: Stack (method calls) vs Heap (objects)
- Example: `Person p = new Person();` → p on stack, object on heap
- State: "Stack = local vars, Heap = objects, Java = pass-by-value"
- Mention thread-safety: stack is per-thread, heap is shared

**Red flags to avoid**:
- "Objects are on stack" (never)
- "Java is pass-by-reference" (it's pass-by-value)
- "Primitive arrays on stack" (all arrays on heap)
- "String Pool in PermGen" (moved to heap in Java 7)

**Bonus points**:
- Mention JVM arguments: -Xss (stack), -Xms/-Xmx (heap)
- Discuss escape analysis and stack allocation (advanced JVM optimization)
- Reference method area/metaspace for static variables and class metadata
- Mention that String Pool optimization reduces memory for string literals
- Discuss memory leaks from static collections or listeners
- Reference generational heap (Young/Old) for GC discussion
- Mention stack frames contain local variables, operand stack, frame data
