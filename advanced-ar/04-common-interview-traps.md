<div dir="rtl">

# فخاخ المقابلات الشائعة

## 1️⃣ نظرة عامة على المفهوم

**فخاخ المقابلات** هي أسئلة أو سيناريوهات دقيقة مصممة لاختبار الفهم العميق لمفاهيم Java. هذه الأسئلة غالباً ما تحتوي على جوانب "خادعة" تكشف المرشحين الذين لديهم معرفة سطحية فقط.

**لماذا توجد هذه الفخاخ**:
- اختبار عمق الفهم بعيداً عن الحفظ
- تحديد المرشحين الذين يفهمون Java داخلياً
- كشف الخبرة العملية في البرمجة
- التمييز بين النظرية والممارسة
- كشف المفاهيم الخاطئة الشائعة

**فئات الفخاخ**:
1. **مساواة String و Object**: الخلط بين == و equals()
2. **تخزين Wrapper Class مؤقتاً**: فخاخ autoboxing في Integer
3. **الخلط في Pass-by-Value**: سوء فهم تمرير المراجع
4. **الوراثة وإعادة التعريف**: إخفاء الدالة مقابل إعادة تعريفها
5. **معالجة الاستثناءات**: return في كتل finally
6. **تعديلات المجموعات**: ConcurrentModificationException
7. **Static و Instance**: ترتيب التهيئة، سياق static
8. **عدم القابلية للتغيير**: فخاخ تعديل String
9. **تحويل الأنواع**: سيناريوهات ClassCastException
10. **NullPointerException**: سيناريوهات NPE الشائعة

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **فصل المبتدئين عن ذوي الخبرة**: هذه الفخاخ تكشف الفهم الحقيقي
- **اختبار الخبرة العملية**: المعرفة النظرية ليست كافية
- **أخطاء إنتاجية شائعة**: هذه المشاكل تظهر في الكود الحقيقي
- **الفهم العميق**: يظهر أن المرشح يفكر بعيداً عن القواعد النحوية
- **حل المشاكل**: هل يمكنهم تصحيح السيناريوهات الصعبة؟
- **مهارات مراجعة الكود**: هل سيكتشفون هذه في مراجعات الأقران؟

---

## 3️⃣ القواعد والحقائق الأساسية

**أكثر فخاخ المقابلات شيوعاً**:

| الفخ | ما يختبرونه | الإجابة التحذيرية |
|------|-------------|-------------------|
| String == مقابل equals() | فهم مساواة الكائنات | "هما نفس الشيء" |
| تخزين Integer مؤقتاً | داخليات Wrapper class | عدم معرفة التخزين المؤقت من -128 إلى 127 |
| Pass-by-value | المرجع مقابل القيمة | "Java تمرر بالمرجع للكائنات" |
| عدم قابلية String للتغيير | سلوك String | "str.concat() تعدل النص" |
| finally مع return | معالجة الاستثناءات | "return في try تنفذ دائماً" |
| إعادة تعريف مقابل إخفاء الدالة | تعدد الأشكال | الخلط في سلوك الدوال static |
| ArrayList.remove() | Collections API | عدم معرفة التحميل الزائد int مقابل Integer |
| Switch بدون break | التحكم في التدفق | نسيان سلوك السقوط |
| التهيئة Static | تحميل Class | عدم معرفة ترتيب كتلة static |
| التعديل أثناء for-each | سلوك Iterator | "يمكنك التعديل أثناء التكرار" |

**علامات التحذير في الأسئلة**:
- "ماذا سيطبع هذا الكود؟" (عادة يحتوي على فخ)
- "هل سيتم ترجمة هذا؟" (اختبار الحالات الحدية)
- "ما الخطأ في هذا الكود؟" (مشاكل متعددة)
- "أصلح هذا الخطأ" (مشكلة دقيقة)
- "لماذا لا يعمل هذا كما هو متوقع؟" (مفهوم خاطئ)

---

## 4️⃣ أمثلة الكود (Java 8)

### المثال 1: فخ String == مقابل equals()

