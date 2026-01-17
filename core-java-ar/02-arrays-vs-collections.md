<div dir="rtl">

# المصفوفات مقابل المجموعات (Arrays vs Collections)

## 1. نظرة عامة على المفهوم

**المصفوفات (Arrays)** و**المجموعات (Collections)** كلاهما يُستخدم لتخزين عناصر متعددة، لكنهما يختلفان بشكل كبير في الوظائف والمرونة وحالات الاستخدام.

**المصفوفات**:
- هيكل بيانات ثابت الحجم
- يمكنها تخزين الأنواع الأولية والكائنات
- جزء من لغة Java الأساسية (ليست إطار عمل)
- الوصول باستخدام الفهرس مع صيغة `[]`
- الطول ثابت عند الإنشاء
- وصول مباشر للذاكرة (أسرع)

**المجموعات**:
- هياكل بيانات ديناميكية الحجم
- يمكنها تخزين الكائنات فقط (استخدم فئات الغلاف للأنواع الأولية)
- جزء من إطار مجموعات Java
- الوصول باستخدام طرق مثل `get()`, `add()`, `remove()`
- الحجم يمكن أن ينمو أو يتقلص ديناميكياً
- وظائف أكثر (الترتيب، البحث، التكرار)

**المبدأ الأساسي**: المصفوفات للسيناريوهات ثابتة الحجم والحرجة للأداء. المجموعات للتعامل الديناميكي والغني بالميزات مع البيانات.

**واجهات المجموعات الشائعة**:
- **List**: مجموعة مرتبة (ArrayList, LinkedList, Vector)
- **Set**: بدون تكرارات (HashSet, TreeSet, LinkedHashSet)
- **Queue**: معالجة FIFO (PriorityQueue, LinkedList)
- **Map**: أزواج مفتاح-قيمة (HashMap, TreeMap, LinkedHashMap)

---

## 2. لماذا يسأل المحاورون عن هذا

- **مفهوم أساسي**: ضروري لأي مطور Java
- **قرارات التصميم**: يُظهر القدرة على اختيار هيكل البيانات المناسب
- **الوعي بالأداء**: فهم المفاضلات بين المصفوفات والمجموعات
- **معرفة الإطار**: يختبر الإلمام بإطار المجموعات
- **التطبيق الواقعي**: يُستخدم في كل مشروع Java
- **إدارة الذاكرة**: فهم كيفية تخزين كليهما

---

## 3. القواعد والحقائق الأساسية

**جدول مقارنة المصفوفات والمجموعات**:

| الميزة | المصفوفة | المجموعة |
|--------|----------|----------|
| **الحجم** | ثابت عند الإنشاء | ديناميكي (ينمو/يتقلص) |
| **أمان النوع** | يمكنها تخزين الأنواع الأولية والكائنات | الكائنات فقط (autoboxing للأنواع الأولية) |
| **الأداء** | أسرع (وصول مباشر للذاكرة) | أبطأ قليلاً (حمل الكائنات) |
| **الذاكرة** | كتلة ذاكرة متصلة | قد تكون متفرقة |
| **الطرق المدمجة** | خاصية `length` فقط | واجهة غنية (add, remove, contains, إلخ) |
| **متعددة الأبعاد** | نعم (int[][]) | لا (استخدم List of Lists) |
| **النوع** | يمكن أن تكون متجانسة أو Object[] | أمان نوع عام |
| **التهيئة** | يجب تحديد الحجم | لا حاجة لحجم أولي |
| **التعديل** | لا يمكن تغيير الحجم | يمكن إضافة/إزالة العناصر |
| **العناصر الفارغة** | مسموحة | معظمها يسمح بـ null (يختلف) |
| **التكرار** | حلقة for، حلقة for المحسنة | Iterator، for-each، streams |

