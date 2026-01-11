# Polymorphism

## 1️⃣ Concept Overview

**Polymorphism** means "many forms." It's the ability of an object to take on many forms or the ability of a method to perform different tasks based on the object that is acting upon.

**Two Types of Polymorphism**:

1. **Compile-time Polymorphism (Static Polymorphism)**:
   - Achieved through **method overloading**
   - Method resolution happens at compile-time
   - Same method name, different parameters
   - Also called static binding or early binding

2. **Runtime Polymorphism (Dynamic Polymorphism)**:
   - Achieved through **method overriding**
   - Method resolution happens at runtime
   - Parent reference pointing to child object
   - Also called dynamic binding or late binding

**Key Principle**: "One interface, multiple implementations"

The most powerful use of polymorphism is when a parent class reference can refer to a child class object and invoke the child's overridden methods at runtime.

---

## 2️⃣ Why Interviewers Ask This

- **Core OOP principle**: Polymorphism is fundamental to object-oriented design
- **Design flexibility**: Tests understanding of flexible, extensible code
- **Common in frameworks**: Most frameworks heavily use polymorphism
- **Runtime behavior**: Shows understanding of how Java resolves methods
- **Design patterns**: Many patterns rely on polymorphism (Strategy, Template, Factory)
- **Interface vs implementation**: Tests separation of concerns knowledge

---

## 3️⃣ Key Rules / Facts

**Method Overloading (Compile-time)**:
- Same method name, different parameter list (type, number, or order)
- Return type alone is NOT sufficient for overloading
- Can be in same class or parent-child relationship
- Resolved at compile-time based on reference type
- Access modifiers can be different

**Method Overriding (Runtime)**:
- Same method signature (name and parameters)
- Must have IS-A relationship (inheritance or interface implementation)
- Child class provides specific implementation
- Return type must be same or covariant (subtype)
- Cannot reduce visibility (public → protected is not allowed)
- Cannot override: final, static, or private methods
- Resolved at runtime based on actual object type

**@Override Annotation**:
- Compile-time check for proper overriding
- Not mandatory but highly recommended
- Catches typos and signature mismatches

**Dynamic Method Dispatch**:
- JVM determines which method to call at runtime
- Based on actual object type, not reference type
- Core mechanism for runtime polymorphism

**Covariant Return Types** (Java 5+):
- Overriding method can return subtype of parent's return type
- Example: Parent returns `Animal`, child can return `Dog`

**Rules for Overriding**:
- Method name must be identical
- Parameter list must be identical
- Return type must be same or covariant subtype
- Access modifier must be same or less restrictive
- Cannot throw new or broader checked exceptions

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Compile-time Polymorphism (Method Overloading)

```java
public class Calculator {
    // Method overloading - same name, different parameters
    
    // Method 1: Two integers
    public int add(int a, int b) {
        return a + b;
    }
    
    // Method 2: Three integers
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // Method 3: Two doubles
    public double add(double a, double b) {
        return a + b;
    }
    
    // Method 4: Different parameter order
    public void display(String message, int number) {
        System.out.println(message + ": " + number);
    }
    
    public void display(int number, String message) {
        System.out.println(number + ": " + message);
    }
    
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        System.out.println(calc.add(5, 10));           // 15 (calls method 1)
        System.out.println(calc.add(5, 10, 15));       // 30 (calls method 2)
        System.out.println(calc.add(5.5, 10.5));       // 16.0 (calls method 3)
        
        calc.display("Number", 42);     // Number: 42 (calls method 4)
        calc.display(42, "Number");     // 42: Number (calls method 5)
    }
}
```

### Example 2: Runtime Polymorphism (Method Overriding)

