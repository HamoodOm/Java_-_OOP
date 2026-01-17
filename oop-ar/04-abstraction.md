<div dir="rtl">

# التجريد (Abstraction)

## 1️⃣ نظرة عامة على المفهوم

**التجريد (Abstraction)** هو مبدأ من مبادئ البرمجة كائنية التوجه يهدف إلى إخفاء تفاصيل التنفيذ وعرض الميزات الأساسية فقط للمستخدم. يركز على **ماذا** يفعل الكائن بدلاً من **كيف** يفعله.

**المفاهيم الأساسية**:
- **الفئة المجردة (Abstract Class)**: فئة معلنة بكلمة `abstract` لا يمكن إنشاء كائنات منها
- **الدالة المجردة (Abstract Method)**: دالة معلنة بدون تنفيذ (بدون جسم)
- **الفئة الملموسة (Concrete Class)**: فئة غير مجردة توفر تنفيذاً لجميع الدوال
- **الواجهة (Interface)**: نوع مرجعي يحتوي فقط على دوال مجردة (حتى Java 8)

**طريقتان لتحقيق التجريد في Java**:
1. **الفئات المجردة (Abstract Classes)** (تجريد من 0-100%)
2. **الواجهات (Interfaces)** (تجريد 100% في Java 7، جزئي في Java 8+)

**الفوائد**:
- تقليل التعقيد من خلال إخفاء تفاصيل التنفيذ
- تمكين إعادة استخدام الكود من خلال الواجهات المشتركة
- دعم الاقتران الضعيف (Loose Coupling)
- تسهيل الصيانة والتحديثات
- تحديد عقود يجب على الفئات المنفذة اتباعها

**تشبيه من الواقع**: أنت تعرف كيف تقود السيارة (الواجهة) دون أن تعرف كيف يعمل المحرك (التنفيذ).

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **مبدأ أساسي في OOP**: جوهري للتصميم كائني التوجه
- **مهارات التصميم**: يختبر القدرة على إنشاء بنيات مرنة
- **Interface مقابل Abstract Class**: سؤال مقابلة كلاسيكي
- **معرفة الأطر البرمجية**: معظم الأطر تستخدم التجريد بكثافة
- **التطبيق العملي**: ضروري لبناء أنظمة قابلة للصيانة
- **تصميم API**: يظهر فهم البرمجة القائمة على العقود

---

## 3️⃣ القواعد والحقائق الأساسية

**الفئات المجردة (Abstract Classes)**:
- لا يمكن إنشاء كائنات منها
- تُعلن باستخدام كلمة `abstract`
- يمكن أن تحتوي على دوال مجردة (بدون جسم) ودوال ملموسة (بجسم)
- يمكن أن تحتوي على constructors (تُستدعى عند إنشاء كائن الفئة الفرعية)
- يمكن أن تحتوي على متغيرات instance
- يمكن أن تحتوي على دوال ومتغيرات static
- يجب على الفئة الفرعية تنفيذ جميع الدوال المجردة أو أن تكون مجردة بدورها
- تدعم الوراثة الفردية فقط (يمكن extends فئة مجردة واحدة)

**الدوال المجردة (Abstract Methods)**:
- تُعلن بدون جسم: `public abstract void method();`
- يجب أن تكون في فئة مجردة أو واجهة
- لا يمكن أن تكون `private` أو `final` أو `static`
- يجب تنفيذها بواسطة أول فئة ملموسة فرعية

**الواجهات (Interfaces)** (Java 8):
- مجردة 100% بشكل افتراضي (قبل Java 8)
- جميع الدوال ضمنياً `public abstract` (قبل Java 8)
- جميع الحقول ضمنياً `public static final` (ثوابت)
- لا يمكن أن تحتوي على constructors
- لا يمكن أن تحتوي على متغيرات instance
- يمكن للفئة تنفيذ واجهات متعددة (وراثة متعددة)
- من Java 8: يمكن أن تحتوي على default methods و static methods
- من Java 9: يمكن أن تحتوي على private methods

