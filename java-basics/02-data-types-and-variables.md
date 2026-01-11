# Data Types and Variables

## 1️⃣ Concept Overview

**Data Types** in Java are divided into two categories:

1. **Primitive Types** (8 types): Store simple values directly in memory
   - Numeric: `byte`, `short`, `int`, `long`, `float`, `double`
   - Character: `char`
   - Boolean: `boolean`

2. **Reference Types**: Store references (memory addresses) to objects
   - Classes, Interfaces, Arrays, Enums

**Variables** are named memory locations that store values. Java has three types:
- **Local variables**: Declared inside methods/blocks
- **Instance variables**: Declared inside class but outside methods (non-static)
- **Static variables**: Declared with `static` keyword (class-level)

---

## 2️⃣ Why Interviewers Ask This

- **Memory management understanding**: Shows if you know how Java stores data
- **Type safety awareness**: Java is strongly typed—understanding types is fundamental
- **Performance implications**: Primitives vs objects affect performance
- **Common bug prevention**: Type casting errors are frequent in production
- **Foundation for OOP**: Reference types are essential for object-oriented programming

---

## 3️⃣ Key Rules / Facts

**Primitive Types:**
- Always have default values (unlike local variables)
- Stored directly in stack memory (for local variables)
- Faster than reference types
- Cannot be null
- Have corresponding wrapper classes

**Size and Range Table:**

| Type    | Size    | Range                                      | Default |
|---------|---------|---------------------------------------------|---------|
| byte    | 8-bit   | -128 to 127                                | 0       |
| short   | 16-bit  | -32,768 to 32,767                          | 0       |
| int     | 32-bit  | -2³¹ to 2³¹-1 (~2 billion)                | 0       |
| long    | 64-bit  | -2⁶³ to 2⁶³-1                              | 0L      |
| float   | 32-bit  | ~±3.4E+38 (6-7 decimal digits)            | 0.0f    |
| double  | 64-bit  | ~±1.7E+308 (15 decimal digits)            | 0.0d    |
| char    | 16-bit  | 0 to 65,535 (Unicode)                     | '\u0000'|
| boolean | 1-bit   | true or false                              | false   |

**Variable Rules:**
- Local variables must be initialized before use (no default value)
- Instance variables automatically initialized to default values
- Static variables initialized once when class loads
- Variables cannot start with digits
- Variable names are case-sensitive

**Wrapper Classes:**
- Each primitive has a wrapper: `Integer`, `Long`, `Double`, `Boolean`, etc.
- Enable primitives to be used in collections
- Support autoboxing/unboxing (automatic conversion)

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Primitive Types and Default Values

```java
public class DataTypesDemo {
    // Instance variables - have default values
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

### Example 2: Local vs Instance vs Static Variables

```java
public class VariableTypes {
    // Instance variable (different for each object)
    int instanceVar = 10;
    
    // Static variable (shared across all objects)
    static int staticVar = 20;
    
    public void demonstrateVariables() {
        // Local variable (must be initialized)
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
        
        obj1.staticVar = 300;  // Changes for all objects
        
        System.out.println("obj1.instance: " + obj1.instanceVar);  // 100
        System.out.println("obj2.instance: " + obj2.instanceVar);  // 200
        System.out.println("obj1.static: " + obj1.staticVar);      // 300
        System.out.println("obj2.static: " + obj2.staticVar);      // 300
    }
}
```

### Example 3: Type Casting

```java
public class TypeCasting {
    public static void main(String[] args) {
        // Implicit casting (widening) - automatic
        int intValue = 100;
        long longValue = intValue;      // int → long (safe)
        double doubleValue = intValue;   // int → double (safe)
        
        System.out.println(longValue);    // 100
        System.out.println(doubleValue);  // 100.0
        
        // Explicit casting (narrowing) - requires cast operator
        double d = 99.99;
        int i = (int) d;  // double → int (loses decimal part)
        
        System.out.println(i);  // 99 (decimal truncated)
        
        // Be careful with overflow
        int largeInt = 130;
        byte b = (byte) largeInt;  // Overflow!
        
        System.out.println(b);  // -126 (overflow wraps around)
    }
}
```

### Example 4: Autoboxing and Unboxing

```java
import java.util.ArrayList;
import java.util.List;

