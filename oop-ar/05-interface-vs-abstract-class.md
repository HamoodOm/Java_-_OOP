<div dir="rtl">

# الواجهة مقابل الفئة المجردة (Interface vs Abstract Class)

## 1️⃣ نظرة عامة على المفهوم

**السؤال الكلاسيكي في المقابلات**: "متى يجب استخدام Interface ومتى يجب استخدام Abstract Class؟"

هذا أحد أكثر أسئلة OOP شيوعاً لأنه يختبر فهمك لمبادئ التصميم كائني التوجه وقدرتك على اتخاذ قرارات معمارية.

**ملخص سريع**:
- **الفئة المجردة (Abstract Class)**: استخدمها لعلاقات IS-A مع تنفيذ مشترك
- **الواجهة (Interface)**: استخدمها لقدرات CAN-DO أو عقود بدون مشاركة التنفيذ

**المبدأ الأساسي**:
- الفئة المجردة = "ما أنت عليه"
- الواجهة = "ما يمكنك فعله"

**مثال**:
- الكلب **هو** حيوان (فئة مجردة)
- الكلب **يمكنه** الجري، النباح، السباحة (واجهات: Runnable، Barkable، Swimmable)

**السياق التاريخي**:
- قبل Java 8: كانت الواجهات مجردة بالكامل (تجريد 100%)
- Java 8+: يمكن للواجهات أن تحتوي على default و static methods (يطمس الخط الفاصل)
- الاختيار الآن يتعلق أكثر بنية التصميم وليس القيود التقنية

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **يختبر التفكير التصميمي**: هل يمكنك اختيار الأداة المناسبة للمهمة؟
- **الصلة بالواقع**: هذا القرار يؤثر على كل نظام تصممه
- **يفصل بين المستويات**: المبتدئون يعرفون الصياغة؛ المحترفون يعرفون متى يستخدمون أياً منهما
- **معرفة الأطر البرمجية**: فهم كيف تتخذ الأطر هذه الاختيارات
- **الوعي بالتطور**: يظهر ما إذا كنت تعرف كيف تطورت Java (تغييرات Java 8)

---

## 3️⃣ القواعد والحقائق الأساسية

**جدول المقارنة الشامل**:

| الميزة | Interface | Abstract Class |
|--------|-----------|----------------|
| **الكلمة المفتاحية** | `interface` | `abstract class` |
| **الإنشاء** | لا يمكن الإنشاء | لا يمكن الإنشاء |
| **أنواع الدوال** | مجردة (+ default/static في Java 8+) | مجردة + ملموسة |
| **المتغيرات** | `public static final` فقط (ثوابت) | أي نوع (instance، static) |
| **Constructors** | لا يوجد | يمكن أن تحتوي |
| **محددات الوصول (للدوال)** | public فقط (ضمني) | أي محدد (public، protected، private، default) |
| **محددات الوصول (للحقول)** | `public static final` (ضمني) | أي محدد |
| **الوراثة المتعددة** | نعم (implements متعددة) | لا (extends واحدة فقط) |
| **التنفيذ الافتراضي** | نعم (default methods في Java 8+) | نعم |
| **الحالة (State)** | لا يمكن الحفاظ عليها (لا متغيرات instance) | يمكن الحفاظ عليها |
| **حالة الاستخدام** | تحديد القدرات/العقود | مشاركة الكود بين فئات مرتبطة |
| **العلاقة** | CAN-DO (قدرة) | IS-A (هوية) |
| **دعم الإصدار** | Java 8+ لها default/static methods | دائماً كانت تملك دوالاً ملموسة |
| **الأداء** | فرق لا يُذكر | فرق لا يُذكر |

**متى تستخدم الفئة المجردة (Abstract Class)**:
1. الفئات المرتبطة تشترك في كود مشترك
2. تريد توفير تنفيذات افتراضية
3. تحتاج حقول non-static و non-final
4. تريد أعضاء غير عامة (protected، private)
5. توجد علاقة IS-A
6. تحتاج constructors لتهيئة الحالة

**متى تستخدم الواجهة (Interface)**:
1. فئات غير مرتبطة تشترك في قدرات
2. تريد تحديد عقد فقط
3. تحتاج الوراثة المتعددة
4. تريد تحديد نوع لسلوك معين
5. توجد علاقة CAN-DO
6. مرونة مستقبلية (يمكن إضافة default methods بدون كسر الكود)