**مقارنة بين Abstract Class و Interface**:
| الميزة | Abstract Class | Interface |
|--------|----------------|-----------|
| الإنشاء | لا يمكن إنشاء كائن | لا يمكن إنشاء كائن |
| الدوال | مجردة + ملموسة | مجردة (+ default/static في Java 8+) |
| المتغيرات | Instance + Static | فقط static final (ثوابت) |
| Constructor | نعم | لا |
| الوراثة | فردية (extends) | متعددة (implements) |
| محددات الوصول | أي محدد | public فقط (للدوال) |
| متى تُستخدم | علاقة IS-A | قدرة CAN-DO |

---

## 4️⃣ أمثلة برمجية (Java 8)

### مثال 1: فئة مجردة أساسية

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

### مثال 2: واجهة أساسية

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

### مثال 3: تنفيذ واجهات متعددة

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

### مثال 4: فئة مجردة مع واجهة

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

### مثال 5: واجهة مع ثوابت

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

### مثال 6: الدوال الافتراضية في الواجهات (Java 8)

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

### مثال 7: مثال واقعي - نظام الدفع

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

### مثال 8: تحقيق التجريد الكامل (100%)

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

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما هو التجريد (Abstraction) في Java؟**
2. **ما الفرق بين Abstract Class و Interface؟**
3. **هل يمكن إنشاء كائن من فئة مجردة؟**
4. **هل يمكن للفئة المجردة أن تحتوي على constructors؟**
5. **هل يمكن أن تكون هناك دوال مجردة في فئة غير مجردة؟**
6. **هل يمكن للواجهة أن تحتوي على دوال ملموسة؟**
7. **متى نستخدم Abstract Class ومتى نستخدم Interface؟**
8. **هل يمكن أن تحتوي الواجهة على static methods؟**
9. **ما الفرق بين التجريد (Abstraction) والتغليف (Encapsulation)؟**
10. **هل يمكن للفئة المجردة أن تنفذ واجهة؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما هو التجريد (Abstraction) في Java؟**

"التجريد هو مبدأ من مبادئ OOP يهدف إلى إخفاء تفاصيل التنفيذ وعرض الميزات الأساسية فقط للمستخدم. يركز على ماذا يفعل الكائن بدلاً من كيف يفعله. في Java، نحقق التجريد من خلال الفئات المجردة والواجهات. على سبيل المثال، عند استخدام List، لا تحتاج لمعرفة ما إذا كانت ArrayList أو LinkedList - فقط تستخدم دوال واجهة List. هذا يقلل التعقيد ويسمح بتغيير التنفيذ دون التأثير على كود العميل."

**س2: ما الفرق بين Abstract Class و Interface؟**

"الفئات المجردة يمكن أن تحتوي على دوال مجردة وملموسة، ومتغيرات instance، و constructors. يمكن للفئة أن ترث فئة مجردة واحدة فقط. الواجهات، قبل Java 8، كانت تحتوي فقط على دوال مجردة وثوابت. من Java 8، يمكن أن تحتوي على default و static methods. يمكن للفئة تنفيذ واجهات متعددة. استخدم الفئات المجردة لعلاقات IS-A مع كود مشترك. استخدم الواجهات لقدرات CAN-DO أو عند الحاجة للوراثة المتعددة. مثلاً، الكلب IS-A حيوان (فئة مجردة) لكنه CAN-DO Runnable، Comparable (واجهات)."

**س3: هل يمكن إنشاء كائن من فئة مجردة؟**

"لا، لا يمكننا إنشاء كائن مباشرة من فئة مجردة. الفئات المجردة غير مكتملة بالتصميم - قد تحتوي على دوال مجردة بدون تنفيذ. ومع ذلك، يمكننا إنشاء مراجع من نوع الفئة المجردة وتعيين كائنات الفئات الفرعية لها. يمكننا أيضاً إنشاء anonymous inner classes من الفئات المجردة، لكن هذا تقنياً إنشاء لكائن من فئة فرعية بدون اسم، وليس الفئة المجردة نفسها."

**س4: هل يمكن للفئة المجردة أن تحتوي على constructors؟**

