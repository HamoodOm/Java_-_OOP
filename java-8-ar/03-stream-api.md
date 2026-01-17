<div dir="rtl">

# واجهة Stream

## 1️⃣ نظرة عامة على المفهوم

**Stream** هو تسلسل من العناصر يدعم العمليات التجميعية المتتابعة والمتوازية. تم تقديمه في Java 8، وتتيح واجهة Stream إجراء عمليات بأسلوب البرمجة الوظيفية على مجموعات العناصر.

**الخصائص الرئيسية**:
- **ليس بنية بيانات**: لا يخزن Stream العناصر
- **وظيفي بطبيعته**: العمليات تنتج نتائج دون تعديل المصدر
- **التقييم الكسول**: لا يتم تنفيذ العمليات الوسيطة حتى يتم استدعاء العملية النهائية
- **قد يكون غير محدود**: يمكن العمل مع streams لا نهائية
- **قابل للاستهلاك مرة واحدة**: يمكن المرور عليه مرة واحدة فقط

**خط أنابيب Stream**:
```
Source → Intermediate Operations → Terminal Operation
         (filter, map, sorted)    (collect, forEach, reduce)
```

**الأجزاء الثلاثة لـ Stream**:
1. **المصدر**: Collection أو array أو قناة I/O أو دالة مولدة
2. **العمليات الوسيطة**: تحول Stream (كسولة)
3. **العملية النهائية**: تنتج نتيجة أو تأثير جانبي (تبدأ التنفيذ)

**العمليات الوسيطة مقابل النهائية**:

| الوسيطة | النهائية |
|---------|----------|
| تعيد Stream | تعيد نتيجة غير Stream |
| كسولة (لا تنفذ فوراً) | نشطة (تبدأ تنفيذ خط الأنابيب) |
| يمكن تسلسلها | تنهي خط الأنابيب |
| أمثلة: filter, map, sorted | أمثلة: collect, forEach, reduce |

**أنواع Streams**:
- `Stream<T>`: stream للكائنات
- `IntStream`, `LongStream`, `DoubleStream`: streams للأنواع البدائية
- Sequential streams (افتراضي)
- Parallel streams (للمعالجة المتوازية)

---

## 2️⃣ لماذا يسأل المحاورون عن هذا الموضوع

- **Java الحديثة**: واجهة Stream أساسية في Java 8 وما بعدها
- **البرمجة الوظيفية**: يختبر فهم النمط الوظيفي
- **الأداء**: فهم التقييم الكسول و parallel streams
- **جودة الكود**: Streams تؤدي إلى كود أكثر قابلية للقراءة وتصريحي
- **العمليات الشائعة**: تستخدم يومياً في كود الإنتاج
- **موضوع مفضل في المقابلات**: يُسأل عنه بشكل متكرر جداً

---

## 3️⃣ القواعد والحقائق الأساسية

**دورة حياة Stream**:
1. إنشاء stream من المصدر
2. تطبيق صفر أو أكثر من العمليات الوسيطة
3. تطبيق العملية النهائية (مطلوبة لتنفيذ خط الأنابيب)
4. يُغلق Stream ولا يمكن إعادة استخدامه

**العمليات الوسيطة** (كسولة، تعيد Stream):

| العملية | الوصف | مثال |
|---------|-------|------|
| filter(Predicate) | تصفية العناصر | `filter(x -> x > 10)` |
| map(Function) | تحويل العناصر | `map(x -> x * 2)` |
| flatMap(Function) | تسطيح الهياكل المتداخلة | `flatMap(list -> list.stream())` |
| distinct() | إزالة المكررات | `distinct()` |
| sorted() | ترتيب العناصر | `sorted()` أو `sorted(Comparator)` |
| peek(Consumer) | تنفيذ إجراء دون تعديل | `peek(System.out::println)` |
| limit(long) | تحديد حجم Stream | `limit(10)` |
| skip(long) | تخطي أول n عنصر | `skip(5)` |

**العمليات النهائية** (نشطة، تبدأ التنفيذ):

| العملية | نوع الإرجاع | الوصف |
|---------|-------------|-------|
| forEach(Consumer) | void | تنفيذ إجراء على كل عنصر |
| collect(Collector) | Collection/Map | جمع العناصر في collection |
| reduce(BinaryOperator) | Optional<T> | اختزال إلى قيمة واحدة |
| count() | long | عد العناصر |
| anyMatch(Predicate) | boolean | اختبار إذا تطابق أي عنصر |
| allMatch(Predicate) | boolean | اختبار إذا تطابقت كل العناصر |
| noneMatch(Predicate) | boolean | اختبار إذا لم يتطابق أي عنصر |
| findFirst() | Optional<T> | إرجاع أول عنصر |
| findAny() | Optional<T> | إرجاع أي عنصر |
| min(Comparator) | Optional<T> | إرجاع الحد الأدنى |
| max(Comparator) | Optional<T> | إرجاع الحد الأقصى |
| toArray() | Object[] | التحويل إلى مصفوفة |