```java
public class StringEqualityTrap {
    public static void main(String[] args) {
        // Trap 1: String literals
        String s1 = "Hello";
        String s2 = "Hello";
        System.out.println(s1 == s2);  // true - same object from String Pool!

        // Trap 2: String with new
        String s3 = new String("Hello");
        String s4 = new String("Hello");
        System.out.println(s3 == s4);  // false - different objects
        System.out.println(s3.equals(s4));  // true - same content

        // Trap 3: Mixed scenarios
        System.out.println(s1 == s3);  // false - different objects
        System.out.println(s1.equals(s3));  // true - same content

        // Trap 4: String concatenation
        String s5 = "Hel" + "lo";  // Compile-time constant
        System.out.println(s1 == s5);  // true - same pool object!

        String s6 = "Hel";
        String s7 = s6 + "lo";  // Runtime concatenation
        System.out.println(s1 == s7);  // false - new object created

        // Trap 5: intern()
        String s8 = s3.intern();  // Add to pool
        System.out.println(s1 == s8);  // true - both from pool now
    }
}

/*
INTERVIEW TRAP:
Q: "What does this print?"
Many candidates say all true or all false.

CORRECT ANSWER:
- String literals share pool reference (==  true)
- new String() creates new object (== false)
- equals() compares content (always true for same text)
- intern() returns pool reference
*/
```

### المثال 2: فخ تخزين Integer مؤقتاً

```java
public class IntegerCachingTrap {
    public static void main(String[] args) {
        // Trap 1: Small integers (-128 to 127)
        Integer i1 = 100;
        Integer i2 = 100;
        System.out.println(i1 == i2);  // true - cached!

        // Trap 2: Large integers (outside cache range)
        Integer i3 = 200;
        Integer i4 = 200;
        System.out.println(i3 == i4);  // false - different objects!

        // Trap 3: new Integer() bypasses cache
        Integer i5 = new Integer(100);
        Integer i6 = new Integer(100);
        System.out.println(i5 == i6);  // false - new objects

        // Trap 4: Proper comparison
        System.out.println(i3.equals(i4));  // true - use equals()!

        // Trap 5: Unboxing comparison
        int primitive = 200;
        System.out.println(i3 == primitive);  // true - i3 unboxed!

        demonstrateAllWrappers();
    }

    static void demonstrateAllWrappers() {
        // All wrapper classes cache some values

        // Byte: -128 to 127 (all values)
        Byte b1 = 10;
        Byte b2 = 10;
        System.out.println("Byte: " + (b1 == b2));  // true

        // Short: -128 to 127
        Short sh1 = 100;
        Short sh2 = 100;
        System.out.println("Short: " + (sh1 == sh2));  // true

        // Long: -128 to 127
        Long l1 = 100L;
        Long l2 = 100L;
        System.out.println("Long: " + (l1 == l2));  // true

        // Character: 0 to 127
        Character c1 = 'A';
        Character c2 = 'A';
        System.out.println("Character: " + (c1 == c2));  // true

        // Boolean: true and false (both cached)
        Boolean bool1 = true;
        Boolean bool2 = true;
        System.out.println("Boolean: " + (bool1 == bool2));  // true
    }
}

/*
INTERVIEW TRAP:
Q: "Will these comparisons return true?"

Integer i1 = 100;
Integer i2 = 100;
System.out.println(i1 == i2);  // true

Integer i3 = 200;
Integer i4 = 200;
System.out.println(i3 == i4);  // false !!!

REASON: Integer caches -128 to 127 for performance
Outside this range, new objects created

RULE: Always use equals() for wrapper objects!
*/
```

### المثال 3: فخ Pass-by-Value

```java
class Person {
    String name;

    Person(String name) {
        this.name = name;
    }
}

public class PassByValueTrap {
    public static void main(String[] args) {
        // Trap 1: Primitive pass-by-value
        int x = 10;
        modifyPrimitive(x);
        System.out.println("After modify: " + x);  // 10 - unchanged!

        // Trap 2: Object reference pass-by-value
        Person p = new Person("Alice");
        modifyObject(p);
        System.out.println("After modify: " + p.name);  // Bob - changed!

        // Trap 3: Reference reassignment
        reassignReference(p);
        System.out.println("After reassign: " + p.name);  // Bob - unchanged!

        // Trap 4: String immutability confusion
        String str = "Hello";
        modifyString(str);
        System.out.println("After modify: " + str);  // Hello - unchanged!
    }

    static void modifyPrimitive(int value) {
        value = 100;  // Modifies local copy only
    }

    static void modifyObject(Person person) {
        person.name = "Bob";  // Modifies object on heap
        // Reference is passed by value, but points to same object
    }

    static void reassignReference(Person person) {
        person = new Person("Charlie");  // Only local reference changed
        // Original reference in main() still points to old object
    }

    static void modifyString(String s) {
        s = s.concat(" World");  // Creates new String
        // Original string unchanged due to immutability
    }
}

/*
INTERVIEW TRAP:
Q: "Is Java pass-by-value or pass-by-reference?"

WRONG ANSWER: "Pass-by-reference for objects"
CORRECT ANSWER: "Always pass-by-value. For objects,
the reference is passed by value (copy of reference)"

KEY POINTS:
1. Primitive: value copied
2. Object: reference copied (both point to same object)
3. Cannot change what original reference points to
4. Can change object state if mutable
*/
```

