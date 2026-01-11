# Functional Interfaces

## 1️⃣ Concept Overview

**Functional Interface** is an interface that contains exactly one abstract method. Also known as **SAM (Single Abstract Method) interface**, it serves as the target type for lambda expressions and method references.

**Key Characteristics**:
- Exactly **one abstract method** (can have multiple default/static methods)
- Can be annotated with `@FunctionalInterface` (optional but recommended)
- Can contain default methods (Java 8+)
- Can contain static methods
- Can inherit methods from Object class (equals, hashCode, toString)
- Serves as the foundation for lambda expressions

**@FunctionalInterface Annotation**:
- Compile-time check ensuring interface has exactly one abstract method
- Optional but recommended for clarity
- Prevents accidental addition of second abstract method
- Documents intent that interface is designed for lambda expressions

**Categories of Functional Interfaces**:

1. **Standard Functional Interfaces** (java.util.function):
   - Core: Predicate, Function, Consumer, Supplier
   - Bi-variants: BiPredicate, BiFunction, BiConsumer
   - Operators: UnaryOperator, BinaryOperator
   - Primitive variants: IntPredicate, LongFunction, etc.

2. **Legacy Functional Interfaces** (pre-Java 8):
   - Runnable, Callable, Comparator
   - ActionListener, ItemListener
   - FileFilter, FilenameFilter

3. **Custom Functional Interfaces**:
   - Domain-specific interfaces you create

---

## 2️⃣ Why Interviewers Ask This

- **Java 8 foundation**: Functional interfaces enable lambda expressions
- **API design**: Tests understanding of interface design principles
- **Built-in interfaces**: Shows knowledge of Java standard library
- **Stream API dependency**: Functional interfaces power Stream operations
- **Real-world usage**: Widely used in modern Java codebases
- **Design patterns**: Functional interfaces enable functional design patterns

---

## 3️⃣ Key Rules / Facts

**Functional Interface Rules**:
- Must have **exactly one** abstract method
- Can have any number of default methods
- Can have any number of static methods
- Can override Object class methods without breaking the rule
- If it extends another interface, combined abstract methods must be one
- Can be generic with type parameters

**@FunctionalInterface Annotation**:
- Not mandatory but highly recommended
- Compiler error if interface has != 1 abstract method
- Improves code documentation and intent
- Helps prevent accidental breaking changes

**Built-in Functional Interfaces** (java.util.function):

**Core Four**:

| Interface | Method Signature | Purpose | Example |
|-----------|-----------------|---------|---------|
| Predicate<T> | boolean test(T t) | Tests condition | `x -> x > 10` |
| Function<T,R> | R apply(T t) | Transforms input | `x -> x * 2` |
| Consumer<T> | void accept(T t) | Consumes input | `x -> System.out.println(x)` |
| Supplier<T> | T get() | Supplies value | `() -> new ArrayList<>()` |

**Bi-Variants** (Two parameters):

| Interface | Method Signature | Purpose |
|-----------|-----------------|---------|
| BiPredicate<T,U> | boolean test(T t, U u) | Tests two inputs |
| BiFunction<T,U,R> | R apply(T t, U u) | Transforms two inputs |
| BiConsumer<T,U> | void accept(T t, U u) | Consumes two inputs |

**Operators** (Same type input/output):

| Interface | Method Signature | Purpose |
|-----------|-----------------|---------|
| UnaryOperator<T> | T apply(T t) | Single input, same type output |
| BinaryOperator<T> | T apply(T t, T u) | Two inputs, same type output |

**Primitive Specializations** (Avoid boxing):

| Type | Interfaces |
|------|------------|
| Int | IntPredicate, IntFunction, IntConsumer, IntSupplier, IntUnaryOperator, IntBinaryOperator |
| Long | LongPredicate, LongFunction, LongConsumer, LongSupplier, LongUnaryOperator, LongBinaryOperator |
| Double | DoublePredicate, DoubleFunction, DoubleConsumer, DoubleSupplier, DoubleUnaryOperator, DoubleBinaryOperator |
| ToInt | ToIntFunction, ToIntBiFunction |
| ToLong | ToLongFunction, ToLongBiFunction |
| ToDouble | ToDoubleFunction, ToDoubleBiFunction |

**Composing Functional Interfaces**:
- Predicate: `and()`, `or()`, `negate()`
- Function: `andThen()`, `compose()`
- Consumer: `andThen()`

---

## 4️⃣ Code Examples (Java 8)

### Example 1: @FunctionalInterface Annotation

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

### Example 2: Predicate Interface

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

### Example 3: Function Interface

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

### Example 4: Consumer Interface

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

### Example 5: Supplier Interface

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

