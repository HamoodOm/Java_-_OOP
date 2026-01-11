# Arrays vs Collections

## 1️⃣ Concept Overview

**Arrays** and **Collections** are both used to store multiple elements, but they have significant differences in functionality, flexibility, and use cases.

**Arrays**:
- Fixed-size data structure
- Can store primitives and objects
- Part of core Java language (not a framework)
- Accessed using index with `[]` syntax
- Length is fixed at creation time
- Direct memory access (faster)

**Collections**:
- Dynamic-size data structures
- Can only store objects (use wrapper classes for primitives)
- Part of Java Collections Framework
- Accessed using methods like `get()`, `add()`, `remove()`
- Size can grow or shrink dynamically
- More functionality (sorting, searching, iteration)

**Key Principle**: Arrays are for fixed-size, performance-critical scenarios. Collections are for dynamic, feature-rich data manipulation.

**Common Collection Interfaces**:
- **List**: Ordered collection (ArrayList, LinkedList, Vector)
- **Set**: No duplicates (HashSet, TreeSet, LinkedHashSet)
- **Queue**: FIFO processing (PriorityQueue, LinkedList)
- **Map**: Key-value pairs (HashMap, TreeMap, LinkedHashMap)

---

## 2️⃣ Why Interviewers Ask This

- **Fundamental concept**: Essential for any Java developer
- **Design decisions**: Shows ability to choose right data structure
- **Performance awareness**: Understanding trade-offs between arrays and collections
- **Framework knowledge**: Tests familiarity with Collections Framework
- **Real-world application**: Used in every Java project
- **Memory management**: Understanding how both are stored

---

## 3️⃣ Key Rules / Facts

**Arrays vs Collections Comparison Table**:

| Feature | Array | Collection |
|---------|-------|------------|
| **Size** | Fixed at creation | Dynamic (grows/shrinks) |
| **Type Safety** | Can store primitives & objects | Objects only (autoboxing for primitives) |
| **Performance** | Faster (direct memory access) | Slightly slower (object overhead) |
| **Memory** | Continuous memory block | May be scattered |
| **Built-in Methods** | Only `length` property | Rich API (add, remove, contains, etc.) |
| **Multi-dimensional** | Yes (int[][]) | No (use List of Lists) |
| **Type** | Can be homogeneous or Object[] | Generic type safety |
| **Initialization** | Must specify size | No initial size needed |
| **Modification** | Cannot resize | Can add/remove elements |
| **Null Elements** | Allowed | Most allow null (varies) |
| **Iteration** | for-loop, enhanced for-loop | Iterator, for-each, streams |

**When to Use Arrays**:
1. Size is known and fixed
2. Need to store primitives directly (performance)
3. Need multi-dimensional data structure
4. Performance is critical (tight loops)
5. Simple data storage without complex operations
6. Interacting with APIs that require arrays

**When to Use Collections**:
1. Size is unknown or changes frequently
2. Need rich functionality (sorting, searching, filtering)
3. Need thread-safe implementations (synchronized collections)
4. Working with complex data structures
5. Need to remove/add elements frequently
6. Need type safety with generics

**Array Methods** (from Arrays utility class):
- `Arrays.sort()`, `Arrays.binarySearch()`
- `Arrays.fill()`, `Arrays.copyOf()`
- `Arrays.equals()`, `Arrays.deepEquals()`
- `Arrays.toString()`, `Arrays.deepToString()`
- `Arrays.asList()` (converts array to List)

**Common Collection Implementations**:
- **ArrayList**: Dynamic array, fast random access
- **LinkedList**: Doubly-linked list, fast insertion/deletion
- **HashSet**: Unordered, no duplicates, O(1) operations
- **TreeSet**: Sorted set, O(log n) operations
- **HashMap**: Key-value pairs, O(1) average access
- **TreeMap**: Sorted map, O(log n) operations

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Basic Array vs ArrayList

