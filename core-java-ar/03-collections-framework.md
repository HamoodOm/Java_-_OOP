<div dir="rtl">

# إطار المجموعات (Collections Framework)

## 1️⃣ نظرة عامة على المفهوم

**إطار المجموعات في Java** هو بنية موحدة لتمثيل ومعالجة مجموعات الكائنات. يوفر واجهات وتطبيقات وخوارزميات للعمل مع مجموعات الكائنات بكفاءة.

**التسلسل الهرمي للواجهات الأساسية**:

```
Collection (interface)
├── List (ordered, allows duplicates)
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector (legacy, synchronized)
├── Set (no duplicates)
│   ├── HashSet
│   ├── LinkedHashSet (maintains insertion order)
│   └── TreeSet (sorted)
└── Queue (FIFO processing)
    ├── PriorityQueue (heap-based)
    └── LinkedList (also implements Queue)

Map (interface) - separate hierarchy (key-value pairs)
├── HashMap
├── LinkedHashMap (maintains insertion order)
├── TreeMap (sorted by keys)
└── Hashtable (legacy, synchronized)
```

**الخصائص الرئيسية**:
- **الواجهات (Interfaces)**: تعرّف أنواع البيانات المجردة (List، Set، Queue، Map)
- **التطبيقات (Implementations)**: فئات محددة (ArrayList، HashSet، HashMap)
- **الخوارزميات (Algorithms)**: طرق للفرز والبحث والخلط (فئة Collections)
- **الأنواع العامة (Generics)**: مجموعات آمنة من حيث النوع باستخدام Generics

**الفرق بين Collections و Collection**:
- **Collection**: واجهة تمثل مجموعة من الكائنات
- **Collections**: فئة أدوات مساعدة تحتوي على طرق ثابتة للعمليات

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **إطار أساسي**: يُستخدم في كل تطبيق Java
- **قرارات التصميم**: يختبر القدرة على اختيار نوع المجموعة المناسب
- **تأثيرات الأداء**: فهم تعقيدات الوقت
- **سيناريوهات واقعية**: العمل اليومي يتضمن المجموعات
- **معرفة واجهة برمجة التطبيقات**: يُظهر الإلمام بمكتبة Java القياسية
- **الوعي بالخوارزميات**: أنماط الفرز والبحث والتكرار

---

## 3️⃣ القواعد والحقائق الأساسية

**واجهة List** (مرتبة، تسمح بالتكرار):

| التطبيق | البنية الأساسية | الوصول | الإدراج/الحذف | متى تُستخدم |
|---------|-----------------|--------|---------------|-------------|
| ArrayList | مصفوفة ديناميكية | O(1) | O(n) | عند الحاجة للوصول العشوائي السريع |
| LinkedList | قائمة مرتبطة مزدوجة | O(n) | O(1) | عند كثرة عمليات الإدراج/الحذف |
| Vector | مصفوفة متزامنة | O(1) | O(n) | عند الحاجة لقائمة آمنة للخيوط (قديم) |

**واجهة Set** (لا تسمح بالتكرار):

| التطبيق | البنية الأساسية | الإضافة/البحث | الترتيب | متى تُستخدم |
|---------|-----------------|---------------|---------|-------------|
| HashSet | HashMap | O(1) متوسط | بدون ترتيب | بحث سريع، بدون تكرار |
| LinkedHashSet | HashMap + قائمة مرتبطة | O(1) متوسط | ترتيب الإدراج | عند أهمية الترتيب |
| TreeSet | شجرة أحمر-أسود | O(log n) | مرتبة | عند الحاجة لمجموعة مرتبة |

**واجهة Queue** (معالجة FIFO):

| التطبيق | البنية الأساسية | Offer/Poll | الترتيب | متى تُستخدم |
|---------|-----------------|------------|---------|-------------|
| LinkedList | قائمة مرتبطة مزدوجة | O(1) | FIFO | طابور قياسي |
| PriorityQueue | كومة ثنائية | O(log n) | ترتيب الأولوية | المعالجة حسب الأولوية |

**واجهة Map** (أزواج مفتاح-قيمة):

| التطبيق | البنية الأساسية | Get/Put | الترتيب | متى تُستخدم |
|---------|-----------------|---------|---------|-------------|
| HashMap | جدول هاش | O(1) متوسط | بدون ترتيب | بحث سريع |
| LinkedHashMap | HashMap + قائمة مرتبطة | O(1) متوسط | ترتيب الإدراج | عند أهمية الترتيب |
| TreeMap | شجرة أحمر-أسود | O(log n) | مفاتيح مرتبة | عند الحاجة لخريطة مرتبة |
| Hashtable | جدول هاش (متزامن) | O(1) متوسط | بدون ترتيب | خريطة آمنة للخيوط (قديم) |

**الطرق الشائعة**:

**واجهة Collection**:
- `add()`, `remove()`, `contains()`, `size()`, `isEmpty()`
- `clear()`, `addAll()`, `removeAll()`, `retainAll()`
- `iterator()`, `toArray()`, `stream()`