### المثال 4: فخ إعادة تعريف مقابل إخفاء الدالة

```java
class Parent {
    static void staticMethod() {
        System.out.println("Parent static");
    }

    void instanceMethod() {
        System.out.println("Parent instance");
    }
}

class Child extends Parent {
    static void staticMethod() {  // Method HIDING, not overriding
        System.out.println("Child static");
    }

    @Override
    void instanceMethod() {  // Method OVERRIDING
        System.out.println("Child instance");
    }
}

public class OverridingVsHidingTrap {
    public static void main(String[] args) {
        Parent p1 = new Parent();
        Parent p2 = new Child();  // Polymorphism
        Child c = new Child();

        // Instance method - runtime polymorphism works
        p1.instanceMethod();  // Parent instance
        p2.instanceMethod();  // Child instance - overridden!
        c.instanceMethod();   // Child instance

        // Static method - NO polymorphism!
        p1.staticMethod();  // Parent static
        p2.staticMethod();  // Parent static - NOT Child!
        c.staticMethod();   // Child static
    }
}

/*
INTERVIEW TRAP:
Q: "What will p2.staticMethod() print?"

Many think: "Child static" (wrong!)
Correct: "Parent static"

REASON:
- Static methods are bound at compile-time
- Reference type determines which method called
- No runtime polymorphism for static methods
- This is method HIDING, not overriding

RULE:
- Instance methods: Runtime polymorphism (overriding)
- Static methods: Compile-time binding (hiding)
*/
```

### المثال 5: فخ return في كتلة finally

```java
public class FinallyReturnTrap {
    public static void main(String[] args) {
        System.out.println("Result 1: " + test1());  // 20 - finally wins!
        System.out.println("Result 2: " + test2());  // 20 - finally wins!
        System.out.println("Result 3: " + test3());  // 30 - finally changes reference!
    }

    // Trap 1: return in finally overrides try
    static int test1() {
        try {
            return 10;
        } finally {
            return 20;  // This overrides try's return!
        }
    }

    // Trap 2: return in finally overrides catch
    static int test2() {
        try {
            throw new Exception();
        } catch (Exception e) {
            return 10;
        } finally {
            return 20;  // This overrides catch's return!
        }
    }

    // Trap 3: modifying object in finally
    static int test3() {
        Container c = new Container(10);
        try {
            c.value = 20;
            return c.value;
        } finally {
            c.value = 30;  // Modifies object before return
            // Return value is 30, not 20!
        }
    }

    static class Container {
        int value;
        Container(int value) { this.value = value; }
    }
}

/*
INTERVIEW TRAP:
Q: "What does test1() return?"

Many think: 10 (try block return)
Correct: 20 (finally return overrides)

CRITICAL RULES:
1. finally ALWAYS executes (except System.exit())
2. return in finally overrides try/catch return
3. finally can modify return value for objects
4. DON'T put return in finally (bad practice!)

BEST PRACTICE:
Never use return statement in finally block!
*/
```

### المثال 6: فخ ArrayList.remove()

```java
import java.util.*;

public class ArrayListRemoveTrap {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        list.add(5);

        // Trap 1: remove by index vs value
        System.out.println("Original: " + list);  // [1, 2, 3, 4, 5]

        list.remove(2);  // Removes index 2 (value 3)
        System.out.println("After remove(2): " + list);  // [1, 2, 4, 5]

        // Trap 2: How to remove Integer value 2?
        list.add(1, 3);  // [1, 3, 2, 4, 5]

        // WRONG: removes index 2
        // list.remove(2);  // Removes index 2

        // CORRECT: remove by value
        list.remove(Integer.valueOf(2));  // Removes value 2
        System.out.println("After removing value 2: " + list);  // [1, 3, 4, 5]

        // Trap 3: ConcurrentModificationException
        demonstrateCME();
    }

    static void demonstrateCME() {
        List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

        // WRONG: Concurrent modification
        try {
            for (Integer num : list) {
                if (num == 3) {
                    list.remove(num);  // ConcurrentModificationException!
                }
            }
        } catch (ConcurrentModificationException e) {
            System.out.println("ConcurrentModificationException caught!");
        }

        // CORRECT: Use Iterator
        list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
        Iterator<Integer> it = list.iterator();
        while (it.hasNext()) {
            Integer num = it.next();
            if (num == 3) {
                it.remove();  // Safe removal
            }
        }
        System.out.println("Correct removal: " + list);  // [1, 2, 4, 5]

        // CORRECT: removeIf (Java 8)
        list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
        list.removeIf(num -> num == 3);
        System.out.println("Using removeIf: " + list);  // [1, 2, 4, 5]
    }
}

/*
INTERVIEW TRAP:
Q: "How to remove Integer value 2 from list?"

WRONG: list.remove(2) - removes index 2!
CORRECT: list.remove(Integer.valueOf(2))

OVERLOADED METHODS:
- remove(int index) - removes by index
- remove(Object o) - removes by value

For Integer list:
- list.remove(2) calls remove(int) - removes index!
- list.remove(Integer.valueOf(2)) - removes value 2

CONCURRENT MODIFICATION:
- Can't modify list during for-each
- Use Iterator.remove() or removeIf()
*/
```

