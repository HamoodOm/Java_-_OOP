# Exception Handling

## 1️⃣ Concept Overview

**Exception** is an unexpected event that occurs during program execution that disrupts the normal flow of instructions. Exception handling is a mechanism to handle runtime errors and maintain normal application flow.

**Exception Hierarchy**:
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

**Two Types of Exceptions**:

1. **Checked Exceptions**: 
   - Checked at compile-time
   - Must be handled or declared with `throws`
   - Extend `Exception` class (but not `RuntimeException`)
   - Examples: IOException, SQLException

2. **Unchecked Exceptions (Runtime Exceptions)**:
   - Checked at runtime
   - Not required to be handled
   - Extend `RuntimeException` class
   - Examples: NullPointerException, ArithmeticException

**Key Keywords**:
- `try`: Block that might throw exception
- `catch`: Block that handles exception
- `finally`: Block that always executes (cleanup code)
- `throw`: Used to explicitly throw an exception
- `throws`: Declares that method might throw exception

---

## 2️⃣ Why Interviewers Ask This

- **Error handling**: Essential skill for robust applications
- **Design decisions**: When to use checked vs unchecked exceptions
- **Resource management**: Understanding try-with-resources and finally
- **Common source of bugs**: NPE, resource leaks are frequent issues
- **Best practices**: Proper exception handling patterns
- **Framework knowledge**: Most frameworks use custom exceptions

---

## 3️⃣ Key Rules / Facts

**Exception Handling Rules**:
- Specific exceptions must be caught before general exceptions
- `finally` block executes even if there's a return in try or catch
- Only one catch block executes for a given exception
- Can have multiple catch blocks for different exceptions
- Try block must be followed by either catch, finally, or both
- Cannot have try alone without catch or finally

**throw vs throws**:

| throw | throws |
|-------|--------|
| Used to explicitly throw exception | Used to declare exceptions |
| Used inside method body | Used in method signature |
| Can throw only one exception | Can declare multiple exceptions |
| Followed by exception instance | Followed by exception class name |
| Example: `throw new IOException()` | Example: `throws IOException, SQLException` |

**Checked vs Unchecked Exceptions**:

| Feature | Checked Exception | Unchecked Exception |
|---------|------------------|---------------------|
| Checking | Compile-time | Runtime |
| Handling | Mandatory | Optional |
| Extends | Exception (not RuntimeException) | RuntimeException |
| Use Case | Recoverable conditions | Programming errors |
| Examples | IOException, SQLException | NullPointerException, ArithmeticException |

**finally Block Rules**:
- Always executes (with rare exceptions: System.exit(), JVM crash)
- Executes even if exception is thrown or return statement exists
- Used for cleanup (closing resources, releasing locks)
- If exception occurs in finally, it overrides exception from try/catch
- Cannot have finally without try

**try-with-resources** (Java 7+):
- Automatically closes resources that implement AutoCloseable
- Cleaner than try-catch-finally for resource management
- Multiple resources can be declared
- Resources are closed in reverse order of creation

**Best Practices**:
- Catch specific exceptions, not generic Exception
- Don't catch Error or Throwable
- Always clean up resources in finally or use try-with-resources
- Don't suppress exceptions silently (empty catch block)
- Use custom exceptions for domain-specific errors
- Log exceptions before re-throwing
- Don't use exceptions for flow control

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Basic Exception Handling

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

### Example 2: Multiple Catch Blocks

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

### Example 3: finally Block

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

### Example 4: throw and throws

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

### Example 5: Custom Exceptions

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

### Example 6: try-with-resources (Java 7+)

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

### Example 7: Exception Chaining

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

### Example 8: Common Exception Scenarios

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

### Example 9: Best Practices

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

### Example 10: Real-World Example - User Registration

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

## 5️⃣ Common Interview Questions

1. **What is the difference between checked and unchecked exceptions?**
2. **What is the difference between throw and throws?**
3. **Can we have try without catch?**
4. **What is the purpose of the finally block?**
5. **Can finally block be skipped?**
6. **What is try-with-resources?**
7. **Can we catch multiple exceptions in one catch block?**
8. **What is exception chaining?**
9. **When should you create custom exceptions?**
10. **What is the difference between Exception and Error?**

---

## 6️⃣ Model Answers

**Q1: What is the difference between checked and unchecked exceptions?**

"Checked exceptions are checked at compile-time, so you must either handle them with try-catch or declare them with throws in the method signature. They extend Exception class but not RuntimeException. Examples include IOException and SQLException. They represent recoverable conditions. Unchecked exceptions extend RuntimeException and are checked at runtime, not compile-time. You're not required to handle them, though you can. Examples include NullPointerException and ArrayIndexOutOfBoundsException. They typically represent programming errors that could have been prevented."

**Q2: What is the difference between throw and throws?**

"Throw is a keyword used inside a method body to explicitly throw an exception instance. For example, 'throw new IllegalArgumentException()'. You can throw only one exception with throw. Throws is used in the method signature to declare that the method might throw one or more exceptions. For example, 'public void method() throws IOException, SQLException'. Throws informs the caller that they need to handle these exceptions. So throw actually throws the exception, while throws just declares the possibility."

**Q3: Can we have try without catch?**

"Yes, you can have try without catch in two scenarios. First, try with finally: try { } finally { }. This is useful when you want cleanup code but don't need to handle exceptions. Second, try-with-resources doesn't require catch: try (Resource r = new Resource()) { }. The resources are automatically closed. However, you cannot have a try block completely alone—it must be followed by either catch, finally, or be a try-with-resources statement."

**Q4: What is the purpose of the finally block?**