**واجهة List** (إضافية):
- `get(index)`, `set(index, element)`, `indexOf()`, `lastIndexOf()`
- `add(index, element)`, `remove(index)`, `subList(from, to)`

**واجهة Set**:
- نفس واجهة Collection (لا توجد طرق إضافية للـ Set الأساسي)

**واجهة Map**:
- `put(key, value)`, `get(key)`, `remove(key)`, `containsKey()`, `containsValue()`
- `keySet()`, `values()`, `entrySet()`, `putAll()`, `replace()`

**فئة Collections المساعدة**:
- `sort()`, `reverse()`, `shuffle()`, `binarySearch()`
- `min()`, `max()`, `frequency()`, `disjoint()`
- `synchronizedList()`, `synchronizedSet()`, `synchronizedMap()`
- `unmodifiableList()`, `unmodifiableSet()`, `unmodifiableMap()`

**مبادئ التصميم الرئيسية**:
- المكررات السريعة الفشل (ترمي ConcurrentModificationException)
- المجموعات ليست آمنة للخيوط افتراضياً (باستثناء Vector و Hashtable القديمين)
- استخدم Collections.synchronized*() أو المجموعات المتزامنة لأمان الخيوط
- التعامل مع null يختلف: HashMap يسمح بمفتاح null واحد، TreeMap لا يسمح بمفاتيح null

---

## 4️⃣ أمثلة على الكود (Java 8)

### المثال 1: ArrayList مقابل LinkedList

```java
import java.util.*;

public class ListComparison {
    public static void main(String[] args) {
        // ArrayList - backed by dynamic array
        List<String> arrayList = new ArrayList<>();
        arrayList.add("Apple");
        arrayList.add("Banana");
        arrayList.add("Cherry");

        // Fast random access
        System.out.println("Element at index 1: " + arrayList.get(1));  // O(1)

        // Slow insertion in middle
        arrayList.add(1, "Apricot");  // O(n) - shifts elements
        System.out.println("ArrayList: " + arrayList);

        // LinkedList - backed by doubly-linked list
        List<String> linkedList = new LinkedList<>();
        linkedList.add("Apple");
        linkedList.add("Banana");
        linkedList.add("Cherry");

        // Slow random access
        System.out.println("Element at index 1: " + linkedList.get(1));  // O(n)

        // Fast insertion in middle (if you have reference to node)
        linkedList.add(1, "Apricot");  // O(1) at ends, O(n) in middle
        System.out.println("LinkedList: " + linkedList);

        // LinkedList as Queue
        Queue<String> queue = new LinkedList<>();
        queue.offer("First");
        queue.offer("Second");
        queue.offer("Third");

        System.out.println("Poll: " + queue.poll());  // First (FIFO)
        System.out.println("Peek: " + queue.peek());  // Second (doesn't remove)

        // Performance comparison
        int size = 100000;

        // ArrayList - fast iteration
        List<Integer> al = new ArrayList<>();
        long start = System.currentTimeMillis();
        for (int i = 0; i < size; i++) {
            al.add(i);
        }
        long end = System.currentTimeMillis();
        System.out.println("ArrayList add at end: " + (end - start) + "ms");

        // LinkedList - fast at ends
        List<Integer> ll = new LinkedList<>();
        start = System.currentTimeMillis();
        for (int i = 0; i < size; i++) {
            ll.add(i);
        }
        end = System.currentTimeMillis();
        System.out.println("LinkedList add at end: " + (end - start) + "ms");

        // Random access comparison
        start = System.currentTimeMillis();
        for (int i = 0; i < 1000; i++) {
            al.get(size / 2);
        }
        end = System.currentTimeMillis();
        System.out.println("ArrayList get(middle): " + (end - start) + "ms");

        start = System.currentTimeMillis();
        for (int i = 0; i < 1000; i++) {
            ll.get(size / 2);
        }
        end = System.currentTimeMillis();
        System.out.println("LinkedList get(middle): " + (end - start) + "ms");
    }
}
```

### المثال 2: HashSet مقابل TreeSet مقابل LinkedHashSet