"نعم، يمكن للفئات المجردة أن تحتوي على constructors. تُستدعى هذه الـ constructors عند إنشاء كائن من الفئة الفرعية. يجب على constructor الفئة الفرعية استدعاء constructor الفئة المجردة الأم باستخدام super(). هذا يسمح للفئة المجردة بتهيئة حقولها. على الرغم من أنك لا تستطيع إنشاء كائن مباشرة من فئة مجردة، إلا أن constructor ضروري للتهيئة الصحيحة للكائنات الفرعية التي ترثها."

**س5: هل يمكن أن تكون هناك دوال مجردة في فئة غير مجردة؟**

"لا، إذا كانت الفئة تحتوي على دالة مجردة واحدة على الأقل، يجب أن تُعلن الفئة كمجردة. إذا حاولت وضع دالة مجردة في فئة ملموسة، ستحصل على خطأ تجميع. هذا منطقي لأن الفئات الملموسة يجب أن تكون قابلة للإنشاء، ولا يمكنك إنشاء كائن من فئة بدوال ليس لها تنفيذ. إذا أردت دوال مجردة، أعلن الفئة كمجردة."

**س6: هل يمكن للواجهة أن تحتوي على دوال ملموسة؟**

"نعم، بدءاً من Java 8، يمكن للواجهات أن تحتوي على دوال ملموسة من خلال default methods و static methods. الدوال الافتراضية توفر تنفيذاً افتراضياً يمكن للفئات المنفذة استخدامه أو تجاوزه. الدوال الثابتة تنتمي للواجهة نفسها وتوفر دوال مساعدة. قبل Java 8، كانت الواجهات تحتوي فقط على دوال مجردة، وهو تجريد 100%. Java 9 أضافت أيضاً private methods في الواجهات."

**س7: متى نستخدم Abstract Class ومتى نستخدم Interface؟**

"استخدم الفئات المجردة عندما تكون هناك علاقة IS-A وتريد مشاركة كود مشترك بين فئات مرتبطة. مثلاً، أنواع مختلفة من الحيوانات تشترك في سلوك مشترك. استخدم الواجهات عندما تريد تحديد قدرة أو عقد يمكن لفئات غير مرتبطة تنفيذه، مثل Comparable أو Serializable. أيضاً، استخدم الواجهات عند الحاجة للوراثة المتعددة. قاعدة عملية: إذا كانت الفئات تشترك في تنفيذ مشترك، استخدم فئة مجردة. إذا كنت تحدد عقداً أو قدرة، استخدم واجهة."

**س8: هل يمكن أن تحتوي الواجهة على static methods؟**

"نعم، من Java 8 وما بعدها، يمكن للواجهات أن تحتوي على static methods. هذه الدوال تنتمي للواجهة نفسها، وليس للفئات المنفذة. إنها مفيدة للدوال المساعدة المتعلقة بالواجهة. مثلاً، واجهة Comparator تحتوي على static methods مثل naturalOrder(). الدوال الثابتة في الواجهات لا يمكن تجاوزها ويجب استدعاؤها باستخدام اسم الواجهة، وليس الفئة المنفذة."

**س9: ما الفرق بين التجريد (Abstraction) والتغليف (Encapsulation)؟**

"التجريد يتعلق بإخفاء التعقيد وعرض الميزات الأساسية فقط - التركيز على ماذا يفعل الكائن. يتحقق من خلال الفئات المجردة والواجهات. التغليف يتعلق بتجميع البيانات والدوال معاً وإخفاء الحالة الداخلية باستخدام محددات الوصول - التركيز على كيفية حماية البيانات. مثلاً، واجهة Car توفر التجريد بتحديد دوال start()، stop() بدون تفاصيل التنفيذ. التغليف عندما تحافظ فئة Car على engine كـ private وتوفر دوال عامة للتفاعل معه. التجريد للتصميم والواجهة، التغليف لحماية البيانات."

**س10: هل يمكن للفئة المجردة أن تنفذ واجهة؟**

