# Collections Framework

## 1️⃣ Concept Overview

The **Java Collections Framework** is a unified architecture for representing and manipulating collections of objects. It provides interfaces, implementations, and algorithms to work with groups of objects efficiently.

**Core Collection Interfaces Hierarchy**:

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

**Key Characteristics**:
- **Interfaces**: Define abstract data types (List, Set, Queue, Map)
- **Implementations**: Concrete classes (ArrayList, HashSet, HashMap)
- **Algorithms**: Methods for sorting, searching, shuffling (Collections class)
- **Generic**: Type-safe collections using generics

**Collections vs Collection**:
- **Collection**: Interface representing a group of objects
- **Collections**: Utility class with static methods for operations

---

## 2️⃣ Why Interviewers Ask This

- **Fundamental framework**: Used in every Java application
- **Design decisions**: Tests ability to choose right collection type
- **Performance implications**: Understanding time complexities
- **Real-world scenarios**: Daily work involves collections
- **API knowledge**: Shows familiarity with Java standard library
- **Algorithm awareness**: Sorting, searching, iteration patterns

---

## 3️⃣ Key Rules / Facts

**List Interface** (Ordered, allows duplicates):

| Implementation | Underlying Structure | Access | Insert/Delete | Use When |
|----------------|---------------------|---------|---------------|----------|
| ArrayList | Dynamic array | O(1) | O(n) | Fast random access needed |
| LinkedList | Doubly-linked list | O(n) | O(1) | Frequent insertions/deletions |
| Vector | Synchronized array | O(1) | O(n) | Thread-safe list needed (legacy) |

**Set Interface** (No duplicates):

| Implementation | Underlying Structure | Add/Contains | Ordering | Use When |
|----------------|---------------------|--------------|----------|----------|
| HashSet | HashMap | O(1) avg | No order | Fast lookups, no duplicates |
| LinkedHashSet | HashMap + Linked list | O(1) avg | Insertion order | Order matters |
| TreeSet | Red-Black tree | O(log n) | Sorted | Sorted set needed |

**Queue Interface** (FIFO processing):

| Implementation | Underlying Structure | Offer/Poll | Ordering | Use When |
|----------------|---------------------|------------|----------|----------|
| LinkedList | Doubly-linked list | O(1) | FIFO | Standard queue |
| PriorityQueue | Binary heap | O(log n) | Priority order | Priority-based processing |

**Map Interface** (Key-value pairs):

| Implementation | Underlying Structure | Get/Put | Ordering | Use When |
|----------------|---------------------|---------|----------|----------|
| HashMap | Hash table | O(1) avg | No order | Fast lookups |
| LinkedHashMap | HashMap + Linked list | O(1) avg | Insertion order | Order matters |
| TreeMap | Red-Black tree | O(log n) | Sorted keys | Sorted map needed |
| Hashtable | Hash table (synchronized) | O(1) avg | No order | Thread-safe map (legacy) |

**Common Methods**:

**Collection Interface**:
- `add()`, `remove()`, `contains()`, `size()`, `isEmpty()`
- `clear()`, `addAll()`, `removeAll()`, `retainAll()`
- `iterator()`, `toArray()`, `stream()`

**List Interface** (additional):
- `get(index)`, `set(index, element)`, `indexOf()`, `lastIndexOf()`
- `add(index, element)`, `remove(index)`, `subList(from, to)`

**Set Interface**:
- Same as Collection (no additional methods for basic Set)

**Map Interface**:
- `put(key, value)`, `get(key)`, `remove(key)`, `containsKey()`, `containsValue()`
- `keySet()`, `values()`, `entrySet()`, `putAll()`, `replace()`

**Collections Utility Class**:
- `sort()`, `reverse()`, `shuffle()`, `binarySearch()`
- `min()`, `max()`, `frequency()`, `disjoint()`
- `synchronizedList()`, `synchronizedSet()`, `synchronizedMap()`
- `unmodifiableList()`, `unmodifiableSet()`, `unmodifiableMap()`

**Key Design Principles**:
- Fail-fast iterators (throw ConcurrentModificationException)
- Collections are not thread-safe by default (except legacy Vector, Hashtable)
- Use Collections.synchronized*() or concurrent collections for thread safety
- null handling varies: HashMap allows one null key, TreeMap doesn't allow null keys

---

## 4️⃣ Code Examples (Java 8)

### Example 1: ArrayList vs LinkedList

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

### Example 2: HashSet vs TreeSet vs LinkedHashSet

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

### Example 3: HashMap vs TreeMap vs LinkedHashMap

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

### Example 4: PriorityQueue

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

