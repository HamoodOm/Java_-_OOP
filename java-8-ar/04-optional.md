<div dir="rtl">

# فئة Optional

## 1️⃣ نظرة عامة على المفهوم

**Optional** هو كائن حاوية تم تقديمه في Java 8 قد يحتوي أو لا يحتوي على قيمة غير null. صُمم لتمثيل غياب القيمة بطريقة أكثر وضوحاً وأماناً من حيث النوع من استخدام `null`.

**الغرض**:
- منع `NullPointerException`
- تمثيل القيم الاختيارية بشكل صريح في الواجهات البرمجية
- جعل الكود أكثر قابلية للقراءة والصيانة
- إجبار المطورين على التعامل مع غياب القيم

**الخصائص الرئيسية**:
- **كائن حاوية**: يغلف قيمة قد تكون موجودة أو لا
- **غير قابل للتغيير**: بمجرد إنشائه، لا يمكن تغيير القيمة المغلفة
- **حامل قيمة نهائية**: ليس مصمماً للحقول أو المعاملات أو المجموعات
- **نوع إرجاع للواجهة البرمجية**: يُستخدم بشكل أفضل كنوع إرجاع للدوال للإشارة إلى نتيجة اختيارية

**Optional ليس**:
- بديلاً لكل مراجع null
- مصمماً للاستخدام كحقول في الفئات
- مصمماً للاستخدام كمعاملات للدوال
- مجموعة (لا تستخدمه في المجموعات)
- قابلاً للتسلسل (Serializable)

**الفلسفة الأساسية**:
```
Instead of:  String name = getName();  // Might be null
Use:         Optional<String> name = getName();  // Explicitly optional
```

---

## 2️⃣ لماذا يسأل المحاورون عن هذا الموضوع

- **ميزة Java 8**: يختبر معرفة Java الحديثة
- **منع NullPointerException**: يُظهر فهم البرمجة الدفاعية
- **تصميم الواجهات البرمجية**: يختبر القدرة على تصميم واجهات نظيفة
- **البرمجة الوظيفية**: Optional يعمل جيداً مع streams و lambdas
- **أفضل الممارسات**: يميز من يفهم متى وكيف يستخدم Optional
- **موضوع شائع**: يُستخدم بكثرة في كود الإنتاج

---

## 3️⃣ القواعد والحقائق الأساسية

**إنشاء كائنات Optional**:

| الدالة | الوصف | مثال |
|--------|-------|------|
| Optional.of(value) | إنشاء Optional بقيمة غير null | `Optional.of("Hello")` |
| Optional.ofNullable(value) | إنشاء Optional، يسمح بـ null | `Optional.ofNullable(null)` |
| Optional.empty() | إنشاء Optional فارغ | `Optional.empty()` |

**التحقق من القيم**:

| الدالة | نوع الإرجاع | الوصف |
|--------|-------------|-------|
| isPresent() | boolean | تعيد true إذا كانت القيمة موجودة |
| isEmpty() | boolean | تعيد true إذا كانت القيمة غائبة (Java 11+) |

**الحصول على القيم**:

| الدالة | الوصف | متى تُستخدم |
|--------|-------|-------------|
| get() | تعيد القيمة، تطرح استثناء إذا غائبة | لا تستخدمها أبداً! تهزم غرض Optional |
| orElse(T) | تعيد القيمة أو القيمة الافتراضية | القيمة الافتراضية رخيصة الإنشاء |
| orElseGet(Supplier) | تعيد القيمة أو نتيجة Supplier | القيمة الافتراضية مكلفة الإنشاء |
| orElseThrow() | تطرح استثناء إذا غائبة | تريد الفشل السريع |
| orElseThrow(Supplier) | تطرح استثناء مخصص | تحتاج استثناء مخصص |

**تحويل القيم**:

| الدالة | الوصف | مثال |
|--------|-------|------|
| map(Function) | تحول القيمة إذا موجودة | `opt.map(String::toUpperCase)` |
| flatMap(Function) | تسطح Optional المتداخل | `opt.flatMap(this::findAddress)` |
| filter(Predicate) | تصفي بناءً على شرط | `opt.filter(s -> s.length() > 3)` |

**تنفيذ الإجراءات**:

| الدالة | الوصف | مثال |
|--------|-------|------|
| ifPresent(Consumer) | تنفذ إجراء إذا موجودة | `opt.ifPresent(System.out::println)` |
| ifPresentOrElse(Consumer, Runnable) | إجراء إذا موجودة، وإلا إجراء آخر (Java 9+) | - |

**دمج Optionals**:

| الدالة | الوصف |
|--------|-------|
| or(Supplier) | تعيد هذا إذا موجود، وإلا نتيجة supplier (Java 9+) |
| stream() | تعيد Stream بـ 0 أو 1 عنصر (Java 9+) |

**أفضل الممارسات**:
- ✅ استخدمه كنوع إرجاع للدوال
- ✅ استخدم orElse/orElseGet بدلاً من get()
- ✅ سلسل العمليات مع map/flatMap/filter
- ✅ استخدمه لتجنب NullPointerException
- ❌ لا تستخدمه في حقول الفئات
- ❌ لا تستخدمه كمعاملات للدوال
- ❌ لا تستخدمه في المجموعات
- ❌ لا تستخدم get() بدون التحقق

