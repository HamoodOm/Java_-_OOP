<div dir="rtl">

# الواجهات الوظيفية

## 1. نظرة عامة على المفهوم

**الواجهة الوظيفية** هي واجهة تحتوي على طريقة مجردة واحدة فقط. تُعرف أيضاً باسم **واجهة SAM (Single Abstract Method)**، وتعمل كنوع مستهدف لتعبيرات Lambda ومراجع الطرق.

**الخصائص الرئيسية**:
- **طريقة مجردة واحدة فقط** (يمكن أن تحتوي على عدة طرق default/static)
- يمكن وضع التعليق التوضيحي `@FunctionalInterface` عليها (اختياري لكن موصى به)
- يمكن أن تحتوي على طرق default (Java 8+)
- يمكن أن تحتوي على طرق static
- يمكن أن ترث طرقاً من صنف Object (equals، hashCode، toString)
- تعمل كأساس لتعبيرات Lambda

**التعليق التوضيحي @FunctionalInterface**:
- فحص وقت الترجمة يضمن أن الواجهة تحتوي على طريقة مجردة واحدة فقط
- اختياري لكن موصى به للوضوح
- يمنع الإضافة العرضية لطريقة مجردة ثانية
- يوثق النية بأن الواجهة مصممة لتعبيرات Lambda

**تصنيفات الواجهات الوظيفية**:

1. **الواجهات الوظيفية القياسية** (java.util.function):
   - الأساسية: Predicate، Function، Consumer، Supplier
   - المتغيرات الثنائية: BiPredicate، BiFunction، BiConsumer
   - المشغلات: UnaryOperator، BinaryOperator
   - المتغيرات البدائية: IntPredicate، LongFunction، إلخ.

2. **الواجهات الوظيفية القديمة** (قبل Java 8):
   - Runnable، Callable، Comparator
   - ActionListener، ItemListener
   - FileFilter، FilenameFilter

3. **الواجهات الوظيفية المخصصة**:
   - واجهات خاصة بالمجال تقوم بإنشائها

---

## 2. لماذا يسأل المحاورون عن هذا

- **أساس Java 8**: الواجهات الوظيفية تمكّن من تعبيرات Lambda
- **تصميم API**: يختبر فهم مبادئ تصميم الواجهات
- **الواجهات المدمجة**: يُظهر المعرفة بمكتبة Java القياسية
- **اعتماد Stream API**: الواجهات الوظيفية تشغّل عمليات Stream
- **الاستخدام في الواقع**: تُستخدم على نطاق واسع في قواعد الكود الحديثة لـ Java
- **أنماط التصميم**: الواجهات الوظيفية تمكّن من أنماط التصميم الوظيفية

---

## 3. القواعد والحقائق الأساسية

**قواعد الواجهة الوظيفية**:
- يجب أن تحتوي على **طريقة مجردة واحدة فقط**
- يمكن أن تحتوي على أي عدد من طرق default
- يمكن أن تحتوي على أي عدد من الطرق static
- يمكن تجاوز طرق صنف Object دون كسر القاعدة
- إذا ورثت واجهة أخرى، يجب أن يكون مجموع الطرق المجردة واحداً
- يمكن أن تكون عامة مع معاملات نوع

**التعليق التوضيحي @FunctionalInterface**:
- غير إلزامي لكن موصى به بشدة
- خطأ ترجمة إذا كانت الواجهة تحتوي على != 1 طريقة مجردة
- يحسن توثيق الكود والنية
- يساعد في منع التغييرات الكسرية العرضية

**الواجهات الوظيفية المدمجة** (java.util.function):

**الأربعة الأساسية**:

| الواجهة | توقيع الطريقة | الغرض | مثال |
|---------|---------------|-------|------|
| Predicate<T> | boolean test(T t) | تختبر شرطاً | `x -> x > 10` |
| Function<T,R> | R apply(T t) | تحول المدخل | `x -> x * 2` |
| Consumer<T> | void accept(T t) | تستهلك المدخل | `x -> System.out.println(x)` |
| Supplier<T> | T get() | توفر قيمة | `() -> new ArrayList<>()` |

**المتغيرات الثنائية** (معاملان):

| الواجهة | توقيع الطريقة | الغرض |
|---------|---------------|-------|
| BiPredicate<T,U> | boolean test(T t, U u) | تختبر مدخلين |
| BiFunction<T,U,R> | R apply(T t, U u) | تحول مدخلين |
| BiConsumer<T,U> | void accept(T t, U u) | تستهلك مدخلين |

**المشغلات** (نفس نوع المدخل/المخرج):

| الواجهة | توقيع الطريقة | الغرض |
|---------|---------------|-------|
| UnaryOperator<T> | T apply(T t) | مدخل واحد، نفس نوع المخرج |
| BinaryOperator<T> | T apply(T t, T u) | مدخلان، نفس نوع المخرج |

**التخصصات البدائية** (تتجنب التغليف):

| النوع | الواجهات |
|-------|----------|
| Int | IntPredicate، IntFunction، IntConsumer، IntSupplier، IntUnaryOperator، IntBinaryOperator |
| Long | LongPredicate، LongFunction، LongConsumer، LongSupplier، LongUnaryOperator، LongBinaryOperator |
| Double | DoublePredicate، DoubleFunction، DoubleConsumer، DoubleSupplier، DoubleUnaryOperator، DoubleBinaryOperator |
| ToInt | ToIntFunction، ToIntBiFunction |
| ToLong | ToLongFunction، ToLongBiFunction |
| ToDouble | ToDoubleFunction، ToDoubleBiFunction |

**تركيب الواجهات الوظيفية**:
- Predicate: `and()`، `or()`، `negate()`
- Function: `andThen()`، `compose()`
- Consumer: `andThen()`