**متى تستخدم المصفوفات**:
1. الحجم معروف وثابت
2. تحتاج تخزين الأنواع الأولية مباشرة (أداء)
3. تحتاج هيكل بيانات متعدد الأبعاد
4. الأداء حرج (حلقات ضيقة)
5. تخزين بيانات بسيط بدون عمليات معقدة
6. التفاعل مع واجهات برمجة تتطلب مصفوفات

**متى تستخدم المجموعات**:
1. الحجم غير معروف أو يتغير بشكل متكرر
2. تحتاج وظائف غنية (الترتيب، البحث، التصفية)
3. تحتاج تطبيقات آمنة للخيوط (مجموعات متزامنة)
4. العمل مع هياكل بيانات معقدة
5. تحتاج إزالة/إضافة عناصر بشكل متكرر
6. تحتاج أمان النوع مع الأنواع العامة

**طرق المصفوفات** (من فئة Arrays المساعدة):
- `Arrays.sort()`, `Arrays.binarySearch()`
- `Arrays.fill()`, `Arrays.copyOf()`
- `Arrays.equals()`, `Arrays.deepEquals()`
- `Arrays.toString()`, `Arrays.deepToString()`
- `Arrays.asList()` (تحويل مصفوفة إلى List)

**تطبيقات المجموعات الشائعة**:
- **ArrayList**: مصفوفة ديناميكية، وصول عشوائي سريع
- **LinkedList**: قائمة مترابطة مزدوجة، إدراج/حذف سريع
- **HashSet**: غير مرتبة، بدون تكرارات، عمليات O(1)
- **TreeSet**: مجموعة مرتبة، عمليات O(log n)
- **HashMap**: أزواج مفتاح-قيمة، وصول متوسط O(1)
- **TreeMap**: خريطة مرتبة، عمليات O(log n)

---

## 4. أمثلة برمجية (Java 8)

### المثال 1: مقارنة أساسية بين Array و ArrayList

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

### المثال 2: الأنواع الأولية مقابل الكائنات

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

### المثال 3: التحويل بين المصفوفات والمجموعات

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

### المثال 4: المصفوفات متعددة الأبعاد مقابل المجموعات المتداخلة

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

### المثال 5: مقارنة الأداء

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

### المثال 6: فئة Arrays المساعدة

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

### المثال 7: عمليات المجموعات غير المتوفرة للمصفوفات

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

### المثال 8: حالات استخدام واقعية

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

## 5. أسئلة المقابلات الشائعة

1. **ما الفرق بين Array و ArrayList؟**
2. **هل يمكن تخزين الأنواع الأولية في ArrayList؟**
3. **كيف تحول مصفوفة إلى List؟**
4. **لماذا المصفوفات أسرع من المجموعات؟**
5. **هل يمكن تغيير حجم المصفوفة بعد إنشائها؟**
6. **ما الفرق بين Arrays.asList() و new ArrayList()؟**
7. **متى تختار مصفوفة بدلاً من ArrayList؟**
8. **هل يمكن إنشاء مصفوفة من الأنواع العامة؟**
9. **ما هو التعقيد الزمني للوصول للعناصر في المصفوفات مقابل ArrayList؟**
10. **كيف تنسخ مصفوفة؟**

---

## 6. إجابات نموذجية

**س1: ما الفرق بين Array و ArrayList؟**

"الفروقات الرئيسية هي: المصفوفات لها حجم ثابت يجب تحديده عند الإنشاء، بينما ArrayList ديناميكية ويمكنها النمو أو التقلص. المصفوفات يمكنها تخزين الأنواع الأولية مباشرة، بينما ArrayList تخزن الكائنات فقط، وتتطلب autoboxing للأنواع الأولية. المصفوفات تستخدم صيغة الأقواس للوصول مثل arr[0]، بينما ArrayList تستخدم طرقاً مثل get(0). المصفوفات لها وظائف محدودة مع خاصية length فقط، بينما ArrayList لها طرق غنية مثل add, remove, contains, sort. المصفوفات أسرع قليلاً بسبب الوصول المباشر للذاكرة، بينما ArrayList لها حمل صغير. استخدم المصفوفات عندما يكون الحجم ثابتاً والأداء حرجاً؛ استخدم ArrayList للمجموعات الديناميكية والمرنة."

