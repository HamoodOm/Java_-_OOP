<div dir="rtl">

# تعدد الأشكال (Polymorphism)

## 1. نظرة عامة على المفهوم

**تعدد الأشكال (Polymorphism)** يعني "أشكال متعددة". هو قدرة الكائن على اتخاذ أشكال عديدة أو قدرة الدالة على أداء مهام مختلفة بناءً على الكائن الذي يعمل عليه.

**نوعان من تعدد الأشكال**:

1. **تعدد الأشكال في وقت الترجمة (Compile-time Polymorphism)**:
   - يُحقق من خلال **method overloading**
   - يُحدد أي دالة تُستدعى في وقت الترجمة
   - نفس اسم الدالة، معاملات مختلفة
   - يُسمى أيضاً الربط الثابت أو الربط المبكر

2. **تعدد الأشكال في وقت التشغيل (Runtime Polymorphism)**:
   - يُحقق من خلال **method overriding**
   - يُحدد أي دالة تُستدعى في وقت التشغيل
   - مرجع الأب يشير إلى كائن الابن
   - يُسمى أيضاً الربط الديناميكي أو الربط المتأخر

**المبدأ الأساسي**: "واجهة واحدة، تنفيذات متعددة"

الاستخدام الأقوى لتعدد الأشكال هو عندما يمكن لمرجع class أب أن يشير إلى كائن class ابن ويستدعي دوال الابن المعاد تعريفها في وقت التشغيل.

---

## 2. لماذا يسأل المحاورون عن هذا

- **مبدأ أساسي في OOP**: تعدد الأشكال أساسي للتصميم كائني التوجه
- **مرونة التصميم**: يختبر فهم الكود المرن والقابل للتوسيع
- **شائع في الـ frameworks**: معظم الـ frameworks تستخدم تعدد الأشكال بكثافة
- **سلوك وقت التشغيل**: يُظهر فهم كيفية حل Java للدوال
- **أنماط التصميم**: العديد من الأنماط تعتمد على تعدد الأشكال (Strategy، Template، Factory)
- **الواجهة مقابل التنفيذ**: يختبر معرفة فصل المسؤوليات

---

## 3. القواعد والحقائق الأساسية

**Method Overloading (وقت الترجمة)**:
- نفس اسم الدالة، قائمة معاملات مختلفة (النوع، العدد، أو الترتيب)
- نوع الإرجاع وحده ليس كافياً للـ overloading
- يمكن أن يكون في نفس الـ class أو في علاقة أب-ابن
- يُحل في وقت الترجمة بناءً على نوع المرجع
- محددات الوصول يمكن أن تكون مختلفة

**Method Overriding (وقت التشغيل)**:
- نفس توقيع الدالة (الاسم والمعاملات)
- يجب أن تكون هناك علاقة IS-A (وراثة أو تنفيذ interface)
- الـ class الابن يوفر تنفيذاً محدداً
- نوع الإرجاع يجب أن يكون نفسه أو متغايراً (covariant) (نوع فرعي)
- لا يمكن تقليل الرؤية (public ← protected غير مسموح)
- لا يمكن إعادة تعريف: final، static، أو private methods
- يُحل في وقت التشغيل بناءً على نوع الكائن الفعلي

**@Override Annotation**:
- فحص في وقت الترجمة لإعادة التعريف الصحيحة
- ليس إلزامياً لكنه موصى به بشدة
- يكشف الأخطاء الإملائية وعدم تطابق التوقيعات

**الإرسال الديناميكي للدوال (Dynamic Method Dispatch)**:
- JVM تحدد أي دالة تُستدعى في وقت التشغيل
- بناءً على نوع الكائن الفعلي، وليس نوع المرجع
- الآلية الأساسية لتعدد الأشكال في وقت التشغيل

**أنواع الإرجاع المتغايرة (Covariant Return Types)** (Java 5+):
- الدالة المعاد تعريفها يمكن أن تُرجع نوعاً فرعياً من نوع إرجاع الأب
- مثال: الأب يُرجع `Animal`، الابن يمكن أن يُرجع `Dog`

