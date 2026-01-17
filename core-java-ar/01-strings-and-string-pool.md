<div dir="rtl">

# النصوص ومجمع النصوص (Strings and String Pool)

## 1. نظرة عامة على المفهوم

**String** هي واحدة من أكثر الفئات استخداماً في Java. تمثل سلسلة من الأحرف ولها معاملة خاصة في JVM.

**الخصائص الأساسية**:
- **غير قابلة للتغيير (Immutable)**: بمجرد إنشاء String، لا يمكن تغيير محتواها
- **نهائية (Final)**: فئة String نهائية ولا يمكن توريثها
- **آمنة للخيوط (Thread-safe)**: عدم قابلية التغيير يجعل String آمنة للخيوط بطبيعتها
- **إدارة ذاكرة خاصة**: يتم تخزين النصوص في منطقة خاصة تسمى مجمع النصوص (String Pool)

**مجمع النصوص (String Constant Pool)**:
- منطقة ذاكرة خاصة في الـ heap (نُقلت من PermGen في Java 7)
- تخزن النصوص الحرفية الفريدة لتوفير الذاكرة
- جزء من تطبيق نمط flyweight في Java
- النصوص المُنشأة كقيم حرفية تُضاف تلقائياً للمجمع (interned)

**ثلاث طرق لإنشاء النصوص**:
1. **القيمة الحرفية**: `String s = "Hello";` (تذهب إلى مجمع النصوص)
2. **كلمة new**: `String s = new String("Hello");` (تُنشئ كائناً في الـ heap)
3. **طريقة intern()**: إضافة String إلى المجمع بشكل صريح

**String مقابل StringBuilder مقابل StringBuffer**:
- **String**: غير قابلة للتغيير، آمنة للخيوط، أبطأ في التعديلات
- **StringBuilder**: قابلة للتغيير، غير آمنة للخيوط، سريعة (تُستخدم في السيناريوهات أحادية الخيط)
- **StringBuffer**: قابلة للتغيير، آمنة للخيوط، أبطأ من StringBuilder

---

## 2. لماذا يسأل المحاورون عن هذا

- **فئة أساسية**: تُستخدم في كل برنامج Java تقريباً
- **تأثيرات الذاكرة**: فهم مجمع النصوص يؤثر على الأداء
- **أخطاء شائعة**: == مقابل equals() هو مصدر متكرر للأخطاء
- **مفهوم عدم قابلية التغيير**: يختبر فهم الكائنات غير القابلة للتغيير
- **الوعي بالأداء**: StringBuilder مقابل ربط النصوص
- **داخليات JVM**: يُظهر فهماً أعمق لإدارة ذاكرة Java

---

## 3. القواعد والحقائق الأساسية

**عدم قابلية String للتغيير**:
- لا يمكن تعديل كائن String موجود
- أي "تعديل" يُنشئ كائن String جديداً
- الفوائد: أمان الخيوط، التخزين المؤقت، الأمان، مشاركة المجمع
- العيب: حمل الذاكرة الزائد للتعديلات المتكررة

**حقائق مجمع النصوص**:
- فقط القيم الحرفية للنصوص تُضاف تلقائياً للمجمع
- النصوص المُنشأة بـ `new` ليست في المجمع (إلا إذا استُخدم intern)
- المجمع يمنع تكرار النصوص الحرفية في الذاكرة
- طريقة `intern()` تضيف النص للمجمع أو تُرجع الموجود
- المجمع في ذاكرة الـ heap (Java 7+)، كان في PermGen قبل Java 7

**مقارنة النصوص**:
- `==` تقارن **المراجع** (عناوين الذاكرة)
- `equals()` تقارن **المحتوى** (الأحرف الفعلية)
- للنصوص في المجمع: `==` و `equals()` كلاهما يُرجع true
- للنصوص الجديدة: `==` تُرجع false، `equals()` تُرجع true (إذا تطابق المحتوى)

**طرق String الشائعة**:
- `length()`, `charAt()`, `substring()`
- `indexOf()`, `lastIndexOf()`
- `toUpperCase()`, `toLowerCase()`
- `trim()`, `replace()`, `split()`
- `startsWith()`, `endsWith()`, `contains()`
- `equals()`, `equalsIgnoreCase()`, `compareTo()`
- `concat()`, `format()`, `valueOf()`