```java
// Parent class
class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
    
    public void sleep() {
        System.out.println("Animal is sleeping");
    }
}

// Child class 1
class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks: Woof! Woof!");
    }
}

// Child class 2
class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows: Meow! Meow!");
    }
}

// Child class 3
class Cow extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cow moos: Moo! Moo!");
    }
}

// Main class
class Main {
    public static void main(String[] args) {
        // Parent reference, child object - polymorphism!
        Animal animal1 = new Dog();
        Animal animal2 = new Cat();
        Animal animal3 = new Cow();
        
        // Runtime polymorphism - method resolved at runtime
        animal1.makeSound();  // Dog barks: Woof! Woof!
        animal2.makeSound();  // Cat meows: Meow! Meow!
        animal3.makeSound();  // Cow moos: Moo! Moo!
        
        // Method not overridden - calls parent's version
        animal1.sleep();  // Animal is sleeping
    }
}
```

### Example 3: Polymorphism with Arrays

```java
class Shape {
    public double getArea() {
        return 0.0;
    }
    
    public void display() {
        System.out.println("This is a shape with area: " + getArea());
    }
}

class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double length;
    private double width;
    
    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }
    
    @Override
    public double getArea() {
        return length * width;
    }
}

class Triangle extends Shape {
    private double base;
    private double height;
    
    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }
    
    @Override
    public double getArea() {
        return 0.5 * base * height;
    }
}

// Main class
class Main {
    public static void main(String[] args) {
        // Array of parent type holding child objects
        Shape[] shapes = new Shape[3];
        shapes[0] = new Circle(5.0);
        shapes[1] = new Rectangle(4.0, 6.0);
        shapes[2] = new Triangle(3.0, 4.0);
        
        // Polymorphic method calls
        for (Shape shape : shapes) {
            shape.display();  // Calls correct getArea() for each object
        }
        
        // Output:
        // This is a shape with area: 78.53981633974483
        // This is a shape with area: 24.0
        // This is a shape with area: 6.0
    }
}
```

### Example 4: Dynamic Method Dispatch

```java
class Parent {
    public void method1() {
        System.out.println("Parent method1");
    }
    
    public void method2() {
        System.out.println("Parent method2");
    }
}

class Child extends Parent {
    @Override
    public void method1() {
        System.out.println("Child method1");
    }
    
    // Child-specific method
    public void method3() {
        System.out.println("Child method3");
    }
}

class Main {
    public static void main(String[] args) {
        // Reference type: Parent, Object type: Child
        Parent ref = new Child();
        
        // Calls Child's version (overridden) - runtime polymorphism
        ref.method1();  // Child method1
        
        // Calls Parent's version (not overridden)
        ref.method2();  // Parent method2
        
        // Compilation error: method3() not in Parent
        // ref.method3();  // Error!
        
        // To call Child-specific methods, need downcasting
        if (ref instanceof Child) {
            Child child = (Child) ref;
            child.method3();  // Child method3
        }
    }
}
```

### Example 5: Polymorphism with Interfaces

```java
// Interface
interface Payment {
    void processPayment(double amount);
    void displayReceipt();
}

// Implementation 1
class CreditCardPayment implements Payment {
    private String cardNumber;
    
    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing credit card payment of $" + amount);
        System.out.println("Card: ****" + cardNumber.substring(cardNumber.length() - 4));
    }
    
    @Override
    public void displayReceipt() {
        System.out.println("Credit Card Receipt");
    }
}

// Implementation 2
class PayPalPayment implements Payment {
    private String email;
    
    public PayPalPayment(String email) {
        this.email = email;
    }
    
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing PayPal payment of $" + amount);
        System.out.println("Account: " + email);
    }
    
    @Override
    public void displayReceipt() {
        System.out.println("PayPal Receipt");
    }
}

// Implementation 3
class CashPayment implements Payment {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing cash payment of $" + amount);
    }
    
    @Override
    public void displayReceipt() {
        System.out.println("Cash Receipt");
    }
}

// Main class
class Main {
    // Polymorphic method - accepts any Payment implementation
    public static void makePayment(Payment payment, double amount) {
        payment.processPayment(amount);
        payment.displayReceipt();
    }
    
    public static void main(String[] args) {
        Payment payment1 = new CreditCardPayment("1234567890123456");
        Payment payment2 = new PayPalPayment("user@example.com");
        Payment payment3 = new CashPayment();
        
        // Same method call, different behavior
        makePayment(payment1, 100.0);
        System.out.println("---");
        makePayment(payment2, 200.0);
        System.out.println("---");
        makePayment(payment3, 50.0);
    }
}
```