```java
import java.util.*;

public class SetComparison {
    public static void main(String[] args) {
        // HashSet - no order, fastest
        Set<String> hashSet = new HashSet<>();
        hashSet.add("Banana");
        hashSet.add("Apple");
        hashSet.add("Cherry");
        hashSet.add("Apple");  // Duplicate - ignored

        System.out.println("HashSet: " + hashSet);
        // Output order is unpredictable: [Banana, Apple, Cherry] or similar

        // LinkedHashSet - maintains insertion order
        Set<String> linkedHashSet = new LinkedHashSet<>();
        linkedHashSet.add("Banana");
        linkedHashSet.add("Apple");
        linkedHashSet.add("Cherry");
        linkedHashSet.add("Apple");  // Duplicate - ignored

        System.out.println("LinkedHashSet: " + linkedHashSet);
        // Output: [Banana, Apple, Cherry] - insertion order maintained

        // TreeSet - sorted order, slower
        Set<String> treeSet = new TreeSet<>();
        treeSet.add("Banana");
        treeSet.add("Apple");
        treeSet.add("Cherry");
        treeSet.add("Apple");  // Duplicate - ignored

        System.out.println("TreeSet: " + treeSet);
        // Output: [Apple, Banana, Cherry] - sorted alphabetically

        // TreeSet with custom comparator (reverse order)
        Set<String> reverseSet = new TreeSet<>(Comparator.reverseOrder());
        reverseSet.addAll(Arrays.asList("Banana", "Apple", "Cherry"));
        System.out.println("Reverse TreeSet: " + reverseSet);
        // Output: [Cherry, Banana, Apple]

        // Performance comparison
        int size = 100000;

        Set<Integer> hs = new HashSet<>();
        long start = System.currentTimeMillis();
        for (int i = 0; i < size; i++) {
            hs.add(i);
        }
        long end = System.currentTimeMillis();
        System.out.println("HashSet add: " + (end - start) + "ms");

        Set<Integer> ts = new TreeSet<>();
        start = System.currentTimeMillis();
        for (int i = 0; i < size; i++) {
            ts.add(i);
        }
        end = System.currentTimeMillis();
        System.out.println("TreeSet add: " + (end - start) + "ms");

        // Contains operation
        start = System.currentTimeMillis();
        for (int i = 0; i < 1000; i++) {
            hs.contains(size / 2);  // O(1)
        }
        end = System.currentTimeMillis();
        System.out.println("HashSet contains: " + (end - start) + "ms");

        start = System.currentTimeMillis();
        for (int i = 0; i < 1000; i++) {
            ts.contains(size / 2);  // O(log n)
        }
        end = System.currentTimeMillis();
        System.out.println("TreeSet contains: " + (end - start) + "ms");
    }
}
```

### المثال 3: HashMap مقابل TreeMap مقابل LinkedHashMap

```java
import java.util.*;

public class MapComparison {
    public static void main(String[] args) {
        // HashMap - no order, fastest
        Map<String, Integer> hashMap = new HashMap<>();
        hashMap.put("Banana", 2);
        hashMap.put("Apple", 5);
        hashMap.put("Cherry", 3);
        hashMap.put("Apple", 7);  // Overwrites previous value

        System.out.println("HashMap: " + hashMap);
        // Output order is unpredictable

        // LinkedHashMap - maintains insertion order
        Map<String, Integer> linkedHashMap = new LinkedHashMap<>();
        linkedHashMap.put("Banana", 2);
        linkedHashMap.put("Apple", 5);
        linkedHashMap.put("Cherry", 3);

        System.out.println("LinkedHashMap: " + linkedHashMap);
        // Output: {Banana=2, Apple=5, Cherry=3} - insertion order

        // TreeMap - sorted by keys
        Map<String, Integer> treeMap = new TreeMap<>();
        treeMap.put("Banana", 2);
        treeMap.put("Apple", 5);
        treeMap.put("Cherry", 3);

        System.out.println("TreeMap: " + treeMap);
        // Output: {Apple=5, Banana=2, Cherry=3} - sorted by keys

        // Common operations
        System.out.println("\n--- Map Operations ---");

        // get()
        System.out.println("Apple count: " + hashMap.get("Apple"));  // 7

        // containsKey() and containsValue()
        System.out.println("Contains 'Banana': " + hashMap.containsKey("Banana"));  // true
        System.out.println("Contains value 3: " + hashMap.containsValue(3));  // true

        // keySet(), values(), entrySet()
        System.out.println("Keys: " + hashMap.keySet());
        System.out.println("Values: " + hashMap.values());

        // Iterate using entrySet()
        System.out.println("\nIterating with entrySet():");
        for (Map.Entry<String, Integer> entry : hashMap.entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }

        // Java 8 forEach
        System.out.println("\nIterating with forEach:");
        hashMap.forEach((key, value) -> System.out.println(key + " = " + value));

        // getOrDefault()
        System.out.println("Date count: " + hashMap.getOrDefault("Date", 0));  // 0

        // putIfAbsent()
        hashMap.putIfAbsent("Date", 4);
        System.out.println("After putIfAbsent: " + hashMap.get("Date"));  // 4

        // compute(), computeIfPresent(), computeIfAbsent()
        hashMap.compute("Apple", (k, v) -> v + 10);  // 7 + 10 = 17
        System.out.println("After compute: " + hashMap.get("Apple"));  // 17

        // merge()
        hashMap.merge("Banana", 5, (oldVal, newVal) -> oldVal + newVal);
        System.out.println("After merge: " + hashMap.get("Banana"));  // 2 + 5 = 7
    }
}
```

### المثال 4: PriorityQueue

