# Encapsulation

## 1️⃣ Concept Overview

**Encapsulation** is one of the four fundamental OOP principles. It is the practice of:
1. **Bundling data (fields) and methods that operate on that data within a single unit (class)**
2. **Hiding internal state and requiring all interaction through methods (information hiding)**

**Core Idea**: Keep fields private and provide public getter/setter methods to access and modify them.

**Benefits**:
- **Data protection**: Prevents unauthorized or invalid modifications
- **Flexibility**: Internal implementation can change without affecting external code
- **Control**: Validation logic can be added in setters
- **Maintainability**: Changes are localized to the class

**Encapsulation = Data Hiding + Abstraction**

---

## 2️⃣ Why Interviewers Ask This

- **Fundamental OOP principle**: Tests if you understand core object-oriented concepts
- **Design skills**: Shows if you can design maintainable, secure classes
- **Real-world relevance**: Used in every production Java application
- **Separates junior from senior**: Juniors know the concept; seniors know when and why to apply it
- **Security awareness**: Understanding data protection and validation

---

## 3️⃣ Key Rules / Facts

**Implementation Rules**:
- Declare class variables/fields as `private`
- Provide public `getter` methods to read values
- Provide public `setter` methods to modify values
- Add validation logic in setters to maintain data integrity
- Use meaningful method names: `getName()`, `setName(String name)`

**Access Modifiers** (from most to least restrictive):
- `private`: Accessible only within the same class
- `default` (no modifier): Accessible within the same package
- `protected`: Accessible within same package and subclasses
- `public`: Accessible from anywhere

**Naming Conventions**:
- Getter: `getPropertyName()` (or `isPropertyName()` for boolean)
- Setter: `setPropertyName(Type value)`
- Follow JavaBeans convention for frameworks

**Best Practices**:
- Don't provide setters if field should be read-only
- Don't return mutable objects directly (defensive copying)
- Validate input in setters
- Initialize fields to valid states
- Use `final` for immutable fields

**Common Patterns**:
- **Read-only class**: Getters but no setters
- **Immutable class**: All fields final, no setters
- **Fluent setters**: Return `this` for method chaining

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Basic Encapsulation

```java
public class Employee {
    // Private fields - hidden from outside
    private String name;
    private int age;
    private double salary;
    
    // Public getter for name
    public String getName() {
        return name;
    }
    
    // Public setter for name with validation
    public void setName(String name) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be null or empty");
        }
        this.name = name;
    }
    
    // Public getter for age
    public int getAge() {
        return age;
    }
    
    // Public setter for age with validation
    public void setAge(int age) {
        if (age < 18 || age > 65) {
            throw new IllegalArgumentException("Age must be between 18 and 65");
        }
        this.age = age;
    }
    
    // Public getter for salary
    public double getSalary() {
        return salary;
    }
    
    // Public setter for salary with validation
    public void setSalary(double salary) {
        if (salary < 0) {
            throw new IllegalArgumentException("Salary cannot be negative");
        }
        this.salary = salary;
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Employee emp = new Employee();
        
        // Cannot access directly: emp.name = "John";  // Compilation error!
        
        // Must use setters
        emp.setName("John Doe");
        emp.setAge(30);
        emp.setSalary(50000);
        
        // Use getters to access
        System.out.println("Name: " + emp.getName());      // John Doe
        System.out.println("Age: " + emp.getAge());        // 30
        System.out.println("Salary: " + emp.getSalary());  // 50000.0
        
        // Validation in action
        try {
            emp.setAge(70);  // Throws exception
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Example 2: Read-Only Properties

```java
public class BankAccount {
    private final String accountNumber;  // Read-only (final)
    private double balance;
    
    public BankAccount(String accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }
    
    // Getter for account number - NO setter (read-only)
    public String getAccountNumber() {
        return accountNumber;
    }
    
    // Getter for balance
    public double getBalance() {
        return balance;
    }
    
    // Controlled modification through business methods, not direct setters
    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Deposit amount must be positive");
        }
        balance += amount;
    }
    
    public void withdraw(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Withdrawal amount must be positive");
        }
        if (amount > balance) {
            throw new IllegalArgumentException("Insufficient funds");
        }
        balance -= amount;
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("ACC123", 1000.0);
        
        // Can read account number
        System.out.println("Account: " + account.getAccountNumber());  // ACC123
        
        // Cannot modify account number (no setter exists)
        // account.setAccountNumber("ACC456");  // Compilation error!
        
        // Controlled balance modification
        account.deposit(500);
        account.withdraw(200);
        System.out.println("Balance: " + account.getBalance());  // 1300.0
    }
}
```

### Example 3: Defensive Copying

```java
import java.util.Date;

