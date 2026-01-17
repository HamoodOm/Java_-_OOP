<div dir="rtl">

# أنواع البيانات والمتغيرات

## 1️⃣ نظرة عامة على المفهوم

**أنواع البيانات** في Java تنقسم إلى فئتين:

1. **الأنواع البدائية (Primitive Types)** (8 أنواع): تخزن القيم البسيطة مباشرة في الذاكرة
   - الرقمية: `byte`، `short`، `int`، `long`، `float`، `double`
   - الحرفية: `char`
   - المنطقية: `boolean`

2. **الأنواع المرجعية (Reference Types)**: تخزن مراجع (عناوين ذاكرة) للكائنات
   - Classes، Interfaces، Arrays، Enums

**المتغيرات** هي مواقع ذاكرة مسماة تخزن القيم. لدى Java ثلاثة أنواع:
- **المتغيرات المحلية**: تُعلن داخل الدوال/الكتل
- **متغيرات النسخة (Instance variables)**: تُعلن داخل الفئة لكن خارج الدوال (غير static)
- **المتغيرات الثابتة (Static variables)**: تُعلن بكلمة `static` (على مستوى الفئة)

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **فهم إدارة الذاكرة**: يُظهر إذا كنت تعرف كيف تخزن Java البيانات
- **الوعي بأمان الأنواع**: Java صارمة في الأنواع—فهم الأنواع أساسي
- **تداعيات الأداء**: الأنواع البدائية مقابل الكائنات تؤثر على الأداء
- **منع الأخطاء الشائعة**: أخطاء تحويل الأنواع شائعة في بيئة الإنتاج
- **أساس البرمجة الكائنية**: الأنواع المرجعية ضرورية للبرمجة كائنية التوجه

---

## 3️⃣ القواعد والحقائق الأساسية

**الأنواع البدائية:**
- لها دائماً قيم افتراضية (على عكس المتغيرات المحلية)
- تُخزن مباشرة في ذاكرة الـ stack (للمتغيرات المحلية)
- أسرع من الأنواع المرجعية
- لا يمكن أن تكون null
- لها فئات غلاف (wrapper classes) مقابلة

**جدول الحجم والنطاق:**

| النوع   | الحجم   | النطاق                                      | الافتراضي |
|---------|---------|---------------------------------------------|-----------|
| byte    | 8-bit   | -128 إلى 127                                | 0         |
| short   | 16-bit  | -32,768 إلى 32,767                          | 0         |
| int     | 32-bit  | -2³¹ إلى 2³¹-1 (~2 مليار)                  | 0         |
| long    | 64-bit  | -2⁶³ إلى 2⁶³-1                              | 0L        |
| float   | 32-bit  | ~±3.4E+38 (6-7 أرقام عشرية)                | 0.0f      |
| double  | 64-bit  | ~±1.7E+308 (15 رقم عشري)                   | 0.0d      |
| char    | 16-bit  | 0 إلى 65,535 (Unicode)                     | '\u0000'  |
| boolean | 1-bit   | true أو false                               | false     |

**قواعد المتغيرات:**
- المتغيرات المحلية يجب تهيئتها قبل الاستخدام (لا قيمة افتراضية)
- متغيرات النسخة تُهيأ تلقائياً للقيم الافتراضية
- المتغيرات الثابتة تُهيأ مرة واحدة عند تحميل الفئة
- المتغيرات لا يمكن أن تبدأ بأرقام
- أسماء المتغيرات حساسة لحالة الأحرف

**فئات الغلاف (Wrapper Classes):**
- كل نوع بدائي له فئة غلاف: `Integer`، `Long`، `Double`، `Boolean`، إلخ
- تمكّن الأنواع البدائية من الاستخدام في المجموعات
- تدعم autoboxing/unboxing (التحويل التلقائي)

---

## 4️⃣ أمثلة برمجية (Java 8)

### المثال 1: الأنواع البدائية والقيم الافتراضية

```java
public class DataTypesDemo {
    // متغيرات النسخة - لها قيم افتراضية
    byte byteVar;           // 0
    short shortVar;         // 0
    int intVar;             // 0
    long longVar;           // 0L
    float floatVar;         // 0.0f
    double doubleVar;       // 0.0d
    char charVar;           // '\u0000'
    boolean boolVar;        // false

    public void printDefaults() {
        System.out.println("byte: " + byteVar);        // 0
        System.out.println("short: " + shortVar);      // 0
        System.out.println("int: " + intVar);          // 0
        System.out.println("long: " + longVar);        // 0
        System.out.println("float: " + floatVar);      // 0.0
        System.out.println("double: " + doubleVar);    // 0.0
        System.out.println("char: [" + charVar + "]"); // []
        System.out.println("boolean: " + boolVar);     // false
    }
}
```

