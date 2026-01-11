# Stream API

## 1️⃣ Concept Overview

**Stream** is a sequence of elements supporting sequential and parallel aggregate operations. Introduced in Java 8, the Stream API enables functional-style operations on collections of elements.

**Key Characteristics**:
- **Not a data structure**: Stream doesn't store elements
- **Functional in nature**: Operations produce results without modifying source
- **Lazy evaluation**: Intermediate operations are not executed until terminal operation
- **Possibly unbounded**: Can work with infinite streams
- **Consumable**: Can be traversed only once

**Stream Pipeline**:
```
Source → Intermediate Operations → Terminal Operation
         (filter, map, sorted)    (collect, forEach, reduce)
```

**Three Parts of Stream**:
1. **Source**: Collection, array, I/O channel, generator function
2. **Intermediate Operations**: Transform stream (lazy)
3. **Terminal Operation**: Produces result or side effect (triggers execution)

**Intermediate vs Terminal Operations**:

| Intermediate | Terminal |
|--------------|----------|
| Returns Stream | Returns non-Stream result |
| Lazy (not executed immediately) | Eager (triggers pipeline execution) |
| Can be chained | Ends the pipeline |
| Examples: filter, map, sorted | Examples: collect, forEach, reduce |

**Types of Streams**:
- `Stream<T>`: Object stream
- `IntStream`, `LongStream`, `DoubleStream`: Primitive streams
- Sequential streams (default)
- Parallel streams (for parallel processing)

---

## 2️⃣ Why Interviewers Ask This

- **Modern Java**: Stream API is central to Java 8+
- **Functional programming**: Tests understanding of functional paradigm
- **Performance**: Understanding lazy evaluation and parallel streams
- **Code quality**: Streams lead to more readable, declarative code
- **Common operations**: Used daily in production code
- **Interview favorite**: Very frequently asked topic

---

## 3️⃣ Key Rules / Facts

**Stream Lifecycle**:
1. Create stream from source
2. Apply zero or more intermediate operations
3. Apply terminal operation (required to execute pipeline)
4. Stream is closed and cannot be reused

**Intermediate Operations** (Lazy, Returns Stream):

| Operation | Description | Example |
|-----------|-------------|---------|
| filter(Predicate) | Filters elements | `filter(x -> x > 10)` |
| map(Function) | Transforms elements | `map(x -> x * 2)` |
| flatMap(Function) | Flattens nested structures | `flatMap(list -> list.stream())` |
| distinct() | Removes duplicates | `distinct()` |
| sorted() | Sorts elements | `sorted()` or `sorted(Comparator)` |
| peek(Consumer) | Performs action without modifying | `peek(System.out::println)` |
| limit(long) | Limits stream size | `limit(10)` |
| skip(long) | Skips first n elements | `skip(5)` |

**Terminal Operations** (Eager, Triggers Execution):

| Operation | Return Type | Description |
|-----------|-------------|-------------|
| forEach(Consumer) | void | Performs action on each element |
| collect(Collector) | Collection/Map | Collects elements into collection |
| reduce(BinaryOperator) | Optional<T> | Reduces to single value |
| count() | long | Counts elements |
| anyMatch(Predicate) | boolean | Tests if any match |
| allMatch(Predicate) | boolean | Tests if all match |
| noneMatch(Predicate) | boolean | Tests if none match |
| findFirst() | Optional<T> | Returns first element |
| findAny() | Optional<T> | Returns any element |
| min(Comparator) | Optional<T> | Returns minimum |
| max(Comparator) | Optional<T> | Returns maximum |
| toArray() | Object[] | Converts to array |

**Collectors** (Common static methods from Collectors class):

| Collector | Purpose | Example |
|-----------|---------|---------|
| toList() | Collect to List | `collect(Collectors.toList())` |
| toSet() | Collect to Set | `collect(Collectors.toSet())` |
| toMap() | Collect to Map | `collect(Collectors.toMap(k, v))` |
| joining() | Join strings | `collect(Collectors.joining(", "))` |
| counting() | Count elements | `collect(Collectors.counting())` |
| summingInt() | Sum integers | `collect(Collectors.summingInt(mapper))` |
| averagingDouble() | Average | `collect(Collectors.averagingDouble(mapper))` |
| groupingBy() | Group by key | `collect(Collectors.groupingBy(classifier))` |
| partitioningBy() | Partition by predicate | `collect(Collectors.partitioningBy(predicate))` |