**إرشادات التصميم**:
- فضّل التركيب على الوراثة
- فضّل الواجهات على الفئات المجردة للمرونة
- استخدم الفئات المجردة عندما يكون لديك كود مشترك للمشاركة
- استخدم الواجهات للسلوك متعدد الأشكال عبر فئات غير مرتبطة
- يمكن استخدام كليهما: فئة مجردة تنفذ واجهة (واجهات)

---

## 4️⃣ أمثلة برمجية (Java 8)

### مثال 1: مقارنة حالات الاستخدام الكلاسيكية

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

### مثال 2: الوراثة المتعددة مع الواجهات

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

### مثال 3: إدارة الحالة (State)

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

### مثال 4: Constructors والتهيئة

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

### مثال 5: الدوال الافتراضية في Java 8 - طمس الحدود

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

### مثال 6: اتخاذ القرارات في الواقع

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

### مثال 7: الجمع بين الاثنين - فئة مجردة تنفذ واجهة

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

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما الفرق بين Interface و Abstract Class؟**
2. **متى تختار Interface على Abstract Class؟**
3. **هل يمكن للفئة أن ترث من فئات مجردة متعددة؟**
4. **هل يمكن للواجهة أن ترث من واجهة أخرى؟**
5. **هل يمكن للفئة المجردة أن تنفذ واجهة دون تنفيذ جميع الدوال؟**
6. **ما الذي تغير في Java 8 بخصوص الواجهات؟**
7. **لماذا لا تدعم Java الوراثة المتعددة للفئات؟**
8. **هل يمكنك إعطاء مثال واقعي لاستخدام كل منهما؟**
9. **ما هي الواجهات العلامية (Marker Interfaces)؟**
10. **هل من الممكن أن تمتد فئة من فئة مجردة وتنفذ واجهات في نفس الوقت؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما الفرق بين Interface و Abstract Class؟**

"الفروقات الرئيسية هي: الفئات المجردة يمكن أن تحتوي على دوال مجردة وملموسة، ومتغيرات instance، و constructors، بينما الواجهات تقليدياً كانت تحتوي فقط على دوال مجردة وثوابت. يمكن للفئة تنفيذ واجهات متعددة لكن ترث فئة مجردة واحدة فقط. استخدم الفئات المجردة لعلاقات IS-A مع تنفيذ مشترك، والواجهات لقدرات CAN-DO. منذ Java 8، يمكن للواجهات أن تحتوي على default و static methods مما يطمس الخط الفاصل إلى حد ما، لكن نية التصميم تبقى مختلفة - الفئات المجردة للفئات المرتبطة التي تشارك الكود، والواجهات لتحديد العقود."

**س2: متى تختار Interface على Abstract Class؟**

"أختار الواجهة عندما: أحتاج لتحديد عقد أو قدرة يمكن لفئات غير مرتبطة تنفيذها، مثل Comparable أو Serializable. عندما أحتاج الوراثة المتعددة، لأن الفئة يمكنها تنفيذ واجهات متعددة. عندما أريد أقصى مرونة، لأن الواجهات أسهل في التطوير مع default methods. أو عندما تكون العلاقة CAN-DO بدلاً من IS-A. مثلاً، كل من Car و Boat يمكن أن يكونا Drivable، رغم أنهما غير مرتبطين بالوراثة. استخدم الفئات المجردة عندما يكون لديك كود مشترك للمشاركة بين فئات مرتبطة."

**س3: هل يمكن للفئة أن ترث من فئات مجردة متعددة؟**

"لا، Java لا تدعم الوراثة المتعددة للفئات، بما في ذلك الفئات المجردة. يمكن للفئة أن ترث من فئة واحدة فقط، سواء كانت مجردة أو ملموسة. هذا القيد موجود لتجنب مشكلة الماسة (Diamond Problem)، حيث يظهر غموض حول أي دالة من الأصل يجب وراثتها. ومع ذلك، يمكن للفئة تنفيذ واجهات متعددة، مما يوفر شكلاً من الوراثة المتعددة لتعريف النوع والعقد، وإن لم يكن لمشاركة التنفيذ - رغم أن default methods في Java 8 تعقد هذا قليلاً."

**س4: هل يمكن للواجهة أن ترث من واجهة أخرى؟**

"نعم، يمكن للواجهة أن ترث من واجهة أو أكثر باستخدام كلمة extends. عندما ترث واجهة من أخرى، ترث جميع الدوال المجردة من الواجهة الأم. يجب على الفئة التي تنفذ الواجهة الفرعية تنفيذ دوال كل من الواجهة الفرعية والأم. مثلاً، List ترث من Collection، لذا أي فئة تنفذ List يجب أن تنفذ دوال كل من List و Collection. على عكس الفئات، الواجهات تدعم الوراثة المتعددة، لذا يمكن للواجهة أن ترث من واجهات متعددة."