### Example 6: Covariant Return Types

```java
class Animal {
    public Animal reproduce() {
        System.out.println("Animal reproducing");
        return new Animal();
    }
}

class Dog extends Animal {
    // Covariant return type - returns Dog instead of Animal
    @Override
    public Dog reproduce() {
        System.out.println("Dog reproducing");
        return new Dog();
    }
    
    public void bark() {
        System.out.println("Woof!");
    }
}

class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        
        // reproduce() returns Dog, not Animal
        Dog puppy = dog.reproduce();
        puppy.bark();  // Can call Dog-specific methods without casting
        
        // With polymorphism
        Animal animal = new Dog();
        Animal offspring = animal.reproduce();  // Returns Dog but stored as Animal
        
        // Need casting to access Dog-specific methods
        if (offspring instanceof Dog) {
            ((Dog) offspring).bark();
        }
    }
}
```

### Example 7: Overloading vs Overriding

```java
class Parent {
    // Original method
    public void display(int num) {
        System.out.println("Parent display(int): " + num);
    }
    
    // Overloaded method in same class
    public void display(String str) {
        System.out.println("Parent display(String): " + str);
    }
}

class Child extends Parent {
    // Overriding parent's display(int)
    @Override
    public void display(int num) {
        System.out.println("Child display(int): " + num);
    }
    
    // Overloading in child class
    public void display(double num) {
        System.out.println("Child display(double): " + num);
    }
}

class Main {
    public static void main(String[] args) {
        Child child = new Child();
        
        child.display(10);       // Child display(int): 10 (overridden)
        child.display("Hello");  // Parent display(String): Hello (inherited)
        child.display(10.5);     // Child display(double): 10.5 (overloaded)
        
        // Polymorphism
        Parent ref = new Child();
        ref.display(20);         // Child display(int): 20 (runtime polymorphism)
        ref.display("World");    // Parent display(String): World
        // ref.display(20.5);    // Compilation error - Parent doesn't have display(double)
    }
}
```

### Example 8: Real-World Example - Payment Processing System