**Important Rules**:
- Stream can only be consumed once
- Intermediate operations are lazy (not executed until terminal operation)
- Terminal operation closes the stream
- Streams are not data structures (don't store elements)
- Operations should be stateless and non-interfering
- Parallel streams may not maintain order

**Creating Streams**:
- From collections: `list.stream()`
- From arrays: `Arrays.stream(array)`
- From values: `Stream.of(1, 2, 3)`
- Empty stream: `Stream.empty()`
- Infinite streams: `Stream.generate()`, `Stream.iterate()`
- Primitive streams: `IntStream.range()`, `IntStream.of()`

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Basic Stream Operations

```java
import java.util.*;
import java.util.stream.*;

public class BasicStreamOperations {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        // 1. filter - select elements matching predicate
        List<Integer> evenNumbers = numbers.stream()
            .filter(n -> n % 2 == 0)
            .collect(Collectors.toList());
        System.out.println("Even numbers: " + evenNumbers);  // [2, 4, 6, 8, 10]
        
        // 2. map - transform elements
        List<Integer> squares = numbers.stream()
            .map(n -> n * n)
            .collect(Collectors.toList());
        System.out.println("Squares: " + squares);  // [1, 4, 9, 16, 25, ...]
        
        // 3. filter + map combination
        List<Integer> evenSquares = numbers.stream()
            .filter(n -> n % 2 == 0)
            .map(n -> n * n)
            .collect(Collectors.toList());
        System.out.println("Even squares: " + evenSquares);  // [4, 16, 36, 64, 100]
        
        // 4. forEach - perform action on each element
        System.out.print("Numbers: ");
        numbers.stream()
            .filter(n -> n > 5)
            .forEach(n -> System.out.print(n + " "));  // 6 7 8 9 10
        System.out.println();
        
        // 5. count - count elements
        long count = numbers.stream()
            .filter(n -> n > 5)
            .count();
        System.out.println("Count > 5: " + count);  // 5
        
        // 6. distinct - remove duplicates
        List<Integer> withDuplicates = Arrays.asList(1, 2, 2, 3, 3, 3, 4, 4, 4, 4);
        List<Integer> unique = withDuplicates.stream()
            .distinct()
            .collect(Collectors.toList());
        System.out.println("Unique: " + unique);  // [1, 2, 3, 4]
        
        // 7. sorted - sort elements
        List<Integer> unsorted = Arrays.asList(5, 2, 8, 1, 9, 3);
        List<Integer> sorted = unsorted.stream()
            .sorted()
            .collect(Collectors.toList());
        System.out.println("Sorted: " + sorted);  // [1, 2, 3, 5, 8, 9]
        
        // Sort in reverse
        List<Integer> reverseSorted = unsorted.stream()
            .sorted(Comparator.reverseOrder())
            .collect(Collectors.toList());
        System.out.println("Reverse sorted: " + reverseSorted);  // [9, 8, 5, 3, 2, 1]
        
        // 8. limit - limit stream size
        List<Integer> first5 = numbers.stream()
            .limit(5)
            .collect(Collectors.toList());
        System.out.println("First 5: " + first5);  // [1, 2, 3, 4, 5]
        
        // 9. skip - skip first n elements
        List<Integer> afterSkip = numbers.stream()
            .skip(7)
            .collect(Collectors.toList());
        System.out.println("After skip 7: " + afterSkip);  // [8, 9, 10]
        
        // 10. peek - perform action without modifying stream
        List<Integer> result = numbers.stream()
            .peek(n -> System.out.print("Before filter: " + n + " "))
            .filter(n -> n > 5)
            .peek(n -> System.out.print("After filter: " + n + " "))
            .collect(Collectors.toList());
        System.out.println("\nResult: " + result);
    }
}
```

### Example 2: reduce Operations

```java
import java.util.*;
import java.util.stream.*;

public class ReduceOperations {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        // 1. reduce with identity and accumulator
        int sum = numbers.stream()
            .reduce(0, (a, b) -> a + b);
        System.out.println("Sum: " + sum);  // 55
        
        // Using Integer::sum method reference
        int sum2 = numbers.stream()
            .reduce(0, Integer::sum);
        System.out.println("Sum (method ref): " + sum2);  // 55
        
        // 2. reduce without identity (returns Optional)
        Optional<Integer> sum3 = numbers.stream()
            .reduce((a, b) -> a + b);
        sum3.ifPresent(s -> System.out.println("Sum (Optional): " + s));  // 55
        
        // 3. Product using reduce
        int product = numbers.stream()
            .reduce(1, (a, b) -> a * b);
        System.out.println("Product: " + product);  // 3628800
        
        // 4. Maximum using reduce
        Optional<Integer> max = numbers.stream()
            .reduce((a, b) -> a > b ? a : b);
        max.ifPresent(m -> System.out.println("Max: " + m));  // 10
        
        // Better: use max() method
        Optional<Integer> max2 = numbers.stream()
            .max(Integer::compare);
        max2.ifPresent(m -> System.out.println("Max (built-in): " + m));  // 10
        
        // 5. Minimum
        Optional<Integer> min = numbers.stream()
            .min(Integer::compare);
        min.ifPresent(m -> System.out.println("Min: " + m));  // 1
        
        // 6. String concatenation
        List<String> words = Arrays.asList("Java", "Stream", "API");
        String concatenated = words.stream()
            .reduce("", (a, b) -> a + b);
        System.out.println("Concatenated: " + concatenated);  // JavaStreamAPI
        
        // With delimiter
        String joined = words.stream()
            .reduce("", (a, b) -> a.isEmpty() ? b : a + " " + b);
        System.out.println("Joined: " + joined);  // Java Stream API
        
        // Better: use joining collector
        String joined2 = words.stream()
            .collect(Collectors.joining(" "));
        System.out.println("Joined (collector): " + joined2);  // Java Stream API
        
        // 7. Complex reduce - sum of squares
        int sumOfSquares = numbers.stream()
            .map(n -> n * n)
            .reduce(0, Integer::sum);
        System.out.println("Sum of squares: " + sumOfSquares);  // 385
        
        // 8. Count using reduce
        long count = numbers.stream()
            .map(n -> 1)
            .reduce(0, Integer::sum);
        System.out.println("Count: " + count);  // 10
    }
}
```

### Example 3: Collectors - Collecting Results

```java
import java.util.*;
import java.util.stream.*;

public class CollectorOperations {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve", "Frank");
        
        // 1. toList()
        List<String> list = names.stream()
            .filter(s -> s.length() > 3)
            .collect(Collectors.toList());
        System.out.println("List: " + list);
        
        // 2. toSet()
        List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3, 4);
        Set<Integer> set = numbers.stream()
            .collect(Collectors.toSet());
        System.out.println("Set: " + set);  // [1, 2, 3, 4]
        
        // 3. toMap()
        Map<String, Integer> nameLength = names.stream()
            .collect(Collectors.toMap(
                name -> name,           // key
                name -> name.length()   // value
            ));
        System.out.println("Name lengths: " + nameLength);
        
        // 4. joining()
        String joined = names.stream()
            .collect(Collectors.joining());
        System.out.println("Joined: " + joined);  // AliceBobCharlie...
        
        // With delimiter
        String joinedWithComma = names.stream()
            .collect(Collectors.joining(", "));
        System.out.println("Joined with comma: " + joinedWithComma);
        
        // With delimiter, prefix, and suffix
        String formatted = names.stream()
            .collect(Collectors.joining(", ", "[", "]"));
        System.out.println("Formatted: " + formatted);  // [Alice, Bob, Charlie, ...]
        
        // 5. counting()
        long count = names.stream()
            .filter(s -> s.startsWith("A"))
            .collect(Collectors.counting());
        System.out.println("Count starting with A: " + count);  // 1
        
        // 6. summingInt(), summingLong(), summingDouble()
        int totalLength = names.stream()
            .collect(Collectors.summingInt(String::length));
        System.out.println("Total length: " + totalLength);
        
        // 7. averagingInt(), averagingLong(), averagingDouble()
        double avgLength = names.stream()
            .collect(Collectors.averagingDouble(String::length));
        System.out.println("Average length: " + avgLength);
        
        // 8. summarizingInt() - statistics
        IntSummaryStatistics stats = names.stream()
            .collect(Collectors.summarizingInt(String::length));
        System.out.println("Statistics: " + stats);
        System.out.println("Count: " + stats.getCount());
        System.out.println("Sum: " + stats.getSum());
        System.out.println("Min: " + stats.getMin());
        System.out.println("Max: " + stats.getMax());
        System.out.println("Average: " + stats.getAverage());
        
        // 9. groupingBy()
        Map<Integer, List<String>> groupedByLength = names.stream()
            .collect(Collectors.groupingBy(String::length));
        System.out.println("Grouped by length: " + groupedByLength);
        
        // groupingBy with counting
        Map<Integer, Long> countByLength = names.stream()
            .collect(Collectors.groupingBy(
                String::length,
                Collectors.counting()
            ));
        System.out.println("Count by length: " + countByLength);
        
        // 10. partitioningBy()
        Map<Boolean, List<String>> partitioned = names.stream()
            .collect(Collectors.partitioningBy(s -> s.length() > 4));
        System.out.println("Partitioned: " + partitioned);
        System.out.println("Long names: " + partitioned.get(true));
        System.out.println("Short names: " + partitioned.get(false));
    }
}
```

### Example 4: flatMap - Flattening Nested Structures

```java
import java.util.*;
import java.util.stream.*;

public class FlatMapOperations {
    public static void main(String[] args) {
        // 1. Flatten list of lists
        List<List<Integer>> listOfLists = Arrays.asList(
            Arrays.asList(1, 2, 3),
            Arrays.asList(4, 5, 6),
            Arrays.asList(7, 8, 9)
        );
        
        List<Integer> flattened = listOfLists.stream()
            .flatMap(list -> list.stream())
            .collect(Collectors.toList());
        System.out.println("Flattened: " + flattened);  // [1, 2, 3, 4, 5, 6, 7, 8, 9]
        
        // 2. Split strings into words
        List<String> sentences = Arrays.asList(
            "Java Stream API",
            "Functional Programming",
            "Lambda Expressions"
        );
        
        List<String> words = sentences.stream()
            .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
            .collect(Collectors.toList());
        System.out.println("Words: " + words);
        
        // 3. Get all characters from strings
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        
        List<Character> characters = names.stream()
            .flatMap(name -> name.chars()
                .mapToObj(c -> (char) c))
            .collect(Collectors.toList());
        System.out.println("Characters: " + characters);
        
        // Unique characters
        Set<Character> uniqueChars = names.stream()
            .flatMap(name -> name.chars()
                .mapToObj(c -> (char) c))
            .collect(Collectors.toSet());
        System.out.println("Unique characters: " + uniqueChars);
        
        // 4. Flatten array of arrays
        Integer[][] arrays = {
            {1, 2, 3},
            {4, 5},
            {6, 7, 8, 9}
        };
        
        List<Integer> flatArray = Arrays.stream(arrays)
            .flatMap(arr -> Arrays.stream(arr))
            .collect(Collectors.toList());
        System.out.println("Flat array: " + flatArray);
        
        // 5. Real-world example: Get all email addresses
        class Person {
            String name;
            List<String> emails;
            
            Person(String name, List<String> emails) {
                this.name = name;
                this.emails = emails;
            }
        }
        
        List<Person> people = Arrays.asList(
            new Person("Alice", Arrays.asList("alice@email.com", "alice@work.com")),
            new Person("Bob", Arrays.asList("bob@email.com")),
            new Person("Charlie", Arrays.asList("charlie@email.com", "charlie@work.com", "charlie@personal.com"))
        );
        
        List<String> allEmails = people.stream()
            .flatMap(person -> person.emails.stream())
            .collect(Collectors.toList());
        System.out.println("All emails: " + allEmails);
        
        // 6. flatMapToInt for primitive stream
        List<String> numberStrings = Arrays.asList("1,2,3", "4,5", "6,7,8");
        
        int sum = numberStrings.stream()
            .flatMapToInt(s -> Arrays.stream(s.split(","))
                .mapToInt(Integer::parseInt))
            .sum();
        System.out.println("Sum: " + sum);  // 36
    }
}
```

### Example 5: Matching and Finding Operations

```java
import java.util.*;
import java.util.stream.*;

public class MatchingAndFinding {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        // 1. anyMatch - check if any element matches
        boolean hasEven = numbers.stream()
            .anyMatch(n -> n % 2 == 0);
        System.out.println("Has even number: " + hasEven);  // true
        
        boolean hasNegative = numbers.stream()
            .anyMatch(n -> n < 0);
        System.out.println("Has negative: " + hasNegative);  // false
        
        // 2. allMatch - check if all elements match
        boolean allPositive = numbers.stream()
            .allMatch(n -> n > 0);
        System.out.println("All positive: " + allPositive);  // true
        
        boolean allEven = numbers.stream()
            .allMatch(n -> n % 2 == 0);
        System.out.println("All even: " + allEven);  // false
        
        // 3. noneMatch - check if no elements match
        boolean noneNegative = numbers.stream()
            .noneMatch(n -> n < 0);
        System.out.println("None negative: " + noneNegative);  // true
        
        boolean noneGreaterThan100 = numbers.stream()
            .noneMatch(n -> n > 100);
        System.out.println("None > 100: " + noneGreaterThan100);  // true
        
        // 4. findFirst - find first element
        Optional<Integer> first = numbers.stream()
            .filter(n -> n > 5)
            .findFirst();
        first.ifPresent(f -> System.out.println("First > 5: " + f));  // 6
        
        // findFirst on empty stream
        Optional<Integer> notFound = numbers.stream()
            .filter(n -> n > 100)
            .findFirst();
        System.out.println("First > 100 present: " + notFound.isPresent());  // false
        
        // 5. findAny - find any element (useful in parallel streams)
        Optional<Integer> any = numbers.stream()
            .filter(n -> n % 2 == 0)
            .findAny();
        any.ifPresent(a -> System.out.println("Any even: " + a));
        
        // 6. min and max
        Optional<Integer> min = numbers.stream()
            .min(Integer::compare);
        min.ifPresent(m -> System.out.println("Min: " + m));  // 1
        
        Optional<Integer> max = numbers.stream()
            .max(Integer::compare);
        max.ifPresent(m -> System.out.println("Max: " + m));  // 10
        
        // Custom comparator
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");
        Optional<String> longest = names.stream()
            .max(Comparator.comparing(String::length));
        longest.ifPresent(l -> System.out.println("Longest name: " + l));  // Charlie
        
        // 7. Short-circuit operations
        // These operations stop processing as soon as result is determined
        boolean result = IntStream.range(1, 1000000)
            .anyMatch(n -> n > 100);  // Stops after finding first match
        System.out.println("Found number > 100: " + result);
    }
}
```

### Example 6: Primitive Streams

```java
import java.util.*;
import java.util.stream.*;

public class PrimitiveStreams {
    public static void main(String[] args) {
        // 1. IntStream
        IntStream intStream = IntStream.of(1, 2, 3, 4, 5);
        int sum = intStream.sum();
        System.out.println("Sum: " + sum);  // 15
        
        // range() - exclusive end
        int sum2 = IntStream.range(1, 11)  // 1 to 10
            .sum();
        System.out.println("Sum 1-10: " + sum2);  // 55
        
        // rangeClosed() - inclusive end
        int sum3 = IntStream.rangeClosed(1, 10)  // 1 to 10 inclusive
            .sum();
        System.out.println("Sum 1-10 (closed): " + sum3);  // 55
        
        // 2. Statistics
        IntSummaryStatistics stats = IntStream.range(1, 101)
            .summaryStatistics();
        System.out.println("Count: " + stats.getCount());      // 100
        System.out.println("Sum: " + stats.getSum());          // 5050
        System.out.println("Min: " + stats.getMin());          // 1
        System.out.println("Max: " + stats.getMax());          // 100
        System.out.println("Average: " + stats.getAverage());  // 50.5
        
        // 3. mapToInt - convert Stream<T> to IntStream
        List<String> numbers = Arrays.asList("1", "2", "3", "4", "5");
        int total = numbers.stream()
            .mapToInt(Integer::parseInt)
            .sum();
        System.out.println("Total: " + total);  // 15
        
        // 4. LongStream
        long factorial = LongStream.rangeClosed(1, 10)
            .reduce(1, (a, b) -> a * b);
        System.out.println("10! = " + factorial);  // 3628800
        
        // 5. DoubleStream
        double average = DoubleStream.of(1.5, 2.5, 3.5, 4.5)
            .average()
            .orElse(0.0);
        System.out.println("Average: " + average);  // 3.0
        
        // 6. Boxing to object streams
        List<Integer> boxed = IntStream.range(1, 6)
            .boxed()
            .collect(Collectors.toList());
        System.out.println("Boxed: " + boxed);  // [1, 2, 3, 4, 5]
        
        // 7. Generate infinite stream
        IntStream.iterate(0, n -> n + 2)  // Even numbers
            .limit(10)
            .forEach(n -> System.out.print(n + " "));  // 0 2 4 6 8 10 12 14 16 18
        System.out.println();
        
        // Random numbers
        IntStream.generate(() -> (int) (Math.random() * 100))
            .limit(5)
            .forEach(n -> System.out.print(n + " "));
        System.out.println();
        
        // 8. Avoiding boxing overhead
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
        
        // With boxing
        int sum4 = list.stream()
            .filter(n -> n % 2 == 0)
            .mapToInt(n -> n)  // Unboxing
            .sum();
        
        // Better - use primitive stream from start
        int[] primitiveArray = {1, 2, 3, 4, 5};
        int sum5 = Arrays.stream(primitiveArray)
            .filter(n -> n % 2 == 0)
            .sum();
        System.out.println("Sum (primitive): " + sum5);
    }
}
```

### Example 7: Creating Streams

```java
import java.util.*;
import java.util.stream.*;
import java.nio.file.*;
import java.io.*;

public class CreatingStreams {
    public static void main(String[] args) {
        // 1. From collection
        List<String> list = Arrays.asList("A", "B", "C");
        Stream<String> streamFromList = list.stream();
        
        // 2. From array
        String[] array = {"X", "Y", "Z"};
        Stream<String> streamFromArray = Arrays.stream(array);
        
        // 3. From values
        Stream<String> streamOfValues = Stream.of("One", "Two", "Three");
        
        // 4. Empty stream
        Stream<String> emptyStream = Stream.empty();
        
        // 5. Stream.builder()
        Stream<String> streamBuilder = Stream.<String>builder()
            .add("A")
            .add("B")
            .add("C")
            .build();
        streamBuilder.forEach(System.out::println);
        
        // 6. Stream.generate() - infinite stream
        Stream<Double> randomNumbers = Stream.generate(Math::random)
            .limit(5);
        randomNumbers.forEach(System.out::println);
        
        // 7. Stream.iterate() - infinite stream with seed
        Stream<Integer> evenNumbers = Stream.iterate(0, n -> n + 2)
            .limit(10);
        evenNumbers.forEach(n -> System.out.print(n + " "));  // 0 2 4 6 ...
        System.out.println();
        
        // Fibonacci sequence
        Stream.iterate(new int[]{0, 1}, fib -> new int[]{fib[1], fib[0] + fib[1]})
            .limit(10)
            .map(fib -> fib[0])
            .forEach(n -> System.out.print(n + " "));  // 0 1 1 2 3 5 8 13 21 34
        System.out.println();
        
        // 8. IntStream, LongStream, DoubleStream
        IntStream intStream = IntStream.range(1, 11);
        LongStream longStream = LongStream.rangeClosed(1, 10);
        DoubleStream doubleStream = DoubleStream.of(1.1, 2.2, 3.3);
        
        // 9. From String
        IntStream chars = "Hello".chars();
        chars.forEach(c -> System.out.print((char) c + " "));  // H e l l o
        System.out.println();
        
        // 10. Parallel stream
        Stream<String> parallelStream = list.parallelStream();
        
        // 11. From file (requires exception handling)
        try {
            Stream<String> lines = Files.lines(Paths.get("file.txt"));
            // lines.forEach(System.out::println);
            lines.close();  // Important to close
        } catch (IOException e) {
            System.out.println("File not found");
        }
        
        // 12. Concatenate streams
        Stream<String> stream1 = Stream.of("A", "B");
        Stream<String> stream2 = Stream.of("C", "D");
        Stream<String> concatenated = Stream.concat(stream1, stream2);
        concatenated.forEach(System.out::print);  // ABCD
        System.out.println();
    }
}
```

### Example 8: Real-World Example - Employee Data Processing

```java
import java.util.*;
import java.util.stream.*;

class Employee {
    private String name;
    private String department;
    private double salary;
    private int age;
    
    public Employee(String name, String department, double salary, int age) {
        this.name = name;
        this.department = department;
        this.salary = salary;
        this.age = age;
    }
    
    public String getName() { return name; }
    public String getDepartment() { return department; }
    public double getSalary() { return salary; }
    public int getAge() { return age; }
    
    @Override
    public String toString() {
        return String.format("%s (%s, $%.0f, %d)", name, department, salary, age);
    }
}

public class EmployeeStreamProcessing {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee("Alice", "IT", 75000, 28),
            new Employee("Bob", "HR", 55000, 35),
            new Employee("Charlie", "IT", 85000, 32),
            new Employee("David", "Finance", 70000, 29),
            new Employee("Eve", "IT", 95000, 40),
            new Employee("Frank", "HR", 60000, 27),
            new Employee("Grace", "Finance", 80000, 31),
            new Employee("Henry", "IT", 72000, 26)
        );
        
        // 1. Get all IT department employees
        System.out.println("IT Employees:");
        employees.stream()
            .filter(e -> e.getDepartment().equals("IT"))
            .forEach(System.out::println);
        
        // 2. Calculate average salary by department
        Map<String, Double> avgSalaryByDept = employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.averagingDouble(Employee::getSalary)
            ));
        System.out.println("\nAverage salary by department: " + avgSalaryByDept);
        
        // 3. Find top 3 earners
        System.out.println("\nTop 3 earners:");
        employees.stream()
            .sorted(Comparator.comparing(Employee::getSalary).reversed())
            .limit(3)
            .forEach(System.out::println);
        
        // 4. Get names of employees earning more than 70k
        List<String> highEarners = employees.stream()
            .filter(e -> e.getSalary() > 70000)
            .map(Employee::getName)
            .collect(Collectors.toList());
        System.out.println("\nHigh earners (>70k): " + highEarners);
        
        // 5. Group employees by department
        Map<String, List<Employee>> byDepartment = employees.stream()
            .collect(Collectors.groupingBy(Employee::getDepartment));
        System.out.println("\nGrouped by department: " + byDepartment.keySet());
        
        // 6. Count employees per department
        Map<String, Long> countByDept = employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.counting()
            ));
        System.out.println("Count by department: " + countByDept);
        
        // 7. Total salary expense
        double totalSalary = employees.stream()
            .mapToDouble(Employee::getSalary)
            .sum();
        System.out.printf("\nTotal salary expense: $%.2f%n", totalSalary);
        
        // 8. Find oldest employee in IT
        Optional<Employee> oldestIT = employees.stream()
            .filter(e -> e.getDepartment().equals("IT"))
            .max(Comparator.comparingInt(Employee::getAge));
        oldestIT.ifPresent(e -> System.out.println("Oldest IT employee: " + e.getName() + " (" + e.getAge() + ")"));
        
        // 9. Check if all employees earn at least 50k
        boolean allEarnMinimum = employees.stream()
            .allMatch(e -> e.getSalary() >= 50000);
        System.out.println("All earn at least 50k: " + allEarnMinimum);
        
        // 10. Get department with highest average salary
        Optional<Map.Entry<String, Double>> highestAvgDept = avgSalaryByDept.entrySet().stream()
            .max(Map.Entry.comparingByValue());
        highestAvgDept.ifPresent(e -> 
            System.out.printf("Dept with highest avg salary: %s ($%.2f)%n", e.getKey(), e.getValue()));
        
        // 11. Partition employees by age (young/experienced)
        Map<Boolean, List<Employee>> partitioned = employees.stream()
            .collect(Collectors.partitioningBy(e -> e.getAge() < 30));
        System.out.println("Young employees (<30): " + partitioned.get(true).size());
        System.out.println("Experienced employees (>=30): " + partitioned.get(false).size());
        
        // 12. Get salary statistics
        DoubleSummaryStatistics salaryStats = employees.stream()
            .collect(Collectors.summarizingDouble(Employee::getSalary));
        System.out.println("\nSalary Statistics:");
        System.out.println("Count: " + salaryStats.getCount());
        System.out.println("Sum: $" + salaryStats.getSum());
        System.out.println("Min: $" + salaryStats.getMin());
        System.out.println("Max: $" + salaryStats.getMax());
        System.out.println("Average: $" + salaryStats.getAverage());
        
        // 13. Custom collector - join names by department
        Map<String, String> namesByDept = employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.mapping(
                    Employee::getName,
                    Collectors.joining(", ")
                )
            ));
        System.out.println("\nNames by department: " + namesByDept);
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is a Stream in Java?**
2. **What is the difference between intermediate and terminal operations?**
3. **Can you reuse a Stream?**
4. **What is lazy evaluation in streams?**
5. **What is the difference between map() and flatMap()?**
6. **What is the difference between findFirst() and findAny()?**
7. **How do parallel streams work?**
8. **What is the difference between Collection and Stream?**
9. **What is the difference between peek() and forEach()?**
10. **How do you convert a Stream to a Collection?**

---

## 6️⃣ Model Answers

**Q1: What is a Stream in Java?**

"A Stream is a sequence of elements supporting sequential and parallel aggregate operations. It's not a data structure—it doesn't store elements. Instead, it's a pipeline of computational operations on data from a source like collections, arrays, or I/O channels. Streams are functional in nature, meaning operations don't modify the source. They support lazy evaluation where intermediate operations are only executed when a terminal operation is invoked. Streams can only be consumed once. Java 8 introduced streams to enable functional-style operations on collections."

**Q2: What is the difference between intermediate and terminal operations?**

"Intermediate operations return a Stream and are lazy—they're not executed until a terminal operation is invoked. Examples include filter, map, sorted, distinct, limit. They can be chained together. Terminal operations return a non-Stream result like a collection, value, or void, and they trigger the execution of the stream pipeline. Examples include collect, forEach, reduce, count, anyMatch. Once a terminal operation is executed, the stream is closed and cannot be reused. A stream pipeline must have exactly one terminal operation to produce a result."

**Q3: Can you reuse a Stream?**

"No, streams can only be consumed once. After a terminal operation is executed, the stream is closed and attempting to use it again throws an IllegalStateException. If you need to perform multiple operations, you must create a new stream. For example, you can't do stream.forEach() and then stream.count() on the same stream. This is by design—streams are meant to be used as one-time pipelines. If you need to reuse the data, store it in a collection first or recreate the stream from the source."

**Q4: What is lazy evaluation in streams?**

"Lazy evaluation means intermediate operations are not executed immediately when called. Instead, they're stored up and only executed when a terminal operation is invoked. This allows for optimization—if you filter then limit, the stream can stop processing once the limit is reached, rather than filtering all elements first. For example, stream.filter().map().limit(5).collect() will stop after finding 5 matching elements, not process the entire stream. This makes streams very efficient, especially with large or infinite streams. Without a terminal operation, intermediate operations do nothing."

**Q5: What is the difference between map() and flatMap()?**

"Map transforms each element to another value—it's a one-to-one mapping. flatMap is used when each element maps to a stream of values, and you want to flatten the result into a single stream. For example, if you have List<List<Integer>>, map would give you Stream<Stream<Integer>>, but flatMap gives you Stream<Integer>. Another example: splitting strings into words. stream.map(s -> s.split(' ')) gives Stream<String[]>, but stream.flatMap(s -> Arrays.stream(s.split(' '))) gives Stream<String>. Use map for transforming values, flatMap for flattening nested structures."

**Q6: What is the difference between findFirst() and findAny()?**

"Both return an Optional with an element from the stream, but findFirst() returns the first element in encounter order, while findAny() returns any element. In sequential streams, they behave similarly, but in parallel streams, findAny() can be more performant because it doesn't need to maintain order. Use findFirst() when order matters, and findAny() when any element satisfying the condition is acceptable. Both are short-circuiting terminal operations, meaning they stop processing once a match is found."

**Q7: How do parallel streams work?**

"Parallel streams divide the data into multiple chunks and process them concurrently using the ForkJoinPool. You create a parallel stream with parallelStream() or by calling parallel() on a sequential stream. The stream framework handles all the threading complexity. However, parallel streams aren't always faster—they have overhead from thread management and data splitting. They're beneficial for large datasets with CPU-intensive operations, but can be slower for small datasets or I/O operations. Be careful with stateful operations and ensure thread safety when using parallel streams."

**Q8: What is the difference between Collection and Stream?**

"Collections are data structures that store elements in memory. Streams are pipelines for processing data and don't store elements. Collections are eagerly constructed—all elements are computed upfront. Streams use lazy evaluation—elements are computed on demand. Collections can be traversed multiple times. Streams can only be consumed once. Collections are primarily about data storage, while streams are about computation. You typically create a stream from a collection, process the data, and collect results back to a collection. Collections are external iteration using loops, streams are internal iteration using functional operations."

**Q9: What is the difference between peek() and forEach()?**

"peek() is an intermediate operation that performs an action on each element and returns a stream, allowing further operations. forEach() is a terminal operation that performs an action and returns void, ending the stream. peek() is mainly for debugging—you can observe elements flowing through the pipeline without affecting them. forEach() is for side effects when you don't need a result. For example, stream.peek(System.out::println).map().collect() lets you see intermediate values, while stream.forEach(System.out::println) is the final operation. Never use peek() for actual business logic—use map or other proper operations."

**Q10: How do you convert a Stream to a Collection?**

"Use the collect() terminal operation with Collectors. Common collectors include Collectors.toList() for ArrayList, Collectors.toSet() for HashSet, and Collectors.toCollection(TreeSet::new) for specific collection types. For maps, use Collectors.toMap(keyMapper, valueMapper). Examples: stream.collect(Collectors.toList()), stream.collect(Collectors.toSet()), stream.collect(Collectors.toMap(String::length, s -> s)). You can also convert to arrays with toArray(). Collectors provide many other options like groupingBy, partitioningBy, and joining for different use cases."

---

## 7️⃣ Common Mistakes

1. **Reusing streams**:
   ```java
   Stream<String> stream = list.stream();
   stream.forEach(System.out::println);
   stream.count();  // IllegalStateException!
   
   // Create new stream for each operation
   ```

2. **Forgetting terminal operation**:
   ```java
   list.stream()
       .filter(x -> x > 10)
       .map(x -> x * 2);  // Does nothing! No terminal operation
   
   // Add terminal operation:
   list.stream()
       .filter(x -> x > 10)
       .map(x -> x * 2)
       .collect(Collectors.toList());
   ```

3. **Modifying source during stream operations**:
   ```java
   List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3));
   list.stream()
       .forEach(n -> list.add(n));  // ConcurrentModificationException!
   ```

4. **Using peek() for business logic**:
   ```java
   // BAD: peek should only be for debugging
   stream.peek(e -> e.setValue(0))
         .collect(Collectors.toList());
   
   // GOOD: use map
   stream.map(e -> {
       e.setValue(0);
       return e;
   }).collect(Collectors.toList());
   ```

5. **Not handling Optional properly**:
   ```java
   String result = list.stream()
       .filter(s -> s.startsWith("A"))
       .findFirst()
       .get();  // May throw NoSuchElementException!
   
   // Better:
   String result = list.stream()
       .filter(s -> s.startsWith("A"))
       .findFirst()
       .orElse("Default");
   ```

6. **Unnecessary boxing in primitive streams**:
   ```java
   // Inefficient
   List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
   int sum = numbers.stream()
       .mapToInt(i -> i)
       .sum();
   
   // Better: use int[] and IntStream
   int[] numbers = {1, 2, 3, 4, 5};
   int sum = Arrays.stream(numbers).sum();
   ```

7. **Using parallel streams incorrectly**:
   ```java
   // BAD: shared mutable state
   List<Integer> result = new ArrayList<>();
   stream.parallel()
         .forEach(n -> result.add(n));  // Not thread-safe!
   
   // GOOD: use collect
   List<Integer> result = stream.parallel()
       .collect(Collectors.toList());
   ```

8. **Confusing map and flatMap**:
   ```java
   List<List<Integer>> lists = ...;
   
   // Returns Stream<List<Integer>> - wrong!
   Stream<List<Integer>> mapped = lists.stream()
       .map(list -> list);
   
   // Returns Stream<Integer> - correct!
   Stream<Integer> flattened = lists.stream()
       .flatMap(list -> list.stream());
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Stream is a pipeline, not a data structure
- Intermediate operations are lazy, terminal operations trigger execution
- Streams can only be consumed once
- filter selects elements, map transforms elements
- collect() is the most common way to get results
- Lazy evaluation enables optimization

**What to avoid**:
- Don't claim streams are "always faster" (they have overhead)
- Don't say streams store data (they don't)
- Don't forget that parallel streams need thread-safe operations
- Don't confuse map and flatMap

**Under pressure**:
- Draw pipeline: Source → filter/map → collect
- Give simple example: `list.stream().filter(x -> x > 5).collect(Collectors.toList())`
- Mention that streams enable functional programming
- State key collectors: toList, toSet, toMap, groupingBy

**Red flags to avoid**:
- "Streams are collections" (they're not)
- "You can reuse streams" (you can't)
- "Parallel streams are always faster" (not always)
- "Intermediate operations execute immediately" (they're lazy)

**Bonus points**:
- Mention short-circuit operations (limit, findFirst, anyMatch)
- Discuss parallel streams and ForkJoinPool
- Reference method references with stream operations
- Mention primitive streams (IntStream, LongStream, DoubleStream)
- Discuss stateless vs stateful operations
- Reference collectors like groupingBy, partitioningBy
- Mention infinite streams with generate and iterate