---

## 4. أمثلة برمجية (Java 8)

### المثال 1: التعليق التوضيحي @FunctionalInterface

```java
// Valid functional interface
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);

    // Default methods allowed
    default int add(int a, int b) {
        return a + b;
    }

    // Static methods allowed
    static int multiply(int a, int b) {
        return a * b;
    }

    // Object class methods allowed
    @Override
    boolean equals(Object obj);
}

// Invalid - compilation error with @FunctionalInterface
/*
@FunctionalInterface
interface Invalid {
    void method1();
    void method2();  // Second abstract method - error!
}
*/

// Valid - only one abstract method (inherited)
@FunctionalInterface
interface ExtendedCalculator extends Calculator {
    // Inherits calculate() - still only one abstract method

    // Can add default methods
    default int subtract(int a, int b) {
        return a - b;
    }
}

public class FunctionalInterfaceDemo {
    public static void main(String[] args) {
        // Using lambda with custom functional interface
        Calculator add = (a, b) -> a + b;
        Calculator multiply = (a, b) -> a * b;

        System.out.println("5 + 3 = " + add.calculate(5, 3));        // 8
        System.out.println("5 * 3 = " + multiply.calculate(5, 3));   // 15

        // Using default method
        System.out.println("5 + 3 (default) = " + add.add(5, 3));    // 8

        // Using static method
        System.out.println("5 * 3 (static) = " + Calculator.multiply(5, 3));  // 15
    }
}
```

### المثال 2: واجهة Predicate

```java
import java.util.*;
import java.util.function.Predicate;

public class PredicateDemo {
    public static void main(String[] args) {
        // Basic Predicate
        Predicate<Integer> isEven = n -> n % 2 == 0;
        System.out.println("10 is even: " + isEven.test(10));  // true
        System.out.println("7 is even: " + isEven.test(7));    // false

        Predicate<String> isEmpty = s -> s.isEmpty();
        System.out.println("'' is empty: " + isEmpty.test(""));        // true
        System.out.println("'Hello' is empty: " + isEmpty.test("Hello")); // false

        // Predicate composition
        Predicate<Integer> isPositive = n -> n > 0;
        Predicate<Integer> isEvenAndPositive = isEven.and(isPositive);

        System.out.println("10 is even and positive: " + isEvenAndPositive.test(10));  // true
        System.out.println("-4 is even and positive: " + isEvenAndPositive.test(-4));  // false

        // or()
        Predicate<Integer> isLessThan5 = n -> n < 5;
        Predicate<Integer> isGreaterThan10 = n -> n > 10;
        Predicate<Integer> isOutOfRange = isLessThan5.or(isGreaterThan10);

        System.out.println("3 is out of range: " + isOutOfRange.test(3));   // true
        System.out.println("7 is out of range: " + isOutOfRange.test(7));   // false
        System.out.println("15 is out of range: " + isOutOfRange.test(15)); // true

        // negate()
        Predicate<Integer> isOdd = isEven.negate();
        System.out.println("7 is odd: " + isOdd.test(7));  // true

        // Static method: isEqual()
        Predicate<String> isHello = Predicate.isEqual("Hello");
        System.out.println("'Hello' equals 'Hello': " + isHello.test("Hello")); // true
        System.out.println("'World' equals 'Hello': " + isHello.test("World")); // false

        // Using Predicate with collections
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // Filter even numbers
        List<Integer> evenNumbers = filter(numbers, isEven);
        System.out.println("Even numbers: " + evenNumbers);  // [2, 4, 6, 8, 10]

        // Filter positive odd numbers
        List<Integer> positiveOdd = filter(numbers, isPositive.and(isOdd));
        System.out.println("Positive odd: " + positiveOdd);  // [1, 3, 5, 7, 9]

        // String predicates
        List<String> words = Arrays.asList("", "Apple", "Banana", "", "Cherry");
        Predicate<String> isNotEmpty = s -> !s.isEmpty();
        List<String> nonEmpty = filter(words, isNotEmpty);
        System.out.println("Non-empty words: " + nonEmpty);  // [Apple, Banana, Cherry]
    }

    // Helper method using Predicate
    public static <T> List<T> filter(List<T> list, Predicate<T> predicate) {
        List<T> result = new ArrayList<>();
        for (T item : list) {
            if (predicate.test(item)) {
                result.add(item);
            }
        }
        return result;
    }
}
```

### المثال 3: واجهة Function