### المثال 7: فخ السقوط في Switch

```java
public class SwitchFallThroughTrap {
    public static void main(String[] args) {
        // Trap 1: Missing break
        printDayType1(3);  // Prints both weekday and weekend!

        // Trap 2: Intentional fall-through
        printDayType2(3);  // Correct with break

        // Trap 3: Multiple cases
        printSeason(4);
    }

    // TRAP: Missing break causes fall-through
    static void printDayType1(int day) {
        System.out.println("\n=== Missing Break ===");
        switch (day) {
            case 1:
            case 2:
            case 3:
            case 4:
            case 5:
                System.out.println("Weekday");
                // Missing break!
            case 6:
            case 7:
                System.out.println("Weekend");  // This also prints!
        }
    }

    // CORRECT: With break
    static void printDayType2(int day) {
        System.out.println("\n=== Correct Version ===");
        switch (day) {
            case 1:
            case 2:
            case 3:
            case 4:
            case 5:
                System.out.println("Weekday");
                break;  // Prevent fall-through
            case 6:
            case 7:
                System.out.println("Weekend");
                break;
            default:
                System.out.println("Invalid day");
        }
    }

    // Intentional fall-through with comment
    static void printSeason(int month) {
        System.out.println("\n=== Season ===");
        switch (month) {
            case 12:
            case 1:
            case 2:
                System.out.println("Winter");
                break;
            case 3:
            case 4:
            case 5:
                System.out.println("Spring");
                break;
            case 6:
            case 7:
            case 8:
                System.out.println("Summer");
                break;
            case 9:
            case 10:
            case 11:
                System.out.println("Fall");
                break;
        }
    }
}

/*
INTERVIEW TRAP:
Q: "What does printDayType1(3) print?"

Many forget: switch has fall-through behavior!

OUTPUT:
Weekday
Weekend

REASON: No break statement after "Weekday"
Execution continues to next case!

RULES:
1. break exits switch
2. Without break, execution falls through
3. default should be last (best practice)
4. Add break even after last case (safety)

MODERN JAVA:
Java 12+ has switch expressions with no fall-through
*/
```

### المثال 8: فخ ترتيب التهيئة Static

```java
public class StaticInitializationTrap {
    // Trap 1: Order matters!
    static int a = 10;
    static int b = a * 2;  // b = 20

    static {
        System.out.println("Static block 1: a=" + a + ", b=" + b);
        a = 30;
    }

    static int c = a + 10;  // c = 40 (after static block 1)

    static {
        System.out.println("Static block 2: a=" + a + ", c=" + c);
    }

    public static void main(String[] args) {
        System.out.println("Main: a=" + a + ", b=" + b + ", c=" + c);
    }
}

class Parent2 {
    static {
        System.out.println("Parent static block");
    }

    {
        System.out.println("Parent instance block");
    }

    Parent2() {
        System.out.println("Parent constructor");
    }
}

class Child2 extends Parent2 {
    static {
        System.out.println("Child static block");
    }

    {
        System.out.println("Child instance block");
    }

    Child2() {
        System.out.println("Child constructor");
    }

    public static void main(String[] args) {
        System.out.println("=== Creating Child2 ===");
        new Child2();
    }
}

/*
INTERVIEW TRAP:
Q: "In what order do these execute?"

INITIALIZATION ORDER:
1. Static variables and blocks (top to bottom)
2. Instance variables and blocks (top to bottom)
3. Constructor

INHERITANCE:
1. Parent static blocks
2. Child static blocks
3. Parent instance blocks
4. Parent constructor
5. Child instance blocks
6. Child constructor

OUTPUT for Child2:
Parent static block
Child static block
=== Creating Child2 ===
Parent instance block
Parent constructor
Child instance block
Child constructor
*/
```

