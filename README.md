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

## 8. Multithreading & Concurrency (0–2 Yrs Exp Focus)
For early-career developers, the focus is on understanding thread safety, preventing race conditions, and knowing when to use modern concurrency utilities instead of raw threads.

### The Core Basics
* **Thread Lifecycle:** 1. **New:** Created but `start()` not yet called.
  2. **Runnable:** Ready to run, waiting for CPU time.
  3. **Running:** Executing the `run()` method.
  4. **Blocked/Waiting:** Waiting for a lock or a signal to continue.
  5. **Terminated:** Execution finished.

  

* **Creating Threads:**
    * **`extends Thread`:** Not recommended. Tightly couples code and prevents extending other classes.
    * **`implements Runnable`:** The standard approach. Decouples the task from the thread.
* **Thread Safety & `synchronized`:** * When multiple threads access shared data, you get **race conditions**.
    * The `synchronized` keyword guarantees that only one thread can execute a method or block at a time, using the object's intrinsic lock (monitor).
    * *Best Practice:* Prefer `synchronized(this) { ... }` blocks over synchronized methods to lock only the critical section of code, improving performance.
* **Deadlock:** Occurs when two or more threads are blocked forever, each waiting for a lock held by the other. Always acquire locks in a consistent order to prevent this.

### Modern Concurrency: `java.util.concurrent`
In modern Java, you should almost never use `new Thread().start()`. Instead, use the Concurrency API.

#### 1. Executors & Thread Pools
Creating threads is expensive for the OS. Thread pools manage and reuse a set number of worker threads.
* **`ExecutorService`:** The interface used to manage pools.
    * `Executors.newFixedThreadPool(n)`: Creates a pool with a fixed number of threads. Great for limiting resource usage.
    * `Executors.newCachedThreadPool()`: Creates new threads as needed and reuses idle ones. Good for short-lived, asynchronous tasks.
* **`Runnable` vs. `Callable`:** * `Runnable` performs a task but cannot return a result or throw checked exceptions.
    * `Callable<T>` returns a result of type `T` and can throw checked exceptions.
* **`Future<T>`:** When you submit a `Callable` to an `ExecutorService`, it returns a `Future`. You use `future.get()` to retrieve the result (this will block until the thread finishes its task).

#### 2. Concurrent Collections
Standard collections (`ArrayList`, `HashMap`) are not thread-safe. Old legacy classes (`Vector`, `Hashtable`) use heavy synchronization that kills performance.
* **`ConcurrentHashMap`:** The modern standard for thread-safe maps. It allows high concurrency by locking only portions of the map during updates, rather than the whole object.
* **`CopyOnWriteArrayList`:** Thread-safe `List`. It creates a fresh copy of the underlying array for every write operation (`add`, `set`). It is heavily optimized for scenarios where you read often but write rarely.

#### 3. Simple Thread-Safe Utilities
* **Atomic Variables (`AtomicInteger`, `AtomicLong`):** If you just need a thread-safe counter (e.g., `count++`), do not use `synchronized`. Use `AtomicInteger.incrementAndGet()`. It is much faster and handles the thread safety under the hood.
* **`ThreadLocal`:** Creates variables that can only be read and written by the same thread. Useful for keeping non-thread-safe objects (like `SimpleDateFormat`) safe without using locks.
---

## 9. File I/O, NIO & Serialization (0–2 Yrs Exp Focus)
Understanding how Java interacts with files, networks, and object states is a core interview topic. 

### 1. Traditional I/O (`java.io`)
Traditional I/O is **Stream-based** and **Blocking**. A stream represents a continuous flow of data. If a thread reads from a stream and no data is available, the thread blocks (waits) until data arrives.



Interviewers typically ask you to distinguish between the two main types of streams:
* **Byte Streams (1-byte at a time):** Used for reading/writing raw binary data (e.g., images, PDFs, audio files).
    * *Base Classes:* `InputStream`, `OutputStream`
    * *Common Implementations:* `FileInputStream`, `FileOutputStream`
* **Character Streams (2-bytes at a time):** Used for reading/writing text data. They automatically handle character encoding (like UTF-8).
    * *Base Classes:* `Reader`, `Writer`
    * *Common Implementations:* `FileReader`, `FileWriter`
* **Buffered Streams (Crucial for Performance):** Reading one byte/character at a time directly from the disk is incredibly slow. `BufferedReader` and `BufferedInputStream` read large chunks of data into memory at once, drastically reducing expensive OS-level read operations.
    * *Pro-tip:* Always wrap standard readers in buffered readers. (e.g., `BufferedReader br = new BufferedReader(new FileReader("file.txt"));`).

### 2. New I/O (`java.nio` & NIO.2)
Introduced to solve the performance bottlenecks of traditional I/O in high-load server environments.
* **Core Differences:** While `java.io` reads data sequentially (Stream-oriented), `java.nio` reads chunks of data into memory blocks (Buffer-oriented) via two-way pathways called **Channels**. It supports **Non-blocking** operations.
* **NIO.2 (`java.nio.file`):** This is what you will actually use day-to-day. Introduced in Java 7, it provides modern, simplified utility classes to handle file operations.
    * **`Path` Interface:** The modern replacement for the old `java.io.File` class. (e.g., `Path path = Paths.get("config.txt");`).
    * **`Files` Utility Class:** Contains static methods that make reading/writing files a one-liner, which interviewers love to see.
        * *Example:* `List<String> lines = Files.readAllLines(Path.of("file.txt"));`

### 3. Serialization & Deserialization
A massive topic for junior-level interviews.



* **Serialization:** The process of converting an object's state (its instance variables) into a byte stream so it can be saved to a database, written to a file, or sent over a network.
* **Deserialization:** The reverse process of recreating the object in memory from the byte stream.
* **How to implement:** A class must implement the `java.io.Serializable` interface. 
    * *Note:* This is a **Marker Interface**—it has no methods to implement. It simply tells the JVM, "This object is allowed to be serialized."
* **The `transient` Keyword:** If you have sensitive data (like a `password` field) or data that shouldn't be saved, mark it as `transient`. During serialization, the JVM will ignore it, and upon deserialization, it will be assigned its default value (e.g., `null` for objects, `0` for ints).
* **`serialVersionUID`:** A unique version ID assigned to a Serializable class. 
    * *Why it matters:* If you serialize a `User` object, save it to a file, and then later add a new field to the `User` class (changing its structure), the `serialVersionUID` changes. When you try to deserialize that old file, the JVM checks the IDs. Since they no longer match, it throws an `InvalidClassException`. Explicitly defining this ID prevents arbitrary breaking changes.

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