public class AutoboxingDemo {
    public static void main(String[] args) {
        // Autoboxing: primitive → wrapper object (automatic)
        int primitiveInt = 10;
        Integer wrapperInt = primitiveInt;  // autoboxing
        
        // Unboxing: wrapper object → primitive (automatic)
        Integer wrapper = 20;
        int primitive = wrapper;  // unboxing
        
        // Useful in collections (collections can't store primitives)
        List<Integer> numbers = new ArrayList<>();
        numbers.add(5);      // autoboxing: int → Integer
        numbers.add(10);     // autoboxing
        
        int first = numbers.get(0);  // unboxing: Integer → int
        System.out.println(first);   // 5
        
        // Watch out for NullPointerException
        Integer nullWrapper = null;
        // int value = nullWrapper;  // NullPointerException at runtime!
    }
}
```

### Example 5: Literal Suffixes

```java
public class Literals {
    public static void main(String[] args) {
        // Long literals need 'L' suffix
        long bigNumber = 10000000000L;  // 'L' required for values > int range
        
        // Float literals need 'f' suffix
        float floatNum = 10.5f;  // 'f' required, else treated as double
        
        // Double is default for decimals
        double doubleNum = 10.5;  // no suffix needed
        
        // Hexadecimal, Octal, Binary (Java 7+)
        int hex = 0xFF;        // hexadecimal
        int octal = 0177;      // octal
        int binary = 0b1010;   // binary (Java 7+)
        
        System.out.println(hex);     // 255
        System.out.println(octal);   // 127
        System.out.println(binary);  // 10
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What are the primitive data types in Java?**
2. **What is the difference between int and Integer?**
3. **What is autoboxing and unboxing?**
4. **What is the default value of local variables?**
5. **What is the difference between instance and static variables?**
6. **Can we store decimal values in int?**
7. **What happens when you assign a larger type to a smaller type?**
8. **What is the difference between float and double?**
9. **Why do we need wrapper classes?**

---

## 6️⃣ Model Answers

**Q1: What are the primitive data types in Java?**

"Java has 8 primitive data types. For integers: byte, short, int, and long. For floating-point: float and double. For characters: char. And for boolean values: boolean. These are stored directly in memory and are not objects, which makes them more efficient than reference types."

**Q2: What is the difference between int and Integer?**

"int is a primitive data type that stores the actual value directly in memory. It cannot be null and has a default value of 0. Integer is a wrapper class—an object that wraps the int primitive. It can be null, can be used in collections, and provides utility methods like parseInt(). Integer is stored in heap memory while int is typically stored in stack memory for local variables."

**Q3: What is autoboxing and unboxing?**

"Autoboxing is the automatic conversion of a primitive type to its corresponding wrapper class, like converting int to Integer. Unboxing is the reverse—converting a wrapper object back to a primitive. This was introduced in Java 5 to make working with collections easier. For example, when you add an int to an ArrayList of Integer, Java automatically boxes it. One thing to watch out for is NullPointerException during unboxing if the wrapper object is null."

**Q4: What is the default value of local variables?**

"Local variables don't have default values. They must be explicitly initialized before use, otherwise the compiler throws an error. This is different from instance and static variables, which are automatically initialized to default values like 0 for numbers, false for boolean, and null for objects."

**Q5: What is the difference between instance and static variables?**

"Instance variables belong to an object and each object has its own copy. They're stored in heap memory and initialized when the object is created. Static variables belong to the class itself and are shared across all instances. They're stored in the method area and initialized when the class is loaded. If one object changes a static variable, it's changed for all objects."

**Q6: Can we store decimal values in int?**

"No, int is an integer type and can only store whole numbers. If you try to assign a decimal value to an int, you must use explicit casting, which will truncate the decimal part. For example, int x = (int) 5.9 will store 5, losing the .9. To store decimal values, use float or double."

**Q7: What happens when you assign a larger type to a smaller type?**

"You need explicit casting and may lose data. This is called narrowing conversion. For example, converting long to int or double to int requires a cast operator. You risk overflow if the value is too large for the target type. For instance, casting an int value of 130 to byte results in -126 due to overflow because byte's range is -128 to 127."

**Q8: What is the difference between float and double?**

"float is a 32-bit single-precision type with about 6-7 decimal digits of precision, while double is a 64-bit double-precision type with about 15 decimal digits. double is the default for decimal literals in Java. float requires an 'f' suffix, like 10.5f. double is preferred for most calculations because it's more precise, and modern processors handle both efficiently."

**Q9: Why do we need wrapper classes?**

"We need wrapper classes for several reasons. First, collections like ArrayList can only store objects, not primitives. Second, they provide utility methods like Integer.parseInt() or Integer.valueOf(). Third, they're needed when working with generics, which don't support primitives. Fourth, they can represent null, which primitives cannot. They're essential for object-oriented features like polymorphism."

---

## 7️⃣ Common Mistakes

1. **Forgetting to initialize local variables**:
   ```java
   int x;
   System.out.println(x);  // Compilation error!
   ```

2. **Comparing wrapper objects with ==**:
   ```java
   Integer a = 128;
   Integer b = 128;
   System.out.println(a == b);  // false (different objects)
   // Use .equals() instead
   ```

3. **Not using 'L' suffix for long literals**:
   ```java
   long big = 10000000000;  // Compilation error!
   long big = 10000000000L; // Correct
   ```

4. **Not using 'f' suffix for float**:
   ```java
   float f = 10.5;   // Compilation error (treated as double)
   float f = 10.5f;  // Correct
   ```

5. **Assuming local variables have default values**:
   ```java
   void method() {
       int count;
       count++;  // Error: variable might not be initialized
   }
   ```

6. **Ignoring overflow during narrowing**:
   ```java
   int i = 300;
   byte b = (byte) i;  // Overflow: b becomes 44, not 300
   ```

7. **Unboxing null wrapper objects**:
   ```java
   Integer num = null;
   int value = num;  // NullPointerException at runtime!
   ```

8. **Confusing char with String**:
   ```java
   char c = 'A';      // single quotes
   String s = "A";    // double quotes
   // char c = "A";   // Compilation error!
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Know all 8 primitive types and their sizes
- Understand the memory difference: primitives in stack, objects in heap
- Mention autoboxing/unboxing as a Java 5+ feature
- Default values: instance/static get defaults, local variables don't
- Wrapper classes enable primitives in collections

**What to avoid**:
- Don't mix up int and Integer sizes (both are 32-bit, but Integer has object overhead)
- Don't claim primitives are stored in heap (they can be, as instance variables)
- Don't forget that static variables are class-level, not object-level

**Under pressure**:
- If stuck on sizes, remember: int is most common (32-bit), long is double that (64-bit), byte is smallest (8-bit)
- Draw a simple table of primitive types if asked
- Mention that char is 16-bit because it supports Unicode

**Red flags to avoid**:
- "Primitives are objects" (no, they're not)
- "Local variables default to 0" (no, they have no default)
- "float is more precise than double" (reversed)
- "Static variables are faster" (misleading—they serve a different purpose)

**Bonus points**:
- Mention Integer caching (-128 to 127) when discussing wrapper comparison
- Discuss memory efficiency: primitives vs wrappers
- Reference the Integer.parseInt() vs Integer.valueOf() difference
- Explain why collections use generics with wrapper classes, not primitives
