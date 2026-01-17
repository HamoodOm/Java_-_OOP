<div dir="rtl">

# التغليف (Encapsulation)

## 1. نظرة عامة على المفهوم

**التغليف (Encapsulation)** هو أحد المبادئ الأربعة الأساسية في البرمجة كائنية التوجه (OOP). وهو يتضمن:
1. **تجميع البيانات (الحقول) والدوال التي تعمل على هذه البيانات داخل وحدة واحدة (class)**
2. **إخفاء الحالة الداخلية وفرض التفاعل من خلال الدوال فقط (إخفاء المعلومات)**

**الفكرة الأساسية**: اجعل الحقول خاصة (private) ووفر دوال getter/setter عامة (public) للوصول إليها وتعديلها.

**الفوائد**:
- **حماية البيانات**: يمنع التعديلات غير المصرح بها أو غير الصالحة
- **المرونة**: يمكن تغيير التنفيذ الداخلي دون التأثير على الكود الخارجي
- **التحكم**: يمكن إضافة منطق التحقق في دوال setter
- **سهولة الصيانة**: التغييرات تكون محصورة داخل الـ class

**التغليف = إخفاء البيانات + التجريد**

---

## 2. لماذا يسأل المحاورون عن هذا

- **مبدأ أساسي في OOP**: يختبر فهمك للمفاهيم الأساسية للبرمجة كائنية التوجه
- **مهارات التصميم**: يظهر قدرتك على تصميم classes قابلة للصيانة وآمنة
- **الأهمية العملية**: يُستخدم في كل تطبيق Java في بيئات الإنتاج
- **يميز المبتدئ عن المحترف**: المبتدئون يعرفون المفهوم؛ المحترفون يعرفون متى ولماذا يطبقونه
- **الوعي الأمني**: فهم حماية البيانات والتحقق منها

---

## 3. القواعد والحقائق الأساسية

**قواعد التطبيق**:
- أعلن عن متغيرات/حقول الـ class كـ `private`
- وفر دوال `getter` عامة لقراءة القيم
- وفر دوال `setter` عامة لتعديل القيم
- أضف منطق التحقق في دوال setter للحفاظ على سلامة البيانات
- استخدم أسماء دوال ذات معنى: `getName()`, `setName(String name)`

**محددات الوصول (Access Modifiers)** (من الأكثر تقييداً إلى الأقل):
- `private`: يمكن الوصول إليه فقط داخل نفس الـ class
- `default` (بدون محدد): يمكن الوصول إليه داخل نفس الـ package
- `protected`: يمكن الوصول إليه داخل نفس الـ package والـ subclasses
- `public`: يمكن الوصول إليه من أي مكان

**اصطلاحات التسمية**:
- Getter: `getPropertyName()` (أو `isPropertyName()` للقيم المنطقية boolean)
- Setter: `setPropertyName(Type value)`
- اتبع اصطلاح JavaBeans للتوافق مع الـ frameworks

**أفضل الممارسات**:
- لا توفر setters إذا كان الحقل للقراءة فقط
- لا تُرجع الكائنات القابلة للتعديل مباشرة (استخدم النسخ الدفاعي)
- تحقق من المدخلات في دوال setter
- قم بتهيئة الحقول بقيم صالحة
- استخدم `final` للحقول غير القابلة للتغيير

**الأنماط الشائعة**:
- **class للقراءة فقط**: getters بدون setters
- **class غير قابل للتغيير (Immutable)**: جميع الحقول final، بدون setters
- **Fluent setters**: تُرجع `this` لتسلسل الدوال

---

## 4. أمثلة برمجية (Java 8)

### المثال 1: التغليف الأساسي

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

### المثال 2: الخصائص للقراءة فقط

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

### المثال 3: النسخ الدفاعي

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

### المثال 4: الواجهة السلسة مع تسلسل الدوال

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

### المثال 5: الـ Class غير القابل للتغيير (التغليف المثالي)

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