**س2: هل يمكن تخزين الأنواع الأولية في ArrayList؟**

"لا، ArrayList لا يمكنها تخزين الأنواع الأولية مباشرة. يمكنها فقط تخزين الكائنات. ومع ذلك، Java توفر فئات غلاف مثل Integer, Double, Boolean للأنواع الأولية، و autoboxing يحول الأنواع الأولية تلقائياً لكائنات الغلاف. مثلاً، عندما تكتب list.add(5)، Java تحول int 5 تلقائياً إلى Integer(5). بالمثل، عندما تسترجعها بـ list.get(0)، يتم تحويلها تلقائياً إلى int. هذه السهولة تأتي مع حمل أداء، لهذا تُفضل المصفوفات لمجموعات البيانات الأولية الكبيرة."

**س3: كيف تحول مصفوفة إلى List؟**

"هناك طريقتان رئيسيتان. أولاً، Arrays.asList() تحول مصفوفة إلى List ثابتة الحجم مدعومة بالمصفوفة الأصلية. التغييرات على القائمة تؤثر على المصفوفة والعكس، ولا يمكنك إضافة أو إزالة عناصر. ثانياً، أنشئ ArrayList جديدة بتمرير Arrays.asList() للمُنشئ: new ArrayList<>(Arrays.asList(array)). هذا يُنشئ نسخة قابلة للتغيير ومستقلة. لمصفوفات الأنواع الأولية، تحتاج تحويلاً يدوياً أو استخدام streams لأن Arrays.asList() على int[] تُنشئ List<int[]>، وليس List<Integer>."

**س4: لماذا المصفوفات أسرع من المجموعات؟**

"المصفوفات أسرع لعدة أسباب. أولاً، تستخدم تخصيص ذاكرة متصل، مما يسمح بوصول مباشر مبني على الفهرس وهو O(1) مع حمل أدنى. ثانياً، عند تخزين الأنواع الأولية، المصفوفات تتجنب حمل boxing/unboxing الذي تتحمله المجموعات. ثالثاً، المصفوفات لها حمل كائنات أقل—بدون كائنات غلاف أو هياكل بيانات داخلية مثل المصفوفة الداعمة لـ ArrayList وتتبع الحجم. رابعاً، وصول المصفوفات يستخدم حساب مؤشرات بسيط على مستوى JVM، بينما المجموعات تستخدم استدعاءات طرق. فرق الأداء أوضح مع الأنواع الأولية وفي الحلقات الضيقة التي تعالج ملايين العناصر."

**س5: هل يمكن تغيير حجم المصفوفة بعد إنشائها؟**

"لا، حجم المصفوفة ثابت عند الإنشاء ولا يمكن تغييره. بمجرد إنشاء مصفوفة بـ 'new int[5]'، ستحتوي دائماً على 5 عناصر بالضبط. إذا احتجت مساحة أكبر، يجب إنشاء مصفوفة جديدة أكبر ونسخ العناصر من القديمة للجديدة. هذا ما تفعله ArrayList داخلياً—عندما تنفد المساحة، تُنشئ مصفوفة جديدة (عادة 1.5x أكبر)، تنسخ العناصر، وتتخلص من القديمة. لهذا تُفضل ArrayList عندما يحتاج الحجم للتغيير بشكل متكرر."

**س6: ما الفرق بين Arrays.asList() و new ArrayList()؟**

"Arrays.asList() تُرجع List ثابتة الحجم مدعومة بالمصفوفة الأصلية. لا يمكنك إضافة أو إزالة عناصر، فقط تعديل الموجودة، والتغييرات تؤثر على المصفوفة الأصلية. إنها عرض فوق المصفوفة، وليست نسخة مستقلة. new ArrayList() تُنشئ ArrayList منفصلة ومستقلة وقابلة للتغيير يمكنها النمو والتقلص. مثلاً، إذا فعلت List<String> list = Arrays.asList(array)، فإن list.add() ترمي UnsupportedOperationException. لكن List<String> list = new ArrayList<>(Arrays.asList(array)) تُنشئ نسخة قابلة للتغيير بالكامل."

