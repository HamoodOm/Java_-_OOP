<div dir="rtl">

# العوامل وتدفق التحكم

## 1️⃣ نظرة عامة على المفهوم

**العوامل (Operators)** هي رموز خاصة تُنفذ عمليات على المعاملات (المتغيرات والقيم). لدى Java عدة فئات:

- **الحسابية**: `+`، `-`، `*`، `/`، `%`
- **العلائقية**: `==`، `!=`، `>`، `<`، `>=`، `<=`
- **المنطقية**: `&&`، `||`، `!`
- **التعيين**: `=`، `+=`، `-=`، `*=`، `/=`، `%=`
- **الأحادية**: `++`، `--`، `+`، `-`، `!`
- **البتية**: `&`، `|`، `^`، `~`، `<<`، `>>`، `>>>`
- **الثلاثية**: `? :`
- **instanceof**: عامل مقارنة الأنواع

**جمل التحكم في التدفق** تتحكم في ترتيب التنفيذ:

- **اتخاذ القرار**: `if`، `if-else`، `if-else-if`، `switch`
- **الحلقات**: `for`، `while`، `do-while`، `enhanced for` (for-each)
- **التفرع**: `break`، `continue`، `return`

---

## 2️⃣ لماذا يسأل المحاورون عن هذا

- **مهارات بناء المنطق**: فهم العوامل أساسي لكتابة أي منطق
- **مصادر الأخطاء الشائعة**: أولوية العوامل والتقييم المختصر يسببان أخطاء كثيرة
- **الوعي بالأداء**: بعض العوامل أكثر كفاءة من غيرها
- **تحسين الحلقات**: فهم متى تستخدم أي نوع حلقة
- **معالجة الحالات الحدية**: منطق break/continue غالباً له حالات حدية

---

## 3️⃣ القواعد والحقائق الأساسية

**أولوية العوامل** (من الأعلى للأدنى):
1. اللاحقة: `expr++`، `expr--`
2. الأحادية: `++expr`، `--expr`، `+`، `-`، `!`، `~`
3. الضرب: `*`، `/`، `%`
4. الجمع: `+`، `-`
5. العلائقية: `<`، `>`، `<=`، `>=`، `instanceof`
6. المساواة: `==`، `!=`
7. المنطقية AND: `&&`
8. المنطقية OR: `||`
9. الثلاثية: `? :`
10. التعيين: `=`، `+=`، `-=`، إلخ

**قواعد مهمة**:
- `&&` و `||` تستخدمان **التقييم المختصر** (قد لا يُقيَّم المعامل الثاني)
- `&` و `|` تُقيّمان دائماً كلا المعاملين
- `++` و `--` لهما شكل بادئة (الزيادة أولاً) ولاحقة (الاستخدام أولاً ثم الزيادة)
- القسمة على صفر تُلقي `ArithmeticException` للأعداد الصحيحة، لكن تُرجع `Infinity` للـ float/double
- عامل باقي القسمة `%` يعمل مع الأعداد العشرية
- `+` للنصوص هو للربط، وليس حسابياً
- `switch` تعمل مع: byte، short، int، char، String (Java 7+)، enum

**قواعد التحكم في التدفق**:
- `switch` تسقط للحالة التالية ما لم يُستخدم `break`
- جميع مكونات حلقة `for` اختيارية: `for(;;)` صالحة (حلقة لا نهائية)
- Enhanced for-loop لا يمكنها تعديل المجموعة المُكرر عليها
- `do-while` تُنفذ مرة واحدة على الأقل (الشرط يُفحص في النهاية)
- يمكن استخدام التسميات (labels) مع `break` و `continue` للحلقات المتداخلة

---

## 4️⃣ أمثلة برمجية (Java 8)

### المثال 1: عوامل الزيادة/النقصان

```java
public class IncrementDemo {
    public static void main(String[] args) {
        int x = 5;

        // اللاحقة: استخدم القيمة الحالية، ثم زِد
        int a = x++;  // a = 5, x = 6
        System.out.println("a: " + a + ", x: " + x);  // a: 5, x: 6

        // البادئة: زِد أولاً، ثم استخدم
        int y = 5;
        int b = ++y;  // b = 6, y = 6
        System.out.println("b: " + b + ", y: " + y);  // b: 6, y: 6

        // فخ مقابلة شائع
        int z = 10;
        int result = z++ + ++z;  // 10 + 12 = 22
        // z تصبح 11 بعد z++، ثم 12 بعد ++z
        System.out.println("result: " + result + ", z: " + z);  // result: 22, z: 12
    }
}
```