---

## 4️⃣ أمثلة الكود (Java 8)

### المثال 1: إنشاء كائنات Optional

```java
import java.util.Optional;

public class CreatingOptionals {
    public static void main(String[] args) {
        // 1. Optional.of() - for non-null values
        Optional<String> nonEmpty = Optional.of("Hello");
        System.out.println("Non-empty: " + nonEmpty);  // Optional[Hello]

        // Optional.of() with null throws NullPointerException
        try {
            Optional<String> nullOpt = Optional.of(null);  // NPE!
        } catch (NullPointerException e) {
            System.out.println("Cannot create Optional.of(null)");
        }

        // 2. Optional.ofNullable() - accepts null
        Optional<String> maybeEmpty = Optional.ofNullable("World");
        System.out.println("Maybe empty: " + maybeEmpty);  // Optional[World]

        Optional<String> empty = Optional.ofNullable(null);
        System.out.println("Empty: " + empty);  // Optional.empty

        // 3. Optional.empty() - explicitly empty
        Optional<String> explicitlyEmpty = Optional.empty();
        System.out.println("Explicitly empty: " + explicitlyEmpty);  // Optional.empty

        // When to use which:
        // - Use of() when you're CERTAIN value is not null
        // - Use ofNullable() when value MIGHT be null
        // - Use empty() when you want to return empty Optional explicitly

        // Example: Converting from nullable API
        String nullableValue = getNullableString();  // Might return null
        Optional<String> optional = Optional.ofNullable(nullableValue);
    }

    static String getNullableString() {
        return Math.random() > 0.5 ? "Value" : null;
    }
}
```

### المثال 2: التحقق والحصول على القيم

```java
import java.util.Optional;

public class CheckingAndGetting {
    public static void main(String[] args) {
        Optional<String> present = Optional.of("Hello");
        Optional<String> absent = Optional.empty();

        // 1. isPresent() - check if value exists
        if (present.isPresent()) {
            System.out.println("Value present: " + present.get());
        }

        if (!absent.isPresent()) {
            System.out.println("Value absent");
        }

        // 2. get() - AVOID! Throws NoSuchElementException if empty
        try {
            String value = present.get();  // OK
            System.out.println("Got value: " + value);

            String emptyValue = absent.get();  // Throws exception!
        } catch (java.util.NoSuchElementException e) {
            System.out.println("get() on empty Optional throws exception");
        }

        // 3. orElse() - return default if absent
        String result1 = present.orElse("Default");
        System.out.println("Result1: " + result1);  // Hello

        String result2 = absent.orElse("Default");
        System.out.println("Result2: " + result2);  // Default

        // 4. orElseGet() - use Supplier for default (lazy)
        String result3 = absent.orElseGet(() -> {
            System.out.println("Computing default...");
            return "Computed Default";
        });
        System.out.println("Result3: " + result3);

        // orElse vs orElseGet
        System.out.println("\norElse vs orElseGet:");

        // orElse - ALWAYS evaluates default (even if not needed)
        String r1 = present.orElse(expensiveOperation());  // Calls expensive!

        // orElseGet - Only evaluates if needed (better performance)
        String r2 = present.orElseGet(() -> expensiveOperation());  // Not called

        // 5. orElseThrow() - throw exception if absent
        try {
            String value = present.orElseThrow();  // OK in Java 10+
            System.out.println("Value: " + value);

            String emptyValue = absent.orElseThrow();  // Throws
        } catch (java.util.NoSuchElementException e) {
            System.out.println("orElseThrow() throws exception on empty");
        }

        // 6. orElseThrow(Supplier) - custom exception
        try {
            String value = absent.orElseThrow(
                () -> new IllegalStateException("Value not found!")
            );
        } catch (IllegalStateException e) {
            System.out.println("Custom exception: " + e.getMessage());
        }
    }

    static String expensiveOperation() {
        System.out.println("Expensive operation called!");
        return "Expensive Result";
    }
}
```

### المثال 3: تحويل قيم Optional

