# Abstraction

## 1️⃣ Concept Overview

**Abstraction** is the OOP principle of hiding implementation details and showing only essential features to the user. It focuses on **what** an object does rather than **how** it does it.

**Key Concepts**:
- **Abstract Class**: A class declared with `abstract` keyword that cannot be instantiated
- **Abstract Method**: A method declared without implementation (no body)
- **Concrete Class**: A non-abstract class that provides implementation for all methods
- **Interface**: A reference type that contains only abstract methods (until Java 8)

**Two Ways to Achieve Abstraction in Java**:
1. **Abstract Classes** (0-100% abstraction)
2. **Interfaces** (100% abstraction in Java 7, partial in Java 8+)

**Benefits**:
- Reduces complexity by hiding implementation details
- Enables code reusability through common interfaces
- Supports loose coupling
- Facilitates easier maintenance and updates
- Defines contracts that implementing classes must follow

**Real-world analogy**: You know how to drive a car (interface) without knowing how the engine works (implementation).

---

## 2️⃣ Why Interviewers Ask This

- **Fundamental OOP principle**: Core to object-oriented design
- **Design skills**: Tests ability to create flexible architectures
- **Interface vs abstract class**: Classic interview question
- **Framework knowledge**: Most frameworks heavily use abstraction
- **Real-world application**: Essential for building maintainable systems
- **API design**: Shows understanding of contract-based programming

---

## 3️⃣ Key Rules / Facts

**Abstract Classes**:
- Cannot be instantiated (cannot create objects)
- Declared using `abstract` keyword
- Can have abstract methods (no body) and concrete methods (with body)
- Can have constructors (called when child object is created)
- Can have instance variables (fields)
- Can have static methods and variables
- Child class must implement all abstract methods or be abstract itself
- Supports single inheritance only (can extend one abstract class)

**Abstract Methods**:
- Declared without body: `public abstract void method();`
- Must be in an abstract class or interface
- Cannot be `private`, `final`, or `static`
- Must be implemented by first concrete child class

**Interfaces** (Java 8):
- 100% abstract by default (before Java 8)
- All methods implicitly `public abstract` (before Java 8)
- All fields implicitly `public static final` (constants)
- Cannot have constructors
- Cannot have instance variables
- A class can implement multiple interfaces (multiple inheritance)
- From Java 8: Can have default methods and static methods
- From Java 9: Can have private methods

**Abstract Class vs Interface**:
| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Instantiation | Cannot instantiate | Cannot instantiate |
| Methods | Abstract + Concrete | Abstract (+ default/static in Java 8+) |
| Variables | Instance + Static | Only static final (constants) |
| Constructor | Yes | No |
| Inheritance | Single (extends) | Multiple (implements) |
| Access Modifiers | Any | Public only (methods) |
| Use When | IS-A relationship | CAN-DO capability |

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Basic Abstract Class

```java
// Abstract class
abstract class Animal {
    protected String name;
    
    // Constructor in abstract class
    public Animal(String name) {
        this.name = name;
    }
    
    // Abstract method - no body
    public abstract void makeSound();
    
    // Abstract method
    public abstract void move();
    
    // Concrete method - has body
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    // Concrete method
    public void eat() {
        System.out.println(name + " is eating");
    }
}

// Concrete class - must implement all abstract methods
class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " barks: Woof! Woof!");
    }
    
    @Override
    public void move() {
        System.out.println(name + " runs on four legs");
    }
}

class Bird extends Animal {
    public Bird(String name) {
        super(name);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " chirps: Tweet! Tweet!");
    }
    
    @Override
    public void move() {
        System.out.println(name + " flies in the sky");
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        // Cannot instantiate abstract class
        // Animal animal = new Animal("Generic");  // Compilation error!
        
        // Create concrete objects
        Animal dog = new Dog("Buddy");
        Animal bird = new Bird("Tweety");
        
        dog.makeSound();   // Buddy barks: Woof! Woof!
        dog.move();        // Buddy runs on four legs
        dog.sleep();       // Buddy is sleeping (inherited concrete method)
        
        System.out.println();
        
        bird.makeSound();  // Tweety chirps: Tweet! Tweet!
        bird.move();       // Tweety flies in the sky
        bird.eat();        // Tweety is eating (inherited concrete method)
    }
}
```

### Example 2: Basic Interface

