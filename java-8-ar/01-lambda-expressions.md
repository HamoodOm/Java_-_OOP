<div dir="rtl">

# تعبيرات Lambda

## 1. نظرة عامة على المفهوم

**تعبير Lambda** هو طريقة مختصرة لتمثيل دالة مجهولة (دالة بدون اسم) يمكن تمريرها كما لو كانت كائناً. تم تقديمها في Java 8، وتمكّن تعبيرات Lambda من البرمجة الوظيفية في Java.

**الصيغة**:
```java
(parameters) -> expression
// أو
(parameters) -> { statements; }
```

**الخصائص الرئيسية**:
- مجهولة: بدون اسم
- دالة: غير مرتبطة بصنف، يمكن تمريرها
- مختصرة: كود أقل مقارنة بالأصناف الداخلية المجهولة
- الواجهة الوظيفية: يمكن استخدامها فقط حيث يُتوقع واجهة وظيفية

**الواجهة الوظيفية (Functional Interface)**:
- واجهة تحتوي على طريقة مجردة واحدة فقط
- يمكن أن تحتوي على عدة طرق default أو static
- توضع عليها التعليق التوضيحي `@FunctionalInterface` (اختياري لكن موصى به)
- أمثلة: `Runnable`، `Callable`، `Comparator`، `Predicate`، `Function`

**مقارنة Lambda مع الصنف المجهول**:

| الميزة | Lambda | الصنف المجهول |
|--------|--------|---------------|
| الصيغة | مختصرة | مطولة |
| مرجع this | يشير إلى الصنف المحيط | يشير إلى الصنف المجهول |
| الترجمة | تعليمة invokedynamic | يُنشئ ملف .class جديد |
| الاستخدام | الواجهات الوظيفية فقط | أي واجهة أو صنف مجرد |
| الأداء | أفضل قليلاً | أبطأ قليلاً |

**حالات الاستخدام الشائعة**:
- عمليات المجموعات (forEach، filter، map)
- معالجة الأحداث
- إنشاء الخيوط
- المقارنات
- عمليات Stream API

---

## 2. لماذا يسأل المحاورون عن هذا

- **اعتماد Java 8**: يختبر المعرفة بميزات Java الحديثة
- **البرمجة الوظيفية**: يُظهر فهم التحول في النموذج البرمجي
- **جودة الكود**: تعبيرات Lambda تؤدي إلى كود أنظف وأكثر قابلية للقراءة
- **أساس Stream API**: تعبيرات Lambda ضرورية للـ streams
- **الأهمية في الواقع**: تُستخدم على نطاق واسع في تطبيقات Java الحديثة
- **أنماط التصميم**: تمكّن من أنماط التصميم الوظيفية

---

## 3. القواعد والحقائق الأساسية

**صيغة تعبير Lambda**:

**1. بدون معاملات**:
```java
() -> System.out.println("Hello")
```

**2. معامل واحد** (الأقواس اختيارية):
```java
x -> x * x
// أو
(x) -> x * x
```

**3. معاملات متعددة**:
```java
(x, y) -> x + y
```

**4. مع تصريحات الأنواع**:
```java
(int x, int y) -> x + y
```

**5. مع كتلة كود**:
```java
(x, y) -> {
    int sum = x + y;
    return sum;
}
```

**6. إرجاع قيمة**:
```java
(x, y) -> x + y  // إرجاع ضمني للتعبير الواحد
(x, y) -> { return x + y; }  // إرجاع صريح للكتلة
```

**القواعد المهمة**:
- Lambda يمكن استخدامها فقط مع الواجهات الوظيفية
- استنتاج النوع: المترجم يستنتج أنواع المعاملات
- final أو effectively final: المتغيرات المستخدمة في Lambda يجب أن تكون final أو effectively final
- لا يمكن الوصول إلى المتغيرات المحلية غير النهائية من النطاق المحيط
- الكلمة المفتاحية `this` تشير إلى مثيل الصنف المحيط، وليس Lambda نفسها
- لا يمكن تعريف متغيرات بنفس اسم النطاق المحيط
- يمكن رمي الاستثناءات المتحقق منها فقط إذا أعلنت طريقة الواجهة الوظيفية عنها

**الواجهات الوظيفية المدمجة** (java.util.function):