**طرق StringBuilder/StringBuffer**:
- `append()`, `insert()`, `delete()`
- `reverse()`, `replace()`
- `capacity()`, `ensureCapacity()`
- `toString()` للتحويل إلى String

**الأداء**:
- ربط النصوص بـ `+` في الحلقات غير فعال
- استخدم StringBuilder لبناء النصوص في الحلقات
- StringBuffer عند الحاجة لأمان الخيوط
- String مناسبة للربط البسيط (المترجم يُحسّن)

---

## 4. أمثلة برمجية (Java 8)

### المثال 1: توضيح مجمع النصوص

```java
public class StringPoolDemo {
    public static void main(String[] args) {
        // String literals - go to String Pool
        String s1 = "Hello";
        String s2 = "Hello";

        System.out.println(s1 == s2);        // true (same reference in pool)
        System.out.println(s1.equals(s2));   // true (same content)

        // String created with new - goes to heap, not pool
        String s3 = new String("Hello");
        String s4 = new String("Hello");

        System.out.println(s3 == s4);        // false (different objects in heap)
        System.out.println(s3.equals(s4));   // true (same content)

        // Comparing literal with new
        System.out.println(s1 == s3);        // false (different memory locations)
        System.out.println(s1.equals(s3));   // true (same content)

        // Memory visualization:
        // String Pool: "Hello" (pointed by s1 and s2)
        // Heap: String object "Hello" (s3)
        // Heap: String object "Hello" (s4)
    }
}
```

### المثال 2: عدم قابلية String للتغيير

```java
public class StringImmutability {
    public static void main(String[] args) {
        String original = "Hello";
        System.out.println("Original: " + original);           // Hello
        System.out.println("HashCode: " + original.hashCode()); // 69609650

        // Attempting to "modify" - actually creates new String
        String modified = original.concat(" World");

        System.out.println("Original: " + original);           // Hello (unchanged!)
        System.out.println("Modified: " + modified);           // Hello World
        System.out.println("Original HashCode: " + original.hashCode()); // 69609650 (same)
        System.out.println("Modified HashCode: " + modified.hashCode()); // Different

        // Another example
        String s = "Java";
        s.toUpperCase();  // Creates new String but doesn't assign
        System.out.println(s);  // Java (still lowercase!)

        s = s.toUpperCase();  // Must reassign to see change
        System.out.println(s);  // JAVA

        // Yet another example
        String str = "Test";
        str.concat(" String");  // New String created but lost
        System.out.println(str);  // Test (unchanged)

        str = str.concat(" String");  // Reassign to capture change
        System.out.println(str);  // Test String
    }
}
```

### المثال 3: طريقة intern()

```java
public class StringInternDemo {
    public static void main(String[] args) {
        // Strings created with new - in heap
        String s1 = new String("Hello");
        String s2 = new String("Hello");

        System.out.println(s1 == s2);        // false
        System.out.println(s1.equals(s2));   // true

        // Using intern() - adds to pool or returns existing
        String s3 = s1.intern();  // "Hello" already in pool (from literal elsewhere)
        String s4 = s2.intern();  // Returns same reference as s3

        System.out.println(s3 == s4);        // true (both from pool)

        // Comparing with literal
        String s5 = "Hello";
        System.out.println(s3 == s5);        // true (both from pool)
        System.out.println(s1 == s5);        // false (s1 in heap, s5 in pool)

        // Practical use of intern()
        String s6 = new String("World").intern();
        String s7 = "World";
        System.out.println(s6 == s7);        // true (both from pool)
    }
}
```

### المثال 4: String مقابل StringBuilder مقابل StringBuffer