**س7: متى تختار مصفوفة بدلاً من ArrayList؟**

"اختر المصفوفات عندما: تعرف الحجم بالضبط ولن يتغير؛ تعمل مع الأنواع الأولية وتحتاج أداءً أمثل؛ تحتاج هياكل متعددة الأبعاد مثل المصفوفات؛ تتفاعل مع واجهات برمجة تتطلب مصفوفات؛ أو تحتاج أفضل أداء ممكن في الحلقات الضيقة. مثلاً، في تطوير الألعاب لإحداثيات المواقع، في معالجة الصور لبيانات البكسل، في الحسابات الرياضية مع مجموعات بيانات كبيرة، أو عند تنفيذ هياكل بيانات منخفضة المستوى. وإلا، تُفضل ArrayList لمرونتها وواجهتها الغنية."

**س8: هل يمكن إنشاء مصفوفة من الأنواع العامة؟**

"لا يمكنك إنشاء مصفوفة من الأنواع العامة مباشرة بسبب محو الأنواع. مثلاً، 'new T[10]' أو 'new List<String>[10]' لن تُترجم. هذا لأن الأنواع العامة تُمحى وقت التشغيل لكن المصفوفات تتطلب معلومات النوع وقت التشغيل. الحل البديل هو إنشاء مصفوفة من Object وتحويلها، مثل (T[]) new Object[10]، رغم أن هذا يولد تحذيراً بعدم التحقق. أو استخدم ArrayList<List<String>> بدلاً من List<String>[]. هذا مجال تكون فيه المجموعات أكثر مرونة من المصفوفات."

**س9: ما هو التعقيد الزمني للوصول للعناصر في المصفوفات مقابل ArrayList؟**

"كلاهما له تعقيد زمني O(1) للوصول العشوائي بالفهرس. المصفوفات تستخدم حساب إزاحة الذاكرة المباشر، بينما ArrayList تستخدم مصفوفة داخلياً وتفوض إليها، لذا وقت الوصول شبه متطابق. الفرق في حمل التنفيذ—المصفوفات تستخدم صيغة الأقواس البسيطة التي تُترجم لوصول ذاكرة مباشر، بينما ArrayList.get() هي استدعاء طريقة ثم تصل للمصفوفة الداخلية. فرق الأداء ضئيل في معظم التطبيقات، رغم أن المصفوفات قد تكون أسرع بميكروثانية في الحلقات الضيقة جداً."

**س10: كيف تنسخ مصفوفة؟**

"هناك عدة طرق. أولاً، Arrays.copyOf(original, length) تُنشئ مصفوفة جديدة بالطول المحدد. ثانياً، System.arraycopy(src, srcPos, dest, destPos, length) تنسخ عناصر بين المصفوفات. ثالثاً، طريقة clone() تُنشئ نسخة سطحية: newArray = oldArray.clone(). رابعاً، النسخ اليدوي بحلقة. للمصفوفات متعددة الأبعاد، هذه الطرق تقوم بنسخ سطحي فقط—المصفوفات الداخلية تُشار إليها، لا تُنسخ. للنسخ العميق، تحتاج نسخ كل مصفوفة متداخلة يدوياً أو استخدام تقنيات التسلسل."

---

## 7. الأخطاء الشائعة

1. **الخلط بين Arrays.asList() كإنشاء قائمة قابلة للتغيير**:
   ```java
   String[] arr = {"A", "B", "C"};
   List<String> list = Arrays.asList(arr);
   list.add("D");  // UnsupportedOperationException!

   // Correct:
   List<String> list = new ArrayList<>(Arrays.asList(arr));
   list.add("D");  // Works!
   ```