```java
import java.util.*;
import java.util.function.Function;

public class FunctionDemo {
    public static void main(String[] args) {
        // Basic Function
        Function<String, Integer> stringLength = s -> s.length();
        System.out.println("Length of 'Hello': " + stringLength.apply("Hello"));  // 5

        Function<Integer, Integer> square = n -> n * n;
        System.out.println("Square of 5: " + square.apply(5));  // 25

        Function<String, String> toUpper = s -> s.toUpperCase();
        System.out.println("Upper: " + toUpper.apply("hello"));  // HELLO

        // Function composition: andThen()
        Function<Integer, Integer> doubleIt = n -> n * 2;
        Function<Integer, Integer> addTen = n -> n + 10;
        Function<Integer, Integer> doubleThenAdd = doubleIt.andThen(addTen);

        System.out.println("(5 * 2) + 10 = " + doubleThenAdd.apply(5));  // 20

        // Function composition: compose()
        Function<Integer, Integer> addThenDouble = doubleIt.compose(addTen);
        System.out.println("(5 + 10) * 2 = " + addThenDouble.apply(5));  // 30

        // Difference between andThen and compose:
        // andThen: f.andThen(g) means g(f(x))
        // compose: f.compose(g) means f(g(x))

        // Static method: identity()
        Function<String, String> identity = Function.identity();
        System.out.println("Identity: " + identity.apply("Hello"));  // Hello

        // Chaining multiple functions
        Function<String, String> process = ((Function<String, String>) String::toLowerCase)
            .andThen(s -> s.replace('a', '*'))
            .andThen(s -> s.toUpperCase());

        System.out.println("Process 'Banana': " + process.apply("Banana"));  // B*N*N*

        // Using Function with collections
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        List<Integer> lengths = map(names, String::length);
        System.out.println("Name lengths: " + lengths);  // [5, 3, 7]

        List<String> upperNames = map(names, String::toUpperCase);
        System.out.println("Upper names: " + upperNames);  // [ALICE, BOB, CHARLIE]

        // Multiple transformations
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        Function<Integer, Integer> transform = n -> n * 2;
        transform = transform.andThen(n -> n + 10);
        transform = transform.andThen(n -> n * n);

        List<Integer> transformed = map(numbers, transform);
        System.out.println("Transformed: " + transformed);  // [(2+10)^2, (4+10)^2, ...]
    }

    // Helper method using Function
    public static <T, R> List<R> map(List<T> list, Function<T, R> function) {
        List<R> result = new ArrayList<>();
        for (T item : list) {
            result.add(function.apply(item));
        }
        return result;
    }
}
```

### المثال 4: واجهة Consumer

```java
import java.util.*;
import java.util.function.Consumer;

public class ConsumerDemo {
    public static void main(String[] args) {
        // Basic Consumer
        Consumer<String> printer = s -> System.out.println(s);
        printer.accept("Hello, World!");  // Hello, World!

        Consumer<Integer> squarePrinter = n -> System.out.println(n * n);
        squarePrinter.accept(5);  // 25

        // Consumer with side effects
        List<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob"));
        Consumer<String> addToList = name -> names.add(name.toUpperCase());

        addToList.accept("Charlie");
        System.out.println("Names: " + names);  // [Alice, Bob, CHARLIE]

        // Consumer composition: andThen()
        Consumer<String> printLower = s -> System.out.println(s.toLowerCase());
        Consumer<String> printUpper = s -> System.out.println(s.toUpperCase());
        Consumer<String> printBoth = printLower.andThen(printUpper);

        printBoth.accept("Hello");
        // Output:
        // hello
        // HELLO

        // Using Consumer with forEach
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        System.out.println("Numbers:");
        numbers.forEach(n -> System.out.println(n));

        System.out.println("Squares:");
        numbers.forEach(n -> System.out.println(n * n));

        // Chaining consumers
        Consumer<Integer> printNumber = n -> System.out.print(n + " ");
        Consumer<Integer> printSquare = n -> System.out.print(n * n + " ");
        Consumer<Integer> combined = printNumber.andThen(printSquare);

        System.out.println("\nNumber and square:");
        numbers.forEach(combined);  // 1 1 2 4 3 9 4 16 5 25
        System.out.println();

        // Map operations
        Map<String, Integer> map = new HashMap<>();
        map.put("Apple", 5);
        map.put("Banana", 3);
        map.put("Cherry", 7);

        // BiConsumer for map forEach
        map.forEach((k, v) -> System.out.println(k + " = " + v));

        // Custom forEach implementation
        List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry");
        forEach(fruits, s -> System.out.println("Fruit: " + s));
    }

    // Helper method using Consumer
    public static <T> void forEach(List<T> list, Consumer<T> consumer) {
        for (T item : list) {
            consumer.accept(item);
        }
    }
}
```

### المثال 5: واجهة Supplier

```java
import java.util.*;
import java.util.function.Supplier;

public class SupplierDemo {
    public static void main(String[] args) {
        // Basic Supplier
        Supplier<String> helloSupplier = () -> "Hello, World!";
        System.out.println(helloSupplier.get());  // Hello, World!

        Supplier<Double> randomSupplier = () -> Math.random();
        System.out.println("Random: " + randomSupplier.get());

        Supplier<Integer> diceRoll = () -> (int) (Math.random() * 6) + 1;
        System.out.println("Dice: " + diceRoll.get());

        // Supplier for object creation
        Supplier<List<String>> listSupplier = () -> new ArrayList<>();
        List<String> list1 = listSupplier.get();
        List<String> list2 = listSupplier.get();

        list1.add("A");
        System.out.println("List1: " + list1);  // [A]
        System.out.println("List2: " + list2);  // [] (different instances)

        Supplier<StringBuilder> sbSupplier = StringBuilder::new;
        StringBuilder sb = sbSupplier.get();
        sb.append("Hello");
        System.out.println(sb);  // Hello

        // Lazy evaluation with Supplier
        System.out.println("Getting expensive value...");
        String result = getValueOrDefault(null, () -> {
            System.out.println("Computing expensive default...");
            return "Expensive Default";
        });
        System.out.println("Result: " + result);

        // When value is not null, supplier is not called
        result = getValueOrDefault("Quick Value", () -> {
            System.out.println("This won't be printed");
            return "Expensive Default";
        });
        System.out.println("Result: " + result);

        // Factory pattern with Supplier
        Supplier<Person> personFactory = () -> new Person("John", 30);
        Person p1 = personFactory.get();
        Person p2 = personFactory.get();

        System.out.println("Person 1: " + p1);
        System.out.println("Person 2: " + p2);

        // Generating values
        List<Integer> randomNumbers = generate(randomSupplier, 5);
        System.out.println("Random numbers: " + randomNumbers);
    }

    // Lazy evaluation helper
    public static <T> T getValueOrDefault(T value, Supplier<T> defaultSupplier) {
        return value != null ? value : defaultSupplier.get();
    }

    // Generate values using Supplier
    public static <T> List<T> generate(Supplier<T> supplier, int count) {
        List<T> result = new ArrayList<>();
        for (int i = 0; i < count; i++) {
            result.add(supplier.get());
        }
        return result;
    }
}

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}
```