```java
import java.util.Optional;

public class TransformingOptionals {
    public static void main(String[] args) {
        // 1. map() - transform value if present
        Optional<String> name = Optional.of("Alice");

        Optional<String> upperName = name.map(String::toUpperCase);
        System.out.println("Upper: " + upperName);  // Optional[ALICE]

        Optional<Integer> length = name.map(String::length);
        System.out.println("Length: " + length);  // Optional[5]

        // map on empty Optional
        Optional<String> empty = Optional.empty();
        Optional<Integer> emptyLength = empty.map(String::length);
        System.out.println("Empty length: " + emptyLength);  // Optional.empty

        // 2. Chaining map operations
        Optional<Integer> result = Optional.of("Hello")
            .map(String::toUpperCase)      // Optional[HELLO]
            .map(s -> s.replace('L', '*')) // Optional[HE**O]
            .map(String::length);          // Optional[5]
        System.out.println("Chained result: " + result);

        // 3. filter() - filter based on predicate
        Optional<String> longName = Optional.of("Alexander")
            .filter(s -> s.length() > 5);
        System.out.println("Long name: " + longName);  // Optional[Alexander]

        Optional<String> shortName = Optional.of("Bob")
            .filter(s -> s.length() > 5);
        System.out.println("Short name: " + shortName);  // Optional.empty

        // 4. flatMap() - avoid nested Optionals
        Optional<String> username = Optional.of("john_doe");

        // Without flatMap - returns Optional<Optional<User>>
        // Optional<Optional<User>> nestedUser = username.map(this::findUser);

        // With flatMap - returns Optional<User>
        Optional<User> user = username.flatMap(this::findUser);
        System.out.println("User: " + user);

        // Chaining flatMap
        Optional<String> email = Optional.of("john_doe")
            .flatMap(this::findUser)
            .flatMap(u -> u.getEmail());
        email.ifPresent(e -> System.out.println("Email: " + e));

        // 5. Complex transformation pipeline
        Optional<String> processedValue = Optional.of("  Hello World  ")
            .map(String::trim)                    // Remove whitespace
            .filter(s -> !s.isEmpty())            // Check not empty
            .map(String::toLowerCase)             // Convert to lowercase
            .filter(s -> s.length() > 5)          // Must be long enough
            .map(s -> s.replace(' ', '_'));       // Replace spaces

        System.out.println("Processed: " + processedValue);  // Optional[hello_world]
    }

    Optional<User> findUser(String username) {
        if (username.equals("john_doe")) {
            return Optional.of(new User("John", "john@example.com"));
        }
        return Optional.empty();
    }

    static class User {
        private String name;
        private String email;

        User(String name, String email) {
            this.name = name;
            this.email = email;
        }

        Optional<String> getEmail() {
            return Optional.ofNullable(email);
        }

        @Override
        public String toString() {
            return "User{name='" + name + "', email='" + email + "'}";
        }
    }
}
```

### المثال 4: ifPresent والإجراءات

```java
import java.util.Optional;

public class OptionalActions {
    public static void main(String[] args) {
        Optional<String> present = Optional.of("Hello");
        Optional<String> absent = Optional.empty();

        // 1. ifPresent(Consumer) - perform action if present
        present.ifPresent(value -> System.out.println("Value: " + value));
        absent.ifPresent(value -> System.out.println("Won't print"));

        // 2. Method reference with ifPresent
        present.ifPresent(System.out::println);

        // 3. Multiple actions
        present.ifPresent(value -> {
            System.out.println("Processing: " + value);
            System.out.println("Length: " + value.length());
            System.out.println("Upper: " + value.toUpperCase());
        });

        // 4. Old style vs Optional style

        // OLD: Null check
        String oldValue = getValue();
        if (oldValue != null) {
            System.out.println(oldValue);
        }

        // NEW: Optional
        getOptionalValue().ifPresent(System.out::println);

        // 5. OLD: Null check with else
        String value = getValue();
        if (value != null) {
            System.out.println("Found: " + value);
        } else {
            System.out.println("Not found");
        }

        // NEW: Optional with orElse
        String result = getOptionalValue()
            .map(v -> "Found: " + v)
            .orElse("Not found");
        System.out.println(result);

        // 6. Conditional execution
        Optional<Integer> number = Optional.of(42);

        number.filter(n -> n > 10)
              .ifPresent(n -> System.out.println("Number is greater than 10: " + n));

        // 7. Side effects with transformation
        Optional<String> processed = Optional.of("data")
            .map(s -> {
                System.out.println("Processing: " + s);  // Side effect
                return s.toUpperCase();
            });
        processed.ifPresent(System.out::println);
    }

    static String getValue() {
        return Math.random() > 0.5 ? "Value" : null;
    }

    static Optional<String> getOptionalValue() {
        return Math.random() > 0.5 ? Optional.of("Value") : Optional.empty();
    }
}
```

### المثال 5: Optional مع Streams