2. **محاولة تخزين الأنواع الأولية في ArrayList**:
   ```java
   // List<int> list = new ArrayList<>();  // Compilation error!

   // Correct:
   List<Integer> list = new ArrayList<>();
   ```

3. **استخدام == لمقارنة المصفوفات**:
   ```java
   int[] arr1 = {1, 2, 3};
   int[] arr2 = {1, 2, 3};
   System.out.println(arr1 == arr2);  // false (different references)

   // Correct:
   System.out.println(Arrays.equals(arr1, arr2));  // true
   ```

4. **ArrayIndexOutOfBoundsException مع المصفوفات**:
   ```java
   int[] arr = new int[5];
   arr[5] = 10;  // Error! Valid indices: 0-4
   ```

5. **عدم تهيئة عناصر المصفوفة**:
   ```java
   int[] arr = new int[3];
   System.out.println(arr[0]);  // 0 (default value, not error)

   Integer[] objArr = new Integer[3];
   System.out.println(objArr[0]);  // null (can cause NPE!)
   ```

6. **تعديل القائمة أثناء التكرار**:
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

7. **افتراض أن ArrayList آمنة للخيوط**:
   ```java
   List<Integer> list = new ArrayList<>();
   // Multiple threads adding to list - race condition!

   // Use synchronized list or CopyOnWriteArrayList
   List<Integer> syncList = Collections.synchronizedList(new ArrayList<>());
   ```

8. **إنشاء مصفوفة بحجم سالب أو صفر**:
   ```java
   int[] arr = new int[-1];  // NegativeArraySizeException!
   int[] arr2 = new int[0];  // Legal but usually not useful
   ```

---

## 8. نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- المصفوفات ثابتة الحجم، المجموعات ديناميكية
- المصفوفات يمكنها تخزين الأنواع الأولية مباشرة، المجموعات تحتاج كائنات
- المصفوفات أسرع للأنواع الأولية، المجموعات لها وظائف غنية
- Arrays.asList() تُنشئ قائمة ثابتة الحجم مدعومة بالمصفوفة
- استخدم المصفوفات للحجم المعروف والأداء؛ المجموعات للمرونة

**ما يجب تجنبه**:
- لا تدّعِ "المجموعات أفضل دائماً" (يعتمد على حالة الاستخدام)
- لا تنسَ حمل autoboxing للمجموعات
- لا تقل أن المصفوفات لا يمكن تغيير حجمها (تقنياً تُنشئ جديدة)
- لا تنسَ أن المصفوفات متعددة الأبعاد أسهل من القوائم المتداخلة

**تحت الضغط**:
- ارسم مقارنة بسيطة: int[] مقابل ArrayList<Integer>
- أظهر فرق الذاكرة: الأنواع الأولية مقابل الكائنات مع الحمل
- اشرح فخ Arrays.asList() (قائمة ثابتة الحجم)
- اذكر أن إطار المجموعات له تطبيقات كثيرة لاحتياجات مختلفة

**علامات تحذيرية يجب تجنبها**:
- "المصفوفات أسرع دائماً" (فرق ضئيل للكائنات)
- "ArrayList يمكنها تخزين الأنواع الأولية" (تحتاج فئات الغلاف)
- "يمكنك تغيير حجم المصفوفات" (لا يمكنك، تُنشئ جديدة)
- "Arrays.asList() تعطيك ArrayList" (لا، إنها قائمة ثابتة الحجم)

**نقاط إضافية**:
- اذكر السعة الداخلية لـ ArrayList وعامل النمو (1.5x)
- ناقش أن ArrayList تنفذ واجهة RandomAccess المؤشرة
- أشر إلى أن مصفوفات الأنواع الأولية أكثر كفاءة للذاكرة
- اذكر أن System.arraycopy() أصلية وسريعة جداً
- ناقش متى تستخدم LinkedList مقابل ArrayList
- أشر إلى أن المصفوفات covariant، الأنواع العامة invariant
- اذكر المصفوفات المتوازية مقابل الكائنات ذات الخصائص (نمط مضاد)

</div>