**Collectors** (الدوال الثابتة الشائعة من فئة Collectors):

| Collector | الغرض | مثال |
|-----------|-------|------|
| toList() | جمع إلى List | `collect(Collectors.toList())` |
| toSet() | جمع إلى Set | `collect(Collectors.toSet())` |
| toMap() | جمع إلى Map | `collect(Collectors.toMap(k, v))` |
| joining() | دمج النصوص | `collect(Collectors.joining(", "))` |
| counting() | عد العناصر | `collect(Collectors.counting())` |
| summingInt() | جمع الأعداد الصحيحة | `collect(Collectors.summingInt(mapper))` |
| averagingDouble() | المتوسط | `collect(Collectors.averagingDouble(mapper))` |
| groupingBy() | التجميع حسب مفتاح | `collect(Collectors.groupingBy(classifier))` |
| partitioningBy() | التقسيم حسب شرط | `collect(Collectors.partitioningBy(predicate))` |

**قواعد مهمة**:
- Stream يمكن استهلاكه مرة واحدة فقط
- العمليات الوسيطة كسولة (لا تنفذ حتى العملية النهائية)
- العملية النهائية تغلق Stream
- Streams ليست بنى بيانات (لا تخزن العناصر)
- العمليات يجب أن تكون عديمة الحالة وغير متداخلة
- Parallel streams قد لا تحافظ على الترتيب

**إنشاء Streams**:
- من collections: `list.stream()`
- من المصفوفات: `Arrays.stream(array)`
- من القيم: `Stream.of(1, 2, 3)`
- Stream فارغ: `Stream.empty()`
- Streams لا نهائية: `Stream.generate()`, `Stream.iterate()`
- Primitive streams: `IntStream.range()`, `IntStream.of()`

---

## 4️⃣ أمثلة الكود (Java 8)

### المثال 1: عمليات Stream الأساسية

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

### المثال 2: عمليات reduce

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

### المثال 3: Collectors - جمع النتائج

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

### المثال 4: flatMap - تسطيح الهياكل المتداخلة

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

### المثال 5: عمليات المطابقة والبحث

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

### المثال 6: Primitive Streams

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

### المثال 7: إنشاء Streams

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

### المثال 8: مثال واقعي - معالجة بيانات الموظفين

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

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما هو Stream في Java؟**
2. **ما الفرق بين العمليات الوسيطة والنهائية؟**
3. **هل يمكن إعادة استخدام Stream؟**
4. **ما هو التقييم الكسول في streams؟**
5. **ما الفرق بين map() و flatMap()؟**
6. **ما الفرق بين findFirst() و findAny()؟**
7. **كيف تعمل parallel streams؟**
8. **ما الفرق بين Collection و Stream؟**
9. **ما الفرق بين peek() و forEach()؟**
10. **كيف تحول Stream إلى Collection؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما هو Stream في Java؟**

"Stream هو تسلسل من العناصر يدعم العمليات التجميعية المتتابعة والمتوازية. إنه ليس بنية بيانات - فهو لا يخزن العناصر. بدلاً من ذلك، هو خط أنابيب من العمليات الحسابية على البيانات من مصدر مثل collections أو المصفوفات أو قنوات I/O. Streams وظيفية بطبيعتها، مما يعني أن العمليات لا تعدل المصدر. تدعم التقييم الكسول حيث تنفذ العمليات الوسيطة فقط عند استدعاء العملية النهائية. يمكن استهلاك Streams مرة واحدة فقط. قدمت Java 8 streams لتمكين العمليات بأسلوب وظيفي على المجموعات."

**س2: ما الفرق بين العمليات الوسيطة والنهائية؟**

"العمليات الوسيطة تعيد Stream وهي كسولة - لا تنفذ حتى يتم استدعاء عملية نهائية. أمثلة عليها تشمل filter, map, sorted, distinct, limit. يمكن تسلسلها معاً. العمليات النهائية تعيد نتيجة غير Stream مثل collection أو قيمة أو void، وتبدأ تنفيذ خط أنابيب stream. أمثلة عليها تشمل collect, forEach, reduce, count, anyMatch. بمجرد تنفيذ العملية النهائية، يُغلق stream ولا يمكن إعادة استخدامه. يجب أن يحتوي خط أنابيب stream على عملية نهائية واحدة بالضبط لإنتاج نتيجة."