// BAD: Exposing mutable internal state
class BadPerson {
    private Date birthDate;
    
    public Date getBirthDate() {
        return birthDate;  // Returns reference to internal mutable object
    }
    
    public void setBirthDate(Date birthDate) {
        this.birthDate = birthDate;  // Stores reference to external object
    }
}

// GOOD: Defensive copying
class GoodPerson {
    private Date birthDate;
    
    // Defensive copy in getter
    public Date getBirthDate() {
        return (birthDate == null) ? null : new Date(birthDate.getTime());
    }
    
    // Defensive copy in setter
    public void setBirthDate(Date birthDate) {
        this.birthDate = (birthDate == null) ? null : new Date(birthDate.getTime());
    }
}

// Demo
class Main {
    public static void main(String[] args) {
        // Problem with BadPerson
        BadPerson bad = new BadPerson();
        Date date = new Date();
        bad.setBirthDate(date);
        
        // External modification affects internal state!
        date.setTime(0);
        System.out.println("Bad: " + bad.getBirthDate());  // Modified!
        
        // Also, we can modify through getter
        bad.getBirthDate().setTime(0);
        
        // Solution with GoodPerson
        GoodPerson good = new GoodPerson();
        Date date2 = new Date();
        good.setBirthDate(date2);
        
        // External modification doesn't affect internal state
        date2.setTime(0);
        System.out.println("Good: " + good.getBirthDate());  // Not modified
        
        // Cannot modify through getter
        good.getBirthDate().setTime(0);  // Modifies copy, not original
    }
}
```

### Example 4: Fluent Interface with Method Chaining

```java
public class Person {
    private String firstName;
    private String lastName;
    private int age;
    private String email;
    
    // Fluent setters - return 'this' for chaining
    public Person setFirstName(String firstName) {
        this.firstName = firstName;
        return this;
    }
    
    public Person setLastName(String lastName) {
        this.lastName = lastName;
        return this;
    }
    
    public Person setAge(int age) {
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("Invalid age");
        }
        this.age = age;
        return this;
    }
    
    public Person setEmail(String email) {
        if (email == null || !email.contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
        this.email = email;
        return this;
    }
    
    // Regular getters
    public String getFirstName() { return firstName; }
    public String lastName() { return lastName; }
    public int getAge() { return age; }
    public String getEmail() { return email; }
    
    @Override
    public String toString() {
        return firstName + " " + lastName + ", " + age + ", " + email;
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        // Method chaining for fluent API
        Person person = new Person()
            .setFirstName("John")
            .setLastName("Doe")
            .setAge(30)
            .setEmail("john@example.com");
        
        System.out.println(person);  // John Doe, 30, john@example.com
    }
}
```

### Example 5: Immutable Class (Ultimate Encapsulation)

```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final Date birthDate;
    
    public ImmutablePerson(String name, int age, Date birthDate) {
        this.name = name;
        this.age = age;
        // Defensive copy in constructor
        this.birthDate = new Date(birthDate.getTime());
    }
    
    // Only getters, no setters
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    // Defensive copy in getter
    public Date getBirthDate() {
        return new Date(birthDate.getTime());
    }
    
    // To "modify" an immutable object, create a new one
    public ImmutablePerson withAge(int newAge) {
        return new ImmutablePerson(this.name, newAge, this.birthDate);
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Date date = new Date();
        ImmutablePerson person = new ImmutablePerson("John", 30, date);
        
        // Cannot modify
        // person.setAge(31);  // No setter exists
        
        // External changes don't affect internal state
        date.setTime(0);
        System.out.println(person.getBirthDate());  // Original date preserved
        
        // To "change" age, create new object
        ImmutablePerson olderPerson = person.withAge(31);
        System.out.println("Original age: " + person.getAge());      // 30
        System.out.println("New object age: " + olderPerson.getAge());  // 31
    }
}
```

### Example 6: Access Modifiers Demonstration

```java
// File: com/example/demo/Parent.java
package com.example.demo;

public class Parent {
    private int privateField = 10;
    int defaultField = 20;            // package-private
    protected int protectedField = 30;
    public int publicField = 40;
    
    private void privateMethod() {
        System.out.println("Private method");
    }
    
    void defaultMethod() {
        System.out.println("Default method");
    }
    