```java
// Interface - defines contract
interface Drawable {
    // Abstract method (implicitly public abstract)
    void draw();
    
    // Another abstract method
    void resize(int width, int height);
}

// Class implementing interface
class Circle implements Drawable {
    private int radius;
    
    public Circle(int radius) {
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing circle with radius: " + radius);
    }
    
    @Override
    public void resize(int width, int height) {
        this.radius = Math.min(width, height) / 2;
        System.out.println("Circle resized to radius: " + radius);
    }
}

class Rectangle implements Drawable {
    private int width;
    private int height;
    
    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing rectangle: " + width + "x" + height);
    }
    
    @Override
    public void resize(int width, int height) {
        this.width = width;
        this.height = height;
        System.out.println("Rectangle resized to: " + width + "x" + height);
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Drawable circle = new Circle(10);
        Drawable rectangle = new Rectangle(20, 30);
        
        circle.draw();              // Drawing circle with radius: 10
        circle.resize(50, 50);      // Circle resized to radius: 25
        
        rectangle.draw();           // Drawing rectangle: 20x30
        rectangle.resize(40, 60);   // Rectangle resized to: 40x60
    }
}
```

### Example 3: Multiple Interface Implementation

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

// Class implementing multiple interfaces
class Duck implements Flyable, Swimmable {
    private String name;
    
    public Duck(String name) {
        this.name = name;
    }
    
    @Override
    public void fly() {
        System.out.println(name + " is flying");
    }
    
    @Override
    public void swim() {
        System.out.println(name + " is swimming");
    }
}

class Fish implements Swimmable {
    private String name;
    
    public Fish(String name) {
        this.name = name;
    }
    
    @Override
    public void swim() {
        System.out.println(name + " is swimming underwater");
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Duck duck = new Duck("Donald");
        duck.fly();   // Donald is flying
        duck.swim();  // Donald is swimming
        
        Fish fish = new Fish("Nemo");
        fish.swim();  // Nemo is swimming underwater
        
        // Polymorphism with interfaces
        Swimmable swimmer1 = new Duck("Daisy");
        Swimmable swimmer2 = new Fish("Dory");
        
        swimmer1.swim();  // Daisy is swimming
        swimmer2.swim();  // Dory is swimming underwater
    }
}
```

### Example 4: Abstract Class with Interface

```java
interface Vehicle {
    void start();
    void stop();
}

// Abstract class implementing interface
abstract class AbstractVehicle implements Vehicle {
    protected String model;
    protected boolean isRunning;
    
    public AbstractVehicle(String model) {
        this.model = model;
        this.isRunning = false;
    }
    
    // Implement some interface methods
    @Override
    public void start() {
        isRunning = true;
        System.out.println(model + " started");
    }
    
    @Override
    public void stop() {
        isRunning = false;
        System.out.println(model + " stopped");
    }
    
    // Add abstract method
    public abstract void accelerate();
}

class Car extends AbstractVehicle {
    public Car(String model) {
        super(model);
    }
    
    @Override
    public void accelerate() {
        System.out.println(model + " accelerating on road");
    }
}

class Boat extends AbstractVehicle {
    public Boat(String model) {
        super(model);
    }
    
    @Override
    public void accelerate() {
        System.out.println(model + " accelerating on water");
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Vehicle car = new Car("Tesla");
        car.start();         // Tesla started
        ((Car) car).accelerate();  // Tesla accelerating on road
        car.stop();          // Tesla stopped
    }
}
```

### Example 5: Interface with Constants

```java
interface MathConstants {
    // Fields in interface are implicitly public static final
    double PI = 3.14159;
    double E = 2.71828;
    int MAX_VALUE = 1000;
}

class Calculator implements MathConstants {
    public double calculateCircleArea(double radius) {
        return PI * radius * radius;  // Using constant from interface
    }
    
    public double calculateExponential(double power) {
        return Math.pow(E, power);
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        System.out.println("Circle area: " + calc.calculateCircleArea(5));
        System.out.println("e^2: " + calc.calculateExponential(2));
        
        // Can access constants directly from interface
        System.out.println("PI: " + MathConstants.PI);
        System.out.println("MAX: " + MathConstants.MAX_VALUE);
        
        // Cannot modify - compilation error
        // MathConstants.PI = 3.14;  // Error: cannot assign to final variable
    }
}
```

### Example 6: Default Methods in Interfaces (Java 8)

```java
interface Logger {
    // Abstract method
    void log(String message);
    