```java
public class StringComparisonDemo {
    public static void main(String[] args) {
        // String - Immutable, creates many objects
        long startTime = System.currentTimeMillis();
        String str = "";
        for (int i = 0; i < 10000; i++) {
            str += i;  // Creates new String object each time!
        }
        long endTime = System.currentTimeMillis();
        System.out.println("String time: " + (endTime - startTime) + "ms");

        // StringBuilder - Mutable, single object, not thread-safe
        startTime = System.currentTimeMillis();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 10000; i++) {
            sb.append(i);  // Modifies same object
        }
        String result = sb.toString();
        endTime = System.currentTimeMillis();
        System.out.println("StringBuilder time: " + (endTime - startTime) + "ms");

        // StringBuffer - Mutable, single object, thread-safe
        startTime = System.currentTimeMillis();
        StringBuffer sbf = new StringBuffer();
        for (int i = 0; i < 10000; i++) {
            sbf.append(i);  // Synchronized, thread-safe
        }
        result = sbf.toString();
        endTime = System.currentTimeMillis();
        System.out.println("StringBuffer time: " + (endTime - startTime) + "ms");

        // Typical output:
        // String time: 150-300ms (very slow!)
        // StringBuilder time: 1-3ms (very fast!)
        // StringBuffer time: 2-5ms (slightly slower than StringBuilder)
    }
}
```

### المثال 5: طرق StringBuilder

```java
public class StringBuilderDemo {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Hello");

        // append() - add to end
        sb.append(" World");
        System.out.println(sb);  // Hello World

        // insert() - add at position
        sb.insert(6, "Beautiful ");
        System.out.println(sb);  // Hello Beautiful World

        // delete() - remove characters
        sb.delete(6, 16);  // Remove "Beautiful "
        System.out.println(sb);  // Hello World

        // reverse() - reverse the string
        sb.reverse();
        System.out.println(sb);  // dlroW olleH

        sb.reverse();  // Reverse back

        // replace() - replace substring
        sb.replace(0, 5, "Hi");
        System.out.println(sb);  // Hi World

        // capacity() - current capacity
        System.out.println("Capacity: " + sb.capacity());  // Default 16 + length

        // length() vs capacity()
        System.out.println("Length: " + sb.length());      // Actual characters
        System.out.println("Capacity: " + sb.capacity());  // Allocated space

        // Method chaining
        StringBuilder sb2 = new StringBuilder();
        sb2.append("Java")
           .append(" ")
           .append("Programming")
           .insert(0, "Learn ")
           .reverse();
        System.out.println(sb2);  // gnimmargorP avaJ nraeL
    }
}
```

### المثال 6: طرق String الشائعة

```java
public class StringMethodsDemo {
    public static void main(String[] args) {
        String str = "  Hello World  ";

        // length()
        System.out.println("Length: " + str.length());  // 15

        // trim() - removes leading/trailing whitespace
        String trimmed = str.trim();
        System.out.println("Trimmed: '" + trimmed + "'");  // 'Hello World'

        // substring()
        System.out.println(trimmed.substring(0, 5));   // Hello
        System.out.println(trimmed.substring(6));      // World

        // charAt()
        System.out.println("Char at 0: " + trimmed.charAt(0));  // H

        // indexOf() / lastIndexOf()
        System.out.println("Index of 'o': " + trimmed.indexOf('o'));      // 4
        System.out.println("Last index of 'o': " + trimmed.lastIndexOf('o')); // 7

        // contains()
        System.out.println("Contains 'World': " + trimmed.contains("World"));  // true

        // startsWith() / endsWith()
        System.out.println("Starts with 'Hello': " + trimmed.startsWith("Hello"));  // true
        System.out.println("Ends with 'World': " + trimmed.endsWith("World"));      // true

        // replace()
        String replaced = trimmed.replace('o', '0');
        System.out.println("Replaced: " + replaced);  // Hell0 W0rld

        // replaceAll() - uses regex
        String allReplaced = trimmed.replaceAll("[aeiou]", "*");
        System.out.println("All replaced: " + allReplaced);  // H*ll* W*rld

        // split()
        String[] words = trimmed.split(" ");
        for (String word : words) {
            System.out.println(word);  // Hello, then World
        }

        // toUpperCase() / toLowerCase()
        System.out.println("Upper: " + trimmed.toUpperCase());  // HELLO WORLD
        System.out.println("Lower: " + trimmed.toLowerCase());  // hello world

        // equals() / equalsIgnoreCase()
        System.out.println("Equals 'Hello World': " + trimmed.equals("Hello World"));  // true
        System.out.println("Equals ignore case 'hello world': " +
                          trimmed.equalsIgnoreCase("hello world"));  // true

        // compareTo()
        System.out.println("Compare to 'Hello': " + trimmed.compareTo("Hello"));  // positive

        // isEmpty() / isBlank() (isBlank from Java 11, but showing isEmpty)
        System.out.println("Is empty: " + "".isEmpty());     // true
        System.out.println("Is empty: " + "  ".isEmpty());   // false
    }
}
```