"The finally block is used for cleanup code that must execute regardless of whether an exception occurs or not. It's guaranteed to execute even if there's a return statement in try or catch, or if an exception is thrown. Common uses include closing files, releasing database connections, or cleaning up resources. It ensures that critical cleanup code runs even in the face of exceptions. However, try-with-resources is now preferred for automatic resource management."

**Q5: Can finally block be skipped?**

"The finally block is almost always executed, but there are rare scenarios where it won't run. If you call System.exit() in the try or catch block, the JVM terminates immediately and finally doesn't execute. If the JVM crashes or the thread is killed, finally won't run. If there's an infinite loop in try or catch, finally never gets a chance to execute. And if the thread executing the try-catch is interrupted or killed, finally might not run. But in normal exception handling, finally always executes."

**Q6: What is try-with-resources?**

"Try-with-resources, introduced in Java 7, automatically closes resources that implement AutoCloseable or Closeable interfaces. You declare resources in parentheses after try, like try (FileReader fr = new FileReader('file.txt')) { }. When the try block exits, whether normally or exceptionally, the resources are automatically closed in reverse order of creation. This eliminates the need for explicit finally blocks for cleanup and prevents resource leaks. It's cleaner and safer than manual resource management. Multiple resources can be declared separated by semicolons."

**Q7: Can we catch multiple exceptions in one catch block?**

"Yes, Java 7 introduced multi-catch where you can catch multiple exceptions in one block using the pipe operator. For example: catch (IOException | SQLException e). This is useful when you want the same handling logic for different exceptions. The caught exception is effectively final, so you cannot assign a new value to the variable. This feature reduces code duplication when multiple exceptions need identical handling. Note that you cannot catch exceptions that have an inheritance relationship in multi-catch."

**Q8: What is exception chaining?**

"Exception chaining is when you catch one exception and throw another, wrapping the original as the cause. This preserves the stack trace of the original exception while adding context with a new exception. You do this by passing the original exception to the constructor of the new exception. For example: catch (SQLException e) { throw new DataAccessException('Database error', e); }. You can later retrieve the original exception using getCause(). This is useful for creating abstraction layers—you convert low-level exceptions to high-level domain exceptions while preserving debugging information."

**Q9: When should you create custom exceptions?**

"Create custom exceptions when standard Java exceptions don't adequately describe your error condition, or when you need domain-specific exceptions. For business logic errors like InsufficientFundsException or InvalidOrderException, custom exceptions make code more readable and maintainable. Extend Exception for checked exceptions when the caller should be forced to handle the condition. Extend RuntimeException for unchecked exceptions representing programming errors. Always provide meaningful constructors—typically one with just a message, one with message and cause for exception chaining, and sometimes one that includes additional error details specific to your domain."

**Q10: What is the difference between Exception and Error?**

"Exception represents conditions that applications should catch and handle. They're typically recoverable and include IOException, SQLException. Error represents serious problems that applications should not try to catch, like OutOfMemoryError or StackOverflowError. Errors indicate conditions that are generally unrecoverable and often represent JVM-level issues. While both extend Throwable, best practice is to catch Exception and its subclasses but not Error. Catching Error can mask serious problems and make debugging difficult. Your code should handle Exceptions but let Errors propagate to terminate the application."

---

## 7️⃣ Common Mistakes

1. **Catching Exception or Throwable too broadly**:
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

2. **Empty catch block (swallowing exceptions)**:
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

3. **Not following exception hierarchy in catch blocks**:
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

4. **Not closing resources properly**:
   ```java
   FileReader reader = new FileReader("file.txt");
   // Use reader
   // Forgot to close - resource leak!
   
   // Use try-with-resources instead
   try (FileReader reader = new FileReader("file.txt")) {
       // Use reader
   }
   ```

5. **Catching exceptions for flow control**:
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

6. **Throwing generic Exception**:
   ```java
   public void process() throws Exception {  // Too generic
       // code
   }
   
   // Better:
   public void process() throws IOException, SQLException {
       // Specific exceptions
   }
   ```

7. **Not preserving stack trace when re-throwing**:
   ```java
   catch (Exception e) {
       throw new CustomException("Error occurred");  // Lost original stack trace
   }
   
   // Better: Include cause
   catch (Exception e) {
       throw new CustomException("Error occurred", e);
   }
   ```

8. **Returning in finally (overrides try/catch return)**:
   ```java
   try {
       return 1;
   } finally {
       return 2;  // This overrides the return from try
   }
   // Returns 2, not 1
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Checked exceptions must be handled or declared (compile-time)
- Unchecked exceptions (RuntimeException) are optional (runtime)
- finally always executes (cleanup code)
- try-with-resources for automatic resource management
- Custom exceptions for domain-specific errors
- Exception chaining preserves stack trace

**What to avoid**:
- Don't say checked exceptions are "better" (depends on use case)
- Don't claim finally "never" doesn't execute (rare cases exist)
- Don't recommend catching Error classes
- Don't advocate for empty catch blocks

**Under pressure**:
- Draw exception hierarchy: Throwable → Error/Exception → RuntimeException
- Remember: throw = throw instance, throws = declare in signature
- State that specific exceptions should be caught before general
- Mention try-with-resources as best practice for resources

**Red flags to avoid**:
- "Exceptions are slow so avoid them" (misses the point—they're for errors)
- "Always catch Exception" (too broad)
- "finally is optional" (it is, but important for cleanup)
- "Checked exceptions are bad" (they have valid use cases)

**Bonus points**:
- Mention suppressed exceptions in try-with-resources
- Discuss when to use checked vs unchecked exceptions
- Reference effective exception messages with context
- Mention exception translation in layered architectures
- Discuss fail-fast vs fail-safe error handling strategies
- Reference logging frameworks for exception handling
- Mention that exceptions have performance cost (object creation, stack trace)