**قواعد إعادة التعريف**:
- اسم الدالة يجب أن يكون متطابقاً
- قائمة المعاملات يجب أن تكون متطابقة
- نوع الإرجاع يجب أن يكون نفسه أو نوعاً فرعياً متغايراً
- محدد الوصول يجب أن يكون نفسه أو أقل تقييداً
- لا يمكن رمي exceptions جديدة أو أوسع نطاقاً

---

## 4. أمثلة برمجية (Java 8)

### المثال 1: تعدد الأشكال في وقت الترجمة (Method Overloading)

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

### المثال 2: تعدد الأشكال في وقت التشغيل (Method Overriding)

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

### المثال 3: تعدد الأشكال مع المصفوفات

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

### المثال 4: الإرسال الديناميكي للدوال

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

### المثال 5: تعدد الأشكال مع الـ Interfaces

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

### المثال 6: أنواع الإرجاع المتغايرة

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

### المثال 7: Overloading مقابل Overriding

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

### المثال 8: مثال واقعي - نظام معالجة المدفوعات

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

## 5. أسئلة المقابلات الشائعة

1. **ما هو تعدد الأشكال في Java؟**
2. **ما هي أنواع تعدد الأشكال؟**
3. **ما الفرق بين تعدد الأشكال في وقت الترجمة ووقت التشغيل؟**
4. **ما هو method overloading؟ أعطِ مثالاً.**
5. **ما هو method overriding؟ أعطِ مثالاً.**
6. **هل يمكننا إعادة تعريف الدوال الثابتة (static)؟**
7. **هل يمكننا عمل overload لدالة main؟**
8. **ما هو الإرسال الديناميكي للدوال (dynamic method dispatch)؟**
9. **ما فائدة @Override annotation؟**
10. **ما هي أنواع الإرجاع المتغايرة (covariant return types)؟**

---

## 6. إجابات نموذجية

**س1: ما هو تعدد الأشكال في Java؟**

"تعدد الأشكال يعني 'أشكال متعددة'. هو قدرة الكائن على اتخاذ أشكال متعددة أو قدرة الدالة على التصرف بشكل مختلف بناءً على الكائن الذي يعمل عليه. في Java، لدينا نوعان: تعدد الأشكال في وقت الترجمة يُحقق من خلال method overloading، وتعدد الأشكال في وقت التشغيل يُحقق من خلال method overriding. الاستخدام الأقوى هو عندما يشير مرجع class أب إلى كائن class ابن، ويُستدعى دالة الابن المناسبة في وقت التشغيل بناءً على نوع الكائن الفعلي."

**س2: ما هي أنواع تعدد الأشكال؟**

"Java لديها نوعان من تعدد الأشكال. تعدد الأشكال في وقت الترجمة أو الثابت يُحقق من خلال method overloading، حيث لدينا عدة دوال بنفس الاسم لكن معاملات مختلفة في نفس الـ class. المترجم يحدد أي دالة تُستدعى بناءً على المعاملات. تعدد الأشكال في وقت التشغيل أو الديناميكي يُحقق من خلال method overriding، حيث يوفر class ابن تنفيذه الخاص لدالة class الأب. JVM تحدد أي دالة تُستدعى في وقت التشغيل بناءً على نوع الكائن الفعلي، وليس نوع المرجع."

**س3: ما الفرق بين تعدد الأشكال في وقت الترجمة ووقت التشغيل؟**

"تعدد الأشكال في وقت الترجمة يُحل أثناء الترجمة من خلال method overloading. المترجم يعرف أي دالة تُستدعى بناءً على توقيع الدالة. يُسمى أيضاً الربط الثابت أو المبكر. تعدد الأشكال في وقت التشغيل يُحل أثناء تنفيذ البرنامج من خلال method overriding. JVM تحدد أي دالة معاد تعريفها تُستدعى بناءً على نوع الكائن الفعلي في وقت التشغيل. يُسمى أيضاً الربط الديناميكي أو المتأخر. تعدد الأشكال في وقت التشغيل أكثر مرونة وهو أساس العديد من أنماط التصميم."

**س4: ما هو method overloading؟ أعطِ مثالاً.**

