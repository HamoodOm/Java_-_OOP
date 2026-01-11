# Optional Class

## 1️⃣ Concept Overview

**Optional** is a container object introduced in Java 8 that may or may not contain a non-null value. It's designed to represent the absence of a value in a more explicit and type-safe way than using `null`.

**Purpose**:
- Prevent `NullPointerException`
- Explicitly represent optional values in APIs
- Make code more readable and maintainable
- Force developers to handle absence of values

**Key Characteristics**:
- **Container object**: Wraps a value that may or may not exist
- **Immutable**: Once created, the wrapped value cannot change
- **Terminal value holder**: Not designed for fields, parameters, or collections
- **API return type**: Best used as method return type to indicate optional result

**Optional is NOT**:
- A replacement for all null references
- Designed for use as class fields
- Designed for use as method parameters
- A collection (don't use in collections)
- Serializable

**Core Philosophy**:
```
Instead of:  String name = getName();  // Might be null
Use:         Optional<String> name = getName();  // Explicitly optional
```

---

## 2️⃣ Why Interviewers Ask This

- **Java 8 feature**: Tests knowledge of modern Java
- **NullPointerException prevention**: Shows understanding of defensive programming
- **API design**: Tests ability to design clean APIs
- **Functional programming**: Optional works well with streams and lambdas
- **Best practices**: Distinguishes those who understand when/how to use Optional
- **Common topic**: Frequently used in production code

---

## 3️⃣ Key Rules / Facts

**Creating Optional Objects**:

| Method | Description | Example |
|--------|-------------|---------|
| Optional.of(value) | Creates Optional with non-null value | `Optional.of("Hello")` |
| Optional.ofNullable(value) | Creates Optional, allows null | `Optional.ofNullable(null)` |
| Optional.empty() | Creates empty Optional | `Optional.empty()` |

**Checking for Values**:

| Method | Return Type | Description |
|--------|-------------|-------------|
| isPresent() | boolean | Returns true if value present |
| isEmpty() | boolean | Returns true if value absent (Java 11+) |

**Getting Values**:

| Method | Description | When to Use |
|--------|-------------|-------------|
| get() | Returns value, throws if absent | Never use! Defeats purpose of Optional |
| orElse(T) | Returns value or default | Default is cheap to create |
| orElseGet(Supplier) | Returns value or supplier result | Default is expensive to create |
| orElseThrow() | Throws if absent | Want to fail fast |
| orElseThrow(Supplier) | Throws custom exception | Need custom exception |

**Transforming Values**:

| Method | Description | Example |
|--------|-------------|---------|
| map(Function) | Transforms value if present | `opt.map(String::toUpperCase)` |
| flatMap(Function) | Flattens nested Optional | `opt.flatMap(this::findAddress)` |
| filter(Predicate) | Filters based on condition | `opt.filter(s -> s.length() > 3)` |

**Performing Actions**:

| Method | Description | Example |
|--------|-------------|---------|
| ifPresent(Consumer) | Performs action if present | `opt.ifPresent(System.out::println)` |
| ifPresentOrElse(Consumer, Runnable) | Action if present, else other action (Java 9+) | - |

**Combining Optionals**:

| Method | Description |
|--------|-------------|
| or(Supplier) | Returns this if present, else supplier result (Java 9+) |
| stream() | Returns Stream with 0 or 1 element (Java 9+) |

**Best Practices**:
- ✅ Use as method return type
- ✅ Use orElse/orElseGet instead of get()
- ✅ Chain operations with map/flatMap/filter
- ✅ Use to avoid NullPointerException
- ❌ Don't use in class fields
- ❌ Don't use as method parameters
- ❌ Don't use in collections
- ❌ Don't use with get() without checking

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Creating Optional Objects

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

### Example 2: Checking and Getting Values

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

### Example 3: Transforming Optional Values

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

### Example 4: ifPresent and Actions

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

### Example 5: Optional with Streams

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

### Example 6: Real-World Example - User Repository

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

### Example 7: Common Anti-Patterns and Solutions

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

### Example 8: Best Practices Examples

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

## 5️⃣ Common Interview Questions

1. **What is Optional and why was it introduced?**
2. **What is the difference between Optional.of() and Optional.ofNullable()?**
3. **Should you use Optional for class fields?**
4. **What is the difference between orElse() and orElseGet()?**
5. **When should you use Optional?**
6. **What is the difference between map() and flatMap() in Optional?**
7. **Is Optional serializable?**
8. **What's wrong with using get() method?**
9. **Can Optional contain null?**
10. **How does Optional prevent NullPointerException?**

---

## 6️⃣ Model Answers

**Q1: What is Optional and why was it introduced?**

"Optional is a container object introduced in Java 8 that may or may not contain a non-null value. It was introduced to address NullPointerException issues and make code more explicit about handling absent values. Instead of returning null which requires null checks everywhere, methods can return Optional to clearly indicate that a value might be absent. This forces developers to consciously handle the absence case, making code more robust. Optional works well with functional programming features like lambdas and streams. However, it's not meant to replace all null references—it's primarily designed as a method return type."

**Q2: What is the difference between Optional.of() and Optional.ofNullable()?**

"Optional.of() creates an Optional with a non-null value and throws NullPointerException if you pass null. Use it when you're certain the value is not null. Optional.ofNullable() creates an Optional that can accept null—if the value is null, it returns Optional.empty(). Use ofNullable() when the value might be null. For example, when wrapping a method that might return null, use ofNullable(). The choice depends on whether you know the value is non-null. Using of() with null is a programming error, while ofNullable() handles null gracefully."

**Q3: Should you use Optional for class fields?**

"No, you should not use Optional for class fields. Optional is not serializable, so it breaks serialization. It adds memory overhead—each Optional is an object wrapper. It makes the code more verbose without significant benefit. Optional is designed specifically as a return type to indicate optional results. For fields, it's better to use null or provide a getter that returns Optional if needed. For example, have a private String field that can be null, and a public Optional<String> getter. This gives you the benefits of Optional for API users without the drawbacks in your class design."

**Q4: What is the difference between orElse() and orElseGet()?**

"Both provide a default value when Optional is empty, but they differ in evaluation. orElse() always evaluates its parameter, even if the Optional contains a value. orElseGet() takes a Supplier and only evaluates it if the Optional is empty. Use orElse() when the default value is cheap to create, like a constant or simple string. Use orElseGet() when creating the default is expensive, like a database call or complex computation. For example, optional.orElse(expensiveMethod()) calls expensiveMethod() even if not needed, but optional.orElseGet(() -> expensiveMethod()) only calls it when necessary."

**Q5: When should you use Optional?**

"Use Optional primarily as a method return type when the result may be absent. This makes the API explicit about optionality and forces callers to handle absence. Use it in Stream operations like findFirst(), reduce(), max(). Don't use it for: class fields (use null instead), method parameters (increases complexity), collection elements (just use null or filter), or primitive types (use OptionalInt, OptionalLong, OptionalDouble). Optional is best for improving API clarity and reducing NullPointerException, not for replacing all nulls. As a rule of thumb, if you find yourself checking isPresent() and calling get(), you're probably misusing it."

**Q6: What is the difference between map() and flatMap() in Optional?**

"map() transforms the value inside Optional using a function that returns a regular value. flatMap() transforms using a function that returns an Optional. The key difference is avoiding nested Optionals. If you have Optional<String> and map it with a function that returns Optional<Address>, you get Optional<Optional<Address>>. With flatMap(), you get Optional<Address>. For example, user.getEmail() returns Optional<String>. Using optional.map(User::getEmail) gives Optional<Optional<String>>, but optional.flatMap(User::getEmail) gives Optional<String>. Use map when your function returns a regular value, flatMap when it returns an Optional."

**Q7: Is Optional serializable?**

"No, Optional is not serializable. This is intentional—Optional is designed as a method return type, not for use in fields or data transfer objects. If you need to serialize a class with optional fields, use nullable fields and provide Optional getters. For example, have private String email that can be null, and public Optional<String> getEmail(). This way the field is serializable but the API returns Optional. Alternatively, mark Optional fields as transient, though this violates the guideline of not using Optional for fields in the first place."

**Q8: What's wrong with using get() method?**

"The get() method defeats the purpose of Optional because it throws NoSuchElementException if the Optional is empty, similar to NullPointerException. If you're checking isPresent() before calling get(), you're not using Optional properly—just use orElse(), orElseGet(), or ifPresent() instead. The pattern 'if (optional.isPresent()) { optional.get() }' is an anti-pattern that's worse than a null check. Instead of get(), use orElse() for defaults, orElseThrow() to fail explicitly, or ifPresent() for actions. The only acceptable use of get() is when you're absolutely certain the value is present, which is rare."

**Q9: Can Optional contain null?**

"No, a properly created Optional cannot contain null. If you create an Optional with Optional.of(null), it throws NullPointerException. Optional.empty() is how you represent absence—it doesn't contain null, it's empty. Optional.ofNullable(null) returns Optional.empty(), not an Optional containing null. The value inside an Optional is either present and non-null, or absent. This is by design—Optional is meant to eliminate null, not wrap it. However, you could theoretically get an Optional<String> and call map with a function that returns null, but this is a programming error."

**Q10: How does Optional prevent NullPointerException?**

"Optional prevents NullPointerException by making absence explicit and forcing you to handle it. Instead of a method returning null which might cause NPE when accessed, it returns Optional.empty(). You can't accidentally use the value without acknowledging it might be absent. Methods like orElse(), orElseGet(), and ifPresent() force you to provide handling for the empty case. The functional methods like map() and filter() do nothing on empty Optionals, preventing NPE. However, Optional doesn't prevent all NPEs—you can still get NPE if you use get() on empty Optional, or if you pass null to of()."

---

## 7️⃣ Common Mistakes

1. **Using get() without checking**:
   ```java
   Optional<String> opt = getOptional();
   String value = opt.get();  // May throw NoSuchElementException!
   
   // Correct:
   String value = opt.orElse("default");
   ```

2. **Checking isPresent() then calling get()**:
   ```java
   // Anti-pattern
   if (opt.isPresent()) {
       System.out.println(opt.get());
   }
   
   // Better:
   opt.ifPresent(System.out::println);
   ```

3. **Using Optional for fields**:
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

4. **Using Optional as method parameter**:
   ```java
   void process(Optional<String> param) { }  // BAD!
   
   // Better:
   void process(String param) {
       Optional.ofNullable(param).ifPresent(...);
   }
   ```

5. **Returning null instead of Optional.empty()**:
   ```java
   Optional<String> find() {
       return null;  // BAD! Defeats purpose
   }
   
   // Correct:
   Optional<String> find() {
       return Optional.empty();
   }
   ```

6. **Using orElse with expensive operation**:
   ```java
   String value = opt.orElse(expensiveCall());  // Always calls!
   
   // Better:
   String value = opt.orElseGet(() -> expensiveCall());
   ```

7. **Creating Optional just to check null**:
   ```java
   Optional.ofNullable(value).isPresent();  // Overkill
   
   // Better:
   value != null;
   ```

8. **Nested Optionals**:
   ```java
   Optional<Optional<String>> nested;  // BAD!
   
   // Use flatMap to avoid nesting
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Optional is a container for possibly absent values
- Use as method return type, not for fields or parameters
- Use orElse/orElseGet instead of get()
- Designed to prevent NullPointerException
- Works well with functional programming (map, flatMap, filter)
- orElseGet is lazy, orElse is eager

**What to avoid**:
- Don't claim Optional eliminates all nulls
- Don't say it's "always better" than null
- Don't advocate using it for fields or parameters
- Don't forget that get() can throw exception

**Under pressure**:
- Explain: "Container that may or may not have a value"
- Give example: `Optional<String> name = findName(id);`
- Mention: `orElse()`, `ifPresent()`, `map()`
- State: "Designed as return type, not for fields"

**Red flags to avoid**:
- "Use Optional everywhere instead of null" (too extreme)
- "Optional makes NPE impossible" (get() can still throw)
- "Optional is serializable" (it's not)
- "Always use get() to access value" (anti-pattern)

**Bonus points**:
- Mention Optional is not Serializable
- Discuss primitive Optionals (OptionalInt, OptionalLong, OptionalDouble)
- Reference that Optional.of(null) throws NPE
- Mention Java 9+ additions (ifPresentOrElse, or, stream)
- Discuss performance implications (object overhead)
- Reference that Brian Goetz says Optional primarily for return types
- Mention that Optional in collections is an anti-pattern
