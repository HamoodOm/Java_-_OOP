# Methods and Pass-by-Value

## 1️⃣ Concept Overview

**Methods** are blocks of code that perform specific tasks and can be called/invoked when needed. They promote code reusability and organization.

**Method Structure**:
```
accessModifier returnType methodName(parameterList) {
    // method body
    return value; // if returnType is not void
}
```

**Pass-by-Value**: Java is **strictly pass-by-value**. This is one of the most misunderstood concepts in Java.

- For **primitives**: A copy of the value is passed
- For **objects**: A copy of the reference (memory address) is passed, **not** the object itself

**Important**: You cannot change what a reference points to from inside a method, but you CAN modify the object's state through the reference.

---

## 2️⃣ Why Interviewers Ask This

- **Tests fundamental understanding**: Many developers incorrectly think Java is pass-by-reference
- **Memory model knowledge**: Shows if you understand how Java manages memory
- **Common bug source**: Misunderstanding this leads to bugs with object modifications
- **Separates intermediate from advanced**: This is a key distinction that experienced developers know
- **Design implications**: Affects how you design APIs and method signatures

---

## 3️⃣ Key Rules / Facts

**Method Rules**:
- Method name must follow identifier rules (camelCase convention)
- Return type is mandatory (use `void` if nothing is returned)
- Parameters are optional (can have zero parameters)
- Methods can be overloaded (same name, different parameters)
- Static methods belong to class; instance methods belong to objects
- `return` statement immediately exits the method

**Pass-by-Value Rules**:
- Java **ALWAYS** passes by value, never by reference
- For primitives: The actual value is copied
- For objects: The reference value (memory address) is copied
- You cannot reassign a reference parameter to point to a different object
- You can modify the object's state through the reference
- Arrays are objects, so they follow object reference rules

**Method Overloading Rules**:
- Same method name, different parameter list (type, number, or order)
- Return type alone is NOT sufficient for overloading
- Access modifier can be different
- Compiler chooses method at compile-time based on arguments
- More specific method is chosen over less specific (widening vs autoboxing)

**Variable Arguments (Varargs)**:
- Syntax: `type... variableName`
- Must be the last parameter
- Treated as an array inside the method
- Can only have one varargs parameter per method

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Pass-by-Value with Primitives

```java
public class PassByValuePrimitive {
    public static void modifyPrimitive(int num) {
        num = 100;  // Changes local copy only
        System.out.println("Inside method: " + num);  // 100
    }
    
    public static void main(String[] args) {
        int value = 10;
        modifyPrimitive(value);
        System.out.println("After method: " + value);  // 10 (unchanged)
        
        // Explanation: A copy of 'value' is passed to the method.
        // The method modifies its local copy, not the original.
    }
}
```

**Output**:
```
Inside method: 100
After method: 10
```

### Example 2: Pass-by-Value with Objects (The Tricky Part)

```java
class Person {
    String name;
    
    Person(String name) {
        this.name = name;
    }
}

public class PassByValueObject {
    // Modifying object's state - WORKS
    public static void modifyObjectState(Person p) {
        p.name = "Modified";  // Changes the object's state
        System.out.println("Inside method: " + p.name);  // Modified
    }
    
    // Reassigning reference - DOES NOT WORK
    public static void reassignReference(Person p) {
        p = new Person("New Person");  // Changes local copy of reference only
        System.out.println("Inside method: " + p.name);  // New Person
    }
    
    public static void main(String[] args) {
        // Test 1: Modifying object state
        Person person1 = new Person("John");
        modifyObjectState(person1);
        System.out.println("After modifyObjectState: " + person1.name);  // Modified
        
        System.out.println("---");
        
        // Test 2: Reassigning reference
        Person person2 = new Person("Alice");
        reassignReference(person2);
        System.out.println("After reassignReference: " + person2.name);  // Alice (unchanged!)
        
        /* 
         * Explanation:
         * - person1 and p (in method) both point to same object initially
         * - Modifying through p affects the actual object
         * - Reassigning p creates a new object, but person1 still points to original
         * - The reference value is copied, not the reference itself
         */
    }
}
```

**Output**:
```
Inside method: Modified
After modifyObjectState: Modified
---
Inside method: New Person
After reassignReference: Alice
```

### Example 3: Pass-by-Value with Arrays

```java
public class PassByValueArray {
    public static void modifyArray(int[] arr) {
        arr[0] = 999;  // Modifies original array
        System.out.println("Inside method, arr[0]: " + arr[0]);  // 999
    }
    
    public static void reassignArray(int[] arr) {
        arr = new int[]{100, 200, 300};  // New array, doesn't affect original
        System.out.println("Inside method, arr[0]: " + arr[0]);  // 100
    }
    
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3};
        
        modifyArray(numbers);
        System.out.println("After modifyArray, numbers[0]: " + numbers[0]);  // 999
        
        reassignArray(numbers);
        System.out.println("After reassignArray, numbers[0]: " + numbers[0]);  // 999 (unchanged)
    }
}
```