### المثال 9: فخ عدم قابلية String للتغيير

```java
public class StringImmutabilityTrap {
    public static void main(String[] args) {
        // Trap 1: String methods don't modify original
        String s1 = "hello";
        s1.toUpperCase();  // Creates new String, doesn't modify s1
        System.out.println(s1);  // hello - unchanged!

        String s2 = s1.toUpperCase();  // Must assign to new variable
        System.out.println(s2);  // HELLO

        // Trap 2: concat() doesn't modify
        String s3 = "Hello";
        s3.concat(" World");  // Creates new String
        System.out.println(s3);  // Hello - unchanged!

        s3 = s3.concat(" World");  // Must reassign
        System.out.println(s3);  // Hello World

        // Trap 3: replace() doesn't modify
        String s4 = "Java";
        s4.replace('a', 'o');
        System.out.println(s4);  // Java - unchanged!

        // Trap 4: StringBuilder is mutable
        StringBuilder sb = new StringBuilder("Hello");
        sb.append(" World");  // Modifies sb
        System.out.println(sb);  // Hello World - modified!

        // Trap 5: String in method
        String s5 = "Original";
        modifyString(s5);
        System.out.println(s5);  // Original - unchanged!
    }

    static void modifyString(String s) {
        s = s + " Modified";  // Creates new String
        // Original reference unchanged
    }
}

/*
INTERVIEW TRAP:
Q: "What does this print?"

String s = "hello";
s.toUpperCase();
System.out.println(s);

Many think: "HELLO"
Correct: "hello"

REASON: Strings are IMMUTABLE
- All String methods return NEW strings
- Original string never modified
- Must assign result to variable

COMMON MISTAKES:
str.concat(" World");      // Wrong - result not captured
str.toUpperCase();         // Wrong - result not captured
str.replace('a', 'b');     // Wrong - result not captured

CORRECT:
str = str.concat(" World");
str = str.toUpperCase();
str = str.replace('a', 'b');
*/
```

### المثال 10: فخاخ NullPointerException

```java
public class NPETrap {
    public static void main(String[] args) {
        // Trap 1: Unboxing null
        Integer i = null;
        try {
            int x = i;  // Unboxing null throws NPE!
        } catch (NullPointerException e) {
            System.out.println("Trap 1: Unboxing null");
        }

        // Trap 2: Auto-unboxing in operations
        Integer a = null;
        Integer b = 10;
        try {
            int sum = a + b;  // NPE on unboxing a!
        } catch (NullPointerException e) {
            System.out.println("Trap 2: Auto-unboxing in operation");
        }

        // Trap 3: String concatenation with null
        String s = null;
        String result = "Value: " + s;  // No NPE! Prints "Value: null"
        System.out.println(result);

        // Trap 4: Calling method on null
        try {
            s.length();  // NPE!
        } catch (NullPointerException e) {
            System.out.println("Trap 4: Method call on null");
        }

        // Trap 5: Array of objects
        String[] array = new String[5];  // Array exists, elements are null
        try {
            System.out.println(array[0].length());  // NPE!
        } catch (NullPointerException e) {
            System.out.println("Trap 5: Array element is null");
        }

        // Trap 6: Chaining with null
        Person p = null;
        try {
            String name = p.getName().toUpperCase();  // NPE!
        } catch (NullPointerException e) {
            System.out.println("Trap 6: Method chaining with null");
        }

        // Trap 7: Collections with null
        java.util.List<String> list = null;
        try {
            list.add("value");  // NPE!
        } catch (NullPointerException e) {
            System.out.println("Trap 7: Calling method on null collection");
        }
    }

    static class Person {
        String getName() { return "Alice"; }
    }
}

/*
INTERVIEW TRAP:
Q: "Which line throws NPE?"

1. Integer i = null;
2. int x = i;        // This one! Unboxing null

3. String s = null;
4. String r = "Hi " + s;  // No NPE! Prints "Hi null"

COMMON NPE SCENARIOS:
1. Unboxing null wrapper → primitive
2. Method call on null reference
3. Array access on null array reference
4. Accessing null array element
5. Auto-unboxing in arithmetic

NULL-SAFE APPROACHES:
1. Use Optional<T>
2. Null checks before use
3. Objects.requireNonNull()
4. @NonNull annotations
*/
```