### المثال 2: المتغيرات المحلية مقابل متغيرات النسخة مقابل المتغيرات الثابتة

```java
public class VariableTypes {
    // متغير نسخة (مختلف لكل كائن)
    int instanceVar = 10;

    // متغير ثابت (مشترك بين جميع الكائنات)
    static int staticVar = 20;

    public void demonstrateVariables() {
        // متغير محلي (يجب تهيئته)
        int localVar = 30;

        System.out.println("Local: " + localVar);        // 30
        System.out.println("Instance: " + instanceVar);  // 10
        System.out.println("Static: " + staticVar);      // 20
    }

    public static void main(String[] args) {
        VariableTypes obj1 = new VariableTypes();
        VariableTypes obj2 = new VariableTypes();

        obj1.instanceVar = 100;
        obj2.instanceVar = 200;

        obj1.staticVar = 300;  // يتغير لجميع الكائنات

        System.out.println("obj1.instance: " + obj1.instanceVar);  // 100
        System.out.println("obj2.instance: " + obj2.instanceVar);  // 200
        System.out.println("obj1.static: " + obj1.staticVar);      // 300
        System.out.println("obj2.static: " + obj2.staticVar);      // 300
    }
}
```

### المثال 3: تحويل الأنواع (Type Casting)

```java
public class TypeCasting {
    public static void main(String[] args) {
        // التحويل الضمني (التوسيع) - تلقائي
        int intValue = 100;
        long longValue = intValue;      // int → long (آمن)
        double doubleValue = intValue;   // int → double (آمن)

        System.out.println(longValue);    // 100
        System.out.println(doubleValue);  // 100.0

        // التحويل الصريح (التضييق) - يتطلب عامل التحويل
        double d = 99.99;
        int i = (int) d;  // double → int (يفقد الجزء العشري)

        System.out.println(i);  // 99 (الجزء العشري مقطوع)

        // احذر من الفيضان (overflow)
        int largeInt = 130;
        byte b = (byte) largeInt;  // فيضان!

        System.out.println(b);  // -126 (الفيضان يلتف)
    }
}
```

### المثال 4: Autoboxing و Unboxing

```java
import java.util.ArrayList;
import java.util.List;

public class AutoboxingDemo {
    public static void main(String[] args) {
        // Autoboxing: بدائي → كائن غلاف (تلقائي)
        int primitiveInt = 10;
        Integer wrapperInt = primitiveInt;  // autoboxing

        // Unboxing: كائن غلاف → بدائي (تلقائي)
        Integer wrapper = 20;
        int primitive = wrapper;  // unboxing

        // مفيد في المجموعات (المجموعات لا تخزن الأنواع البدائية)
        List<Integer> numbers = new ArrayList<>();
        numbers.add(5);      // autoboxing: int → Integer
        numbers.add(10);     // autoboxing

        int first = numbers.get(0);  // unboxing: Integer → int
        System.out.println(first);   // 5

        // احذر من NullPointerException
        Integer nullWrapper = null;
        // int value = nullWrapper;  // NullPointerException عند التشغيل!
    }
}
```

### المثال 5: لواحق القيم الحرفية (Literal Suffixes)

```java
public class Literals {
    public static void main(String[] args) {
        // قيم long تحتاج لاحقة 'L'
        long bigNumber = 10000000000L;  // 'L' مطلوبة للقيم > نطاق int

        // قيم float تحتاج لاحقة 'f'
        float floatNum = 10.5f;  // 'f' مطلوبة، وإلا تُعامل كـ double

        // double هو الافتراضي للأرقام العشرية
        double doubleNum = 10.5;  // لا حاجة للاحقة

        // الست عشري، الثماني، الثنائي (Java 7+)
        int hex = 0xFF;        // ست عشري
        int octal = 0177;      // ثماني
        int binary = 0b1010;   // ثنائي (Java 7+)

        System.out.println(hex);     // 255
        System.out.println(octal);   // 127
        System.out.println(binary);  // 10
    }
}
```