```java
import java.util.ArrayList;
import java.util.List;

public class ArrayVsArrayList {
    public static void main(String[] args) {
        // Array - fixed size
        int[] intArray = new int[5];
        intArray[0] = 10;
        intArray[1] = 20;
        intArray[2] = 30;
        System.out.println("Array length: " + intArray.length);  // 5
        
        // Cannot resize array
        // intArray[5] = 40;  // ArrayIndexOutOfBoundsException!
        
        // ArrayList - dynamic size
        List<Integer> intList = new ArrayList<>();
        intList.add(10);
        intList.add(20);
        intList.add(30);
        System.out.println("ArrayList size: " + intList.size());  // 3
        
        // Can add more elements
        intList.add(40);
        intList.add(50);
        System.out.println("ArrayList size after adding: " + intList.size());  // 5
        
        // Can remove elements
        intList.remove(0);  // Removes element at index 0
        System.out.println("ArrayList size after removal: " + intList.size());  // 4
        
        // Array access
        System.out.println("Array element: " + intArray[0]);  // 10
        
        // ArrayList access
        System.out.println("ArrayList element: " + intList.get(0));  // 20
    }
}
```

### Example 2: Primitives vs Objects

```java
import java.util.ArrayList;
import java.util.List;

public class PrimitivesVsObjects {
    public static void main(String[] args) {
        // Array can store primitives directly
        int[] primitiveArray = {1, 2, 3, 4, 5};
        System.out.println("Primitive array: " + primitiveArray[0]);  // 1
        
        // ArrayList cannot store primitives - uses wrapper classes
        // List<int> list = new ArrayList<>();  // Compilation error!
        
        List<Integer> objectList = new ArrayList<>();
        objectList.add(1);  // Autoboxing: int -> Integer
        objectList.add(2);
        
        int value = objectList.get(0);  // Unboxing: Integer -> int
        System.out.println("Value from list: " + value);  // 1
        
        // Memory implications
        // primitiveArray uses: 5 * 4 bytes = 20 bytes (int is 4 bytes)
        // objectList uses: 5 * (4 bytes + object overhead ~16 bytes) ≈ 100 bytes
        
        // Performance test
        long startTime, endTime;
        
        // Array with primitives
        startTime = System.nanoTime();
        int sum = 0;
        for (int i = 0; i < primitiveArray.length; i++) {
            sum += primitiveArray[i];
        }
        endTime = System.nanoTime();
        System.out.println("Array time: " + (endTime - startTime) + "ns");
        
        // ArrayList with objects
        startTime = System.nanoTime();
        int sum2 = 0;
        for (Integer num : objectList) {
            sum2 += num;  // Unboxing overhead
        }
        endTime = System.nanoTime();
        System.out.println("ArrayList time: " + (endTime - startTime) + "ns");
    }
}
```

### Example 3: Converting Between Arrays and Collections

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class ArrayCollectionConversion {
    public static void main(String[] args) {
        // Array to List using Arrays.asList()
        String[] array = {"Apple", "Banana", "Cherry"};
        List<String> list = Arrays.asList(array);
        System.out.println("List from array: " + list);
        
        // WARNING: Arrays.asList() returns FIXED-SIZE list
        // list.add("Date");  // UnsupportedOperationException!
        // list.remove(0);    // UnsupportedOperationException!
        
        // Modifying element works
        list.set(0, "Apricot");
        System.out.println("Modified list: " + list);
        System.out.println("Original array: " + Arrays.toString(array));  // Also modified!
        
        // Array to MUTABLE ArrayList
        List<String> mutableList = new ArrayList<>(Arrays.asList(array));
        mutableList.add("Date");  // Works!
        mutableList.remove(0);    // Works!
        System.out.println("Mutable list: " + mutableList);
        
        // List to Array - method 1 (Object array)
        Object[] objectArray = mutableList.toArray();
        System.out.println("Object array: " + Arrays.toString(objectArray));
        
        // List to Array - method 2 (typed array, PREFERRED)
        String[] stringArray = mutableList.toArray(new String[0]);
        System.out.println("String array: " + Arrays.toString(stringArray));
        
        // Primitive array to List (requires manual conversion)
        int[] intArray = {1, 2, 3, 4, 5};
        // List<Integer> intList = Arrays.asList(intArray);  // Wrong! Creates List<int[]>
        
        // Correct way
        List<Integer> intList = new ArrayList<>();
        for (int num : intArray) {
            intList.add(num);  // Autoboxing
        }
        System.out.println("Int list: " + intList);
        
        // Java 8+ Stream approach
        List<Integer> intList2 = new ArrayList<>();
        Arrays.stream(intArray).forEach(intList2::add);
        System.out.println("Int list (stream): " + intList2);
    }
}
```

### Example 4: Multi-dimensional Arrays vs Nested Collections

```java
import java.util.ArrayList;
import java.util.List;