---

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما الفرق بين == و equals() للنصوص؟**
2. **لماذا Integer i1 = 100; i2 = 100; i1 == i2 ترجع true، لكن مع 200 ترجع false؟**
3. **هل Java تمرر بالقيمة أم بالمرجع؟**
4. **ماذا يحدث إذا أعدت تعريف دالة static؟**
5. **ماذا تفعل عبارة return في كتلة finally؟**
6. **كيف تزيل كائن Integer بقيمة 2 من ArrayList<Integer>؟**
7. **ما هو سلوك السقوط في switch؟**
8. **بأي ترتيب تنفذ كتل static والـ constructors؟**
9. **لماذا لا يغير str.toUpperCase() النص؟**
10. **متى يحدث NullPointerException مع autoboxing؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما الفرق بين == و equals() للنصوص؟**

"للنصوص، == تقارن المراجع - هل كلا المتغيرين يشيران لنفس الكائن في الذاكرة. equals() تقارن المحتوى - هل النصوص لها نفس الأحرف. بسبب String Pool، النصوص الحرفية بنفس القيمة تشترك في نفس الكائن، لذا == ترجع true. لكن النصوص المنشأة بـ 'new' هي كائنات مختلفة، لذا == ترجع false حتى مع نفس المحتوى. استخدم دائماً equals() لمقارنة محتوى النص. على سبيل المثال، 'String s1 = \"Hi\"; String s2 = \"Hi\";' s1 == s2 تكون true لأن كلاهما يشير للـ pool. لكن 'String s3 = new String(\"Hi\");' s1 == s3 تكون false رغم أن s1.equals(s3) تكون true."

**س2: لماذا Integer i1 = 100; i2 = 100; i1 == i2 ترجع true، لكن مع 200 ترجع false؟**

"Java تخزن كائنات Integer للقيم من -128 إلى 127 لتحسين الأداء. عندما تعين Integer i = 100، Java ترجع الكائن المخزن. لذا كلا i1 و i2 يشيران لنفس الكائن المخزن، مما يجعل == ترجع true. للقيمة 200، التي خارج نطاق التخزين المؤقت، يتم إنشاء كائنات Integer جديدة. لذا i3 و i4 هما كائنان مختلفان، مما يجعل == ترجع false. هذا ينطبق على جميع wrapper classes بنطاقات تخزين مشابهة. الدرس هو: استخدم دائماً equals() لمقارنة كائنات wrapper، وليس ==. التخزين المؤقت هو تفصيل تحسين لا يجب أن يؤثر على منطق المقارنة."

**س3: هل Java تمرر بالقيمة أم بالمرجع؟**

"Java دائماً تمرر بالقيمة، بشكل صارم. للأنواع البدائية، القيمة نفسها تُنسخ. للكائنات، المرجع يُنسخ - وليس الكائن. هذا يعني أن كلاً من المرجع الأصلي والمعامل يشيران لنفس الكائن، لذا يمكنك تعديل حالة الكائن، لكن لا يمكنك تغيير ما يشير إليه المرجع الأصلي. إذا أعدت تعيين المعامل لكائن جديد داخل الدالة، فإنه يؤثر فقط على النسخة المحلية. الخلط ينشأ لأنك تستطيع تعديل حالة الكائن، مما يبدو كتمرير بالمرجع، لكنك في الواقع تعدل عبر مرجع منسوخ. لهذا السبب عمل 'person = new Person()' في دالة لا يؤثر على المرجع الأصلي."

**س4: ماذا يحدث إذا أعدت تعريف دالة static؟**

"لا يمكنك إعادة تعريف الدوال static - يمكنك فقط إخفاؤها. إعادة تعريف الدالة تتطلب تعدد الأشكال وقت التشغيل، لكن الدوال static ترتبط وقت الترجمة بناءً على نوع المرجع، وليس نوع الكائن. إذا عرّفت دالة static بنفس التوقيع في subclass، فهو إخفاء دالة وليس إعادة تعريف. عندما تستدعي parent.staticMethod() حيث parent يشير لكائن Child، نسخة Parent تنفذ لأن نوع المرجع هو Parent. هذا مختلف جوهرياً عن دوال instance حيث ستنفذ نسخة Child. الدوال static تنتمي للـ class وليس للـ instances، لذا تعدد الأشكال لا ينطبق. @Override annotation لن تترجم حتى للدوال static."

**س5: ماذا تفعل عبارة return في كتلة finally؟**

