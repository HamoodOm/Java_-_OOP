# Operators and Control Flow

## 1️⃣ Concept Overview

**Operators** are special symbols that perform operations on operands (variables and values). Java has several categories:

- **Arithmetic**: `+`, `-`, `*`, `/`, `%`
- **Relational**: `==`, `!=`, `>`, `<`, `>=`, `<=`
- **Logical**: `&&`, `||`, `!`
- **Assignment**: `=`, `+=`, `-=`, `*=`, `/=`, `%=`
- **Unary**: `++`, `--`, `+`, `-`, `!`
- **Bitwise**: `&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`
- **Ternary**: `? :`
- **instanceof**: Type comparison operator

**Control Flow Statements** control the execution order:

- **Decision-making**: `if`, `if-else`, `if-else-if`, `switch`
- **Looping**: `for`, `while`, `do-while`, `enhanced for` (for-each)
- **Branching**: `break`, `continue`, `return`

---

## 2️⃣ Why Interviewers Ask This

- **Logic building skills**: Understanding operators is fundamental to writing any logic
- **Common bug sources**: Operator precedence and short-circuit evaluation cause many bugs
- **Performance awareness**: Some operators are more efficient than others
- **Loop optimization**: Understanding when to use which loop type
- **Edge case handling**: Break/continue logic often has edge cases

---

## 3️⃣ Key Rules / Facts

**Operator Precedence** (High to Low):
1. Postfix: `expr++`, `expr--`
2. Unary: `++expr`, `--expr`, `+`, `-`, `!`, `~`
3. Multiplicative: `*`, `/`, `%`
4. Additive: `+`, `-`
5. Relational: `<`, `>`, `<=`, `>=`, `instanceof`
6. Equality: `==`, `!=`
7. Logical AND: `&&`
8. Logical OR: `||`
9. Ternary: `? :`
10. Assignment: `=`, `+=`, `-=`, etc.

**Important Rules**:
- `&&` and `||` use **short-circuit evaluation** (second operand may not be evaluated)
- `&` and `|` always evaluate both operands
- `++` and `--` have prefix (increment first) and postfix (use first, then increment) forms
- Division by zero throws `ArithmeticException` for integers, but returns `Infinity` for floats/doubles
- `%` modulo operator can work with floating-point numbers
- String `+` is concatenation, not arithmetic
- `switch` works with: byte, short, int, char, String (Java 7+), enum

**Control Flow Rules**:
- `switch` falls through unless `break` is used
- `for` loop components are all optional: `for(;;)` is valid (infinite loop)
- Enhanced for-loop cannot modify the collection being iterated
- `do-while` executes at least once (condition checked at end)
- Labels can be used with `break` and `continue` for nested loops

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Increment/Decrement Operators

```java
public class IncrementDemo {
    public static void main(String[] args) {
        int x = 5;
        
        // Postfix: use current value, then increment
        int a = x++;  // a = 5, x = 6
        System.out.println("a: " + a + ", x: " + x);  // a: 5, x: 6
        
        // Prefix: increment first, then use
        int y = 5;
        int b = ++y;  // b = 6, y = 6
        System.out.println("b: " + b + ", y: " + y);  // b: 6, y: 6
        
        // Common interview trap
        int z = 10;
        int result = z++ + ++z;  // 10 + 12 = 22
        // z becomes 11 after z++, then 12 after ++z
        System.out.println("result: " + result + ", z: " + z);  // result: 22, z: 12
    }
}
```

### Example 2: Short-Circuit Evaluation