```java
import java.util.*;

public class PriorityQueueDemo {
    public static void main(String[] args) {
        // Default: Min heap (natural ordering)
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        minHeap.offer(5);
        minHeap.offer(2);
        minHeap.offer(8);
        minHeap.offer(1);
        minHeap.offer(9);

        System.out.println("Min Heap:");
        while (!minHeap.isEmpty()) {
            System.out.print(minHeap.poll() + " ");  // 1 2 5 8 9
        }
        System.out.println();

        // Max heap (reverse ordering)
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Comparator.reverseOrder());
        maxHeap.offer(5);
        maxHeap.offer(2);
        maxHeap.offer(8);
        maxHeap.offer(1);
        maxHeap.offer(9);

        System.out.println("Max Heap:");
        while (!maxHeap.isEmpty()) {
            System.out.print(maxHeap.poll() + " ");  // 9 8 5 2 1
        }
        System.out.println();

        // Custom objects with PriorityQueue
        class Task implements Comparable<Task> {
            String name;
            int priority;

            Task(String name, int priority) {
                this.name = name;
                this.priority = priority;
            }

            @Override
            public int compareTo(Task other) {
                return Integer.compare(this.priority, other.priority);
            }

            @Override
            public String toString() {
                return name + "(" + priority + ")";
            }
        }

        PriorityQueue<Task> taskQueue = new PriorityQueue<>();
        taskQueue.offer(new Task("Task A", 3));
        taskQueue.offer(new Task("Task B", 1));
        taskQueue.offer(new Task("Task C", 2));

        System.out.println("\nTask Queue (by priority):");
        while (!taskQueue.isEmpty()) {
            System.out.println(taskQueue.poll());
        }
        // Output: Task B(1), Task C(2), Task A(3)
    }
}
```

### المثال 5: فئة Collections المساعدة

```java
import java.util.*;

public class CollectionsUtilityDemo {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>(Arrays.asList(5, 2, 8, 1, 9, 3, 7));

        // sort() - sorts in natural order
        Collections.sort(numbers);
        System.out.println("Sorted: " + numbers);  // [1, 2, 3, 5, 7, 8, 9]

        // sort with custom comparator
        Collections.sort(numbers, Comparator.reverseOrder());
        System.out.println("Reverse sorted: " + numbers);  // [9, 8, 7, 5, 3, 2, 1]

        // reverse() - reverses the list
        Collections.reverse(numbers);
        System.out.println("Reversed: " + numbers);  // [1, 2, 3, 5, 7, 8, 9]

        // shuffle() - randomly shuffles
        Collections.shuffle(numbers);
        System.out.println("Shuffled: " + numbers);

        // binarySearch() - requires sorted list
        Collections.sort(numbers);
        int index = Collections.binarySearch(numbers, 5);
        System.out.println("Index of 5: " + index);

        // min() and max()
        System.out.println("Min: " + Collections.min(numbers));  // 1
        System.out.println("Max: " + Collections.max(numbers));  // 9

        // frequency() - count occurrences
        List<String> fruits = Arrays.asList("Apple", "Banana", "Apple", "Cherry", "Apple");
        System.out.println("Apple count: " + Collections.frequency(fruits, "Apple"));  // 3

        // disjoint() - check if two collections have no common elements
        List<Integer> list1 = Arrays.asList(1, 2, 3);
        List<Integer> list2 = Arrays.asList(4, 5, 6);
        List<Integer> list3 = Arrays.asList(3, 4, 5);
        System.out.println("list1 and list2 disjoint: " + Collections.disjoint(list1, list2));  // true
        System.out.println("list1 and list3 disjoint: " + Collections.disjoint(list1, list3));  // false

        // fill() - replace all elements
        List<String> names = new ArrayList<>(Arrays.asList("A", "B", "C"));
        Collections.fill(names, "X");
        System.out.println("Filled: " + names);  // [X, X, X]

        // replaceAll()
        List<String> words = new ArrayList<>(Arrays.asList("hello", "world", "hello"));
        Collections.replaceAll(words, "hello", "hi");
        System.out.println("Replaced: " + words);  // [hi, world, hi]

        // copy()
        List<String> source = Arrays.asList("A", "B", "C");
        List<String> dest = new ArrayList<>(Arrays.asList("1", "2", "3", "4"));
        Collections.copy(dest, source);
        System.out.println("After copy: " + dest);  // [A, B, C, 4]

        // swap()
        List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
        Collections.swap(nums, 0, 4);
        System.out.println("After swap: " + nums);  // [5, 2, 3, 4, 1]

        // addAll()
        List<String> list = new ArrayList<>();
        Collections.addAll(list, "A", "B", "C");
        System.out.println("After addAll: " + list);  // [A, B, C]
    }
}
```

### المثال 6: المجموعات المتزامنة وغير القابلة للتعديل

