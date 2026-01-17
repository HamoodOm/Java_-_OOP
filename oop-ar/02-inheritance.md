<div dir="rtl">

# الوراثة (Inheritance)

## 1. نظرة عامة على المفهوم

**الوراثة (Inheritance)** هي مبدأ في OOP حيث يكتسب class جديد (فرعي/subclass) الخصائص والسلوكيات من class موجود (أب/superclass). تُمكّن من إعادة استخدام الكود وتؤسس علاقة بين الـ classes.

**المصطلحات الأساسية**:
- **Superclass/Parent/Base class**: الـ class الذي يُورث منه
- **Subclass/Child/Derived class**: الـ class الذي يرث
- **كلمة extends**: تُستخدم لتنفيذ الوراثة
- **علاقة IS-A**: الـ Subclass هو نوع من الـ Superclass

**الصيغة**:
```java
class Parent {
    // parent members
}

class Child extends Parent {
    // child inherits parent members + can add its own
}
```

**ما يُورث**:
- الأعضاء العامة والمحمية (الحقول والدوال)
- الأعضاء الافتراضية (package-private) إذا كانوا في نفس الـ package

**ما لا يُورث**:
- الأعضاء الخاصة (private) (يمكن الوصول إليها عبر الدوال الموروثة العامة/المحمية)
- الـ Constructors (لكن تُستدعى عبر super())
- الأعضاء الثابتة (static) (مشتركة، لا تُورث بالمعنى الحقيقي)

---

## 2. لماذا يسأل المحاورون عن هذا

- **مبدأ أساسي في OOP**: أساسي للتصميم كائني التوجه
- **إعادة استخدام الكود**: يختبر فهم مبدأ DRY (لا تكرر نفسك)
- **أنماط التصميم**: العديد من الأنماط تعتمد على الوراثة
- **أساس تعدد الأشكال**: الوراثة تُمكّن تعدد الأشكال في وقت التشغيل
- **نمذجة العالم الحقيقي**: يُظهر القدرة على نمذجة العلاقات الهرمية
- **المشاكل الشائعة**: تسلسل الـ Constructors واستخدام كلمة super

---

## 3. القواعد والحقائق الأساسية

**قواعد الوراثة**:
- Java تدعم **الوراثة الفردية** فقط (أب مباشر واحد)
- Java **لا تدعم الوراثة المتعددة** للـ classes (مشكلة الماسة)
- يمكن للـ class تنفيذ (implement) عدة interfaces
- جميع الـ classes ترث من `Object` class (الجذر الضمني)
- الـ Subclass لا يمكنه وراثة الأعضاء الخاصة لكن يمكنه الوصول إليها عبر الدوال الموروثة
- الـ Constructors لا تُورث لكن يجب استدعاؤها عبر `super()`
- الـ class المعلن كـ `final` لا يمكن وراثته
- الدالة المعلنة كـ `final` لا يمكن إعادة تعريفها (override)

**أنواع الوراثة** (المدعومة في Java):
1. **الفردية (Single)**: Class B يرث من Class A
2. **المتعددة المستويات (Multilevel)**: Class C يرث من B، B يرث من A
3. **الهرمية (Hierarchical)**: عدة classes ترث من نفس الأب

**غير مدعومة في Java**:
- **الوراثة المتعددة (Multiple)**: Class C يرث من A و B (تسبب مشكلة الماسة)
- **الهجينة (Hybrid)**: مزيج يتضمن الوراثة المتعددة

**كلمة super**:
- `super.method()`: تستدعي دالة الـ class الأب
- `super.field`: تصل إلى حقل الـ class الأب
- `super()`: تستدعي constructor الـ class الأب (يجب أن تكون أول جملة)

**this مقابل super**:
- `this`: تشير إلى instance الـ class الحالي
- `super`: تشير إلى instance الـ class الأب المباشر

**تسلسل الـ Constructors**:
- الـ Constructors تُنفذ من الأعلى إلى الأسفل في التسلسل الهرمي
- إذا لم يُستدعى `super()` صراحةً، Java تدرجه تلقائياً
- constructor الأب يُنفذ دائماً قبل constructor الابن

