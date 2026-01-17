<div dir="rtl">

# معالجة الاستثناءات (Exception Handling)

## 1️⃣ نظرة عامة على المفهوم

**الاستثناء (Exception)** هو حدث غير متوقع يحدث أثناء تنفيذ البرنامج ويعطل التدفق الطبيعي للتعليمات. معالجة الاستثناءات هي آلية للتعامل مع أخطاء وقت التشغيل والحفاظ على التدفق الطبيعي للتطبيق.

**التسلسل الهرمي للاستثناءات**:
```
Throwable
├── Error (JVM errors - not to be handled)
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   └── VirtualMachineError
└── Exception (Application errors - should be handled)
    ├── Checked Exceptions (compile-time checking)
    │   ├── IOException
    │   ├── SQLException
    │   ├── ClassNotFoundException
    │   └── FileNotFoundException
    └── RuntimeException (Unchecked Exceptions)
        ├── NullPointerException
        ├── ArrayIndexOutOfBoundsException
        ├── ArithmeticException
        ├── IllegalArgumentException
        └── NumberFormatException
```

**نوعان من الاستثناءات**:

1. **الاستثناءات المُراقَبة (Checked Exceptions)**:
   - تُفحص في وقت الترجمة
   - يجب معالجتها أو الإعلان عنها باستخدام `throws`
   - تمتد من فئة `Exception` (لكن ليس `RuntimeException`)
   - أمثلة: IOException، SQLException

2. **الاستثناءات غير المُراقَبة (Unchecked/Runtime Exceptions)**:
   - تُفحص في وقت التشغيل
   - ليس مطلوباً معالجتها
   - تمتد من فئة `RuntimeException`
   - أمثلة: NullPointerException، ArithmeticException

**الكلمات المفتاحية الأساسية**:
- `try`: كتلة قد ترمي استثناءً
- `catch`: كتلة تعالج الاستثناء
- `finally`: كتلة تُنفذ دائماً (كود التنظيف)
- `throw`: تُستخدم لرمي استثناء صراحةً
- `throws`: تُعلن أن الطريقة قد ترمي استثناءً

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **معالجة الأخطاء**: مهارة أساسية للتطبيقات المتينة
- **قرارات التصميم**: متى تستخدم الاستثناءات المُراقَبة مقابل غير المُراقَبة
- **إدارة الموارد**: فهم try-with-resources وfinally
- **مصدر شائع للأخطاء**: NullPointerException وتسرب الموارد مشاكل متكررة
- **أفضل الممارسات**: أنماط معالجة الاستثناءات الصحيحة
- **معرفة الأُطر**: معظم الأُطر تستخدم استثناءات مخصصة

---

## 3️⃣ القواعد والحقائق الأساسية

**قواعد معالجة الاستثناءات**:
- يجب التقاط الاستثناءات المحددة قبل الاستثناءات العامة
- كتلة `finally` تُنفذ حتى لو كان هناك return في try أو catch
- كتلة catch واحدة فقط تُنفذ لاستثناء معين
- يمكن أن يكون هناك عدة كتل catch لاستثناءات مختلفة
- كتلة try يجب أن تُتبع بـ catch أو finally أو كليهما
- لا يمكن أن يكون هناك try وحده بدون catch أو finally

**الفرق بين throw و throws**:

| throw | throws |
|-------|--------|
| تُستخدم لرمي استثناء صراحةً | تُستخدم للإعلان عن الاستثناءات |
| تُستخدم داخل جسم الطريقة | تُستخدم في توقيع الطريقة |
| يمكن رمي استثناء واحد فقط | يمكن الإعلان عن عدة استثناءات |
| يتبعها نسخة من الاستثناء | يتبعها اسم فئة الاستثناء |
| مثال: `throw new IOException()` | مثال: `throws IOException, SQLException` |

**الاستثناءات المُراقَبة مقابل غير المُراقَبة**:

| الميزة | الاستثناء المُراقَب | الاستثناء غير المُراقَب |
|--------|-------------------|------------------------|
| الفحص | وقت الترجمة | وقت التشغيل |
| المعالجة | إلزامية | اختيارية |
| يمتد من | Exception (ليس RuntimeException) | RuntimeException |
| حالة الاستخدام | الحالات القابلة للاسترداد | أخطاء البرمجة |
| أمثلة | IOException, SQLException | NullPointerException, ArithmeticException |

