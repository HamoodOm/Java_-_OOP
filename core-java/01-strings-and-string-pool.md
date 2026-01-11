# Strings and String Pool

## 1️⃣ Concept Overview

**String** is one of the most widely used classes in Java. It represents a sequence of characters and has special treatment in the JVM.

**Key Characteristics**:
- **Immutable**: Once created, String content cannot be changed
- **Final**: String class is final and cannot be extended
- **Thread-safe**: Immutability makes Strings inherently thread-safe
- **Special Memory Management**: Strings are stored in a special area called the String Pool

**String Pool (String Constant Pool)**:
- A special memory region in the heap (moved from PermGen in Java 7)
- Stores unique String literals to save memory
- Part of Java's flyweight pattern implementation
- Strings created with literals are automatically interned (stored in pool)

**Three Ways to Create Strings**:
1. **String Literal**: `String s = "Hello";` (goes to String Pool)
2. **new Keyword**: `String s = new String("Hello");` (creates object in heap)
3. **intern() Method**: Explicitly add String to pool

**String vs StringBuilder vs StringBuffer**:
- **String**: Immutable, thread-safe, slower for modifications
- **StringBuilder**: Mutable, not thread-safe, fast (use in single-threaded scenarios)
- **StringBuffer**: Mutable, thread-safe, slower than StringBuilder

---

## 2️⃣ Why Interviewers Ask This

- **Fundamental class**: Used in virtually every Java program
- **Memory implications**: Understanding String Pool affects performance
- **Common bugs**: == vs equals() is a frequent source of bugs
- **Immutability concept**: Tests understanding of immutable objects
- **Performance awareness**: StringBuilder vs String concatenation
- **JVM internals**: Shows deeper understanding of Java memory management

---

## 3️⃣ Key Rules / Facts

**String Immutability**:
- Cannot modify existing String object
- Any "modification" creates a new String object
- Benefits: Thread-safety, caching, security, pool sharing
- Drawback: Memory overhead for frequent modifications

**String Pool Facts**:
- Only String literals are automatically pooled
- Strings created with `new` are NOT in pool (unless interned)
- Pool prevents duplicate String literals in memory
- `intern()` method adds String to pool or returns existing one
- Pool is in heap memory (Java 7+), was in PermGen before Java 7

**String Comparison**:
- `==` compares **references** (memory addresses)
- `equals()` compares **content** (actual characters)
- For pool Strings: `==` and `equals()` both return true
- For new Strings: `==` returns false, `equals()` returns true (if content same)

**Common String Methods**:
- `length()`, `charAt()`, `substring()`
- `indexOf()`, `lastIndexOf()`
- `toUpperCase()`, `toLowerCase()`
- `trim()`, `replace()`, `split()`
- `startsWith()`, `endsWith()`, `contains()`
- `equals()`, `equalsIgnoreCase()`, `compareTo()`
- `concat()`, `format()`, `valueOf()`

**StringBuilder/StringBuffer Methods**:
- `append()`, `insert()`, `delete()`
- `reverse()`, `replace()`
- `capacity()`, `ensureCapacity()`
- `toString()` to convert to String

**Performance**:
- String concatenation with `+` in loops is inefficient
- Use StringBuilder for building strings in loops
- StringBuffer when thread-safety is needed
- String is fine for simple concatenations (compiler optimizes)

---

## 4️⃣ Code Examples (Java 8)

### Example 1: String Pool Demonstration

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

### Example 2: String Immutability

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

### Example 3: intern() Method

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

### Example 4: String vs StringBuilder vs StringBuffer

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

### Example 5: StringBuilder Methods

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

### Example 6: Common String Methods

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

### Example 7: String Concatenation - Behind the Scenes

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

### Example 8: String Comparison Traps

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

### Example 9: Memory Impact

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

## 5️⃣ Common Interview Questions

