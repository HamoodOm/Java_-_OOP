# Inheritance

## 1️⃣ Concept Overview

**Inheritance** is an OOP principle where a new class (child/subclass) acquires properties and behaviors from an existing class (parent/superclass). It enables code reusability and establishes a relationship between classes.

**Key Terminology**:
- **Superclass/Parent/Base class**: The class being inherited from
- **Subclass/Child/Derived class**: The class that inherits
- **extends keyword**: Used to implement inheritance
- **IS-A relationship**: Subclass IS-A type of superclass

**Syntax**:
```java
class Parent {
    // parent members
}

class Child extends Parent {
    // child inherits parent members + can add its own
}
```

**What is inherited**:
- Public and protected members (fields and methods)
- Default (package-private) members if in same package

**What is NOT inherited**:
- Private members (accessible via inherited public/protected methods)
- Constructors (but invoked via super())
- Static members (shared, not inherited in true sense)

---

## 2️⃣ Why Interviewers Ask This

- **Core OOP principle**: Fundamental to object-oriented design
- **Code reusability**: Tests understanding of DRY (Don't Repeat Yourself)
- **Design patterns**: Many patterns rely on inheritance
- **Polymorphism foundation**: Inheritance enables runtime polymorphism
- **Real-world modeling**: Shows ability to model hierarchical relationships
- **Common pitfalls**: Constructor chaining and super keyword usage

---

## 3️⃣ Key Rules / Facts

**Inheritance Rules**:
- Java supports **single inheritance** only (one direct parent)
- Java does **NOT support multiple inheritance** for classes (diamond problem)
- A class can implement multiple interfaces
- All classes inherit from `Object` class (implicit root)
- Subclass cannot inherit private members but can access via inherited methods
- Constructors are not inherited but must be called via `super()`
- `final` class cannot be inherited
- `final` method cannot be overridden

**Types of Inheritance** (supported in Java):
1. **Single**: Class B extends Class A
2. **Multilevel**: Class C extends B, B extends A
3. **Hierarchical**: Multiple classes extend same parent

**NOT supported in Java**:
- **Multiple inheritance**: Class C extends A, B (causes diamond problem)
- **Hybrid**: Combination involving multiple inheritance

**super Keyword**:
- `super.method()`: Calls parent class method
- `super.field`: Accesses parent class field
- `super()`: Calls parent class constructor (must be first statement)

**this vs super**:
- `this`: Refers to current class instance
- `super`: Refers to immediate parent class instance

**Constructor Chaining**:
- Constructors execute from top to bottom in hierarchy
- If no `super()` is explicitly called, Java inserts `super()` automatically
- Parent constructor always executes before child constructor

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Basic Inheritance

```java
// Parent class
class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("Animal constructor called");
    }
    
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
}

// Child class
class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);  // Call parent constructor
        this.breed = breed;
        System.out.println("Dog constructor called");
    }
    
    // Dog inherits eat() and sleep() from Animal
    
    // Dog-specific method
    public void bark() {
        System.out.println(name + " is barking");
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy", 3, "Labrador");
        // Output:
        // Animal constructor called
        // Dog constructor called
        
        dog.eat();    // Inherited from Animal
        dog.sleep();  // Inherited from Animal
        dog.bark();   // Dog's own method
        
        // Output:
        // Buddy is eating
        // Buddy is sleeping
        // Buddy is barking
    }
}
```

### Example 2: Method Overriding

```java
class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
    
    public void move() {
        System.out.println("Animal moves");
    }
}

class Dog extends Animal {
    // Override parent method
    @Override
    public void makeSound() {
        System.out.println("Dog barks: Woof! Woof!");
    }
    
    // Don't override move() - inherited as is
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows: Meow! Meow!");
    }
    
    @Override
    public void move() {
        System.out.println("Cat walks gracefully");
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Animal animal = new Animal();
        animal.makeSound();  // Animal makes a sound
        
        Dog dog = new Dog();
        dog.makeSound();     // Dog barks: Woof! Woof!
        dog.move();          // Animal moves (inherited, not overridden)
        
        Cat cat = new Cat();
        cat.makeSound();     // Cat meows: Meow! Meow!
        cat.move();          // Cat walks gracefully
    }
}
```

### Example 3: super Keyword Usage

```java
class Vehicle {
    protected String brand;
    protected int speed;
    
    public Vehicle(String brand) {
        this.brand = brand;
        this.speed = 0;
    }
    
    public void displayInfo() {
        System.out.println("Brand: " + brand);
        System.out.println("Speed: " + speed + " km/h");
    }
    
    public void accelerate() {
        speed += 10;
        System.out.println("Vehicle accelerating...");
    }
}

class Car extends Vehicle {
    private int numDoors;
    
    public Car(String brand, int numDoors) {
        super(brand);  // Call parent constructor
        this.numDoors = numDoors;
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();  // Call parent method first
        System.out.println("Number of doors: " + numDoors);
    }
    
    @Override
    public void accelerate() {
        super.accelerate();  // Call parent's accelerate
        speed += 20;  // Additional acceleration for car
        System.out.println("Car accelerating faster...");
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Car car = new Car("Toyota", 4);
        car.displayInfo();
        // Output:
        // Brand: Toyota
        // Speed: 0 km/h
        // Number of doors: 4
        
        car.accelerate();
        // Output:
        // Vehicle accelerating...
        // Car accelerating faster...
        
        System.out.println("Final speed: " + car.speed);  // 30
    }
}
```

### Example 4: Multilevel Inheritance

```java
// Level 1
class LivingBeing {
    public void breathe() {
        System.out.println("Breathing...");
    }
}

// Level 2
class Animal extends LivingBeing {
    public void eat() {
        System.out.println("Eating...");
    }
}

// Level 3
class Dog extends Animal {
    public void bark() {
        System.out.println("Barking...");
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        
        // Dog has access to all methods in the hierarchy
        dog.breathe();  // From LivingBeing
        dog.eat();      // From Animal
        dog.bark();     // From Dog
    }
}
```

### Example 5: Hierarchical Inheritance

```java
// Parent class
class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    public void display() {
        System.out.println("This is a " + color + " shape");
    }
}

// Multiple children inherit from same parent
class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    public double getArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double length;
    private double width;
    
    public Rectangle(String color, double length, double width) {
        super(color);
        this.length = length;
        this.width = width;
    }
    
    public double getArea() {
        return length * width;
    }
}

class Triangle extends Shape {
    private double base;
    private double height;
    
    public Triangle(String color, double base, double height) {
        super(color);
        this.base = base;
        this.height = height;
    }
    
    public double getArea() {
        return 0.5 * base * height;
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        Circle circle = new Circle("Red", 5.0);
        Rectangle rectangle = new Rectangle("Blue", 4.0, 6.0);
        Triangle triangle = new Triangle("Green", 3.0, 4.0);
        
        circle.display();     // From Shape
        rectangle.display();  // From Shape
        triangle.display();   // From Shape
        
        System.out.println("Circle area: " + circle.getArea());
        System.out.println("Rectangle area: " + rectangle.getArea());
        System.out.println("Triangle area: " + triangle.getArea());
    }
}
```

### Example 6: Constructor Chaining

```java
class GrandParent {
    public GrandParent() {
        System.out.println("1. GrandParent constructor");
    }
}

class Parent extends GrandParent {
    public Parent() {
        super();  // Optional - inserted automatically if not present
        System.out.println("2. Parent constructor");
    }
    
    public Parent(String message) {
        super();  // Can be explicit
        System.out.println("2. Parent constructor with: " + message);
    }
}

class Child extends Parent {
    public Child() {
        // super() is automatically inserted here
        System.out.println("3. Child constructor");
    }
    
    public Child(String message) {
        super(message);  // Call specific parent constructor
        System.out.println("3. Child constructor with: " + message);
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        System.out.println("Creating Child():");
        Child child1 = new Child();
        // Output:
        // 1. GrandParent constructor
        // 2. Parent constructor
        // 3. Child constructor
        
        System.out.println("\nCreating Child(String):");
        Child child2 = new Child("Hello");
        // Output:
        // 1. GrandParent constructor
        // 2. Parent constructor with: Hello
        // 3. Child constructor with: Hello
    }
}
```

### Example 7: final Keyword with Inheritance

```java
// final class - cannot be inherited
final class ImmutableValue {
    private final int value;
    
    public ImmutableValue(int value) {
        this.value = value;
    }
    
    public int getValue() {
        return value;
    }
}

// Compilation error: cannot inherit from final class
// class ExtendedValue extends ImmutableValue { }

// Parent with final method
class Parent {
    // final method - cannot be overridden
    public final void criticalMethod() {
        System.out.println("This method cannot be overridden");
    }
    
    public void regularMethod() {
        System.out.println("This method can be overridden");
    }
}

class Child extends Parent {
    // Compilation error: cannot override final method
    // @Override
    // public void criticalMethod() { }
    
    @Override
    public void regularMethod() {
        System.out.println("Regular method overridden");
    }
}
```

### Example 8: IS-A Relationship and Type Casting

```java
class Animal {
    public void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    public void bark() {
        System.out.println("Dog is barking");
    }
}

class Main {
    public static void main(String[] args) {
        // IS-A relationship: Dog IS-A Animal
        Dog dog = new Dog();
        
        // Upcasting (automatic) - Child to Parent
        Animal animal = dog;  // Dog is an Animal
        animal.eat();         // OK
        // animal.bark();     // Compilation error - Animal doesn't have bark()
        
        // Downcasting (explicit) - Parent to Child
        Animal animal2 = new Dog();
        Dog dog2 = (Dog) animal2;  // Explicit cast required
        dog2.bark();               // OK now
        
        // Runtime error - ClassCastException
        Animal animal3 = new Animal();
        // Dog dog3 = (Dog) animal3;  // Compiles but throws ClassCastException at runtime
        
        // Safe casting with instanceof
        Animal animal4 = new Dog();
        if (animal4 instanceof Dog) {
            Dog dog4 = (Dog) animal4;
            dog4.bark();  // Safe
        }
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is inheritance in Java?**
2. **What are the types of inheritance supported in Java?**
3. **Why doesn't Java support multiple inheritance?**
4. **What is the difference between this and super keywords?**
5. **Can we override private methods?**
6. **Can we override static methods?**
7. **What happens if you don't call super() in a constructor?**
8. **What is the use of final keyword in inheritance?**
9. **Can you inherit constructors in Java?**
10. **What is the difference between method overloading and method overriding?**

---

## 6️⃣ Model Answers

**Q1: What is inheritance in Java?**

"Inheritance is an OOP principle where a class acquires properties and behaviors from another class. The class that inherits is called the child or subclass, and the class being inherited from is called the parent or superclass. It's implemented using the extends keyword and enables code reusability. For example, if we have a Vehicle class, we can create a Car class that extends Vehicle, inheriting its properties like speed and methods like accelerate(), while adding car-specific features."

**Q2: What are the types of inheritance supported in Java?**

"Java supports three types of inheritance. Single inheritance, where a class extends one parent class. Multilevel inheritance, where we have a chain like C extends B, and B extends A. And hierarchical inheritance, where multiple classes inherit from the same parent. Java does not support multiple inheritance for classes—where one class extends multiple classes—to avoid the diamond problem. However, Java does support multiple inheritance through interfaces."

**Q3: Why doesn't Java support multiple inheritance?**

"Java doesn't support multiple inheritance for classes because of the diamond problem. This occurs when a class inherits from two classes that both inherit from a common parent and override the same method. The child class wouldn't know which version of the method to inherit. This ambiguity creates complexity and unpredictable behavior. Java's designers chose to avoid this by allowing only single inheritance for classes, though multiple inheritance is allowed through interfaces since interfaces don't have implementation conflicts in the traditional sense."

**Q4: What is the difference between this and super keywords?**

"The this keyword refers to the current class instance, while super refers to the immediate parent class instance. This is used to access current class members and call current class constructors. Super is used to access parent class members, call parent class methods even when overridden, and invoke parent class constructors. For example, super() calls the parent constructor and must be the first statement in a child constructor. Super.method() calls the parent's version of an overridden method."

**Q5: Can we override private methods?**

"No, we cannot override private methods. Private methods are not visible to subclasses, so there's nothing to override. If you define a method with the same signature in the child class, it's not overriding—it's a completely new method that happens to have the same name. Method overriding requires the method to be accessible, which private methods are not. Only public, protected, and default access methods can be overridden."

**Q6: Can we override static methods?**

"No, static methods cannot be overridden. Static methods belong to the class, not to instances, so they're resolved at compile-time based on the reference type, not the object type. If you define a static method in a child class with the same signature as a parent's static method, it's called method hiding, not overriding. The method that gets called depends on the reference type, not the actual object type. This is different from instance method overriding, which is resolved at runtime."

**Q7: What happens if you don't call super() in a constructor?**

"If you don't explicitly call super() in a constructor, Java automatically inserts a call to super() as the first statement. This ensures the parent class constructor is always executed before the child class constructor. However, if the parent class doesn't have a no-argument constructor and you don't explicitly call super() with arguments, you'll get a compilation error. This is why it's often necessary to explicitly call super() with appropriate parameters when the parent has only parameterized constructors."

**Q8: What is the use of final keyword in inheritance?**

"The final keyword has two main uses in inheritance. First, a final class cannot be extended—it terminates the inheritance chain. This is used for classes that shouldn't be subclassed, like String or wrapper classes. Second, a final method cannot be overridden by child classes, which is useful for methods that contain critical logic that must remain unchanged. This provides security and guarantees behavior consistency across the hierarchy."

**Q9: Can you inherit constructors in Java?**

"No, constructors are not inherited in Java. Each class must define its own constructors. However, child class constructors must call parent class constructors using super(), which happens automatically if not explicitly specified. This is called constructor chaining. While you don't inherit constructors, you do need to ensure the parent is properly initialized, which is why super() exists. The parent constructor always executes before the child constructor's body."

**Q10: What is the difference between method overloading and method overriding?**

"Method overloading is having multiple methods in the same class with the same name but different parameters. It's resolved at compile-time and is also called static polymorphism. Method overriding is when a child class provides a specific implementation for a method already defined in the parent class. It's resolved at runtime based on the actual object type and is called dynamic polymorphism. Overloading is about multiple methods with same name but different signatures, while overriding is about replacing parent's behavior in child class."

---

## 7️⃣ Common Mistakes

1. **Forgetting to call super() with parameterized parent constructor**:
   ```java
   class Parent {
       Parent(String name) { }
       // No default constructor
   }
   
   class Child extends Parent {
       Child() {  // Compilation error!
           // Java tries to insert super() but Parent has no default constructor
       }
   }
   // Fix: Child() { super("someName"); }
   ```

2. **Trying to override private methods**:
   ```java
   class Parent {
       private void privateMethod() { }
   }
   
   class Child extends Parent {
       @Override  // This is NOT overriding, just a new method
       private void privateMethod() { }
   }
   ```

3. **Attempting multiple inheritance**:
   ```java
   class A { }
   class B { }
   class C extends A, B { }  // Compilation error!
   ```

4. **Not using @Override annotation**:
   ```java
   class Parent {
       public void display() { }
   }
   
   class Child extends Parent {
       public void Display() { }  // Typo - new method, not override
       // Should use @Override to catch this
   }
   ```

5. **Calling super() not as first statement**:
   ```java
   class Child extends Parent {
       Child() {
           System.out.println("Child");
           super();  // Compilation error - must be first
       }
   }
   ```

6. **Narrowing access modifier in override**:
   ```java
   class Parent {
       public void method() { }
   }
   
   class Child extends Parent {
       @Override
       private void method() { }  // Compilation error - can't reduce visibility
   }
   ```

7. **Confusing IS-A with HAS-A**:
   ```java
   // Wrong: Car IS-A Engine (No!)
   class Car extends Engine { }
   
   // Correct: Car HAS-A Engine (Composition)
   class Car {
       private Engine engine;
   }
   ```

8. **Casting without instanceof check**:
   ```java
   Animal animal = new Animal();
   Dog dog = (Dog) animal;  // ClassCastException at runtime!
   
   // Better:
   if (animal instanceof Dog) {
       Dog dog = (Dog) animal;
   }
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Inheritance enables code reusability and IS-A relationships
- Java supports single, multilevel, and hierarchical inheritance
- Java does NOT support multiple inheritance for classes (diamond problem)
- Constructor chaining: parent constructor always executes first
- super keyword for accessing parent members
- Method overriding vs method overloading

**What to avoid**:
- Don't say "Java doesn't support multiple inheritance" without clarifying "for classes"
- Don't confuse method hiding (static) with method overriding (instance)
- Don't claim constructors are inherited (they're not)
- Don't forget that all classes implicitly extend Object

**Under pressure**:
- Draw a simple hierarchy: Animal → Dog → Puppy
- Explain with concrete examples (Vehicle/Car is universally understood)
- Remember the diamond problem explanation for multiple inheritance
- State that private members are not inherited (but accessible via inherited methods)

**Red flags to avoid**:
- "Multiple inheritance is possible in Java" (not for classes)
- "Static methods can be overridden" (they can be hidden, not overridden)
- "Constructors are inherited" (they are not)
- "Private methods can be overridden" (they cannot)
- "You can inherit from multiple classes" (only one direct parent)

**Bonus points**:
- Mention that Object is the root of all classes
- Discuss composition vs inheritance (favor composition)
- Reference Liskov Substitution Principle (child should be substitutable for parent)
- Mention that final classes are used for security and immutability
- Explain method resolution at runtime vs compile-time
- Discuss when NOT to use inheritance (prefer interfaces or composition)