**س3: هل يمكن إعادة استخدام Stream؟**

"لا، يمكن استهلاك streams مرة واحدة فقط. بعد تنفيذ عملية نهائية، يُغلق stream ومحاولة استخدامه مرة أخرى تطرح IllegalStateException. إذا كنت بحاجة لتنفيذ عمليات متعددة، يجب إنشاء stream جديد. على سبيل المثال، لا يمكنك تنفيذ stream.forEach() ثم stream.count() على نفس stream. هذا بالتصميم - streams مصممة للاستخدام كخطوط أنابيب مرة واحدة. إذا كنت بحاجة لإعادة استخدام البيانات، خزنها في collection أولاً أو أعد إنشاء stream من المصدر."

**س4: ما هو التقييم الكسول في streams؟**

"التقييم الكسول يعني أن العمليات الوسيطة لا تنفذ فوراً عند استدعائها. بدلاً من ذلك، يتم تخزينها وتنفذ فقط عند استدعاء عملية نهائية. هذا يسمح بالتحسين - إذا قمت بالتصفية ثم التحديد، يمكن لـ stream إيقاف المعالجة بمجرد الوصول للحد، بدلاً من تصفية كل العناصر أولاً. على سبيل المثال، stream.filter().map().limit(5).collect() ستتوقف بعد إيجاد 5 عناصر مطابقة، وليس معالجة stream بأكمله. هذا يجعل streams فعالة جداً، خاصة مع streams كبيرة أو لا نهائية. بدون عملية نهائية، لا تفعل العمليات الوسيطة شيئاً."

**س5: ما الفرق بين map() و flatMap()؟**

"map تحول كل عنصر إلى قيمة أخرى - إنها تعيين واحد لواحد. flatMap تُستخدم عندما يُعين كل عنصر إلى stream من القيم، وتريد تسطيح النتيجة إلى stream واحد. على سبيل المثال، إذا كان لديك List<List<Integer>>، فإن map ستعطيك Stream<Stream<Integer>>، لكن flatMap تعطيك Stream<Integer>. مثال آخر: تقسيم النصوص إلى كلمات. stream.map(s -> s.split(' ')) تعطي Stream<String[]>، لكن stream.flatMap(s -> Arrays.stream(s.split(' '))) تعطي Stream<String>. استخدم map لتحويل القيم، flatMap لتسطيح الهياكل المتداخلة."

**س6: ما الفرق بين findFirst() و findAny()؟**

"كلاهما يعيد Optional بعنصر من stream، لكن findFirst() تعيد أول عنصر بترتيب المواجهة، بينما findAny() تعيد أي عنصر. في sequential streams، تتصرفان بشكل مماثل، لكن في parallel streams، يمكن أن تكون findAny() أكثر كفاءة لأنها لا تحتاج للحفاظ على الترتيب. استخدم findFirst() عندما يهم الترتيب، و findAny() عندما يكون أي عنصر يحقق الشرط مقبولاً. كلاهما عمليات نهائية قصيرة الدائرة، مما يعني أنها تتوقف عن المعالجة بمجرد إيجاد تطابق."

**س7: كيف تعمل parallel streams؟**

"Parallel streams تقسم البيانات إلى أجزاء متعددة وتعالجها بشكل متزامن باستخدام ForkJoinPool. تنشئ parallel stream بـ parallelStream() أو باستدعاء parallel() على sequential stream. يتعامل إطار عمل stream مع كل تعقيدات threading. ومع ذلك، parallel streams ليست دائماً أسرع - لها overhead من إدارة threads وتقسيم البيانات. تفيد لمجموعات البيانات الكبيرة مع عمليات كثيفة المعالج، لكن يمكن أن تكون أبطأ لمجموعات البيانات الصغيرة أو عمليات I/O. كن حذراً مع العمليات ذات الحالة وتأكد من thread safety عند استخدام parallel streams."

**س8: ما الفرق بين Collection و Stream؟**

"Collections هي بنى بيانات تخزن العناصر في الذاكرة. Streams هي خطوط أنابيب لمعالجة البيانات ولا تخزن العناصر. Collections تُبنى بشكل نشط - كل العناصر تُحسب مقدماً. Streams تستخدم التقييم الكسول - العناصر تُحسب حسب الطلب. Collections يمكن المرور عليها عدة مرات. Streams يمكن استهلاكها مرة واحدة فقط. Collections أساساً عن تخزين البيانات، بينما streams عن الحساب. عادة تنشئ stream من collection، تعالج البيانات، وتجمع النتائج مرة أخرى إلى collection. Collections هي تكرار خارجي باستخدام loops، streams هي تكرار داخلي باستخدام عمليات وظيفية."