1. **Why is String immutable in Java?**
2. **What is String Pool and how does it work?**
3. **What is the difference between String, StringBuilder, and StringBuffer?**
4. **What is the difference between == and equals() for Strings?**
5. **How many String objects are created in: String s = new String("Hello");?**
6. **What does the intern() method do?**
7. **Why is String concatenation in loops inefficient?**
8. **Can we make String mutable?**
9. **What is the output of: String s1 = "Test"; String s2 = "Test"; System.out.println(s1 == s2);?**
10. **When would you use StringBuffer over StringBuilder?**

---

## 6️⃣ Model Answers

**Q1: Why is String immutable in Java?**

"String is immutable for several reasons. First, for security—Strings are used for sensitive data like passwords, file paths, and network connections, so immutability prevents malicious modifications. Second, for String Pool optimization—immutability allows safe sharing of String literals across the application, saving memory. Third, for thread safety—immutable objects are inherently thread-safe without synchronization. Fourth, for caching—the hashCode can be cached since it won't change. And fifth, for safety with collections—Strings as HashMap keys won't break the map if they can't be modified after insertion."

**Q2: What is String Pool and how does it work?**

"The String Pool, or String Constant Pool, is a special memory area in the heap where Java stores String literals to optimize memory usage. When you create a String literal like 'Hello', Java first checks if it exists in the pool. If it does, it returns the existing reference. If not, it creates a new String in the pool. This means multiple variables can point to the same String object in the pool, saving memory. Strings created with new keyword don't go to the pool automatically unless you call intern(). The pool was moved from PermGen to heap in Java 7."

**Q3: What is the difference between String, StringBuilder, and StringBuffer?**

"String is immutable—any modification creates a new object, making it inefficient for frequent changes but thread-safe. StringBuilder is mutable and not synchronized, making it fast and ideal for single-threaded scenarios where you're building strings. StringBuffer is also mutable but synchronized, making it thread-safe but slower than StringBuilder. For simple concatenations, use String. For building strings in loops or when making many modifications in a single thread, use StringBuilder. Use StringBuffer only when you need thread safety, which is rare in modern code."

**Q4: What is the difference between == and equals() for Strings?**

"The == operator compares references—whether two variables point to the same memory location. The equals() method compares content—whether two Strings contain the same sequence of characters. For String literals from the pool, both return true because they reference the same object. But for Strings created with new, == returns false while equals() returns true if the content matches. You should almost always use equals() for String comparison to compare actual content, not memory addresses."

**Q5: How many String objects are created in: String s = new String("Hello");?**

"Two String objects are created. One is the literal 'Hello' which goes to the String Pool. The second is the object created in heap memory by the new keyword, which contains a copy of 'Hello'. So we have one in the pool and one in the heap. If 'Hello' already existed in the pool before this statement, then only one new object is created in the heap. This is why using new String() is generally discouraged—it defeats the purpose of the String Pool."

**Q6: What does the intern() method do?**

"The intern() method adds a String to the String Pool if it doesn't already exist, or returns the reference to the existing String in the pool. When you call str.intern(), Java checks if an equivalent String exists in the pool. If yes, it returns that pooled String's reference. If no, it adds str to the pool and returns the reference. This is useful when you have Strings created dynamically or with new keyword and want to take advantage of String Pool optimization. However, use it carefully as it can impact performance if overused."

**Q7: Why is String concatenation in loops inefficient?**

"String concatenation in loops is inefficient because Strings are immutable. Each concatenation creates a new String object and copies all existing characters plus the new ones. In a loop with n iterations, this creates n String objects and performs O(n²) character copying. For example, concatenating 1000 strings creates 1000 intermediate objects. StringBuilder avoids this by maintaining a mutable buffer that grows as needed, creating only one final String object at the end. This makes it O(n) instead of O(n²), which is significantly faster."

**Q8: Can we make String mutable?**

"No, you cannot make String mutable because the String class is declared final, which prevents extending it, and its internal character array is private and final. There's no way to modify an existing String object. This is by design for security, thread-safety, and optimization. If you need mutable strings, use StringBuilder or StringBuffer instead. The immutability of String is a fundamental design decision in Java that you cannot circumvent."