### المثال 6: BiFunction و BiPredicate و BiConsumer

```java
import java.util.*;
import java.util.function.*;

public class BiFunctionDemo {
    public static void main(String[] args) {
        // BiFunction<T, U, R>
        BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
        BiFunction<Integer, Integer, Integer> multiply = (a, b) -> a * b;
        BiFunction<String, String, String> concat = (s1, s2) -> s1 + s2;

        System.out.println("5 + 3 = " + add.apply(5, 3));          // 8
        System.out.println("5 * 3 = " + multiply.apply(5, 3));     // 15
        System.out.println(concat.apply("Hello", "World"));         // HelloWorld

        // BiFunction with andThen
        BiFunction<Integer, Integer, Integer> addThenSquare =
            add.andThen(n -> n * n);
        System.out.println("(5 + 3)^2 = " + addThenSquare.apply(5, 3));  // 64

        // BiPredicate<T, U>
        BiPredicate<Integer, Integer> isGreater = (a, b) -> a > b;
        BiPredicate<String, Integer> isLongerThan = (s, len) -> s.length() > len;

        System.out.println("10 > 5: " + isGreater.test(10, 5));    // true
        System.out.println("'Hello' longer than 3: " + isLongerThan.test("Hello", 3));  // true

        // BiPredicate composition
        BiPredicate<Integer, Integer> isLess = (a, b) -> a < b;
        BiPredicate<Integer, Integer> isNotEqual = isGreater.or(isLess);

        System.out.println("5 != 5: " + isNotEqual.test(5, 5));    // false
        System.out.println("5 != 10: " + isNotEqual.test(5, 10));  // true

        // BiConsumer<T, U>
        BiConsumer<String, Integer> printInfo = (name, age) ->
            System.out.println(name + " is " + age + " years old");

        printInfo.accept("Alice", 25);  // Alice is 25 years old

        // BiConsumer with Map
        Map<String, Integer> map = new HashMap<>();
        map.put("Apple", 5);
        map.put("Banana", 3);

        BiConsumer<String, Integer> mapPrinter = (k, v) ->
            System.out.println(k + ": " + v);

        map.forEach(mapPrinter);

        // BiConsumer chaining
        BiConsumer<String, Integer> printer1 = (s, n) -> System.out.println(s);
        BiConsumer<String, Integer> printer2 = (s, n) -> System.out.println(n);
        BiConsumer<String, Integer> both = printer1.andThen(printer2);

        both.accept("Number", 42);
        // Output:
        // Number
        // 42
    }
}
```

### المثال 7: UnaryOperator و BinaryOperator

```java
import java.util.*;
import java.util.function.*;

public class OperatorDemo {
    public static void main(String[] args) {
        // UnaryOperator<T> extends Function<T, T>
        UnaryOperator<Integer> square = n -> n * n;
        UnaryOperator<Integer> negate = n -> -n;
        UnaryOperator<String> toUpper = s -> s.toUpperCase();

        System.out.println("Square of 5: " + square.apply(5));       // 25
        System.out.println("Negate 5: " + negate.apply(5));          // -5
        System.out.println("Upper 'hello': " + toUpper.apply("hello"));  // HELLO

        // UnaryOperator composition
        UnaryOperator<Integer> squareThenNegate = square.andThen(negate);
        System.out.println("Square then negate 5: " + squareThenNegate.apply(5));  // -25

        // BinaryOperator<T> extends BiFunction<T, T, T>
        BinaryOperator<Integer> add = (a, b) -> a + b;
        BinaryOperator<Integer> multiply = (a, b) -> a * b;
        BinaryOperator<Integer> max = (a, b) -> a > b ? a : b;
        BinaryOperator<String> concat = (s1, s2) -> s1 + s2;

        System.out.println("10 + 20 = " + add.apply(10, 20));        // 30
        System.out.println("10 * 20 = " + multiply.apply(10, 20));   // 200
        System.out.println("Max(10, 20) = " + max.apply(10, 20));    // 20
        System.out.println(concat.apply("Hello", "World"));           // HelloWorld

        // BinaryOperator static methods
        BinaryOperator<Integer> min = BinaryOperator.minBy(Integer::compare);
        BinaryOperator<Integer> maxBy = BinaryOperator.maxBy(Integer::compare);

        System.out.println("Min(10, 20) = " + min.apply(10, 20));    // 10
        System.out.println("Max(10, 20) = " + maxBy.apply(10, 20));  // 20

        // Using with Stream reduce
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        int sum = numbers.stream().reduce(0, add);
        System.out.println("Sum: " + sum);  // 15

        int product = numbers.stream().reduce(1, multiply);
        System.out.println("Product: " + product);  // 120

        Optional<Integer> maximum = numbers.stream().reduce(max);
        maximum.ifPresent(m -> System.out.println("Maximum: " + m));  // 5

        // Transform list with UnaryOperator
        List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
        nums.replaceAll(square);
        System.out.println("Squared: " + nums);  // [1, 4, 9, 16, 25]
    }
}
```

### المثال 8: الواجهات الوظيفية البدائية