```java
import java.util.*;
import java.util.stream.*;

public class OptionalWithStreams {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

        // 1. findFirst() returns Optional
        Optional<String> first = names.stream()
            .filter(s -> s.startsWith("C"))
            .findFirst();

        first.ifPresent(name -> System.out.println("First C name: " + name));

        // 2. findAny() returns Optional
        Optional<String> any = names.stream()
            .filter(s -> s.length() > 3)
            .findAny();

        any.ifPresent(name -> System.out.println("Any long name: " + name));

        // 3. reduce() returns Optional
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        Optional<Integer> sum = numbers.stream()
            .reduce((a, b) -> a + b);

        sum.ifPresent(s -> System.out.println("Sum: " + s));

        // 4. max() and min() return Optional
        Optional<Integer> max = numbers.stream()
            .max(Integer::compare);

        Optional<Integer> min = numbers.stream()
            .min(Integer::compare);

        System.out.println("Max: " + max.orElse(0));
        System.out.println("Min: " + min.orElse(0));

        // 5. Filtering out empty Optionals
        List<Optional<String>> optionals = Arrays.asList(
            Optional.of("A"),
            Optional.empty(),
            Optional.of("B"),
            Optional.empty(),
            Optional.of("C")
        );

        List<String> values = optionals.stream()
            .filter(Optional::isPresent)
            .map(Optional::get)
            .collect(Collectors.toList());

        System.out.println("Values: " + values);  // [A, B, C]

        // Better in Java 9+ with flatMap and stream()
        // List<String> values = optionals.stream()
        //     .flatMap(Optional::stream)
        //     .collect(Collectors.toList());

        // 6. Collecting to Optional
        List<String> list = Arrays.asList("Apple", "Banana", "Cherry");

        Optional<String> longest = list.stream()
            .max(Comparator.comparing(String::length));

        longest.ifPresent(l -> System.out.println("Longest: " + l));

        // 7. FlatMapping Optionals
        class Person {
            String name;
            Optional<String> email;

            Person(String name, String email) {
                this.name = name;
                this.email = Optional.ofNullable(email);
            }
        }

        List<Person> people = Arrays.asList(
            new Person("Alice", "alice@example.com"),
            new Person("Bob", null),
            new Person("Charlie", "charlie@example.com")
        );

        // Get all emails (filtering out people without email)
        List<String> emails = people.stream()
            .map(p -> p.email)
            .filter(Optional::isPresent)
            .map(Optional::get)
            .collect(Collectors.toList());

        System.out.println("Emails: " + emails);
    }
}
```

### المثال 6: مثال واقعي - مستودع المستخدمين

```java
import java.util.*;

class User {
    private Long id;
    private String username;
    private String email;
    private String phone;

    public User(Long id, String username, String email, String phone) {
        this.id = id;
        this.username = username;
        this.email = email;
        this.phone = phone;
    }

    public Long getId() { return id; }
    public String getUsername() { return username; }
    public Optional<String> getEmail() { return Optional.ofNullable(email); }
    public Optional<String> getPhone() { return Optional.ofNullable(phone); }

    @Override
    public String toString() {
        return "User{id=" + id + ", username='" + username + "'}";
    }
}

class UserRepository {
    private Map<Long, User> users = new HashMap<>();

    public UserRepository() {
        users.put(1L, new User(1L, "alice", "alice@example.com", "123-456"));
        users.put(2L, new User(2L, "bob", null, "789-012"));
        users.put(3L, new User(3L, "charlie", "charlie@example.com", null));
    }

    // Returns Optional instead of null
    public Optional<User> findById(Long id) {
        return Optional.ofNullable(users.get(id));
    }

    public Optional<User> findByUsername(String username) {
        return users.values().stream()
            .filter(u -> u.getUsername().equals(username))
            .findFirst();
    }

    public Optional<String> getEmailById(Long id) {
        return findById(id)
            .flatMap(User::getEmail);
    }
}

public class UserRepositoryExample {
    public static void main(String[] args) {
        UserRepository repo = new UserRepository();

        // 1. Finding user by ID
        System.out.println("=== Finding Users ===");
        Optional<User> user1 = repo.findById(1L);
        user1.ifPresent(u -> System.out.println("Found: " + u));

        Optional<User> user999 = repo.findById(999L);
        System.out.println("User 999 present: " + user999.isPresent());  // false

        // 2. Getting user or default
        User user = repo.findById(999L)
            .orElse(new User(0L, "guest", null, null));
        System.out.println("User or default: " + user);

        // 3. Throwing exception if not found
        try {
            User mustExist = repo.findById(999L)
                .orElseThrow(() -> new IllegalArgumentException("User not found"));
        } catch (IllegalArgumentException e) {
            System.out.println("Exception: " + e.getMessage());
        }

        // 4. Chaining operations
        System.out.println("\n=== Chaining Operations ===");
        String email = repo.findById(1L)
            .flatMap(User::getEmail)
            .map(String::toUpperCase)
            .orElse("NO EMAIL");
        System.out.println("Email: " + email);  // ALICE@EXAMPLE.COM

        // User without email
        String noEmail = repo.findById(2L)
            .flatMap(User::getEmail)
            .orElse("NO EMAIL");
        System.out.println("Bob's email: " + noEmail);  // NO EMAIL

        // 5. Complex business logic
        System.out.println("\n=== Complex Logic ===");

        // Get email or phone
        String contact = repo.findById(2L)
            .flatMap(u -> u.getEmail()
                .map(e -> "Email: " + e)
                .or(() -> u.getPhone().map(p -> "Phone: " + p)))
            .orElse("No contact info");
        System.out.println("Bob's contact: " + contact);  // Phone: 789-012

        // 6. Validation with filter
        System.out.println("\n=== Validation ===");
        Optional<User> validUser = repo.findByUsername("alice")
            .filter(u -> u.getEmail().isPresent());
        validUser.ifPresent(u -> System.out.println("Valid user: " + u));

        Optional<User> invalidUser = repo.findByUsername("charlie")
            .filter(u -> u.getPhone().isPresent());
        System.out.println("Charlie has phone: " + invalidUser.isPresent());  // false

        // 7. Multiple Optional operations
        System.out.println("\n=== Multiple Operations ===");

        // Process only users with email
        Arrays.asList(1L, 2L, 3L).stream()
            .map(repo::findById)
            .filter(Optional::isPresent)
            .map(Optional::get)
            .filter(u -> u.getEmail().isPresent())
            .forEach(u -> System.out.println(u.getUsername() + " has email"));

        // 8. Conditional actions
        System.out.println("\n=== Conditional Actions ===");
        repo.findById(1L)
            .flatMap(User::getEmail)
            .ifPresent(email -> sendEmail(email));

        repo.findById(2L)
            .flatMap(User::getEmail)
            .ifPresent(email -> sendEmail(email));  // Won't execute
    }

    static void sendEmail(String email) {
        System.out.println("Sending email to: " + email);
    }
}
```