---

## 4. أمثلة برمجية (Java 8)

### المثال 1: الوراثة الأساسية

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

### المثال 2: إعادة تعريف الدوال (Method Overriding)

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

### المثال 3: استخدام كلمة super

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

### المثال 4: الوراثة متعددة المستويات

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

### المثال 5: الوراثة الهرمية

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

### المثال 6: تسلسل الـ Constructors

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

### المثال 7: كلمة final مع الوراثة

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

### المثال 8: علاقة IS-A وتحويل الأنواع

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

## 5. أسئلة المقابلات الشائعة

1. **ما هي الوراثة في Java؟**
2. **ما هي أنواع الوراثة المدعومة في Java؟**
3. **لماذا لا تدعم Java الوراثة المتعددة؟**
4. **ما الفرق بين كلمتي this و super؟**
5. **هل يمكننا إعادة تعريف الدوال الخاصة (private)؟**
6. **هل يمكننا إعادة تعريف الدوال الثابتة (static)؟**
7. **ماذا يحدث إذا لم تستدعِ super() في constructor؟**
8. **ما استخدام كلمة final في الوراثة؟**
9. **هل يمكنك وراثة الـ constructors في Java؟**
10. **ما الفرق بين method overloading و method overriding؟**

---

## 6. إجابات نموذجية

**س1: ما هي الوراثة في Java؟**

"الوراثة هي مبدأ في OOP حيث يكتسب class الخصائص والسلوكيات من class آخر. الـ class الذي يرث يُسمى الابن أو subclass، والـ class الذي يُورث منه يُسمى الأب أو superclass. تُنفذ باستخدام كلمة extends وتُمكّن من إعادة استخدام الكود. على سبيل المثال، إذا كان لدينا class Vehicle، يمكننا إنشاء class Car يرث من Vehicle، يرث خصائصه مثل speed ودواله مثل accelerate()، مع إضافة ميزات خاصة بالسيارة."

**س2: ما هي أنواع الوراثة المدعومة في Java؟**

"Java تدعم ثلاثة أنواع من الوراثة. الوراثة الفردية، حيث يرث class من أب واحد. الوراثة متعددة المستويات، حيث لدينا سلسلة مثل C يرث من B، و B يرث من A. والوراثة الهرمية، حيث ترث عدة classes من نفس الأب. Java لا تدعم الوراثة المتعددة للـ classes - حيث يرث class واحد من عدة classes - لتجنب مشكلة الماسة. لكن Java تدعم الوراثة المتعددة من خلال الـ interfaces."

**س3: لماذا لا تدعم Java الوراثة المتعددة؟**

"Java لا تدعم الوراثة المتعددة للـ classes بسبب مشكلة الماسة. تحدث عندما يرث class من اثنين من الـ classes يرثان كلاهما من أب مشترك ويعيدان تعريف نفس الدالة. الـ class الابن لن يعرف أي نسخة من الدالة يرث. هذا الغموض يُنشئ تعقيداً وسلوكاً غير متوقع. اختار مصممو Java تجنب هذا بالسماح بالوراثة الفردية فقط للـ classes، رغم أن الوراثة المتعددة مسموحة عبر الـ interfaces لأن الـ interfaces لم يكن لديها تعارضات في التنفيذ بالمعنى التقليدي."

**س4: ما الفرق بين كلمتي this و super؟**

"كلمة this تشير إلى instance الـ class الحالي، بينما super تشير إلى instance الـ class الأب المباشر. This تُستخدم للوصول إلى أعضاء الـ class الحالي واستدعاء constructors الـ class الحالي. Super تُستخدم للوصول إلى أعضاء الـ class الأب، واستدعاء دوال الأب حتى عند إعادة تعريفها، واستدعاء constructors الـ class الأب. على سبيل المثال، super() تستدعي constructor الأب ويجب أن تكون أول جملة في constructor الابن. Super.method() تستدعي نسخة الأب من دالة معاد تعريفها."

**س5: هل يمكننا إعادة تعريف الدوال الخاصة (private)؟**

