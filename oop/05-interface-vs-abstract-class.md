# Interface vs Abstract Class

## 1️⃣ Concept Overview

**The Classic Interview Question**: "When should you use an interface vs an abstract class?"

This is one of the most frequently asked OOP questions because it tests your understanding of object-oriented design principles and your ability to make architectural decisions.

**Quick Summary**:
- **Abstract Class**: Use for IS-A relationships with shared implementation
- **Interface**: Use for CAN-DO capabilities or contracts without implementation sharing

**Key Principle**:
- Abstract Class = "What you ARE"
- Interface = "What you CAN DO"

**Example**:
- A Dog **IS-A** Animal (abstract class)
- A Dog **CAN-DO** Run, Bark, Swim (interfaces: Runnable, Barkable, Swimmable)

**Historical Context**:
- Before Java 8: Interfaces were purely abstract (100% abstraction)
- Java 8+: Interfaces can have default and static methods (blurs the line)
- The choice is now more about design intent than technical limitations

---

## 2️⃣ Why Interviewers Ask This

- **Tests design thinking**: Can you choose the right tool for the job?
- **Real-world relevance**: This decision affects every system you design
- **Separates levels**: Juniors know the syntax; seniors know when to use which
- **Framework knowledge**: Understanding how frameworks make these choices
- **Evolution awareness**: Shows if you know how Java has evolved (Java 8 changes)

---

## 3️⃣ Key Rules / Facts

**Complete Comparison Table**:

| Feature | Interface | Abstract Class |
|---------|-----------|----------------|
| **Keyword** | `interface` | `abstract class` |
| **Instantiation** | Cannot instantiate | Cannot instantiate |
| **Method Types** | Abstract (+ default/static in Java 8+) | Abstract + Concrete |
| **Variables** | `public static final` only (constants) | Any type (instance, static) |
| **Constructors** | No constructors | Can have constructors |
| **Access Modifiers (methods)** | Public only (implicit) | Any (public, protected, private, default) |
| **Access Modifiers (fields)** | `public static final` (implicit) | Any |
| **Multiple Inheritance** | Yes (implements multiple) | No (extends only one) |
| **Default Implementation** | Yes (default methods in Java 8+) | Yes |
| **State** | Cannot maintain state (no instance vars) | Can maintain state |
| **Use Case** | Define capabilities/contracts | Share code among related classes |
| **Relationship** | CAN-DO (capability) | IS-A (identity) |
| **Version Support** | Java 8+ has default/static methods | Always had concrete methods |
| **Performance** | Negligible difference | Negligible difference |

**When to Use Abstract Class**:
1. Related classes share common code
2. You want to provide default implementations
3. You need non-static, non-final fields
4. You want non-public members (protected, private)
5. IS-A relationship exists
6. You need constructors to initialize state

**When to Use Interface**:
1. Unrelated classes share capabilities
2. You want to specify contract only
3. You need multiple inheritance
4. You want to define a type for particular behavior
5. CAN-DO relationship exists
6. Future flexibility (can add default methods without breaking code)

**Design Guidelines**:
- Favor composition over inheritance
- Favor interfaces over abstract classes for flexibility
- Use abstract classes when you have common code to share
- Use interfaces for polymorphic behavior across unrelated classes
- Can use both: abstract class implements interface(s)

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Classic Use Case Comparison

```java
// INTERFACE - Defining a capability
interface Flyable {
    void fly();
    int getMaxAltitude();
}

// Different unrelated classes can fly
class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Bird flying with wings");
    }
    
    @Override
    public int getMaxAltitude() {
        return 10000;
    }
}

class Airplane implements Flyable {
    @Override
    public void fly() {
        System.out.println("Airplane flying with engines");
    }
    
    @Override
    public int getMaxAltitude() {
        return 40000;
    }
}

// Superman can fly too!
class Superman implements Flyable {
    @Override
    public void fly() {
        System.out.println("Superman flying with superpowers");
    }
    
    @Override
    public int getMaxAltitude() {
        return Integer.MAX_VALUE;
    }
}

// ABSTRACT CLASS - Defining common implementation
abstract class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Common concrete method
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    // Common concrete method
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    // Abstract method - each animal makes different sound
    public abstract void makeSound();
}

class Dog extends Animal {
    public Dog(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " says: Woof!");
    }
}

class Cat extends Animal {
    public Cat(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " says: Meow!");
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        // Interface polymorphism - unrelated classes
        Flyable bird = new Bird();
        Flyable plane = new Airplane();
        Flyable superman = new Superman();
        
        bird.fly();      // Bird flying with wings
        plane.fly();     // Airplane flying with engines
        superman.fly();  // Superman flying with superpowers
        
        // Abstract class polymorphism - related classes
        Animal dog = new Dog("Buddy", 3);
        Animal cat = new Cat("Whiskers", 2);
        
        dog.makeSound();  // Buddy says: Woof!
        dog.sleep();      // Buddy is sleeping (shared method)
        
        cat.makeSound();  // Whiskers says: Meow!
        cat.eat();        // Whiskers is eating (shared method)
    }
}
```