### المثال 7: الأنماط المضادة الشائعة والحلول

```java
import java.util.Optional;

public class OptionalAntiPatterns {
    public static void main(String[] args) {
        // ANTI-PATTERN 1: Using get() without checking
        Optional<String> optional = Optional.of("Hello");

        // BAD
        if (optional.isPresent()) {
            String value = optional.get();
            System.out.println(value);
        }

        // GOOD
        optional.ifPresent(System.out::println);

        // ANTI-PATTERN 2: Using Optional for fields
        class BadPerson {
            Optional<String> name;  // BAD! Don't use Optional for fields
        }

        class GoodPerson {
            String name;  // Can be null

            Optional<String> getName() {  // GOOD! Return Optional
                return Optional.ofNullable(name);
            }
        }

        // ANTI-PATTERN 3: Using Optional for method parameters
        // BAD
        void badMethod(Optional<String> param) {
            param.ifPresent(System.out::println);
        }

        // GOOD
        void goodMethod(String param) {
            Optional.ofNullable(param).ifPresent(System.out::println);
        }

        // ANTI-PATTERN 4: Creating Optional just to check null
        String value = "test";

        // BAD
        Optional.ofNullable(value).ifPresent(System.out::println);

        // GOOD
        if (value != null) {
            System.out.println(value);
        }

        // ANTI-PATTERN 5: Returning null from Optional method
        // BAD
        Optional<String> badGetValue() {
            return null;  // Defeats purpose of Optional!
        }

        // GOOD
        Optional<String> goodGetValue() {
            return Optional.empty();
        }

        // ANTI-PATTERN 6: Using orElse with expensive operation
        // BAD - expensive() called even when not needed
        String result1 = optional.orElse(expensiveOperation());

        // GOOD - only called when needed
        String result2 = optional.orElseGet(() -> expensiveOperation());

        // ANTI-PATTERN 7: Nested Optional
        // BAD
        Optional<Optional<String>> nested = Optional.of(Optional.of("Value"));

        // GOOD - use flatMap
        Optional<String> flat = Optional.of("Value");

        // ANTI-PATTERN 8: Optional in collections
        // BAD
        java.util.List<Optional<String>> badList = java.util.Arrays.asList(
            Optional.of("A"),
            Optional.empty(),
            Optional.of("B")
        );

        // GOOD - just use null or filter out nulls
        java.util.List<String> goodList = java.util.Arrays.asList("A", null, "B");
        goodList = goodList.stream()
            .filter(s -> s != null)
            .collect(java.util.stream.Collectors.toList());

        // ANTI-PATTERN 9: Optional.of() with possibly null value
        String possiblyNull = getValue();

        // BAD - throws NPE if null
        // Optional<String> bad = Optional.of(possiblyNull);

        // GOOD
        Optional<String> good = Optional.ofNullable(possiblyNull);

        // ANTI-PATTERN 10: Returning Optional<Collection>
        // BAD
        Optional<java.util.List<String>> badGetList() {
            return Optional.of(java.util.Collections.emptyList());
        }

        // GOOD - return empty collection instead
        java.util.List<String> goodGetList() {
            return java.util.Collections.emptyList();
        }
    }

    static String expensiveOperation() {
        System.out.println("Expensive operation!");
        return "Result";
    }

    static String getValue() {
        return Math.random() > 0.5 ? "Value" : null;
    }
}
```

### المثال 8: أمثلة أفضل الممارسات