public class MultiDimensional {
    public static void main(String[] args) {
        // Multi-dimensional array
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        
        System.out.println("Matrix[1][1]: " + matrix[1][1]);  // 5
        
        // Print entire matrix
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
        
        // Nested Collections (List of Lists)
        List<List<Integer>> listMatrix = new ArrayList<>();
        
        // Add rows
        List<Integer> row1 = new ArrayList<>();
        row1.add(1); row1.add(2); row1.add(3);
        listMatrix.add(row1);
        
        List<Integer> row2 = new ArrayList<>();
        row2.add(4); row2.add(5); row2.add(6);
        listMatrix.add(row2);
        
        System.out.println("List matrix: " + listMatrix);
        System.out.println("Element [1][1]: " + listMatrix.get(1).get(1));  // 5
        
        // Advantage of List of Lists: Can have different row sizes
        List<Integer> row3 = new ArrayList<>();
        row3.add(7); row3.add(8);  // Only 2 elements
        listMatrix.add(row3);
        
        System.out.println("Jagged list matrix: " + listMatrix);
        
        // Jagged array (different row sizes)
        int[][] jaggedArray = {
            {1, 2, 3, 4},
            {5, 6},
            {7, 8, 9}
        };
        
        System.out.println("Jagged array length: " + jaggedArray.length);
        System.out.println("Row 0 length: " + jaggedArray[0].length);
        System.out.println("Row 1 length: " + jaggedArray[1].length);
    }
}
```

### Example 5: Performance Comparison

```java
import java.util.ArrayList;
import java.util.List;

public class PerformanceComparison {
    public static void main(String[] args) {
        int size = 1000000;
        
        // Test 1: Creation and population
        long startTime, endTime;
        
        // Array
        startTime = System.currentTimeMillis();
        int[] array = new int[size];
        for (int i = 0; i < size; i++) {
            array[i] = i;
        }
        endTime = System.currentTimeMillis();
        System.out.println("Array creation & population: " + (endTime - startTime) + "ms");
        
        // ArrayList
        startTime = System.currentTimeMillis();
        List<Integer> list = new ArrayList<>(size);  // Pre-size for fairness
        for (int i = 0; i < size; i++) {
            list.add(i);
        }
        endTime = System.currentTimeMillis();
        System.out.println("ArrayList creation & population: " + (endTime - startTime) + "ms");
        
        // Test 2: Random access
        startTime = System.currentTimeMillis();
        int sum = 0;
        for (int i = 0; i < size; i++) {
            sum += array[i];
        }
        endTime = System.currentTimeMillis();
        System.out.println("Array random access: " + (endTime - startTime) + "ms");
        
        startTime = System.currentTimeMillis();
        sum = 0;
        for (int i = 0; i < size; i++) {
            sum += list.get(i);  // Unboxing overhead
        }
        endTime = System.currentTimeMillis();
        System.out.println("ArrayList random access: " + (endTime - startTime) + "ms");
        
        // Test 3: Iteration
        startTime = System.currentTimeMillis();
        sum = 0;
        for (int num : array) {
            sum += num;
        }
        endTime = System.currentTimeMillis();
        System.out.println("Array iteration: " + (endTime - startTime) + "ms");
        
        startTime = System.currentTimeMillis();
        sum = 0;
        for (int num : list) {
            sum += num;
        }
        endTime = System.currentTimeMillis();
        System.out.println("ArrayList iteration: " + (endTime - startTime) + "ms");
        
        // Typical results:
        // Arrays are 2-3x faster for primitives due to no boxing/unboxing
        // For objects, difference is minimal
    }
}
```

### Example 6: Arrays Utility Class

```java
import java.util.Arrays;