### المثال 2: التقييم المختصر

```java
public class ShortCircuitDemo {
    public static void main(String[] args) {
        int x = 5;

        // && تختصر: الشرط الثاني لا يُقيَّم إذا كان الأول خطأ
        if (x < 3 && ++x > 5) {
            System.out.println("Inside if");
        }
        System.out.println("x: " + x);  // x: 5 (لم تُزد)

        // || تختصر: الشرط الثاني لا يُقيَّم إذا كان الأول صحيحاً
        int y = 5;
        if (y > 3 || ++y > 5) {
            System.out.println("Inside if");  // هذا يُنفذ
        }
        System.out.println("y: " + y);  // y: 5 (لم تُزد)

        // & لا تختصر: كلا الشرطين يُقيَّمان دائماً
        int z = 5;
        if (z < 3 & ++z > 5) {
            System.out.println("Inside if");
        }
        System.out.println("z: " + z);  // z: 6 (زادت رغم أن الشرط الأول خطأ)
    }
}
```

### المثال 3: العامل الثلاثي

```java
public class TernaryDemo {
    public static void main(String[] args) {
        int age = 20;

        // الشرط ? القيمة_إذا_صحيح : القيمة_إذا_خطأ
        String status = (age >= 18) ? "Adult" : "Minor";
        System.out.println(status);  // Adult

        // ثلاثي متداخل (تجنبه في الكود الحقيقي - صعب القراءة)
        int score = 85;
        String grade = (score >= 90) ? "A" :
                       (score >= 80) ? "B" :
                       (score >= 70) ? "C" : "F";
        System.out.println(grade);  // B

        // ثلاثي مع أنواع مختلفة (يجب أن تكون متوافقة)
        Object result = (age > 18) ? "Adult" : 100;  // يعمل (كلاهما Objects)
        System.out.println(result);  // Adult
    }
}
```

### المثال 4: جملة Switch (Java 8)

```java
public class SwitchDemo {
    public static void main(String[] args) {
        // switch تقليدية مع break
        int day = 3;
        String dayName;

        switch (day) {
            case 1:
                dayName = "Monday";
                break;
            case 2:
                dayName = "Tuesday";
                break;
            case 3:
                dayName = "Wednesday";
                break;
            default:
                dayName = "Other";
                break;
        }
        System.out.println(dayName);  // Wednesday

        // مثال السقوط (سؤال مقابلة شائع)
        int month = 2;
        int days;

        switch (month) {
            case 1: case 3: case 5: case 7: case 8: case 10: case 12:
                days = 31;
                break;
            case 4: case 6: case 9: case 11:
                days = 30;
                break;
            case 2:
                days = 28;
                break;
            default:
                days = 0;
        }
        System.out.println("Days: " + days);  // Days: 28

        // Switch مع String (Java 7+)
        String fruit = "Apple";

        switch (fruit) {
            case "Apple":
                System.out.println("Red fruit");
                break;
            case "Banana":
                System.out.println("Yellow fruit");
                break;
            default:
                System.out.println("Unknown fruit");
        }
    }
}
```

### المثال 5: أنواع الحلقات

```java
import java.util.Arrays;
import java.util.List;

public class LoopDemo {
    public static void main(String[] args) {
        // حلقة for تقليدية
        for (int i = 0; i < 5; i++) {
            System.out.print(i + " ");  // 0 1 2 3 4
        }
        System.out.println();

        // حلقة While
        int count = 0;
        while (count < 3) {
            System.out.print(count + " ");  // 0 1 2
            count++;
        }
        System.out.println();

        // Do-while (تُنفذ مرة واحدة على الأقل)
        int num = 10;
        do {
            System.out.print(num + " ");  // 10
            num++;
        } while (num < 10);  // الشرط خطأ، لكن الجسم نُفذ مرة
        System.out.println();

        // Enhanced for-loop (for-each)
        int[] numbers = {10, 20, 30, 40};
        for (int n : numbers) {
            System.out.print(n + " ");  // 10 20 30 40
        }
        System.out.println();

        // Enhanced for-loop مع List
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        for (String name : names) {
            System.out.print(name + " ");  // Alice Bob Charlie
        }
        System.out.println();

        // أشكال الحلقات اللانهائية
        // for (;;) { }           // لانهائية
        // while (true) { }       // لانهائية
        // do { } while (true);   // لانهائية
    }
}
```