**س5: هل يمكن للفئة المجردة أن تنفذ واجهة دون تنفيذ جميع الدوال؟**

"نعم، بالتأكيد. يمكن للفئة المجردة أن تنفذ واجهة وتختار عدم تنفيذ بعض أو كل دوالها. تلك الدوال غير المنفذة تبقى مجردة، ويجب على أول فئة فرعية ملموسة تنفيذها. هذا في الواقع نمط شائع - الفئة المجردة توفر تنفيذاً جزئياً للواجهة، والفئات الفرعية الملموسة تكمله. مثلاً، AbstractList تنفذ List لكنها لا تنفذ جميع الدوال، تاركة بعضها للفئات الفرعية مثل ArrayList."

**س6: ما الذي تغير في Java 8 بخصوص الواجهات؟**

"Java 8 قدمت تغييرين رئيسيين للواجهات: default methods و static methods. الدوال الافتراضية تسمح للواجهات بتوفير تنفيذات للدوال باستخدام كلمة default. هذا يتيح لك إضافة دوال جديدة للواجهات بدون كسر التنفيذات الموجودة. الدوال الثابتة في الواجهات تعمل مثل دوال الفئة - تنتمي للواجهة نفسها. هذه التغييرات كانت بشكل أساسي لدعم تعبيرات lambda وتحسين Collections API. Java 9 لاحقاً أضافت private methods في الواجهات لدعم إعادة استخدام الكود في الدوال الافتراضية."

**س7: لماذا لا تدعم Java الوراثة المتعددة للفئات؟**

"Java لا تدعم الوراثة المتعددة للفئات لتجنب مشكلة الماسة (Diamond Problem). هذه تحدث عندما ترث فئة من فئتين كلتاهما لديهما دالة بنفس التوقيع، مما يخلق غموضاً حول أي نسخة يجب وراثتها. مثلاً، إذا كانت فئة C ترث من كل من A و B، وكل من A و B لديهما دالة display()، أي نسخة يجب أن ترثها C؟ لتجنب هذا التعقيد والسلوك غير المتوقع، Java تسمح فقط بالوراثة الفردية للفئات. الواجهات يمكن أن يكون لها وراثة متعددة لأنها تقليدياً كانت تحدد عقوداً فقط بدون تنفيذ، رغم أن default methods في Java 8 تعيد بعض التعقيد هنا."

**س8: هل يمكنك إعطاء مثال واقعي لاستخدام كل منهما؟**

"بالتأكيد. لمثال الفئة المجردة: أنت تبني نظام دفع. ستنشئ فئة مجردة PaymentProcessor مع حقول مشتركة مثل transactionId ودوال مثل generateReceipt()، ثم يكون لديك فئات ملموسة مثل CreditCardProcessor و PayPalProcessor ترثها. لمثال الواجهة: تريد أن تكون فئات مختلفة غير مرتبطة قابلة للمقارنة. ستنشئ واجهة Comparable مع دالة compareTo(). الآن كل من فئات Student و Product يمكنها تنفيذ Comparable، رغم أنهما غير مرتبطتين تماماً. الفئة المجردة تشارك الكود بين أنواع مرتبطة، الواجهة تحدد قدرة عبر أنواع غير مرتبطة."

**س9: ما هي الواجهات العلامية (Marker Interfaces)؟**

"الواجهات العلامية هي واجهات فارغة بدون دوال تعمل كعلامات أو وسوم للإشارة إلى شيء ما عن الفئة. الأمثلة تشمل Serializable و Cloneable و Remote. تخبر الـ JVM أو الإطار البرمجي أن الفئة لديها خصائص أو قدرات معينة. مثلاً، تنفيذ Serializable يُعلّم الفئة كمؤهلة للتسلسل (Serialization). بينما تُعتبر قديمة إلى حد ما - الـ Annotations غالباً مفضلة الآن - الواجهات العلامية لا تزال مستخدمة في Java. إنها شكل من البيانات الوصفية التي تؤثر على كيفية معاملة الفئة من قبل وقت التشغيل أو الإطار البرمجي."

**س10: هل من الممكن أن تمتد فئة من فئة مجردة وتنفذ واجهات في نفس الوقت؟**

"نعم، بالتأكيد. يمكن للفئة أن ترث من فئة مجردة واحدة وتنفذ واجهات متعددة. هذا شائع في الأنظمة جيدة التصميم. مثلاً: 'class Dog extends Animal implements Runnable, Comparable'. هذا يمنحك أفضل ما في العالمين - تحصل على التنفيذ المشترك من الفئة المجردة وقدرات إضافية من الواجهات. القاعدة الوحيدة هي أن جملة extends يجب أن تأتي قبل implements في إعلان الفئة. هذا النمط مستخدم بشكل متكرر في إطار Java Collections."