### Example 5: Collections Utility Class

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

### Example 6: Synchronized and Unmodifiable Collections

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

### Example 7: Iteration Patterns

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

### Example 8: Real-World Examples

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

## 5️⃣ Common Interview Questions

1. **What is the difference between ArrayList and LinkedList?**
2. **What is the difference between HashSet and TreeSet?**
3. **What is the difference between HashMap and Hashtable?**
4. **How does HashMap work internally?**
5. **What is the difference between Collection and Collections?**
6. **How do you make a collection thread-safe?**
7. **What is fail-fast and fail-safe iterators?**
8. **When would you use TreeMap over HashMap?**
9. **What is the time complexity of operations in HashMap?**
10. **Can HashMap store null keys and values?**

---

## 6️⃣ Model Answers

**Q1: What is the difference between ArrayList and LinkedList?**

"ArrayList is backed by a dynamic array providing O(1) random access using get(index), making it ideal when you need fast element retrieval. LinkedList uses a doubly-linked list structure with O(n) access time but O(1) insertion/deletion at the ends, making it better for frequent add/remove operations. ArrayList is more memory-efficient for most use cases and has better cache locality. LinkedList implements both List and Deque interfaces, so it can function as a queue. In practice, ArrayList is preferred unless you need frequent insertions/deletions at arbitrary positions."

**Q2: What is the difference between HashSet and TreeSet?**

"HashSet is backed by a HashMap and provides O(1) average time for add, remove, and contains operations, with no guaranteed order. TreeSet is backed by a TreeMap (Red-Black tree) and maintains elements in sorted order, but operations are O(log n). Use HashSet when you need fast operations and don't care about order. Use TreeSet when you need a sorted set or need to perform range operations. HashSet allows one null element, while TreeSet doesn't allow null because it needs to compare elements for sorting."

**Q3: What is the difference between HashMap and Hashtable?**

"HashMap is not synchronized and allows one null key and multiple null values, making it faster for single-threaded applications. Hashtable is synchronized (thread-safe) but slower, and doesn't allow any null keys or values. HashMap is preferred in modern code—if you need thread safety, use ConcurrentHashMap instead of Hashtable. Hashtable is a legacy class from Java 1.0, while HashMap was introduced in Java 1.2 as part of the Collections Framework. HashMap also supports iteration through its fail-fast iterators."

**Q4: How does HashMap work internally?**

"HashMap uses an array of buckets where each bucket can contain a linked list or tree of entries. When you put a key-value pair, HashMap calculates the key's hashCode, applies a hash function to determine the bucket index, and stores the entry there. If multiple keys hash to the same bucket (collision), they're stored in a linked list. Since Java 8, when a bucket's list exceeds 8 elements, it converts to a balanced tree for better performance. The default initial capacity is 16 buckets, and it resizes when 75% full (load factor 0.75). Get and put operations are O(1) on average, but O(n) in worst case if many collisions occur."

**Q5: What is the difference between Collection and Collections?**

"Collection is an interface that represents a group of objects, the root of the collection hierarchy. It's extended by List, Set, and Queue interfaces. Collections is a utility class with static methods for operating on collections, like sort(), reverse(), shuffle(), binarySearch(), min(), max(), and methods to create synchronized or unmodifiable wrappers. You implement Collection in your classes, but you call Collections class methods to manipulate them. Think of Collection as the type, and Collections as the toolbox."

**Q6: How do you make a collection thread-safe?**

"There are several approaches. First, use Collections.synchronizedList(), synchronizedSet(), or synchronizedMap() to wrap a collection, though you must manually synchronize during iteration. Second, use concurrent collections from java.util.concurrent like ConcurrentHashMap, CopyOnWriteArrayList, or ConcurrentLinkedQueue, which provide better performance through lock-free or fine-grained locking. Third, use the legacy Vector or Hashtable, though these are not recommended. The best practice is to use concurrent collections as they're designed specifically for multithreaded scenarios."

**Q7: What is fail-fast and fail-safe iterators?**

"Fail-fast iterators throw ConcurrentModificationException if the collection is structurally modified during iteration, except through the iterator's own remove method. Most collections in java.util use fail-fast iterators. They detect modification by checking a modification count. Fail-safe iterators work on a cloned copy of the collection, so they don't throw exceptions when the original is modified, but they might not reflect the latest state. Collections in java.util.concurrent like CopyOnWriteArrayList use fail-safe iterators. Fail-fast provides early detection of problems, while fail-safe allows modifications during iteration."

**Q8: When would you use TreeMap over HashMap?**

