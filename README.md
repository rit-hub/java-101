# Core Java Cheat Sheet

## 1. Java Program Structure
Every Java application begins with a class definition and a `main` method. 

```java
public class HelloWorld {
    // Execution starts here
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

---

## 2. Data Types
Java is strictly typed. Data types are split into Primitives (stored directly in memory) and Non-Primitives (references to objects).

| Category | Type | Size | Description/Range |
| :--- | :--- | :--- | :--- |
| **Primitive** | `byte` | 8-bit | -128 to 127 |
| | `short` | 16-bit | -32,768 to 32,767 |
| | `int` | 32-bit | Default for whole numbers |
| | `long` | 64-bit | Append 'L' (e.g., `100L`) |
| | `float` | 32-bit | Append 'f' (e.g., `10.5f`) |
| | `double` | 64-bit | Default for decimal numbers |
| | `char` | 16-bit | Single Unicode character (e.g., `'A'`) |
| | `boolean` | 1-bit | `true` or `false` |
| **Non-Primitive** | `String` | Varies | Sequence of characters (Immutable) |
| | `Arrays` | Varies | Fixed-size collection of similar types |
| | `Classes/Objects`| Varies | User-defined blueprints and instances |

---

## 3. Operators & Control Flow

**Key Operators:**
* **Arithmetic:** `+`, `-`, `*`, `/`, `%`
* **Logical:** `&&` (AND), `||` (OR), `!` (NOT)
* **Relational:** `==`, `!=`, `>`, `<`, `>=`, `<=`
* **Ternary:** `condition ? ifTrue : ifFalse`

**Control Statements:**
* **`if-else`:** Executes a block if a condition is true.
* **`switch`:** Tests a variable against multiple cases (supports Strings, Enums, and pattern matching in Java 21+).
* **`for` loop:** Used when the number of iterations is known.
* **`while` loop:** Runs as long as a condition is true.
* **`do-while` loop:** Executes the block at least once before checking the condition.

---

## 4. Object-Oriented Programming (OOP)
Java is fundamentally built on objects and classes. 

**The 4 Pillars of OOP:**
* **Encapsulation:** Hiding internal state and requiring all interaction to be performed through an object's methods (using `private` fields and `public` getters/setters).
* **Inheritance:** Acquiring properties of another class using the `extends` keyword to promote code reusability.
* **Polymorphism:** The ability of a single action to behave differently. Achieved via **Compile-time** (Method Overloading) and **Run-time** (Method Overriding).
* **Abstraction:** Hiding complex implementation details and showing only the essential features using `abstract` classes or `interface` types.

**Access Modifiers:**
* **`private`:** Accessible only within the same class.
* **`default` (no keyword):** Accessible within the same package.
* **`protected`:** Accessible within the same package and subclasses.
* **`public`:** Accessible from anywhere.

---

## 5. Exception Handling
Exceptions disrupt the normal flow of the program. Java uses a robust system to catch and handle them.

```java
try {
    // Code that may throw an exception
    int result = 10 / 0;
} catch (ArithmeticException e) {
    // Block to handle the specific exception
    System.out.println("Cannot divide by zero.");
} finally {
    // Executes regardless of whether an exception occurred
    System.out.println("Cleanup operations go here.");
}
```
**Important Keywords:**
* **`throw`:** Used to explicitly throw an exception.
* **`throws`:** Used in a method signature to declare that it might throw an exception.

---

## 6. Collections Framework
The Collections framework provides an architecture to store and manipulate groups of objects. 



| Interface | Implementations | Key Characteristics |
| :--- | :--- | :--- |
| **List** | `ArrayList`, `LinkedList` | Ordered collection, allows duplicate elements. |
| **Set** | `HashSet`, `TreeSet` | Unordered collection, does not allow duplicates. |
| **Map** | `HashMap`, `TreeMap` | Stores Key-Value pairs, keys must be unique. |
| **Queue** | `PriorityQueue`, `ArrayDeque`| Follows FIFO (First-In-First-Out) principle. |

---

## 7. Memory Management & JVM
Java handles memory automatically, primarily dividing it into two main areas.



* **Heap:** Where all objects and their instance variables are allocated. This is the area managed by the Garbage Collector (GC).
* **Stack:** Where method executions and local variables are stored. Each thread has its own stack.
* **Garbage Collection:** An automatic process that reclaims memory by deleting unreachable objects (e.g., G1 GC, ZGC).

---

## 8. Modern Java Features (Java 8 to Java 21)
Java has evolved significantly to become more concise and powerful. 

**Java 8:**
* **Lambda Expressions:** `(a, b) -> a + b` provides a clear and concise way to represent a method interface using an expression.
* **Streams API:** Allows functional-style operations on collections. Example: `list.stream().filter(x -> x > 10).collect(Collectors.toList());`
* **Optional:** A container object used to avoid `NullPointerException`.

**Java 11 - 16:**
* **`var` Keyword:** Local variable type inference (Java 10/11).
* **Text Blocks:** Multi-line string literals using `"""` (Java 15).
* **Records:** A concise syntax to create immutable data carrier classes (Java 16). 
    * *Example:* `public record User(String name, int age) {}`

**Java 17 - 21:**
* **Sealed Classes:** Restricts which other classes or interfaces may extend or implement them (Java 17).
* **Pattern Matching for `switch`:** Simplifies complex conditional logic based on object types (Java 21).
* **Virtual Threads:** Lightweight threads that dramatically reduce the effort of writing, maintaining, and observing high-throughput concurrent applications (Java 21).

---

## 9. Multithreading & Concurrency
Java supports concurrent execution out of the box, allowing multiple tasks to run simultaneously.

* **Creating a Thread:** Implement the `Runnable` interface or extend the `Thread` class.
* **ExecutorService:** A higher-level replacement for working with threads directly, managing a pool of worker threads.
* **Synchronization:** Using the `synchronized` keyword locks a method or block to prevent thread interference and memory consistency errors.