### المثال 7: ربط النصوص - ما يحدث خلف الكواليس

```java
public class StringConcatenation {
    public static void main(String[] args) {
        // Simple concatenation - compiler optimizes
        String s1 = "Hello" + " " + "World";
        // Compiler converts to: String s1 = "Hello World";

        // Concatenation with variables
        String firstName = "John";
        String lastName = "Doe";
        String fullName = firstName + " " + lastName;
        // Compiler uses StringBuilder internally:
        // StringBuilder temp = new StringBuilder();
        // temp.append(firstName).append(" ").append(lastName);
        // String fullName = temp.toString();

        // BAD: Concatenation in loop
        String result = "";
        for (int i = 0; i < 5; i++) {
            result += i;  // Creates new String object each iteration!
        }
        // This creates many intermediate String objects: "", "0", "01", "012", etc.

        // GOOD: StringBuilder in loop
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 5; i++) {
            sb.append(i);  // Modifies same object
        }
        result = sb.toString();  // One String created at end

        // concat() method
        String s2 = "Hello";
        String s3 = s2.concat(" World");
        System.out.println(s3);  // Hello World
    }
}
```

### المثال 8: فخاخ مقارنة النصوص

```java
public class StringComparisonTraps {
    public static void main(String[] args) {
        // Trap 1: == vs equals
        String s1 = "Test";
        String s2 = "Test";
        String s3 = new String("Test");

        System.out.println(s1 == s2);         // true (same pool reference)
        System.out.println(s1 == s3);         // false (different memory locations)
        System.out.println(s1.equals(s3));    // true (same content)

        // Trap 2: String concatenation and pool
        String s4 = "Hello" + "World";  // Compile-time constant
        String s5 = "HelloWorld";
        System.out.println(s4 == s5);   // true (both in pool)

        String hello = "Hello";
        String world = "World";
        String s6 = hello + world;  // Runtime concatenation
        System.out.println(s6 == s5);   // false (s6 not in pool)

        // Trap 3: final makes it compile-time constant
        final String HELLO = "Hello";
        final String WORLD = "World";
        String s7 = HELLO + WORLD;
        System.out.println(s7 == s5);   // true (compile-time constant)

        // Trap 4: null comparisons
        String s8 = null;
        // System.out.println(s8.equals("Test"));  // NullPointerException!
        System.out.println("Test".equals(s8));     // false (safe - no exception)

        // Trap 5: String pool with substring (before Java 7)
        String large = "HelloWorld";
        String small = large.substring(0, 5);  // "Hello"
        String literal = "Hello";
        System.out.println(small == literal);  // false (substring creates new String)
    }
}
```

### المثال 9: تأثير الذاكرة

```java
public class StringMemoryDemo {
    public static void main(String[] args) {
        // Scenario 1: All literals - only 1 String object in pool
        String s1 = "Java";
        String s2 = "Java";
        String s3 = "Java";
        // Memory: 1 String object in pool

        // Scenario 2: Using new - 3 String objects in heap + 1 in pool
        String s4 = new String("Java");
        String s5 = new String("Java");
        String s6 = new String("Java");
        // Memory: 1 in pool + 3 in heap = 4 String objects

        // Scenario 3: Repeated concatenation
        String result = "";
        for (int i = 0; i < 5; i++) {
            result = result + i;
        }
        // Creates: "", "0", "01", "012", "0123", "01234" = 6 String objects

        // Scenario 4: StringBuilder
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 5; i++) {
            sb.append(i);
        }
        result = sb.toString();
        // Creates: 1 StringBuilder object + 1 final String = 2 objects

        System.out.println("String concatenation creates many objects");
        System.out.println("StringBuilder creates fewer objects");
    }
}
```

---

## 5. أسئلة المقابلات الشائعة