**قواعد كتلة finally**:
- تُنفذ دائماً (مع استثناءات نادرة: System.exit()، انهيار JVM)
- تُنفذ حتى لو رُمي استثناء أو وُجدت عبارة return
- تُستخدم للتنظيف (إغلاق الموارد، تحرير الأقفال)
- إذا حدث استثناء في finally، فإنه يتجاوز الاستثناء من try/catch
- لا يمكن أن يكون هناك finally بدون try

**try-with-resources** (Java 7+):
- يغلق الموارد تلقائياً التي تنفذ AutoCloseable
- أنظف من try-catch-finally لإدارة الموارد
- يمكن الإعلان عن موارد متعددة
- تُغلق الموارد بالترتيب العكسي لإنشائها

**أفضل الممارسات**:
- التقط استثناءات محددة، وليس Exception العام
- لا تلتقط Error أو Throwable
- نظّف الموارد دائماً في finally أو استخدم try-with-resources
- لا تكتم الاستثناءات بصمت (كتلة catch فارغة)
- استخدم استثناءات مخصصة للأخطاء الخاصة بالمجال
- سجّل الاستثناءات قبل إعادة رميها
- لا تستخدم الاستثناءات للتحكم في التدفق

---

## 4️⃣ أمثلة على الكود (Java 8)

### المثال 1: معالجة الاستثناءات الأساسية

```java
public class BasicExceptionHandling {
    public static void main(String[] args) {
        // Example 1: ArithmeticException
        try {
            int result = 10 / 0;  // Division by zero
            System.out.println(result);
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero!");
            System.out.println("Exception message: " + e.getMessage());
        }

        System.out.println("Program continues...");

        // Example 2: ArrayIndexOutOfBoundsException
        try {
            int[] numbers = {1, 2, 3};
            System.out.println(numbers[5]);  // Invalid index
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Array index out of bounds!");
        }

        // Example 3: NullPointerException
        try {
            String str = null;
            System.out.println(str.length());  // NPE
        } catch (NullPointerException e) {
            System.out.println("Null reference accessed!");
        }

        // Example 4: NumberFormatException
        try {
            int num = Integer.parseInt("abc");  // Invalid number format
        } catch (NumberFormatException e) {
            System.out.println("Invalid number format!");
        }

        System.out.println("Program completed successfully");
    }
}
```

### المثال 2: كتل catch المتعددة

```java
public class MultipleCatchBlocks {
    public static void main(String[] args) {
        // Order matters: Specific exceptions before general ones
        try {
            int[] arr = {1, 2, 3};
            System.out.println(arr[5]);  // ArrayIndexOutOfBoundsException
            int result = 10 / 0;  // ArithmeticException
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Array index error: " + e.getMessage());
        } catch (ArithmeticException e) {
            System.out.println("Arithmetic error: " + e.getMessage());
        } catch (Exception e) {
            // General exception - catches anything not caught above
            System.out.println("General error: " + e.getMessage());
        }

        // Multi-catch (Java 7+) - for same handling logic
        try {
            String str = null;
            str.length();
        } catch (NullPointerException | IllegalArgumentException e) {
            System.out.println("Error occurred: " + e.getMessage());
        }

        // WRONG: Compiler error - unreachable code
        /*
        try {
            int x = 10 / 0;
        } catch (Exception e) {
            // This catches everything
        } catch (ArithmeticException e) {
            // Compilation error: Already caught by Exception above
        }
        */
    }
}
```

### المثال 3: كتلة finally

```java
public class FinallyBlockDemo {
    public static void main(String[] args) {
        // Example 1: finally executes even with exception
        try {
            System.out.println("Try block");
            int result = 10 / 0;
        } catch (ArithmeticException e) {
            System.out.println("Catch block");
        } finally {
            System.out.println("Finally block always executes");
        }

        // Example 2: finally executes even with return
        System.out.println("\nReturn value: " + testFinally());

        // Example 3: Resource cleanup in finally
        java.io.FileReader reader = null;
        try {
            reader = new java.io.FileReader("test.txt");
            // Read file
        } catch (java.io.FileNotFoundException e) {
            System.out.println("File not found");
        } finally {
            // Always close resources
            if (reader != null) {
                try {
                    reader.close();
                } catch (java.io.IOException e) {
                    System.out.println("Error closing file");
                }
            }
        }
    }

    public static int testFinally() {
        try {
            System.out.println("Try block in method");
            return 1;
        } catch (Exception e) {
            System.out.println("Catch block in method");
            return 2;
        } finally {
            System.out.println("Finally block in method");
            // return 3;  // If uncommented, this overrides return from try
        }
        // Output:
        // Try block in method
        // Finally block in method
        // Return value: 1
    }
}
```