public class ArraysUtilityDemo {
    public static void main(String[] args) {
        int[] numbers = {5, 2, 8, 1, 9, 3, 7};
        
        // sort() - sorts in ascending order
        Arrays.sort(numbers);
        System.out.println("Sorted: " + Arrays.toString(numbers));
        // Output: [1, 2, 3, 5, 7, 8, 9]
        
        // binarySearch() - requires sorted array
        int index = Arrays.binarySearch(numbers, 5);
        System.out.println("Index of 5: " + index);  // 3
        
        // fill() - fills array with value
        int[] filled = new int[5];
        Arrays.fill(filled, 10);
        System.out.println("Filled: " + Arrays.toString(filled));
        // Output: [10, 10, 10, 10, 10]
        
        // copyOf() - copies array to new array
        int[] copy = Arrays.copyOf(numbers, 5);
        System.out.println("Copy: " + Arrays.toString(copy));
        
        // copyOfRange() - copies specific range
        int[] rangeCopy = Arrays.copyOfRange(numbers, 2, 5);
        System.out.println("Range copy: " + Arrays.toString(rangeCopy));
        
        // equals() - compares arrays
        int[] arr1 = {1, 2, 3};
        int[] arr2 = {1, 2, 3};
        int[] arr3 = {1, 2, 4};
        System.out.println("arr1 equals arr2: " + Arrays.equals(arr1, arr2));  // true
        System.out.println("arr1 equals arr3: " + Arrays.equals(arr1, arr3));  // false
        
        // deepEquals() - for multi-dimensional arrays
        int[][] matrix1 = {{1, 2}, {3, 4}};
        int[][] matrix2 = {{1, 2}, {3, 4}};
        System.out.println("Deep equals: " + Arrays.deepEquals(matrix1, matrix2));  // true
        
        // toString() - converts array to string
        System.out.println("Array: " + Arrays.toString(numbers));
        
        // deepToString() - for multi-dimensional arrays
        System.out.println("Matrix: " + Arrays.deepToString(matrix1));
        
        // asList() - converts array to List
        String[] fruits = {"Apple", "Banana", "Cherry"};
        System.out.println("List: " + Arrays.asList(fruits));
    }
}
```

### Example 7: Collection Operations Not Available for Arrays

```java
import java.util.*;

public class CollectionAdvantages {
    public static void main(String[] args) {
        // Array limitations
        String[] array = {"Apple", "Banana", "Cherry"};
        
        // Cannot easily:
        // - Add element (need to create new array)
        // - Remove element (need to create new array)
        // - Check if contains (need manual loop)
        // - Find size dynamically
        
        // Collection advantages
        List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Cherry"));
        
        // 1. Easy addition
        list.add("Date");
        System.out.println("After add: " + list);
        
        // 2. Easy removal
        list.remove("Banana");
        System.out.println("After remove: " + list);
        
        // 3. Contains check
        System.out.println("Contains Apple: " + list.contains("Apple"));  // true
        
        // 4. Index of element
        System.out.println("Index of Cherry: " + list.indexOf("Cherry"));  // 1
        
        // 5. Sorting with custom comparator
        list.sort(Comparator.reverseOrder());
        System.out.println("Sorted reverse: " + list);
        
        // 6. Sublist
        List<String> subList = list.subList(0, 2);
        System.out.println("Sublist: " + subList);
        
        // 7. Clear all elements
        List<String> temp = new ArrayList<>(list);
        temp.clear();
        System.out.println("After clear: " + temp);  // []
        
        // 8. AddAll - add another collection
        List<String> fruits = new ArrayList<>();
        fruits.add("Apple");
        fruits.addAll(Arrays.asList("Banana", "Cherry"));
        System.out.println("After addAll: " + fruits);
        
        // 9. RemoveAll
        fruits.removeAll(Arrays.asList("Banana"));
        System.out.println("After removeAll: " + fruits);
        
        // 10. RetainAll - keep only specified elements
        fruits.addAll(Arrays.asList("Banana", "Date", "Elderberry"));
        fruits.retainAll(Arrays.asList("Apple", "Date"));
        System.out.println("After retainAll: " + fruits);  // [Apple, Date]
    }
}
```

### Example 8: Real-World Use Cases

```java
import java.util.*;