**Output**:
```
Inside method, arr[0]: 999
After modifyArray, numbers[0]: 999
Inside method, arr[0]: 100
After reassignArray, numbers[0]: 999
```

### Example 4: Method Overloading

```java
public class MethodOverloading {
    // Overloaded methods: same name, different parameters
    
    public int add(int a, int b) {
        return a + b;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public String add(String a, String b) {
        return a + b;
    }
    
    public static void main(String[] args) {
        MethodOverloading obj = new MethodOverloading();
        
        System.out.println(obj.add(5, 10));           // 15 (int version)
        System.out.println(obj.add(5.5, 10.5));       // 16.0 (double version)
        System.out.println(obj.add(1, 2, 3));         // 6 (three int version)
        System.out.println(obj.add("Hello", "World")); // HelloWorld (String version)
    }
}
```

### Example 5: Variable Arguments (Varargs)

```java
public class VarargsDemo {
    // Varargs: can accept 0 or more arguments
    public static int sum(int... numbers) {
        int total = 0;
        for (int num : numbers) {
            total += num;
        }
        return total;
    }
    
    // Varargs must be last parameter
    public static void display(String message, int... numbers) {
        System.out.print(message + ": ");
        for (int num : numbers) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        System.out.println(sum());              // 0 (no arguments)
        System.out.println(sum(5));             // 5 (one argument)
        System.out.println(sum(1, 2, 3, 4));   // 10 (multiple arguments)
        
        // Can also pass an array
        int[] nums = {10, 20, 30};
        System.out.println(sum(nums));          // 60
        
        display("Numbers", 1, 2, 3, 4, 5);     // Numbers: 1 2 3 4 5
    }
}
```

### Example 6: Return Types and Method Chaining

```java
class Calculator {
    private int value;
    
    public Calculator setValue(int value) {
        this.value = value;
        return this;  // Return 'this' for method chaining
    }
    
    public Calculator add(int num) {
        this.value += num;
        return this;
    }
    
    public Calculator multiply(int num) {
        this.value *= num;
        return this;
    }
    
    public int getValue() {
        return this.value;
    }
}

public class MethodChaining {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        // Method chaining (fluent interface)
        int result = calc.setValue(10)
                         .add(5)
                         .multiply(2)
                         .getValue();
        
        System.out.println(result);  // 30 [(10 + 5) * 2]
    }
}
```

### Example 7: Static vs Instance Methods