### Example 2: Multiple Inheritance with Interfaces

```java
interface Swimmable {
    void swim();
}

interface Flyable {
    void fly();
}

interface Walkable {
    void walk();
}

// Duck can do all three - multiple inheritance!
class Duck implements Swimmable, Flyable, Walkable {
    @Override
    public void swim() {
        System.out.println("Duck swimming");
    }
    
    @Override
    public void fly() {
        System.out.println("Duck flying");
    }
    
    @Override
    public void walk() {
        System.out.println("Duck walking");
    }
}

// Cannot do this with abstract classes - single inheritance only
abstract class Vehicle {
    abstract void move();
}

abstract class WaterVehicle {
    abstract void sail();
}

// Compilation error - can't extend two abstract classes
// class Amphibious extends Vehicle, WaterVehicle { }

// But you can extend one and implement multiple interfaces
class AmphibiousVehicle extends Vehicle implements Swimmable {
    @Override
    void move() {
        System.out.println("Moving on land or water");
    }
    
    @Override
    public void swim() {
        System.out.println("Swimming in water");
    }
}
```

### Example 3: State Management

```java
// Interface cannot maintain state (no instance variables)
interface Calculator {
    int add(int a, int b);
    int subtract(int a, int b);
}

// Abstract class CAN maintain state
abstract class StatefulCalculator {
    protected int memory;  // Instance variable - maintains state
    
    public StatefulCalculator() {
        this.memory = 0;
    }
    
    public void storeInMemory(int value) {
        this.memory = value;
    }
    
    public int recallMemory() {
        return this.memory;
    }
    
    public abstract int calculate(int a, int b);
}

class ScientificCalculator extends StatefulCalculator {
    @Override
    public int calculate(int a, int b) {
        int result = a * b;
        storeInMemory(result);  // Using inherited state
        return result;
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        ScientificCalculator calc = new ScientificCalculator();
        
        calc.calculate(5, 10);
        System.out.println("Memory: " + calc.recallMemory());  // 50
        
        calc.storeInMemory(100);
        System.out.println("Memory: " + calc.recallMemory());  // 100
    }
}
```

### Example 4: Constructors and Initialization

```java
// Interface - no constructors
interface Shape {
    double getArea();
    double getPerimeter();
}

// Abstract class - has constructor
abstract class AbstractShape {
    protected String color;
    protected String name;
    
    // Constructor
    public AbstractShape(String color, String name) {
        this.color = color;
        this.name = name;
        System.out.println("AbstractShape constructor called");
    }
    
    // Concrete method
    public void display() {
        System.out.println(name + " colored " + color);
    }
    
    // Abstract methods
    public abstract double getArea();
    public abstract double getPerimeter();
}

class Circle extends AbstractShape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color, "Circle");  // Must call parent constructor
        this.radius = radius;
        System.out.println("Circle constructor called");
    }
    
    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Circle circle = new Circle("Red", 5.0);
        // Output:
        // AbstractShape constructor called
        // Circle constructor called
        
        circle.display();  // Circle colored Red
        System.out.println("Area: " + circle.getArea());
    }
}
```

### Example 5: Java 8 Default Methods - Blurring the Lines