```java
abstract class PaymentProcessor {
    protected double amount;
    
    public PaymentProcessor(double amount) {
        this.amount = amount;
    }
    
    // Template method (uses polymorphism)
    public final void processTransaction() {
        validatePayment();
        executePayment();
        sendConfirmation();
    }
    
    // Common method
    protected void validatePayment() {
        if (amount <= 0) {
            throw new IllegalArgumentException("Invalid amount");
        }
        System.out.println("Validating payment of $" + amount);
    }
    
    // Abstract method - must be overridden
    protected abstract void executePayment();
    
    // Common method
    protected void sendConfirmation() {
        System.out.println("Payment confirmation sent");
    }
}

class CreditCardProcessor extends PaymentProcessor {
    private String cardNumber;
    
    public CreditCardProcessor(double amount, String cardNumber) {
        super(amount);
        this.cardNumber = cardNumber;
    }
    
    @Override
    protected void executePayment() {
        System.out.println("Charging credit card: ****" + cardNumber.substring(12));
        System.out.println("Amount: $" + amount);
    }
}

class BankTransferProcessor extends PaymentProcessor {
    private String accountNumber;
    
    public BankTransferProcessor(double amount, String accountNumber) {
        super(amount);
        this.accountNumber = accountNumber;
    }
    
    @Override
    protected void executePayment() {
        System.out.println("Processing bank transfer to: " + accountNumber);
        System.out.println("Amount: $" + amount);
    }
}

class Main {
    public static void main(String[] args) {
        PaymentProcessor payment1 = new CreditCardProcessor(150.0, "1234567890123456");
        PaymentProcessor payment2 = new BankTransferProcessor(500.0, "ACC123456");
        
        System.out.println("Transaction 1:");
        payment1.processTransaction();
        
        System.out.println("\nTransaction 2:");
        payment2.processTransaction();
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is polymorphism in Java?**
2. **What are the types of polymorphism?**
3. **What is the difference between compile-time and runtime polymorphism?**
4. **What is method overloading? Give an example.**
5. **What is method overriding? Give an example.**
6. **Can we override static methods?**
7. **Can we overload main method?**
8. **What is dynamic method dispatch?**
9. **What is the use of @Override annotation?**
10. **What are covariant return types?**

---

## 6️⃣ Model Answers

**Q1: What is polymorphism in Java?**

"Polymorphism means 'many forms.' It's the ability of an object to take multiple forms or for a method to behave differently based on the object acting upon it. In Java, we have two types: compile-time polymorphism achieved through method overloading, and runtime polymorphism achieved through method overriding. The most powerful use is when a parent class reference points to a child class object, and the appropriate child method is invoked at runtime based on the actual object type."

**Q2: What are the types of polymorphism?**

"Java has two types of polymorphism. Compile-time or static polymorphism is achieved through method overloading, where we have multiple methods with the same name but different parameters in the same class. The compiler determines which method to call based on the arguments. Runtime or dynamic polymorphism is achieved through method overriding, where a child class provides its own implementation of a parent class method. The JVM determines which method to call at runtime based on the actual object type, not the reference type."

**Q3: What is the difference between compile-time and runtime polymorphism?**

"Compile-time polymorphism is resolved during compilation through method overloading. The compiler knows which method to call based on the method signature. It's also called static binding or early binding. Runtime polymorphism is resolved during program execution through method overriding. The JVM determines which overridden method to call based on the actual object type at runtime. It's also called dynamic binding or late binding. Runtime polymorphism is more flexible and is the foundation of many design patterns."

**Q4: What is method overloading? Give an example.**

"Method overloading is when we have multiple methods with the same name but different parameter lists in the same class. The parameters can differ in number, type, or order. For example, a Calculator class might have add(int, int), add(double, double), and add(int, int, int). The compiler selects the appropriate method based on the arguments provided. Return type alone is not sufficient for overloading—the parameter list must differ."

**Q5: What is method overriding? Give an example.**

"Method overriding is when a child class provides a specific implementation for a method already defined in its parent class. The method must have the same signature—same name and parameters. For example, if an Animal class has a makeSound() method, a Dog class can override it to print 'Woof' while a Cat class overrides it to print 'Meow.' When we call makeSound() on an Animal reference pointing to a Dog object, the Dog's version executes at runtime."

**Q6: Can we override static methods?**

"No, we cannot override static methods. Static methods belong to the class, not to instances, so they're bound at compile-time based on the reference type. If you define a static method in a child class with the same signature as the parent's static method, it's called method hiding, not overriding. The method that executes depends on the reference type, not the actual object type. This is fundamentally different from instance method overriding, which is resolved at runtime."

**Q7: Can we overload main method?**

"Yes, we can overload the main method, but only public static void main(String[] args) serves as the entry point for the JVM. You can create other main methods with different parameters like main(int num) or main(String str, int num), and they'll be treated as regular overloaded methods. However, the JVM will always look for the standard signature to start the program."

**Q8: What is dynamic method dispatch?**

"Dynamic method dispatch is the mechanism by which Java implements runtime polymorphism. When a method is called on an object reference, the JVM determines at runtime which version of the method to execute based on the actual object type, not the reference type. For example, if we have Animal ref = new Dog(), and call ref.makeSound(), the JVM dispatches the call to Dog's makeSound() method at runtime, even though the reference type is Animal. This is the core mechanism that makes polymorphism work."

**Q9: What is the use of @Override annotation?**

"The @Override annotation is a compile-time check that ensures you're actually overriding a method from the parent class. If you make a mistake in the method signature, like a typo in the method name or wrong parameters, the compiler will throw an error. It's not mandatory, but it's highly recommended because it catches errors early. For example, if you write 'public void dispaly()' instead of 'display(),' without @Override you'd create a new method, but with @Override, you'd get a compilation error alerting you to the typo."

**Q10: What are covariant return types?**

"Covariant return types, introduced in Java 5, allow an overriding method to return a subtype of the type returned by the parent method. For example, if a parent class method returns Animal, the child class can override it to return Dog, which is a subtype of Animal. This makes the code more type-safe and eliminates the need for casting. It's particularly useful in clone() methods and factory patterns where you want to return the specific type rather than a general type."

---

## 7️⃣ Common Mistakes

1. **Confusing overloading with overriding**:
   ```java
   class Parent {
       public void display(int num) { }
   }
   
   class Child extends Parent {
       // This is overloading, NOT overriding
       public void display(double num) { }
   }
   ```

2. **Trying to override with incompatible return types**:
   ```java
   class Parent {
       public int getValue() { return 10; }
   }
   
   class Child extends Parent {
       // Compilation error - incompatible return type
       // @Override
       // public String getValue() { return "10"; }
   }
   ```

3. **Reducing access modifier visibility**:
   ```java
   class Parent {
       public void method() { }
   }
   
   class Child extends Parent {
       @Override
       protected void method() { }  // Compilation error!
   }
   ```

4. **Attempting to override final methods**:
   ```java
   class Parent {
       public final void method() { }
   }
   
   class Child extends Parent {
       // @Override
       // public void method() { }  // Compilation error!
   }
   ```

5. **Assuming overloading works with inheritance automatically**:
   ```java
   class Parent {
       public void method(int a) { }
   }
   
   class Child extends Parent {
       public void method(double a) { }
   }
   
   Child obj = new Child();
   obj.method(5);  // Calls Parent's method(int), not Child's method(double)
   ```

6. **Not using instanceof before downcasting**:
   ```java
   Animal animal = new Animal();
   Dog dog = (Dog) animal;  // ClassCastException at runtime!
   
   // Should use:
   if (animal instanceof Dog) {
       Dog dog = (Dog) animal;
   }
   ```

7. **Thinking static methods can be overridden**:
   ```java
   class Parent {
       public static void method() { }
   }
   
   class Child extends Parent {
       public static void method() { }  // Hiding, not overriding
   }
   
   Parent ref = new Child();
   ref.method();  // Calls Parent's method (resolved at compile-time)
   ```

8. **Forgetting @Override annotation**:
   ```java
   class Parent {
       public void calculate() { }
   }
   
   class Child extends Parent {
       // Typo - creates new method instead of overriding
       public void Calculate() { }  // Should have used @Override to catch this
   }
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Two types: compile-time (overloading) and runtime (overriding)
- Runtime polymorphism is resolved based on object type, not reference type
- Dynamic method dispatch is the mechanism for runtime polymorphism
- Polymorphism enables flexible, extensible code
- "One interface, multiple implementations" principle

**What to avoid**:
- Don't confuse method hiding (static) with method overriding (instance)
- Don't claim overloading is only in the same class (can be in parent-child too)
- Don't say "polymorphism means method overriding" (it's broader)
- Don't forget that interfaces also enable polymorphism

**Under pressure**:
- Start with a simple example: Animal/Dog/Cat with makeSound()
- Draw it: Parent reference → Child object → method call at runtime
- Mention both types: compile-time (overloading) and runtime (overriding)
- Use the phrase "resolved at compile-time vs runtime"

**Red flags to avoid**:
- "Overloading and overriding are the same" (completely different)
- "Static methods can be overridden" (they're hidden, not overridden)
- "Return type is enough for overloading" (parameter list must differ)
- "Polymorphism only works with classes" (works with interfaces too)

**Bonus points**:
- Mention that polymorphism is key to SOLID principles (Open/Closed, Liskov Substitution)
- Discuss real design patterns: Strategy, Template Method, Factory
- Reference that all collection classes use polymorphism (List interface)
- Mention dynamic method dispatch and virtual method table (vtable)
- Explain how polymorphism enables plugin architectures
- Discuss how frameworks like Spring rely heavily on polymorphism