1. **لماذا String غير قابلة للتغيير في Java؟**
2. **ما هو مجمع النصوص وكيف يعمل؟**
3. **ما الفرق بين String و StringBuilder و StringBuffer؟**
4. **ما الفرق بين == و equals() للنصوص؟**
5. **كم عدد كائنات String التي تُنشأ في: String s = new String("Hello");؟**
6. **ماذا تفعل طريقة intern()؟**
7. **لماذا ربط النصوص في الحلقات غير فعال؟**
8. **هل يمكن جعل String قابلة للتغيير؟**
9. **ما هو ناتج: String s1 = "Test"; String s2 = "Test"; System.out.println(s1 == s2);؟**
10. **متى تستخدم StringBuffer بدلاً من StringBuilder؟**

---

## 6. إجابات نموذجية

**س1: لماذا String غير قابلة للتغيير في Java؟**

"String غير قابلة للتغيير لعدة أسباب. أولاً، للأمان—تُستخدم النصوص للبيانات الحساسة مثل كلمات المرور ومسارات الملفات واتصالات الشبكة، لذا عدم قابلية التغيير يمنع التعديلات الضارة. ثانياً، لتحسين مجمع النصوص—عدم قابلية التغيير يسمح بمشاركة آمنة للنصوص الحرفية عبر التطبيق، مما يوفر الذاكرة. ثالثاً، لأمان الخيوط—الكائنات غير القابلة للتغيير آمنة للخيوط بطبيعتها بدون مزامنة. رابعاً، للتخزين المؤقت—يمكن تخزين hashCode مؤقتاً لأنه لن يتغير. وخامساً، للسلامة مع المجموعات—النصوص كمفاتيح HashMap لن تُفسد الخريطة إذا لم يكن بالإمكان تعديلها بعد الإدراج."

**س2: ما هو مجمع النصوص وكيف يعمل؟**

"مجمع النصوص، أو String Constant Pool، هو منطقة ذاكرة خاصة في الـ heap حيث تخزن Java النصوص الحرفية لتحسين استخدام الذاكرة. عندما تُنشئ نصاً حرفياً مثل 'Hello'، تتحقق Java أولاً من وجوده في المجمع. إذا وُجد، تُرجع المرجع الموجود. إذا لم يوجد، تُنشئ نصاً جديداً في المجمع. هذا يعني أن متغيرات متعددة يمكنها الإشارة لنفس كائن String في المجمع، مما يوفر الذاكرة. النصوص المُنشأة بكلمة new لا تذهب للمجمع تلقائياً إلا إذا استدعيت intern(). نُقل المجمع من PermGen إلى heap في Java 7."

**س3: ما الفرق بين String و StringBuilder و StringBuffer؟**

"String غير قابلة للتغيير—أي تعديل يُنشئ كائناً جديداً، مما يجعلها غير فعالة للتغييرات المتكررة لكن آمنة للخيوط. StringBuilder قابلة للتغيير وغير متزامنة، مما يجعلها سريعة ومثالية للسيناريوهات أحادية الخيط حيث تبني نصوصاً. StringBuffer قابلة للتغيير أيضاً لكن متزامنة، مما يجعلها آمنة للخيوط لكن أبطأ من StringBuilder. للربط البسيط، استخدم String. لبناء النصوص في الحلقات أو عند إجراء تعديلات كثيرة في خيط واحد، استخدم StringBuilder. استخدم StringBuffer فقط عندما تحتاج أمان الخيوط، وهو نادر في الكود الحديث."

**س4: ما الفرق بين == و equals() للنصوص؟**

"عامل == يقارن المراجع—هل متغيران يشيران لنفس موقع الذاكرة. طريقة equals() تقارن المحتوى—هل نصان يحتويان نفس تسلسل الأحرف. للنصوص الحرفية من المجمع، كلاهما يُرجع true لأنهما يشيران لنفس الكائن. لكن للنصوص المُنشأة بـ new، == تُرجع false بينما equals() تُرجع true إذا تطابق المحتوى. يجب استخدام equals() دائماً تقريباً لمقارنة النصوص لمقارنة المحتوى الفعلي، وليس عناوين الذاكرة."

**س5: كم عدد كائنات String التي تُنشأ في: String s = new String("Hello");؟**

"يُنشأ كائنا String. الأول هو الحرفي 'Hello' الذي يذهب لمجمع النصوص. الثاني هو الكائن المُنشأ في ذاكرة heap بواسطة كلمة new، والذي يحتوي نسخة من 'Hello'. لذا لدينا واحد في المجمع وواحد في الـ heap. إذا كان 'Hello' موجوداً مسبقاً في المجمع قبل هذه العبارة، فيُنشأ كائن جديد واحد فقط في الـ heap. لهذا يُنصح عموماً بعدم استخدام new String()—لأنه يُبطل غرض مجمع النصوص."