### المثال 4: throw و throws

```java
public class ThrowAndThrows {
    // throws - declares exception in method signature
    public static void validateAge(int age) throws IllegalArgumentException {
        if (age < 18) {
            // throw - explicitly throws exception
            throw new IllegalArgumentException("Age must be 18 or above");
        }
        System.out.println("Age is valid");
    }

    // Checked exception - must handle or declare
    public static void readFile(String filename) throws java.io.IOException {
        java.io.FileReader reader = new java.io.FileReader(filename);
        // File operations
        reader.close();
    }

    // Multiple exceptions in throws clause
    public static void processData() throws java.io.IOException, java.sql.SQLException {
        // Method that might throw multiple exceptions
    }

    public static void main(String[] args) {
        // Example 1: Handling unchecked exception
        try {
            validateAge(15);
        } catch (IllegalArgumentException e) {
            System.out.println("Caught: " + e.getMessage());
        }

        // Example 2: Handling checked exception
        try {
            readFile("nonexistent.txt");
        } catch (java.io.IOException e) {
            System.out.println("File error: " + e.getMessage());
        }

        // Example 3: Re-throwing exception
        try {
            validateAge(16);
        } catch (IllegalArgumentException e) {
            System.out.println("Logging error...");
            throw e;  // Re-throw the same exception
        }
    }
}
```

### المثال 5: الاستثناءات المخصصة

```java
// Custom checked exception
class InsufficientFundsException extends Exception {
    private double amount;

    public InsufficientFundsException(double amount) {
        super("Insufficient funds. Required: " + amount);
        this.amount = amount;
    }

    public double getAmount() {
        return amount;
    }
}

// Custom unchecked exception
class InvalidAccountException extends RuntimeException {
    public InvalidAccountException(String message) {
        super(message);
    }
}

class BankAccount {
    private double balance;
    private String accountNumber;

    public BankAccount(String accountNumber, double balance) {
        if (accountNumber == null || accountNumber.isEmpty()) {
            throw new InvalidAccountException("Account number cannot be empty");
        }
        this.accountNumber = accountNumber;
        this.balance = balance;
    }

    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(amount - balance);
        }
        balance -= amount;
        System.out.println("Withdrawn: $" + amount);
    }

    public double getBalance() {
        return balance;
    }
}

public class CustomExceptionDemo {
    public static void main(String[] args) {
        // Test custom unchecked exception
        try {
            BankAccount invalidAccount = new BankAccount("", 1000);
        } catch (InvalidAccountException e) {
            System.out.println("Error: " + e.getMessage());
        }

        // Test custom checked exception
        BankAccount account = new BankAccount("ACC123", 500);
        try {
            account.withdraw(600);
        } catch (InsufficientFundsException e) {
            System.out.println("Error: " + e.getMessage());
            System.out.println("Short by: $" + e.getAmount());
        }

        System.out.println("Final balance: $" + account.getBalance());
    }
}
```

### المثال 6: try-with-resources (Java 7+)

```java
import java.io.*;

public class TryWithResourcesDemo {
    // Before Java 7 - manual resource management
    public static void readFileOldWay(String filename) {
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader(filename));
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // Must manually close
            if (reader != null) {
                try {
                    reader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    // Java 7+ - automatic resource management
    public static void readFileNewWay(String filename) {
        // Resources declared in try() are auto-closed
        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        // reader.close() called automatically
    }

    // Multiple resources (closed in reverse order)
    public static void copyFile(String source, String dest) {
        try (
            FileInputStream in = new FileInputStream(source);
            FileOutputStream out = new FileOutputStream(dest)
        ) {
            byte[] buffer = new byte[1024];
            int length;
            while ((length = in.read(buffer)) > 0) {
                out.write(buffer, 0, length);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        // Both streams closed automatically in reverse order
    }

    // Custom AutoCloseable resource
    static class DatabaseConnection implements AutoCloseable {
        public DatabaseConnection() {
            System.out.println("Opening database connection");
        }

        public void executeQuery() {
            System.out.println("Executing query");
        }

        @Override
        public void close() {
            System.out.println("Closing database connection");
        }
    }

    public static void main(String[] args) {
        // Using custom AutoCloseable
        try (DatabaseConnection conn = new DatabaseConnection()) {
            conn.executeQuery();
        }
        // Output:
        // Opening database connection
        // Executing query
        // Closing database connection
    }
}
```