**Q9: What is the output of: String s1 = "Test"; String s2 = "Test"; System.out.println(s1 == s2);?**

"The output is true. Both s1 and s2 are created using String literals, so they reference the same object in the String Pool. When Java encounters the literal 'Test' the second time, it reuses the existing object from the pool instead of creating a new one. Therefore, both variables point to the same memory location, making == return true. This is different from using new String('Test'), which would create separate objects and make == return false."

**Q10: When would you use StringBuffer over StringBuilder?**

"You should use StringBuffer when you need thread safety—when multiple threads might access and modify the same String-building object concurrently. StringBuffer's methods are synchronized, preventing race conditions. However, in modern Java development, this is rarely needed because you can use StringBuilder with external synchronization if necessary, or better yet, use separate StringBuilder instances per thread. StringBuilder is preferred for single-threaded scenarios because it's faster. In practice, StringBuffer is rarely used anymore, and StringBuilder is the default choice for string building."

---

## 7️⃣ Common Mistakes

1. **Using == instead of equals() for content comparison**:
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

2. **String concatenation in loops**:
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

3. **Forgetting that String methods return new Strings**:
   ```java
   String s = "hello";
   s.toUpperCase();  // Returns new String, doesn't modify s
   System.out.println(s);  // Still "hello"
   
   // Correct:
   s = s.toUpperCase();
   System.out.println(s);  // "HELLO"
   ```

4. **Calling methods on null String**:
   ```java
   String s = null;
   s.equals("Test");  // NullPointerException!
   
   // Safe way:
   "Test".equals(s);  // Returns false, no exception
   // Or use Objects.equals(s, "Test");
   ```

5. **Unnecessary use of new String()**:
   ```java
   String s = new String("Hello");  // BAD - creates extra object
   
   String s = "Hello";  // GOOD - uses pool
   ```

6. **Not handling empty/null strings**:
   ```java
   String s = getUserInput();
   s.trim();  // NullPointerException if s is null!
   
   // Better:
   if (s != null && !s.isEmpty()) {
       s = s.trim();
   }
   ```

7. **Modifying string during iteration**:
   ```java
   String s = "abc";
   for (int i = 0; i < s.length(); i++) {
       s = s + i;  // Creates new string each iteration!
   }
   ```

8. **Assuming substring shares memory** (pre-Java 7 issue):
   ```java
   String large = generateLargeString();
   String small = large.substring(0, 10);
   // In Java 6, 'small' kept reference to 'large' array
   // In Java 7+, substring creates new char array
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- String is immutable for security, thread-safety, and optimization
- String Pool saves memory by reusing literals
- == compares references, equals() compares content
- StringBuilder for single-threaded, StringBuffer for multi-threaded (rare)
- Avoid String concatenation in loops

**What to avoid**:
- Don't claim String is stored in stack (it's in heap/pool)
- Don't say StringBuilder is "always better" (String is fine for simple cases)
- Don't forget to mention String Pool moved to heap in Java 7
- Don't confuse immutability with being final

**Under pressure**:
- Draw memory diagram showing pool vs heap
- Show example: String s1 = "Test"; String s2 = "Test"; (same reference)
- Explain why String concatenation creates many objects
- Remember: "String literal → pool, new String() → heap"

**Red flags to avoid**:
- "String is mutable" (it's not)
- "String Pool is in PermGen" (was before Java 7, now in heap)
- "== is fine for String comparison" (use equals())
- "StringBuffer is faster than StringBuilder" (it's slower due to synchronization)

**Bonus points**:
- Mention that String's hashCode() is cached
- Discuss String deduplication in G1 GC (Java 8u20+)
- Reference that String uses char array internally (byte array in Java 9+)
- Mention String.format() for complex formatting
- Discuss regular expressions with String methods (replaceAll, split, matches)
- Explain why String Pool optimization matters for large applications
- Reference that compiler optimizes simple concatenations to StringBuilder