```java
import java.util.function.*;

public class PrimitiveFunctionInterfaces {
    public static void main(String[] args) {
        // IntPredicate - avoids autoboxing
        IntPredicate isEven = n -> n % 2 == 0;
        System.out.println("10 is even: " + isEven.test(10));  // true

        // IntFunction<R> - takes int, returns R
        IntFunction<String> intToString = n -> "Number: " + n;
        System.out.println(intToString.apply(42));  // Number: 42

        // ToIntFunction<T> - takes T, returns int
        ToIntFunction<String> stringLength = s -> s.length();
        System.out.println("Length of 'Hello': " + stringLength.applyAsInt("Hello"));  // 5

        // IntConsumer
        IntConsumer printer = n -> System.out.println("Value: " + n);
        printer.accept(100);  // Value: 100

        // IntSupplier
        IntSupplier randomInt = () -> (int) (Math.random() * 100);
        System.out.println("Random int: " + randomInt.getAsInt());

        // IntUnaryOperator
        IntUnaryOperator square = n -> n * n;
        System.out.println("Square of 5: " + square.applyAsInt(5));  // 25

        // IntBinaryOperator
        IntBinaryOperator add = (a, b) -> a + b;
        System.out.println("10 + 20 = " + add.applyAsInt(10, 20));  // 30

        // LongPredicate
        LongPredicate isPositive = n -> n > 0;
        System.out.println("100L is positive: " + isPositive.test(100L));  // true

        // DoublePredicate
        DoublePredicate isGreaterThanPi = d -> d > Math.PI;
        System.out.println("4.0 > PI: " + isGreaterThanPi.test(4.0));  // true

        // ObjIntConsumer - takes object and int
        ObjIntConsumer<String> printWithIndex = (s, i) ->
            System.out.println(i + ": " + s);
        printWithIndex.accept("Hello", 0);  // 0: Hello

        // Performance comparison
        long startTime, endTime;

        // With boxing (Predicate<Integer>)
        Predicate<Integer> evenBoxed = n -> n % 2 == 0;
        startTime = System.nanoTime();
        for (int i = 0; i < 1000000; i++) {
            evenBoxed.test(i);  // Boxing overhead
        }
        endTime = System.nanoTime();
        System.out.println("With boxing: " + (endTime - startTime) + "ns");

        // Without boxing (IntPredicate)
        IntPredicate evenPrimitive = n -> n % 2 == 0;
        startTime = System.nanoTime();
        for (int i = 0; i < 1000000; i++) {
            evenPrimitive.test(i);  // No boxing
        }
        endTime = System.nanoTime();
        System.out.println("Without boxing: " + (endTime - startTime) + "ns");
    }
}
```

### المثال 9: واجهات وظيفية مخصصة

```java
@FunctionalInterface
interface TriFunction<T, U, V, R> {
    R apply(T t, U u, V v);

    default <W> TriFunction<T, U, V, W> andThen(Function<R, W> after) {
        return (t, u, v) -> after.apply(apply(t, u, v));
    }
}

@FunctionalInterface
interface StringValidator {
    boolean validate(String input);

    default StringValidator and(StringValidator other) {
        return input -> this.validate(input) && other.validate(input);
    }

    default StringValidator or(StringValidator other) {
        return input -> this.validate(input) || other.validate(input);
    }
}

@FunctionalInterface
interface Transformer<T> {
    T transform(T input);

    default Transformer<T> andThen(Transformer<T> after) {
        return input -> after.transform(this.transform(input));
    }
}

public class CustomFunctionalInterfaces {
    public static void main(String[] args) {
        // TriFunction example
        TriFunction<Integer, Integer, Integer, Integer> sumOfThree =
            (a, b, c) -> a + b + c;

        System.out.println("Sum of 1, 2, 3: " + sumOfThree.apply(1, 2, 3));  // 6

        TriFunction<Integer, Integer, Integer, String> sumToString =
            sumOfThree.andThen(n -> "Result: " + n);

        System.out.println(sumToString.apply(5, 10, 15));  // Result: 30

        // StringValidator example
        StringValidator notEmpty = s -> !s.isEmpty();
        StringValidator minLength = s -> s.length() >= 3;
        StringValidator hasDigit = s -> s.chars().anyMatch(Character::isDigit);

        StringValidator basicValidator = notEmpty.and(minLength);
        StringValidator fullValidator = basicValidator.and(hasDigit);

        System.out.println("'ab' valid (basic): " + basicValidator.validate("ab"));      // false
        System.out.println("'abc' valid (basic): " + basicValidator.validate("abc"));    // true
        System.out.println("'abc' valid (full): " + fullValidator.validate("abc"));      // false
        System.out.println("'abc1' valid (full): " + fullValidator.validate("abc1"));    // true

        // Transformer example
        Transformer<String> toUpper = s -> s.toUpperCase();
        Transformer<String> addPrefix = s -> "PREFIX_" + s;
        Transformer<String> addSuffix = s -> s + "_SUFFIX";

        Transformer<String> fullTransform = toUpper
            .andThen(addPrefix)
            .andThen(addSuffix);

        System.out.println(fullTransform.transform("hello"));
        // PREFIX_HELLO_SUFFIX
    }
}
```

### المثال 10: مثال واقعي - خط معالجة البيانات