```java
import java.util.*;

public class ThreadSafeCollections {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");

        // Synchronized collections - thread-safe
        List<String> syncList = Collections.synchronizedList(list);
        Set<String> syncSet = Collections.synchronizedSet(new HashSet<>());
        Map<String, Integer> syncMap = Collections.synchronizedMap(new HashMap<>());

        // IMPORTANT: Manual synchronization needed for iteration
        synchronized(syncList) {
            for (String item : syncList) {
                System.out.println(item);
            }
        }

        // Unmodifiable collections - read-only
        List<String> unmodList = Collections.unmodifiableList(list);
        System.out.println("Unmodifiable list: " + unmodList);

        try {
            unmodList.add("Cherry");  // UnsupportedOperationException!
        } catch (UnsupportedOperationException e) {
            System.out.println("Cannot modify unmodifiable list");
        }

        // Changes to original list affect unmodifiable view
        list.add("Cherry");
        System.out.println("Unmodifiable list after original change: " + unmodList);
        // Output includes Cherry

        // Singleton collections
        Set<String> singleton = Collections.singleton("OnlyElement");
        System.out.println("Singleton: " + singleton);  // [OnlyElement]

        // Empty collections
        List<String> emptyList = Collections.emptyList();
        Set<String> emptySet = Collections.emptySet();
        Map<String, String> emptyMap = Collections.emptyMap();

        // Checked collections - runtime type checking
        List<String> checkedList = Collections.checkedList(new ArrayList<>(), String.class);
        checkedList.add("Valid");
        // checkedList.add(123);  // Would throw ClassCastException at runtime
    }
}
```

### المثال 7: أنماط التكرار

```java
import java.util.*;

public class IterationPatterns {
    public static void main(String[] args) {
        List<String> fruits = new ArrayList<>(Arrays.asList("Apple", "Banana", "Cherry", "Date"));

        // 1. Traditional for loop (index-based)
        System.out.println("Traditional for loop:");
        for (int i = 0; i < fruits.size(); i++) {
            System.out.println(i + ": " + fruits.get(i));
        }

        // 2. Enhanced for loop (for-each)
        System.out.println("\nEnhanced for loop:");
        for (String fruit : fruits) {
            System.out.println(fruit);
        }

        // 3. Iterator
        System.out.println("\nIterator:");
        Iterator<String> iterator = fruits.iterator();
        while (iterator.hasNext()) {
            String fruit = iterator.next();
            System.out.println(fruit);
            if (fruit.equals("Banana")) {
                iterator.remove();  // Safe removal during iteration
            }
        }
        System.out.println("After iterator removal: " + fruits);

        // 4. ListIterator (bidirectional)
        System.out.println("\nListIterator (reverse):");
        ListIterator<String> listIterator = fruits.listIterator(fruits.size());
        while (listIterator.hasPrevious()) {
            System.out.println(listIterator.previous());
        }

        // 5. forEach (Java 8)
        System.out.println("\nforEach method:");
        fruits.forEach(fruit -> System.out.println(fruit));

        // 6. Stream API (Java 8)
        System.out.println("\nStream API:");
        fruits.stream()
              .filter(f -> f.startsWith("C"))
              .forEach(System.out::println);

        // Map iteration
        Map<String, Integer> map = new HashMap<>();
        map.put("Apple", 5);
        map.put("Banana", 3);
        map.put("Cherry", 7);

        // Iterate through entries
        System.out.println("\nMap iteration (entrySet):");
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }

        // Java 8 forEach
        System.out.println("\nMap forEach:");
        map.forEach((key, value) -> System.out.println(key + " = " + value));

        // Iterate keys only
        System.out.println("\nKeys only:");
        for (String key : map.keySet()) {
            System.out.println(key);
        }

        // Iterate values only
        System.out.println("\nValues only:");
        for (Integer value : map.values()) {
            System.out.println(value);
        }
    }
}
```

### المثال 8: أمثلة واقعية

```java
import java.util.*;

public class RealWorldExamples {
    public static void main(String[] args) {
        // Example 1: User session management (HashMap)
        Map<String, String> sessionMap = new HashMap<>();
        sessionMap.put("session123", "user_john");
        sessionMap.put("session456", "user_jane");
        String user = sessionMap.get("session123");
        System.out.println("User for session123: " + user);

        // Example 2: Remove duplicates (HashSet)
        List<Integer> numbersWithDuplicates = Arrays.asList(1, 2, 2, 3, 4, 4, 5);
        Set<Integer> uniqueNumbers = new HashSet<>(numbersWithDuplicates);
        System.out.println("Unique numbers: " + uniqueNumbers);

        // Example 3: Leaderboard (TreeMap - sorted by score)
        Map<Integer, String> leaderboard = new TreeMap<>(Comparator.reverseOrder());
        leaderboard.put(95, "Alice");
        leaderboard.put(88, "Bob");
        leaderboard.put(92, "Charlie");
        System.out.println("Leaderboard: " + leaderboard);

        // Example 4: LRU Cache (LinkedHashMap with access order)
        class LRUCache<K, V> extends LinkedHashMap<K, V> {
            private final int capacity;

            public LRUCache(int capacity) {
                super(capacity, 0.75f, true);  // true = access order
                this.capacity = capacity;
            }

            @Override
            protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
                return size() > capacity;
            }
        }

        LRUCache<String, String> cache = new LRUCache<>(3);
        cache.put("key1", "value1");
        cache.put("key2", "value2");
        cache.put("key3", "value3");
        cache.get("key1");  // Access key1
        cache.put("key4", "value4");  // This removes key2 (least recently used)
        System.out.println("LRU Cache: " + cache);  // {key3=value3, key1=value1, key4=value4}

        // Example 5: Word frequency counter (HashMap)
        String text = "the quick brown fox jumps over the lazy dog the";
        Map<String, Integer> wordCount = new HashMap<>();
        for (String word : text.split(" ")) {
            wordCount.put(word, wordCount.getOrDefault(word, 0) + 1);
        }
        System.out.println("Word frequencies: " + wordCount);

        // Example 6: Task scheduling (PriorityQueue)
        class Task implements Comparable<Task> {
            String name;
            int priority;

            Task(String name, int priority) {
                this.name = name;
                this.priority = priority;
            }

            public int compareTo(Task other) {
                return Integer.compare(this.priority, other.priority);
            }

            public String toString() {
                return name;
            }
        }

        PriorityQueue<Task> taskQueue = new PriorityQueue<>();
        taskQueue.offer(new Task("Low priority task", 3));
        taskQueue.offer(new Task("High priority task", 1));
        taskQueue.offer(new Task("Medium priority task", 2));

        System.out.println("Processing tasks by priority:");
        while (!taskQueue.isEmpty()) {
            System.out.println(taskQueue.poll());
        }

        // Example 7: Finding common elements (retainAll)
        Set<String> set1 = new HashSet<>(Arrays.asList("A", "B", "C", "D"));
        Set<String> set2 = new HashSet<>(Arrays.asList("C", "D", "E", "F"));
        set1.retainAll(set2);  // Intersection
        System.out.println("Common elements: " + set1);  // [C, D]
    }
}
```