### المثال 6: Break و Continue مع التسميات

```java
public class BreakContinueDemo {
    public static void main(String[] args) {
        // جملة Break
        for (int i = 0; i < 10; i++) {
            if (i == 5) {
                break;  // اخرج من الحلقة عندما i = 5
            }
            System.out.print(i + " ");  // 0 1 2 3 4
        }
        System.out.println();

        // جملة Continue
        for (int i = 0; i < 5; i++) {
            if (i == 2) {
                continue;  // تخطَّ التكرار عندما i = 2
            }
            System.out.print(i + " ");  // 0 1 3 4
        }
        System.out.println();

        // Break مع تسمية (للحلقات المتداخلة)
        outer:
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (i == 1 && j == 1) {
                    break outer;  // اخرج من كلتا الحلقتين
                }
                System.out.print("(" + i + "," + j + ") ");
            }
        }
        // Output: (0,0) (0,1) (0,2) (1,0)
        System.out.println();

        // Continue مع تسمية
        outer:
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (j == 1) {
                    continue outer;  // استمر للحلقة الخارجية
                }
                System.out.print("(" + i + "," + j + ") ");
            }
        }
        // Output: (0,0) (1,0) (2,0)
    }
}
```

### المثال 7: فخاخ أولوية العوامل

```java
public class PrecedenceDemo {
    public static void main(String[] args) {
        // خطأ شائع: && لها أولوية أعلى من ||
        boolean result1 = true || false && false;
        // تُقيَّم كـ: true || (false && false)
        System.out.println(result1);  // true

        // الحسابية قبل العلائقية
        int x = 5;
        boolean result2 = x + 2 > 6;  // (x + 2) > 6 = 7 > 6
        System.out.println(result2);  // true

        // التعيين لها أدنى أولوية
        int a, b;
        a = b = 10;  // من اليمين لليسار: b = 10، ثم a = b
        System.out.println("a: " + a + ", b: " + b);  // a: 10, b: 10

        // فخ عامل الزيادة
        int y = 5;
        int z = y++ * 2;  // y++ لها أولوية أعلى، لكن اللاحقة تستخدم القيمة الحالية
        // z = 5 * 2 = 10، ثم y تصبح 6
        System.out.println("y: " + y + ", z: " + z);  // y: 6, z: 10
    }
}
```

---

## 5️⃣ أسئلة المقابلات الشائعة

1. **ما الفرق بين `++i` و `i++`؟**
2. **ما هو التقييم المختصر؟ ما الفرق بين `&&` و `&`؟**
3. **ما ناتج `i = i++ + ++i` إذا كانت i = 5؟**
4. **هل يمكننا استخدام float أو double في جمل switch؟**
5. **ما الفرق بين break و continue؟**
6. **ماذا يحدث إذا لم تستخدم break في switch؟**
7. **هل يمكننا استخدام String في switch؟ من أي إصدار Java؟**
8. **ما الفرق بين while و do-while؟**
9. **كيف يعمل العامل الثلاثي؟**
10. **هل يمكننا تعديل مجموعة أثناء التكرار عليها بـ enhanced for-loop؟**

---

## 6️⃣ إجابات نموذجية

**س1: ما الفرق بين `++i` و `i++`؟**

"الفرق في متى تحدث الزيادة. مع `i++` (اللاحقة)، تُستخدم القيمة الحالية في التعبير أولاً، ثم يُزاد المتغير. مع `++i` (البادئة)، يُزاد المتغير أولاً، ثم تُستخدم القيمة الجديدة. مثلاً، إذا كانت i = 5، فإن `x = i++` تضع x = 5 وi تصبح 6. لكن `x = ++i` تضع كلاً من x وi = 6."

**س2: ما هو التقييم المختصر؟ ما الفرق بين `&&` و `&`؟**