### المثال 6: توضيح محددات الوصول

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

## 5. أسئلة المقابلات الشائعة

1. **ما هو التغليف (Encapsulation) في Java؟**
2. **لماذا نحتاج إلى التغليف؟**
3. **ما الفرق بين التغليف والتجريد؟**
4. **كيف تحقق التغليف في Java؟**
5. **ما هي دوال getters و setters؟**
6. **هل يمكن أن يكون لدينا class بدوال getters فقط بدون setters؟**
7. **ما هو النسخ الدفاعي ولماذا هو مهم؟**
8. **ما هي محددات الوصول الأربعة في Java؟**
9. **هل من الضروري جعل جميع الحقول private لتحقيق التغليف؟**
10. **كيف يوفر التغليف إخفاء البيانات؟**

---

## 6. إجابات نموذجية

**س1: ما هو التغليف (Encapsulation) في Java؟**

"التغليف هو مبدأ في OOP يقوم على تجميع البيانات والدوال التي تعمل على هذه البيانات داخل class واحد، مع إخفاء الحالة الداخلية عن الوصول الخارجي. يتم تحقيقه بجعل الحقول private وتوفير دوال getter و setter عامة للتحكم في الوصول. هذا يحمي سلامة البيانات بمنع التعديلات غير المصرح بها أو غير الصالحة، ويسمح بتغيير التنفيذ الداخلي دون التأثير على الكود الخارجي."

**س2: لماذا نحتاج إلى التغليف؟**

"نحتاج إلى التغليف لعدة أسباب. أولاً، يوفر حماية البيانات بمنع الوصول المباشر للحقول، مما قد يؤدي إلى حالات غير صالحة. ثانياً، يوفر المرونة - يمكننا تغيير التنفيذ الداخلي دون كسر الكود الخارجي. ثالثاً، يمكننا إضافة منطق التحقق في دوال setter للحفاظ على سلامة البيانات. رابعاً، يحسن قابلية الصيانة بحصر التغييرات داخل الـ class. وأخيراً، يوفر تحكماً أفضل في كيفية الوصول إلى البيانات وتعديلها، وهو أمر ضروري لبناء تطبيقات قوية."

**س3: ما الفرق بين التغليف والتجريد؟**

"التغليف يتعلق بإخفاء البيانات الداخلية وتفاصيل التنفيذ من خلال تجميعها في class والتحكم في الوصول عبر الدوال. يركز على 'كيف' نحقق إخفاء البيانات باستخدام محددات الوصول. التجريد يتعلق بإخفاء التعقيد وإظهار الميزات الأساسية فقط للمستخدم. يركز على 'ماذا' يفعل الكائن بدلاً من 'كيف' يفعله. التغليف هو آلية لتحقيق التجريد، لكنهما يخدمان أغراضاً مختلفة - التغليف يحمي البيانات، بينما التجريد يقلل التعقيد."

**س4: كيف تحقق التغليف في Java؟**

"لتحقيق التغليف، اتبع هذه الخطوات: أولاً، أعلن عن جميع الحقول كـ private. ثانياً، وفر دوال getter عامة لقراءة قيم الحقول. ثالثاً، وفر دوال setter عامة لتعديل قيم الحقول، متضمنة منطق التحقق. رابعاً، تأكد من أن دوال setter تتحقق من المدخلات للحفاظ على سلامة البيانات. اختيارياً، يمكنك جعل الحقول final وحذف setters للـ classes غير القابلة للتغيير، أو استخدام النسخ الدفاعي للكائنات القابلة للتعديل لمنع التعديلات الخارجية."

**س5: ما هي دوال getters و setters؟**