---

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما الفرق بين ArrayList و LinkedList؟**
2. **ما الفرق بين HashSet و TreeSet؟**
3. **ما الفرق بين HashMap و Hashtable؟**
4. **كيف يعمل HashMap داخلياً؟**
5. **ما الفرق بين Collection و Collections؟**
6. **كيف تجعل المجموعة آمنة للخيوط؟**
7. **ما هي المكررات السريعة الفشل والآمنة من الفشل؟**
8. **متى تستخدم TreeMap بدلاً من HashMap؟**
9. **ما هو تعقيد الوقت للعمليات في HashMap؟**
10. **هل يمكن لـ HashMap تخزين مفاتيح وقيم null؟**

---

## 6️⃣ نماذج الإجابات

**س1: ما الفرق بين ArrayList و LinkedList؟**

"ArrayList مدعومة بمصفوفة ديناميكية وتوفر وصولاً عشوائياً بتعقيد O(1) باستخدام get(index)، مما يجعلها مثالية عند الحاجة لاسترجاع العناصر بسرعة. LinkedList تستخدم بنية قائمة مرتبطة مزدوجة مع وقت وصول O(n) لكن إدراج/حذف بتعقيد O(1) في الأطراف، مما يجعلها أفضل لعمليات الإضافة/الإزالة المتكررة. ArrayList أكثر كفاءة في استخدام الذاكرة لمعظم الحالات ولها موقع تخزين مؤقت أفضل. LinkedList تنفذ كلاً من واجهتي List و Deque، لذا يمكن أن تعمل كطابور. عملياً، يُفضل ArrayList إلا إذا كنت تحتاج إدراجات/حذف متكررة في مواضع عشوائية."

**س2: ما الفرق بين HashSet و TreeSet؟**

"HashSet مدعومة بـ HashMap وتوفر وقتاً متوسطاً O(1) لعمليات الإضافة والإزالة والبحث، بدون ترتيب مضمون. TreeSet مدعومة بـ TreeMap (شجرة أحمر-أسود) وتحافظ على العناصر مرتبة، لكن العمليات تكون O(log n). استخدم HashSet عند الحاجة لعمليات سريعة ولا تهتم بالترتيب. استخدم TreeSet عند الحاجة لمجموعة مرتبة أو للقيام بعمليات النطاق. HashSet تسمح بعنصر null واحد، بينما TreeSet لا تسمح بـ null لأنها تحتاج مقارنة العناصر للفرز."

**س3: ما الفرق بين HashMap و Hashtable؟**

"HashMap غير متزامنة وتسمح بمفتاح null واحد وعدة قيم null، مما يجعلها أسرع للتطبيقات أحادية الخيط. Hashtable متزامنة (آمنة للخيوط) لكن أبطأ، ولا تسمح بأي مفاتيح أو قيم null. يُفضل HashMap في الكود الحديث - إذا احتجت أمان الخيوط، استخدم ConcurrentHashMap بدلاً من Hashtable. Hashtable هي فئة قديمة من Java 1.0، بينما HashMap قُدمت في Java 1.2 كجزء من إطار المجموعات. HashMap تدعم أيضاً التكرار عبر مكرراتها السريعة الفشل."

**س4: كيف يعمل HashMap داخلياً؟**