```java
import java.util.*;
import java.util.stream.*;

public class OptionalBestPractices {
    public static void main(String[] args) {
        // BEST PRACTICE 1: Use as return type
        Optional<String> result = findUserName(1L);
        result.ifPresent(System.out::println);

        // BEST PRACTICE 2: Chain operations
        String processedName = findUserName(1L)
            .map(String::toUpperCase)
            .filter(s -> s.length() > 3)
            .orElse("UNKNOWN");
        System.out.println("Processed: " + processedName);

        // BEST PRACTICE 3: Avoid get(), use orElse family
        // String name = findUserName(1L).get();  // BAD
        String name = findUserName(1L).orElse("Default");  // GOOD

        // BEST PRACTICE 4: Use orElseGet for expensive defaults
        String expensiveName = findUserName(999L)
            .orElseGet(() -> {
                // This only executes if Optional is empty
                return loadDefaultFromDatabase();
            });

        // BEST PRACTICE 5: Use orElseThrow for must-exist scenarios
        try {
            String mustExist = findUserName(999L)
                .orElseThrow(() -> new IllegalArgumentException("User not found"));
        } catch (IllegalArgumentException e) {
            System.out.println("Exception as expected");
        }

        // BEST PRACTICE 6: Filter before transforming
        Optional<Integer> age = findUserAge(1L)
            .filter(a -> a >= 18)  // Only process adults
            .map(a -> a * 12);     // Convert to months

        // BEST PRACTICE 7: Use flatMap to avoid nested Optionals
        Optional<String> email = findUser(1L)
            .flatMap(User::getEmail);  // User::getEmail returns Optional

        // BEST PRACTICE 8: Provide meaningful defaults
        String displayName = findUserName(1L)
            .orElse("Guest User");

        // BEST PRACTICE 9: Use ifPresent for side effects
        findUserName(1L)
            .ifPresent(n -> logUserAccess(n));

        // BEST PRACTICE 10: Combine with streams
        List<User> users = Arrays.asList(
            new User(1L, "alice", "alice@example.com", null),
            new User(2L, "bob", null, null)
        );

        List<String> emails = users.stream()
            .map(User::getEmail)
            .filter(Optional::isPresent)
            .map(Optional::get)
            .collect(Collectors.toList());

        System.out.println("Emails: " + emails);
    }

    static Optional<String> findUserName(Long id) {
        return id == 1L ? Optional.of("Alice") : Optional.empty();
    }

    static Optional<Integer> findUserAge(Long id) {
        return id == 1L ? Optional.of(25) : Optional.empty();
    }

    static Optional<User> findUser(Long id) {
        return id == 1L ?
            Optional.of(new User(1L, "alice", "alice@example.com", null)) :
            Optional.empty();
    }

    static String loadDefaultFromDatabase() {
        System.out.println("Loading default from database...");
        return "Default User";
    }

    static void logUserAccess(String username) {
        System.out.println("User accessed: " + username);
    }

    static class User {
        Long id;
        String username;
        String email;
        String phone;

        User(Long id, String username, String email, String phone) {
            this.id = id;
            this.username = username;
            this.email = email;
            this.phone = phone;
        }

        Optional<String> getEmail() {
            return Optional.ofNullable(email);
        }
    }
}
```

---

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما هو Optional ولماذا تم تقديمه؟**
2. **ما الفرق بين Optional.of() و Optional.ofNullable()؟**
3. **هل يجب استخدام Optional لحقول الفئات؟**
4. **ما الفرق بين orElse() و orElseGet()؟**
5. **متى يجب استخدام Optional؟**
6. **ما الفرق بين map() و flatMap() في Optional؟**
7. **هل Optional قابل للتسلسل؟**
8. **ما المشكلة في استخدام دالة get()؟**
9. **هل يمكن أن يحتوي Optional على null؟**
10. **كيف يمنع Optional الـ NullPointerException؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما هو Optional ولماذا تم تقديمه؟**

"Optional هو كائن حاوية تم تقديمه في Java 8 قد يحتوي أو لا يحتوي على قيمة غير null. تم تقديمه لمعالجة مشاكل NullPointerException وجعل الكود أكثر وضوحاً في التعامل مع القيم الغائبة. بدلاً من إرجاع null الذي يتطلب فحوصات null في كل مكان، يمكن للدوال إرجاع Optional للإشارة بوضوح إلى أن القيمة قد تكون غائبة. هذا يجبر المطورين على التعامل بوعي مع حالة الغياب، مما يجعل الكود أكثر متانة. يعمل Optional جيداً مع ميزات البرمجة الوظيفية مثل lambdas و streams. ومع ذلك، ليس المقصود منه استبدال كل مراجع null - إنه مصمم أساساً كنوع إرجاع للدوال."

**س2: ما الفرق بين Optional.of() و Optional.ofNullable()؟**

"Optional.of() تنشئ Optional بقيمة غير null وتطرح NullPointerException إذا مررت null. استخدمها عندما تكون متأكداً أن القيمة ليست null. Optional.ofNullable() تنشئ Optional يمكنه قبول null - إذا كانت القيمة null، تعيد Optional.empty(). استخدم ofNullable() عندما قد تكون القيمة null. على سبيل المثال، عند تغليف دالة قد تعيد null، استخدم ofNullable(). الاختيار يعتمد على ما إذا كنت تعرف أن القيمة غير null. استخدام of() مع null هو خطأ برمجي، بينما ofNullable() تتعامل مع null بسلاسة."

**س3: هل يجب استخدام Optional لحقول الفئات؟**