public class RealWorldUseCases {
    public static void main(String[] args) {
        // Use Case 1: Fixed coordinates (Array)
        // When you know exact structure and size
        int[][] chessBoard = new int[8][8];
        int[] rgbColor = {255, 128, 0};  // Fixed: R, G, B
        
        // Use Case 2: Shopping cart (ArrayList)
        // Dynamic - items can be added/removed
        List<String> shoppingCart = new ArrayList<>();
        shoppingCart.add("Laptop");
        shoppingCart.add("Mouse");
        shoppingCart.add("Keyboard");
        shoppingCart.remove("Mouse");  // Changed mind
        System.out.println("Cart: " + shoppingCart);
        
        // Use Case 3: Unique user IDs (HashSet)
        // No duplicates needed
        Set<String> activeUsers = new HashSet<>();
        activeUsers.add("user123");
        activeUsers.add("user456");
        activeUsers.add("user123");  // Duplicate - ignored
        System.out.println("Active users: " + activeUsers.size());  // 2
        
        // Use Case 4: Leaderboard (TreeSet)
        // Sorted set for rankings
        Set<Integer> scores = new TreeSet<>(Comparator.reverseOrder());
        scores.add(100);
        scores.add(250);
        scores.add(180);
        System.out.println("Top scores: " + scores);  // [250, 180, 100]
        
        // Use Case 5: Configuration (HashMap)
        // Key-value pairs
        Map<String, String> config = new HashMap<>();
        config.put("host", "localhost");
        config.put("port", "8080");
        config.put("db", "mysql");
        System.out.println("Port: " + config.get("port"));
        
        // Use Case 6: LRU Cache (LinkedHashMap)
        // Maintains insertion order
        Map<String, String> cache = new LinkedHashMap<>(16, 0.75f, true);
        cache.put("key1", "value1");
        cache.put("key2", "value2");
        cache.get("key1");  // Access order updated
        
        // Use Case 7: Task queue (Queue)
        // FIFO processing
        Queue<String> taskQueue = new LinkedList<>();
        taskQueue.offer("Task1");
        taskQueue.offer("Task2");
        taskQueue.offer("Task3");
        System.out.println("Processing: " + taskQueue.poll());  // Task1
        
        // Use Case 8: Method parameters (Array for varargs)
        printNumbers(1, 2, 3, 4, 5);  // Internally uses array
    }
    