"التقييم المختصر يعني أن المعامل الثاني يُقيَّم فقط إذا لزم الأمر. مع `&&`، إذا كان الشرط الأول خطأ، لا يُفحص الثاني لأن النتيجة ستكون خطأ على أي حال. بالمثل، مع `||`، إذا كان الشرط الأول صحيحاً، لا يُقيَّم الثاني. عاملا `&` و `|` المفردان لا يختصران—يُقيّمان دائماً كلا المعاملين. التقييم المختصر أكثر كفاءة ويمكن أن يمنع أخطاء مثل null pointer exceptions."

**س3: ما ناتج `i = i++ + ++i` إذا كانت i = 5؟**

"هذا سؤال مخادع لأنه يتضمن سلوكاً غير محدد بطرق معينة، لكن في Java مع i = 5: أولاً، `i++` تستخدم 5 ثم تزيد i إلى 6. ثم `++i` تزيد i إلى 7 وتستخدم 7. فيصبح التعبير 5 + 7 = 12، وتُعيَّن i لـ 12. القيمة النهائية لـ i هي 12. ومع ذلك، يجب تجنب كود كهذا عملياً لأنه مربك وصعب الصيانة."

**س4: هل يمكننا استخدام float أو double في جمل switch؟**

"لا، جمل switch في Java لا تدعم float أو double. تعمل فقط مع byte، short، int، char، String (منذ Java 7)، وأنواع enum. لمقارنات الأعداد العشرية، يجب استخدام جمل if-else بدلاً من ذلك."

**س5: ما الفرق بين break و continue؟**

"Break تنهي الحلقة تماماً وتنقل التحكم للجملة بعد الحلقة. Continue تتخطى بقية التكرار الحالي وتنتقل للتكرار التالي. مثلاً، في حلقة من 0 إلى 9، إذا استخدمت break عند 5، تتوقف الحلقة. لكن إذا استخدمت continue عند 5، تتخطى ذلك التكرار فقط وتستمر مع 6، 7، 8، 9."

**س6: ماذا يحدث إذا لم تستخدم break في switch؟**

"بدون break، جملة switch ستسقط للحالة التالية. هذا يعني أن جميع الحالات اللاحقة ستُنفذ حتى يُصادف break أو تنتهي switch. هذا يُسمى سلوك السقوط. بينما يمكن أن يكون مفيداً لتجميع الحالات معاً، غالباً ما يكون غير مقصود ويؤدي لأخطاء، لذا معظم أدوات الفحص الحديثة تحذر من break مفقود."

**س7: هل يمكننا استخدام String في switch؟ من أي إصدار Java؟**

"نعم، يمكننا استخدام String في جمل switch، لكن فقط من Java 7 فصاعداً. قبل Java 7، switch كانت تعمل فقط مع byte، short، int، char، و enum. مقارنة String في switch حساسة لحالة الأحرف وداخلياً تستخدم hashCode و equals الخاصة بـ String للمطابقة."

**س8: ما الفرق بين while و do-while؟**

"الفرق الرئيسي في متى يُفحص الشرط. While هي حلقة مُتحكم بها من الدخول—تفحص الشرط قبل تنفيذ جسم الحلقة. فإذا كان الشرط خطأ في البداية، الجسم لا يُنفذ أبداً. Do-while هي حلقة مُتحكم بها من الخروج—تفحص الشرط بعد تنفيذ الجسم، فالجسم يُنفذ دائماً مرة واحدة على الأقل. استخدم do-while عندما تريد ضمان تنفيذ واحد على الأقل، مثل التحقق من إدخال المستخدم."

**س9: كيف يعمل العامل الثلاثي؟**

"العامل الثلاثي هو اختصار لـ if-else وله الصيغة: الشرط ? القيمة_إذا_صحيح : القيمة_إذا_خطأ. يُقيّم الشرط، وإذا كان صحيحاً، يُرجع القيمة الأولى؛ إذا خطأ، يُرجع القيمة الثانية. مثلاً، `String status = (age >= 18) ? 'Adult' : 'Minor'`. مفيد للشروط البسيطة لكن يمكن أن يصبح صعب القراءة عند التداخل."

**س10: هل يمكننا تعديل مجموعة أثناء التكرار عليها بـ enhanced for-loop؟**