```java
// Before Java 8: Interface could only have abstract methods
// After Java 8: Interface can have default implementations

interface Payment {
    // Abstract method
    void processPayment(double amount);
    
    // Default method (Java 8+) - provides implementation
    default void validateAmount(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        System.out.println("Amount validated: $" + amount);
    }
    
    // Static method (Java 8+)
    static void printPaymentInfo() {
        System.out.println("Payment processing system v1.0");
    }
}

// Classes can use default implementation or override
class CreditCardPayment implements Payment {
    @Override
    public void processPayment(double amount) {
        validateAmount(amount);  // Using default method
        System.out.println("Processing credit card payment: $" + amount);
    }
}

class CashPayment implements Payment {
    @Override
    public void processPayment(double amount) {
        // Using default implementation
        validateAmount(amount);
        System.out.println("Processing cash payment: $" + amount);
    }
    
    // Override default method
    @Override
    public void validateAmount(double amount) {
        System.out.println("Cash payment validation");
        if (amount <= 0 || amount > 10000) {
            throw new IllegalArgumentException("Cash amount must be between $0 and $10000");
        }
    }
}
```

### Example 6: Real-World Decision Making

```java
// SCENARIO 1: Use INTERFACE for unrelated classes with common behavior

interface Sortable {
    int compare(Sortable other);
}

class Student implements Sortable {
    private int grade;
    
    public Student(int grade) {
        this.grade = grade;
    }
    
    @Override
    public int compare(Sortable other) {
        return this.grade - ((Student) other).grade;
    }
}

class Product implements Sortable {
    private double price;
    
    public Product(double price) {
        this.price = price;
    }
    
    @Override
    public int compare(Sortable other) {
        return Double.compare(this.price, ((Product) other).price);
    }
}

// SCENARIO 2: Use ABSTRACT CLASS for related classes with shared code

abstract class Employee {
    protected String name;
    protected double baseSalary;
    
    public Employee(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }
    
    // Common implementation
    public void displayInfo() {
        System.out.println("Name: " + name);
        System.out.println("Total Salary: $" + calculateSalary());
    }
    
    // Abstract method - each type calculates differently
    public abstract double calculateSalary();
}

class FullTimeEmployee extends Employee {
    private double benefits;
    
    public FullTimeEmployee(String name, double baseSalary, double benefits) {
        super(name, baseSalary);
        this.benefits = benefits;
    }
    
    @Override
    public double calculateSalary() {
        return baseSalary + benefits;
    }
}

class ContractEmployee extends Employee {
    private int hoursWorked;
    private double hourlyRate;
    
    public ContractEmployee(String name, int hoursWorked, double hourlyRate) {
        super(name, 0);
        this.hoursWorked = hoursWorked;
        this.hourlyRate = hourlyRate;
    }
    
    @Override
    public double calculateSalary() {
        return hoursWorked * hourlyRate;
    }
}
```

### Example 7: Combining Both - Abstract Class Implementing Interface

