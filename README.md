# Complete Core Java Cheat Sheet (Basic to Advanced)

## 1. Java Program Structure & Execution
Java code is compiled into bytecode (`.class`) by the compiler (`javac`) and executed by the Java Virtual Machine (JVM).

```java
public class HelloWorld {
    // Execution starting point
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

---

## 2. Data Types & String Manipulation
Java is strongly typed. 

**Primitives:** `byte` (8-bit), `short` (16-bit), `int` (32-bit), `long` (64-bit), `float` (32-bit), `double` (64-bit), `char` (16-bit Unicode), `boolean` (1-bit).

**String Handling:**
Strings are immutable in Java. For concatenation or manipulation in loops, use mutable alternatives.
* **`String`:** Stored in the String Constant Pool (Heap). Reusing literals saves memory.
* **`StringBuilder`:** Mutable, fast, but **not** thread-safe.
* **`StringBuffer`:** Mutable and thread-safe (synchronized).



---

## 3. Object-Oriented Programming (OOP)
Java revolves around objects.

**The 4 Pillars:**
1. **Encapsulation:** Wrapping data (variables) and code acting on data (methods) together. Using `private` fields with `public` getters/setters.
2. **Inheritance:** `extends` (Classes) or `implements` (Interfaces) to reuse code. Java supports single inheritance for classes, multiple for interfaces.
3. **Polymorphism:** 
    * *Compile-time:* Method Overloading (same name, different parameters).
    * *Run-time:* Method Overriding (subclass provides specific implementation of superclass method).
4. **Abstraction:** Hiding implementation details via `abstract` classes or `interface`s.

**Advanced OOP Constructs:**
* **Enums:** Special classes representing a group of constants. Can have fields, methods, and constructors.
* **Annotations:** Metadata tags (e.g., `@Override`, `@FunctionalInterface`, `@Deprecated`) used by the compiler or at runtime via Reflection.

---

## 4. Exception Handling
Handling runtime disruptions gracefully.

```java
try {
    // Risky code
} catch (SpecificException e) {
    // Handle specific
} catch (Exception e) {
    // Handle generic
} finally {
    // Always executes (used to close resources)
}
```

* **Checked vs. Unchecked:** Checked exceptions (e.g., `IOException`) are checked at compile-time. Unchecked exceptions (`RuntimeException`, e.g., `NullPointerException`) happen at runtime.
* **Try-With-Resources (Java 7+):** Automatically closes resources that implement `AutoCloseable`.
* **`throw` vs. `throws`:**
    * **`throw`:** Used *inside* a method body to explicitly throw a single exception instance (e.g., `throw new IllegalArgumentException("Invalid input");`).
    * **`throws`:** Used in a method *signature* to declare that the method might throw one or more exceptions, passing the responsibility of handling them to the caller (e.g., `public void readFile() throws IOException { ... }`).

---

## 5. Collections Framework & Generics
A unified architecture for storing and manipulating groups of objects.



| Interface | Implementations | Key Characteristics |
| :--- | :--- | :--- |
| **List** | `ArrayList`, `LinkedList` | Ordered, allows duplicates. `ArrayList` is fast for reads; `LinkedList` for inserts/deletes. |
| **Set** | `HashSet`, `TreeSet` | Unordered, no duplicates. `TreeSet` keeps elements sorted. |
| **Map** | `HashMap`, `TreeMap` | Key-Value pairs. `HashMap` is fast (O(1)); `TreeMap` is sorted by keys. |

**Generics (`<T>`):**
Provides compile-time type safety and eliminates the need for casting.
* **Wildcards:** `?` (Unknown), `? extends T` (Upper bound - read-only), `? super T` (Lower bound - write-only).
* **Type Erasure:** Generics are removed by the compiler; the JVM only sees `Object`.

---

## 6. JVM Architecture & Memory Management
Understanding what happens under the hood.



* **ClassLoaders:** Load `.class` files into memory (Bootstrap -> Platform -> Application).
* **Heap:** Where objects live. Managed by the Garbage Collector (GC).
* **Stack:** Where local variables and method calls are stored. Each thread gets its own Stack.
* **Garbage Collection (GC):** Automatically reclaims memory by deleting unreachable objects (e.g., G1 GC, ZGC).



---

## 7. The Java Memory Model (JMM)
Crucial for understanding how threads interact with memory.



* **`volatile` Keyword:** Forces threads to read the variable's value directly from main memory, bypassing the CPU cache. Guarantees visibility across threads.
* **`transient` Keyword:** Excludes a variable from being serialized.
* **Happens-Before:** The JMM guarantee that if action A happens-before action B, A's results are visible to B.

---

## 8. Multithreading & Concurrency
Moving from basic threads to advanced synchronization.

**Basics:**
* Implementing `Runnable` or extending `Thread`.
* `synchronized` blocks/methods to prevent race conditions.

**Advanced (`java.util.concurrent`):**
* **ExecutorService:** Thread pool management (e.g., `Executors.newFixedThreadPool(10)`).
* **Callable & Future:** Like `Runnable`, but returns a result or throws an exception.
* **Locks:** `ReentrantLock`, `ReadWriteLock` for finer control than `synchronized`.
* **Synchronizers:** `CountDownLatch`, `CyclicBarrier` for orchestrating multiple threads.
* **Concurrent Collections:** `ConcurrentHashMap` (lock stripping for high throughput), `CopyOnWriteArrayList`.
* **Atomic Variables:** `AtomicInteger` (uses lock-free CAS - Compare-And-Swap operations).

---

## 9. File I/O, NIO & Serialization
* **`java.io`:** Stream-based, blocking I/O (`FileInputStream`, `BufferedReader`).
* **`java.nio` (New I/O):** Channel and Buffer-based, non-blocking. Better for high-performance server applications.
* **Serialization:** Converting an object to a byte stream (`implements Serializable`). `serialVersionUID` is crucial for version control during deserialization.

---

## 10. Modern Java Features (Java 8 to 21)
Modern paradigms that shifted how Java is written.

**Java 8:**
* **Lambdas:** `(a, b) -> a + b` (Functional programming).
* **Streams API:** Declarative data processing (`list.stream().filter(x -> x > 10).map(String::valueOf).toList();`).
* **Optional:** Avoids `NullPointerException`.
* **CompletableFuture:** Asynchronous, non-blocking task chains.

**Java 9 - 17:**
* **Modules (JPMS):** `module-info.java` for stronger encapsulation.
* **`var` (Java 10):** Local variable type inference.
* **Text Blocks (Java 15):** Multi-line strings using `"""`.
* **Records (Java 16):** Immutable data carriers (`public record User(String name, int age) {}`).
* **Sealed Classes (Java 17):** Restricts which classes can extend them.

**Java 21:**
* **Pattern Matching for `switch`:** Switching over object types natively.
* **Virtual Threads:** Lightweight threads managed by the JVM (Project Loom), allowing millions of concurrent threads without OS overhead.