"Method overloading هو عندما يكون لدينا عدة دوال بنفس الاسم لكن قوائم معاملات مختلفة في نفس الـ class. المعاملات يمكن أن تختلف في العدد أو النوع أو الترتيب. على سبيل المثال، class Calculator قد يكون لديه add(int, int)، add(double, double)، و add(int, int, int). المترجم يختار الدالة المناسبة بناءً على المعاملات المقدمة. نوع الإرجاع وحده ليس كافياً للـ overloading - قائمة المعاملات يجب أن تختلف."

**س5: ما هو method overriding؟ أعطِ مثالاً.**

"Method overriding هو عندما يوفر class ابن تنفيذاً محدداً لدالة معرّفة بالفعل في class الأب. الدالة يجب أن يكون لها نفس التوقيع - نفس الاسم والمعاملات. على سبيل المثال، إذا كان لدى class Animal دالة makeSound()، يمكن لـ class Dog إعادة تعريفها لطباعة 'Woof' بينما class Cat يعيد تعريفها لطباعة 'Meow'. عندما نستدعي makeSound() على مرجع Animal يشير إلى كائن Dog، نسخة Dog تُنفذ في وقت التشغيل."

**س6: هل يمكننا إعادة تعريف الدوال الثابتة (static)؟**

"لا، لا يمكننا إعادة تعريف الدوال الثابتة. الدوال الثابتة تنتمي للـ class، وليس للـ instances، لذا تُربط في وقت الترجمة بناءً على نوع المرجع. إذا عرّفت دالة ثابتة في class ابن بنفس توقيع دالة ثابتة في الأب، يُسمى هذا إخفاء الدالة (method hiding)، وليس إعادة تعريف. الدالة التي تُنفذ تعتمد على نوع المرجع، وليس نوع الكائن الفعلي. هذا يختلف جوهرياً عن إعادة تعريف دوال الـ instance، التي تُحل في وقت التشغيل."

**س7: هل يمكننا عمل overload لدالة main؟**

"نعم، يمكننا عمل overload لدالة main، لكن فقط public static void main(String[] args) تعمل كنقطة دخول لـ JVM. يمكنك إنشاء دوال main أخرى بمعاملات مختلفة مثل main(int num) أو main(String str, int num)، وستُعامل كدوال عادية معمول لها overload. ومع ذلك، JVM ستبحث دائماً عن التوقيع القياسي لبدء البرنامج."

**س8: ما هو الإرسال الديناميكي للدوال (dynamic method dispatch)؟**

"الإرسال الديناميكي للدوال هو الآلية التي تُنفذ بها Java تعدد الأشكال في وقت التشغيل. عندما تُستدعى دالة على مرجع كائن، تحدد JVM في وقت التشغيل أي نسخة من الدالة تُنفذ بناءً على نوع الكائن الفعلي، وليس نوع المرجع. على سبيل المثال، إذا كان لدينا Animal ref = new Dog()، واستدعينا ref.makeSound()، JVM تُرسل الاستدعاء لدالة makeSound() الخاصة بـ Dog في وقت التشغيل، رغم أن نوع المرجع هو Animal. هذه هي الآلية الأساسية التي تجعل تعدد الأشكال يعمل."

**س9: ما فائدة @Override annotation؟**

"@Override annotation هو فحص في وقت الترجمة يضمن أنك فعلاً تعيد تعريف دالة من class الأب. إذا ارتكبت خطأ في توقيع الدالة، مثل خطأ إملائي في اسم الدالة أو معاملات خاطئة، المترجم سيرمي خطأ. ليس إلزامياً، لكنه موصى به بشدة لأنه يكتشف الأخطاء مبكراً. على سبيل المثال، إذا كتبت 'public void dispaly()' بدلاً من 'display()'، بدون @Override ستُنشئ دالة جديدة، لكن مع @Override ستحصل على خطأ ترجمة يُنبهك للخطأ الإملائي."

**س10: ما هي أنواع الإرجاع المتغايرة (covariant return types)؟**