"HashMap تستخدم مصفوفة من السلال حيث يمكن لكل سلة أن تحتوي قائمة مرتبطة أو شجرة من المدخلات. عند وضع زوج مفتاح-قيمة، يحسب HashMap الـ hashCode للمفتاح، ويطبق دالة هاش لتحديد فهرس السلة، ويخزن المدخل هناك. إذا تحولت مفاتيح متعددة إلى نفس السلة (تصادم)، تُخزن في قائمة مرتبطة. منذ Java 8، عندما تتجاوز قائمة السلة 8 عناصر، تتحول إلى شجرة متوازنة لأداء أفضل. السعة الابتدائية الافتراضية هي 16 سلة، وتُعاد الحجم عند امتلاء 75% (عامل الحمل 0.75). عمليات Get و put هي O(1) في المتوسط، لكن O(n) في أسوأ الحالات عند حدوث تصادمات كثيرة."

**س5: ما الفرق بين Collection و Collections؟**

"Collection هي واجهة تمثل مجموعة من الكائنات، جذر تسلسل المجموعات الهرمي. تُوسّع بواجهات List و Set و Queue. Collections هي فئة أدوات مساعدة بطرق ثابتة للعمل على المجموعات، مثل sort() و reverse() و shuffle() و binarySearch() و min() و max()، وطرق لإنشاء أغلفة متزامنة أو غير قابلة للتعديل. تُنفذ Collection في فئاتك، لكن تستدعي طرق فئة Collections لمعالجتها. فكر في Collection كالنوع، وCollections كصندوق الأدوات."

**س6: كيف تجعل المجموعة آمنة للخيوط؟**

"هناك عدة مناهج. أولاً، استخدم Collections.synchronizedList() أو synchronizedSet() أو synchronizedMap() لتغليف مجموعة، مع ضرورة المزامنة اليدوية أثناء التكرار. ثانياً، استخدم المجموعات المتزامنة من java.util.concurrent مثل ConcurrentHashMap و CopyOnWriteArrayList أو ConcurrentLinkedQueue، التي توفر أداءً أفضل عبر القفل الدقيق أو بدون قفل. ثالثاً، استخدم Vector أو Hashtable القديمين، رغم أنها غير موصى بها. أفضل الممارسات استخدام المجموعات المتزامنة لأنها مصممة خصيصاً للسيناريوهات متعددة الخيوط."

**س7: ما هي المكررات السريعة الفشل والآمنة من الفشل؟**

"المكررات السريعة الفشل ترمي ConcurrentModificationException إذا عُدّلت المجموعة هيكلياً أثناء التكرار، باستثناء طريقة remove الخاصة بالمكرر. معظم المجموعات في java.util تستخدم مكررات سريعة الفشل. تكتشف التعديل بفحص عداد التعديلات. المكررات الآمنة من الفشل تعمل على نسخة مستنسخة من المجموعة، لذا لا ترمي استثناءات عند تعديل الأصل، لكن قد لا تعكس أحدث حالة. المجموعات في java.util.concurrent مثل CopyOnWriteArrayList تستخدم مكررات آمنة من الفشل. السريعة الفشل توفر كشفاً مبكراً للمشاكل، بينما الآمنة من الفشل تسمح بالتعديلات أثناء التكرار."

**س8: متى تستخدم TreeMap بدلاً من HashMap؟**

"استخدم TreeMap عند الحاجة للمفاتيح مرتبة، سواء بالترتيب الطبيعي أو بمقارن مخصص. TreeMap مفيدة لاستعلامات النطاق، وإيجاد أدنى أو أعلى مفتاح، أو التكرار بترتيب مرتب. العمليات O(log n) مقارنة بمتوسط O(1) لـ HashMap. استخدم TreeMap لسيناريوهات مثل الحفاظ على ذاكرة تخزين مؤقت مرتبة لأسعار الأسهم حسب الرمز، أو تنفيذ دليل هاتف بأسماء مرتبة أبجدياً، أو الحفاظ على بيانات السلاسل الزمنية مرتبة بالطابع الزمني. HashMap أسرع لكن لا تحافظ على الترتيب، لذا اختر بناءً على ما إذا كان الترتيب يستحق تكلفة الأداء."

**س9: ما هو تعقيد الوقت للعمليات في HashMap؟**

"في الحالة المتوسطة مع توزيع هاش جيد، get و put و remove هي O(1) وقت ثابت. في أسوأ الحالات مع تحويل جميع المفاتيح لنفس السلة، العمليات تتدهور إلى O(n) حيث n هو عدد المدخلات، رغم أن Java 8 تحسن هذا إلى O(log n) باستخدام الأشجار عندما تتجاوز السلال 8 مدخلات. التكرار على جميع المدخلات هو O(n) حيث n هو السعة زائد الحجم. عملية contains هي O(1) متوسط. إعادة الهاش عند نمو الخريطة تأخذ وقت O(n) لكنها موزعة على الإدراجات."

**س10: هل يمكن لـ HashMap تخزين مفاتيح وقيم null؟**

"نعم، HashMap تسمح بمفتاح null واحد وعدة قيم null. المفتاح null يُخزن دائماً في الفهرس 0 من المصفوفة الداخلية لأن hashCode لا يمكن استدعاؤه على null. هذا مختلف عن Hashtable التي لا تسمح بأي مفاتيح أو قيم null، وTreeMap التي تسمح بقيم null لكن ليس مفاتيح null لأنها تحتاج مقارنة المفاتيح للفرز. ConcurrentHashMap أيضاً لا تسمح بمفاتيح أو قيم null لتجنب الغموض في السيناريوهات المتزامنة حيث يمكن أن تعني null إما 'غير موجود' أو 'موجود بقيمة null'."