"لا، لا يجب استخدام Optional لحقول الفئات. Optional غير قابل للتسلسل، لذا يكسر التسلسل. يضيف overhead للذاكرة - كل Optional هو غلاف كائن. يجعل الكود أكثر تفصيلاً دون فائدة كبيرة. Optional مصمم خصيصاً كنوع إرجاع للإشارة إلى نتائج اختيارية. للحقول، من الأفضل استخدام null أو توفير getter يعيد Optional إذا لزم الأمر. على سبيل المثال، اجعل حقل String خاص يمكن أن يكون null، و getter عام يعيد Optional<String>. هذا يمنحك فوائد Optional لمستخدمي الواجهة البرمجية دون العيوب في تصميم فئتك."

**س4: ما الفرق بين orElse() و orElseGet()؟**

"كلاهما يوفر قيمة افتراضية عندما يكون Optional فارغاً، لكنهما يختلفان في التقييم. orElse() دائماً تقيّم معاملها، حتى لو احتوى Optional على قيمة. orElseGet() تأخذ Supplier وتقيّمه فقط إذا كان Optional فارغاً. استخدم orElse() عندما تكون القيمة الافتراضية رخيصة الإنشاء، مثل ثابت أو نص بسيط. استخدم orElseGet() عندما يكون إنشاء الافتراضي مكلفاً، مثل استدعاء قاعدة بيانات أو حساب معقد. على سبيل المثال، optional.orElse(expensiveMethod()) تستدعي expensiveMethod() حتى لو لم تكن هناك حاجة، لكن optional.orElseGet(() -> expensiveMethod()) تستدعيها فقط عند الضرورة."

**س5: متى يجب استخدام Optional؟**

"استخدم Optional أساساً كنوع إرجاع للدوال عندما قد تكون النتيجة غائبة. هذا يجعل الواجهة البرمجية صريحة بشأن الاختيارية ويجبر المستدعين على التعامل مع الغياب. استخدمه في عمليات Stream مثل findFirst(), reduce(), max(). لا تستخدمه لـ: حقول الفئات (استخدم null بدلاً)، معاملات الدوال (تزيد التعقيد)، عناصر المجموعات (فقط استخدم null أو صفّ)، أو الأنواع البدائية (استخدم OptionalInt, OptionalLong, OptionalDouble). Optional أفضل لتحسين وضوح الواجهة البرمجية وتقليل NullPointerException، وليس لاستبدال كل nulls. كقاعدة عامة، إذا وجدت نفسك تتحقق من isPresent() وتستدعي get()، فأنت على الأرجح تسيء استخدامه."

**س6: ما الفرق بين map() و flatMap() في Optional؟**

"map() تحول القيمة داخل Optional باستخدام دالة تعيد قيمة عادية. flatMap() تحول باستخدام دالة تعيد Optional. الفرق الرئيسي هو تجنب Optional المتداخل. إذا كان لديك Optional<String> وعيّنته بدالة تعيد Optional<Address>، تحصل على Optional<Optional<Address>>. مع flatMap()، تحصل على Optional<Address>. على سبيل المثال، user.getEmail() تعيد Optional<String>. استخدام optional.map(User::getEmail) يعطي Optional<Optional<String>>، لكن optional.flatMap(User::getEmail) يعطي Optional<String>. استخدم map عندما تعيد دالتك قيمة عادية، flatMap عندما تعيد Optional."

**س7: هل Optional قابل للتسلسل؟**

"لا، Optional غير قابل للتسلسل. هذا مقصود - Optional مصمم كنوع إرجاع، وليس للاستخدام في الحقول أو كائنات نقل البيانات. إذا كنت بحاجة لتسلسل فئة بحقول اختيارية، استخدم حقول قابلة لـ null ووفر getters تعيد Optional. على سبيل المثال، اجعل private String email يمكن أن يكون null، و public Optional<String> getEmail(). بهذه الطريقة الحقل قابل للتسلسل لكن الواجهة البرمجية تعيد Optional. بديلاً، علّم حقول Optional كـ transient، رغم أن هذا يخالف التوجيه بعدم استخدام Optional للحقول من الأساس."

**س8: ما المشكلة في استخدام دالة get()؟**

"دالة get() تهزم غرض Optional لأنها تطرح NoSuchElementException إذا كان Optional فارغاً، مشابه لـ NullPointerException. إذا كنت تتحقق من isPresent() قبل استدعاء get()، فأنت لا تستخدم Optional بشكل صحيح - فقط استخدم orElse(), orElseGet(), أو ifPresent() بدلاً. النمط 'if (optional.isPresent()) { optional.get() }' هو نمط مضاد أسوأ من فحص null. بدلاً من get()، استخدم orElse() للافتراضيات، orElseThrow() للفشل الصريح، أو ifPresent() للإجراءات. الاستخدام المقبول الوحيد لـ get() هو عندما تكون متأكداً تماماً أن القيمة موجودة، وهو أمر نادر."

**س9: هل يمكن أن يحتوي Optional على null؟**