**س9: ما الفرق بين peek() و forEach()؟**

"peek() عملية وسيطة تنفذ إجراءً على كل عنصر وتعيد stream، مما يسمح بعمليات أخرى. forEach() عملية نهائية تنفذ إجراءً وتعيد void، منهية stream. peek() أساساً للتصحيح - يمكنك مراقبة العناصر المتدفقة عبر خط الأنابيب دون التأثير عليها. forEach() للتأثيرات الجانبية عندما لا تحتاج نتيجة. على سبيل المثال، stream.peek(System.out::println).map().collect() تتيح رؤية القيم الوسيطة، بينما stream.forEach(System.out::println) هي العملية النهائية. لا تستخدم peek() أبداً لمنطق الأعمال الفعلي - استخدم map أو عمليات مناسبة أخرى."

**س10: كيف تحول Stream إلى Collection؟**

"استخدم العملية النهائية collect() مع Collectors. Collectors الشائعة تشمل Collectors.toList() لـ ArrayList، Collectors.toSet() لـ HashSet، و Collectors.toCollection(TreeSet::new) لأنواع collection محددة. للـ maps، استخدم Collectors.toMap(keyMapper, valueMapper). أمثلة: stream.collect(Collectors.toList())، stream.collect(Collectors.toSet())، stream.collect(Collectors.toMap(String::length, s -> s)). يمكنك أيضاً التحويل إلى مصفوفات بـ toArray(). توفر Collectors خيارات أخرى كثيرة مثل groupingBy، partitioningBy، و joining لحالات استخدام مختلفة."

---

## 7️⃣ الأخطاء الشائعة

1. **إعادة استخدام streams**:
   ```java
   Stream<String> stream = list.stream();
   stream.forEach(System.out::println);
   stream.count();  // IllegalStateException!

   // Create new stream for each operation
   ```

2. **نسيان العملية النهائية**:
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

3. **تعديل المصدر أثناء عمليات stream**:
   ```java
   List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3));
   list.stream()
       .forEach(n -> list.add(n));  // ConcurrentModificationException!
   ```

4. **استخدام peek() لمنطق الأعمال**:
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

5. **عدم التعامل مع Optional بشكل صحيح**:
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

6. **boxing غير ضروري في primitive streams**:
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

7. **استخدام parallel streams بشكل خاطئ**:
   ```java
   // BAD: shared mutable state
   List<Integer> result = new ArrayList<>();
   stream.parallel()
         .forEach(n -> result.add(n));  // Not thread-safe!

   // GOOD: use collect
   List<Integer> result = stream.parallel()
       .collect(Collectors.toList());
   ```

8. **الخلط بين map و flatMap**:
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

## 8️⃣ نصائح المقابلات الفعلية

**ما يجب التأكيد عليه**:
- Stream هو خط أنابيب، وليس بنية بيانات
- العمليات الوسيطة كسولة، العمليات النهائية تبدأ التنفيذ
- Streams يمكن استهلاكها مرة واحدة فقط
- filter تختار العناصر، map تحول العناصر
- collect() هي الطريقة الأكثر شيوعاً للحصول على النتائج
- التقييم الكسول يمكّن من التحسين

**ما يجب تجنبه**:
- لا تدعي أن streams "دائماً أسرع" (لها overhead)
- لا تقل أن streams تخزن البيانات (لا تفعل)
- لا تنسَ أن parallel streams تحتاج عمليات thread-safe
- لا تخلط بين map و flatMap

**تحت الضغط**:
- ارسم خط الأنابيب: Source → filter/map → collect
- أعطِ مثالاً بسيطاً: `list.stream().filter(x -> x > 5).collect(Collectors.toList())`
- اذكر أن streams تمكّن البرمجة الوظيفية
- اذكر collectors الرئيسية: toList, toSet, toMap, groupingBy

**علامات حمراء يجب تجنبها**:
- "Streams هي collections" (ليست كذلك)
- "يمكنك إعادة استخدام streams" (لا يمكنك)
- "Parallel streams دائماً أسرع" (ليس دائماً)
- "العمليات الوسيطة تنفذ فوراً" (إنها كسولة)

**نقاط إضافية**:
- اذكر عمليات قصيرة الدائرة (limit, findFirst, anyMatch)
- ناقش parallel streams و ForkJoinPool
- أشر إلى method references مع عمليات stream
- اذكر primitive streams (IntStream, LongStream, DoubleStream)
- ناقش العمليات عديمة الحالة مقابل ذات الحالة
- أشر إلى collectors مثل groupingBy, partitioningBy
- اذكر streams اللانهائية مع generate و iterate

</div>