```java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;

class DataRecord {
    private String id;
    private double value;
    private String category;

    public DataRecord(String id, double value, String category) {
        this.id = id;
        this.value = value;
        this.category = category;
    }

    public String getId() { return id; }
    public double getValue() { return value; }
    public String getCategory() { return category; }

    @Override
    public String toString() {
        return String.format("%s: %.2f (%s)", id, value, category);
    }
}

public class DataProcessingPipeline {
    public static void main(String[] args) {
        List<DataRecord> records = Arrays.asList(
            new DataRecord("R001", 100.5, "A"),
            new DataRecord("R002", 250.0, "B"),
            new DataRecord("R003", 75.3, "A"),
            new DataRecord("R004", 310.7, "C"),
            new DataRecord("R005", 180.2, "B"),
            new DataRecord("R006", 50.0, "A")
        );

        // 1. Predicate - Filter records
        Predicate<DataRecord> isHighValue = r -> r.getValue() > 100;
        Predicate<DataRecord> isCategoryA = r -> r.getCategory().equals("A");
        Predicate<DataRecord> isHighValueCategoryA = isHighValue.and(isCategoryA);

        System.out.println("High value Category A records:");
        records.stream()
            .filter(isHighValueCategoryA)
            .forEach(System.out::println);

        // 2. Function - Transform records
        Function<DataRecord, String> toSummary = r ->
            r.getId() + " (" + r.getCategory() + ")";

        List<String> summaries = records.stream()
            .map(toSummary)
            .collect(Collectors.toList());
        System.out.println("\nSummaries: " + summaries);

        // 3. Function - Calculate discounted value
        Function<Double, Double> applyDiscount = v -> v * 0.9;
        Function<DataRecord, DataRecord> discountRecord = r ->
            new DataRecord(r.getId(), applyDiscount.apply(r.getValue()), r.getCategory());

        System.out.println("\nDiscounted records:");
        records.stream()
            .map(discountRecord)
            .forEach(System.out::println);

        // 4. Consumer - Process records
        Consumer<DataRecord> printHighValue = r -> {
            if (r.getValue() > 200) {
                System.out.println("HIGH VALUE: " + r);
            }
        };

        Consumer<DataRecord> logRecord = r -> {
            // In real app, would log to file/database
            System.out.println("Logged: " + r.getId());
        };

        Consumer<DataRecord> processRecord = printHighValue.andThen(logRecord);

        System.out.println("\nProcessing records:");
        records.forEach(processRecord);

        // 5. Supplier - Generate default record
        Supplier<DataRecord> defaultRecord = () ->
            new DataRecord("DEFAULT", 0.0, "NONE");

        DataRecord fallback = defaultRecord.get();
        System.out.println("\nDefault record: " + fallback);

        // 6. BiFunction - Combine records
        BiFunction<DataRecord, DataRecord, DataRecord> combineRecords = (r1, r2) ->
            new DataRecord(
                r1.getId() + "+" + r2.getId(),
                r1.getValue() + r2.getValue(),
                r1.getCategory()
            );

        DataRecord combined = combineRecords.apply(records.get(0), records.get(1));
        System.out.println("\nCombined record: " + combined);

        // 7. BiPredicate - Compare records
        BiPredicate<DataRecord, DataRecord> isSameCategory = (r1, r2) ->
            r1.getCategory().equals(r2.getCategory());

        System.out.println("\nR001 and R003 same category: " +
            isSameCategory.test(records.get(0), records.get(2)));  // true

        // 8. UnaryOperator - Normalize values
        UnaryOperator<Double> normalize = v -> v / 100.0;

        System.out.println("\nNormalized values:");
        records.stream()
            .map(DataRecord::getValue)
            .map(normalize)
            .forEach(v -> System.out.printf("%.2f ", v));
        System.out.println();

        // 9. BinaryOperator - Aggregate values
        BinaryOperator<Double> sum = (a, b) -> a + b;
        BinaryOperator<Double> max = BinaryOperator.maxBy(Double::compare);

        double totalValue = records.stream()
            .map(DataRecord::getValue)
            .reduce(0.0, sum);
        System.out.println("\nTotal value: " + totalValue);

        Optional<Double> maxValue = records.stream()
            .map(DataRecord::getValue)
            .reduce(max);
        maxValue.ifPresent(m -> System.out.println("Max value: " + m));

        // 10. Complex pipeline
        Map<String, Double> categoryTotals = records.stream()
            .filter(isHighValue)  // Predicate
            .collect(Collectors.groupingBy(
                DataRecord::getCategory,  // Function
                Collectors.summingDouble(DataRecord::getValue)  // ToDoubleFunction
            ));

        System.out.println("\nCategory totals (high value only): " + categoryTotals);
    }
}
```

---

## 5. أسئلة المقابلات الشائعة

1. **ما هي الواجهة الوظيفية؟**
2. **ما الغرض من التعليق التوضيحي @FunctionalInterface؟**
3. **هل يمكن للواجهة الوظيفية أن تحتوي على طرق default؟**
4. **ما هي الواجهات الوظيفية المدمجة الرئيسية في Java 8؟**
5. **ما الفرق بين Predicate و Function؟**
6. **ما الفرق بين Consumer و Supplier؟**
7. **هل يمكن للواجهة الوظيفية أن ترث واجهة وظيفية أخرى؟**
8. **لماذا لدينا واجهات وظيفية بدائية مثل IntPredicate؟**
9. **كيف تقوم بتركيب الواجهات الوظيفية؟**
10. **ما الفرق بين Function.andThen() و Function.compose()؟**

---

## 6. إجابات نموذجية

**س1: ما هي الواجهة الوظيفية؟**

"الواجهة الوظيفية هي واجهة بطريقة مجردة واحدة فقط. تُسمى أيضاً واجهة SAM - Single Abstract Method. يمكن أن تحتوي على عدة طرق default أو static، لكن طريقة مجردة واحدة فقط. الواجهات الوظيفية تعمل كأنواع مستهدفة لتعبيرات Lambda ومراجع الطرق. يمكنك اختيارياً وضع التعليق التوضيحي @FunctionalInterface للتحقق وقت الترجمة. الأمثلة تشمل Runnable مع run()، وPredicate مع test()، وFunction مع apply(). هي أساس البرمجة الوظيفية في Java 8."

**س2: ما الغرض من التعليق التوضيحي @FunctionalInterface؟**