**س6: ماذا تفعل طريقة intern()؟**

"طريقة intern() تضيف String لمجمع النصوص إذا لم تكن موجودة مسبقاً، أو تُرجع المرجع للنص الموجود في المجمع. عندما تستدعي str.intern()، تتحقق Java من وجود نص مكافئ في المجمع. إذا نعم، تُرجع مرجع ذلك النص في المجمع. إذا لا، تضيف str للمجمع وتُرجع المرجع. هذا مفيد عندما يكون لديك نصوص مُنشأة ديناميكياً أو بكلمة new وتريد الاستفادة من تحسين مجمع النصوص. لكن استخدمها بحذر لأنها يمكن أن تؤثر على الأداء إذا أُفرط في استخدامها."

**س7: لماذا ربط النصوص في الحلقات غير فعال؟**

"ربط النصوص في الحلقات غير فعال لأن النصوص غير قابلة للتغيير. كل عملية ربط تُنشئ كائن String جديداً وتنسخ كل الأحرف الموجودة بالإضافة للجديدة. في حلقة من n تكرار، هذا يُنشئ n كائن String ويُجري نسخ أحرف بتعقيد O(n²). مثلاً، ربط 1000 نص يُنشئ 1000 كائن وسيط. StringBuilder يتجنب هذا بالحفاظ على مخزن مؤقت قابل للتغيير ينمو حسب الحاجة، مُنشئاً كائن String نهائياً واحداً فقط في النهاية. هذا يجعله O(n) بدلاً من O(n²)، وهو أسرع بشكل ملحوظ."

**س8: هل يمكن جعل String قابلة للتغيير؟**

"لا، لا يمكنك جعل String قابلة للتغيير لأن فئة String مُعلنة كـ final، مما يمنع توريثها، ومصفوفة الأحرف الداخلية خاصة ونهائية. لا توجد طريقة لتعديل كائن String موجود. هذا بالتصميم للأمان وأمان الخيوط والتحسين. إذا احتجت نصوصاً قابلة للتغيير، استخدم StringBuilder أو StringBuffer بدلاً من ذلك. عدم قابلية String للتغيير هو قرار تصميم جوهري في Java لا يمكنك تجاوزه."

**س9: ما هو ناتج: String s1 = "Test"; String s2 = "Test"; System.out.println(s1 == s2);؟**

"الناتج هو true. كلا s1 و s2 أُنشئا باستخدام نصوص حرفية، لذا يشيران لنفس الكائن في مجمع النصوص. عندما تصادف Java الحرفي 'Test' للمرة الثانية، تُعيد استخدام الكائن الموجود من المجمع بدلاً من إنشاء جديد. لذلك، كلا المتغيرين يشيران لنفس موقع الذاكرة، مما يجعل == تُرجع true. هذا يختلف عن استخدام new String('Test')، الذي سيُنشئ كائنات منفصلة ويجعل == تُرجع false."

**س10: متى تستخدم StringBuffer بدلاً من StringBuilder؟**

"يجب استخدام StringBuffer عندما تحتاج أمان الخيوط—عندما قد تصل خيوط متعددة وتعدل نفس كائن بناء النصوص بشكل متزامن. طرق StringBuffer متزامنة، مما يمنع حالات السباق. لكن في تطوير Java الحديث، هذا نادراً ما يُحتاج لأنه يمكنك استخدام StringBuilder مع مزامنة خارجية إذا لزم الأمر، أو أفضل من ذلك، استخدام مثيلات StringBuilder منفصلة لكل خيط. StringBuilder مفضل للسيناريوهات أحادية الخيط لأنه أسرع. عملياً، StringBuffer نادراً ما يُستخدم الآن، و StringBuilder هو الخيار الافتراضي لبناء النصوص."

---

## 7. الأخطاء الشائعة

1. **استخدام == بدلاً من equals() لمقارنة المحتوى**:
   ```java
   String s1 = new String("Hello");
   String s2 = new String("Hello");
   if (s1 == s2) {  // Wrong! Compares references
       // This won't execute
   }
   // Correct:
   if (s1.equals(s2)) {  // Compares content
       // This will execute
   }
   ```