"دوال Getters و setters هي دوال عامة توفر وصولاً متحكماً فيه للحقول الخاصة. دوال Getters تسترجع قيم الحقول وتتبع عادة اصطلاح التسمية getName() لحقل يسمى 'name'، أو isFlag() للحقول المنطقية. دوال Setters تعدل قيم الحقول وتتبع الاصطلاح setName(Type value). هي مهمة لأنها تسمح لك بإضافة التحقق أو التسجيل أو منطق آخر عند الوصول إلى الحقول أو تعديلها، بدلاً من السماح بالوصول المباشر للحقول."

**س6: هل يمكن أن يكون لدينا class بدوال getters فقط بدون setters؟**

"نعم، بالتأكيد. هذا ينشئ class للقراءة فقط أو غير قابل للتغيير. هذا شائع عندما تريد عرض البيانات لكن منع التعديل الخارجي. على سبيل المثال، في class BankAccount، قد توفر getter لرقم الحساب لكن بدون setter لأن أرقام الحسابات لا يجب أن تتغير بعد الإنشاء. للـ classes غير القابلة للتغيير مثل String أو LocalDate، جميع الحقول final ويتم توفير getters فقط، مما يضمن أن حالة الكائن لا تتغير أبداً بعد الإنشاء."

**س7: ما هو النسخ الدفاعي ولماذا هو مهم؟**

"النسخ الدفاعي هو إنشاء نسخة من كائن قابل للتعديل عند تخزينه أو إرجاعه، بدلاً من استخدام المرجع مباشرة. هو مهم لأنه إذا خزنت مرجعاً لكائن خارجي قابل للتعديل، يمكن تعديل هذا الكائن خارجياً، مما يكسر التغليف. وبالمثل، إذا أرجعت مرجعاً لكائن داخلي قابل للتعديل، يمكن للمستدعين تعديله. بإنشاء نسخ في دوال getters و setters، تضمن بقاء الحالة الداخلية محمية. هذا مهم بشكل خاص للكائنات القابلة للتعديل مثل Date والمصفوفات والـ collections."

**س8: ما هي محددات الوصول الأربعة في Java؟**

"Java لديها أربعة محددات وصول، من الأكثر تقييداً إلى الأقل: private - يمكن الوصول إليه فقط داخل نفس الـ class؛ default أو package-private - يمكن الوصول إليه داخل نفس الـ package، يُحدد بعدم استخدام أي محدد؛ protected - يمكن الوصول إليه داخل نفس الـ package ومن الـ subclasses في packages أخرى؛ و public - يمكن الوصول إليه من أي مكان. هذه المحددات تتحكم في التغليف بتحديد أي أجزاء من الـ class مرئية للعالم الخارجي."

**س9: هل من الضروري جعل جميع الحقول private لتحقيق التغليف؟**

"على الرغم من أنه أفضل الممارسات والموصى به للتغليف الصحيح، إلا أنه ليس إلزامياً صارماً في جميع الحالات. يمكن أن تكون الحقول public إذا كان هناك سبب وجيه، رغم أن هذا نادر. الثوابت عادة تكون public static final. الحقول protected قد تُستخدم في تسلسلات الوراثة حيث يُقصد الوصول المتحكم فيه من الـ subclasses. ومع ذلك، للتغليف القياسي وإخفاء البيانات، الحقول private مع getters و setters عامة هو النهج الموصى به. الهدف هو التحكم في الوصول، و private يعطيك أكبر قدر من التحكم."

**س10: كيف يوفر التغليف إخفاء البيانات؟**

"التغليف يوفر إخفاء البيانات بجعل الحقول private، مما يمنع الوصول المباشر من خارج الـ class. كل التفاعل مع البيانات يجب أن يمر عبر الدوال العامة - getters و setters. هذا يعطي الـ class تحكماً كاملاً في كيفية الوصول إلى بياناته وتعديلها. يمكنك إضافة التحقق في setters لرفض البيانات غير الصالحة، وتنفيذ منطق الأعمال في getters، أو حتى تغيير التمثيل الداخلي للبيانات دون التأثير على الكود الخارجي، طالما بقيت الواجهة العامة كما هي. هذا الفصل بين الواجهة والتنفيذ هو جوهر إخفاء البيانات."