"يخدم التعليق التوضيحي @FunctionalInterface غرضين. أولاً، هو فحص وقت الترجمة يضمن أن الواجهة تحتوي على طريقة مجردة واحدة فقط - إذا أضفت عن طريق الخطأ طريقة مجردة ثانية، تحصل على خطأ ترجمة. ثانياً، يوثق النية بأن هذه الواجهة مصممة لتعبيرات Lambda، مما يجعل الكود أكثر قابلية للصيانة. رغم أنه اختياري، فهو موصى به بشدة لأنه يمنع التغييرات الكسرية ويوصل نية التصميم بوضوح للمطورين الآخرين. إنه مشابه لـ @Override في أنه يلتقط الأخطاء وقت الترجمة."

**س3: هل يمكن للواجهة الوظيفية أن تحتوي على طرق default؟**

"نعم، الواجهات الوظيفية يمكن أن تحتوي على أي عدد من طرق default. المتطلب الرئيسي هو طريقة مجردة واحدة فقط. طرق default لها تنفيذات، لذا لا تُحسب ضمن قاعدة الطريقة المجردة الواحدة. يمكنها أيضاً أن تحتوي على طرق static وطرق من صنف Object مثل equals() وhashCode(). مثلاً، Predicate لديها الطريقة المجردة test() لكن أيضاً لديها طرق default مثل and() وor() وnegate() للتركيب. هذا يسمح للواجهات الوظيفية بتوفير خدمات إضافية مع الحفاظ على عقد الطريقة المجردة الواحدة لتعبيرات Lambda."

**س4: ما هي الواجهات الوظيفية المدمجة الرئيسية في Java 8؟**

"Java 8 توفر أربع واجهات وظيفية أساسية في حزمة java.util.function. Predicate<T> تأخذ مدخلاً وترجع boolean، تُستخدم لاختبار الشروط. Function<T,R> تأخذ مدخلاً من نوع T وترجع مخرجاً من نوع R، تُستخدم للتحويلات. Consumer<T> تأخذ مدخلاً ولا ترجع شيئاً، تُستخدم للعمليات ذات الآثار الجانبية مثل الطباعة. Supplier<T> لا تأخذ مدخلاً وترجع قيمة، تُستخدم لتوفير أو توليد القيم. هناك أيضاً متغيرات ثنائية مثل BiPredicate وBiFunction، ومشغلات مثل UnaryOperator وBinaryOperator، وتخصصات بدائية مثل IntPredicate لتجنب عبء التغليف."

**س5: ما الفرق بين Predicate و Function؟**

"Predicate وFunction تخدمان أغراضاً مختلفة. Predicate<T> تأخذ مدخلاً وترجع boolean - تُستخدم لاختبار الشروط أو التصفية. طريقتها المجردة هي boolean test(T t). Function<T,R> تأخذ مدخلاً من نوع T وترجع قيمة من نوع R - تُستخدم للتحويلات أو التعيين. طريقتها المجردة هي R apply(T t). لذا Predicate هي في الأساس Function<T, Boolean> متخصصة، لكنها مختلفة دلالياً. استخدم Predicate عندما تتحقق من شرط، وFunction عندما تحول البيانات. مثلاً، stream.filter() تستخدم Predicate، بينما stream.map() تستخدم Function."

**س6: ما الفرق بين Consumer و Supplier؟**

"Consumer وSupplier متضادتان. Consumer<T> تأخذ مدخلاً ولا ترجع شيئاً - طريقتها المجردة هي void accept(T t). تُستخدم للعمليات ذات الآثار الجانبية مثل الطباعة والتسجيل أو تعديل المجموعات. Supplier<T> لا تأخذ مدخلاً وترجع قيمة - طريقتها المجردة هي T get(). تُستخدم لتوفير أو توليد القيم، مثل طرق المصنع أو التقييم الكسول. Consumer تستهلك البيانات، Supplier توفر البيانات. مثلاً، list.forEach() تستخدم Consumer لمعالجة كل عنصر، بينما Optional.orElseGet() تستخدم Supplier لتوفير قيمة افتراضية فقط إذا لزم الأمر."

**س7: هل يمكن للواجهة الوظيفية أن ترث واجهة وظيفية أخرى؟**

"نعم، يمكن للواجهة الوظيفية أن ترث واجهة وظيفية أخرى، لكن العدد الإجمالي للطرق المجردة يجب أن يظل واحداً. إذا كان للأب طريقة مجردة واحدة والابن لم يضف طرقاً مجردة جديدة، فهي لا تزال واجهة وظيفية صالحة. مثلاً، UnaryOperator<T> ترث Function<T,T> لكنها لا تضيف طرقاً مجردة جديدة، لذا هي لا تزال وظيفية. ومع ذلك، إذا أضاف الابن طريقة مجردة جديدة، لم تعد واجهة وظيفية. المفتاح هو أنه بعد الوراثة، يجب أن تكون هناك طريقة مجردة واحدة فقط للتنفيذ."

**س8: لماذا لدينا واجهات وظيفية بدائية مثل IntPredicate؟**

"الواجهات الوظيفية البدائية موجودة لتجنب عبء أداء التغليف وفك التغليف التلقائي. عندما تستخدم Predicate<Integer>، كل قيمة int يتم تغليفها إلى Integer، مما يُنشئ عبء الكائنات. IntPredicate تعمل مباشرة مع قيم int البدائية، متجنبة هذا العبء. هذا مهم في الكود الحساس للأداء، خاصة مع مجموعات البيانات الكبيرة أو الحلقات الضيقة. Java توفر نسخاً بدائية لـ int وlong وdouble - الأنواع الرقمية الأكثر استخداماً. مثلاً، IntStream تستخدم IntPredicate للتصفية لتجنب تغليف ملايين الأعداد الصحيحة. فرق الأداء يمكن أن يكون كبيراً في العمليات عالية الحجم."

**س9: كيف تقوم بتركيب الواجهات الوظيفية؟**