```java
interface Drawable {
    void draw();
    void resize(int percentage);
}

interface Colorable {
    void setColor(String color);
    String getColor();
}

// Abstract class implementing multiple interfaces
abstract class GraphicObject implements Drawable, Colorable {
    protected String color;
    protected int x;
    protected int y;
    
    public GraphicObject(String color, int x, int y) {
        this.color = color;
        this.x = x;
        this.y = y;
    }
    
    // Implement Colorable interface
    @Override
    public void setColor(String color) {
        this.color = color;
    }
    
    @Override
    public String getColor() {
        return color;
    }
    
    // Common implementation
    public void move(int newX, int newY) {
        this.x = newX;
        this.y = newY;
        System.out.println("Moved to (" + x + ", " + y + ")");
    }
    
    // Drawable.draw() remains abstract
    // Drawable.resize() remains abstract
}

class Circle extends GraphicObject {
    private int radius;
    
    public Circle(String color, int x, int y, int radius) {
        super(color, x, y);
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing " + color + " circle at (" + x + ", " + y + ")");
    }
    
    @Override
    public void resize(int percentage) {
        radius = radius * percentage / 100;
        System.out.println("Circle resized. New radius: " + radius);
    }
}

class Rectangle extends GraphicObject {
    private int width;
    private int height;
    
    public Rectangle(String color, int x, int y, int width, int height) {
        super(color, x, y);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing " + color + " rectangle at (" + x + ", " + y + ")");
    }
    
    @Override
    public void resize(int percentage) {
        width = width * percentage / 100;
        height = height * percentage / 100;
        System.out.println("Rectangle resized: " + width + "x" + height);
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is the difference between interface and abstract class?**
2. **When would you choose an interface over an abstract class?**
3. **Can a class extend multiple abstract classes?**
4. **Can an interface extend another interface?**
5. **Can an abstract class implement an interface without implementing all methods?**
6. **What changed in Java 8 regarding interfaces?**
7. **Why doesn't Java support multiple inheritance for classes?**
8. **Can you give a real-world example of when to use each?**
9. **What are marker interfaces?**
10. **Is it possible to have a class that both extends an abstract class and implements interfaces?**

---

## 6️⃣ Model Answers

**Q1: What is the difference between interface and abstract class?**

"The key differences are: Abstract classes can have both abstract and concrete methods, instance variables, and constructors, while interfaces traditionally had only abstract methods and constants. A class can implement multiple interfaces but extend only one abstract class. Use abstract classes for IS-A relationships with shared implementation, and interfaces for CAN-DO capabilities. Since Java 8, interfaces can have default and static methods, which blurs the line somewhat, but the design intent remains different—abstract classes are for related classes sharing code, interfaces are for defining contracts."

**Q2: When would you choose an interface over an abstract class?**

"I'd choose an interface when: I need to define a contract or capability that unrelated classes can implement, like Comparable or Serializable. When I need multiple inheritance, since a class can implement multiple interfaces. When I want maximum flexibility, as interfaces are easier to evolve with default methods. Or when the relationship is CAN-DO rather than IS-A. For example, both Car and Boat can be Drivable, but they're not related by inheritance. Use abstract classes when you have common code to share among related classes."

**Q3: Can a class extend multiple abstract classes?**

"No, Java doesn't support multiple inheritance for classes, including abstract classes. A class can extend only one class, whether abstract or concrete. This restriction exists to avoid the diamond problem, where ambiguity arises about which parent's method to inherit. However, a class can implement multiple interfaces, which provides a form of multiple inheritance for type and contract definition, though not for implementation sharing—though Java 8 default methods complicate this slightly."

**Q4: Can an interface extend another interface?**

"Yes, an interface can extend one or more other interfaces using the extends keyword. When an interface extends another, it inherits all the abstract methods from the parent interface. A class implementing the child interface must implement methods from both the child and parent interfaces. For example, List extends Collection, so any class implementing List must implement both List and Collection methods. Unlike classes, interfaces support multiple inheritance, so an interface can extend multiple interfaces."

**Q5: Can an abstract class implement an interface without implementing all methods?**

"Yes, absolutely. An abstract class can implement an interface and choose not to implement some or all of its methods. Those unimplemented methods remain abstract, and the first concrete subclass must implement them. This is actually a common pattern—the abstract class provides partial implementation of an interface, and concrete subclasses complete it. For example, AbstractList implements List but doesn't implement all methods, leaving some for subclasses like ArrayList."

**Q6: What changed in Java 8 regarding interfaces?**

"Java 8 introduced two major changes to interfaces: default methods and static methods. Default methods allow interfaces to provide method implementations using the default keyword. This lets you add new methods to interfaces without breaking existing implementations. Static methods in interfaces work like class methods—they belong to the interface itself. These changes were primarily to support lambda expressions and enhance the Collections API. Java 9 later added private methods in interfaces to support code reuse in default methods."

**Q7: Why doesn't Java support multiple inheritance for classes?**

"Java doesn't support multiple inheritance for classes to avoid the diamond problem. This occurs when a class inherits from two classes that both have a method with the same signature, creating ambiguity about which version to inherit. For example, if class C extends both A and B, and both A and B have a method display(), which version should C inherit? To avoid this complexity and unpredictable behavior, Java only allows single inheritance for classes. Interfaces can have multiple inheritance because they traditionally only defined contracts without implementation, though Java 8 default methods reintroduce some complexity here."

**Q8: Can you give a real-world example of when to use each?**

"Sure. For an abstract class example: You're building a payment system. You'd create an abstract class PaymentProcessor with common fields like transactionId and methods like generateReceipt(), then have concrete classes like CreditCardProcessor and PayPalProcessor extending it. For an interface example: You want different unrelated classes to be comparable. You'd create a Comparable interface with a compareTo() method. Now both Student and Product classes can implement Comparable, even though they're completely unrelated. The abstract class shares code among related types, the interface defines a capability across unrelated types."

**Q9: What are marker interfaces?**

"Marker interfaces are empty interfaces with no methods that serve as tags or markers to indicate something about the class. Examples include Serializable, Cloneable, and Remote. They tell the JVM or framework that the class has certain properties or capabilities. For instance, implementing Serializable marks a class as eligible for serialization. While considered somewhat outdated—annotations are often preferred now—marker interfaces are still used in Java. They're a form of metadata that affects how the class is treated by the runtime or framework."

**Q10: Is it possible to have a class that both extends an abstract class and implements interfaces?**

"Yes, absolutely. A class can extend one abstract class and implement multiple interfaces. This is common in well-designed systems. For example: 'class Dog extends Animal implements Runnable, Comparable'. This gives you the best of both worlds—you get the shared implementation from the abstract class and additional capabilities from interfaces. The only rule is that the extends clause must come before implements in the class declaration. This pattern is frequently used in the Java Collections framework."

---

## 7️⃣ Common Mistakes

1. **Thinking you can extend multiple abstract classes**:
   ```java
   abstract class A { }
   abstract class B { }
   
   class C extends A, B { }  // Compilation error!
   ```

2. **Trying to add instance variables to interfaces**:
   ```java
   interface MyInterface {
       int count = 0;  // OK - but this is a constant (public static final)
       String name;    // Compilation error - must be initialized!
   }
   ```

3. **Using wrong keyword for inheritance**:
   ```java
   interface MyInterface { }
   
   class MyClass implements MyInterface { }  // Correct
   // class MyClass extends MyInterface { }  // Wrong!
   
   abstract class MyAbstract { }
   
   class MyClass extends MyAbstract { }  // Correct
   // class MyClass implements MyAbstract { }  // Wrong!
   ```

4. **Assuming interfaces can have constructors**:
   ```java
   interface MyInterface {
       MyInterface() { }  // Compilation error!
   }
   ```

5. **Not making interface implementation methods public**:
   ```java
   interface MyInterface {
       void method();  // Implicitly public
   }
   
   class MyClass implements MyInterface {
       void method() { }  // Compilation error - must be public!
   }
   ```

6. **Overusing abstract classes instead of interfaces**:
   ```java
   // Wrong: Using abstract class for simple contract
   abstract class Clickable {
       abstract void onClick();
   }
   
   // Better: Use interface for simple contract
   interface Clickable {
       void onClick();
   }
   ```

7. **Forgetting @Override annotation**:
   ```java
   interface MyInterface {
       void display();
   }
   
   class MyClass implements MyInterface {
       public void dispaly() { }  // Typo - should use @Override
   }
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Abstract class for IS-A (sharing code), interface for CAN-DO (defining capabilities)
- Multiple interfaces possible, only one abstract class
- Java 8 added default/static methods to interfaces
- Both cannot be instantiated
- Real-world examples from Java API (List, AbstractList, Comparable, Serializable)