"لا، لا يمكننا إعادة تعريف الدوال الخاصة. الدوال الخاصة غير مرئية للـ subclasses، لذا لا يوجد شيء لإعادة تعريفه. إذا عرّفت دالة بنفس التوقيع في الـ class الابن، فهي ليست إعادة تعريف - إنها دالة جديدة تماماً تصادف أن لها نفس الاسم. إعادة تعريف الدوال تتطلب أن تكون الدالة قابلة للوصول، والدوال الخاصة ليست كذلك. فقط الدوال العامة والمحمية والافتراضية يمكن إعادة تعريفها."

**س6: هل يمكننا إعادة تعريف الدوال الثابتة (static)؟**

"لا، الدوال الثابتة لا يمكن إعادة تعريفها. الدوال الثابتة تنتمي للـ class، وليس للـ instances، لذا تُحل في وقت الترجمة بناءً على نوع المرجع، وليس نوع الكائن. إذا عرّفت دالة ثابتة في class ابن بنفس توقيع دالة ثابتة في الأب، يُسمى هذا إخفاء الدالة (method hiding)، وليس إعادة تعريف. الدالة التي تُنفذ تعتمد على نوع المرجع، وليس نوع الكائن الفعلي. هذا يختلف جوهرياً عن إعادة تعريف دوال الـ instance، التي تُحل في وقت التشغيل."

**س7: ماذا يحدث إذا لم تستدعِ super() في constructor؟**

"إذا لم تستدعِ super() صراحةً في constructor، Java تدرج تلقائياً استدعاء super() كأول جملة. هذا يضمن أن constructor الـ class الأب يُنفذ دائماً قبل constructor الـ class الابن. ومع ذلك، إذا لم يكن للـ class الأب constructor بدون معاملات ولم تستدعِ super() بالمعاملات صراحةً، ستحصل على خطأ ترجمة. لهذا غالباً ما يكون ضرورياً استدعاء super() صراحةً بالمعاملات المناسبة عندما يكون للأب constructors بمعاملات فقط."

**س8: ما استخدام كلمة final في الوراثة؟**

"كلمة final لها استخدامان رئيسيان في الوراثة. أولاً، الـ class المعلن كـ final لا يمكن توريثه - ينهي سلسلة الوراثة. يُستخدم هذا للـ classes التي لا يجب أن تُورث، مثل String أو الـ wrapper classes. ثانياً، الدالة المعلنة كـ final لا يمكن إعادة تعريفها من الـ classes الأبناء، وهذا مفيد للدوال التي تحتوي منطقاً حرجاً يجب أن يبقى دون تغيير. هذا يوفر الأمان ويضمن اتساق السلوك عبر التسلسل الهرمي."

**س9: هل يمكنك وراثة الـ constructors في Java؟**

"لا، الـ constructors لا تُورث في Java. كل class يجب أن يُعرّف constructors الخاصة به. ومع ذلك، constructors الـ class الابن يجب أن تستدعي constructors الـ class الأب باستخدام super()، وهذا يحدث تلقائياً إذا لم يُحدد صراحةً. هذا يُسمى تسلسل الـ Constructors. بينما لا ترث الـ constructors، تحتاج إلى التأكد من تهيئة الأب بشكل صحيح، ولهذا توجد super(). constructor الأب يُنفذ دائماً قبل جسم constructor الابن."

**س10: ما الفرق بين method overloading و method overriding؟**

"Method overloading هو وجود عدة دوال في نفس الـ class بنفس الاسم لكن معاملات مختلفة. يُحل في وقت الترجمة ويُسمى أيضاً static polymorphism. Method overriding هو عندما يوفر class ابن تنفيذاً محدداً لدالة معرّفة بالفعل في الـ class الأب. يُحل في وقت التشغيل بناءً على نوع الكائن الفعلي ويُسمى dynamic polymorphism. Overloading يتعلق بعدة دوال بنفس الاسم لكن توقيعات مختلفة، بينما overriding يتعلق باستبدال سلوك الأب في الـ class الابن."

---

## 7. الأخطاء الشائعة