| الواجهة | الطريقة | الوصف | مثال |
|---------|---------|-------|------|
| Predicate<T> | boolean test(T t) | تختبر شرطاً | `x -> x > 10` |
| Function<T,R> | R apply(T t) | تحول المدخل إلى مخرج | `x -> x.toString()` |
| Consumer<T> | void accept(T t) | تستهلك المدخل، بدون إرجاع | `x -> System.out.println(x)` |
| Supplier<T> | T get() | توفر قيمة | `() -> new ArrayList<>()` |
| BiPredicate<T,U> | boolean test(T t, U u) | تختبر مدخلين | `(x,y) -> x.equals(y)` |
| BiFunction<T,U,R> | R apply(T t, U u) | تأخذ مدخلين | `(x,y) -> x + y` |
| BiConsumer<T,U> | void accept(T t, U u) | تستهلك مدخلين | `(k,v) -> map.put(k,v)` |
| UnaryOperator<T> | T apply(T t) | مدخل واحد، نفس نوع المخرج | `x -> x * 2` |
| BinaryOperator<T> | T apply(T t, T u) | مدخلان، نفس نوع المخرج | `(x,y) -> x + y` |

**مراجع الطرق (Method References)** (اختصار لـ Lambda):
- طريقة ثابتة: `ClassName::staticMethod`
- طريقة مثيل: `instance::instanceMethod`
- طريقة مثيل لكائن عشوائي: `ClassName::instanceMethod`
- المُنشئ: `ClassName::new`

**التقاط المتغيرات**:
- Lambda يمكنها الوصول إلى المتغيرات effectively final من النطاق المحيط
- لا يمكن تعديل المتغيرات من النطاق المحيط
- تُنشئ closure على المتغيرات

---

## 4. أمثلة برمجية (Java 8)

### المثال 1: صيغة Lambda الأساسية

```java
public class BasicLambdaSyntax {
    public static void main(String[] args) {
        // Traditional anonymous class
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello from anonymous class");
            }
        };
        r1.run();

        // Lambda expression - much cleaner!
        Runnable r2 = () -> System.out.println("Hello from lambda");
        r2.run();

        // Comparator with anonymous class
        java.util.Comparator<String> comp1 = new java.util.Comparator<String>() {
            @Override
            public int compare(String s1, String s2) {
                return s1.length() - s2.length();
            }
        };

        // Comparator with lambda
        java.util.Comparator<String> comp2 = (s1, s2) -> s1.length() - s2.length();

        // Even shorter with method reference
        java.util.Comparator<String> comp3 = java.util.Comparator.comparingInt(String::length);

        // Different lambda syntaxes

        // No parameters
        Runnable noParams = () -> System.out.println("No parameters");

        // One parameter (parentheses optional)
        java.util.function.Consumer<String> oneParam = s -> System.out.println(s);
        java.util.function.Consumer<String> oneParamWithParens = (s) -> System.out.println(s);

        // Multiple parameters
        java.util.function.BiFunction<Integer, Integer, Integer> twoParams =
            (a, b) -> a + b;

        // With type declarations (usually not needed due to type inference)
        java.util.function.BiFunction<Integer, Integer, Integer> withTypes =
            (Integer a, Integer b) -> a + b;

        // Multi-line lambda with code block
        java.util.function.BiFunction<Integer, Integer, Integer> multiLine =
            (a, b) -> {
                int sum = a + b;
                int square = sum * sum;
                return square;
            };

        System.out.println("Result: " + multiLine.apply(3, 4));  // (3+4)^2 = 49
    }
}
```

### المثال 2: Lambda مع الواجهات الوظيفية

```java
// Custom functional interface
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

@FunctionalInterface
interface StringProcessor {
    String process(String input);
}

public class FunctionalInterfaceDemo {
    public static void main(String[] args) {
        // Using custom functional interface
        MathOperation add = (a, b) -> a + b;
        MathOperation subtract = (a, b) -> a - b;
        MathOperation multiply = (a, b) -> a * b;
        MathOperation divide = (a, b) -> a / b;

        System.out.println("10 + 5 = " + add.operate(10, 5));        // 15
        System.out.println("10 - 5 = " + subtract.operate(10, 5));   // 5
        System.out.println("10 * 5 = " + multiply.operate(10, 5));   // 50
        System.out.println("10 / 5 = " + divide.operate(10, 5));     // 2

        // Passing lambda to method
        System.out.println("Custom operation: " +
            performOperation(10, 5, (a, b) -> a * a + b * b));  // 100 + 25 = 125

        // String processing
        StringProcessor toUpper = s -> s.toUpperCase();
        StringProcessor toLower = s -> s.toLowerCase();
        StringProcessor reverse = s -> new StringBuilder(s).reverse().toString();

        String input = "Hello World";
        System.out.println("Upper: " + toUpper.process(input));
        System.out.println("Lower: " + toLower.process(input));
        System.out.println("Reverse: " + reverse.process(input));
    }

    // Method accepting lambda
    public static int performOperation(int a, int b, MathOperation operation) {
        return operation.operate(a, b);
    }
}
```