"أنواع الإرجاع المتغايرة، المقدمة في Java 5، تسمح للدالة المعاد تعريفها بإرجاع نوع فرعي من النوع الذي تُرجعه دالة الأب. على سبيل المثال، إذا كانت دالة class الأب تُرجع Animal، يمكن لـ class الابن إعادة تعريفها لتُرجع Dog، وهو نوع فرعي من Animal. هذا يجعل الكود أكثر أماناً من حيث الأنواع ويُلغي الحاجة للتحويل (casting). مفيد بشكل خاص في دوال clone() وأنماط Factory حيث تريد إرجاع النوع المحدد بدلاً من نوع عام."

---

## 7. الأخطاء الشائعة

1. **الخلط بين overloading و overriding**:
   ```java
   class Parent {
       public void display(int num) { }
   }

   class Child extends Parent {
       // This is overloading, NOT overriding
       public void display(double num) { }
   }
   ```

2. **محاولة إعادة التعريف بأنواع إرجاع غير متوافقة**:
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

3. **تقليل رؤية محدد الوصول**:
   ```java
   class Parent {
       public void method() { }
   }

   class Child extends Parent {
       @Override
       protected void method() { }  // Compilation error!
   }
   ```

4. **محاولة إعادة تعريف دوال final**:
   ```java
   class Parent {
       public final void method() { }
   }

   class Child extends Parent {
       // @Override
       // public void method() { }  // Compilation error!
   }
   ```

5. **افتراض أن overloading يعمل مع الوراثة تلقائياً**:
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

6. **عدم استخدام instanceof قبل التحويل (downcasting)**:
   ```java
   Animal animal = new Animal();
   Dog dog = (Dog) animal;  // ClassCastException at runtime!

   // Should use:
   if (animal instanceof Dog) {
       Dog dog = (Dog) animal;
   }
   ```

7. **الاعتقاد أن الدوال الثابتة يمكن إعادة تعريفها**:
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

8. **نسيان @Override annotation**:
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

## 8. نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- نوعان: وقت الترجمة (overloading) ووقت التشغيل (overriding)
- تعدد الأشكال في وقت التشغيل يُحل بناءً على نوع الكائن، وليس نوع المرجع
- الإرسال الديناميكي للدوال هو آلية تعدد الأشكال في وقت التشغيل
- تعدد الأشكال يُمكّن من كود مرن وقابل للتوسيع
- مبدأ "واجهة واحدة، تنفيذات متعددة"

**ما يجب تجنبه**:
- لا تخلط بين إخفاء الدوال (static) وإعادة تعريف الدوال (instance)
- لا تدعي أن overloading فقط في نفس الـ class (يمكن أن يكون في علاقة أب-ابن أيضاً)
- لا تقل "تعدد الأشكال يعني method overriding" (هو أوسع)
- لا تنسى أن الـ interfaces أيضاً تُمكّن من تعدد الأشكال

**تحت الضغط**:
- ابدأ بمثال بسيط: Animal/Dog/Cat مع makeSound()
- ارسمها: مرجع الأب ← كائن الابن ← استدعاء الدالة في وقت التشغيل
- اذكر كلا النوعين: وقت الترجمة (overloading) ووقت التشغيل (overriding)
- استخدم عبارة "يُحل في وقت الترجمة مقابل وقت التشغيل"

**علامات حمراء يجب تجنبها**:
- "Overloading و overriding نفس الشيء" (مختلفان تماماً)
- "الدوال الثابتة يمكن إعادة تعريفها" (تُخفى، لا تُعاد تعريفها)
- "نوع الإرجاع كافٍ للـ overloading" (قائمة المعاملات يجب أن تختلف)
- "تعدد الأشكال يعمل فقط مع الـ classes" (يعمل مع الـ interfaces أيضاً)

**نقاط إضافية**:
- اذكر أن تعدد الأشكال أساسي لمبادئ SOLID (Open/Closed، Liskov Substitution)
- ناقش أنماط التصميم الحقيقية: Strategy، Template Method، Factory
- أشر إلى أن جميع الـ collection classes تستخدم تعدد الأشكال (List interface)
- اذكر الإرسال الديناميكي للدوال وجدول الدوال الافتراضية (vtable)
- اشرح كيف يُمكّن تعدد الأشكال من معماريات الـ plugins
- ناقش كيف تعتمد frameworks مثل Spring بشكل كبير على تعدد الأشكال

</div>