```java
public class ShortCircuitDemo {
    public static void main(String[] args) {
        int x = 5;
        
        // && short-circuits: second condition not evaluated if first is false
        if (x < 3 && ++x > 5) {
            System.out.println("Inside if");
        }
        System.out.println("x: " + x);  // x: 5 (not incremented)
        
        // || short-circuits: second condition not evaluated if first is true
        int y = 5;
        if (y > 3 || ++y > 5) {
            System.out.println("Inside if");  // This executes
        }
        System.out.println("y: " + y);  // y: 5 (not incremented)
        
        // & doesn't short-circuit: both conditions always evaluated
        int z = 5;
        if (z < 3 & ++z > 5) {
            System.out.println("Inside if");
        }
        System.out.println("z: " + z);  // z: 6 (incremented even though first condition was false)
    }
}
```

### Example 3: Ternary Operator

```java
public class TernaryDemo {
    public static void main(String[] args) {
        int age = 20;
        
        // condition ? valueIfTrue : valueIfFalse
        String status = (age >= 18) ? "Adult" : "Minor";
        System.out.println(status);  // Adult
        
        // Nested ternary (avoid in real code - hard to read)
        int score = 85;
        String grade = (score >= 90) ? "A" : 
                       (score >= 80) ? "B" : 
                       (score >= 70) ? "C" : "F";
        System.out.println(grade);  // B
        
        // Ternary with different types (must be compatible)
        Object result = (age > 18) ? "Adult" : 100;  // Works (both are Objects)
        System.out.println(result);  // Adult
    }
}
```

### Example 4: Switch Statement (Java 8)

```java
public class SwitchDemo {
    public static void main(String[] args) {
        // Traditional switch with break
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
        
        // Fall-through example (common interview question)
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
        
        // Switch with String (Java 7+)
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

### Example 5: Loop Variations

```java
import java.util.Arrays;
import java.util.List;