### المثال 7: سلسلة الاستثناءات

```java
class DataProcessingException extends Exception {
    public DataProcessingException(String message, Throwable cause) {
        super(message, cause);
    }
}

public class ExceptionChaining {
    public static void parseData(String data) throws DataProcessingException {
        try {
            int number = Integer.parseInt(data);
            if (number < 0) {
                throw new IllegalArgumentException("Number cannot be negative");
            }
        } catch (NumberFormatException e) {
            // Chain the original exception
            throw new DataProcessingException("Failed to parse data: " + data, e);
        }
    }

    public static void processFile(String filename) throws DataProcessingException {
        try {
            java.io.BufferedReader reader = new java.io.BufferedReader(
                new java.io.FileReader(filename)
            );
            String line = reader.readLine();
            parseData(line);
            reader.close();
        } catch (java.io.IOException e) {
            throw new DataProcessingException("File processing failed", e);
        }
    }

    public static void main(String[] args) {
        try {
            processFile("data.txt");
        } catch (DataProcessingException e) {
            System.out.println("Error: " + e.getMessage());
            System.out.println("Caused by: " + e.getCause());

            // Print full stack trace
            e.printStackTrace();

            // Get root cause
            Throwable rootCause = e.getCause();
            while (rootCause.getCause() != null) {
                rootCause = rootCause.getCause();
            }
            System.out.println("Root cause: " + rootCause);
        }
    }
}
```

### المثال 8: سيناريوهات الاستثناءات الشائعة

```java
import java.util.*;

public class CommonExceptionScenarios {
    public static void main(String[] args) {
        // 1. NullPointerException
        try {
            String str = null;
            System.out.println(str.length());
        } catch (NullPointerException e) {
            System.out.println("NPE: Cannot call method on null");
        }

        // 2. ArrayIndexOutOfBoundsException
        try {
            int[] arr = new int[5];
            arr[10] = 100;
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Invalid array index");
        }

        // 3. NumberFormatException
        try {
            int num = Integer.parseInt("12.34");  // Not a valid integer
        } catch (NumberFormatException e) {
            System.out.println("Invalid number format");
        }

        // 4. ClassCastException
        try {
            Object obj = "Hello";
            Integer num = (Integer) obj;  // Cannot cast String to Integer
        } catch (ClassCastException e) {
            System.out.println("Invalid cast");
        }

        // 5. IllegalArgumentException
        try {
            Thread.sleep(-1000);  // Negative sleep time
        } catch (IllegalArgumentException e) {
            System.out.println("Invalid argument: " + e.getMessage());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 6. ConcurrentModificationException
        try {
            List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
            for (String s : list) {
                list.remove(s);  // Modifying during iteration
            }
        } catch (ConcurrentModificationException e) {
            System.out.println("Cannot modify collection during iteration");
        }

        // 7. StackOverflowError (not Exception, but Error)
        try {
            recursiveMethod();
        } catch (StackOverflowError e) {
            System.out.println("Stack overflow - infinite recursion");
        }
    }

    public static void recursiveMethod() {
        recursiveMethod();  // Infinite recursion
    }
}
```

### المثال 9: أفضل الممارسات