---

## 7️⃣ الأخطاء الشائعة

1. **الاعتقاد بأنه يمكنك وراثة فئات مجردة متعددة**:
   ```java
   abstract class A { }
   abstract class B { }

   class C extends A, B { }  // Compilation error!
   ```

2. **محاولة إضافة متغيرات instance للواجهات**:
   ```java
   interface MyInterface {
       int count = 0;  // OK - but this is a constant (public static final)
       String name;    // Compilation error - must be initialized!
   }
   ```

3. **استخدام الكلمة المفتاحية الخاطئة للوراثة**:
   ```java
   interface MyInterface { }

   class MyClass implements MyInterface { }  // Correct
   // class MyClass extends MyInterface { }  // Wrong!

   abstract class MyAbstract { }

   class MyClass extends MyAbstract { }  // Correct
   // class MyClass implements MyAbstract { }  // Wrong!
   ```

4. **الافتراض بأن الواجهات يمكن أن تحتوي على constructors**:
   ```java
   interface MyInterface {
       MyInterface() { }  // Compilation error!
   }
   ```

5. **عدم جعل دوال تنفيذ الواجهة public**:
   ```java
   interface MyInterface {
       void method();  // Implicitly public
   }

   class MyClass implements MyInterface {
       void method() { }  // Compilation error - must be public!
   }
   ```

6. **الإفراط في استخدام الفئات المجردة بدلاً من الواجهات**:
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

7. **نسيان تعليق @Override**:
   ```java
   interface MyInterface {
       void display();
   }

   class MyClass implements MyInterface {
       public void dispaly() { }  // Typo - should use @Override
   }
   ```

---

## 8️⃣ نصائح للمقابلات

**ما يجب التأكيد عليه**:
- الفئة المجردة لعلاقة IS-A (مشاركة الكود)، الواجهة لـ CAN-DO (تحديد القدرات)
- واجهات متعددة ممكنة، فئة مجردة واحدة فقط
- Java 8 أضافت default/static methods للواجهات
- كلاهما لا يمكن إنشاء كائنات منهما
- أمثلة واقعية من Java API (List، AbstractList، Comparable، Serializable)

**ما يجب تجنبه**:
- لا تقل أن أحدهما "أفضل" من الآخر (يخدمان أغراضاً مختلفة)
- لا تدعي أن الواجهات "أسرع" (فرق أداء لا يُذكر)
- لا تنسَ ذكر تغييرات Java 8
- لا تخلط بين كلمات extends و implements

**تحت الضغط**:
- ارسم جدول مقارنة بسيط
- استخدم قاعدة IS-A مقابل CAN-DO
- أعطِ أمثلة ملموسة: Animal (فئة مجردة) مقابل Flyable (واجهة)
- اذكر أنه يمكنك استخدام كليهما معاً

**أخطاء يجب تجنبها**:
- "يمكنك وراثة فئات مجردة متعددة" (لا)
- "الواجهات يمكن أن تحتوي على متغيرات instance" (فقط ثوابت)
- "الواجهات يمكن أن تحتوي على constructors" (لا)
- "الفئات المجردة دائماً أفضل" أو العكس (يعتمد على حالة الاستخدام)

**نقاط إضافية**:
- اذكر أنماط التصميم: Template Method (فئة مجردة)، Strategy (واجهة)
- أشر إلى Java Collections: List هي interface، AbstractList هي فئة مجردة
- ناقش التطور: تغييرات Java 8، functional interfaces
- اذكر مبدأ التركيب على الوراثة
- أشر إلى مبادئ SOLID: Dependency Inversion (فضّل الواجهات)
- ناقش marker interfaces مقابل annotations
- اذكر أن التصميم الحديث غالباً يفضل الواجهات للمرونة

**إطار اتخاذ القرار للمشاركة**:
```
استخدم Abstract Class عندما:
✓ الفئات مرتبطة ارتباطاً وثيقاً
✓ لديك كود مشترك للمشاركة
✓ تحتاج متغيرات instance أو constructors
✓ تحتاج أعضاء غير عامة

استخدم Interface عندما:
✓ فئات غير مرتبطة تحتاج سلوكاً مشتركاً
✓ تحتاج الوراثة المتعددة
✓ تريد تحديد عقد/قدرة
✓ تريد أقصى مرونة
```

</div>