"معظم الواجهات الوظيفية توفر طرق تركيب. Predicate لديها and() وor() وnegate() لدمج المسندات. Function لديها andThen() وcompose() لسلسلة التحويلات. Consumer لديها andThen() لتسلسل العمليات. مثلاً، يمكنك إنشاء Predicate<String> isLongAndUpper = isLong.and(isUpperCase) للتحقق من كلا الشرطين. مع Function، يمكنك سلسلة العمليات: Function<String, Integer> getLength = String::length; Function<Integer, Integer> square = x -> x * x; Function<String, Integer> composed = getLength.andThen(square). هذا يمكّن من بناء منطق معقد من مكونات بسيطة قابلة لإعادة الاستخدام."

**س10: ما الفرق بين Function.andThen() و Function.compose()؟**

"كلاهما تركب الدوال لكن بترتيب مختلف. andThen() تطبق الدالة الحالية أولاً، ثم الدالة التالية. compose() تطبق دالة المعامل أولاً، ثم الدالة الحالية. إذا كان لديك f.andThen(g)، فهذا يعني g(f(x)). إذا كان لديك f.compose(g)، فهذا يعني f(g(x)). مثلاً، إذا كانت f تضاعف رقماً وg تضيف 10: f.andThen(g) على 5 تعطي (5*2)+10=20. f.compose(g) على 5 تعطي (5+10)*2=30. فكر في andThen كـ 'افعل هذا، ثم ذاك' وcompose كـ 'افعل ذاك أولاً، ثم هذا'. معظم الناس يجدون andThen أكثر بديهية."

---

## 7. الأخطاء الشائعة

1. **إضافة عدة طرق مجردة إلى @FunctionalInterface**:
   ```java
   @FunctionalInterface
   interface Invalid {
       void method1();
       void method2();  // Compilation error!
   }
   ```

2. **نسيان أن الواجهة الوظيفية يمكن أن تحتوي على طرق default**:
   ```java
   @FunctionalInterface
   interface MyInterface {
       void doSomething();

       default void helper() {  // This is fine!
           System.out.println("Helper");
       }
   }
   ```

3. **عدم استخدام الواجهات الوظيفية البدائية عند الملائمة**:
   ```java
   // Inefficient - boxing overhead
   Predicate<Integer> isEven = n -> n % 2 == 0;

   // Better - no boxing
   IntPredicate isEven = n -> n % 2 == 0;
   ```

4. **الخلط بين andThen و compose**:
   ```java
   Function<Integer, Integer> f = x -> x * 2;
   Function<Integer, Integer> g = x -> x + 10;

   f.andThen(g).apply(5);  // (5*2)+10 = 20
   f.compose(g).apply(5);  // (5+10)*2 = 30
   ```

5. **عدم معالجة null في الواجهات الوظيفية**:
   ```java
   Predicate<String> startsWithA = s -> s.startsWith("A");
   startsWithA.test(null);  // NullPointerException!

   // Better:
   Predicate<String> startsWithA = s -> s != null && s.startsWith("A");
   ```

6. **استخدام واجهة وظيفية خاطئة**:
   ```java
   // Wrong - Consumer returns void
   List<String> names = list.stream()
       .map(s -> System.out.println(s))  // Returns void!
       .collect(Collectors.toList());

   // Correct - use Function or peek
   ```

7. **عدم فهم نوع نتيجة BiFunction**:
   ```java
   BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
   // Returns Integer, not void
   ```

8. **نسيان أن Supplier لا تأخذ معاملات**:
   ```java
   // Supplier<Integer> random = (max) -> ...; // Error!

   // Correct:
   Supplier<Integer> random = () -> (int)(Math.random() * 100);
   ```

---

## 8. نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- الواجهة الوظيفية = طريقة مجردة واحدة فقط
- @FunctionalInterface اختياري لكن موصى به
- الأربعة الأساسية: Predicate (test)، Function (transform)، Consumer (consume)، Supplier (supply)
- يمكن أن تحتوي على طرق default و static
- النسخ البدائية تتجنب عبء التغليف
- طرق التركيب: and/or/negate، andThen/compose

**ما يجب تجنبه**:
- لا تقل أن الواجهات الوظيفية لا يمكن أن تحتوي على طرق default
- لا تدّعِ أن @FunctionalInterface إلزامي
- لا تنسَ التخصصات البدائية
- لا تخلط بين UnaryOperator و Function (نفس نوع المدخل/المخرج)

**تحت الضغط**:
- تذكر SAM: Single Abstract Method
- اذكر الأربعة الأساسية مع طرقها
- اشرح بمثال بسيط: `Predicate<Integer> isEven = n -> n % 2 == 0`
- اذكر أن تعبيرات Lambda تستهدف الواجهات الوظيفية

**علامات تحذيرية يجب تجنبها**:
- "الواجهات الوظيفية يمكن أن تحتوي على طريقة واحدة فقط" (يمكن أن تحتوي على default/static)
- "يجب استخدام @FunctionalInterface" (إنه اختياري)
- "BiFunction تأخذ ثلاثة معاملات" (تأخذ اثنين، ترجع واحداً)
- "Consumer ترجع قيمة" (هي void)

**نقاط إضافية**:
- اذكر أن Runnable وCallable وComparator هي واجهات وظيفية
- ناقش مراجع الطرق كاختصار لـ Lambda
- أشر إلى طرق التركيب لبناء منطق معقد
- اذكر أن الواجهات الوظيفية تمكّن من البرمجة التصريحية
- ناقش متى تُنشئ واجهات وظيفية مخصصة مقابل استخدام المدمجة
- أشر إلى أن الواجهات الوظيفية تُستخدم في جميع أنحاء Stream API

</div>