```java
import java.io.*;
import java.util.logging.*;

public class ExceptionBestPractices {
    private static final Logger logger = Logger.getLogger(ExceptionBestPractices.class.getName());

    // BAD: Catching generic Exception
    public void badPractice1() {
        try {
            // Some code
        } catch (Exception e) {
            // Too broad - catches everything
        }
    }

    // GOOD: Catch specific exceptions
    public void goodPractice1() {
        try {
            // Some code
        } catch (IOException e) {
            // Handle IO exception
        } catch (SQLException e) {
            // Handle SQL exception
        }
    }

    // BAD: Empty catch block (swallowing exception)
    public void badPractice2() {
        try {
            int result = 10 / 0;
        } catch (ArithmeticException e) {
            // Silent failure - very bad!
        }
    }

    // GOOD: Log the exception
    public void goodPractice2() {
        try {
            int result = 10 / 0;
        } catch (ArithmeticException e) {
            logger.log(Level.SEVERE, "Division by zero", e);
            // Or at minimum: e.printStackTrace();
        }
    }

    // BAD: Using exceptions for flow control
    public int badPractice3(String input) {
        try {
            return Integer.parseInt(input);
        } catch (NumberFormatException e) {
            return 0;  // Using exception as normal flow
        }
    }

    // GOOD: Validate first
    public int goodPractice3(String input) {
        if (input == null || input.isEmpty()) {
            return 0;
        }
        try {
            return Integer.parseInt(input);
        } catch (NumberFormatException e) {
            logger.warning("Invalid number format: " + input);
            return 0;
        }
    }

    // BAD: Not closing resources
    public void badPractice4() throws IOException {
        FileReader reader = new FileReader("file.txt");
        // Use reader
        // Forgot to close - resource leak!
    }

    // GOOD: Use try-with-resources
    public void goodPractice4() throws IOException {
        try (FileReader reader = new FileReader("file.txt")) {
            // Use reader
        } // Automatically closed
    }

    // BAD: Throwing Exception
    public void badPractice5() throws Exception {
        // Too generic
    }

    // GOOD: Throw specific exception
    public void goodPractice5() throws IOException, SQLException {
        // Specific exceptions
    }

    // GOOD: Provide context in exception message
    public void withdraw(double amount, double balance) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(
                String.format("Cannot withdraw %.2f, balance is %.2f", amount, balance)
            );
        }
    }
}

class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

### المثال 10: مثال واقعي - تسجيل المستخدم

```java
class UserRegistrationException extends Exception {
    public UserRegistrationException(String message) {
        super(message);
    }
}

class InvalidEmailException extends UserRegistrationException {
    public InvalidEmailException(String email) {
        super("Invalid email format: " + email);
    }
}

class PasswordTooWeakException extends UserRegistrationException {
    public PasswordTooWeakException() {
        super("Password must be at least 8 characters with 1 digit and 1 special char");
    }
}

class UsernameAlreadyExistsException extends UserRegistrationException {
    public UsernameAlreadyExistsException(String username) {
        super("Username already exists: " + username);
    }
}

class UserRegistrationService {
    private java.util.Set<String> existingUsernames = new java.util.HashSet<>();

    public void registerUser(String username, String email, String password)
            throws UserRegistrationException {

        // Validate username
        if (username == null || username.trim().isEmpty()) {
            throw new UserRegistrationException("Username cannot be empty");
        }

        if (existingUsernames.contains(username)) {
            throw new UsernameAlreadyExistsException(username);
        }

        // Validate email
        if (!isValidEmail(email)) {
            throw new InvalidEmailException(email);
        }

        // Validate password
        if (!isStrongPassword(password)) {
            throw new PasswordTooWeakException();
        }

        // Registration successful
        existingUsernames.add(username);
        System.out.println("User registered successfully: " + username);
    }

    private boolean isValidEmail(String email) {
        return email != null && email.contains("@") && email.contains(".");
    }

    private boolean isStrongPassword(String password) {
        if (password == null || password.length() < 8) {
            return false;
        }
        boolean hasDigit = password.chars().anyMatch(Character::isDigit);
        boolean hasSpecial = password.chars().anyMatch(c -> "!@#$%^&*".indexOf(c) >= 0);
        return hasDigit && hasSpecial;
    }
}