### المثال 3: الواجهات الوظيفية المدمجة

```java
import java.util.function.*;
import java.util.*;

public class BuiltInFunctionalInterfaces {
    public static void main(String[] args) {
        // 1. Predicate<T> - boolean test(T t)
        Predicate<Integer> isEven = n -> n % 2 == 0;
        System.out.println("10 is even: " + isEven.test(10));  // true
        System.out.println("7 is even: " + isEven.test(7));    // false

        Predicate<String> startsWithA = s -> s.startsWith("A");
        System.out.println("Apple starts with A: " + startsWithA.test("Apple"));  // true

        // 2. Function<T, R> - R apply(T t)
        Function<String, Integer> stringLength = s -> s.length();
        System.out.println("Length of 'Hello': " + stringLength.apply("Hello"));  // 5

        Function<Integer, Integer> square = n -> n * n;
        System.out.println("Square of 5: " + square.apply(5));  // 25

        // 3. Consumer<T> - void accept(T t)
        Consumer<String> printer = s -> System.out.println("Value: " + s);
        printer.accept("Hello");  // Value: Hello

        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        names.forEach(name -> System.out.println("Name: " + name));

        // 4. Supplier<T> - T get()
        Supplier<Double> randomValue = () -> Math.random();
        System.out.println("Random: " + randomValue.get());

        Supplier<List<String>> listSupplier = () -> new ArrayList<>();
        List<String> list = listSupplier.get();

        // 5. BiPredicate<T, U> - boolean test(T t, U u)
        BiPredicate<String, Integer> isLengthGreater = (s, len) -> s.length() > len;
        System.out.println("Is 'Hello' length > 3: " +
            isLengthGreater.test("Hello", 3));  // true

        // 6. BiFunction<T, U, R> - R apply(T t, U u)
        BiFunction<Integer, Integer, Integer> sum = (a, b) -> a + b;
        System.out.println("10 + 20 = " + sum.apply(10, 20));  // 30

        // 7. BiConsumer<T, U> - void accept(T t, U u)
        BiConsumer<String, Integer> printKeyValue = (k, v) ->
            System.out.println(k + " = " + v);
        printKeyValue.accept("Age", 25);  // Age = 25

        // 8. UnaryOperator<T> - T apply(T t)
        UnaryOperator<Integer> doubleIt = n -> n * 2;
        System.out.println("Double of 5: " + doubleIt.apply(5));  // 10

        // 9. BinaryOperator<T> - T apply(T t, T u)
        BinaryOperator<Integer> max = (a, b) -> a > b ? a : b;
        System.out.println("Max of 10 and 20: " + max.apply(10, 20));  // 20

        // Chaining functional interfaces
        Function<Integer, Integer> multiplyBy2 = n -> n * 2;
        Function<Integer, Integer> add10 = n -> n + 10;
        Function<Integer, Integer> combined = multiplyBy2.andThen(add10);
        System.out.println("(5 * 2) + 10 = " + combined.apply(5));  // 20
    }
}
```

### المثال 4: Lambda مع المجموعات

```java
import java.util.*;
import java.util.stream.*;

public class LambdaWithCollections {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");

        // 1. forEach
        System.out.println("Using forEach:");
        names.forEach(name -> System.out.println(name));

        // 2. removeIf
        List<String> numbers = new ArrayList<>(Arrays.asList("1", "2", "3", "4", "5"));
        numbers.removeIf(n -> Integer.parseInt(n) % 2 == 0);
        System.out.println("After removeIf (odd only): " + numbers);  // [1, 3, 5]

        // 3. replaceAll
        List<String> words = new ArrayList<>(Arrays.asList("hello", "world"));
        words.replaceAll(s -> s.toUpperCase());
        System.out.println("After replaceAll: " + words);  // [HELLO, WORLD]

        // 4. sort with Comparator
        List<String> fruits = new ArrayList<>(Arrays.asList("Banana", "Apple", "Cherry"));
        fruits.sort((s1, s2) -> s1.compareTo(s2));  // Sort alphabetically
        System.out.println("Sorted: " + fruits);

        // Sort by length
        fruits.sort((s1, s2) -> Integer.compare(s1.length(), s2.length()));
        System.out.println("Sorted by length: " + fruits);

        // 5. Map operations
        Map<String, Integer> map = new HashMap<>();
        map.put("Apple", 5);
        map.put("Banana", 3);
        map.put("Cherry", 7);

        // forEach on map
        map.forEach((key, value) ->
            System.out.println(key + " = " + value));

        // computeIfAbsent
        map.computeIfAbsent("Date", k -> 10);
        System.out.println("After computeIfAbsent: " + map);

        // merge
        map.merge("Apple", 5, (oldVal, newVal) -> oldVal + newVal);
        System.out.println("After merge (Apple): " + map.get("Apple"));  // 10

        // 6. Stream operations (heavily use lambdas)
        List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> evenSquares = nums.stream()
            .filter(n -> n % 2 == 0)           // Filter even numbers
            .map(n -> n * n)                    // Square them
            .collect(Collectors.toList());
        System.out.println("Even squares: " + evenSquares);  // [4, 16, 36, 64, 100]

        // Find sum of squares of odd numbers
        int sumOfOddSquares = nums.stream()
            .filter(n -> n % 2 != 0)
            .map(n -> n * n)
            .reduce(0, (a, b) -> a + b);
        System.out.println("Sum of odd squares: " + sumOfOddSquares);  // 165
    }
}
```