---

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما هي أنواع البيانات البدائية في Java؟**
2. **ما الفرق بين int و Integer؟**
3. **ما هو autoboxing و unboxing؟**
4. **ما القيمة الافتراضية للمتغيرات المحلية؟**
5. **ما الفرق بين متغيرات النسخة والمتغيرات الثابتة؟**
6. **هل يمكننا تخزين قيم عشرية في int؟**
7. **ماذا يحدث عند تعيين نوع أكبر لنوع أصغر؟**
8. **ما الفرق بين float و double؟**
9. **لماذا نحتاج فئات الغلاف؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما هي أنواع البيانات البدائية في Java؟**

"لدى Java 8 أنواع بيانات بدائية. للأعداد الصحيحة: byte، short، int، و long. للأعداد العشرية: float و double. للأحرف: char. وللقيم المنطقية: boolean. هذه تُخزن مباشرة في الذاكرة وليست كائنات، مما يجعلها أكثر كفاءة من الأنواع المرجعية."

**س2: ما الفرق بين int و Integer؟**

"int هو نوع بيانات بدائي يخزن القيمة الفعلية مباشرة في الذاكرة. لا يمكن أن يكون null وله قيمة افتراضية 0. Integer هي فئة غلاف—كائن يغلف النوع البدائي int. يمكن أن تكون null، ويمكن استخدامها في المجموعات، وتوفر دوال مساعدة مثل parseInt(). Integer تُخزن في ذاكرة heap بينما int عادة ما تُخزن في ذاكرة stack للمتغيرات المحلية."

**س3: ما هو autoboxing و unboxing؟**

"Autoboxing هو التحويل التلقائي من نوع بدائي إلى فئة الغلاف المقابلة، مثل تحويل int إلى Integer. Unboxing هو العكس—تحويل كائن الغلاف إلى النوع البدائي. قُدمت هذه الميزة في Java 5 لتسهيل العمل مع المجموعات. مثلاً، عند إضافة int إلى ArrayList من Integer، تقوم Java بتغليفها تلقائياً. يجب الانتباه لـ NullPointerException أثناء unboxing إذا كان كائن الغلاف null."

**س4: ما القيمة الافتراضية للمتغيرات المحلية؟**

"المتغيرات المحلية ليس لها قيم افتراضية. يجب تهيئتها صراحة قبل الاستخدام، وإلا يُصدر المترجم خطأ. هذا يختلف عن متغيرات النسخة والمتغيرات الثابتة، التي تُهيأ تلقائياً للقيم الافتراضية مثل 0 للأرقام، false للـ boolean، و null للكائنات."

**س5: ما الفرق بين متغيرات النسخة والمتغيرات الثابتة؟**

"متغيرات النسخة تنتمي لكائن ولكل كائن نسخته الخاصة. تُخزن في ذاكرة heap وتُهيأ عند إنشاء الكائن. المتغيرات الثابتة تنتمي للفئة نفسها ومشتركة بين جميع النسخ. تُخزن في منطقة الدوال (method area) وتُهيأ عند تحميل الفئة. إذا غيّر كائن متغيراً ثابتاً، يتغير لجميع الكائنات."

**س6: هل يمكننا تخزين قيم عشرية في int؟**

"لا، int هو نوع عدد صحيح ويمكنه فقط تخزين الأعداد الكاملة. إذا حاولت تعيين قيمة عشرية لـ int، يجب استخدام التحويل الصريح، الذي يقطع الجزء العشري. مثلاً، int x = (int) 5.9 سيخزن 5، ويفقد .9. لتخزين القيم العشرية، استخدم float أو double."

**س7: ماذا يحدث عند تعيين نوع أكبر لنوع أصغر؟**

"تحتاج تحويلاً صريحاً وقد تفقد بيانات. هذا يُسمى تحويل التضييق. مثلاً، تحويل long إلى int أو double إلى int يتطلب عامل تحويل. تخاطر بالفيضان إذا كانت القيمة كبيرة جداً للنوع المستهدف. مثلاً، تحويل قيمة int من 130 إلى byte ينتج -126 بسبب الفيضان لأن نطاق byte هو -128 إلى 127."

**س8: ما الفرق بين float و double؟**

"float هو نوع 32-bit بدقة مفردة مع حوالي 6-7 أرقام عشرية دقة، بينما double هو نوع 64-bit بدقة مزدوجة مع حوالي 15 رقماً عشرياً. double هو الافتراضي للقيم العشرية الحرفية في Java. float يتطلب لاحقة 'f'، مثل 10.5f. يُفضل double لمعظم الحسابات لأنه أكثر دقة، والمعالجات الحديثة تتعامل مع كليهما بكفاءة."