"لا، Optional المنشأ بشكل صحيح لا يمكن أن يحتوي على null. إذا أنشأت Optional بـ Optional.of(null)، يطرح NullPointerException. Optional.empty() هي كيفية تمثيل الغياب - إنها لا تحتوي على null، إنها فارغة. Optional.ofNullable(null) تعيد Optional.empty()، وليس Optional يحتوي null. القيمة داخل Optional إما موجودة وغير null، أو غائبة. هذا بالتصميم - Optional مقصود لإزالة null، وليس لتغليفه. ومع ذلك، يمكنك نظرياً الحصول على Optional<String> واستدعاء map بدالة تعيد null، لكن هذا خطأ برمجي."

**س10: كيف يمنع Optional الـ NullPointerException؟**

"Optional يمنع NullPointerException بجعل الغياب صريحاً وإجبارك على التعامل معه. بدلاً من دالة تعيد null التي قد تسبب NPE عند الوصول، تعيد Optional.empty(). لا يمكنك استخدام القيمة عرضياً دون الاعتراف بأنها قد تكون غائبة. دوال مثل orElse(), orElseGet(), و ifPresent() تجبرك على توفير معالجة لحالة الفراغ. الدوال الوظيفية مثل map() و filter() لا تفعل شيئاً على Optional الفارغ، مانعة NPE. ومع ذلك، Optional لا يمنع كل NPEs - لا يزال بإمكانك الحصول على NPE إذا استخدمت get() على Optional فارغ، أو إذا مررت null لـ of()."

---

## 7️⃣ الأخطاء الشائعة

1. **استخدام get() بدون التحقق**:
   ```java
   Optional<String> opt = getOptional();
   String value = opt.get();  // May throw NoSuchElementException!

   // Correct:
   String value = opt.orElse("default");
   ```

2. **التحقق من isPresent() ثم استدعاء get()**:
   ```java
   // Anti-pattern
   if (opt.isPresent()) {
       System.out.println(opt.get());
   }

   // Better:
   opt.ifPresent(System.out::println);
   ```

3. **استخدام Optional للحقول**:
   ```java
   class Person {
       Optional<String> name;  // BAD!
   }

   // Better:
   class Person {
       String name;  // Can be null

       Optional<String> getName() {
           return Optional.ofNullable(name);
       }
   }
   ```

4. **استخدام Optional كمعامل للدالة**:
   ```java
   void process(Optional<String> param) { }  // BAD!

   // Better:
   void process(String param) {
       Optional.ofNullable(param).ifPresent(...);
   }
   ```

5. **إرجاع null بدلاً من Optional.empty()**:
   ```java
   Optional<String> find() {
       return null;  // BAD! Defeats purpose
   }

   // Correct:
   Optional<String> find() {
       return Optional.empty();
   }
   ```

6. **استخدام orElse مع عملية مكلفة**:
   ```java
   String value = opt.orElse(expensiveCall());  // Always calls!

   // Better:
   String value = opt.orElseGet(() -> expensiveCall());
   ```

7. **إنشاء Optional فقط للتحقق من null**:
   ```java
   Optional.ofNullable(value).isPresent();  // Overkill

   // Better:
   value != null;
   ```

8. **Optional متداخل**:
   ```java
   Optional<Optional<String>> nested;  // BAD!

   // Use flatMap to avoid nesting
   ```

---

## 8️⃣ نصائح المقابلات الفعلية

**ما يجب التأكيد عليه**:
- Optional هو حاوية لقيم قد تكون غائبة
- استخدمه كنوع إرجاع للدوال، وليس للحقول أو المعاملات
- استخدم orElse/orElseGet بدلاً من get()
- مصمم لمنع NullPointerException
- يعمل جيداً مع البرمجة الوظيفية (map, flatMap, filter)
- orElseGet كسولة، orElse نشطة

**ما يجب تجنبه**:
- لا تدعي أن Optional يزيل كل nulls
- لا تقل إنه "دائماً أفضل" من null
- لا تدافع عن استخدامه للحقول أو المعاملات
- لا تنسَ أن get() يمكن أن تطرح استثناء

**تحت الضغط**:
- اشرح: "حاوية قد تحتوي أو لا تحتوي على قيمة"
- أعطِ مثالاً: `Optional<String> name = findName(id);`
- اذكر: `orElse()`, `ifPresent()`, `map()`
- قل: "مصمم كنوع إرجاع، وليس للحقول"

**علامات حمراء يجب تجنبها**:
- "استخدم Optional في كل مكان بدلاً من null" (متطرف جداً)
- "Optional يجعل NPE مستحيلاً" (get() لا تزال يمكن أن تطرح)
- "Optional قابل للتسلسل" (ليس كذلك)
- "دائماً استخدم get() للوصول للقيمة" (نمط مضاد)

**نقاط إضافية**:
- اذكر أن Optional غير قابل للتسلسل (Serializable)
- ناقش Optional البدائية (OptionalInt, OptionalLong, OptionalDouble)
- أشر إلى أن Optional.of(null) تطرح NPE
- اذكر إضافات Java 9+ (ifPresentOrElse, or, stream)
- ناقش تأثيرات الأداء (overhead الكائن)
- أشر إلى أن Brian Goetz يقول Optional أساساً لأنواع الإرجاع
- اذكر أن Optional في المجموعات نمط مضاد

</div>