"Use TreeMap when you need keys in sorted order, either natural ordering or custom comparator order. TreeMap is useful for range queries, finding the lowest or highest key, or iterating in sorted order. Operations are O(log n) compared to HashMap's O(1) average. Use TreeMap for scenarios like maintaining a sorted cache of stock prices by symbol, implementing a phone directory with alphabetically sorted names, or maintaining time-series data sorted by timestamp. HashMap is faster but doesn't maintain order, so choose based on whether sorted order is worth the performance cost."

**Q9: What is the time complexity of operations in HashMap?**

"In average case with good hash distribution, get, put, and remove are O(1) constant time. In worst case with all keys hashing to the same bucket, operations degrade to O(n) where n is the number of entries, though Java 8 improves this to O(log n) by using trees when buckets exceed 8 entries. Iteration over all entries is O(n) where n is capacity plus size. The contains operation is O(1) average. Rehashing when the map grows takes O(n) time but is amortized across insertions."

**Q10: Can HashMap store null keys and values?**

"Yes, HashMap allows one null key and multiple null values. The null key is always stored at index 0 of the internal array since hashCode cannot be called on null. This is different from Hashtable which doesn't allow any null keys or values, and TreeMap which allows null values but not null keys because it needs to compare keys for sorting. ConcurrentHashMap also doesn't allow null keys or values to avoid ambiguity in concurrent scenarios where null could mean either 'not found' or 'found with null value'."

---

## 7️⃣ Common Mistakes

1. **Using == instead of equals() for contains**:
   ```java
   List<String> list = new ArrayList<>();
   list.add(new String("Hello"));
   System.out.println(list.contains(new String("Hello")));  // true (uses equals)
   // list.contains() uses equals(), not ==
   ```

2. **Modifying collection during iteration**:
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

3. **Not overriding hashCode() when overriding equals()**:
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

4. **Assuming HashMap maintains order**:
   ```java
   Map<String, Integer> map = new HashMap<>();
   map.put("B", 2);
   map.put("A", 1);
   // Iteration order is NOT guaranteed
   // Use LinkedHashMap for insertion order
   ```

5. **Using ArrayList for frequent insertions/deletions**:
   ```java
   List<Integer> list = new ArrayList<>();
   for (int i = 0; i < 10000; i++) {
       list.add(0, i);  // O(n) each time - very slow!
   }
   // Use LinkedList for frequent insertions at beginning
   ```

6. **Not specifying initial capacity for large collections**:
   ```java
   List<Integer> list = new ArrayList<>();  // Default capacity 10
   for (int i = 0; i < 100000; i++) {
       list.add(i);  // Multiple resizing operations
   }
   
   // Better:
   List<Integer> list = new ArrayList<>(100000);  // Pre-size
   ```

7. **Mixing up Collection hierarchy**:
   ```java
   // Map is NOT part of Collection hierarchy
   // Map<String, Integer> map = (Collection) new HashMap<>();  // Error!
   ```

8. **Using null in TreeSet/TreeMap**:
   ```java
   Set<String> set = new TreeSet<>();
   set.add(null);  // NullPointerException!
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- List: ArrayList (fast access) vs LinkedList (fast insertion)
- Set: HashSet (fastest) vs TreeSet (sorted) vs LinkedHashSet (ordered)
- Map: HashMap (fastest) vs TreeMap (sorted) vs LinkedHashMap (ordered)
- Time complexities: HashMap O(1), TreeMap O(log n), ArrayList access O(1)
- Collections utility class for operations

**What to avoid**:
- Don't confuse Collection (interface) with Collections (utility class)
- Don't claim HashMap is thread-safe (it's not)
- Don't say ArrayList is always better than LinkedList (depends on use case)
- Don't forget that Map is separate from Collection hierarchy

**Under pressure**:
- Draw the hierarchy: Collection → List/Set/Queue, separate Map hierarchy
- Remember: Hash = fast, Tree = sorted, Linked = ordered
- Use real examples: shopping cart (ArrayList), unique IDs (HashSet), config (HashMap)
- Know time complexities for common operations

**Red flags to avoid**:
- "HashMap and Hashtable are the same" (different thread-safety, null handling)
- "ArrayList and LinkedList have same performance" (very different)
- "TreeSet is faster than HashSet" (opposite is true)
- "You can't have duplicates in List" (Lists allow duplicates)

**Bonus points**:
- Mention HashMap improvements in Java 8 (tree buckets)
- Discuss load factor and rehashing
- Reference ConcurrentHashMap for thread-safe maps
- Mention fail-fast vs fail-safe iterators
- Discuss how equals() and hashCode() affect collections
- Reference specific use cases for each collection type
- Mention NavigableSet/NavigableMap for range operations