2. **ربط النصوص في الحلقات**:
   ```java
   // BAD - creates many objects
   String result = "";
   for (int i = 0; i < 1000; i++) {
       result += i;  // Very inefficient!
   }

   // GOOD - use StringBuilder
   StringBuilder sb = new StringBuilder();
   for (int i = 0; i < 1000; i++) {
       sb.append(i);
   }
   String result = sb.toString();
   ```

3. **نسيان أن طرق String تُرجع نصوصاً جديدة**:
   ```java
   String s = "hello";
   s.toUpperCase();  // Returns new String, doesn't modify s
   System.out.println(s);  // Still "hello"

   // Correct:
   s = s.toUpperCase();
   System.out.println(s);  // "HELLO"
   ```

4. **استدعاء طرق على نص null**:
   ```java
   String s = null;
   s.equals("Test");  // NullPointerException!

   // Safe way:
   "Test".equals(s);  // Returns false, no exception
   // Or use Objects.equals(s, "Test");
   ```

5. **استخدام غير ضروري لـ new String()**:
   ```java
   String s = new String("Hello");  // BAD - creates extra object

   String s = "Hello";  // GOOD - uses pool
   ```

6. **عدم معالجة النصوص الفارغة/null**:
   ```java
   String s = getUserInput();
   s.trim();  // NullPointerException if s is null!

   // Better:
   if (s != null && !s.isEmpty()) {
       s = s.trim();
   }
   ```

7. **تعديل النص أثناء التكرار**:
   ```java
   String s = "abc";
   for (int i = 0; i < s.length(); i++) {
       s = s + i;  // Creates new string each iteration!
   }
   ```

8. **افتراض أن substring تشارك الذاكرة** (مشكلة ما قبل Java 7):
   ```java
   String large = generateLargeString();
   String small = large.substring(0, 10);
   // In Java 6, 'small' kept reference to 'large' array
   // In Java 7+, substring creates new char array
   ```

---

## 8. نصائح من المقابلات الفعلية

**ما يجب التأكيد عليه**:
- String غير قابلة للتغيير للأمان وأمان الخيوط والتحسين
- مجمع النصوص يوفر الذاكرة بإعادة استخدام الحروف
- == تقارن المراجع، equals() تقارن المحتوى
- StringBuilder لأحادية الخيط، StringBuffer لتعدد الخيوط (نادر)
- تجنب ربط النصوص في الحلقات

**ما يجب تجنبه**:
- لا تدّعِ أن String مخزنة في الـ stack (هي في heap/المجمع)
- لا تقل أن StringBuilder "أفضل دائماً" (String مناسبة للحالات البسيطة)
- لا تنسَ ذكر أن مجمع النصوص انتقل للـ heap في Java 7
- لا تخلط بين عدم قابلية التغيير وكون الفئة final

**تحت الضغط**:
- ارسم مخططاً للذاكرة يوضح المجمع مقابل الـ heap
- أظهر مثالاً: String s1 = "Test"; String s2 = "Test"; (نفس المرجع)
- اشرح لماذا ربط النصوص يُنشئ كائنات كثيرة
- تذكر: "نص حرفي ← المجمع، new String() ← heap"

**علامات تحذيرية يجب تجنبها**:
- "String قابلة للتغيير" (ليست كذلك)
- "مجمع النصوص في PermGen" (كان قبل Java 7، الآن في heap)
- "== مناسبة لمقارنة النصوص" (استخدم equals())
- "StringBuffer أسرع من StringBuilder" (أبطأ بسبب المزامنة)

**نقاط إضافية**:
- اذكر أن hashCode() للـ String مخزنة مؤقتاً
- ناقش إزالة تكرار النصوص في G1 GC (Java 8u20+)
- أشر إلى أن String تستخدم مصفوفة char داخلياً (مصفوفة byte في Java 9+)
- اذكر String.format() للتنسيق المعقد
- ناقش التعبيرات النمطية مع طرق String (replaceAll, split, matches)
- اشرح لماذا تحسين مجمع النصوص مهم للتطبيقات الكبيرة
- أشر إلى أن المترجم يُحسّن عمليات الربط البسيطة إلى StringBuilder

</div>