"لا، لا يمكنك تعديل بنية المجموعة أثناء التكرار بـ enhanced for-loop. إذا حاولت إضافة أو إزالة عناصر، ستحصل على ConcurrentModificationException. يمكنك تعديل خصائص الكائنات في المجموعة، لكن ليس بنية المجموعة نفسها. لتعديل مجموعة أثناء التكرار، يجب استخدام Iterator ودالة remove الخاصة به، أو استخدام حلقة for تقليدية مع الفهارس."

---

## 7️⃣ الأخطاء الشائعة

1. **الخلط بين `==` و `equals()` للنصوص**:
   ```java
   String s1 = new String("hello");
   String s2 = new String("hello");
   System.out.println(s1 == s2);        // false (كائنات مختلفة)
   System.out.println(s1.equals(s2));   // true (نفس المحتوى)
   ```

2. **نسيان break في switch**:
   ```java
   int day = 1;
   switch(day) {
       case 1: System.out.println("Mon");
       case 2: System.out.println("Tue");  // تُطبع أيضاً بدون break!
   }
   // Output: Mon
   //         Tue
   ```

3. **حلقات لانهائية بسبب شرط خاطئ**:
   ```java
   for (int i = 0; i < 10; i--) {  // i-- بدلاً من i++
       // حلقة لانهائية!
   }
   ```

4. **تعديل متغير الحلقة في enhanced for-loop**:
   ```java
   int[] arr = {1, 2, 3};
   for (int num : arr) {
       num = num * 2;  // لا تُعدل المصفوفة، فقط نسخة محلية
   }
   // arr لا تزال {1, 2, 3}
   ```

5. **Null pointer مع auto-unboxing**:
   ```java
   Integer num = null;
   if (num > 0) {  // NullPointerException! (فشل auto-unboxing)
   }
   ```

6. **فاصلة منقوطة بعد شرط الحلقة**:
   ```java
   for (int i = 0; i < 5; i++);  // الفاصلة المنقوطة تُنشئ جسم حلقة فارغ
   {
       System.out.println(i);  // كتلة منفصلة، ليست جزءاً من الحلقة
   }
   ```

7. **استخدام = بدلاً من == في الشروط**:
   ```java
   int x = 5;
   if (x = 10) {  // خطأ ترجمة (يُعيّن، لا يُقارن)
       // ...
   }
   ```

8. **افتراض أولوية العوامل**:
   ```java
   int result = 10 + 20 * 2;  // 50، وليس 60 (* لها أولوية أعلى)
   ```

---

## 8️⃣ نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- التقييم المختصر وفوائده في الأداء
- الفرق بين الزيادة البادئة واللاحقة
- سلوك السقوط في switch
- متى تستخدم أي نوع حلقة
- أولوية العوامل في التعبيرات المعقدة

**ما يجب تجنبه**:
- لا تدّعِ أن جميع العوامل تُقيَّم من اليسار لليمين (الأولوية مهمة)
- لا تقل "switch أسرع من if-else" (يعتمد على السياق)
- لا تنسَ أن enhanced for-loop تُنشئ نسخة من العناصر (للأنواع البدائية)

**تحت الضغط**:
- إذا سُئلت عن تعبيرات معقدة، اعمل عليها خطوة بخطوة
- ارسم الحلقات المتداخلة لتصور break/continue مع التسميات
- تذكر: `&&` و `||` تختصران، `&` و `|` لا

**علامات التحذير التي يجب تجنبها**:
- "++i أسرع دائماً من i++" (المترجم يُحسّن هذا)
- "Break يمكنها الخروج من حلقة واحدة فقط" (التسميات تسمح بالخروج من مستويات متعددة)
- "Enhanced for-loop أفضل دائماً" (ليس عندما تحتاج الفهرس أو التعديل)
- "Switch تعمل مع جميع الأنواع" (محدودة لأنواع محددة)

**نقاط إضافية**:
- اذكر أن switch على String تستخدم hashCode() للكفاءة
- ناقش متى `&` و `|` مفيدان (العمليات البتية، ضمان التأثيرات الجانبية)
- اشرح كيف تعمل enhanced for-loop مع واجهة Iterable
- أشر لـ ConcurrentModificationException المحتملة
- اذكر أن Java 12+ لديها تعبيرات switch محسّنة (لكن اذكر أنك تناقش Java 8)

</div>