    protected void protectedMethod() {
        System.out.println("Protected method");
    }
    
    public void publicMethod() {
        System.out.println("Public method");
    }
    
    public void testAccess() {
        // All accessible within same class
        System.out.println(privateField);    // OK
        System.out.println(defaultField);    // OK
        System.out.println(protectedField);  // OK
        System.out.println(publicField);     // OK
        privateMethod();    // OK
        defaultMethod();    // OK
        protectedMethod();  // OK
        publicMethod();     // OK
    }
}

// File: com/example/demo/SamePackage.java
package com.example.demo;

public class SamePackage {
    public void testAccess() {
        Parent p = new Parent();
        // System.out.println(p.privateField);    // Error: private
        System.out.println(p.defaultField);       // OK: same package
        System.out.println(p.protectedField);     // OK: same package
        System.out.println(p.publicField);        // OK: public
    }
}

// File: com/example/other/DifferentPackage.java
package com.example.other;
import com.example.demo.Parent;

public class DifferentPackage {
    public void testAccess() {
        Parent p = new Parent();
        // System.out.println(p.privateField);    // Error: private
        // System.out.println(p.defaultField);    // Error: different package
        // System.out.println(p.protectedField);  // Error: not subclass access
        System.out.println(p.publicField);        // OK: public
    }
}

// File: com/example/other/Child.java
package com.example.other;
import com.example.demo.Parent;