"عبارة return في كتلة finally تتجاوز أي return من كتل try أو catch. هذا لأن finally تنفذ بعد try/catch لكن قبل أن ترجع الدالة فعلياً. إذا كان لـ finally عبارة return، تصبح هي قيمة إرجاع الدالة، متجاهلة ما كانت try أو catch ستُرجعه. هذا خطير ويعتبر ممارسة سيئة لأنه يمكن أن يخفي الاستثناءات - إذا رمت try استثناء وأرجعت finally قيمة، الاستثناء يُفقد. الدالة ترجع بشكل طبيعي كأن لا استثناء حدث. بالمثل، إذا أرجعت try قيمة وأرجعت finally قيمة مختلفة، return الخاص بـ try يُتجاهل. أفضل ممارسة: لا تضع أبداً عبارات return في كتل finally."

**س6: كيف تزيل كائن Integer بقيمة 2 من ArrayList<Integer>؟**

"ArrayList لديها دالتان remove محملتان زائداً: remove(int index) و remove(Object o). عندما تستدعي list.remove(2) على ArrayList<Integer>، تستدعي remove(int index) وتزيل العنصر في الفهرس 2، وليس Integer بقيمة 2. لإزالة قيمة Integer 2، يجب أن تستدعي list.remove(Integer.valueOf(2)) التي تستدعي نسخة remove(Object). هذا فخ شائع لأن معامل int له الأولوية على autoboxing لـ Integer. بدلاً من ذلك، يمكنك استخدام list.removeIf(n -> n == 2) في Java 8+. هذا الفخ يظهر أهمية فهم التحميل الزائد للدوال وسلوك autoboxing."

**س7: ما هو سلوك السقوط في switch؟**

"في عبارة switch، إذا لم تضمّن عبارة break، التنفيذ يسقط للحالة التالية. بعد مطابقة case وتنفيذ كودها، إذا لم يكن هناك break، التنفيذ يستمر لكود الحالة التالية بغض النظر عما إذا كانت تتطابق. هذا غالباً غير مقصود ويسبب أخطاء. على سبيل المثال، إذا كان day هو 3 (الأربعاء) ولديك حالات 1-5 تطبع 'Weekday' بدون break، متبوعة بحالات 6-7 تطبع 'Weekend'، سيطبع كلاً من 'Weekday' و 'Weekend'. هذا السلوك مقصود في تصميم Java للحالات التي تريد فيها عدة labels تنفذ نفس الكود، لكن يجب أن تكون حذراً. ضمّن دائماً break إلا إذا كنت تريد السقوط عمداً، وفي هذه الحالة أضف تعليقاً يشرح ذلك."

**س8: بأي ترتيب تنفذ كتل static والـ constructors؟**

"الترتيب هو: كتل parent static، كتل child static، كتل parent instance، constructor الـ parent، كتل child instance، constructor الـ child. كتل static تنفذ عند تحميل الـ class لأول مرة، قبل إنشاء أي instances. داخل class، المتغيرات والكتل static تنفذ من الأعلى للأسفل. تهيئة instance تحدث عندما تنشئ كائن: أولاً كتل instance الـ parent والـ constructor، ثم كتل instance الـ child والـ constructor. هذا يتبع مبدأ أن الـ parent يجب أن يُهيأ بالكامل قبل الـ child. التهيئة static تحدث مرة واحدة فقط عند تحميل الـ class، لكن تهيئة instance تحدث كل مرة تنشئ فيها كائن. فهم هذا الترتيب حاسم لتصحيح المشاكل المتعلقة بالتهيئة."

**س9: لماذا لا يغير str.toUpperCase() النص؟**

"لأن الـ Strings غير قابلة للتغيير في Java. بمجرد إنشائها، محتوى String لا يمكن تغييره. الدوال مثل toUpperCase() و concat() و replace() لا تعدل النص الأصلي - تنشئ وترجع كائن String جديد. إذا استدعيت str.toUpperCase() بدون تعيين النتيجة، النص الجديد يُنشأ لكنه يصبح فوراً مؤهلاً لجمع القمامة بينما str يبقى بدون تغيير. يجب أن تعين النتيجة: str = str.toUpperCase(). عدم القابلية للتغيير هذه توفر أمان الخيوط وتسمح بتحسين String Pool. إذا كنت تحتاج نصوص قابلة للتغيير، استخدم StringBuilder أو StringBuffer. هذا فخ شائع جداً للمبتدئين الذين يتوقعون أن دوال النص تعدل الأصلي."

**س10: متى يحدث NullPointerException مع autoboxing؟**