public class LoopDemo {
    public static void main(String[] args) {
        // Traditional for loop
        for (int i = 0; i < 5; i++) {
            System.out.print(i + " ");  // 0 1 2 3 4
        }
        System.out.println();
        
        // While loop
        int count = 0;
        while (count < 3) {
            System.out.print(count + " ");  // 0 1 2
            count++;
        }
        System.out.println();
        
        // Do-while (executes at least once)
        int num = 10;
        do {
            System.out.print(num + " ");  // 10
            num++;
        } while (num < 10);  // Condition false, but body executed once
        System.out.println();
        
        // Enhanced for-loop (for-each)
        int[] numbers = {10, 20, 30, 40};
        for (int n : numbers) {
            System.out.print(n + " ");  // 10 20 30 40
        }
        System.out.println();
        
        // Enhanced for-loop with List
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        for (String name : names) {
            System.out.print(name + " ");  // Alice Bob Charlie
        }
        System.out.println();
        
        // Infinite loop variations
        // for (;;) { }           // infinite
        // while (true) { }       // infinite
        // do { } while (true);   // infinite
    }
}
```

### Example 6: Break and Continue with Labels

```java
public class BreakContinueDemo {
    public static void main(String[] args) {
        // Break statement
        for (int i = 0; i < 10; i++) {
            if (i == 5) {
                break;  // Exit loop when i is 5
            }
            System.out.print(i + " ");  // 0 1 2 3 4
        }
        System.out.println();
        
        // Continue statement
        for (int i = 0; i < 5; i++) {
            if (i == 2) {
                continue;  // Skip iteration when i is 2
            }
            System.out.print(i + " ");  // 0 1 3 4
        }
        System.out.println();
        
        // Labeled break (for nested loops)
        outer:
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (i == 1 && j == 1) {
                    break outer;  // Breaks out of both loops
                }
                System.out.print("(" + i + "," + j + ") ");
            }
        }
        // Output: (0,0) (0,1) (0,2) (1,0)
        System.out.println();
        
        // Labeled continue
        outer:
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (j == 1) {
                    continue outer;  // Continues outer loop
                }
                System.out.print("(" + i + "," + j + ") ");
            }
        }
        // Output: (0,0) (1,0) (2,0)
    }
}
```

### Example 7: Operator Precedence Traps

```java
public class PrecedenceDemo {
    public static void main(String[] args) {
        // Common mistake: && has higher precedence than ||
        boolean result1 = true || false && false;
        // Evaluated as: true || (false && false)
        System.out.println(result1);  // true
        
        // Arithmetic before relational
        int x = 5;
        boolean result2 = x + 2 > 6;  // (x + 2) > 6 = 7 > 6
        System.out.println(result2);  // true
        
        // Assignment has lowest precedence
        int a, b;
        a = b = 10;  // Right to left: b = 10, then a = b
        System.out.println("a: " + a + ", b: " + b);  // a: 10, b: 10
        
        // Increment operator trap
        int y = 5;
        int z = y++ * 2;  // y++ has higher precedence, but postfix uses current value
        // z = 5 * 2 = 10, then y becomes 6
        System.out.println("y: " + y + ", z: " + z);  // y: 6, z: 10
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is the difference between `++i` and `i++`?**
2. **What is short-circuit evaluation? Difference between `&&` and `&`?**
3. **What is the output of `i = i++ + ++i` if i = 5?**
4. **Can we use float or double in switch statements?**
5. **What is the difference between break and continue?**
6. **What happens if you don't use break in switch?**
7. **Can we use String in switch? Since which Java version?**
8. **What is the difference between while and do-while?**
9. **How does the ternary operator work?**
10. **Can we modify a collection while iterating with enhanced for-loop?**

---

## 6️⃣ Model Answers

**Q1: What is the difference between `++i` and `i++`?**

"The difference is in when the increment happens. With `i++` (postfix), the current value is used in the expression first, then the variable is incremented. With `++i` (prefix), the variable is incremented first, then the new value is used. For example, if i is 5, then `x = i++` sets x to 5 and i becomes 6. But `x = ++i` sets both x and i to 6."

**Q2: What is short-circuit evaluation? Difference between `&&` and `&`?**

"Short-circuit evaluation means the second operand is only evaluated if necessary. With `&&`, if the first condition is false, the second isn't checked because the result will be false anyway. Similarly, with `||`, if the first condition is true, the second isn't evaluated. The single `&` and `|` operators don't short-circuit—they always evaluate both operands. Short-circuit evaluation is more efficient and can prevent errors like null pointer exceptions."

**Q3: What is the output of `i = i++ + ++i` if i = 5?**

"This is a tricky question because it involves undefined behavior in some ways, but in Java with i = 5: First, `i++` uses 5 and then increments i to 6. Then `++i` increments i to 7 and uses 7. So the expression becomes 5 + 7 = 12, and i is assigned 12. The final value of i is 12. However, code like this should be avoided in practice as it's confusing and hard to maintain."

**Q4: Can we use float or double in switch statements?**

"No, switch statements in Java don't support float or double. They only work with byte, short, int, char, String (since Java 7), and enum types. For floating-point comparisons, you should use if-else statements instead."

**Q5: What is the difference between break and continue?**

"Break terminates the loop entirely and transfers control to the statement after the loop. Continue skips the rest of the current iteration and moves to the next iteration of the loop. For example, in a loop from 0 to 9, if you use break at 5, the loop stops. But if you use continue at 5, it just skips that iteration and continues with 6, 7, 8, 9."

**Q6: What happens if you don't use break in switch?**

"Without break, the switch statement will fall through to the next case. This means all subsequent cases will execute until a break is encountered or the switch ends. This is called fall-through behavior. While this can be useful for grouping cases together, it's often unintentional and leads to bugs, so most modern linters warn about missing breaks."

**Q7: Can we use String in switch? Since which Java version?**

"Yes, we can use String in switch statements, but only from Java 7 onwards. Before Java 7, switch only worked with byte, short, int, char, and enum. The String comparison in switch is case-sensitive and internally uses the String's hashCode and equals method for matching."

**Q8: What is the difference between while and do-while?**

"The key difference is when the condition is checked. While is an entry-controlled loop—it checks the condition before executing the loop body. So if the condition is false initially, the body never executes. Do-while is an exit-controlled loop—it checks the condition after executing the body, so the body always executes at least once. Use do-while when you want to ensure at least one execution, like validating user input."

**Q9: How does the ternary operator work?**

"The ternary operator is a shorthand for if-else and has the syntax: condition ? valueIfTrue : valueIfFalse. It evaluates the condition, and if true, returns the first value; if false, returns the second value. For example, `String status = (age >= 18) ? 'Adult' : 'Minor'`. It's useful for simple conditions but can become hard to read when nested."

**Q10: Can we modify a collection while iterating with enhanced for-loop?**

"No, you cannot modify the collection structurally while iterating with an enhanced for-loop. If you try to add or remove elements, you'll get a ConcurrentModificationException. You can modify the properties of objects in the collection, but not the collection structure itself. To modify a collection during iteration, you should use an Iterator and its remove method, or use traditional for-loop with indices."

---

## 7️⃣ Common Mistakes

1. **Confusing `==` with `equals()` for Strings**:
   ```java
   String s1 = new String("hello");
   String s2 = new String("hello");
   System.out.println(s1 == s2);        // false (different objects)
   System.out.println(s1.equals(s2));   // true (same content)
   ```

2. **Forgetting break in switch**:
   ```java
   int day = 1;
   switch(day) {
       case 1: System.out.println("Mon");
       case 2: System.out.println("Tue");  // Also prints without break!
   }
   // Output: Mon
   //         Tue
   ```

3. **Infinite loops due to wrong condition**:
   ```java
   for (int i = 0; i < 10; i--) {  // i-- instead of i++
       // Infinite loop!
   }
   ```

4. **Modifying loop variable in enhanced for-loop**:
   ```java
   int[] arr = {1, 2, 3};
   for (int num : arr) {
       num = num * 2;  // Doesn't modify array, only local copy
   }
   // arr is still {1, 2, 3}
   ```

5. **Null pointer with auto-unboxing**:
   ```java
   Integer num = null;
   if (num > 0) {  // NullPointerException! (auto-unboxing fails)
   }
   ```

6. **Semicolon after loop condition**:
   ```java
   for (int i = 0; i < 5; i++);  // Semicolon creates empty loop body
   {
       System.out.println(i);  // Separate block, not part of loop
   }
   ```

7. **Using = instead of == in conditions**:
   ```java
   int x = 5;
   if (x = 10) {  // Compilation error (assigns, doesn't compare)
       // ...
   }
   ```

8. **Assuming operator precedence**:
   ```java
   int result = 10 + 20 * 2;  // 50, not 60 (* has higher precedence)
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Short-circuit evaluation and its performance benefits
- Difference between prefix and postfix increment
- Fall-through behavior in switch
- When to use which loop type
- Operator precedence in complex expressions

**What to avoid**:
- Don't claim all operators evaluate left-to-right (precedence matters)
- Don't say "switch is faster than if-else" (depends on context)
- Don't forget that enhanced for-loop creates a copy of elements (for primitives)

**Under pressure**:
- If asked about complex expressions, work through them step-by-step
- Draw out nested loops to visualize break/continue with labels
- Remember: `&&` and `||` short-circuit, `&` and `|` don't
- When in doubt about precedence, use parentheses for clarity

**Red flags to avoid**:
- "++i is always faster than i++" (compiler optimizes this)
- "Break can only exit one loop" (labels allow breaking multiple levels)
- "Enhanced for-loop is always better" (not when you need index or modification)
- "Switch works with all types" (limited to specific types)

**Bonus points**:
- Mention that switch on String uses hashCode() for efficiency
- Discuss when `&` and `|` are useful (bitwise operations, ensuring side effects)
- Explain how enhanced for-loop works with Iterable interface
- Reference potential ConcurrentModificationException
- Mention that Java 12+ has enhanced switch expressions (but state you're discussing Java 8)