### المثال 5: مراجع الطرق

```java
import java.util.*;
import java.util.function.*;

public class MethodReferenceDemo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

        // 1. Static method reference
        // Lambda: s -> Integer.parseInt(s)
        // Method reference: Integer::parseInt
        Function<String, Integer> parser1 = s -> Integer.parseInt(s);
        Function<String, Integer> parser2 = Integer::parseInt;
        System.out.println(parser2.apply("123"));  // 123

        // 2. Instance method reference of a particular object
        String prefix = "Hello, ";
        // Lambda: s -> prefix.concat(s)
        // Method reference: prefix::concat
        Function<String, String> greeter1 = s -> prefix.concat(s);
        Function<String, String> greeter2 = prefix::concat;
        System.out.println(greeter2.apply("World"));  // Hello, World

        // 3. Instance method reference of an arbitrary object of a particular type
        // Lambda: s -> s.toUpperCase()
        // Method reference: String::toUpperCase
        Function<String, String> upper1 = s -> s.toUpperCase();
        Function<String, String> upper2 = String::toUpperCase;

        names.stream()
            .map(String::toUpperCase)
            .forEach(System.out::println);

        // 4. Constructor reference
        // Lambda: () -> new ArrayList<>()
        // Method reference: ArrayList::new
        Supplier<List<String>> listCreator1 = () -> new ArrayList<>();
        Supplier<List<String>> listCreator2 = ArrayList::new;

        // Lambda: s -> new String(s)
        // Method reference: String::new
        Function<char[], String> stringCreator = String::new;

        // 5. More examples
        List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 9);

        // Using method reference with forEach
        numbers.forEach(System.out::println);

        // Sorting with method reference
        List<String> words = new ArrayList<>(Arrays.asList("banana", "apple", "cherry"));
        words.sort(String::compareToIgnoreCase);
        System.out.println("Sorted: " + words);

        // Constructor reference with map
        List<Integer> nums = Arrays.asList(1, 2, 3);
        List<String> strings = nums.stream()
            .map(String::valueOf)  // Method reference
            .collect(java.util.stream.Collectors.toList());
        System.out.println("Strings: " + strings);
    }
}
```

### المثال 6: التقاط المتغيرات و Effectively Final

```java
public class VariableCaptureDemo {
    public static void main(String[] args) {
        // Effectively final variable (not modified after initialization)
        String prefix = "Hello ";

        // Lambda can access effectively final variables
        Consumer<String> greeter = name -> System.out.println(prefix + name);
        greeter.accept("Alice");  // Hello Alice

        // prefix = "Hi ";  // If uncommented, compilation error!
        // Lambda captures variables, they must be final or effectively final

        // Final variable - explicitly final
        final String suffix = "!";
        Consumer<String> exclaimer = name -> System.out.println(name + suffix);
        exclaimer.accept("Bob");  // Bob!

        // Instance variables and static variables can be modified
        Counter counter = new Counter();
        Runnable incrementer = () -> counter.count++;  // OK - modifying instance var

        incrementer.run();
        incrementer.run();
        System.out.println("Count: " + counter.count);  // 2

        // Cannot modify local variable from lambda
        int localVar = 10;
        // Runnable modifier = () -> localVar++;  // Compilation error!

        // But can use it if not modified
        Runnable printer = () -> System.out.println("Value: " + localVar);
        printer.run();  // Value: 10

        // The variable is effectively final
        // localVar = 20;  // If uncommented, compilation error!

        // this reference in lambda
        new LambdaThis().testThis();
    }
}

class Counter {
    int count = 0;
}

class LambdaThis {
    private String value = "Enclosing class";

    public void testThis() {
        // In lambda, 'this' refers to enclosing class
        Runnable r1 = () -> System.out.println("Lambda this: " + this.value);
        r1.run();  // Lambda this: Enclosing class

        // In anonymous class, 'this' refers to anonymous class itself
        Runnable r2 = new Runnable() {
            private String value = "Anonymous class";

            @Override
            public void run() {
                System.out.println("Anonymous this: " + this.value);
            }
        };
        r2.run();  // Anonymous this: Anonymous class
    }
}
```