**What to avoid**:
- Don't say one is "better" than the other (they serve different purposes)
- Don't claim interfaces are "faster" (negligible performance difference)
- Don't forget to mention Java 8 changes
- Don't confuse extends vs implements keywords

**Under pressure**:
- Draw a simple comparison table
- Use the IS-A vs CAN-DO rule
- Give concrete examples: Animal (abstract class) vs Flyable (interface)
- Mention that you can use both together

**Red flags to avoid**:
- "You can extend multiple abstract classes" (no)
- "Interfaces can have instance variables" (only constants)
- "Interfaces can have constructors" (no)
- "Abstract classes are always better" or vice versa (depends on use case)

**Bonus points**:
- Mention design patterns: Template Method (abstract class), Strategy (interface)
- Reference Java Collections: List is interface, AbstractList is abstract class
- Discuss evolution: Java 8 changes, functional interfaces
- Mention composition over inheritance principle
- Reference SOLID principles: Dependency Inversion (prefer interfaces)
- Discuss marker interfaces vs annotations
- Mention that modern design often favors interfaces for flexibility

**Decision Framework to Share**:
```
Use Abstract Class when:
✓ Classes are closely related
✓ You have common code to share
✓ You need instance variables or constructors
✓ You need non-public members

Use Interface when:
✓ Unrelated classes need common behavior
✓ You need multiple inheritance
✓ You want to define a contract/capability
✓ You want maximum flexibility
```
