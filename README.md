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



### Collections Interfaces & Implementations
| Interface | Implementations | Time Complexity (Avg) | Key Characteristics |
| :--- | :--- | :--- | :--- |
| **List** | `ArrayList`, `LinkedList`, `Vector` | Read: O(1) for AL, O(n) for LL | Ordered, allows duplicates. `ArrayList` (dynamic array) is fast for random access; `LinkedList` (doubly linked) is fast for inserts/deletes at ends. |
| **Set** | `HashSet`, `LinkedHashSet`, `TreeSet` | Add/Contains: O(1) for HS, O(log n) for TS | No duplicates. `HashSet` is unordered. `LinkedHashSet` maintains insertion order. `TreeSet` (Red-Black tree) keeps elements sorted naturally or via Comparator. |
| **Map** | `HashMap`, `LinkedHashMap`, `TreeMap`, `Hashtable` | Get/Put: O(1) for HM, O(log n) for TM | Key-Value pairs. `HashMap` allows one null key. `TreeMap` is sorted by keys. `LinkedHashMap` maintains insertion/access order. |
| **Queue/Deque** | `PriorityQueue`, `ArrayDeque` | Offer/Poll: O(log n) for PQ, O(1) for AD | `PriorityQueue` orders elements by priority (min/max heap). `ArrayDeque` is a fast double-ended queue (better than Stack/LinkedList). |

### Crucial Collection Concepts & Internals
* **`equals()` & `hashCode()` Contract:** If two objects are equal (`equals()` returns true), they *must* have the same `hashCode()`. Essential for Hash-based collections to locate the correct bucket.
* **`HashMap` Internal Working (Java 8+):** * Backed by an array of Nodes (buckets). Calculates the array index using bitwise operations: `(n - 1) & hash`.
    * **Collision Resolution:** When hashes collide, elements are stored in a LinkedList. If a bucket's list size exceeds 8 (and table size $\ge$ 64), it converts to a **Red-Black Tree** (Treeification) to improve worst-case search time from O(n) to O(log n).
    * **Resizing:** Default initial capacity is 16, default load factor is 0.75. When capacity reaches 75%, it doubles in size and rehashing occurs.
* **Fail-Fast vs. Fail-Safe Iterators:** * *Fail-Fast:* Throws `ConcurrentModificationException` if a collection is modified structurally during iteration (e.g., standard `ArrayList` Iterator using `modCount`).
    * *Fail-Safe:* Operates on a clone or uses lock-free structures, avoiding the exception (e.g., `ConcurrentHashMap`, `CopyOnWriteArrayList`).
* **Sorting (`Comparable` vs `Comparator`):** * `Comparable<T>`: Implemented by the element class itself (defines default natural sorting via the `compareTo()` method).
    * `Comparator<T>`: External sorting logic (defines custom sorting via the `compare()` method), often passed as a lambda: `list.sort((a, b) -> a.age - b.age);`.



### Generics (`<T>`)
Provides compile-time type safety, eliminating the need for manual casting and `ClassCastException`s at runtime.

* **Type Parameters:** `<T>` (Type), `<E>` (Element), `<K, V>` (Key, Value).
* **Wildcards & Bounds:** * `?` (Unbounded wildcard): Any type.
    * `? extends T` (Upper Bound): Any class that is or extends `T`.
    * `? super T` (Lower Bound): Any class that is or is a superclass of `T`.
* **The PECS Principle (Producer `extends`, Consumer `super`):** * Use `? extends T` if you only need to **read** from a collection (it produces `T`).
    * Use `? super T` if you only need to **write** to a collection (it consumes `T`).
* **Type Erasure:** Generics are strictly a compile-time concept. The compiler replaces generic type parameters with `Object` (or their highest bound) and inserts necessary casts. The JVM has no knowledge of generic types at runtime (hence you cannot do `new T()` or `instanceof T`).
* **Bridge Methods:** Synthetic methods generated by the compiler during type erasure to preserve polymorphism when extending generic classes.

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