---

## 7. الأخطاء الشائعة

1. **جعل الحقول public بدلاً من private**:
   ```java
   public class Person {
       public String name;  // BAD: Direct access, no control
       public int age;
   }
   ```

2. **نسيان التحقق في دوال setters**:
   ```java
   public void setAge(int age) {
       this.age = age;  // BAD: Allows negative or invalid ages
   }
   // GOOD: if (age < 0) throw new IllegalArgumentException();
   ```

3. **إرجاع الكائنات القابلة للتعديل مباشرة**:
   ```java
   public Date getBirthDate() {
       return birthDate;  // BAD: Caller can modify internal state
   }
   // GOOD: return new Date(birthDate.getTime());
   ```

4. **تخزين مراجع للمعاملات القابلة للتعديل**:
   ```java
   public void setItems(List<String> items) {
       this.items = items;  // BAD: External list can be modified
   }
   // GOOD: this.items = new ArrayList<>(items);
   ```

5. **توفير setters لحقول لا يجب أن تتغير**:
   ```java
   private String accountNumber;
   public void setAccountNumber(String num) { ... }  // BAD: Should be read-only
   ```

6. **استخدام اصطلاح getter خاطئ للـ boolean**:
   ```java
   private boolean active;
   public boolean getActive() { ... }  // Should be isActive()
   ```

7. **عدم استخدام كلمة `this` عند الحاجة**:
   ```java
   public void setName(String name) {
       name = name;  // BAD: Assigns parameter to itself, doesn't set field
   }
   // GOOD: this.name = name;
   ```

8. **كسر التغليف بالوصول package-private**:
   ```java
   int salary;  // Default access: accessible from same package
   // Better: private int salary; with getters/setters
   ```

---

## 8. نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- التغليف = إخفاء البيانات من خلال حقول private + دوال عامة
- الفوائد الرئيسية: حماية البيانات، المرونة، التحقق، قابلية الصيانة
- الفرق عن التجريد (مفاهيم مرتبطة لكن مختلفة)
- الأهمية العملية في كل كود الإنتاج
- النسخ الدفاعي للكائنات القابلة للتعديل

**ما يجب تجنبه**:
- لا تخلط بين التغليف والتجريد (خطأ شائع)
- لا تدعي أن التغليف يتعلق فقط بـ getters/setters (هو أوسع)
- لا تقل "التغليف يجعل الكود أبطأ" (تكلفة ضئيلة، فوائد ضخمة)
- لا تنسى ذكر التحقق كفائدة رئيسية

**تحت الضغط**:
- ابدأ بالتعريف البسيط: "إخفاء البيانات وتوفير وصول متحكم فيه"
- أعطِ مثالاً سريعاً: BankAccount مع balance خاص ودوال deposit/withdraw
- اذكر محددات الوصول الأربعة إذا سُئلت عن التنفيذ
- ارسم مخطط class بسيط يوضح الحقول الخاصة والدوال العامة

**علامات حمراء يجب تجنبها**:
- "الحقول العامة مقبولة إذا كنت حذراً" (ينتهك التغليف)
- "Getters و setters مجرد كود زائد" (توفر التحكم والتحقق)
- "التغليف والتجريد نفس الشيء" (مرتبطان لكن مختلفان)
- "لا تحتاج إلى النسخ الدفاعي" (يعتمد على القابلية للتعديل)

**نقاط إضافية**:
- اذكر اصطلاح JavaBeans (مهم للـ frameworks)
- ناقش عدم القابلية للتغيير كالتغليف المثالي
- أشر إلى نمط Builder كبديل للعديد من setters
- اذكر أن التغليف يمكّن الربط الضعيف
- ناقش علاقته بمبدأ Open/Closed (مفتوح للتوسيع، مغلق للتعديل)

</div>