"نعم، يمكن للفئة المجردة أن تنفذ واجهة. يمكن للفئة المجردة أن تختار تنفيذ بعض أو كل أو لا شيء من دوال الواجهة. أي دوال لم تُنفذ تبقى مجردة، ويجب على أول فئة فرعية ملموسة تنفيذها. هذا نمط شائع ومفيد عندما تريد توفير تنفيذ جزئي لواجهة وترك الفئات الفرعية تكمله. إنه نمط شائع في الأطر البرمجية حيث توجد فئة قاعدة مجردة تنفذ واجهة وتوفر وظائف مشتركة."

---

## 7️⃣ الأخطاء الشائعة

1. **محاولة إنشاء كائن من فئة مجردة**:
   ```java
   abstract class Animal { }

   Animal animal = new Animal();  // Compilation error!
   ```

2. **نسيان تنفيذ جميع الدوال المجردة**:
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

3. **جعل الدوال المجردة private**:
   ```java
   abstract class MyClass {
       private abstract void method();  // Compilation error!
       // Abstract methods cannot be private
   }
   ```

4. **إعلان الدوال المجردة كـ final**:
   ```java
   abstract class MyClass {
       final abstract void method();  // Compilation error!
       // Cannot be both final and abstract
   }
   ```

5. **إنشاء متغيرات instance في الواجهات** (قبل Java 8):
   ```java
   interface MyInterface {
       int value = 10;  // OK - this is a constant (public static final)
       // But you cannot have non-final variables
   }
   ```

6. **عدم استخدام @Override عند تنفيذ دوال الواجهة**:
   ```java
   interface MyInterface {
       void display();
   }

   class MyClass implements MyInterface {
       public void Display() { }  // Typo - should use @Override
   }
   ```

7. **تقليل محدد الوصول عند تنفيذ الواجهة**:
   ```java
   interface MyInterface {
       void method();  // Implicitly public
   }

   class MyClass implements MyInterface {
       void method() { }  // Compilation error - must be public!
   }
   ```

8. **الخلط بين استخدام الفئة المجردة والواجهة**:
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

## 8️⃣ نصائح للمقابلات

**ما يجب التأكيد عليه**:
- التجريد يخفي التعقيد ويعرض الميزات الأساسية فقط
- طريقتان: الفئات المجردة (0-100%) والواجهات (100%)
- الفئة المجردة لعلاقة IS-A مع كود مشترك؛ الواجهة لقدرات CAN-DO
- الفئة المجردة = وراثة فردية؛ الواجهة = وراثة متعددة
- Java 8 أضافت default و static methods للواجهات

**ما يجب تجنبه**:
- لا تدعي أن الواجهات يمكن أن تحتوي على متغيرات instance (فقط ثوابت)
- لا تقل أن الفئات المجردة "أبطأ" (لا يوجد فرق أداء ملحوظ)
- لا تخلط بين التجريد والتغليف (أغراض مختلفة)
- لا تنسَ ذكر تغييرات Java 8 للواجهات

**تحت الضغط**:
- استخدم أمثلة بسيطة: Vehicle (فئة مجردة) مقابل Driveable (واجهة)
- ارسم جدول مقارنة بين الفئة المجردة والواجهة
- اذكر أمثلة Java الحقيقية: إطار Collections يستخدم كليهما
- اذكر الفرق الرئيسي: الفئة المجردة بها كود، الواجهة تحدد عقداً

**أخطاء يجب تجنبها**:
- "يمكنك إنشاء كائن من فئة مجردة" (لا يمكنك)
- "الواجهات يمكن أن تحتوي على constructors" (لا يمكنها)
- "الفئة المجردة والواجهة نفس الشيء" (أدوات مختلفة)
- "لا يمكن أن تحتوي الواجهات على دوال بتنفيذ" (خطأ من Java 8+)

**نقاط إضافية**:
- اذكر marker interfaces مثل Serializable و Cloneable
- ناقش functional interfaces في Java 8 لتعبيرات lambda
- أشر إلى نمط Template Method باستخدام الفئات المجردة
- اذكر أن فئة Object ملموسة وليست مجردة
- اشرح كيف يمكّن التجريد من تعدد الأشكال (Polymorphism)
- اشرح الوراثة المتعددة مع الواجهات (interface A، interface B)
- أشر إلى الأطر الحقيقية: Spring تستخدم كليهما بكثافة

</div>