### المثال 7: Lambda مع الخيوط

```java
public class LambdaWithThreads {
    public static void main(String[] args) {
        // Traditional way with anonymous class
        Thread t1 = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Thread 1: " + Thread.currentThread().getName());
            }
        });
        t1.start();

        // Lambda way - much cleaner
        Thread t2 = new Thread(() ->
            System.out.println("Thread 2: " + Thread.currentThread().getName())
        );
        t2.start();

        // Multi-line lambda
        Thread t3 = new Thread(() -> {
            System.out.println("Thread 3 starting");
            for (int i = 0; i < 3; i++) {
                System.out.println("Thread 3: " + i);
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("Thread 3 ending");
        });
        t3.start();

        // Using ExecutorService with lambda
        java.util.concurrent.ExecutorService executor =
            java.util.concurrent.Executors.newFixedThreadPool(2);

        executor.submit(() -> System.out.println("Task 1"));
        executor.submit(() -> System.out.println("Task 2"));
        executor.submit(() -> {
            System.out.println("Task 3 with multiple statements");
            return "Result from Task 3";
        });

        executor.shutdown();
    }
}
```

### المثال 8: مثال واقعي - إدارة الموظفين

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

    // Getters
    public String getName() { return name; }
    public String getDepartment() { return department; }
    public double getSalary() { return salary; }
    public int getAge() { return age; }

    @Override
    public String toString() {
        return String.format("%s (%s, $%.0f, %d years)",
            name, department, salary, age);
    }
}