---

## 7️⃣ الأخطاء الشائعة

1. **استخدام == بدلاً من equals() للبحث**:
   ```java
   List<String> list = new ArrayList<>();
   list.add(new String("Hello"));
   System.out.println(list.contains(new String("Hello")));  // true (uses equals)
   // list.contains() uses equals(), not ==
   ```

2. **تعديل المجموعة أثناء التكرار**:
   ```java
   List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
   for (String s : list) {
       list.remove(s);  // ConcurrentModificationException!
   }

   // Use Iterator.remove() instead
   Iterator<String> it = list.iterator();
   while (it.hasNext()) {
       it.next();
       it.remove();  // Safe
   }
   ```

3. **عدم تجاوز hashCode() عند تجاوز equals()**:
   ```java
   class Person {
       String name;

       @Override
       public boolean equals(Object o) {
           return ((Person) o).name.equals(this.name);
       }
       // Must override hashCode() too for HashSet/HashMap to work correctly!
   }
   ```

4. **افتراض أن HashMap تحافظ على الترتيب**:
   ```java
   Map<String, Integer> map = new HashMap<>();
   map.put("B", 2);
   map.put("A", 1);
   // Iteration order is NOT guaranteed
   // Use LinkedHashMap for insertion order
   ```

5. **استخدام ArrayList للإدراج/الحذف المتكرر**:
   ```java
   List<Integer> list = new ArrayList<>();
   for (int i = 0; i < 10000; i++) {
       list.add(0, i);  // O(n) each time - very slow!
   }
   // Use LinkedList for frequent insertions at beginning
   ```

6. **عدم تحديد السعة الابتدائية للمجموعات الكبيرة**:
   ```java
   List<Integer> list = new ArrayList<>();  // Default capacity 10
   for (int i = 0; i < 100000; i++) {
       list.add(i);  // Multiple resizing operations
   }

   // Better:
   List<Integer> list = new ArrayList<>(100000);  // Pre-size
   ```

7. **الخلط في التسلسل الهرمي للمجموعات**:
   ```java
   // Map is NOT part of Collection hierarchy
   // Map<String, Integer> map = (Collection) new HashMap<>();  // Error!
   ```

8. **استخدام null في TreeSet/TreeMap**:
   ```java
   Set<String> set = new TreeSet<>();
   set.add(null);  // NullPointerException!
   ```

---

## 8️⃣ نصائح للمقابلات الحقيقية

**ما يجب التأكيد عليه**:
- List: ArrayList (وصول سريع) مقابل LinkedList (إدراج سريع)
- Set: HashSet (الأسرع) مقابل TreeSet (مرتبة) مقابل LinkedHashSet (مرتبة بالإدراج)
- Map: HashMap (الأسرع) مقابل TreeMap (مرتبة) مقابل LinkedHashMap (مرتبة بالإدراج)
- تعقيدات الوقت: HashMap O(1)، TreeMap O(log n)، وصول ArrayList O(1)
- فئة Collections المساعدة للعمليات

**ما يجب تجنبه**:
- لا تخلط بين Collection (واجهة) وCollections (فئة مساعدة)
- لا تدّعي أن HashMap آمنة للخيوط (ليست كذلك)
- لا تقل أن ArrayList دائماً أفضل من LinkedList (يعتمد على الاستخدام)
- لا تنسَ أن Map منفصلة عن تسلسل Collection الهرمي

**تحت الضغط**:
- ارسم التسلسل الهرمي: Collection → List/Set/Queue، تسلسل Map المنفصل
- تذكر: Hash = سريع، Tree = مرتب، Linked = بترتيب الإدراج
- استخدم أمثلة واقعية: عربة التسوق (ArrayList)، معرفات فريدة (HashSet)، الإعدادات (HashMap)
- اعرف تعقيدات الوقت للعمليات الشائعة

**علامات تحذيرية يجب تجنبها**:
- "HashMap و Hashtable متطابقتان" (مختلفتان في أمان الخيوط ومعالجة null)
- "ArrayList و LinkedList لهما نفس الأداء" (مختلفان جداً)
- "TreeSet أسرع من HashSet" (العكس صحيح)
- "لا يمكن أن يكون هناك تكرار في List" (List تسمح بالتكرار)

**نقاط إضافية**:
- اذكر تحسينات HashMap في Java 8 (سلال الأشجار)
- ناقش عامل الحمل وإعادة الهاش
- أشر إلى ConcurrentHashMap للخرائط الآمنة للخيوط
- اذكر المكررات السريعة الفشل مقابل الآمنة من الفشل
- ناقش كيف يؤثر equals() وhashCode() على المجموعات
- أشر لحالات استخدام محددة لكل نوع مجموعة
- اذكر NavigableSet/NavigableMap لعمليات النطاق

</div>