public class Child extends Parent {
    public void testAccess() {
        // System.out.println(privateField);    // Error: private
        // System.out.println(defaultField);    // Error: different package
        System.out.println(protectedField);     // OK: inherited, accessed in subclass
        System.out.println(publicField);        // OK: public
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is encapsulation in Java?**
2. **Why do we need encapsulation?**
3. **What is the difference between encapsulation and abstraction?**
4. **How do you achieve encapsulation in Java?**
5. **What are getters and setters?**
6. **Can we have a class with only getters and no setters?**
7. **What is defensive copying and why is it important?**
8. **What are the four access modifiers in Java?**
9. **Is it mandatory to make all fields private for encapsulation?**
10. **How does encapsulation provide data hiding?**

---

## 6️⃣ Model Answers

**Q1: What is encapsulation in Java?**

"Encapsulation is the OOP principle of bundling data and methods that operate on that data within a single class, while hiding the internal state from outside access. It's achieved by making fields private and providing public getter and setter methods to control access. This protects data integrity by preventing unauthorized or invalid modifications and allows internal implementation changes without affecting external code."

**Q2: Why do we need encapsulation?**

"We need encapsulation for several reasons. First, it provides data protection by preventing direct access to fields, which could lead to invalid states. Second, it offers flexibility—we can change internal implementation without breaking external code. Third, we can add validation logic in setters to maintain data integrity. Fourth, it improves maintainability by localizing changes to the class. Finally, it provides better control over how data is accessed and modified, which is essential for building robust applications."

**Q3: What is the difference between encapsulation and abstraction?**

"Encapsulation is about hiding internal data and implementation details by bundling them in a class and controlling access through methods. It focuses on 'how' to achieve data hiding using access modifiers. Abstraction is about hiding complexity and showing only essential features to the user. It focuses on 'what' an object does rather than 'how' it does it. Encapsulation is a mechanism to achieve abstraction, but they serve different purposes—encapsulation protects data, while abstraction reduces complexity."

**Q4: How do you achieve encapsulation in Java?**

"To achieve encapsulation, you follow these steps: First, declare all fields as private. Second, provide public getter methods to read field values. Third, provide public setter methods to modify field values, including validation logic. Fourth, ensure setters validate input to maintain data integrity. Optionally, you can make fields final and omit setters for immutable classes, or use defensive copying for mutable objects to prevent external modifications."

**Q5: What are getters and setters?**

"Getters and setters are public methods that provide controlled access to private fields. Getters retrieve field values and typically follow the naming convention getName() for a field called 'name', or isFlag() for boolean fields. Setters modify field values and follow the convention setName(Type value). They're important because they allow you to add validation, logging, or other logic when accessing or modifying fields, rather than allowing direct field access."

**Q6: Can we have a class with only getters and no setters?**

"Yes, absolutely. This creates a read-only or immutable class. It's common when you want to expose data but prevent external modification. For example, in a BankAccount class, you might provide a getter for the account number but no setter because account numbers shouldn't change after creation. For immutable classes like String or LocalDate, all fields are final and only getters are provided, ensuring the object's state never changes after construction."

**Q7: What is defensive copying and why is it important?**

"Defensive copying is creating a copy of a mutable object when storing it or returning it, rather than using the reference directly. It's important because if you store a reference to an external mutable object, that object can be modified externally, breaking encapsulation. Similarly, if you return a reference to an internal mutable object, callers can modify it. By creating copies in getters and setters, you ensure the internal state remains protected. This is especially important for mutable objects like Date, arrays, or collections."

**Q8: What are the four access modifiers in Java?**

"Java has four access modifiers, from most to least restrictive: private—accessible only within the same class; default or package-private—accessible within the same package, specified by using no modifier; protected—accessible within the same package and by subclasses in other packages; and public—accessible from anywhere. These modifiers control encapsulation by determining what parts of a class are visible to the outside world."

**Q9: Is it mandatory to make all fields private for encapsulation?**

"While it's best practice and recommended for proper encapsulation, it's not strictly mandatory in all cases. Fields can be public if there's a valid reason, though this is rare. Constants are typically public static final. Protected fields might be used in inheritance hierarchies where controlled access by subclasses is intended. However, for standard encapsulation and data hiding, private fields with public getters and setters is the recommended approach. The goal is to control access, and private gives you the most control."

**Q10: How does encapsulation provide data hiding?**

"Encapsulation provides data hiding by making fields private, which prevents direct access from outside the class. All interaction with the data must go through public methods—getters and setters. This gives the class complete control over how its data is accessed and modified. You can add validation in setters to reject invalid data, implement business logic in getters, or even change the internal representation of data without affecting external code, as long as the public interface remains the same. This separation of interface and implementation is the essence of data hiding."

---

## 7️⃣ Common Mistakes

1. **Making fields public instead of private**:
   ```java
   public class Person {
       public String name;  // BAD: Direct access, no control
       public int age;
   }
   ```

2. **Forgetting validation in setters**:
   ```java
   public void setAge(int age) {
       this.age = age;  // BAD: Allows negative or invalid ages
   }
   // GOOD: if (age < 0) throw new IllegalArgumentException();
   ```

3. **Returning mutable objects directly**:
   ```java
   public Date getBirthDate() {
       return birthDate;  // BAD: Caller can modify internal state
   }
   // GOOD: return new Date(birthDate.getTime());
   ```

4. **Storing references to mutable parameters**:
   ```java
   public void setItems(List<String> items) {
       this.items = items;  // BAD: External list can be modified
   }
   // GOOD: this.items = new ArrayList<>(items);
   ```

5. **Providing setters for fields that shouldn't change**:
   ```java
   private String accountNumber;
   public void setAccountNumber(String num) { ... }  // BAD: Should be read-only
   ```

6. **Using wrong getter convention for boolean**:
   ```java
   private boolean active;
   public boolean getActive() { ... }  // Should be isActive()
   ```

7. **Not using `this` keyword when needed**:
   ```java
   public void setName(String name) {
       name = name;  // BAD: Assigns parameter to itself, doesn't set field
   }
   // GOOD: this.name = name;
   ```

8. **Breaking encapsulation with package-private access**:
   ```java
   int salary;  // Default access: accessible from same package
   // Better: private int salary; with getters/setters
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Encapsulation = data hiding through private fields + public methods
- Primary benefits: data protection, flexibility, validation, maintainability
- Difference from abstraction (related but different concepts)
- Real-world importance in all production code
- Defensive copying for mutable objects

**What to avoid**:
- Don't confuse encapsulation with abstraction (common mistake)
- Don't claim encapsulation is only about getters/setters (it's broader)
- Don't say "encapsulation makes code slower" (minimal overhead, huge benefits)
- Don't forget to mention validation as a key benefit

**Under pressure**:
- Start with the simple definition: "hiding data and providing controlled access"
- Give a quick example: BankAccount with private balance and deposit/withdraw methods
- Mention the four access modifiers if asked about implementation
- Draw a simple class diagram showing private fields and public methods

**Red flags to avoid**:
- "Public fields are fine if you're careful" (violates encapsulation)
- "Getters and setters are just boilerplate" (they provide control and validation)
- "Encapsulation and abstraction are the same" (they're related but distinct)
- "You don't need defensive copying" (depends on mutability)

**Bonus points**:
- Mention JavaBeans convention (important for frameworks)
- Discuss immutability as the ultimate encapsulation
- Reference builder pattern as an alternative to many setters
- Mention that encapsulation enables loose coupling
- Discuss how it relates to the Open/Closed Principle (open for extension, closed for modification)