public class EmployeeManagement {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee("Alice", "IT", 75000, 28),
            new Employee("Bob", "HR", 55000, 35),
            new Employee("Charlie", "IT", 85000, 32),
            new Employee("David", "Finance", 70000, 29),
            new Employee("Eve", "IT", 95000, 40),
            new Employee("Frank", "HR", 60000, 27)
        );

        // 1. Filter IT department employees
        System.out.println("IT Employees:");
        employees.stream()
            .filter(e -> e.getDepartment().equals("IT"))
            .forEach(System.out::println);

        // 2. Get names of employees earning more than 70k
        List<String> highEarners = employees.stream()
            .filter(e -> e.getSalary() > 70000)
            .map(Employee::getName)
            .collect(Collectors.toList());
        System.out.println("\nHigh earners: " + highEarners);

        // 3. Average salary by department
        Map<String, Double> avgSalaryByDept = employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.averagingDouble(Employee::getSalary)
            ));
        System.out.println("\nAverage salary by department:");
        avgSalaryByDept.forEach((dept, avg) ->
            System.out.printf("%s: $%.2f%n", dept, avg));

        // 4. Find oldest employee in IT
        Optional<Employee> oldestIT = employees.stream()
            .filter(e -> e.getDepartment().equals("IT"))
            .max(Comparator.comparingInt(Employee::getAge));
        oldestIT.ifPresent(e ->
            System.out.println("\nOldest IT employee: " + e.getName()));

        // 5. Sort by salary descending
        System.out.println("\nEmployees sorted by salary (desc):");
        employees.stream()
            .sorted((e1, e2) -> Double.compare(e2.getSalary(), e1.getSalary()))
            .forEach(System.out::println);

        // 6. Count employees per department
        Map<String, Long> countByDept = employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.counting()
            ));
        System.out.println("\nEmployee count by department: " + countByDept);

        // 7. Total salary expense
        double totalSalary = employees.stream()
            .mapToDouble(Employee::getSalary)
            .sum();
        System.out.printf("\nTotal salary expense: $%.2f%n", totalSalary);

        // 8. Get top 3 earners
        List<Employee> top3 = employees.stream()
            .sorted(Comparator.comparingDouble(Employee::getSalary).reversed())
            .limit(3)
            .collect(Collectors.toList());
        System.out.println("\nTop 3 earners:");
        top3.forEach(System.out::println);

        // 9. Check if any employee is over 35
        boolean hasOlderEmployee = employees.stream()
            .anyMatch(e -> e.getAge() > 35);
        System.out.println("\nHas employee over 35: " + hasOlderEmployee);

        // 10. Custom operation with lambda
        employees.stream()
            .filter(e -> e.getSalary() > 60000 && e.getAge() < 35)
            .forEach(e -> System.out.println(
                "Young high earner: " + e.getName() + " ($" + e.getSalary() + ")"));
    }
}
```

---

## 5. أسئلة المقابلات الشائعة

1. **ما هو تعبير Lambda في Java؟**
2. **ما هي الواجهة الوظيفية؟**
3. **ما الفرق بين Lambda والصنف المجهول؟**
4. **هل يمكن لتعبير Lambda الوصول إلى المتغيرات المحلية؟ ما هي القيود؟**
5. **ما هو مرجع الطريقة وكيف يرتبط بـ Lambda؟**
6. **ما هي الواجهات الوظيفية المدمجة في Java 8؟**
7. **هل يمكن لتعبيرات Lambda رمي الاستثناءات؟**
8. **إلى ماذا تشير الكلمة المفتاحية 'this' في تعبير Lambda؟**
9. **هل يمكن أن يكون لدينا عدة طرق مجردة في واجهة وظيفية؟**
10. **ما ميزة تعبيرات Lambda على الأصناف المجهولة؟**

---

## 6. إجابات نموذجية

**س1: ما هو تعبير Lambda في Java؟**

"تعبير Lambda هو طريقة مختصرة لتمثيل دالة مجهولة يمكن معاملتها كقيمة. تم تقديمها في Java 8، وتمكّن من البرمجة الوظيفية بالسماح لك بتمرير السلوك كمعامل. الصيغة الأساسية هي (parameters) -> expression أو (parameters) -> { statements }. مثلاً، بدلاً من إنشاء Runnable مجهول بعدة أسطر، يمكنك كتابة () -> System.out.println('Hello'). تعبيرات Lambda يمكن استخدامها فقط حيث يُتوقع واجهة وظيفية - واجهة بطريقة مجردة واحدة فقط."

**س2: ما هي الواجهة الوظيفية؟**

"الواجهة الوظيفية هي واجهة بطريقة مجردة واحدة فقط، وتسمى أيضاً واجهة SAM (Single Abstract Method). يمكن أن تحتوي على عدة طرق default أو static، لكن طريقة مجردة واحدة فقط. الأمثلة تشمل Runnable مع run()، وComparator مع compare()، وPredicate مع test(). يمكنك وضع التعليق التوضيحي @FunctionalInterface للتحقق وقت الترجمة، رغم أنه اختياري. الواجهات الوظيفية هي الأنواع المستهدفة لتعبيرات Lambda - كلما كتبت Lambda، فأنت تنفذ الطريقة المجردة الوحيدة للواجهة الوظيفية."

**س3: ما الفرق بين Lambda والصنف المجهول؟**

"الاختلافات الرئيسية هي: أولاً، الصيغة - Lambda أكثر اختصاراً. ثانياً، الكلمة المفتاحية 'this' - في Lambda، 'this' تشير إلى الصنف المحيط؛ في الصنف المجهول، 'this' تشير إلى الصنف المجهول نفسه. ثالثاً، الترجمة - Lambda تستخدم تعليمة invokedynamic الأكثر كفاءة، بينما الأصناف المجهولة تُنشئ ملفات .class منفصلة. رابعاً، الاستخدام - Lambda تعمل فقط مع الواجهات الوظيفية، بينما الأصناف المجهولة يمكنها تنفيذ أي واجهة أو توسيع أي صنف. خامساً، Lambda أسرع قليلاً في الأداء. عملياً، Lambda مفضلة عند تنفيذ واجهة وظيفية."

**س4: هل يمكن لتعبير Lambda الوصول إلى المتغيرات المحلية؟ ما هي القيود؟**

"نعم، تعبيرات Lambda يمكنها الوصول إلى المتغيرات المحلية من النطاق المحيط، لكن بقيد: المتغيرات يجب أن تكون final أو effectively final. effectively final تعني أن المتغير لم يُعدَّل بعد التهيئة. هذا لأن Lambda تُنشئ closures - تلتقط المتغيرات من السياق المحيط. إذا كان المتغير قابلاً للتعديل، سيسبب مشاكل التزامن عند تنفيذ Lambda لاحقاً أو في خيط مختلف. متغيرات المثيل والمتغيرات الثابتة يمكن الوصول إليها وتعديلها بدون قيود. محاولة تعديل متغير محلي من Lambda يؤدي إلى خطأ ترجمة."

**س5: ما هو مرجع الطريقة وكيف يرتبط بـ Lambda؟**

"مرجع الطريقة هو تدوين مختصر لتعبيرات Lambda التي تستدعي ببساطة طريقة موجودة. هناك أربعة أنواع: مراجع الطرق الثابتة مثل Integer::parseInt، مراجع طرق المثيل مثل someString::length، مراجع طرق المثيل لكائنات عشوائية مثل String::toUpperCase، ومراجع المُنشئ مثل ArrayList::new. ترتبط بـ Lambda لأن مرجع الطريقة هو في الأساس Lambda أكثر اختصاراً. مثلاً، s -> Integer.parseInt(s) يمكن كتابتها كـ Integer::parseInt. مراجع الطرق تجعل الكود أكثر قابلية للقراءة عندما تفوض Lambda ببساطة إلى طريقة موجودة."

**س6: ما هي الواجهات الوظيفية المدمجة في Java 8؟**

"Java 8 توفر عدة واجهات وظيفية في حزمة java.util.function. الرئيسية هي: Predicate<T> لاختبار الشروط مع طريقة test()؛ Function<T,R> للتحويلات مع apply()؛ Consumer<T> للعمليات بدون إرجاع باستخدام accept()؛ Supplier<T> لتوفير القيم مع get()؛ وتنويعاتها مثل BiPredicate وBiFunction وBiConsumer لمعاملين. هناك أيضاً نسخ متخصصة مثل UnaryOperator وBinaryOperator. هذه الواجهات تغطي معظم أنماط البرمجة الوظيفية الشائعة وتعمل بسلاسة مع Stream API وتعبيرات Lambda."

**س7: هل يمكن لتعبيرات Lambda رمي الاستثناءات؟**

"تعبيرات Lambda يمكنها رمي الاستثناءات، لكن بقيود. يمكنها رمي الاستثناءات غير المتحقق منها بحرية. للاستثناءات المتحقق منها، يجب أن تعلن الطريقة المجردة للواجهة الوظيفية عن تلك الاستثناءات في جملة throws. مثلاً، run() في Runnable لا تعلن عن أي استثناءات متحقق منها، لذا Lambda التي تنفذ Runnable لا يمكنها رمي استثناءات متحقق منها. إذا احتجت لرمي استثناءات متحقق منها، عليك إما معالجتها داخل Lambda، أو استخدام واجهة وظيفية مخصصة تعلن عن الاستثناء، أو لفها في استثناءات غير متحقق منها مثل RuntimeException."

**س8: إلى ماذا تشير الكلمة المفتاحية 'this' في تعبير Lambda؟**

"في تعبير Lambda، 'this' تشير إلى مثيل الصنف المحيط، وليس Lambda نفسها. هذا يختلف عن الأصناف المجهولة حيث 'this' تشير إلى مثيل الصنف المجهول. هذا السلوك يجعل Lambda أكثر بديهية - يمكنك الوصول إلى متغيرات وطرق الصنف الخارجي مباشرة باستخدام 'this'. مثلاً، إذا كان لديك Lambda داخل طريقة صنف، 'this.field' في Lambda تشير إلى حقل الصنف الخارجي. هذا أحد الأسباب التي تجعل Lambda تُعتبر أكثر أناقة من الأصناف المجهولة."

**س9: هل يمكن أن يكون لدينا عدة طرق مجردة في واجهة وظيفية؟**

"لا، الواجهة الوظيفية يجب أن تحتوي على طريقة مجردة واحدة فقط. هذا ما يجعلها 'وظيفية' - تمثل دالة واحدة. ومع ذلك، يمكن أن تحتوي على عدة طرق default وstatic لأن لها تنفيذات. يمكنها أيضاً أن تحتوي على طرق من صنف Object مثل equals() وhashCode() وtoString() دون انتهاك قاعدة الطريقة المجردة الواحدة. إذا حاولت إضافة طريقة مجردة ثانية لواجهة موضوع عليها @FunctionalInterface، ستحصل على خطأ ترجمة. الطريقة المجردة الواحدة هي ما ينفذه تعبير Lambda."

**س10: ما ميزة تعبيرات Lambda على الأصناف المجهولة؟**

"تعبيرات Lambda تقدم عدة مزايا: أولاً، الاختصار - تقلل الكود المكرر بشكل كبير، غالباً من 6-7 أسطر إلى سطر واحد. ثانياً، القابلية للقراءة - النية أوضح بدون كل التفاصيل الإضافية. ثالثاً، أداء أفضل - Lambda تستخدم تعليمة invokedynamic التي تسمح بتحسينات JVM، بينما الأصناف المجهولة تُنشئ ملفات .class جديدة. رابعاً، برمجة وظيفية أسهل - Lambda تمكّن من النمط الوظيفي مع عمليات مثل map وfilter وreduce. خامساً، استنتاج النوع - عادة لا تحتاج لتحديد الأنواع صراحة. إجمالاً، Lambda تجعل الكود أكثر قابلية للصيانة وتمكّن من أنماط البرمجة الوظيفية القوية التي لم تكن عملية مع الأصناف المجهولة."

---

## 7. الأخطاء الشائعة

1. **تعديل المتغيرات المحلية من Lambda**:
   ```java
   int count = 0;
   Runnable r = () -> count++;  // Compilation error!
   // Local variables must be final or effectively final
   ```

2. **استخدام Lambda مع واجهة غير وظيفية**:
   ```java
   interface NotFunctional {
       void method1();
       void method2();  // Two abstract methods
   }

   // NotFunctional f = () -> {};  // Compilation error!
   ```

3. **نسيان جملة return في Lambda متعددة الأسطر**:
   ```java
   Function<Integer, Integer> square = n -> {
       n * n;  // Missing return!
   };

   // Correct:
   Function<Integer, Integer> square = n -> {
       return n * n;
   };
   ```

4. **استخدام 'this' بشكل خاطئ**:
   ```java
   class MyClass {
       String value = "outer";

       void test() {
           Runnable r = () -> {
               // 'this' refers to MyClass, not lambda
               System.out.println(this.value);  // "outer"
           };
       }
   }
   ```

5. **عدم معالجة الاستثناءات المتحقق منها بشكل صحيح**:
   ```java
   Runnable r = () -> {
       Thread.sleep(1000);  // Compilation error!
       // Runnable.run() doesn't declare InterruptedException
   };

   // Must handle it:
   Runnable r = () -> {
       try {
           Thread.sleep(1000);
       } catch (InterruptedException e) {
           e.printStackTrace();
       }
   };
   ```

6. **محاولة استخدام Lambda للأصناف المجردة**:
   ```java
   abstract class AbstractClass {
       abstract void method();
   }

   // AbstractClass obj = () -> {};  // Cannot use lambda!
   // Lambdas only work with interfaces
   ```

7. **افتراض أن Lambda تُنشئ نطاقاً جديداً للمتغيرات**:
   ```java
   int x = 10;
   Consumer<Integer> c = x -> System.out.println(x);  // Error!
   // Cannot redeclare variable 'x' from outer scope

   // Correct:
   Consumer<Integer> c = y -> System.out.println(y);
   ```

8. **عدم استخدام الأقواس للمعاملات المتعددة**:
   ```java
   BiFunction<Integer, Integer, Integer> add = a, b -> a + b;  // Error!

   // Correct:
   BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
   ```

---

## 8. نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- Lambda صيغة مختصرة للدوال المجهولة
- تعمل فقط مع الواجهات الوظيفية (طريقة مجردة واحدة)
- المتغيرات من النطاق المحيط يجب أن تكون final أو effectively final
- 'this' تشير إلى الصنف المحيط، وليس Lambda
- مراجع الطرق هي اختصار لـ Lambda
- تمكّن من البرمجة الوظيفية وتعمل مع Stream API

**ما يجب تجنبه**:
- لا تدّعِ أن Lambda "دائماً أفضل" من الطرق (يعتمد على حالة الاستخدام)
- لا تقل أن Lambda "أصناف جديدة" (تستخدم invokedynamic)
- لا تنسَ قيد final/effectively final
- لا تخلط بين Lambda ومرجع الطريقة (مرجع الطريقة هو اختصار لـ Lambda)

**تحت الضغط**:
- أظهر الصيغة الأساسية: `(params) -> expression`
- اذكر متطلب الواجهة الوظيفية
- أعطِ مثالاً بسيطاً: `Runnable r = () -> System.out.println("Hello")`
- اشرح أنها تحل محل الأصناف الداخلية المجهولة
- اذكر أنها تعمل مع Stream API

**علامات تحذيرية يجب تجنبها**:
- "Lambda هي صنف" (هي دالة مجهولة)
- "يمكنك تعديل المتغيرات المحلية من Lambda" (يجب أن تكون final/effectively final)
- "الواجهة الوظيفية يمكن أن تحتوي على عدة طرق مجردة" (واحدة فقط)
- "Lambda أبطأ من الصنف المجهول" (هي أسرع في الواقع)

**نقاط إضافية**:
- اذكر أن Lambda تستخدم تعليمة invokedynamic bytecode
- ناقش استنتاج النوع وتحديد النوع المستهدف
- أشر إلى مفهوم closure والتقاط المتغيرات
- اذكر أن Lambda يمكنها الوصول إلى متغيرات المثيل والمتغيرات الثابتة بحرية
- ناقش متى تستخدم Lambda مقابل مرجع الطريقة مقابل الطريقة الكاملة
- أشر إلى الواجهات الوظيفية الشائعة: Predicate، Function، Consumer، Supplier
- اذكر أن Lambda تمكّن من أسلوب البرمجة التصريحية

</div>