**س9: لماذا نحتاج فئات الغلاف؟**

"نحتاج فئات الغلاف لعدة أسباب. أولاً، المجموعات مثل ArrayList يمكنها فقط تخزين الكائنات، وليس الأنواع البدائية. ثانياً، توفر دوال مساعدة مثل Integer.parseInt() أو Integer.valueOf(). ثالثاً، مطلوبة عند العمل مع generics، التي لا تدعم الأنواع البدائية. رابعاً، يمكنها تمثيل null، وهو ما لا تستطيعه الأنواع البدائية. إنها ضرورية لميزات البرمجة الكائنية مثل تعدد الأشكال."

---

## 7️⃣ الأخطاء الشائعة

1. **نسيان تهيئة المتغيرات المحلية**:
   ```java
   int x;
   System.out.println(x);  // خطأ ترجمة!
   ```

2. **مقارنة كائنات الغلاف بـ ==**:
   ```java
   Integer a = 128;
   Integer b = 128;
   System.out.println(a == b);  // false (كائنات مختلفة)
   // استخدم .equals() بدلاً من ذلك
   ```

3. **عدم استخدام لاحقة 'L' لقيم long الحرفية**:
   ```java
   long big = 10000000000;  // خطأ ترجمة!
   long big = 10000000000L; // صحيح
   ```

4. **عدم استخدام لاحقة 'f' لـ float**:
   ```java
   float f = 10.5;   // خطأ ترجمة (تُعامل كـ double)
   float f = 10.5f;  // صحيح
   ```

5. **افتراض أن المتغيرات المحلية لها قيم افتراضية**:
   ```java
   void method() {
       int count;
       count++;  // خطأ: المتغير قد لا يكون مُهيأً
   }
   ```

6. **تجاهل الفيضان أثناء التضييق**:
   ```java
   int i = 300;
   byte b = (byte) i;  // فيضان: b تصبح 44، وليس 300
   ```

7. **فك تغليف كائنات غلاف null**:
   ```java
   Integer num = null;
   int value = num;  // NullPointerException عند التشغيل!
   ```

8. **الخلط بين char و String**:
   ```java
   char c = 'A';      // علامات اقتباس مفردة
   String s = "A";    // علامات اقتباس مزدوجة
   // char c = "A";   // خطأ ترجمة!
   ```

---

## 8️⃣ نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- اعرف جميع الأنواع البدائية الـ 8 وأحجامها
- افهم فرق الذاكرة: الأنواع البدائية في stack، الكائنات في heap
- اذكر autoboxing/unboxing كميزة Java 5+
- القيم الافتراضية: متغيرات النسخة/الثابتة لها افتراضيات، المحلية لا
- فئات الغلاف تمكّن الأنواع البدائية في المجموعات

**ما يجب تجنبه**:
- لا تخلط بين حجم int و Integer (كلاهما 32-bit، لكن Integer له عبء الكائن)
- لا تدّعِ أن الأنواع البدائية تُخزن في heap (يمكن ذلك كمتغيرات نسخة)
- لا تنسَ أن المتغيرات الثابتة على مستوى الفئة، وليس الكائن

**تحت الضغط**:
- إذا عُلقت في الأحجام، تذكر: int الأكثر شيوعاً (32-bit)، long ضعف ذلك (64-bit)، byte الأصغر (8-bit)
- ارسم جدولاً بسيطاً للأنواع البدائية إذا طُلب
- اذكر أن char هو 16-bit لأنه يدعم Unicode

**علامات التحذير التي يجب تجنبها**:
- "الأنواع البدائية هي كائنات" (لا، ليست كذلك)
- "المتغيرات المحلية قيمتها الافتراضية 0" (لا، ليس لها افتراضي)
- "float أكثر دقة من double" (العكس)
- "المتغيرات الثابتة أسرع" (مضلل—لها غرض مختلف)

**نقاط إضافية**:
- اذكر تخزين Integer المؤقت (-128 إلى 127) عند مناقشة مقارنة الأغلفة
- ناقش كفاءة الذاكرة: الأنواع البدائية مقابل الأغلفة
- أشر للفرق بين Integer.parseInt() و Integer.valueOf()
- اشرح لماذا المجموعات تستخدم generics مع فئات الغلاف، وليس الأنواع البدائية

</div>