### Example 6: BiFunction, BiPredicate, BiConsumer

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

### Example 7: UnaryOperator and BinaryOperator

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

### Example 8: Primitive Functional Interfaces

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

### Example 9: Custom Functional Interfaces

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

### Example 10: Real-World Example - Data Processing Pipeline

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

## 5️⃣ Common Interview Questions

1. **What is a functional interface?**
2. **What is the purpose of @FunctionalInterface annotation?**
3. **Can a functional interface have default methods?**
4. **What are the main built-in functional interfaces in Java 8?**
5. **What is the difference between Predicate and Function?**
6. **What is the difference between Consumer and Supplier?**
7. **Can a functional interface extend another functional interface?**
8. **Why do we have primitive functional interfaces like IntPredicate?**
9. **How do you compose functional interfaces?**
10. **What is the difference between Function.andThen() and Function.compose()?**

---

## 6️⃣ Model Answers

**Q1: What is a functional interface?**

"A functional interface is an interface with exactly one abstract method. It's also called a SAM interface—Single Abstract Method. It can have multiple default or static methods, but only one abstract method. Functional interfaces serve as target types for lambda expressions and method references. You can optionally annotate them with @FunctionalInterface for compile-time checking. Examples include Runnable with run(), Predicate with test(), and Function with apply(). They're the foundation of functional programming in Java 8."

**Q2: What is the purpose of @FunctionalInterface annotation?**

"The @FunctionalInterface annotation serves two purposes. First, it's a compile-time check that ensures the interface has exactly one abstract method—if you accidentally add a second abstract method, you get a compilation error. Second, it documents the intent that this interface is designed for lambda expressions, making the code more maintainable. While it's optional, it's highly recommended because it prevents breaking changes and clearly communicates design intent to other developers. It's similar to @Override in that it catches errors at compile-time."

**Q3: Can a functional interface have default methods?**

"Yes, functional interfaces can have any number of default methods. The key requirement is exactly one abstract method. Default methods have implementations, so they don't count against the single abstract method rule. They can also have static methods and methods from Object class like equals() and hashCode(). For example, Predicate has the abstract method test() but also has default methods and(), or(), and negate() for composition. This allows functional interfaces to provide additional utility while maintaining the single abstract method contract for lambda expressions."

**Q4: What are the main built-in functional interfaces in Java 8?**

"Java 8 provides four core functional interfaces in java.util.function package. Predicate<T> takes an input and returns boolean, used for testing conditions. Function<T,R> takes an input of type T and returns output of type R, used for transformations. Consumer<T> takes an input and returns nothing, used for operations with side effects like printing. Supplier<T> takes no input and returns a value, used for providing or generating values. There are also bi-variants like BiPredicate and BiFunction, operators like UnaryOperator and BinaryOperator, and primitive specializations like IntPredicate to avoid boxing overhead."

**Q5: What is the difference between Predicate and Function?**

"Predicate and Function serve different purposes. Predicate<T> takes an input and returns a boolean—it's used for testing conditions or filtering. Its abstract method is boolean test(T t). Function<T,R> takes an input of type T and returns a value of type R—it's used for transformations or mapping. Its abstract method is R apply(T t). So Predicate is essentially a specialized Function<T, Boolean>, but it's semantically different. Use Predicate when you're checking a condition, and Function when you're transforming data. For example, stream.filter() uses Predicate, while stream.map() uses Function."

**Q6: What is the difference between Consumer and Supplier?**

"Consumer and Supplier are opposites. Consumer<T> takes an input and returns nothing—its abstract method is void accept(T t). It's used for operations with side effects like printing, logging, or modifying collections. Supplier<T> takes no input and returns a value—its abstract method is T get(). It's used for providing or generating values, like factory methods or lazy evaluation. Consumer consumes data, Supplier supplies data. For example, list.forEach() uses Consumer to process each element, while Optional.orElseGet() uses Supplier to provide a default value only if needed."

**Q7: Can a functional interface extend another functional interface?**

"Yes, a functional interface can extend another functional interface, but the total number of abstract methods must still be one. If the parent has one abstract method and the child doesn't add any new abstract methods, it's still a valid functional interface. For example, UnaryOperator<T> extends Function<T,T> but doesn't add new abstract methods, so it's still functional. However, if the child adds a new abstract method, it's no longer a functional interface. The key is that after inheritance, there must be exactly one abstract method to implement."

**Q8: Why do we have primitive functional interfaces like IntPredicate?**