1. **نسيان استدعاء super() مع constructor الأب بمعاملات**:
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

2. **محاولة إعادة تعريف الدوال الخاصة**:
   ```java
   class Parent {
       private void privateMethod() { }
   }

   class Child extends Parent {
       @Override  // This is NOT overriding, just a new method
       private void privateMethod() { }
   }
   ```

3. **محاولة الوراثة المتعددة**:
   ```java
   class A { }
   class B { }
   class C extends A, B { }  // Compilation error!
   ```

4. **عدم استخدام @Override annotation**:
   ```java
   class Parent {
       public void display() { }
   }

   class Child extends Parent {
       public void Display() { }  // Typo - new method, not override
       // Should use @Override to catch this
   }
   ```

5. **استدعاء super() ليس كأول جملة**:
   ```java
   class Child extends Parent {
       Child() {
           System.out.println("Child");
           super();  // Compilation error - must be first
       }
   }
   ```

6. **تضييق محدد الوصول في إعادة التعريف**:
   ```java
   class Parent {
       public void method() { }
   }

   class Child extends Parent {
       @Override
       private void method() { }  // Compilation error - can't reduce visibility
   }
   ```

7. **الخلط بين IS-A و HAS-A**:
   ```java
   // Wrong: Car IS-A Engine (No!)
   class Car extends Engine { }

   // Correct: Car HAS-A Engine (Composition)
   class Car {
       private Engine engine;
   }
   ```

8. **التحويل بدون فحص instanceof**:
   ```java
   Animal animal = new Animal();
   Dog dog = (Dog) animal;  // ClassCastException at runtime!

   // Better:
   if (animal instanceof Dog) {
       Dog dog = (Dog) animal;
   }
   ```

---

## 8. نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- الوراثة تُمكّن من إعادة استخدام الكود وعلاقات IS-A
- Java تدعم الوراثة الفردية ومتعددة المستويات والهرمية
- Java لا تدعم الوراثة المتعددة للـ classes (مشكلة الماسة)
- تسلسل الـ Constructors: constructor الأب يُنفذ دائماً أولاً
- كلمة super للوصول إلى أعضاء الأب
- Method overriding مقابل method overloading

**ما يجب تجنبه**:
- لا تقل "Java لا تدعم الوراثة المتعددة" بدون توضيح "للـ classes"
- لا تخلط بين إخفاء الدوال (static) وإعادة تعريف الدوال (instance)
- لا تدعي أن الـ constructors تُورث (لا تُورث)
- لا تنسى أن جميع الـ classes ترث ضمنياً من Object

**تحت الضغط**:
- ارسم تسلسلاً هرمياً بسيطاً: Animal ← Dog ← Puppy
- اشرح بأمثلة ملموسة (Vehicle/Car مفهوم عالمياً)
- تذكر شرح مشكلة الماسة للوراثة المتعددة
- اذكر أن الأعضاء الخاصة لا تُورث (لكن يمكن الوصول إليها عبر الدوال الموروثة)

**علامات حمراء يجب تجنبها**:
- "الوراثة المتعددة ممكنة في Java" (ليست للـ classes)
- "الدوال الثابتة يمكن إعادة تعريفها" (يمكن إخفاؤها، لا إعادة تعريفها)
- "الـ Constructors تُورث" (لا تُورث)
- "الدوال الخاصة يمكن إعادة تعريفها" (لا يمكن)
- "يمكنك الوراثة من عدة classes" (أب مباشر واحد فقط)

**نقاط إضافية**:
- اذكر أن Object هو جذر جميع الـ classes
- ناقش التركيب (Composition) مقابل الوراثة (فضل التركيب)
- أشر إلى مبدأ Liskov Substitution (الابن يجب أن يكون قابلاً للاستبدال مكان الأب)
- اذكر أن الـ classes المعلنة كـ final تُستخدم للأمان وعدم القابلية للتغيير
- اشرح حل الدوال في وقت التشغيل مقابل وقت الترجمة
- ناقش متى لا تستخدم الوراثة (فضل الـ interfaces أو التركيب)

</div>