    // Default method (Java 8+)
    default void logError(String error) {
        log("ERROR: " + error);
    }
    
    default void logWarning(String warning) {
        log("WARNING: " + warning);
    }
    
    // Static method (Java 8+)
    static void logInfo(String info) {
        System.out.println("INFO: " + info);
    }
}

class ConsoleLogger implements Logger {
    @Override
    public void log(String message) {
        System.out.println("[Console] " + message);
    }
    
    // Can optionally override default methods
    @Override
    public void logError(String error) {
        System.out.println("[Console ERROR] " + error + " !!!");
    }
}

class FileLogger implements Logger {
    @Override
    public void log(String message) {
        System.out.println("[File] " + message);
    }
    
    // Inherits default methods without overriding
}

// Usage
class Main {
    public static void main(String[] args) {
        Logger console = new ConsoleLogger();
        console.log("Regular message");           // [Console] Regular message
        console.logError("Something went wrong"); // [Console ERROR] Something went wrong !!!
        console.logWarning("Be careful");         // [Console] WARNING: Be careful
        
        Logger file = new FileLogger();
        file.log("File message");                 // [File] File message
        file.logError("File error");              // [File] ERROR: File error
        
        // Static method called on interface
        Logger.logInfo("Application started");    // INFO: Application started
    }
}
```

### Example 7: Real-World Example - Payment System

```java
// Interface for payment processing
interface PaymentProcessor {
    boolean processPayment(double amount);
    void refund(double amount);
    String getPaymentMethod();
}

// Abstract class with common functionality
abstract class AbstractPaymentProcessor implements PaymentProcessor {
    protected double transactionFee;
    
    public AbstractPaymentProcessor(double transactionFee) {
        this.transactionFee = transactionFee;
    }
    
    // Common validation method
    protected boolean validateAmount(double amount) {
        return amount > 0;
    }
    
    // Template method
    public final boolean processPaymentWithFee(double amount) {
        if (!validateAmount(amount)) {
            System.out.println("Invalid amount");
            return false;
        }
        
        double totalAmount = amount + transactionFee;
        System.out.println("Processing payment of $" + amount + " (Fee: $" + transactionFee + ")");
        return processPayment(totalAmount);
    }
    
    // Abstract method - to be implemented by subclasses
    @Override
    public abstract boolean processPayment(double amount);
}

// Concrete implementation 1
class CreditCardProcessor extends AbstractPaymentProcessor {
    private String cardNumber;
    
    public CreditCardProcessor(String cardNumber) {
        super(2.50);  // $2.50 transaction fee
        this.cardNumber = cardNumber;
    }
    
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Charging credit card ****" + cardNumber.substring(12) + ": $" + amount);
        return true;
    }
    
    @Override
    public void refund(double amount) {
        System.out.println("Refunding $" + amount + " to credit card");
    }
    
    @Override
    public String getPaymentMethod() {
        return "Credit Card";
    }
}

// Concrete implementation 2
class PayPalProcessor extends AbstractPaymentProcessor {
    private String email;
    
    public PayPalProcessor(String email) {
        super(1.50);  // $1.50 transaction fee
        this.email = email;
    }
    
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Processing PayPal payment to " + email + ": $" + amount);
        return true;
    }
    
    @Override
    public void refund(double amount) {
        System.out.println("Refunding $" + amount + " to PayPal account");
    }
    
    @Override
    public String getPaymentMethod() {
        return "PayPal";
    }
}

// Usage
class Main {
    public static void executePayment(PaymentProcessor processor, double amount) {
        System.out.println("Payment method: " + processor.getPaymentMethod());
        if (processor instanceof AbstractPaymentProcessor) {
            ((AbstractPaymentProcessor) processor).processPaymentWithFee(amount);
        } else {
            processor.processPayment(amount);
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        PaymentProcessor cc = new CreditCardProcessor("1234567890123456");
        PaymentProcessor pp = new PayPalProcessor("user@example.com");
        
        executePayment(cc, 100.00);
        executePayment(pp, 200.00);
    }
}
```

### Example 8: Achieving 100% Abstraction

```java
// 100% abstraction using interface (all methods abstract)
interface Database {
    void connect();
    void disconnect();
    void executeQuery(String query);
}

// Partial abstraction using abstract class
abstract class AbstractDatabase {
    protected String connectionString;
    