"Primitive functional interfaces exist to avoid the performance overhead of autoboxing and unboxing. When you use Predicate<Integer>, every int value is boxed to Integer, which creates object overhead. IntPredicate works directly with primitive int values, avoiding this cost. This matters in performance-critical code, especially with large datasets or tight loops. Java provides primitive versions for int, long, and double—the most commonly used numeric types. For example, IntStream uses IntPredicate for filtering to avoid boxing millions of integers. The performance difference can be significant in high-volume operations."

**Q9: How do you compose functional interfaces?**

"Most functional interfaces provide composition methods. Predicate has and(), or(), and negate() for combining predicates. Function has andThen() and compose() for chaining transformations. Consumer has andThen() for sequencing operations. For example, you can create Predicate<String> isLongAndUpper = isLong.and(isUpperCase) to check both conditions. With Function, you can chain operations: Function<String, Integer> getLength = String::length; Function<Integer, Integer> square = x -> x * x; Function<String, Integer> composed = getLength.andThen(square). This enables building complex logic from simple, reusable components."

**Q10: What is the difference between Function.andThen() and Function.compose()?**

"Both compose functions but in different orders. andThen() applies the current function first, then the next function. compose() applies the parameter function first, then the current function. If you have f.andThen(g), it means g(f(x)). If you have f.compose(g), it means f(g(x)). For example, if f doubles a number and g adds 10: f.andThen(g) on 5 gives (5*2)+10=20. f.compose(g) on 5 gives (5+10)*2=30. Think of andThen as 'do this, then that' and compose as 'do that first, then this'. Most people find andThen more intuitive."

---

## 7️⃣ Common Mistakes

1. **Adding multiple abstract methods to @FunctionalInterface**:
   ```java
   @FunctionalInterface
   interface Invalid {
       void method1();
       void method2();  // Compilation error!
   }
   ```

2. **Forgetting that functional interface can have default methods**:
   ```java
   @FunctionalInterface
   interface MyInterface {
       void doSomething();
       
       default void helper() {  // This is fine!
           System.out.println("Helper");
       }
   }
   ```

3. **Not using primitive functional interfaces when appropriate**:
   ```java
   // Inefficient - boxing overhead
   Predicate<Integer> isEven = n -> n % 2 == 0;
   
   // Better - no boxing
   IntPredicate isEven = n -> n % 2 == 0;
   ```

4. **Confusing andThen and compose**:
   ```java
   Function<Integer, Integer> f = x -> x * 2;
   Function<Integer, Integer> g = x -> x + 10;
   
   f.andThen(g).apply(5);  // (5*2)+10 = 20
   f.compose(g).apply(5);  // (5+10)*2 = 30
   ```

5. **Not handling null in functional interfaces**:
   ```java
   Predicate<String> startsWithA = s -> s.startsWith("A");
   startsWithA.test(null);  // NullPointerException!
   
   // Better:
   Predicate<String> startsWithA = s -> s != null && s.startsWith("A");
   ```

6. **Using wrong functional interface**:
   ```java
   // Wrong - Consumer returns void
   List<String> names = list.stream()
       .map(s -> System.out.println(s))  // Returns void!
       .collect(Collectors.toList());
   
   // Correct - use Function or peek
   ```

7. **Not understanding BiFunction result type**:
   ```java
   BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
   // Returns Integer, not void
   ```

8. **Forgetting Supplier takes no parameters**:
   ```java
   // Supplier<Integer> random = (max) -> ...; // Error!
   
   // Correct:
   Supplier<Integer> random = () -> (int)(Math.random() * 100);
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Functional interface = exactly one abstract method
- @FunctionalInterface is optional but recommended
- Core four: Predicate (test), Function (transform), Consumer (consume), Supplier (supply)
- Can have default and static methods
- Primitive versions avoid boxing overhead
- Composition methods: and/or/negate, andThen/compose

**What to avoid**:
- Don't say functional interfaces can't have default methods
- Don't claim @FunctionalInterface is mandatory
- Don't forget primitive specializations
- Don't confuse UnaryOperator with Function (same type in/out)

**Under pressure**:
- Remember SAM: Single Abstract Method
- List the core four with their methods
- Explain with simple example: `Predicate<Integer> isEven = n -> n % 2 == 0`
- Mention lambda expressions target functional interfaces

**Red flags to avoid**:
- "Functional interfaces can only have one method" (can have default/static)
- "You must use @FunctionalInterface" (it's optional)
- "BiFunction takes three parameters" (takes two, returns one)
- "Consumer returns a value" (it's void)

**Bonus points**:
- Mention that Runnable, Callable, Comparator are functional interfaces
- Discuss method references as shorthand for lambdas
- Reference composition methods for building complex logic
- Mention functional interfaces enable declarative programming
- Discuss when to create custom functional interfaces vs using built-in ones
- Reference that functional interfaces are used throughout Stream API