    // Varargs uses array internally
    public static void printNumbers(int... numbers) {
        // 'numbers' is actually an int[]
        System.out.println("Numbers: " + Arrays.toString(numbers));
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is the difference between Array and ArrayList?**
2. **Can we store primitives in ArrayList?**
3. **How do you convert an array to a List?**
4. **Why are arrays faster than collections?**
5. **Can the size of an array change after creation?**
6. **What is the difference between Arrays.asList() and new ArrayList()?**
7. **When would you choose an array over an ArrayList?**
8. **Can you have an array of generics?**
9. **What is the time complexity of accessing elements in arrays vs ArrayList?**
10. **How do you copy an array?**

---

## 6️⃣ Model Answers

**Q1: What is the difference between Array and ArrayList?**

"The main differences are: Arrays have fixed size that must be specified at creation, while ArrayList is dynamic and can grow or shrink. Arrays can store primitives directly, while ArrayList can only store objects, requiring autoboxing for primitives. Arrays use bracket notation for access like arr[0], while ArrayList uses methods like get(0). Arrays have limited functionality with just length property, while ArrayList has rich methods like add, remove, contains, sort. Arrays are slightly faster due to direct memory access, while ArrayList has small overhead. Use arrays when size is fixed and performance is critical; use ArrayList for dynamic, flexible collections."

**Q2: Can we store primitives in ArrayList?**

"No, ArrayList cannot directly store primitives. It can only store objects. However, Java provides wrapper classes like Integer, Double, Boolean for primitives, and autoboxing automatically converts primitives to their wrapper objects. For example, when you write list.add(5), Java automatically boxes the int 5 to Integer(5). Similarly, when you retrieve it with list.get(0), it's automatically unboxed back to int. This convenience comes with performance overhead, which is why arrays are preferred for large primitive datasets."

**Q3: How do you convert an array to a List?**

"There are two main ways. First, Arrays.asList() converts an array to a fixed-size List backed by the original array. Changes to the list affect the array and vice versa, and you cannot add or remove elements. Second, create a new ArrayList passing Arrays.asList() to the constructor: new ArrayList<>(Arrays.asList(array)). This creates a mutable, independent copy. For primitive arrays, you need manual conversion or use streams because Arrays.asList() on int[] creates List<int[]>, not List<Integer>."

**Q4: Why are arrays faster than collections?**

"Arrays are faster for several reasons. First, they use contiguous memory allocation, allowing direct index-based access which is O(1) with minimal overhead. Second, when storing primitives, arrays avoid the boxing/unboxing overhead that collections incur. Third, arrays have less object overhead—no wrapper objects or internal data structures like ArrayList's backing array and size tracking. Fourth, array access uses simple pointer arithmetic at the JVM level, while collections use method calls. The performance difference is most noticeable with primitives and in tight loops processing millions of elements."

**Q5: Can the size of an array change after creation?**

"No, array size is fixed at creation and cannot change. Once you create an array with 'new int[5]', it will always have exactly 5 elements. If you need more space, you must create a new larger array and copy elements from the old array to the new one. This is what ArrayList does internally—when it runs out of space, it creates a new array (typically 1.5x larger), copies elements over, and discards the old array. This is why ArrayList is preferred when size needs to change frequently."

**Q6: What is the difference between Arrays.asList() and new ArrayList()?**

"Arrays.asList() returns a fixed-size List backed by the original array. You cannot add or remove elements, only modify existing ones, and changes affect the original array. It's a view over the array, not an independent copy. new ArrayList() creates a separate, independent, mutable ArrayList that can grow and shrink. For example, if you do List<String> list = Arrays.asList(array), then list.add() throws UnsupportedOperationException. But List<String> list = new ArrayList<>(Arrays.asList(array)) creates a fully mutable copy."

**Q7: When would you choose an array over an ArrayList?**

"Choose arrays when: you know the exact size and it won't change; you're working with primitives and need optimal performance; you need multi-dimensional structures like matrices; you're interfacing with APIs that require arrays; or you need the absolute best performance in tight loops. For example, in game development for position coordinates, in image processing for pixel data, in mathematical computations with large datasets, or when implementing low-level data structures. Otherwise, ArrayList is preferred for its flexibility and rich API."

**Q8: Can you have an array of generics?**

"You cannot directly create an array of generic types due to type erasure. For example, 'new T[10]' or 'new List<String>[10]' won't compile. This is because generics are erased at runtime but arrays require runtime type information. The workaround is to create an array of Object and cast it, like (T[]) new Object[10], though this generates an unchecked warning. Or use ArrayList<List<String>> instead of List<String>[]. This is one area where collections are more flexible than arrays."

**Q9: What is the time complexity of accessing elements in arrays vs ArrayList?**

"Both have O(1) time complexity for random access by index. Arrays use direct memory offset calculation, while ArrayList internally uses an array and delegates to it, so access time is nearly identical. The difference is in implementation overhead—arrays use simple bracket notation that compiles to direct memory access, while ArrayList.get() is a method call that then accesses the internal array. The performance difference is negligible in most applications, though arrays might be microseconds faster in extremely tight loops."

**Q10: How do you copy an array?**

"There are several ways. First, Arrays.copyOf(original, length) creates a new array of specified length. Second, System.arraycopy(src, srcPos, dest, destPos, length) copies elements between arrays. Third, clone() method creates a shallow copy: newArray = oldArray.clone(). Fourth, manual copying with a loop. For multi-dimensional arrays, these methods only do shallow copying—inner arrays are referenced, not copied. For deep copying, you need to manually copy each nested array or use serialization techniques."

---

## 7️⃣ Common Mistakes

1. **Confusing Arrays.asList() as creating mutable list**:
   ```java
   String[] arr = {"A", "B", "C"};
   List<String> list = Arrays.asList(arr);
   list.add("D");  // UnsupportedOperationException!
   
   // Correct:
   List<String> list = new ArrayList<>(Arrays.asList(arr));
   list.add("D");  // Works!
   ```

2. **Trying to store primitives in ArrayList**:
   ```java
   // List<int> list = new ArrayList<>();  // Compilation error!
   
   // Correct:
   List<Integer> list = new ArrayList<>();
   ```

3. **Using == to compare arrays**:
   ```java
   int[] arr1 = {1, 2, 3};
   int[] arr2 = {1, 2, 3};
   System.out.println(arr1 == arr2);  // false (different references)
   
   // Correct:
   System.out.println(Arrays.equals(arr1, arr2));  // true
   ```

4. **ArrayIndexOutOfBoundsException with arrays**:
   ```java
   int[] arr = new int[5];
   arr[5] = 10;  // Error! Valid indices: 0-4
   ```

5. **Not initializing array elements**:
   ```java
   int[] arr = new int[3];
   System.out.println(arr[0]);  // 0 (default value, not error)
   
   Integer[] objArr = new Integer[3];
   System.out.println(objArr[0]);  // null (can cause NPE!)
   ```

6. **Modifying list during iteration**:
   ```java
   List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
   for (String s : list) {
       list.remove(s);  // ConcurrentModificationException!
   }
   
   // Correct: Use Iterator
   Iterator<String> it = list.iterator();
   while (it.hasNext()) {
       it.next();
       it.remove();  // Safe removal
   }
   ```

7. **Assuming ArrayList is thread-safe**:
   ```java
   List<Integer> list = new ArrayList<>();
   // Multiple threads adding to list - race condition!
   
   // Use synchronized list or CopyOnWriteArrayList
   List<Integer> syncList = Collections.synchronizedList(new ArrayList<>());
   ```

8. **Creating array with negative or zero size**:
   ```java
   int[] arr = new int[-1];  // NegativeArraySizeException!
   int[] arr2 = new int[0];  // Legal but usually not useful
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Arrays are fixed-size, Collections are dynamic
- Arrays can store primitives directly, Collections need objects
- Arrays are faster for primitives, Collections have rich functionality
- Arrays.asList() creates fixed-size list backed by array
- Use arrays for known size and performance; Collections for flexibility

**What to avoid**:
- Don't claim "Collections are always better" (depends on use case)
- Don't forget autoboxing overhead for Collections
- Don't say arrays can't be resized (technically you create new one)
- Don't forget that multi-dimensional arrays are easier than nested Lists

**Under pressure**:
- Draw simple comparison: int[] vs ArrayList<Integer>
- Show memory difference: primitives vs objects with overhead
- Explain Arrays.asList() trap (fixed-size list)
- Mention that Collections Framework has many implementations for different needs

**Red flags to avoid**:
- "Arrays are always faster" (minimal difference for objects)
- "ArrayList can store primitives" (needs wrapper classes)
- "You can resize arrays" (you can't, you create new ones)
- "Arrays.asList() gives you ArrayList" (it doesn't, it's a fixed-size list)

**Bonus points**:
- Mention ArrayList's internal capacity and growth factor (1.5x)
- Discuss that ArrayList implements RandomAccess marker interface
- Reference that primitive arrays are more memory-efficient
- Mention System.arraycopy() is native and very fast
- Discuss when to use LinkedList vs ArrayList
- Reference that arrays are covariant, generics are invariant
- Mention parallel arrays vs objects with properties (anti-pattern)