    public AbstractDatabase(String connectionString) {
        this.connectionString = connectionString;
    }
    
    // Concrete method
    public void printConnection() {
        System.out.println("Connected to: " + connectionString);
    }
    
    // Abstract methods
    public abstract void connect();
    public abstract void disconnect();
}

// MySQL implementation
class MySQLDatabase implements Database {
    private String host;
    
    public MySQLDatabase(String host) {
        this.host = host;
    }
    
    @Override
    public void connect() {
        System.out.println("Connecting to MySQL at " + host);
    }
    
    @Override
    public void disconnect() {
        System.out.println("Disconnecting from MySQL");
    }
    
    @Override
    public void executeQuery(String query) {
        System.out.println("Executing MySQL query: " + query);
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Database db = new MySQLDatabase("localhost");
        db.connect();
        db.executeQuery("SELECT * FROM users");
        db.disconnect();
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is abstraction in Java?**
2. **What is the difference between abstract class and interface?**
3. **Can we instantiate an abstract class?**
4. **Can an abstract class have constructors?**
5. **Can we have abstract methods in a non-abstract class?**
6. **Can an interface have concrete methods?**
7. **When should we use abstract class vs interface?**
8. **Can we have static methods in an interface?**
9. **What is the difference between abstraction and encapsulation?**
10. **Can an abstract class implement an interface?**

---

## 6️⃣ Model Answers

**Q1: What is abstraction in Java?**

"Abstraction is the OOP principle of hiding implementation details and showing only essential features to the user. It focuses on what an object does rather than how it does it. In Java, we achieve abstraction through abstract classes and interfaces. For example, when you use a List, you don't need to know whether it's an ArrayList or LinkedList—you just use the List interface methods. This reduces complexity and allows implementation to change without affecting the client code."

**Q2: What is the difference between abstract class and interface?**

"Abstract classes can have both abstract and concrete methods, instance variables, and constructors. A class can extend only one abstract class. Interfaces, before Java 8, could only have abstract methods and constants. From Java 8, they can have default and static methods. A class can implement multiple interfaces. Use abstract classes for IS-A relationships with common code to share. Use interfaces for CAN-DO capabilities or when you need multiple inheritance. For example, a Dog IS-A Animal (abstract class) but CAN-DO Runnable, Comparable (interfaces)."

**Q3: Can we instantiate an abstract class?**

"No, we cannot directly instantiate an abstract class. Abstract classes are incomplete by design—they may have abstract methods without implementation. However, we can create references of abstract class type and assign child class objects to them. We can also create anonymous inner classes from abstract classes, but that's technically creating an instance of an unnamed subclass, not the abstract class itself."

**Q4: Can an abstract class have constructors?**

"Yes, abstract classes can have constructors. These constructors are called when a child class object is created. The child class constructor must call the parent abstract class constructor using super(). This allows the abstract class to initialize its fields. Even though you can't directly instantiate an abstract class, its constructor is essential for proper initialization of child objects that extend it."

**Q5: Can we have abstract methods in a non-abstract class?**

"No, if a class has even one abstract method, the class must be declared abstract. If you try to have an abstract method in a concrete class, you'll get a compilation error. This makes sense because concrete classes must be instantiable, and you can't instantiate a class with methods that have no implementation. If you want abstract methods, declare the class as abstract."

**Q6: Can an interface have concrete methods?**

"Yes, starting from Java 8, interfaces can have concrete methods through default methods and static methods. Default methods provide a default implementation that implementing classes can use or override. Static methods belong to the interface itself and provide utility functions. Before Java 8, interfaces could only have abstract methods, which was 100% abstraction. Java 9 added private methods in interfaces as well."

**Q7: When should we use abstract class vs interface?**

"Use abstract classes when you have an IS-A relationship and want to share common code among related classes. For example, different types of animals sharing common behavior. Use interfaces when you want to define a capability or contract that unrelated classes can implement, like Comparable or Serializable. Also, use interfaces when you need multiple inheritance. A practical rule: if classes share common implementation, use abstract class. If you're defining a contract or capability, use interface."

**Q8: Can we have static methods in an interface?**

"Yes, from Java 8 onwards, interfaces can have static methods. These methods belong to the interface itself, not to implementing classes. They're useful for utility or helper methods related to the interface. For example, Comparator interface has static methods like naturalOrder(). Static methods in interfaces cannot be overridden and must be called using the interface name, not the implementing class."

**Q9: What is the difference between abstraction and encapsulation?**

"Abstraction is about hiding complexity and showing only essential features—focusing on what an object does. It's achieved through abstract classes and interfaces. Encapsulation is about bundling data and methods together and hiding internal state using access modifiers—focusing on how to protect data. For example, a Car interface provides abstraction by defining start(), stop() methods without implementation details. Encapsulation is when the Car class keeps its engine private and provides public methods to interact with it. Abstraction is about design and interface, encapsulation is about data protection."

**Q10: Can an abstract class implement an interface?**

"Yes, an abstract class can implement an interface. The abstract class can choose to implement some, all, or none of the interface methods. Any methods not implemented remain abstract, and the concrete subclass must implement them. This is useful when you want to provide partial implementation of an interface and let subclasses complete it. It's a common pattern in frameworks where you have an abstract base class implementing an interface, providing common functionality."

---

## 7️⃣ Common Mistakes

1. **Trying to instantiate an abstract class**:
   ```java
   abstract class Animal { }
   
   Animal animal = new Animal();  // Compilation error!
   ```

2. **Forgetting to implement all abstract methods**:
   ```java
   abstract class Parent {
       abstract void method1();
       abstract void method2();
   }
   
   class Child extends Parent {
       @Override
       void method1() { }
       // Forgot method2() - Compilation error unless Child is also abstract
   }
   ```

3. **Making abstract methods private**:
   ```java
   abstract class MyClass {
       private abstract void method();  // Compilation error!
       // Abstract methods cannot be private
   }
   ```

4. **Declaring abstract methods as final**:
   ```java
   abstract class MyClass {
       final abstract void method();  // Compilation error!
       // Cannot be both final and abstract
   }
   ```

5. **Creating instance variables in interfaces** (pre-Java 8):
   ```java
   interface MyInterface {
       int value = 10;  // OK - this is a constant (public static final)
       // But you cannot have non-final variables
   }
   ```

6. **Not using @Override when implementing interface methods**:
   ```java
   interface MyInterface {
       void display();
   }
   
   class MyClass implements MyInterface {
       public void Display() { }  // Typo - should use @Override
   }
   ```

7. **Reducing access modifier when implementing interface**:
   ```java
   interface MyInterface {
       void method();  // Implicitly public
   }
   
   class MyClass implements MyInterface {
       void method() { }  // Compilation error - must be public!
   }
   ```

8. **Confusing abstract class with interface usage**:
   ```java
   // Wrong: Using interface for IS-A relationship with shared code
   interface Animal {
       default void breathe() {
           System.out.println("Breathing");
       }
   }
   
   // Better: Use abstract class when sharing implementation
   abstract class Animal {
       public void breathe() {
           System.out.println("Breathing");
       }
   }
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Abstraction hides complexity, shows only essential features
- Two ways: abstract classes (0-100%) and interfaces (100%)
- Abstract class for IS-A with shared code; interface for CAN-DO capabilities
- Abstract class = single inheritance; interface = multiple inheritance
- Java 8 added default and static methods to interfaces

**What to avoid**:
- Don't claim interfaces can have instance variables (only constants)
- Don't say abstract classes are "slower" (no significant performance difference)
- Don't confuse abstraction with encapsulation (different purposes)
- Don't forget to mention Java 8 changes for interfaces

**Under pressure**:
- Use simple examples: Vehicle (abstract class) vs Driveable (interface)
- Draw a comparison table for abstract class vs interface
- Mention real Java examples: Collections framework uses both
- State the key difference: abstract class has code, interface defines contract

**Red flags to avoid**:
- "You can instantiate abstract classes" (you cannot)
- "Interfaces can have constructors" (they cannot)
- "Abstract class and interface are the same" (they're different tools)
- "You can't have any methods with implementation in interfaces" (wrong from Java 8+)

**Bonus points**:
- Mention marker interfaces (Serializable, Cloneable)
- Discuss functional interfaces (Java 8) for lambda expressions
- Reference Template Method pattern using abstract classes
- Mention that Object class is concrete, not abstract
- Discuss how abstraction enables polymorphism
- Explain multiple inheritance with interfaces (interface A, interface B)
- Reference real frameworks: Spring uses both extensively