public class UserRegistrationDemo {
    public static void main(String[] args) {
        UserRegistrationService service = new UserRegistrationService();

        // Test cases
        String[][] testCases = {
            {"john_doe", "john@example.com", "Pass123!"},      // Valid
            {"jane_doe", "invalid-email", "Pass123!"},         // Invalid email
            {"bob", "bob@example.com", "weak"},                // Weak password
            {"", "empty@example.com", "Pass123!"},             // Empty username
        };

        for (String[] testCase : testCases) {
            try {
                service.registerUser(testCase[0], testCase[1], testCase[2]);
            } catch (InvalidEmailException e) {
                System.out.println("Email error: " + e.getMessage());
            } catch (PasswordTooWeakException e) {
                System.out.println("Password error: " + e.getMessage());
            } catch (UsernameAlreadyExistsException e) {
                System.out.println("Username error: " + e.getMessage());
            } catch (UserRegistrationException e) {
                System.out.println("Registration error: " + e.getMessage());
            }
            System.out.println("---");
        }

        // Try to register duplicate username
        try {
            service.registerUser("john_doe", "another@example.com", "Pass123!");
        } catch (UserRegistrationException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما الفرق بين الاستثناءات المُراقَبة وغير المُراقَبة؟**
2. **ما الفرق بين throw و throws؟**
3. **هل يمكن أن يكون هناك try بدون catch؟**
4. **ما هو الغرض من كتلة finally؟**
5. **هل يمكن تخطي كتلة finally؟**
6. **ما هو try-with-resources؟**
7. **هل يمكن التقاط عدة استثناءات في كتلة catch واحدة؟**
8. **ما هي سلسلة الاستثناءات؟**
9. **متى يجب إنشاء استثناءات مخصصة؟**
10. **ما الفرق بين Exception و Error؟**

---

## 6️⃣ نماذج الإجابات

**س1: ما الفرق بين الاستثناءات المُراقَبة وغير المُراقَبة؟**

"الاستثناءات المُراقَبة تُفحص في وقت الترجمة، لذا يجب إما معالجتها باستخدام try-catch أو الإعلان عنها باستخدام throws في توقيع الطريقة. تمتد من فئة Exception لكن ليس RuntimeException. أمثلة تشمل IOException و SQLException. تمثل حالات قابلة للاسترداد. الاستثناءات غير المُراقَبة تمتد من RuntimeException وتُفحص في وقت التشغيل، وليس وقت الترجمة. لست مطالباً بمعالجتها، رغم أنه يمكنك ذلك. أمثلة تشمل NullPointerException و ArrayIndexOutOfBoundsException. عادةً تمثل أخطاء برمجية كان يمكن تجنبها."

**س2: ما الفرق بين throw و throws؟**

"throw هي كلمة مفتاحية تُستخدم داخل جسم الطريقة لرمي نسخة استثناء صراحةً. مثلاً، 'throw new IllegalArgumentException()'. يمكنك رمي استثناء واحد فقط مع throw. throws تُستخدم في توقيع الطريقة للإعلان أن الطريقة قد ترمي واحداً أو أكثر من الاستثناءات. مثلاً، 'public void method() throws IOException, SQLException'. throws تُعلم المستدعي أنه يحتاج معالجة هذه الاستثناءات. إذن throw ترمي الاستثناء فعلياً، بينما throws تُعلن فقط عن الإمكانية."

**س3: هل يمكن أن يكون هناك try بدون catch؟**

"نعم، يمكن أن يكون هناك try بدون catch في سيناريوهين. أولاً، try مع finally: try { } finally { }. هذا مفيد عندما تريد كود تنظيف لكن لا تحتاج معالجة الاستثناءات. ثانياً، try-with-resources لا يتطلب catch: try (Resource r = new Resource()) { }. الموارد تُغلق تلقائياً. لكن لا يمكن أن يكون هناك كتلة try وحدها تماماً - يجب أن تُتبع إما بـ catch أو finally أو تكون عبارة try-with-resources."

**س4: ما هو الغرض من كتلة finally؟**

"كتلة finally تُستخدم لكود التنظيف الذي يجب تنفيذه بغض النظر عما إذا حدث استثناء أم لا. مضمون تنفيذها حتى لو كانت هناك عبارة return في try أو catch، أو إذا رُمي استثناء. الاستخدامات الشائعة تشمل إغلاق الملفات، وتحرير اتصالات قاعدة البيانات، أو تنظيف الموارد. تضمن تشغيل كود التنظيف الحرج حتى في مواجهة الاستثناءات. لكن try-with-resources يُفضل الآن لإدارة الموارد التلقائية."

**س5: هل يمكن تخطي كتلة finally؟**

"كتلة finally تُنفذ دائماً تقريباً، لكن هناك سيناريوهات نادرة لن تعمل فيها. إذا استدعيت System.exit() في كتلة try أو catch، تنتهي JVM فوراً ولا تُنفذ finally. إذا انهارت JVM أو قُتل الخيط، لن تعمل finally. إذا كانت هناك حلقة لا نهائية في try أو catch، لن تحصل finally على فرصة للتنفيذ. وإذا تم مقاطعة أو قتل الخيط الذي ينفذ try-catch، قد لا تعمل finally. لكن في معالجة الاستثناءات العادية، تُنفذ finally دائماً."

**س6: ما هو try-with-resources؟**

"try-with-resources، المقدمة في Java 7، تغلق الموارد تلقائياً التي تنفذ واجهات AutoCloseable أو Closeable. تُعلن الموارد في الأقواس بعد try، مثل try (FileReader fr = new FileReader('file.txt')) { }. عندما تخرج كتلة try، سواء بشكل طبيعي أو باستثناء، تُغلق الموارد تلقائياً بالترتيب العكسي لإنشائها. هذا يلغي الحاجة لكتل finally صريحة للتنظيف ويمنع تسرب الموارد. أنظف وأكثر أماناً من إدارة الموارد اليدوية. يمكن الإعلان عن موارد متعددة مفصولة بفواصل منقوطة."

**س7: هل يمكن التقاط عدة استثناءات في كتلة catch واحدة؟**

"نعم، Java 7 قدمت multi-catch حيث يمكنك التقاط عدة استثناءات في كتلة واحدة باستخدام عامل الأنبوب. مثلاً: catch (IOException | SQLException e). هذا مفيد عندما تريد نفس منطق المعالجة لاستثناءات مختلفة. الاستثناء الملتقط يكون نهائياً فعلياً، لذا لا يمكنك تعيين قيمة جديدة للمتغير. هذه الميزة تقلل تكرار الكود عندما تحتاج استثناءات متعددة معالجة متطابقة. لاحظ أنك لا تستطيع التقاط استثناءات لها علاقة وراثة في multi-catch."

**س8: ما هي سلسلة الاستثناءات؟**

"سلسلة الاستثناءات هي عندما تلتقط استثناءاً وترمي آخر، مغلفاً الأصلي كسبب. هذا يحافظ على تتبع المكدس للاستثناء الأصلي مع إضافة سياق باستثناء جديد. تفعل هذا بتمرير الاستثناء الأصلي لمنشئ الاستثناء الجديد. مثلاً: catch (SQLException e) { throw new DataAccessException('Database error', e); }. يمكنك لاحقاً استرجاع الاستثناء الأصلي باستخدام getCause(). هذا مفيد لإنشاء طبقات تجريد - تحول الاستثناءات منخفضة المستوى إلى استثناءات مجال عالية المستوى مع الحفاظ على معلومات التصحيح."

**س9: متى يجب إنشاء استثناءات مخصصة؟**

"أنشئ استثناءات مخصصة عندما لا تصف استثناءات Java القياسية حالة الخطأ بشكل كافٍ، أو عندما تحتاج استثناءات خاصة بالمجال. لأخطاء منطق الأعمال مثل InsufficientFundsException أو InvalidOrderException، الاستثناءات المخصصة تجعل الكود أكثر قابلية للقراءة والصيانة. امتد من Exception للاستثناءات المُراقَبة عندما يجب إجبار المستدعي على معالجة الحالة. امتد من RuntimeException للاستثناءات غير المُراقَبة التي تمثل أخطاء برمجية. دائماً وفر منشئات ذات معنى - عادةً واحد برسالة فقط، وواحد برسالة وسبب لسلسلة الاستثناءات، وأحياناً واحد يتضمن تفاصيل خطأ إضافية خاصة بمجالك."

**س10: ما الفرق بين Exception و Error؟**

"Exception تمثل حالات يجب أن تلتقطها التطبيقات وتعالجها. عادةً قابلة للاسترداد وتشمل IOException و SQLException. Error تمثل مشاكل خطيرة يجب ألا تحاول التطبيقات التقاطها، مثل OutOfMemoryError أو StackOverflowError. الأخطاء تشير لحالات عادةً غير قابلة للاسترداد وغالباً تمثل مشاكل على مستوى JVM. بينما كلاهما يمتد من Throwable، أفضل الممارسات التقاط Exception وفئاتها الفرعية لكن ليس Error. التقاط Error يمكن أن يخفي مشاكل خطيرة ويجعل التصحيح صعباً. كودك يجب أن يعالج Exceptions لكن يترك الأخطاء تنتشر لإنهاء التطبيق."

---

## 7️⃣ الأخطاء الشائعة

1. **التقاط Exception أو Throwable بشكل واسع جداً**:
   ```java
   try {
       // code
   } catch (Exception e) {
       // Catches everything - too broad!
   }

   // Better: Catch specific exceptions
   try {
       // code
   } catch (IOException e) {
       // Handle IO error
   } catch (SQLException e) {
       // Handle SQL error
   }
   ```

2. **كتلة catch فارغة (ابتلاع الاستثناءات)**:
   ```java
   try {
       riskyOperation();
   } catch (Exception e) {
       // Silent failure - very bad!
   }

   // At minimum, log it:
   catch (Exception e) {
       e.printStackTrace();
       // Or use logging framework
   }
   ```

3. **عدم اتباع التسلسل الهرمي للاستثناءات في كتل catch**:
   ```java
   try {
       // code
   } catch (Exception e) {
       // General exception
   } catch (IOException e) {
       // Compilation error: unreachable code
   }

   // Correct order: specific before general
   ```

4. **عدم إغلاق الموارد بشكل صحيح**:
   ```java
   FileReader reader = new FileReader("file.txt");
   // Use reader
   // Forgot to close - resource leak!

   // Use try-with-resources instead
   try (FileReader reader = new FileReader("file.txt")) {
       // Use reader
   }
   ```

5. **التقاط الاستثناءات للتحكم في التدفق**:
   ```java
   // BAD: Using exceptions for normal flow
   try {
       return array[index];
   } catch (ArrayIndexOutOfBoundsException e) {
       return null;
   }

   // GOOD: Check first
   if (index >= 0 && index < array.length) {
       return array[index];
   }
   return null;
   ```

6. **رمي Exception عام**:
   ```java
   public void process() throws Exception {  // Too generic
       // code
   }

   // Better:
   public void process() throws IOException, SQLException {
       // Specific exceptions
   }
   ```

7. **عدم الحفاظ على تتبع المكدس عند إعادة الرمي**:
   ```java
   catch (Exception e) {
       throw new CustomException("Error occurred");  // Lost original stack trace
   }

   // Better: Include cause
   catch (Exception e) {
       throw new CustomException("Error occurred", e);
   }
   ```

8. **الإرجاع في finally (يتجاوز إرجاع try/catch)**:
   ```java
   try {
       return 1;
   } finally {
       return 2;  // This overrides the return from try
   }
   // Returns 2, not 1
   ```

---

## 8️⃣ نصائح للمقابلات الحقيقية

**ما يجب التأكيد عليه**:
- الاستثناءات المُراقَبة يجب معالجتها أو الإعلان عنها (وقت الترجمة)
- الاستثناءات غير المُراقَبة (RuntimeException) اختيارية (وقت التشغيل)
- finally تُنفذ دائماً (كود التنظيف)
- try-with-resources لإدارة الموارد التلقائية
- الاستثناءات المخصصة للأخطاء الخاصة بالمجال
- سلسلة الاستثناءات تحافظ على تتبع المكدس

**ما يجب تجنبه**:
- لا تقل أن الاستثناءات المُراقَبة "أفضل" (يعتمد على حالة الاستخدام)
- لا تدّعي أن finally "أبداً" لا تُنفذ (توجد حالات نادرة)
- لا توصي بالتقاط فئات Error
- لا تدافع عن كتل catch الفارغة

**تحت الضغط**:
- ارسم التسلسل الهرمي للاستثناءات: Throwable → Error/Exception → RuntimeException
- تذكر: throw = رمي نسخة، throws = إعلان في التوقيع
- اذكر أن الاستثناءات المحددة يجب التقاطها قبل العامة
- اذكر try-with-resources كأفضل ممارسة للموارد

**علامات تحذيرية يجب تجنبها**:
- "الاستثناءات بطيئة لذا تجنبها" (يفتقد النقطة - هي للأخطاء)
- "التقط دائماً Exception" (واسع جداً)
- "finally اختيارية" (هي كذلك، لكن مهمة للتنظيف)
- "الاستثناءات المُراقَبة سيئة" (لها حالات استخدام صالحة)

**نقاط إضافية**:
- اذكر الاستثناءات المكبوتة في try-with-resources
- ناقش متى تستخدم المُراقَبة مقابل غير المُراقَبة
- أشر لرسائل الاستثناء الفعالة مع السياق
- اذكر ترجمة الاستثناءات في البنى متعددة الطبقات
- ناقش استراتيجيات معالجة الأخطاء السريعة الفشل مقابل الآمنة من الفشل
- أشر لأُطر التسجيل لمعالجة الاستثناءات
- اذكر أن الاستثناءات لها تكلفة أداء (إنشاء الكائن، تتبع المكدس)

</div>