"NullPointerException يحدث عند فك تغليف كائن wrapper فارغ إلى نوع بدائي. على سبيل المثال، إذا كان لديك Integer i = null وعملت int x = i، Java تحاول فك تغليف i باستدعاء i.intValue()، مما يرمي NPE لأن i فارغ. هذا يحدث أيضاً في العمليات الحسابية: إذا عملت Integer a = null; int sum = a + 5، Java تفك تغليف a مسببة NPE. الأمر خادع بشكل خاص لأن NPE يحدث عند خطوة فك التغليف، وليس حيث عينت null. دمج النصوص استثناء - \"Value: \" + null لا ترمي NPE، تطبع \"Value: null\". لمنع هذه NPEs، تحقق من null قبل فك التغليف، استخدم Optional، أو تجنب فك تغليف wrappers فارغة. هذا الفخ يصطاد كثير من المطورين الذين ينسون أن autoboxing/unboxing يمكن أن يخفي هذه المشاكل."

---

## 7️⃣ الأخطاء الشائعة

1. **استخدام == لمقارنة String/wrapper**:
   ```java
   String s1 = new String("Hi");
   String s2 = new String("Hi");
   if (s1 == s2) { }  // BAD - compares references!

   // GOOD:
   if (s1.equals(s2)) { }
   ```

2. **افتراض pass-by-reference للكائنات**:
   ```java
   void swap(Integer a, Integer b) {
       Integer temp = a;
       a = b;  // Only affects local copy!
       b = temp;
   }
   ```

3. **نسيان break في switch**:
   ```java
   switch (x) {
       case 1:
           System.out.println("One");
           // Missing break - falls through!
       case 2:
           System.out.println("Two");
   }
   ```

4. **عدم تعيين نتيجة دالة String**:
   ```java
   String s = "hello";
   s.toUpperCase();  // BAD - result not captured!

   // GOOD:
   s = s.toUpperCase();
   ```

5. **تعديل القائمة أثناء for-each**:
   ```java
   for (Integer i : list) {
       list.remove(i);  // ConcurrentModificationException!
   }

   // GOOD:
   list.removeIf(i -> condition);
   ```

6. **الخلط بين تحميلات ArrayList.remove() الزائدة**:
   ```java
   list.remove(2);  // Removes index 2, not value 2!

   // GOOD:
   list.remove(Integer.valueOf(2));
   ```

7. **return في كتلة finally**:
   ```java
   try {
       return 10;
   } finally {
       return 20;  // BAD - overrides try's return!
   }
   ```

8. **فك تغليف wrapper فارغ**:
   ```java
   Integer i = null;
   int x = i;  // NullPointerException!
   ```

---

## 8️⃣ نصائح المقابلات الحقيقية

**ما يجب التأكيد عليه**:
- == تقارن المراجع، equals() تقارن المحتوى
- تخزين Integer مؤقتاً: -128 إلى 127
- Java دائماً تمرر بالقيمة
- الدوال static لا يمكن إعادة تعريفها (فقط إخفاؤها)
- الـ Strings غير قابلة للتغيير
- استخدم دائماً equals() لمقارنة wrapper/String

**ما يجب تجنبه**:
- لا تقل "Java تمرر بالمرجع للكائنات"
- لا تنسَ نطاق تخزين Integer مؤقتاً
- لا تخلط بين إعادة تعريف وإخفاء الدالة
- لا تظن أن دوال String تعدل الأصلي

**تحت الضغط**:
- تذكر: "Java دائماً تمرر بالقيمة"
- اذكر: "استخدم equals() للكائنات، == للأنواع البدائية"
- أشر إلى: "Integer تخزن مؤقتاً -128 إلى 127"
- ارجع إلى: "الـ Strings غير قابلة للتغيير - الدوال ترجع String جديد"

**علامات التحذير لتجنبها**:
- "== تعمل جيداً لمقارنة String" (استخدم equals()!)
- "الدوال static يمكن إعادة تعريفها" (فقط إخفاؤها)
- "str.toUpperCase() تغير str" (ترجع String جديد)
- "الكائنات تُمرر بالمرجع" (المراجع تُمرر بالقيمة)

**نقاط إضافية**:
- اذكر لماذا عدم القابلية للتغيير مهم (أمان الخيوط، String Pool)
- ناقش تحسين أداء تخزين wrapper class مؤقتاً
- ارجع إلى علاقة happens-before لرؤية الذاكرة
- اذكر البدائل الحديثة (Optional لأمان null)
- ناقش switch expressions في Java 12+ (بدون سقوط)
- ارجع إلى أدوات التحليل الثابت التي تكتشف هذه المشاكل
- اذكر لماذا توجد هذه الفخاخ في تصميم Java

</div>