```java
class MathUtils {
    // Static method - belongs to class
    public static int square(int num) {
        return num * num;
    }
    
    // Instance method - belongs to object
    private int multiplier = 2;
    
    public int multiply(int num) {
        return num * multiplier;
    }
}

public class StaticVsInstance {
    public static void main(String[] args) {
        // Static method - called on class
        int squared = MathUtils.square(5);
        System.out.println(squared);  // 25
        
        // Instance method - requires object
        MathUtils utils = new MathUtils();
        int multiplied = utils.multiply(5);
        System.out.println(multiplied);  // 10
        
        // Static method can also be called on object (not recommended)
        // int squared2 = utils.square(5);  // Works but confusing
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **Is Java pass-by-value or pass-by-reference?**
2. **Explain with an example why Java is pass-by-value for objects.**
3. **Can you change the value of a primitive passed to a method?**
4. **What is method overloading? What are the rules?**
5. **Can we overload methods by changing only the return type?**
6. **What is the difference between static and instance methods?**
7. **What are varargs? What are the rules for using them?**
8. **Can a method have multiple return statements?**
9. **What happens if you don't return a value in a non-void method?**
10. **Why can't you modify what a reference parameter points to?**

---

## 6️⃣ Model Answers

**Q1: Is Java pass-by-value or pass-by-reference?**

"Java is strictly pass-by-value, always. For primitives, the actual value is copied and passed. For objects, the value of the reference—which is the memory address—is copied and passed. This means you receive a copy of the reference, not the reference itself. So you can modify the object's state through the reference, but you cannot make the original reference point to a different object."

**Q2: Explain with an example why Java is pass-by-value for objects.**

"Let me give you a clear example. If I have a Person object and pass it to a method, both the original variable and the parameter point to the same object. If the method modifies the object's properties, those changes are visible because they're modifying the same object. However, if the method tries to reassign the parameter to a new object with 'p = new Person()', this only changes the local copy of the reference. The original variable still points to the original object. If Java were pass-by-reference, reassigning the parameter would also change what the original variable points to, but it doesn't."

**Q3: Can you change the value of a primitive passed to a method?**

"No, you cannot change the original primitive value from inside a method. When you pass a primitive to a method, a copy of that value is created. Any modifications inside the method only affect the local copy, not the original variable. The original variable remains unchanged after the method returns."

**Q4: What is method overloading? What are the rules?**

"Method overloading is having multiple methods with the same name but different parameter lists in the same class. The parameter list can differ in the number of parameters, the types of parameters, or their order. The return type alone is not sufficient for overloading. The compiler determines which method to call based on the arguments provided at compile-time. This is also called compile-time polymorphism or static polymorphism."

**Q5: Can we overload methods by changing only the return type?**

"No, you cannot overload methods by changing only the return type. The method signature includes the method name and parameter list, but not the return type. If two methods have the same name and parameters but different return types, the compiler won't be able to distinguish between them based on the method call, resulting in a compilation error. However, you can have different return types as long as the parameter lists are different."

**Q6: What is the difference between static and instance methods?**

"Static methods belong to the class and can be called without creating an object. They can only access static members directly. Instance methods belong to objects and require an object to be called. They can access both static and instance members. Static methods are useful for utility functions that don't need object state, while instance methods typically work with object-specific data. Static methods cannot use 'this' keyword because they're not tied to any instance."

**Q7: What are varargs? What are the rules for using them?**

"Varargs, or variable arguments, allow a method to accept zero or more arguments of a specified type. They're declared using three dots after the type, like 'int... numbers'. Inside the method, varargs are treated as an array. The key rules are: varargs must be the last parameter in the method signature, you can only have one varargs parameter per method, and you can pass zero or more arguments or even an array directly."

**Q8: Can a method have multiple return statements?**

"Yes, a method can have multiple return statements, but only one will execute for any given method call. This is commonly used in conditional logic where different paths return different values. However, all possible execution paths must end with a return statement for non-void methods, or the compiler will throw an error about missing return statements."

**Q9: What happens if you don't return a value in a non-void method?**

"If a non-void method doesn't return a value in all possible execution paths, the code won't compile. The compiler performs flow analysis to ensure every path through the method ends with a return statement of the correct type. This is a compile-time error, not a runtime error, which helps catch bugs early."

**Q10: Why can't you modify what a reference parameter points to?**

"Because Java passes the value of the reference, not the reference itself. When you pass an object to a method, you're passing a copy of the memory address. The method parameter and the original variable both point to the same object, but they're separate variables. If you reassign the parameter to point to a new object, you're only changing the local copy of the reference. The original variable still holds the original reference value. It's like having two remote controls pointing at the same TV—changing which TV one remote points to doesn't affect the other remote."

---

## 7️⃣ Common Mistakes

1. **Thinking Java is pass-by-reference for objects**:
   ```java
   void swap(Person p1, Person p2) {
       Person temp = p1;
       p1 = p2;
       p2 = temp;
   }
   // This doesn't swap the original references!
   ```

2. **Trying to overload based on return type only**:
   ```java
   int getValue() { return 10; }
   String getValue() { return "10"; }  // Compilation error!
   ```

3. **Forgetting that arrays are objects**:
   ```java
   void modify(int[] arr) {
       arr = new int[]{100};  // Doesn't change original array reference
   }
   ```

4. **Placing varargs before other parameters**:
   ```java
   void method(int... nums, String str) { }  // Compilation error!
   // Correct: void method(String str, int... nums) { }
   ```

5. **Missing return in all code paths**:
   ```java
   int getValue(boolean flag) {
       if (flag) {
           return 10;
       }
       // Compilation error: missing return statement
   }
   ```

6. **Assuming method can modify primitive parameter**:
   ```java
   void increment(int x) {
       x++;  // Only modifies local copy
   }
   ```

7. **Confusing static method context**:
   ```java
   class MyClass {
       int instanceVar = 10;
       
       static void staticMethod() {
           System.out.println(instanceVar);  // Compilation error!
           // Static methods can't access instance variables directly
       }
   }
   ```

8. **Null reference parameter**:
   ```java
   void modify(Person p) {
       p.name = "John";  // NullPointerException if p is null
   }
   // Always check for null if parameter might be null
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Java is ALWAYS pass-by-value (this is critical)
- For objects, we pass the value of the reference, not the reference itself
- You can modify object state but not what the reference points to
- Draw diagrams showing memory/references if needed
- Method overloading is compile-time polymorphism

**What to avoid**:
- Never say "Java is pass-by-reference for objects"
- Don't confuse modifying object state with changing the reference
- Don't claim return type is part of method signature for overloading
- Avoid saying "varargs is an array" (it's treated as one, but syntax differs)

**Under pressure**:
- If asked about pass-by-value vs pass-by-reference, draw two boxes: one for the original variable, one for the method parameter, both pointing to the same object
- For swap example, explain why it doesn't work (changes local copies only)
- Use the phrase "copy of the reference" to clarify object passing

**Red flags to avoid**:
- "Java has pass-by-reference" (incorrect)
- "Objects are passed by reference" (imprecise—values of references are passed)
- "You can't modify objects in methods" (you can modify state)
- "Static methods are faster" (not a design consideration)

**Bonus points**:
- Mention that String is immutable, so modifications create new objects
- Discuss method overloading vs overriding (overloading is compile-time, overriding is runtime)
- Reference varargs ambiguity when overloading methods
- Explain that wrapper classes are immutable like String
- Mention the StringBuilder example where you CAN modify inside methods (because you're modifying object state)
