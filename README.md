# Java / Spring Boot Interview Questions.

# Java

- What is Relaxed Binding in Spring Boot ?
    
    <aside>
    🔥
    
    Relaxed binding in Spring Boot refers to the framework's ability to flexibly bind external configuration properties to Java classes. It allows property names to be specified in various formats (e.g., camelCase, kebab-case, snake_case, uppercase with underscores) and automatically maps them to the corresponding fields in the configuration classes, without requiring an exact match. 
    
    </aside>
    
- Sorting Algorithms
    
    ![1706557513963.gif](assets/1706557513963.gif)
    
- If a static block throws an exception , what happens to the class loading process ?
    
    When a **static block** in a Java class throws an exception, the **class loading process fails**, and the class will not be loaded into memory. Here's a detailed breakdown:
    
    ### **Key Points:**
    
    1. **Class Loading & Initialization:**
        - When a class is referenced for the first time, the JVM loads it and initializes it.
        - The initialization phase executes static blocks and initializes static variables.
    2. **Exception in Static Block:**
        - If a checked or unchecked exception is thrown inside a static block, the class initialization process **fails**.
        - The **`ExceptionInInitializerError`** is thrown, wrapping the original exception.
    3. **Impact on Class Usage**
        - The class remains **unloaded**, meaning you cannot create instances of it or access any static members.
        - Any subsequent attempt to reference the class will result in a **`NoClassDefFoundError`**.
        
        ### **Important Errors:**
        
        1. **`ExceptionInInitializerError`**:
            - This wraps the original exception that occurred in the static block.
            - It prevents the class from being loaded successfully.
        2. **`NoClassDefFoundError`**:
            - If the class was already referenced earlier and failed to initialize, future attempts to use it result in this error.
    
    ### **Example Code:**
    
    ```java
    class Test {
        static {
            System.out.println("Static block executing...");
            int x = 10 / 0;  // This will cause an exception
        }
    
        public static void display() {
            System.out.println("Display method called");
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            try {
                Test.display(); // Triggers class loading
            } catch (Throwable e) {
                e.printStackTrace();
            }
        }
    }
    
    ```
    
- Why do some financial services still prefer SOAP over REST ?
- Purpose of private method inside the interface if they can’t be accessed outside the interface ?
    
    In Java **(since Java 9)**, interfaces can have **private methods**. While these methods **cannot be accessed outside the interface**, they serve an important purpose within the interface itself.
    
    ### **Key Reasons for Private Methods in Interfaces:**
    
    1. **Code Reusability within the Interface**
        - Private methods help **avoid code duplication** inside default and static methods of the interface.
        - They allow common logic to be encapsulated and reused by multiple methods.
    2. **Improved Code Maintainability**
        - If a logic is used in multiple places inside the interface, maintaining it in a **single private method** makes updates easier.
        - If changes are required, they need to be made in one place instead of multiple default/static methods.
    3. **Encapsulation & Abstraction**
        - Private methods **hide implementation details** from implementing classes.
        - This keeps the interface **clean** and prevents exposing unnecessary helper methods.
    4. **Better Modularity**
        - Instead of writing redundant logic in multiple default/static methods, private methods allow **modular code organization** within the interface.
- Which one is more faster streams or for-loop and why ?
    
    for loop is faster as compared to streams because streams do autoboxing / unboxing and some lambda and stream overhead because of that .
    
    ```java
    import java.util.*;
    import java.util.stream.IntStream;
    
    public class PerformanceTest {
        public static void main(String[] args) {
            List<Integer> numbers = new ArrayList<>();
            for (int i = 0; i < 10_000_000; i++) {
                numbers.add(i);
            }
    
            long start, end;
    
            // Traditional For Loop
            start = System.nanoTime();
            long sum1 = 0;
            for (int num : numbers) {
                sum1 += num;
            }
            end = System.nanoTime();
            System.out.println("For Loop Time: " + (end - start) / 1_000_000 + " ms");
    
            // Stream API
            start = System.nanoTime();
            long sum2 = numbers.stream().mapToLong(Integer::longValue).sum();
            end = System.nanoTime();
            System.out.println("Stream Time: " + (end - start) / 1_000_000 + " ms");
    
            // Parallel Stream
            start = System.nanoTime();
            long sum3 = numbers.parallelStream().mapToLong(Integer::longValue).sum();
            end = System.nanoTime();
            System.out.println("Parallel Stream Time: " + (end - start) / 1_000_000 + " ms");
        }
    }
    
    ```
    
- Which is faster to use ConcurrentHashMap or  Collections.synchronizedMap() and why?
    
    ## **Key Differences**
    
    | Feature | **ConcurrentHashMap** | **SynchronizedHashMap** (`Collections.synchronizedMap()`) |
    | --- | --- | --- |
    | **Synchronization Mechanism** | Uses **lock stripping** (multiple locks per bucket) | Uses a **single lock** for the entire map |
    | **Concurrency Level** | Multiple threads can access different segments simultaneously | Only **one thread** can modify the map at a time |
    | **Read Performance** | ✅ **Faster** (Reads are mostly non-blocking) | ❌ **Slower** (All reads require acquiring a lock) |
    | **Write Performance** | ✅ **Faster** (Only locks specific bucket) | ❌ **Slower** (Entire map is locked for updates) |
    | **Iteration Safety** | ✅ **Safe** (`ConcurrentModificationException` is avoided using weakly consistent iterators) | ❌ **Unsafe** (Iterator throws `ConcurrentModificationException` if modified during iteration) |
    | **Best for** | High-concurrency scenarios | Low-concurrency, simple synchronization needs |
- **Intern() Method in string.**
    
    → used to put a string in the pool of unique string, Known as **String Pool**
    
    → In String Pool a special area in which java stores the Literal String and Interned String to improve performance and reduce memory consumption.
    
    What does Intern Method do ?
    
    → When you call intern method of a string, it check that if the particular string is present in the pool or not, if it is present, it returns the reference of the existing string in the pool, if it doesn’t exist then it add the string to the string pool.
    
    <aside>
    👨🏽‍💻
    
    Please checkout : ‣ 
    
    </aside>
    
    ```java
    public class Main {
        public static void main(String[] args) {
            String str1 = "Hello"; // This string will be added to the string pool automatically.
            String str2 = new String("Hello"); // This creates a new string object outside the pool, in the heap.
            // (But the string is added in the string pool as well but str2 is not pointing to that)
            String str3 = str2.intern(); // This adds str2 to the string pool and returns the reference to it.
    
            System.out.println(str1 == str2); // Output: false (Different objects)
            System.out.println(str1 == str3); // Output: true (Same object from the string pool)
        }
    }
    
    ```
    
    In the example above, `str1` is automatically added to the string pool when the program is loaded. `str2` is created using the `new` keyword, so it is a different object from `str1`, even though their contents are the same. However, when we call `str2.intern()`, it adds the string "Hello" to the string pool and returns a reference to the interned string, which is then assigned to `str3`. As a result, `str3` points to the same object as `str1`, and the comparison `str1 == str3` returns `true`.
    
    It's important to use the `intern()` method with caution, as it can lead to unexpected memory usage in some cases. In general, it is most useful when you have many duplicate strings and want to reduce memory consumption by reusing the same string instances from the pool.
    
    Personal tried code with explanation
    
    ```tsx
        public static void main(String[] args) {
            String s = "abc"; // created in string pool
            String sb= new String("abc"); // created in heap memeory
            String a = sb.intern(); // moving sb to string pool then java automatically check that this string already lies in the string poll hence just assign the reference
            System.out.println(s==sb); //false
            System.out.println(s.equals(sb)); // true
            System.out.println(s.equals(a)); // true
        }
    ```
    
    **More examples**
    
    ```java
    String a = "hel";
    String b = a + "lo"; // New string on the heap, since it happens at runtime, as a is variable
    String c = "hello";
    System.out.println(b == c); // false
    
    public class Main {
        public static void main(String[] args) {
            String a = "abc"; //pool
            String c = "abc"; //pool
            String e = new String("abc"); // heap but refers to the "abc" in the pool.
            e = e.intern(); //explicit call to intern() returned a reference to "abc" string from the pool
            System.out.println(a == e); // true
    
            String d = "ab" + "c"; //pool, since concatanation bw literals happens during compile time.
            System.out.println(a == d); //hence, true
            /*
            string creation due to method calls, so string object is created in heap.
            Can be stored in pool or referenced by calling the intern()
            So "abc" is created in heap.
             */
            String b = "ab" + a.charAt(2);
            System.out.println(a == b); // hence false
        }
    ```
    
    New Case explanation:
    
    If you are concatenating a string which is created using new String(””), then new string will be created in the HEAP memory only, not in the string pool, but if you apply intern() then it will be moved to string pool.
    
    1. **Initial State**:
        
        ```java
        String b = new String("def");
        String c = b + "ghi"; // Concatenation
        ```
        
        - `b` is a heap object referencing the string `"def"` from the string pool.
        - `"ghi"` is a string literal, so it resides in the string pool.
    2. **Concatenation (`b + "ghi"`)**:
        - Since `b` is not a compile-time constant or a literal, the concatenation occurs at runtime.
        - A **new string object** is created on the **heap**, containing the result `"defghi"`. This string is not automatically placed in the string pool.
    3. **String Pool Status**:
        - The string `"defghi"` is not added to the string pool unless explicitly interned:
            
            ```java
            String d = (b + "ghi").intern();
            ```
            
    
    **KEEP IN MIND**
    
    ### Key Points:
    
    1. **Heap vs. Pool**:
        - Concatenation at **compile-time** (e.g., `"def" + "ghi"`) results in the string being added to the pool because the result is resolved at compile-time.
        - Concatenation involving variables or objects (like `b + "ghi"`) is resolved at runtime and creates a new string object on the **heap**.
    2. **Adding to the Pool**:
        - A new string will only enter the string pool if you explicitly use the `intern()` method:
        Now `"defghi"` will be added to the string pool (if it doesn't already exist).
            
            ```java
            String concatenated = (b + "ghi").intern();
            ```
            
    3. **Efficiency**:
        - If string pooling is desired for memory efficiency or comparison purposes, explicitly use `.intern()` on dynamically created strings.
- **What is a deep copy in immutable classes but for mutable fields?**
    
    → copy means creating **new reference** of every object.
    
    - A deep copy means creating a **new object** that is an **exact replica** of the original object, along with copies of all the objects it references.
    - This ensures the new object doesn’t share references with the original, avoiding unintended changes.
    
    When you make a deep copy of any object it means you ensure that any fields inside the copied object are **completely separate** from the original object’s fields.
    
    •	**Shallow copy:** The original and copy **share the same memory reference** for fields like a list.
    
    •	**Deep copy:** The original and copy **each have their own memory** for those fields.
    
    ```java
    import java.util.ArrayList;
    import java.util.List;
    
    final class Person {
        private final String name;
        private final List<String> hobbies; // Mutable field
    
        // Constructor
        public Person(String name, List<String> hobbies) {
            this.name = name;
            this.hobbies = new ArrayList<>(hobbies); // Deep copy of list -> whenever object of Person class will be created, 
                                                                                                 // new hobbies list **will be created**
        }
    
        public String getName() {
            return name;
        }
    
        public List<String> getHobbies() {
            return new ArrayList<>(hobbies); // Return deep copy
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            List<String> hobbies = new ArrayList<>();
            hobbies.add("Reading");
    
            Person person = new Person("Alice", hobbies);
            hobbies.add("Swimming"); // Original list modified
    
            System.out.println(person.getHobbies()); // Immutable copy, not affected by original list
        }
    }
    ```
    
- **Difference b/w == and .equals (Overriding .equals Method)**
    1. ==
        - == ka use **reference comparison** ke liye hota hai.
        - Iska matlab hai ki yeh check karta hai ki do objects **same memory location** ko point karte hain ya nahi.
        
        ```java
        public class Main {
            public static void main(String[] args) {
                String s1 = "Hello";
                String s2 = "Hello";
                String s3 = new String("Hello");
        
                // Reference Comparison
                System.out.println(s1 == s2); // true - s1 and s2 point to the same memory location (String pool)
                System.out.println(s1 == s3); // false - s3 is a new object in heap memory
            }
        }
        ```
        
        •	Agar objects same memory location par hain, toh == true return karega.
        
        •	Strings me agar **string pool** ka concept use ho raha hai, toh == same reference ko identify karega.
        
    2. .equals()
        - .equals() ka use **content comparison** ke liye hota hai.
        - Iska matlab hai ki yeh check karta hai ki objects ka **data ya value** same hai ya nahi.
        
        ```java
        public class Main {
            public static void main(String[] args) {
                String s1 = "Hello";
                String s2 = "Hello";
                String s3 = new String("Hello");
        
                // Content Comparison
                System.out.println(s1.equals(s2)); // true - content is the same
                System.out.println(s1.equals(s3)); // true - content is the same
            }
        }
        ```
        
        .equals() method ka behavior override kiya ja sakta hai.
        
        → For example, custom objects me aap .equals() ko apne hisaab se define kar sakte hain.
        
    3. **Comparison in User-Defined Classes**
        
        ```java
        class Person {
            String name;
        
            Person(String name) {
                this.name = name;
            }
        }
        
        public class Main {
            public static void main(String[] args) {
                Person p1 = new Person("John");
                Person p2 = new Person("John");
        
                // Reference Comparison
                System.out.println(p1 == p2); // false - different memory locations
        
                // Content Comparison
                System.out.println(p1.equals(p2)); // false - default equals() does reference comparison
            }
        }
        ```
        
        **Why .equals() Fails Here?**
        
        Default .equals() method inherited from Object class behaves like ==.
        Agar aapko content compare karna hai, toh .equals() ko override karna padega.
        
    4. **Overriding .equals() Method**
        
        ```java
        class Person {
            String name;
        
            Person(String name) {
                this.name = name;
            }
        
            @Override
            public boolean equals(Object obj) {
                if (this == obj) return true;
                if (obj == null || getClass() != obj.getClass()) return false;
                Person person = (Person) obj;
                return name.equals(person.name);
            }
        }
        
        public class Main {
            public static void main(String[] args) {
                Person p1 = new Person("John");
                Person p2 = new Person("John");
        
                // Content Comparison after overriding equals()
                System.out.println(p1.equals(p2)); // true - content is the same
            }
        }
        ```
        
        | Aspect | == (Double Equals) | .equals() Method |
        | --- | --- | --- |
        | Comparison Type | Reference comparison (memory address). | Content comparison (data/values). |
        | Default Behavior | Compares memory locations. | Can be overridden for custom content check. |
        | For Primitives | Checks actual values (e.g., int, char). | Not applicable. |
        | For Objects | Checks reference equality. | Checks content equality (if overridden). |
        | Usage | Fast, but not reliable for content comparison. | Preferred for comparing object content. |
- **What is class and object definition**
    
    Think of a class as a “design” for an object.
    
    → A class in Java is a user-defined data type that contains fields (variables) and methods (functions) to define the characteristics and behavior of an object.
    
    An **object** is an instance of a class. 
    
    An object is a concrete entity created using a class.
    
    It contains the actual data and methods to operate on that data.
    
    →  represents a real-world entity with specific values for its attributes and the ability to perform actions defined by the methods of its class.
    
- **Solid Principles**
    
    The **SOLID principles** are a set of design principles in object-oriented programming aimed at creating software that is easy to maintain, extend, and understand. Each letter in **SOLID** stands for a principle:
    
    ---
    
    ## 🟢 **1. Single Responsibility Principle (SRP)**
    
    A class should have only **one reason to change**, meaning it should only have **one responsibility**.
    
    ### 🚗 **Real-world Example**:
    
    Imagine you have a **Car**.
    
    ```java
    // Bad Design: A single class handling multiple responsibilities
    class Car {
        public void drive() {
            System.out.println("Car is being driven");
        }
    
        public void calculateMaintenanceCost() {
            System.out.println("Calculating maintenance cost");
        }
    }
    
    // Good Design: Separate the responsibilities
    class Car {
        public void drive() {
            System.out.println("Car is being driven");
        }
    }
    
    class MaintenanceService {
        public void calculateMaintenanceCost(Car car) {
            System.out.println("Calculating maintenance cost");
        }
    }
    
    ```
    
    - Driving the car is different from **calculating maintenance costs**.
    - These should be separate responsibilities.
    
    ### ✅ **Code Implementation**
    
    👉 **Why?** If maintenance logic changes, we don't need to modify the `Car` class.
    
    ---
    
    ## 🔵 **2. Open/Closed Principle (OCP)**
    
    **"Open for extension, but closed for modification"**
    
    - You should be able to extend a class without modifying existing code.
    
    ### 🏦 **Real-world Example**:
    
    A bank allows different types of **loan interest calculations (Home Loan, Personal Loan, etc.)**. Instead of modifying a class for every new loan type, we should extend it.
    
    ### ✅ **Code Implementation**
    
    ```java
    // Bad Design: Modifying code every time a new loan type is added
    class Loan {
        public double getInterestRate(String loanType) {
            if (loanType.equals("Home")) {
                return 7.5;
            } else if (loanType.equals("Personal")) {
                return 10.5;
            }
            return 0;
        }
    }
    
    // Good Design: Extend behavior without modifying the existing class
    abstract class Loan {
        abstract double getInterestRate();
    }
    
    class HomeLoan extends Loan {
        public double getInterestRate() {
            return 7.5;
        }
    }
    
    class PersonalLoan extends Loan {
        public double getInterestRate() {
            return 10.5;
        }
    }
    
    ```
    
    👉 **Why?** Now, adding a new loan type is easy—just create a new subclass.
    
    ---
    
    ## 🟣 **3. Liskov Substitution Principle (LSP)**
    
    Derived classes should be **substitutable** for their base class **without breaking the program**.
    
    ### 🐦 **Real-world Example**:
    
    A **Bird** can fly, but what about **Penguin**?
    
    - If a `Bird` class has a `fly()` method, then `Penguin` (which extends Bird) **should not break** when used in place of `Bird`.
    
    ### ✅ **Code Implementation**
    
    ```java
    
    // Bad Design: Violates LSP
    class Bird {
        public void fly() {
            System.out.println("Flying");
        }
    }
    
    class Penguin extends Bird {
        @Override
        public void fly() {
            throw new UnsupportedOperationException("Penguins cannot fly!");
        }
    }
    
    // Good Design: Create separate interfaces
    interface Bird {
        void eat();
    }
    
    interface FlyingBird extends Bird {
        void fly();
    }
    
    class Sparrow implements FlyingBird {
        public void eat() {
            System.out.println("Sparrow is eating");
        }
    
        public void fly() {
            System.out.println("Sparrow is flying");
        }
    }
    
    class Penguin implements Bird {
        public void eat() {
            System.out.println("Penguin is eating");
        }
    }
    
    ```
    
    👉 **Why?** Now `Penguin` and `Sparrow` follow their natural behavior.
    
    ---
    
    ## 🟠 **4. Interface Segregation Principle (ISP)**
    
    Clients should not be forced to implement **unused** methods from large interfaces.
    
    ### 🍽 **Real-world Example**:
    
    A restaurant has **Waiters** and **Chefs**.
    
    - Waiters take **orders**, but they don't **cook**.
    - Chefs **cook**, but they don't **take orders**.
    
    ### ✅ **Code Implementation**
    
    ```java
    // Bad Design: A single interface with unrelated responsibilities
    interface RestaurantWorker {
        void takeOrder();
        void cookFood();
    }
    
    // Good Design: Separate interfaces for different responsibilities
    interface Waiter {
        void takeOrder();
    }
    
    interface Chef {
        void cookFood();
    }
    
    class Server implements Waiter {
        public void takeOrder() {
            System.out.println("Taking customer order");
        }
    }
    
    class Cook implements Chef {
        public void cookFood() {
            System.out.println("Cooking food");
        }
    }
    
    ```
    
    👉 **Why?** Now `Waiter` and `Chef` only implement relevant methods.
    
    ---
    
    ## 🔴 **5. Dependency Inversion Principle (DIP)**
    
    **High-level modules should not depend on low-level modules. Both should depend on abstractions.**
    
    ### 💳 **Real-world Example**:
    
    A payment system should work with **CreditCard, DebitCard, or UPI payments**.
    
    - Instead of tying the system to a specific **payment method**, it should depend on a common **Payment interface**.
    
    ### ✅ **Code Implementation**
    
    ```java
    // Bad Design: Tightly coupled to CreditCard
    class ShoppingMall {
        private CreditCard creditCard;
    
        public ShoppingMall(CreditCard creditCard) {
            this.creditCard = creditCard;
        }
    
        public void checkout(int amount) {
            creditCard.pay(amount);
        }
    }
    
    // Good Design: Depend on an abstractio (interface)
    interface PaymentMethod {
        void pay(int amount);
    }
    
    class CreditCard implements PaymentMethod {
        public void pay(int amount) {
            System.out.println("Paid " + amount + " using Credit Card");
        }
    }
    
    class DebitCard implements PaymentMethod {
        public void pay(int amount) {
            System.out.println("Paid " + amount + " using Debit Card");
        }
    }
    
    class ShoppingMall {
        private PaymentMethod paymentMethod;
    
        public ShoppingMall(PaymentMethod paymentMethod) {
            this.paymentMethod = paymentMethod;
        }
    
        public void checkout(int amount) {
            paymentMethod.pay(amount);
        }
    }
    
    ```
    
    👉 **Why?** Now we can use **any payment method** without modifying `ShoppingMall`.
    
    ---
    
    ## 🎯 **Conclusion**
    
    | Principle | Key Idea | Real-world Example |
    | --- | --- | --- |
    | **SRP** | One class, one responsibility | Car should not handle maintenance |
    | **OCP** | Extend behavior without modifying existing code | Add new loan types without modifying the base class |
    | **LSP** | Subtypes should be replaceable without breaking code | Birds can fly, but Penguins cannot |
    | **ISP** | Don't force classes to implement unused methods | Waiters take orders, but they don't cook |
    | **DIP** | Depend on abstractions, not concrete classes | Payment system works with multiple payment types |
- **Singleton Classes**
    
    → A **singleton class** ensures that only one instance of the class is created during the application’s lifetime. This is achieved by restricting instantiation.
    
    **Characteristics:**
    
    1.	Private constructor.
    
    2.	A static variable to hold the single instance.
    
    3.	A public static method to provide the instance.
    
    ```java
    class Singleton {
        private static Singleton instance;
    
        private Singleton() {
            // Private Constructor
        }
    
        public static Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
    }
    ```
    
    **Use Cases:**
    
    •	Database connections.
    
    •	Logging.
    
    **Thread-Safe Singleton:**
    
    ```java
    class ThreadSafeSingleton {
        private static ThreadSafeSingleton instance;
    
        private ThreadSafeSingleton() {}
    
        public static synchronized ThreadSafeSingleton getInstance() { // just made the intances returning methods synchronized.
            if (instance == null) {
                instance = new ThreadSafeSingleton();
            }
            return instance;
        }
    }
    ```
    
    **Drawback**: This approach may lead to **performance issues** because the entire method is synchronized, even though synchronization is needed only for the first instance creation.
    
    ### **Method 2: Double-Checked Locking**
    
    - Synchronize only the block of code that creates the instance. This improves performance by reducing unnecessary locking after the instance is created.
    
    ```java
    
    public class Singleton {
    // Setting it volatile because sometime what happens when Thread A is accessing the instance, and its the 1st time, so no instance is 
    // present, as thread A goes inside the synchronized block and instance is in making, partially contructed instance is set and when 
    // Thread B comes and checks the outer IF condition and finds tha partially contructed instance, it gets that and starts working on it
    // Hence,making it volatile so that this variables valus is always picked up from the main memory as it cannot be cached.
        private static volatile Singleton instance;
        private String data;
    
        private Singleton(String data) {
        this.data = data
        }
    
        public static Singleton getInstance(String data) {
            if (instance == null) { // First check (without locking)
                synchronized (Singleton.class) {
                    if (instance == null) { // Second check (with locking)
                        instance = new Singleton(data);
                    }
                }
            }
            return instance;
        }
    }
    
    ```
    
    **Explanation**:
    
    1. The first `if (instance == null)` avoids unnecessary synchronization after the instance is created.
    2. The second `if (instance == null)` inside the synchronized block ensures that only one thread creates the instance.
    
    Calling Method:
    
    ```tsx
    public class TestSingleton {
        public static void main(String[] args) {
            Singleton instance1 = Singleton.getInstance();
            Singleton instance2 = Singleton.getInstance();
    
            System.out.println(instance1 == instance2); // Output: true (same instance)
        }
    }
    
    ```
    
- Which is better (Properties or YML)
    
    Both have their advantage YML is better because we need to write some less lines but .properties file we need to write more lines YML is prone to issues because of their indentation while .properties file is less prone to issues.
    
- Why do we need spring profiles ?
    1. application.dev.properties
    2. application.production.properties
    3. application.staging.properties
    
    profileName=dev, production, staging
    
    To apply spring profiles we can add in .properties → ***spring.profiles.active*** = profileName
    
- Difference Between FixedDelay and FixedRate in Scheduling ?
    
    FixedDealy - the fixedDealy makes sure that there is a delay of a milliseconds between the finish time of an executions of a task and the start time of the next execution of the task.
    
    @Scheduled(fixedDelay = 1000)
    
    FixedRate - the fixedrate property runs the scheduled task at every n milliseconds it doesn’t check for any previous execution of the task.
    
    @Scheduled(fixedRate=1000)
    
    CRON Expressions in Scheduling ?
    
- AOP(Aspect Oriented Programming) in spring boot ?
    
    Aspect Orient Programming is a programming paradigm aiming to segregate cross-cutting functionalities, such as logging , from business logic in an application.
    
    This is used to do some common things in spring boot application suppose you have 100 of controller and you want to add logging in every controller so you can do it in here only it contains 4 Annotation you can add for exception, before calling the method ,after calling the method etc.
    
    @Before(`JoinPoint joinPoint`) → Advice that executes before a join point, but which does not have the ability to prevent execution flow proceeding to the joint point ( unless it throws an exception )
    @AfterRturning(`JoinPoint joinPoint`) : → Advice to be executed after a joint point completes normally.
    @AfterThrowing(`JoinPoint joinPoint`):→  Advice to be executed if a method exits by throwing an exception
    @Around(`ProceedingJoinPoint proceedingJoinPoint`) → 
    
    1. Advice that surrounds a join point such as method invocation. The first parameter of the advice method must be of type ProceedingJointPoint.
        1. Within the body of the advice , calling proceed() on the ProceedingJointPoint cause the underlying method to execute.
    2. The proceed method may also be called passing in as Object[] - the values in the array will be used as the arguments to the method execution when it proceeds.
    3. The value return by the around advice will be the return value seen by the caller of the method.
- **Difference Between `@Autowired` and Constructor Injection in Spring Boot**
    
    ## **1️⃣ `@Autowired` Field Injection (Not Recommended)**
    
    **Example:**
    
    ```java
    @Component
    public class CarService {
        @Autowired
        private Engine engine;  // Direct field injection
    
        public void start() {
            engine.run();
        }
    }
    
    ```
    
    ### **Drawbacks of Field Injection:**
    
    ❌ **Difficult to Test** – You cannot create unit tests easily because dependencies are hidden inside private fields.
    
    ❌ **Tightly Coupled** – The Spring framework is required to inject dependencies.
    
    ❌ **No `final` Fields** – You cannot declare dependencies as `final`, making immutability harder.
    
    ❌ **Encapsulation Violation** – Directly modifying fields can break encapsulation.
    
    ---
    
    ## **2️⃣ Constructor Injection (Recommended)**
    
    **Example:**
    
    ```java
    @Component
    public class CarService {
        private final Engine engine;
    
        @Autowired  // Optional since Spring 4.3+
        public CarService(Engine engine) {
            this.engine = engine;
        }
    
        public void start() {
            engine.run();
        }
    }
    ```
    
    ### **Advantages of Constructor Injection:**
    
    ✅ **Mandatory Dependencies are Enforced** – The object cannot be created without the required dependencies.
    
    ✅ **Easy to Test** – You can instantiate the class with mock dependencies for unit testing.
    
    ✅ **Supports `final` Fields** – Makes the dependency immutable, improving code safety.
    
    ✅ **Works without Spring** – The class can be used without requiring Spring’s container.
    
    ✅ **Clear Dependency Requirements** – The constructor explicitly defines what dependencies are needed.
    
    ---
    
    ## **When to Use Which?**
    
    | Approach | Pros | Cons | When to Use? |
    | --- | --- | --- | --- |
    | **Field Injection (`@Autowired` on field)** | Quick & simple | Hard to test, tightly coupled, no `final` | **Avoid using it in production** |
    | **Constructor Injection (Recommended)** | Easy to test, immutable fields, clear dependencies | Slightly more boilerplate | **Always prefer it for mandatory dependencies** |
- Actuator in spring boot ?
    
    Actuator is used to get all the production ready features like health and all the api
    
    Spring Boot Actuator is like a toolbox for monitoring and managing our Spring Boot application. It gives us endpoints (think of them as special URLs) where we can check health, view configurations, gather metrics, and more. It's super useful for keeping an eye on how your app is doing.
    In a production environment (which is like the real world where your app is being used by people), these endpoints can reveal sensitive information about your application. Imagine leaving our diary open in a public place – we wouldn't want that, right? Similarly, we don't want just anyone peeking into the internals of your application.
    
    ### **Important Spring Boot Actuator Endpoints & Their Uses**
    
    | **Endpoint** | **Use Case** |
    | --- | --- |
    | **`/actuator`** | Lists all available actuator endpoints. |
    | **`/actuator/health`** | Shows application health status (✅ UP / ❌ DOWN). |
    | **`/actuator/info`** | Displays application metadata (version, environment details). |
    | **`/actuator/metrics`** | Exposes performance metrics (memory, CPU, GC, etc.). |
    | **`/actuator/metrics/{metricName}`** | Shows specific metric details (e.g., `/metrics/jvm.memory.used`). |
    | **`/actuator/env`** | Displays all environment properties. |
    | **`/actuator/env/{propertyName}`** | Fetches a specific environment property. |
    | **`/actuator/mappings`** | Lists all request mappings (URLs and handlers). |
    | **`/actuator/beans`** | Shows all Spring Beans in the application context. |
    | **`/actuator/loggers`** | Manages log levels dynamically at runtime. |
    | **`/actuator/threaddump`** | Provides thread dump for debugging. |
    | **`/actuator/heapdump`** | Generates a heap dump for memory analysis. |
    | **`/actuator/scheduledtasks`** | Lists all scheduled tasks in the app. |
    | **`/actuator/httptrace`** | Shows recent HTTP requests (disabled by default in Spring Boot 3). |
    
    ### **How to Enable Actuator Endpoints?**
    
    - By default, only `/health` and `/info` are enabled.
    - Enable all endpoints via **`application.properties`**:
        
        ```
        management.endpoints.web.exposure.include=*
        ```
        
    - Or expose specific endpoints
        
        ```
        management.endpoints.web.exposure.include=health,info,metrics
        ```
        
- Circuit Breaker & Retry Implementation (micro - service).
- Fault Torrence
    
    resilience4j.readme.io
    
    we used fault tolerance with the help of this.
    
    CircuitBreaker
    
- Important Annotations
    1. What is meta annotations→annotations that we can apply over other annotations.
    2. @SpringBootApplication → consists of @Configuration , @EnableAutoConfiguration, @ComponentScan
    3. public @interface custiomAnnotation() → way to create custom annotation.
    4. @EnableAutoConfiguration → used to auto configure like if you put database source url and user and password it will auto connect to DB.
    5. @ComponentScan → This tells Spring to scan the package of the annotated class (and its sub-packages) for components like @Component, @Service, @Repository, and @Controller so that they can be registered as beans in the Spring context.
    6. **@Filter Annotations:**
    7. @EnableCaching
    8. @Service
    9. @Condtional annotations which which is used in AutoConfiguration .
    
    ### **Summary**
    
    | **Annotation** | **Condition Checked** | **Use Case** |
    | --- | --- | --- |
    | `@Conditional` | Custom logic (`Condition` interface) | Complex conditions |
    | `@ConditionalOnProperty` | Property in `application.properties` | Feature toggling |
    | `@ConditionalOnClass` | If a class exists | Check for dependencies (e.g., Hibernate) |
    | `@ConditionalOnMissingClass` | If a class is missing | Provide alternative implementation |
    | `@ConditionalOnBean` | If a bean exists | Enable feature if another bean is present |
    | `@ConditionalOnMissingBean` | If a bean is missing | Define default beans |
    | `@ConditionalOnExpression` | Spring Expression (SpEL) | Advanced conditions (profiles, flags) |
    | `@ConditionalOnWebApplication` | If it's a web app | Web-specific beans |
    | `@ConditionalOnNotWebApplication` | If it's NOT a web app | Non-web applications |
- What is Generics in java ?
    
    Generics means **parameterized types**. The idea is to allow a type (like `Integer`, `String`, etc., or user-defined types) to be a parameter to methods, classes, and interfaces. Using Generics, it is possible to create classes that work with different data types. An entity such as a class, interface, or method that operates on a parameterised type is a **generic entity**.
    
    ```jsx
    package Generic;
    
    import java.util.List;
    
    public class GenericClass<T> {
    
        private T name;
        private List<T> nameList;
    
        GenericClass(T name,List<T> nameList){
            this.name=name;
            this.nameList=nameList;
        }
    
        public T getName(){
            return name;
        }
    
        public List<T> getNameList(){
            return nameList;
        }
    }
    
    package Generic;
    
    import java.util.List;
    
    public class GenericMainClass {
    
        public static void main(String[] args) {
            GenericClass<String> sh=new GenericClass<>("hello", List.of("AB","CD"));
            System.out.println(sh.getName());
            System.out.println(sh.getNameList());
        }
    }
    
    ```
    
- Atomicity in java using Atomic package available in java.util.concurrent.atomic
    
    ```java
    import java.util.concurrent.atomic.AtomicInteger;
    
    public class AtomicExample {
        public static void main(String[] args) {
            AtomicInteger atomicInt = new AtomicInteger(0);
    
            // Increment and get the new value
            int newValue = atomicInt.incrementAndGet();
            System.out.println("New Value: " + newValue); // Output: 1
    
            // Add a delta and get the new value
            int updatedValue = atomicInt.addAndGet(5);
            System.out.println("Updated Value: " + updatedValue); // Output: 6
    
            // Compare and set
            boolean success = atomicInt.compareAndSet(6, 10);
            System.out.println("Compare and Set Success: " + success); // Output: true
            System.out.println("Current Value: " + atomicInt.get()); // Output: 10
    
            // Get and set
            int oldValue = atomicInt.getAndSet(20);
            System.out.println("Old Value: " + oldValue); // Output: 10
            System.out.println("Current Value: " + atomicInt.get()); // Output: 20
        }
    }
    ```
    
- **How to Create a Custom Type in Java?**
    
    In Java, **custom types** allow you to define **your own data structures** beyond built-in types like `int`, `String`, etc. You can create custom types using **classes, records, enums, and interfaces**.
    
- Deadlock Concept
    
    Deadlock occurs when two or more threads are waiting for each other to release resources, and none of them can proceed. 
    
    ```java
    public class DeadlockExample {
    
        private static final Object LOCK1 = new Object();
        private static final Object LOCK2 = new Object();
    
        public static void main(String[] args) {
            Thread thread1 = new Thread(() -> {
                synchronized (LOCK1) {
                    System.out.println("Thread 1: Holding LOCK1...");
                    try {
                        Thread.sleep(100); // Simulate some work
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println("Thread 1: Waiting for LOCK2...");
                    synchronized (LOCK2) {
                        System.out.println("Thread 1: Acquired LOCK2!");
                    }
                }
            });
    
            Thread thread2 = new Thread(() -> {
                synchronized (LOCK2) {
                    System.out.println("Thread 2: Holding LOCK2...");
                    try {
                        Thread.sleep(100); // Simulate some work
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println("Thread 2: Waiting for LOCK1...");
                    synchronized (LOCK1) {
                        System.out.println("Thread 2: Acquired LOCK1!");
                    }
                }
            });
    
            thread1.start();
            thread2.start();
        }
    }
    ```
    
    ### Explanation of the Deadlock:
    
    1. **Thread 1**:
        - Acquires `LOCK1` and then waits for `LOCK2`.
    2. **Thread 2**:
        - Acquires `LOCK2` and then waits for `LOCK1`.
    3. **Deadlock**:
        - Both threads are waiting for each other to release their respective locks, leading to a deadlock.
- Optional Class in java
    
    The `Optional` class in Java, introduced in Java 8, is a container object that may or may not contain a non-null value. It is designed to help handle `null` values more gracefully and reduce the likelihood of `NullPointerException`. Instead of returning `null`, methods can return an `Optional` to indicate that the result might be absent.
    
    ```java
    import java.util.Optional;
    
    public class OptionalExample {
        public static void main(String[] args) {
            // Creating Optional instances
            Optional<String> optionalWithValue = Optional.of("Hello, World!");
            Optional<String> optionalEmpty = Optional.empty();
            Optional<String> optionalNullable = Optional.ofNullable(null);
    
            // Checking if a value is present
            System.out.println("optionalWithValue is present: " + optionalWithValue.isPresent()); // true
            System.out.println("optionalEmpty is present: " + optionalEmpty.isPresent()); // false
    
            // Getting the value (unsafe, throws NoSuchElementException if empty)
            if (optionalWithValue.isPresent()) {
                System.out.println("Value: " + optionalWithValue.get()); // Hello, World!
            }
    
            // Safe way to handle Optional
            optionalWithValue.ifPresent(value -> System.out.println("Value: " + value)); // Hello, World!
    
            // Providing a default value
            String valueOrDefault = optionalEmpty.orElse("Default Value");
            System.out.println("Value or Default: " + valueOrDefault); // Default Value
    
            // Using orElseGet with a Supplier
            String valueOrSupplier = optionalEmpty.orElseGet(() -> "Value from Supplier");
            System.out.println("Value or Supplier: " + valueOrSupplier); // Value from Supplier
    
            // Using orElseThrow to throw an exception if empty
            try {
                String valueOrThrow = optionalEmpty.orElseThrow(() -> new RuntimeException("No value found"));
            } catch (RuntimeException e) {
                System.out.println("Exception: " + e.getMessage()); // No value found
            }
    
            // Using map to transform the value
            Optional<String> upperCaseValue = optionalWithValue.map(String::toUpperCase);
            upperCaseValue.ifPresent(value -> System.out.println("Uppercase Value: " + value)); // HELLO, WORLD!
    
            // Using flatMap to chain Optional operations
            Optional<String> flatMappedValue = optionalWithValue.flatMap(value -> Optional.of(value + " - FlatMapped"));
            flatMappedValue.ifPresent(value -> System.out.println("FlatMapped Value: " + value)); // Hello, World! - FlatMapped
    
            // Using filter to conditionally keep the value
            Optional<String> filteredValue = optionalWithValue.filter(value -> value.contains("World"));
            filteredValue.ifPresent(value -> System.out.println("Filtered Value: " + value)); // Hello, World!
        }
    }
    ```
    
- Visibility problem in java Multithreading
    
    can be solved using volatile keyword
    
- Thread pool in java
    
    A **Thread Pool** in Java is a collection of **pre-created** worker threads that execute tasks efficiently. Instead of creating a new thread for each task, the pool **reuses existing threads**, improving performance and resource management.
    
    ---
    
    ## **1. Why Use a Thread Pool?**
    
    - ✅ **Performance** → Reduces overhead of creating/destroying threads.
    - ✅ **Resource Management** → Controls the number of active threads to prevent system overload.
    - ✅ **Concurrency** → Allows multiple tasks to run simultaneously using limited threads.
    
    ---
    
    ## **2. Creating a Thread Pool Using `ExecutorService`**
    
    Java provides the **`ExecutorService`** interface in the `java.util.concurrent` package to manage thread pools.
    
    ### **Example: Fixed Thread Pool**
    
    ```java
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    
    public class ThreadPoolExample {
        public static void main(String[] args) {
            ExecutorService executor = Executors.newFixedThreadPool(3); // Pool with 3 threads
    
            for (int i = 1; i <= 5; i++) {
                int taskId = i;
                executor.execute(() -> {
                    System.out.println("Task " + taskId + " executed by " + Thread.currentThread().getName());
                });
            }
    
            executor.shutdown(); // Shut down the pool after all tasks complete
        }
    }
    
    ```
    
    ### **Output Example:**
    
    ```
    Task 1 executed by pool-1-thread-1
    Task 2 executed by pool-1-thread-2
    Task 3 executed by pool-1-thread-3
    Task 4 executed by pool-1-thread-1
    Task 5 executed by pool-1-thread-2
    ```
    
    ✅ **Threads are reused** instead of creating new ones.
    
    ---
    
    ## **3. Types of Thread Pools**
    
    The `Executors` class provides different types of thread pools:
    
    | **Method** | **Description** |
    | --- | --- |
    | `Executors.newFixedThreadPool(n)` | A **fixed number** of threads (`n`). Best for stable workloads. |
    | `Executors.newCachedThreadPool()` | **Creates threads as needed** and reuses idle ones. Best for many short-lived tasks. |
    | `Executors.newSingleThreadExecutor()` | **Single thread** handles all tasks sequentially. Ensures **one task at a time**. |
    | `Executors.newScheduledThreadPool(n)` | **Scheduled execution** (e.g., delay tasks). Best for periodic tasks. |
    
    ## **When to Use Which Thread Pool?**
    
    | **Scenario** | **Best Thread Pool** |
    | --- | --- |
    | Fixed number of tasks | `newFixedThreadPool(n)` |
    | Large number of short-lived tasks | `newCachedThreadPool()` |
    | Single-threaded execution | `newSingleThreadExecutor()` |
    | Periodic/scheduled tasks | `newScheduledThreadPool(n)` |
- Daemon Thread in java
    
    A **Daemon Thread** in Java is a **low-priority, background thread** that runs **to support other threads** but does not prevent the JVM from exiting. When all **non-daemon (user) threads** finish execution, the JVM **terminates all daemon threads automatically**, even if they are still running.
    
    ---
    
    ## **1. Characteristics of Daemon Threads**
    
    ✅ **Runs in the background** to support non-daemon threads.
    
    ✅ **Automatically stops when JVM exits**, even if running.
    
    ✅ **Used for tasks like garbage collection, logging, monitoring, etc.**
    
    ✅ **Lower priority** than user threads.
    
    ---
    
    ## **2. How to Create a Daemon Thread?**
    
    You can set a thread as a daemon using the `setDaemon(true)` method **before starting the thread**.
    
    ### **Example: Creating a Daemon Thread**
    
    ```java
    class DaemonExample extends Thread {
        public void run() {
            while (true) {
                System.out.println("Daemon Thread is running...");
                try { Thread.sleep(1000); } catch (InterruptedException e) { e.printStackTrace(); }
            }
        }
    
        public static void main(String[] args) {
            DaemonExample daemonThread = new DaemonExample();
            daemonThread.setDaemon(true); // Marking thread as Daemon
            daemonThread.start();
    
            System.out.println("Main thread is exiting...");
        }
    }
    
    ```
    
    ### **Output Example:**
    
    ```
    Main thread is exiting...
    Daemon Thread is running...
    ```
    
    🔹 The daemon thread **may or may not** print its message because the JVM exits immediately after the main thread finishes.
    
    ---
    
    ## **3. Daemon vs. User Threads**
    
    | Feature | **Daemon Thread** | **User Thread** |
    | --- | --- | --- |
    | Runs in Background | ✅ Yes | ❌ No |
    | Stops when JVM Exits | ✅ Yes | ❌ No (JVM waits for user threads) |
    | Use Case | Logging, GC, monitoring | Main application logic |
    
    ---
    
    ## **4. Important Points About Daemon Threads**
    
    🚨 **Must set `setDaemon(true)` before `start()`. Otherwise, it throws `IllegalThreadStateException`.**
    
    🚨 **JVM terminates daemon threads if no user thread is running.**
    
    🚨 **Daemon threads are not recommended for critical tasks (like writing to a file) since they may stop abruptly.**
    
    ### **Example: What Happens If We Set `setDaemon(true)` After `start()`?**
    
    ```java
    Thread t = new Thread();
    t.start();
    t.setDaemon(true); // ❌ ERROR: IllegalThreadStateException
    ```
    
    ✅ Always set **before** calling `start()`.
    
    ---
    
    ## **5. When to Use Daemon Threads?**
    
    🔹 **Garbage Collection (`GC` Thread)**
    
    🔹 **Logging & Monitoring** (Background tasks)
    
    🔹 **Auto-saving feature in applications**
    
    ---
    
    ### **Summary**
    
    ✅ Daemon threads are **background** threads that **terminate** when all user threads finish.
    
    ✅ Use **`setDaemon(true)`** before starting a thread.
    
    ✅ JVM does **not wait** for daemon threads before shutting down.
    
    ✅ Best suited for **non-essential** background tasks.
    
- Anonymous class vs lambda
    
    
    | Concept | Description |
    | --- | --- |
    | **Anonymous Class** | A way to create a **one-time-use subclass** of an interface or class without giving it a name. Introduced in Java 1.1. |
    | **Lambda Expression** | A **shorter, cleaner syntax** for implementing **functional interfaces** (interfaces with just one abstract method). Introduced in Java 8. |
    
    ---
    
    ## 🧾 Syntax Comparison
    
    ### ✅ Example: Using `Runnable`
    
    **1. Anonymous Class:**
    
    ```java
    Runnable r = new Runnable() {
        @Override
        public void run() {
            System.out.println("Running in anonymous class");
        }
    };
    r.run();
    ```
    
    **2. Lambda Expression:**
    
    ```java
    Runnable r = () -> System.out.println("Running in lambda");
    r.run();
    ```
    
    ---
    
    ## 🔍 Key Differences
    
    | Feature | Anonymous Class | Lambda Expression |
    | --- | --- | --- |
    | ✅ **Verbosity** | More code | Concise and readable |
    | ✅ **Type** | Creates a new class | Just a function, no new class |
    | ✅ **Access to `this`** | Refers to the **anonymous class** itself | Refers to the **enclosing class** |
    | ✅ **Use Cases** | Can implement interfaces with **multiple methods** (or extend classes) | Only for **functional interfaces** (1 method) |
    | ✅ **Readability** | Verbose, but explicit | Cleaner, but might be confusing to beginners |
    | ✅ **Performance** | Slightly more overhead (inner class instance) | Potentially more optimized (captured only as needed) |
    
    ---
    
    ### 🔁 Another Example: Comparator
    
    **Anonymous Class:**
    
    ```java
    Comparator<String> comp = new Comparator<String>() {
        @Override
        public int compare(String a, String b) {
            return a.compareTo(b);
        }
    };
    ```
    
    **Lambda:**
    
    ```java
    Comparator<String> comp = (a, b) -> a.compareTo(b);
    ```
    
    ---
    
    ## 🔐 Bonus Tip: Access to `this`
    
    ```java
    public class Test {
        public void demo() {
            Runnable r1 = new Runnable() {
                public void run() {
                    System.out.println(this); // refers to anonymous class instance
                }
            };
    
            Runnable r2 = () -> {
                System.out.println(this); // refers to outer class (Test)
            };
        }
    }
    
    ```
    
    ---
    
    ## 🧠 When to Use What?
    
    | Situation | Use This |
    | --- | --- |
    | Simple, one-method interface (e.g., Runnable, Comparator) | ✅ **Lambda** |
    | Need to override multiple methods or create a subclass | ❌ **Anonymous class** |
    | Need access to your own `this` | ❌ **Anonymous class** |
    | Want better readability and less code | ✅ **Lambda** |
    
    ---
    
    ## 💡 TL;DR
    
    - Lambdas are **cleaner and preferred** when using functional interfaces.
    - Anonymous classes are still needed when you need more than just a single method or want to create a full custom implementation.
- Callable vs Runnable Interafce
    
    Both are Functional Interface through which we an execute the task.
    
    Runnable doesn't return a result and can’t throw a checked Exception.
    
    ## 🔄 Runnable vs Callable: At a Glance
    
    | Feature | `Runnable` | `Callable<V>` |
    | --- | --- | --- |
    | Return Value | **No** return value (`void`) | **Returns** a result of type `V` |
    | Throws Exception | **Cannot** throw checked exceptions | **Can** throw checked exceptions |
    | Introduced In | Java 1.0 | Java 5 |
    | Used With | `Thread`, `ExecutorService` | `ExecutorService` (with `Future`) |
    
    ---
    
    ## ✅ 1. Runnable Example (No return value)
    
    ```java
    public class RunnableExample {
        public static void main(String[] args) {
            Runnable task = () -> {
                System.out.println("Running task with Runnable");
            };
    
            Thread thread = new Thread(task);
            thread.start();
        }
    }
    ```
    
    ---
    
    ## ✅ 2. Callable Example (With return value)
    
    ```java
    import java.util.concurrent.*;
    
    public class CallableExample {
        public static void main(String[] args) throws Exception {
            Callable<String> task = () -> {
                return "Result from Callable";
            };
    
            ExecutorService executor = Executors.newSingleThreadExecutor();
            Future<String> future = executor.submit(task);
    
            // Blocks until the result is available
            String result = future.get();
            System.out.println("Result: " + result);
    
            executor.shutdown();
        }
    }
    ```
    
    ---
    
    ## 🧠 Summary
    
    | If you need... | Use... |
    | --- | --- |
    | Just run a task with no result | `Runnable` |
    | Run a task and get a result | `Callable` |
    | To throw checked exceptions in task | `Callable` |
    | Use with `Thread` | `Runnable` only |
    | Use with `ExecutorService` | Both work |
- CompletableFuture
    
    `CompletableFuture` is part of the **java.util.concurrent** package and is used for **asynchronous programming** in Java. It allows you to run tasks in the background, combine multiple futures, handle results, and manage exceptions efficiently.
    
    ```java
    import java.util.concurrent.CompletableFuture;
    
    public class CompletableFutureExample {
        public static void main(String[] args) {
            CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello, World!");
            future.thenAccept(System.out::println);
        }
    }
    ```
    
    🔹**`supplyAsync()`** runs a task in a separate thread and returns a result.
    
    🔹**`thenAccept()`**  processes the result asynchronously.
    
- Blue-green deployment
    
    **Blue-Green Deployment** is a **zero-downtime deployment strategy** where two identical environments (**Blue** and **Green**) are used to deploy and switch traffic seamlessly.
    
    ---
    
    ### **1. How It Works?**
    
    1️⃣ **Blue Environment** → Running the **current version** of the application.
    
    2️⃣ **Green Environment** → Deploy the **new version** of the application.
    
    3️⃣ **Traffic Switch** → Once the Green environment is **fully tested**, traffic is switched from **Blue to Green**.
    
    4️⃣ **Rollback Option** → If an issue occurs, revert back to the **Blue** environment.
    
- Define Collection Interface
    
    Consist of Map , List, Queue and Set in which Map doesn’t inherit the Collection interafac
    
    Map-
    
- List Interface
    
    ArrayList→ Dynamic resizing -50% of original size / non-synchornized
    
    LinkedList→ implement List and Dequeue interface maintains the insertion , non synchronised. does not support accessing elements randomly. can use ListIterator to iterate
    
    LinkedList elements.
    
- Set Interface
    
    Set (extends Sorted Set & Tree Set)→HashSet→LinkedHashSet→
    
- Association in java
    
    Association depicts the Relationship between two classes 
    
    It’s of 2 types → (Weak) → Aggregation - One object and can live without another object
    
    For example : A car has a driver or a driver has a car.
    
    ( Strong ) → Composition - One object cannot exists without another object.
    
    For example : Engine cannot exists without car.
    
- Covariant return type
    
    Covariant return type means → return type may vary during overriding.
    
    Before java 5 it was not allowed to overriding any method if return type is changed in child class. but now it’s possible only if return type is subclass type.
    
- Exception handling
    
    Exception is an abnormal condition which occurs during the execution of a program and disrupts normal flow of the program. If not handled properly it can cause the program to terminate abruptly.
    
    - To handle the exception we use try , catch and finally
    - Hierarch of exception
    
    ![image.png](assets/image.png)
    
    ![image.png](assets/image%201.png)
    
    ![image.png](assets/image%202.png)
    
    Multi catch block Java 7 → catch ( NullPointerException | SQLException e)
    
- What is the difference between final , Finally and finalize.
    
    **Final** → it’s a keyword is used to apply restrictions on the class ,method and variables. The final class can’t be inherited final method can it be overridden and final variable can be changed.
    
    **Finally** → this keyword is used with the try -catch block to provide statements that will always get executed even if some exceptions arises. usually , finally is used to close resources.
    
    **Finalize** → is used to perform the clean up procession just before the object is garbage collected.
    
- Why JAVA 8 ? Main agenda behind JAVA 8
    
    Significant reason for introduction JAVA 8 was to introduce Conciseness in the code.
    
    Java bring in Functional programming which is enabled by Lambda expressions ( a powerful tool to create concise code base.)
    
    - Lambda Expression
    - Stream API
    - Default and static methods in interface
    - Static methods
    - Functional Interface : Only one abstract method and any default and static methods. example : Runnable
    - Optional Class
    - Method reference
    - DateTime API
    - Nashorn , JavaScript Engine
- What is the use of interface and abstract class
    
    Both **Interfaces** and **Abstract Classes** help in achieving **abstraction** in Java, but they have different use cases.
    
    ---
    
    ## **1️⃣ Interface**
    
    - **100% abstraction** (until Java 8, after which it supports default/static methods).
    - **Defines a contract** that implementing classes must follow.
    - **Cannot have constructors** or instance variables.
    - **Supports multiple inheritance** (a class can implement multiple interfaces).
    
    🔹 **Real-World Example: Payment Gateway**
    
    ```java
    interface PaymentGateway {
        void processPayment(double amount);
    }
    
    class PayPal implements PaymentGateway {
        public void processPayment(double amount) {
            System.out.println("Processing payment of $" + amount + " via PayPal.");
        }
    }
    
    class Stripe implements PaymentGateway {
        public void processPayment(double amount) {
            System.out.println("Processing payment of $" + amount + " via Stripe.");
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            PaymentGateway payment = new PayPal();
            payment.processPayment(500);
        }
    }
    ```
    
    ✅ **Use Interface when:** You need multiple classes to follow the same contract but implement their logic differently.
    
    ---
    
    ## **2️⃣ Abstract Class**
    
    - **Can have both abstract and concrete methods**.
    - **Can have instance variables, constructors, and methods with implementations**.
    - **Cannot be instantiated** (objects can't be created directly).
    - **Supports single inheritance** (a class can extend only one abstract class).
    
    🔹 **Real-World Example: Vehicle**
    
    ```java
    abstract class Vehicle {
        String name;
    
        Vehicle(String name) { this.name = name; } // Constructor
    
        abstract void startEngine(); // Abstract method
    
        void stopEngine() { // Concrete method
            System.out.println(name + "'s engine stopped.");
        }
    }
    
    class Car extends Vehicle {
        Car() { super("Car"); }
    
        void startEngine() {
            System.out.println("Car engine started with key.");
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Vehicle myCar = new Car();
            myCar.startEngine();
            myCar.stopEngine();
        }
    }
    ```
    
    ✅ **Use Abstract Class when:** You want to share common code among multiple related classes while forcing them to implement specific behaviours.
    
    ---
    
    ## **3️⃣ Key Differences**
    
    | Feature | **Interface** | **Abstract Class** |
    | --- | --- | --- |
    | Methods | Only abstract (Java 8+ allows `default` & `static`) | Both abstract & concrete |
    | Fields | Only `public static final` (constants) | Can have instance variables |
    | Constructor | ❌ No constructor | ✅ Can have constructor |
    | Inheritance | Multiple inheritance (multiple interfaces) | Single inheritance |
    | Use Case | Defining a contract | Providing a base for related classes |
- What is diamond problem in interface
    
    In Java, **multiple inheritance is allowed via interfaces**, which can sometimes lead to **method conflicts** (Diamond Problem).
    
    It happens when:
    
    1️⃣ A method is inherited from **multiple interfaces**.
    
    2️⃣ There is an **ambiguity** about which method to execute.
    
    ```java
    class C implements A, B { 
        public void show() {  
            A.super.show();  // Explicitly calling A's method
            B.super.show();  // Explicitly calling B's method
        }
    }
    ```
    
- How default methods in interface handle cope up with Diamond problem.
    
    Using `InterfaceName.super.methodName()` to Resolve Conflicts
    
- Why Static methods were introduced in JAVA 8.
    
    Only reason for introducing static methods in interface is that you can call those methods with just interface name. No need to create class and then it’s object
    
- Predicated → java.util.function.Predicate
    
    Predicate is predefined Functional Interface (Having only 1 abstract method)
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    import java.util.function.Predicate;
    import java.util.stream.Collectors;
    
    public class PredicateExample {
        public static void main(String[] args) {
            // Creating a list of numbers
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
            
            // Creating predicates
            Predicate<Integer> isEven = num -> num % 2 == 0;
            Predicate<Integer> isGreaterThan5 = num -> num > 5;
            
            // Using a single predicate
            List<Integer> evenNumbers = numbers.stream()
                        .filter(isEven)
                    .collect(Collectors.toList());
            System.out.println("Even numbers: " + evenNumbers);
            
            // Chaining predicates with and()
            List<Integer> evenNumbersGreaterThan5 = numbers.stream()
                    .filter(isEven.and(isGreaterThan5))
                    .collect(Collectors.toList());
            System.out.println("Even numbers greater than 5: " + evenNumbersGreaterThan5);
            
            // Using or() to combine predicates
            List<Integer> evenOrGreaterThan5 = numbers.stream()
                    .filter(isEven.or(isGreaterThan5))
                    .collect(Collectors.toList());
            System.out.println("Numbers that are either even or greater than 5: " + evenOrGreaterThan5);
            
            // Using negate() to invert a predicate
            List<Integer> notEvenNumbers = numbers.stream()
                    .filter(isEven.negate())
                    .collect(Collectors.toList());
            System.out.println("Not even numbers (odd): " + notEvenNumbers);
            
            // Using predicate in a method
            System.out.println("Testing if 4 is even: " + testNumber(4, isEven));
            System.out.println("Testing if 7 is even: " + testNumber(7, isEven));
        }
        
        // Method that takes a predicate as parameter
        public static boolean testNumber(Integer number, Predicate<Integer> predicate) {
            return predicate.test(number);
        }
    }
    ```
    
- What are Functions
    
    Functions is also a predefined Functional Interface ( Having only 1 abstract method).
    
    The only abstract method of Function is apply(T t)
    
    R apply(T t)
    
    Given some input perform some operation on input and then produce/ return result (not necessary a boolean value).
    
    Function<Integer,Integer> squareMe=i→i*i;
    
- What is Consumer
    
    Consumer<T> → it will consume item. Consumers never return anything (never supply ), they just consume.
    
- What is Supplier Functional interface ?
    
    Supplier<R> → it will just supply required objects and will not take any input 
    
    it always going to supply never consume/ take any input
    
    No chaining as no input is given to supplier.
    
- Use of BiConsumer, BiPredicate, BiSupplier
    
    ## **1️⃣ `BiConsumer<T, U>` (Performs an operation on two inputs, no return)**
    
    🔹 **Use case:** Logging user details.
    
    ```java
    import java.util.function.BiConsumer;
    
    public class Main {
        public static void main(String[] args) {
            BiConsumer<String, Integer> logUser = (name, age) ->
                System.out.println(name + " is " + age + " years old.");
    
            logUser.accept("John", 25); // Output: John is 25 years old.
        }
    }
    
    ```
    
    ✅ **Use `BiConsumer` when you need to perform an action but don’t return a result.**
    
    ---
    
    ## **2️⃣ `BiPredicate<T, U>` (Tests a condition on two inputs, returns `boolean`)**
    
    🔹 **Use case:** Checking if a user is eligible for voting.
    
    ```java
    import java.util.function.BiPredicate;
    
    public class Main {
        public static void main(String[] args) {
            BiPredicate<Integer, String> isEligible = (age, country) ->
                age >= 18 && country.equals("India");
    
            System.out.println(isEligible.test(20, "India"));  // Output: true
            System.out.println(isEligible.test(16, "India"));  // Output: false
        }
    }
    ```
    
    ✅ **Use `BiPredicate` when you need to check a condition with two parameters.**
    
    ---
    
    ## **3️⃣ `BiSupplier<T, U>` (Not available, but can be simulated with `Supplier`)**
    
    🔹 **Use case:** Generating a random full name (first & last name).
    
    ```java
    import java.util.function.Supplier;
    
    public class Main {
        public static void main(String[] args) {
            Supplier<String> nameSupplier = () -> "John Doe";
    
            System.out.println(nameSupplier.get()); // Output: John Doe
        }
    }
    
    ```
    
    ✅ **Java does not have a `BiSupplier`, but you can return a `List`, `Pair`, or custom object instead.**
    
- Java 8 Interface Changes for default method
    
    Static and default methods were added because even if we wanna change or add any new method in interface then it requires change in all implementing class
    
- Static Methods in interface
    
    These are similar to default methods, just that its static hence it’s implementation cannot be changed in child classes.
    
    This implementing class need not and cannot change the definition.
    
- Categories Java Design Pattern ?
    - Creational patterns
    - Behavioural patterns
    - Structural patterns
    - J2EE patterns
- What are the Creational Design Pattern
    
    Creational design patterns are related to the way of creating objects.
    
    This pattern is used to define and describe how objects are created at class instantiation time.
    
    Eg. Employee emp= new Employee();
    
    here we are 
    
    Categories of Creational Design pattern
    
    - Factory method
    - Abstract Factory
    - Builder
    - Prototype
    - Singleton
- What is Factory Pattern ?
    
    In the Factory pattern, we don’t expose the creation logic to the client and refer the created object using a standard interface.
    
    The Factory Pattern is also known as Virtual Constructor.
    
    ```java
    // Product interface
    interface Vehicle {
        void drive();
    }
    
    // Concrete products
    class Car implements Vehicle {
        @Override
        public void drive() {
            System.out.println("Driving a car");
        }
    }
    
    class Motorcycle implements Vehicle {
        @Override
        public void drive() {
            System.out.println("Riding a motorcycle");
        }
    }
    
    class Truck implements Vehicle {
        @Override
        public void drive() {
            System.out.println("Driving a truck");
        }
    }
    
    // Factory class
    class VehicleFactory {
        public Vehicle createVehicle(String type) {
            if (type == null || type.isEmpty()) {
                return null;
            }
            
            switch (type.toLowerCase()) {
                case "car":
                    return new Car();
                case "motorcycle":
                    return new Motorcycle();
                case "truck":
                    return new Truck();
                default:
                    throw new IllegalArgumentException("Unknown vehicle type: " + type);
            }
        }
    }
    
    // Client code
    public class FactoryPatternExample {
        public static void main(String[] args) {
            VehicleFactory factory = new VehicleFactory();
            
            // Create different vehicles using the factory
            Vehicle car = factory.createVehicle("car");
            car.drive();
            
            Vehicle motorcycle = factory.createVehicle("motorcycle");
            motorcycle.drive();
            
            Vehicle truck = factory.createVehicle("truck");
            truck.drive();
        }
    }
    ```
    
    Steps:
    
    1. Create main class which call factory class.
    2. Factory class returns required class instance.
- What is Abstract Factory Design pattern ?
    
    The **Abstract Factory Pattern** is a **creational design pattern** used to create families of related objects **without specifying their concrete classes**.
    
    ---
    
    ## **1. When to Use Abstract Factory Pattern?**
    
    ✅ When you need to create objects of related families without exposing their concrete implementations.
    
    ✅ When the system should be independent of how objects are created.
    
    ✅ When you want to provide multiple variants of objects (e.g., different UI themes, database connectors).
    
    Let's create a UI factory that can generate **buttons** and **checkboxes** for different operating systems (**Windows & MacOS**).
    
    ---
    
    ### **Step 1: Define Abstract Products**
    
    ```java
    
    // Abstract product - Button
    interface Button {
        void click();
    }
    
    // Abstract product - Checkbox
    interface Checkbox {
        void check();
    }
    
    ```
    
    ---
    
    ### **Step 2: Create Concrete Product Implementations**
    
    ```java
    
    // Concrete Product - Windows Button
    class WindowsButton implements Button {
        public void click() {
            System.out.println("Windows Button Clicked");
        }
    }
    
    // Concrete Product - MacOS Button
    class MacOSButton implements Button {
        public void click() {
            System.out.println("MacOS Button Clicked");
        }
    }
    
    // Concrete Product - Windows Checkbox
    class WindowsCheckbox implements Checkbox {
        public void check() {
            System.out.println("Windows Checkbox Checked");
        }
    }
    
    // Concrete Product - MacOS Checkbox
    class MacOSCheckbox implements Checkbox {
        public void check() {
            System.out.println("MacOS Checkbox Checked");
        }
    
    ```
    
    ---
    
    ### **Step 3: Create an Abstract Factory**
    
    ```java
    // Abstract Factory
    interface GUIFactory {
        Button createButton();
        Checkbox createCheckbox();
    }
    
    ```
    
    ---
    
    ### **Step 4: Implement Concrete Factories**
    
    ```java
    // Concrete Factory - Windows
    class WindowsFactory implements GUIFactory {
        public Button createButton() {
            return new WindowsButton();
        }
    
        public Checkbox createCheckbox() {
            return new WindowsCheckbox();
        }
    }
    
    // Concrete Factory - MacOS
    class MacOSFactory implements GUIFactory {
        public Button createButton() {
            return new MacOSButton();
        }
    
        public Checkbox createCheckbox() {
            return new MacOSCheckbox();
        }
    }
    
    ```
    
    ---
    
    ### **Step 5: Client Code**
    
    ```java
    
    public class AbstractFactoryDemo {
        public static void main(String[] args) {
            // Simulating OS detection (Windows or MacOS)
            String os = "Windows"; // Change to "MacOS" to test MacOS UI
    
            GUIFactory factory;
    
            if (os.equalsIgnoreCase("Windows")) {
                factory = new WindowsFactory();
            } else {
                factory = new MacOSFactory();
            }
    
            // Create UI elements
            Button button = factory.createButton();
            Checkbox checkbox = factory.createCheckbox();
    
            // Use UI elements
            button.click();
            checkbox.check();
        }
    }
    
    ```
    
- What is Singleton Design Pattern ?
    
    ## 🧠 What is the Singleton Design Pattern?
    
    > The Singleton Pattern ensures that only one instance of a class is created and provides a global point of access to that instance.
    > 
    
    ---
    
    ## 💡 When to Use?
    
    - When you need to **manage shared resources** (e.g., logging, configuration, DB connection pool).
    - When creating multiple instances would lead to **inconsistent behavior or waste of resources**.
    
    ---
    
    ## 📦 Real-Life Example
    
    Think of a **printer spooler** or **logging system** — you don’t want multiple objects doing the same job. A **singleton ensures** there's only **one instance** used throughout the app.
    
    ---
    
    ## ✅ Basic Implementation in Java
    
    ```java
    java
    CopyEdit
    public class Singleton {
    
        // 1. Create a private static instance of the class
        private static Singleton instance;
    
        // 2. Private constructor to prevent instantiation
        private Singleton() {
            System.out.println("Singleton Instance Created");
        }
    
        // 3. Public method to provide access to the instance
        public static Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton(); // lazy initialization
            }
            return instance;
        }
    }
    
    ```
    
    ### Usage:
    
    ```java
    java
    CopyEdit
    public class Main {
        public static void main(String[] args) {
            Singleton s1 = Singleton.getInstance();
            Singleton s2 = Singleton.getInstance();
    
            System.out.println(s1 == s2);  // true
        }
    }
    
    ```
    
    ---
    
    ## 🛡️ Thread-Safe Singleton (Double-Checked Locking)
    
    ```java
    
    public class Singleton {
    
        private static volatile Singleton instance;
    
        private Singleton() {}
    
        public static Singleton getInstance() {
            if (instance == null) {
                synchronized (Singleton.class) {
                    if (instance == null) {
                        instance = new Singleton();  // Thread-safe lazy initialization
                    }
                }
            }
            return instance;
        }
    }
    
    ```
    
    ---
    
    ## 📌 Key Characteristics
    
    | Feature | Description |
    | --- | --- |
    | 🔁 Only one instance | Controls instantiation |
    | 🌍 Global access point | Can be accessed from anywhere |
    | 🔒 Can be thread-safe | Prevents issues in multithreading |
    
    ---
    
    ## 🔥 Bonus: Singleton in Spring Boot
    
    Spring beans by default are **Singleton scoped**:
    
    ```java
    java
    CopyEdit
    @Service
    public class MyService {
        // Spring will make sure only one instance exists
    }
    ```
    
    ---
    
    ## 🧠 TL;DR
    
    - Singleton ensures **only one object** is created.
    - Use when **shared resource or global access** is required.
    - Can be made **thread-safe** using double-checked locking.
    - In Spring Boot, **beans are Singleton by default**.
    
    ---
    
- What is Prototype Design Pattern ?
    
    ## 🧠 What is the Prototype Design Pattern?
    
    > The Prototype Pattern allows you to create new objects by copying (cloning) an existing object, instead of creating from scratch.
    > 
    
    ---
    
    ## 📦 Real-Life Example
    
    Imagine a form template:
    
    You don’t want to build the whole form from scratch each time, you just want to **duplicate an existing form**, tweak it a bit, and move on.
    
    Same goes with objects in code — cloning saves time and resources.
    
    ---
    
    ## ✅ When to Use?
    
    - When creating a new object is **expensive** (e.g., complex object graph).
    - You want to **avoid building an object from scratch** every time.
    - You need **similar objects with small variations**.
    
    ---
    
    ## 🔧 How to Implement in Java?
    
    ### Step 1: Implement `Cloneable` interface
    
    ### Step 2: Override `clone()` method
    
    ```java
    java
    CopyEdit
    class Employee implements Cloneable {
        private int id;
        private String name;
    
        public Employee(int id, String name) {
            this.id = id;
            this.name = name;
        }
    
        // Getter and Setter (optional)
    
        @Override
        public Employee clone() {
            try {
                return (Employee) super.clone(); // shallow copy
            } catch (CloneNotSupportedException e) {
                throw new RuntimeException(e);
            }
        }
    
        public void show() {
            System.out.println("ID: " + id + ", Name: " + name);
        }
    }
    
    ```
    
    ### Usage:
    
    ```java
    java
    CopyEdit
    public class Main {
        public static void main(String[] args) {
            Employee emp1 = new Employee(101, "John");
            Employee emp2 = emp1.clone();
    
            emp2.show();  // ID: 101, Name: John
        }
    }
    
    ```
    
    ---
    
    ## 🧠 Shallow vs Deep Copy
    
    | Type | Description | Example |
    | --- | --- | --- |
    | **Shallow Copy** | Copies references of objects | One change may affect both |
    | **Deep Copy** | Copies the actual data | Independent objects |
    
    ---
    
    ## 🧪 In Spring: Prototype Scope
    
    In Spring, if you mark a bean as `@Scope("prototype")`, a **new object is created each time you request it**:
    
    ```java
    java
    CopyEdit
    @Component
    @Scope("prototype")
    public class MyPrototypeBean {
        public MyPrototypeBean() {
            System.out.println("Prototype bean created");
        }
    }
    
    ```
    
    ---
    
    ## 🧠 TL;DR
    
    | Feature | Prototype Pattern |
    | --- | --- |
    | What it does | Clones an object instead of creating from scratch |
    | Use case | When object creation is costly |
    | Java support | `Cloneable` + `clone()` |
    | Spring support | `@Scope("prototype")` |
    
- What is Builder Design Pattern ?
    
    ## 🧠 What is the Builder Design Pattern?
    
    > The Builder Pattern lets you construct complex objects by separating the construction process from the final representation.
    > 
    
    It’s perfect when an object has **many parameters**, especially **optional ones**, and you want a **readable and maintainable** way to build it.
    
    ---
    
    ## 🚫 Problem Without Builder
    
    Say you have a class like this:
    
    ```java
    java
    CopyEdit
    public class User {
        private String name;
        private int age;
        private String email;
        private String phone;
        private String address;
    
        public User(String name, int age, String email, String phone, String address) {
            // many params, hard to read
        }
    }
    
    ```
    
    Creating the object becomes messy and error-prone:
    
    ```java
    java
    CopyEdit
    User user = new User("John", 30, "john@email.com", "123456", "Somewhere");
    
    ```
    
    What if you want only some fields? You’ll end up needing **tons of constructors or setters** → confusing and cluttered.
    
    ---
    
    ## ✅ Builder Pattern to the Rescue!
    
    ### Step 1: Create a static inner builder class
    
    ### Step 2: Set fields in a **fluent style**
    
    ### Step 3: Build the object
    
    ---
    
    ## 🛠 Code Example:
    
    ```java
    java
    CopyEdit
    public class User {
        private String name;
        private int age;
        private String email;
        private String phone;
    
        // private constructor (only accessible via builder)
        private User(Builder builder) {
            this.name = builder.name;
            this.age = builder.age;
            this.email = builder.email;
            this.phone = builder.phone;
        }
    
        // Static inner class - Builder
        public static class Builder {
            private String name;
            private int age;
            private String email;
            private String phone;
    
            public Builder setName(String name) {
                this.name = name;
                return this;
            }
    
            public Builder setAge(int age) {
                this.age = age;
                return this;
            }
    
            public Builder setEmail(String email) {
                this.email = email;
                return this;
            }
    
            public Builder setPhone(String phone) {
                this.phone = phone;
                return this;
            }
    
            public User build() {
                return new User(this);
            }
        }
    
        public void display() {
            System.out.println("User: " + name + ", Age: " + age);
        }
    }
    
    ```
    
    ---
    
    ### 🔧 Usage:
    
    ```java
    java
    CopyEdit
    public class Main {
        public static void main(String[] args) {
            User user = new User.Builder()
                            .setName("Alice")
                            .setAge(28)
                            .setEmail("alice@example.com")
                            .build();
    
            user.display();  // Output: User: Alice, Age: 28
        }
    }
    
    ```
    
    ---
    
    ## 📦 Advantages of Builder Pattern
    
    | Benefit | Description |
    | --- | --- |
    | ✅ Readability | Fluent, chainable API (like `new Builder().setX().setY()`) |
    | ✅ Flexibility | Optional fields, avoids constructor overloading |
    | ✅ Immutable | Final object can be immutable if you don’t expose setters |
    | ✅ Maintains Clean Code | Keeps client code clean and intention clear |
    
    ---
    
    ## 🧪 Bonus: Builder in Lombok (Shortcut)
    
    If you're using **Project Lombok**, just use `@Builder` annotation:
    
    ```java
    java
    CopyEdit
    @Builder
    public class User {
        private String name;
        private int age;
        private String email;
    }
    
    ```
    
    Usage:
    
    ```java
    java
    CopyEdit
    User user = User.builder()
                    .name("Bob")
                    .age(35)
                    .build();
    
    ```
    
    ---
    
    ## 🧠 TL;DR
    
    | Aspect | Builder Design Pattern |
    | --- | --- |
    | Purpose | To build complex objects step-by-step |
    | Core idea | Separate construction from representation |
    | Use when | You have many constructor parameters (especially optional ones) |
    | Benefit | Clean, readable, flexible code |
    | Tools | Manual or with Lombok (`@Builder`) |
- What are the Structural Patterns ?
    
    ## 🧱 What are Structural Design Patterns?
    
    > Structural patterns focus on how classes and objects are composed to form larger structures while keeping them flexible and efficient.
    > 
    
    They help ensure that if one part of a system changes, the entire system doesn’t need to be rewritten.
    
    ---
    
    ## 🏗️ Common Structural Patterns
    
    Here’s a list of the 7 main structural design patterns:
    
    | Pattern | Purpose |
    | --- | --- |
    | **1. Adapter** | Convert one interface into another one the client expects |
    | **2. Bridge** | Separate abstraction from implementation so they can vary independently |
    | **3. Composite** | Treat individual objects and groups of objects in the same way |
    | **4. Decorator** | Add behavior to an object dynamically without altering its structure |
    | **5. Facade** | Provide a simplified interface to a complex subsystem |
    | **6. Flyweight** | Use sharing to support large numbers of fine-grained objects efficiently |
    | **7. Proxy** | Provide a placeholder or surrogate for another object to control access |
    
    ---
    
    ## 🔍 Short Explanations with Real-World Analogy
    
    | Pattern | Real-world analogy |
    | --- | --- |
    | **Adapter** | Power plug adapter that converts one plug type to another |
    | **Bridge** | TV remote: abstraction (`Remote`) is separate from implementation (`TV`) |
    | **Composite** | A folder contains files and other folders; you can treat both the same |
    | **Decorator** | Wrapping a gift with layers (paper, ribbon) — adds features |
    | **Facade** | A hotel receptionist — gives you one simple entry point to many services |
    | **Flyweight** | Characters in a document — share font objects instead of creating new for each |
    | **Proxy** | A lawyer who speaks on your behalf — controls access to the actual subject |
    
    ---
    
    ## ✅ Real-world Java Use Cases
    
    | Pattern | Used in... |
    | --- | --- |
    | **Adapter** | `java.util.Arrays#asList()` |
    | **Composite** | `javax.swing.JComponent` (contains child components) |
    | **Decorator** | `java.io.BufferedReader` (adds features to `Reader`) |
    | **Facade** | `javax.faces.context.FacesContext` |
    | **Proxy** | Spring AOP / Hibernate lazy loading uses Proxy |
    | **Flyweight** | `java.lang.Integer.valueOf()` (caches values) |
    
    ---
    
    ## 📌 TL;DR
    
    | Structural Pattern | Use When... |
    | --- | --- |
    | **Adapter** | You need to integrate incompatible interfaces |
    | **Bridge** | You want to separate abstraction from implementation |
    | **Composite** | You want to treat groups and individual objects uniformly |
    | **Decorator** | You want to add features to objects at runtime |
    | **Facade** | You want to simplify a complex system |
    | **Flyweight** | You want to minimize memory usage with shared objects |
    | **Proxy** | You need to control access or add behavior before/after calling an object |
- What is Heapdump ?
    
    ## 🧠 What is a Heap Dump?
    
    > A Heap Dump is a snapshot of the memory (heap) of a Java process at a specific point in time.
    > 
    > 
    > It contains **all the objects** that are in memory, their **classes**, **references**, and **values**.
    > 
    
    ---
    
    ## 🧩 Why is it useful?
    
    Heap dumps are extremely useful for:
    
    ✅ **Diagnosing memory leaks**
    
    ✅ **Finding what’s consuming memory** (e.g., huge `List`, large cache, etc.)
    
    ✅ **Debugging OutOfMemoryError (OOM)**
    
    ✅ Understanding object reference chains and object lifetimes
    
    ---
    
    ## 📍 When is Heap Dump created?
    
    It can be generated:
    
    - Automatically when an **OutOfMemoryError** occurs (with JVM flags)
    - Manually via tools like:
        - `jmap`
        - JVisualVM
        - Eclipse MAT
        - IntelliJ
        - Java Flight Recorder
    
    ---
    
    ## 🔧 How to Generate Heap Dump?
    
    ### ✅ Using JVM options (auto on OOM):
    
    ```bash
    bash
    CopyEdit
    -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/path/to/dumps
    
    ```
    
    ### ✅ Using `jmap`:
    
    ```bash
    bash
    CopyEdit
    jmap -dump:format=b,file=heapdump.hprof <pid>
    
    ```
    
    ---
    
    ## 🔍 How to Analyze a Heap Dump?
    
    You can open `.hprof` heap dump files using tools like:
    
    🛠 **Eclipse MAT (Memory Analyzer Tool)**
    
    🛠 **JVisualVM**
    
    🛠 **IntelliJ IDEA Profiler**
    
    🛠 **YourKit / JProfiler (paid tools)**
    
    These tools let you:
    
    - Find large objects (retained size)
    - Detect memory leaks
    - Explore object references
    
    ---
    
    ## 📦 Where is Heap Stored in JVM?
    
    - The **heap** is the part of memory where **Java objects** are allocated during runtime.
    - It's divided into:
        - **Young Generation** (Eden + Survivor)
        - **Old Generation** (Tenured)
        - (optionally) **Metaspace**
    
    ---
    
    ## ⚠️ Common Heap Dump Use Case: OOM Error
    
    Let’s say your app crashes with:
    
    ```bash
    bash
    CopyEdit
    java.lang.OutOfMemoryError: Java heap space
    
    ```
    
    If you had enabled the flag:
    
    ```bash
    bash
    CopyEdit
    -XX:+HeapDumpOnOutOfMemoryError
    
    ```
    
    It will create a file like:
    
    ```bash
    bash
    CopyEdit
    java_pid12345.hprof
    
    ```
    
    You can open that in Eclipse MAT to see **what caused the memory issue**.
    
    ---
    
    ## 🧠 TL;DR
    
    | Feature | Details |
    | --- | --- |
    | What is it? | Snapshot of all Java objects in heap |
    | Use case | Debug memory leaks, OutOfMemory errors |
    | Generated by | `jmap`, JVM crash, profiling tools |
    | Analyzed using | Eclipse MAT, VisualVM, IntelliJ, etc. |
- What is ConcurrentHashMap vs SynchronizedHashMap
    
    ## 🔄 Both are thread-safe, but **how** they provide thread safety is different.
    
    | Feature | `ConcurrentHashMap` | `SynchronizedHashMap` |
    | --- | --- | --- |
    | Thread-safe? | ✅ Yes | ✅ Yes |
    | Locking | Fine-grained locking (on segments or buckets) | Entire map is locked |
    | Performance | ⭐ High (better concurrency) | ❌ Slower (single thread at a time) |
    | Null Keys/Values | ❌ Not allowed | ✅ 1 null key, multiple null values |
    | Use Case | High-concurrency environments | Simpler, low-concurrency use cases |
    
    ---
    
    ## 🧠 1. **Synchronized HashMap** (aka synchronized `Map`)
    
    It's just a normal `HashMap` wrapped using `Collections.synchronizedMap()`:
    
    ```java
    java
    CopyEdit
    Map<String, String> map = Collections.synchronizedMap(new HashMap<>());
    
    ```
    
    💡 **Drawback**: Every method (read/write) is synchronized on the entire map, so **only one thread can access it at a time**, which leads to bottlenecks in high concurrency.
    
    ---
    
    ## 🧠 2. **ConcurrentHashMap**
    
    It is designed specifically for concurrency — instead of locking the entire map, it uses **lock-striping** (Java 7) or **bucket-level locking** (Java 8+).
    
    ```java
    java
    CopyEdit
    ConcurrentHashMap<String, String> map = new ConcurrentHashMap<>();
    map.put("name", "John");
    map.get("name");
    
    ```
    
    ✅ Multiple threads can read and write **safely and simultaneously**.
    
    ✅ Much better performance under high thread load.
    
    ---
    
    ## 🔥 Example: Difference in Locking
    
    ### Synchronized HashMap
    
    ```java
    java
    CopyEdit
    synchronized(map) {
       map.put("key", "value");
    }
    
    ```
    
    ### ConcurrentHashMap
    
    No need for external synchronization:
    
    ```java
    java
    CopyEdit
    map.put("key", "value"); // thread-safe internally
    
    ```
    
    ---
    
    ## 🧪 Null Handling
    
    | Map Type | Allows null keys? | Allows null values? |
    | --- | --- | --- |
    | HashMap | ✅ One null key | ✅ Multiple null values |
    | SynchronizedMap | ✅ One null key | ✅ Multiple null values |
    | ConcurrentHashMap | ❌ No null key | ❌ No null values |
    
    Trying to put a null in a `ConcurrentHashMap` will throw `NullPointerException`.
    
    ---
    
    ## 📦 When to Use What?
    
    | Scenario | Best Choice |
    | --- | --- |
    | High-performance multithreaded access | `ConcurrentHashMap` ✅ |
    | Low thread contention or legacy code | `SynchronizedMap` (ok) |
    | You need to store nulls | Not `ConcurrentHashMap` ❌ |
    
    ---
    
    ## 🧠 TL;DR
    
    - 🔐 Both are thread-safe.
    - 🚀 `ConcurrentHashMap` is more **scalable and performant**.
    - ❌ `ConcurrentHashMap` does **not allow null** keys or values.
    - ✅ Prefer `ConcurrentHashMap` in modern multithreaded apps.
- Some of the most important SQL Commands
    
    SELECT , UDPATE , DELETE, INSERT INTO , CREATE DATABASE , ALTER DATABASE, CREATE TABLE , ALTER TABLE , DROP TABLE , CREATE INDEX , DROP INDEX.
    
- What is metaspace
    
    Metaspace is a **memory space** in the JVM that **stores class metadata**. It was introduced in **Java 8**, replacing the **PermGen (Permanent Generation) space**.
    
    ### **🔹 Why was Metaspace Introduced? (Compared to PermGen)**
    
    | Feature | **PermGen (Java ≤7)** | **Metaspace (Java 8+)** |
    | --- | --- | --- |
    | **Stores** | Class metadata, interned Strings | Class metadata only |
    | **Memory Limit** | Fixed (configured with `-XX:MaxPermSize`) | **Dynamically grows** (default) |
    | **GC Impact** | Frequent `OutOfMemoryError` | Less likely due to dynamic sizing |
    | **Tuning Option** | `-XX:PermSize`, `-XX:MaxPermSize` | `-XX:MetaspaceSize`, `-XX:MaxMetaspaceSize` |
    
    ---
    
    ### **🔹 Where is Metaspace Located?**
    
    - Unlike **PermGen** (which was part of the heap), **Metaspace is allocated in native memory (off-heap)**.
    - This means it **grows dynamically** without being constrained by the heap.
- Date-Time API Java 8
    
    Issue with legacy Date and Calendar classes.
    
    - Mutable
    - Confusing
    - Limited support of zone ,diff
    
    > ***New Date and time classes***
    > 
    - LocalDate → Represents a date without a time zone.
    - LocalTime→ Represents a time without a date or time zone.
    - LocalDateTime→ Represents a date and time without a time zone.
    - ZonedDateTime→ Represents a date and time with a time zone.
    - Instant → Represents an instantaneous point on the timeline, typically used for machine timestamps.
    - Period→ Represents a period of time between two dates.
    - Duration → Represents a duration of time between two points in time.
    - DateTimeFormatter → Formats and parses date and time.
- **What is the role of the *ClassLoader*?**
    
    The **ClassLoader** is a crucial component in Java's runtime environment, responsible for loading class files into memory. It’s a part of JRE that dynamically loads java classes into the java Virtual machine. java code is compiled into class file by javac compiler and JVM executes Java program, by executing bytes codes written in class file. ClassLoader is responsible for loading class file from file system, network or any other source.
    
    Types of ClassLoader
    
    1. Bootstrap ClassLoader → search area rt.jar
    2. Extension ClassLoader → search area jre/lib/rt.jar
    3. Application ClassLoader
    4. Custom ClassLoader
- What is volatile keyword
    
    ## 🧠 What is `volatile` in Java?
    
    > volatile is a keyword used to indicate that a variable's value may be changed by multiple threads and should always be read directly from main memory.
    > 
    
    It ensures **visibility**, **not atomicity**.
    
    ---
    
    ## 🧾 Syntax:
    
    ```java
    private volatile boolean flag;
    ```
    
    ---
    
    ## 🔍 Why do we need `volatile`?
    
    In Java, threads can **cache variables** locally for performance.
    
    So one thread might not see the **latest value** updated by another thread.
    
    ### 🔥 Problem without `volatile`:
    
    ```java
    class MyTask extends Thread {
        private boolean running = true;
    
        public void run() {
            while (running) {
                // do something
            }
        }
    
        public void stopRunning() {
            running = false;
        }
    }
    
    ```
    
    Even if you call `stopRunning()`, the thread might still be looping forever!
    
    Why? Because `running` might be **cached in the thread's local memory**.
    
    ---
    
    ## ✅ Solution: Use `volatile`
    
    ```java
    class MyTask extends Thread {
        private volatile boolean running = true;
    
        public void run() {
            while (running) {
                // do something
            }
        }
    
        public void stopRunning() {
            running = false;
        }
    }
    ```
    
    Now the `running` variable is always read from **main memory**, so the update is visible to all threads.
    
    ---
    
    ## 🛑 Important: `volatile` is **not atomic**
    
    This is **not thread-safe**, even if you use `volatile`:
    
    ```java
    volatile int count = 0;
    
    public void increment() {
        count++;  // NOT atomic: read-modify-write
    }
    ```
    
    To make it atomic, use:
    
    ✅ `AtomicInteger`
    
    ✅ `synchronized` block
    
    ---
    
    ## 📦 Use Cases
    
    | Scenario | Use `volatile`? |
    | --- | --- |
    | Flag to stop thread | ✅ Yes |
    | Double-checked locking | ✅ Often used |
    | Counter with increment | ❌ No (use `AtomicInteger`) |
    
    ---
    
    ## 📌 Summary Table
    
    | Feature | Volatile Keyword |
    | --- | --- |
    | Thread Safety | ❌ No (visibility only, not atomic) |
    | Visibility | ✅ Yes |
    | Guarantees | Changes by one thread are visible to others |
    | Atomicity | ❌ No |
    | Performance | ✅ Lightweight (better than locking) |
    
    ---
    
    ## ✅ TL;DR
    
    - `volatile` ensures that **all threads see the latest value** of a variable.
    - It does **not make operations atomic**.
    - Use it for **flags, state variables**, etc., but **not** for counters or complex shared data.
- What is the difference between HashMap, LinkedHashMap and TreeMap
    
    ## 🔍 Quick Summary
    
    | Feature | `HashMap` | `LinkedHashMap` | `TreeMap` |
    | --- | --- | --- | --- |
    | **Ordering** | No ordering | Maintains insertion order | Sorted by keys (natural or custom) |
    | **Performance** | Fastest (O(1) for get/put) | Slightly slower than HashMap | Slower (O(log n) for get/put) |
    | **Null Keys/Values** | 1 null key, multiple null values | 1 null key, multiple null values | ❌ No null keys (null values allowed) |
    | **Implements** | `Map` | `Map` | `NavigableMap` (extends `SortedMap`) |
    | **Use Case** | General-purpose hash-based map | Order-preserving map | Sorted/ordered map |
    
    ---
    
    ## 🔧 Code Examples
    
    ### 1. `HashMap`
    
    ```java
    Map<String, String> map = new HashMap<>();
    map.put("b", "Banana");
    map.put("a", "Apple");
    map.put("c", "Cherry");
    
    System.out.println(map); // Order is not guaranteed
    ```
    
    🔄 Output (order may vary):
    
    ```java
    {a=Apple, b=Banana, c=Cherry}
    ```
    
    ---
    
    ### 2. `LinkedHashMap`
    
    ```java
    Map<String, String> map = new LinkedHashMap<>();
    map.put("b", "Banana");
    map.put("a", "Apple");
    map.put("c", "Cherry");
    
    System.out.println(map); // Maintains insertion order
    ```
    
    📦 Output:
    
    ```java
    
    {b=Banana, a=Apple, c=Cherry}
    ```
    
    ---
    
    ### 3. `TreeMap`
    
    ```java
    Map<String, String> map = new TreeMap<>();
    map.put("b", "Banana");
    map.put("a", "Apple");
    map.put("c", "Cherry");
    
    System.out.println(map); // Sorted by key
    ```
    
    📈 Output:
    
    ```java
    {a=Apple, b=Banana, c=Cherry}
    ```
    
    ---
    
    ## 🧠 When to Use What?
    
    | Need this... | Use this Map |
    | --- | --- |
    | Fast access, no order needed | `HashMap` |
    | Insertion order needs to be preserved | `LinkedHashMap` |
    | Keys need to be sorted | `TreeMap` |
    
    ---
    
    ## 🧪 Null Behavior
    
    | Map Type | Null Key | Null Values |
    | --- | --- | --- |
    | `HashMap` | ✅ One | ✅ Yes |
    | `LinkedHashMap` | ✅ One | ✅ Yes |
    | `TreeMap` | ❌ No | ✅ Yes |
    
    ---
    
    ## ✅ TL;DR
    
    - **`HashMap`**: Fast, unordered
    - **`LinkedHashMap`**: Maintains insertion order
    - **`TreeMap`**: Keeps keys sorted
- Why we are using the spring boot instead of traditional spring
    
    It’s simply the entire development process reduces the boilerplate code in spring framework we 
    
- Diff b/w @Controller and @RestController
    
    ### **🔹 1. `@Controller` (For Web Pages - MVC)**
    
    - Used in **Spring MVC** to handle **web pages (UI rendering)**.
    - Returns **views (HTML, JSP, Thymeleaf, etc.)** instead of raw data.
    - Requires `@ResponseBody` for returning JSON or plain text.
    
    ### **🔹 2. `@RestController` (For REST APIs)**
    
    - A combination of `@Controller + @ResponseBody`.
    - Used for **RESTful web services** to return **JSON/XML** responses.
    - No need to use `@ResponseBody` explicitly.
- Find the Second highest salary
    
    *Select Distinct Salary from Employee order by Salary DESC LIMIT 1 OFFSET 1;*
    
    *SELECT MAX(Salary) as SecondHighestSalary FROM Employee WHERE Salary < (Select MAX(Salary) FROM Employee;*
    
    | EmplD | Empname | Salary |
    | --- | --- | --- |
    | 1 | John | 50000 |
    | 2  | Smith | 60000 |
    | 3 | Raj | 70000 |
    | 4 | Priya | 80000 |
    | 5 | Peter | 60000 |
- What is Agile Methodology
    
    Agile methodology is an iterative and flexible approach to software development that emphasizes collaboration, customer feedback, and the ability to adapt to change throughout the development process. Unlike traditional methods, which often follow a linear, waterfall model, Agile encourages continuous improvement, frequent delivery of working software, and active participation from stakeholders.
    
    Here are some key aspects of Agile methodology:
    
    1. **Iterative Development**: Work is divided into small, manageable units called iterations or sprints. Each sprint usually lasts 1–4 weeks and delivers a usable product increment.
    2. **Collaboration**: Agile emphasizes teamwork between developers, stakeholders, and customers. Communication is frequent and essential for ensuring the product aligns with user needs.
    3. **Customer Feedback**: Feedback is gathered at the end of each iteration, allowing the team to adjust the product or project direction based on evolving customer requirements or market changes.
    4. **Responding to Change**: Agile is highly adaptive to change. The process encourages flexibility, allowing developers to change course as new requirements emerge, rather than sticking to a rigid plan.
    5. **Continuous Improvement**: After each iteration, the team holds a retrospective to discuss what went well, what didn’t, and how they can improve in the next iteration.
    6. **Focus on Individuals and Interactions**: Agile values individuals and communication over processes and tools, emphasizing the importance of a motivated, skilled team.
    
    Some common Agile frameworks include:
    
    - **Scrum**: A specific Agile framework that organizes work into fixed-length sprints and has roles like Scrum Master and Product Owner.
    - **Kanban**: A more flexible Agile framework focused on visualizing and managing the flow of work.
    - **Extreme Programming (XP)**: A software development methodology that focuses on technical excellence and practices such as pair programming and test-driven development.
- Did Functional Interface existed before java 8 ?
    
    **No, the term "Functional Interface" was introduced in Java 8, meaning it did not exist before that version**; however, interfaces with only one abstract method (the defining characteristic of a functional interface) were present in earlier Java versions, but they were not explicitly called "functional interfaces" until Java 8 brought the concept and the `@FunctionalInterface` annotation to formally identify them. 
    
    Other Interfaces are → Runnable and Callable
    
    ```java
    public interface Runnable{
        void run()
    }
    
    // Before java8
    Runnable runnable=new Runnable(){
        @Override
        public void run(){
            Sysout("Before java8");
        }
    }
    new Thread(runnable).start();
    
    // After Java 8
    Runnable runnable=()-> sysout("After java8");
    new Thread(runnable).start();
    ```
    
- What all Functional Interface introduced in java 8 ?
    - Predicate
    - Functional
    - Consumer
    - Supplier
    - BiConsumer
    - BiPredicate
- In Which Scenarios you should go for Set  ?
    
    Set Doesn’t store elements in the order way but it store only unique elements if you are setting the objects then object hashCode and equals method checks it’s not present the in the set already. It’s uses to store the unique elements .
    
    if you want to store the order of insertion then you can use TreeSet.
    
- String Vs StringBuilder Vs StringBuffer
    
    In Java, `String`, `StringBuilder`, and `StringBuffer` are all used to represent sequences of characters, but they differ significantly in terms of performance and behavior when modifying the string.
    
    Here’s a breakdown of the differences between them:
    
    ### 1. **String**
    
    - **Immutability**: `String` objects are immutable, meaning that once a `String` is created, its value cannot be changed.
    - **Performance**: When you perform operations that modify a `String` (e.g., concatenation), a new `String` object is created every time, which can lead to poor performance when making multiple modifications to a string in a loop or during heavy string operations.
    - **Usage**: Suitable for cases where the string is not going to change, or if only a few changes are required.
    
    **Example**:
    
    ```java
    public class StringExample {
        public static void main(String[] args) {
            String str = "Hello";
            str = str + " World"; // This creates a new String object
            System.out.println(str); // Output: Hello World
        }
    }
    ```
    
    ### 2. **StringBuilder**
    
    - **Mutability**: `StringBuilder` is mutable, meaning the content of the object can be changed without creating new objects.
    - **Performance**: It is more efficient than `String` when performing a lot of string concatenation or modification because it doesn't create new objects every time a change is made.
    - **Thread-safety**: `StringBuilder` is **not** thread-safe. If you're working in a single-threaded environment, it’s a good choice for string manipulation.
    - **Usage**: Ideal for scenarios where string manipulation occurs frequently, and thread safety is not a concern.
    
    **Example**:
    
    ```java
    public class StringBuilderExample {
        public static void main(String[] args) {
            StringBuilder sb = new StringBuilder("Hello");
            sb.append(" World");  // Modifies the same object
            System.out.println(sb);  // Output: Hello World
        }
    }
    
    ```
    
    ### 3. **StringBuffer**
    
    - **Mutability**: Like `StringBuilder`, `StringBuffer` is also mutable and allows efficient modification of strings.
    - **Performance**: It’s slower than `StringBuilder` because `StringBuffer` is **synchronized** to make it thread-safe.
    - **Thread-safety**: `StringBuffer` is thread-safe, meaning that multiple threads can safely modify the same `StringBuffer` object.
    - **Usage**: Best for scenarios where thread safety is important, but performance is not the highest priority.
    
    **Example**:
    
    ```java
    
    public class StringBufferExample {
        public static void main(String[] args) {
            StringBuffer sbf = new StringBuffer("Hello");
            sbf.append(" World");  // Modifies the same object
            System.out.println(sbf);  // Output: Hello World
        }
    }
    
    ```
    
    ### Comparison Table:
    
    | Feature | **String** | **StringBuilder** | **StringBuffer** |
    | --- | --- | --- | --- |
    | **Mutability** | Immutable | Mutable | Mutable |
    | **Thread-safety** | Not thread-safe | Not thread-safe | Thread-safe |
    | **Performance** | Poor for frequent changes | Efficient for single-threaded use | Slightly slower than `StringBuilder` due to synchronization |
    | **Use Case** | Constant strings, infrequent changes | Frequent string modifications (single-threaded) | Frequent string modifications (multi-threaded) |
    
    ### When to use:
    
    - **`String`**: When the value doesn't need to be changed, or only a few changes are needed.
    - **`StringBuilder`**: When the string needs to be modified frequently in a single-threaded environment.
    - **`StringBuffer`**: When you need to modify a string frequently in a multi-threaded environment where thread safety is required.
- What is Singleton class. What are the steps to create a singleton class.
    
    A **Singleton class** is a class in Java designed to ensure that only one instance of the class exists in the Java Virtual Machine (JVM). This pattern is useful when you need to control access to shared resources, such as a database connection or a configuration manager, and ensure that only one instance is created throughout the application's lifetime.
    
    ### Characteristics of a Singleton Class:
    
    1. **Only One Instance**: Only one instance of the Singleton class is created and shared.
    2. **Global Access**: The instance is accessible globally, but access is controlled, usually through a public static method.
    3. **Lazy or Eager Instantiation**: The instance can be created either when needed (lazy) or at the start of the application (eager).
    
    ### Steps to Create a Singleton Class
    
    1. **Private Constructor**: The constructor is made private to prevent instantiation from outside the class.
    2. **Static Variable for Instance**: A private static variable holds the single instance of the class.
    3. **Public Static Method**: A public static method is provided to return the instance of the class. This method is typically named `getInstance()`.
    4. **Thread Safety (optional)**: If the Singleton is to be used in a multithreaded environment, additional steps (like synchronization or using `volatile`) may be necessary to ensure thread safety.
    
    ### Types of Singleton Implementations:
    
    ### 1. **Eager Initialization Singleton**
    
    - The instance is created when the class is loaded, before any method is called.
    - Thread-safe by default.
    
    **Code Example**:
    
    ```java
    
    public class EagerSingleton {
        // Eagerly initialized instance
        private static final EagerSingleton instance = new EagerSingleton();
    
        // Private constructor to prevent instantiation
        private EagerSingleton() {}
    
        // Public method to provide access to the instance
        public static EagerSingleton getInstance() {
            return instance;
        }
    }
    
    ```
    
    ### 2. **Lazy Initialization Singleton**
    
    - The instance is created when `getInstance()` is called for the first time.
    - Not thread-safe by default, so additional measures need to be taken for thread safety.
    
    **Code Example**:
    
    ```java
    public class LazySingleton {
        // Instance is declared but not initialized
        private static LazySingleton instance;
    
        // Private constructor to prevent instantiation
        private LazySingleton() {}
    
        // Public method to provide access to the instance
        public static LazySingleton getInstance() {
            if (instance == null) {
                instance = new LazySingleton(); // Creates the instance only once
            }
            return instance;
        }
    }
    
    ```
    
    ### 3. **Thread-Safe Singleton (using synchronized method)**
    
    - The `getInstance()` method is synchronized to ensure thread safety.
    - This can introduce performance overhead, especially if `getInstance()` is called frequently.
    
    **Code Example**:
    
    ```java
    
    public class ThreadSafeSingleton {
        private static ThreadSafeSingleton instance;
    
        private ThreadSafeSingleton() {}
    
        public static synchronized ThreadSafeSingleton getInstance() {
            if (instance == null) {
                instance = new ThreadSafeSingleton();
            }
            return instance;
        }
    }
    
    ```
    
    ### 4. **Double-Checked Locking Singleton (Thread-Safe & Performance Optimized)**
    
    - Uses a `synchronized` block with a double check to reduce the overhead of synchronization.
    - Requires the instance to be marked as `volatile` to ensure visibility between threads.
    
    **Code Example**:
    
    ```java
    
    public class DoubleCheckedLockingSingleton {
        private static volatile DoubleCheckedLockingSingleton instance;
    
        private DoubleCheckedLockingSingleton() {}
    
        public static DoubleCheckedLockingSingleton getInstance() {
            if (instance == null) {
                synchronized (DoubleCheckedLockingSingleton.class) {
                    if (instance == null) {
                        instance = new DoubleCheckedLockingSingleton();
                    }
                }
            }
            return instance;
        }
    }
    ```
    
    ### 5. **Bill Pugh Singleton (using static inner class)**
    
    - This is a more modern and efficient approach to implement the Singleton pattern.
    - The inner static class is not loaded until the `getInstance()` method is called, making it thread-safe and efficient.
    - It is also lazy-loaded and ensures that the instance is created only when needed.
    
    **Code Example**:
    
    ```java
    java
    CopyEdit
    public class BillPughSingleton {
        // Inner static class responsible for instance creation
        private static class SingletonHelper {
            // Static final instance, loaded only when required
            private static final BillPughSingleton INSTANCE = new BillPughSingleton();
        }
    
        // Private constructor to prevent instantiation
        private BillPughSingleton() {}
    
        // Public method to provide access to the instance
        public static BillPughSingleton getInstance() {
            return SingletonHelper.INSTANCE;
        }
    }
    
    ```
    
    ### Advantages of Singleton Class:
    
    - **Control over Instance Creation**: Ensures that only one instance is created and provides a single point of access.
    - **Memory Efficiency**: Saves memory by reusing the same instance.
    - **Global Access**: Provides global access to the instance across the application.
    
    ### Disadvantages:
    
    - **Global State**: A singleton introduces global state to an application, which can sometimes lead to undesirable dependencies and difficulties in testing.
    - **Difficult to Mock**: Since the Singleton controls its own instantiation, it can be hard to mock in unit tests.
- What is difference between Collector and Collectors.
    
    In Java Streams API, **`Collector`** and **`Collectors`** are closely related but **serve different roles**. Here’s a simple explanation to help distinguish them:
    
    ---
    
    ### 🔹 `Collector` (Interface)
    
    - `Collector` is an **interface** in the `java.util.stream` package.
    - It defines **how** to collect data from a stream — for example, accumulating elements into a list, set, map, string, etc.
    - You typically don’t use `Collector` directly — instead, you use implementations provided by the `Collectors` utility class.
    
    ### Interface Definition:
    
    ```java
    public interface Collector<T, A, R> {
        // T - input element type
        // A - mutable accumulator type
        // R - result type
    }
    ```
    
    ---
    
    ### 🔹 `Collectors` (Utility Class)
    
    - `Collectors` is a **final class** that provides **ready-made static methods** which return instances of `Collector`.
    - It's part of the `java.util.stream` package.
    - You use `Collectors` when you want to collect the result of a stream operation into a collection or other form.
    
    ### Example Collectors:
    
    ```java
    Collectors.toList()
    Collectors.toSet()
    Collectors.joining()
    Collectors.groupingBy()
    Collectors.mapping()
    ```
    
    ---
    
    ### ✅ Code Example:
    
    ```java
    import java.util.*;
    import java.util.stream.*;
    
    public class CollectorExample {
        public static void main(String[] args) {
            List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    
            // Using a Collector provided by Collectors
            List<String> upperCaseNames = names.stream()
                                               .map(String::toUpperCase)
                                               .collect(Collectors.toList());
    
            System.out.println(upperCaseNames); // Output: [ALICE, BOB, CHARLIE]
        }
    }
    ```
    
    - Here, `Collectors.toList()` is a static method from the `Collectors` class.
    - It returns an object that implements the `Collector` interface.
    
    ---
    
    ### 📌 Summary
    
    | Feature | `Collector` | `Collectors` |
    | --- | --- | --- |
    | Type | Interface | Final utility class |
    | Purpose | Defines a contract for collecting | Provides ready-to-use collector implementations |
    | Usage | Used indirectly | Used directly to collect stream results |
    | Located in | `java.util.stream` | `java.util.stream` |
- What is the difference between ListIterator and Iterator ?
    
    Iterator and ListIterator is used to traverse a list.
    
    Iterator is used to traverse only forward traversing is allowed here - > List , Map and Set
    
    ListIterator is used to traverse only List forward and backward both traversing is allowed here
    
    ---
    
    How to traverse Map
    
    Extra : - map.entrySet().removeIf(entry -> entry.getKey().equals("one"));
    
    ### **Summary of Methods**
    
    | **Method** | **Usage** | **Applicable Map Methods** |
    | --- | --- | --- |
    | `entrySet()` | Traverse key-value pairs. | `map.entrySet()` |
    | `keySet()` | Traverse keys only. | `map.keySet()` |
    | `values()` | Traverse values only. | `map.values()` |
    | `forEach()` | Lambda-based traversal (Java 8+). | `map.forEach()` |
    | Streams API | Functional-style traversal and filtering. | `map.entrySet().stream()` |
    | `Enumeration` | Legacy style (for `Hashtable`). | `table.keys()`, `table.elements()` |
    
    ---
    
    ### **Key Differences Between Iterator and ListIterator**
    
    | **Feature** | **Iterator** | **ListIterator** |
    | --- | --- | --- |
    | **Traversal Direction** | Forward only. | Forward and backward. |
    | **Applicable Collections** | `List`, `Set`, `Map` (indirectly). | Only `List`. |
    | **Element Modification** | Can only remove elements. | Can add, remove, or modify elements. |
    | **Index Access** | Not supported. | Provides access to indices. |
    | **Introduction** | Java 1.2. | Java 1.2. |
- How to create a prefilled Map.
    
    ## **1️⃣ Using `Map.of()` (Java 9+)**
    
    ```java
    import java.util.Map;
    
    public class Main {
        public static void main(String[] args) {
            Map<String, Integer> map = Map.of(
                "Apple", 10,
                "Banana", 20,
                "Cherry", 30
            );
    
            System.out.println(map);
        }
    }
    ```
    
    ✅ **Pros:** Immutable, compact, best for small maps.
    
    ❌ **Cons:** Limited to **10 entries**.
    
    ---
    
    ## **2️⃣ Using `Map.ofEntries()` (For More than 10 Entries)**
    
    ```java
    
    import java.util.Map;
    
    public class Main {
        public static void main(String[] args) {
            Map<String, Integer> map = Map.ofEntries(
                Map.entry("Apple", 10),
                Map.entry("Banana", 20),
                Map.entry("Cherry", 30),
                Map.entry("Date", 40)
            );
    
            System.out.println(map);
        }
    }
    
    ```
    
    ✅ **Pros:** Immutable, supports more than 10 entries.
    
    ❌ **Cons:** Still **not modifiable**.
    
    ---
    
    ## **3️⃣ Using `HashMap` with `put()` (Mutable Map)**
    
    ```java
    
    import java.util.HashMap;
    import java.util.Map;
    
    public class Main {
        public static void main(String[] args) {
            Map<String, Integer> map = new HashMap<>();
            map.put("Apple", 10);
            map.put("Banana", 20);
            map.put("Cherry", 30);
    
            System.out.println(map);
        }
    }
    
    ```
    
    ✅ **Pros:** **Mutable**, easy to modify later.
    
    ❌ **Cons:** More verbose.
    
    ---
    
    ## **4️⃣ Using Double Brace Initialization (Not Recommended)**
    
    ```java
    import java.util.HashMap;
    import java.util.Map;
    
    public class Main {
        public static void main(String[] args) {
            Map<String, Integer> map = new HashMap<>() {{
                put("Apple", 10);
                put("Banana", 20);
                put("Cherry", 30);
            }};
    
            System.out.println(map);
        }
    }
    
    ```
    
    ❌ **Cons:** Creates an **anonymous inner class**, causing **memory leaks**.
    
    ---
    
    ## **🚀 Best Choice?**
    
    | Approach | Mutability | Best For |
    | --- | --- | --- |
    | `Map.of()` | ❌ Immutable | Small maps (≤10) |
    | `Map.ofEntries()` | ❌ Immutable | Large maps (>10) |
    | `HashMap + put()` | ✅ Mutable | General use |
    | **Double Brace Initialization** | ✅ Mutable (⚠️ Memory Leak Risk) | Avoid in production |
- How can we make a class Immutable.
    
    An immutable class in Java is a class whose instances cannot be modified after they are created. This means that once an object of an immutable class is instantiated, its state remains constant throughout its lifecycle.
    
    Steps to Make a class Immutable
    
    1. Declare the class final so it can’t be extended
    2. Make all fields private so that direct access is not allowed.
    3. Don’t provide setter methods for variables.
    4. Make all mutable fields final so that it’s value can be assigned only once.
    5. Initialize all the fields via a constructor performing a deep copy.
    6. Perform cloning of objects in the getter methods to return a copy rather than returning the actual object reference.
- What is the difference between ArrayList and LinkedList ?
    
    
    | Feature | ArrayList | LinkedList |
    | --- | --- | --- |
    | **Implementation** | Dynamically resizable array | Doubly linked list |
    | **Storage** | Contiguous memory locations | Elements can be scattered in memory |
    | **Access** | Fast (O(1)) for random access (by index) | Slow (O(n)) for random access (traversal needed) |
    | **Insertion/Deletion (Middle)** | Slow (O(n)) - requires shifting elements | Fast (O(1)) - only pointer updates needed |
    | **Insertion/Deletion (End)** | Amortized O(1) - generally fast | Fast (O(1)) |
    | **Memory Usage** | Less overhead (just array storage) | More overhead (pointers for each element) |
    | **Use Cases** | Frequent random access, less frequent inserts/deletes | Frequent inserts/deletes, less random access |
    | **Resizing** | Involves copying elements to a new, larger array | No resizing needed (just adding/removing nodes) |
    | **Iterator Performance** | Fast | Fast |
    | **Search** | Can use binary search if sorted (O(log n)) | Linear search (O(n)) |
- What is the difference between HashSet and TreeSet ?
    
    
    | Feature | HashSet | TreeSet |
    | --- | --- | --- |
    | **Implementation** | Uses a hash table | Uses a self-balancing binary search tree (e.g., Red-Black Tree) |
    | **Ordering** | No guaranteed order (elements are unordered) | Elements are stored in sorted order (either natural or based on a Comparator) |
    | **Performance (add, remove, contains)** | Average O(1), worst-case O(n) (rare) | O(log n) |
    | **Null Elements** | Allows one null element | May or may not allow null elements (depends on the Comparator and specific tree implementation) |
    | **Memory Usage** | Generally less memory overhead | Generally more memory overhead due to tree structure |
    | **Underlying Data Structure** | HashMap (keys are the elements) | NavigableMap (TreeMap) |
    | **Duplicates** | Does not allow duplicate elements | Does not allow duplicate elements |
    | **Use Cases** | When order is not important, fast operations | When sorted order is required |
    | **Iteration Order** | Unpredictable | Sorted |
- Internal working of HashMap and HashSet.
    
    **1. HashMap:**
    
    - **Key-Value Pairs:** `HashMap` stores data in the form of key-value pairs. Each key is unique (or, more accurately, each key's hash code is unique), and it maps to a specific value.
    - **Hashing:** The core idea behind `HashMap` is *hashing*. When you put a key-value pair into a `HashMap`, the key's `hashCode()` method is called to calculate a hash code. This hash code is then used to determine the *bucket* (an index in an internal array called a *table*) where the entry (key-value pair) should be stored.
    - **Buckets and Linked Lists (or Trees):** Multiple keys can potentially have the same hash code (this is called a *collision*). To handle collisions, each bucket in the table typically stores a linked list (or, in Java 8 and later, a balanced tree if the number of colliding keys in a bucket becomes large enough) of entries.
    - **Retrieval:** When you get a value using a key, the key's hash code is calculated again. This determines the bucket where the entry should be. The linked list (or tree) at that bucket is then searched to find the entry with the matching key (using the `equals()` method to compare keys).
    - **Capacity and Load Factor:** `HashMap` has a *capacity* (the initial size of the table) and a *load factor* (a threshold that determines when the table should be resized). When the number of entries exceeds the capacity multiplied by the load factor, the table is resized (usually doubled), and all the entries are rehashed and moved to the new table. Resizing is an expensive operation.
    - **Example:** Imagine you have a `HashMap<String, Integer>`. When you put a pair like ("apple", 1), the "apple" string's `hashCode()` is calculated. Let's say it results in 5. The `HashMap` might have a table of size 16. The entry would be stored in bucket 5. If "banana" also hashes to 5, it would be added to the linked list at bucket 5.
    
    **2. HashSet:**
    
    - **Wrapper around HashMap:** `HashSet` internally uses a `HashMap` to store its elements. The elements of the `HashSet` are actually stored as *keys* in the `HashMap`, and a dummy value (often just a `PRESENT` constant) is used as the *value*.
    - **Uniqueness:** The `HashMap`'s key uniqueness ensures that the `HashSet` does not allow duplicate elements. When you try to add an element that is already present, the `HashMap.put()` method will effectively replace the existing entry (with the same key) with the dummy value. Since the value is always the same dummy value, the set doesn't appear to change.
    - **Operations:** Adding, removing, and checking for the presence of an element in a `HashSet` are delegated to the underlying `HashMap`. Therefore, these operations have the same time complexity characteristics as `HashMap` operations.
    - **Example:** A `HashSet<String>` would use a `HashMap<String, Object>` (where `Object` is the type of the dummy value). When you add "apple" to the `HashSet`, it is added as a key to the internal `HashMap` with the dummy value.
    
    **Key Differences Summarized:**
    
    | Feature | HashMap | HashSet |
    | --- | --- | --- |
    | **Purpose** | Store key-value pairs | Store unique elements |
    | **Internal Storage** | Uses a hash table with linked lists/trees | Uses a HashMap (elements are keys) |
    | **Duplicates** | Keys must be unique | Elements must be unique |
    | **Core Function** | Mapping keys to values | Ensuring uniqueness of elements |
- What is hashCode and equals Contract in java ?
    
    In Java, the `hashCode()` and `equals()` methods are used for **object comparison** and **hash-based collections** like `HashMap`, `HashSet`, and `HashTable`.
    
    If you override `equals()`, you **must** override `hashCode()` to maintain consistency.
    
    ---
    
    ## **🔹 Contract Between `equals()` and `hashCode()`**
    
    1. **If two objects are equal (`equals()` returns `true`), they must have the same `hashCode()`.**
    2. **If two objects have the same `hashCode()`, they may or may not be equal (`equals()` can return `false`).**
    3. **If `equals()` is overridden, `hashCode()` must also be overridden to maintain consistency in hash-based collections.**
    
    ---
    
    ## **✅ Example of Proper `equals()` and `hashCode()` Implementation**
    
    ```java
    import java.util.Objects;
    
    class Employee {
        private int id;
        private String name;
    
        public Employee(int id, String name) {
            this.id = id;
            this.name = name;
        }
    
        @Override
        public boolean equals(Object obj) {
            if (this == obj) return true; // Same reference check
            if (obj == null || getClass() != obj.getClass()) return false;
    
            Employee employee = (Employee) obj;
            return id == employee.id && name.equals(employee.name);
        }
    
        @Override
        public int hashCode() {
            return Objects.hash(id, name); // Generates a consistent hashCode
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Employee e1 = new Employee(1, "John");
            Employee e2 = new Employee(1, "John");
    
            System.out.println(e1.equals(e2)); // ✅ true (because id and name are same)
            System.out.println(e1.hashCode() == e2.hashCode()); // ✅ true (consistent with equals)
        }
    }
    ```
    
    ---
    
    ## **🔹 Why is This Contract Important?**
    
    ### **Scenario: HashMap Without Proper `hashCode()` Implementation**
    
    If you override `equals()` but **don’t override `hashCode()`**, objects with the same data might be treated as different in a `HashMap`:
    
    ```java
    import java.util.HashMap;
    import java.util.Map;
    
    public class Main {
        public static void main(String[] args) {
            Map<Employee, String> map = new HashMap<>();
    
            Employee e1 = new Employee(1, "John");
            Employee e2 = new Employee(1, "John");
    
            map.put(e1, "Developer");
            System.out.println(map.get(e2)); // ❌ null if hashCode() is not overridden!
        }
    }
    ```
    
    ### **🔴 Issue:**
    
    - `e1` and `e2` are **logically equal** but have different hash codes (default implementation).
    - `HashMap` stores them in different hash buckets.
    - `map.get(e2)` returns `null` instead of `"Developer"`.
    
    ✅ **Fix:** Implement `hashCode()` properly!
    
    ---
    
    ## **🚀 Summary**
    
    🔹 `equals()` checks **logical equality**.
    
    🔹 `hashCode()` returns an **integer hash** for fast lookup in hash-based collections.
    
    🔹 If `equals()` returns `true`, `hashCode()` **must** return the same value.
    
    🔹 Always override **both methods together** when working with hash-based collections.
    
- If We have multiple constructor of same class is it possible to call one constructor from another constructor ?
    
    yes we can call using this() but this should be first statement inside the constructor .
    
- Why String is immutable ?
    
    Strings are immutable because of security reasons and for performance optimisation as well.
    
- What is the difference between Collection and Collections ?
    
    ## Collection:
    
    Represents a group of objects, known as elements. It's the root interface for many other collection interfaces like `List`, `Set`, and `Queue`.  It defines the basic operations that can be performed on a collection of objects, such as adding, removing, or iterating over elements.
    
    Declares common methods for working with collections, but it doesn’t provide concrete implementations . These methods includes add(), remove(), contains() , size() , iterator() etc.
    
    ## Collections:
    
    Collections is an utility class 
    
    **Purpose:** Provides static methods for operating on or manipulating collections. It offers a variety of helper methods for tasks like sorting, searching, synchronizing, and making collections immutable.  It doesn't represent a collection itself.  
    
    **Methods:** Contains static utility methods, not instance methods.
    
    Examples include Collections.sort(), Collections.binarySearch(), Collections.synchronizedList(), Collections.unmodifiableList(), etc.
    
    **Table summarising the differences:**
    
    | Feature | `Collection` (Interface) | `Collections` (Class) |
    | --- | --- | --- |
    | **Type** | Interface | Utility Class |
    | **Purpose** | Represents a group of objects | Provides utility methods for collections |
    | **Methods** | Instance methods (e.g., `add()`, `remove()`) | Static methods (e.g., `sort()`, `synchronizedList()`) |
    | **Relationship to Collections** | The root interface for collection types | A class that provides helper methods for collections |
    | **Example Use** | `List<String> names = new ArrayList<>();` | `Collections.sort(names);` |
- What will happen if the exception happens in the finally block ?
    
    If an exception occurs in the `finally` block in Java, it **overrides any previously thrown exception** from the `try` or `catch` block. This means the exception in the `finally` block will be the one propagated, and any earlier exception will be lost.
    
    ### Key Points:
    
    1. **Finally Block Execution**:
        - The `finally` block is always executed after the `try` block, whether or not an exception occurs.
        - If an exception is thrown in the `finally` block, it will replace any exception thrown from the `try` or `catch`.
    2. **Impact on Exception Handling**:
        - The exception in the `finally` block will take precedence, and the original exception (if any) from the `try` or `catch` block will not be propagated.
    
    ```java
    public class FinallyExceptionDemo {
        public static void main(String[] args) {
            try {
                System.out.println("Inside try block.");
                throw new RuntimeException("Exception in try block");
            } catch (Exception e) {
                System.out.println("Caught exception: " + e.getMessage());
            } finally {
                System.out.println("Inside finally block.");
                throw new RuntimeException("Exception in finally block");
            }
        }
    }
    
    ```
    
- Difference throw and Throws keyword ?
    
    **`throw` Keyword:**
    
    - **Purpose:** Used to *explicitly* throw an exception. You use `throw` when you want to create an exception object and signal that an exception has occurred.
    
    **`throws` Keyword:**
    
    - **Purpose:** Used in a method declaration to indicate that the method *might* throw one or more *checked* exceptions. It doesn't actually throw the exception; it simply declares that the method is capable of throwing it. This is important for checked exceptions because the compiler *forces* the calling code to either handle the exception or declare that it also throws the exception.
    
    **Key Differences Summarised:**
    
    | Feature | `throw` | `throws` |
    | --- | --- | --- |
    | **Purpose** | Explicitly throws an exception | Declares that a method might throw exceptions |
    | **Usage** | Followed by an exception object | Followed by a list of exception class names |
    | **Location** | Within a method (anywhere) | In the method signature |
    | **What it does** | Creates and throws an exception | Indicates that a method might throw exceptions |
    | **Checked Exceptions** | Can throw checked or unchecked exceptions | Primarily used for checked exceptions |
- Dis-advantages of using parallel stream ?
    - **Overhead:** Parallel streams introduce overhead (thread management, task splitting, combining results) that can negate benefits for small datasets.
    - **Complexity:** Debugging and reasoning about parallel code is significantly more complex than sequential code due to concurrency issues.
    - **Ordering:** Parallel streams don't preserve encounter order unless explicitly using ordered operations, making them unsuitable for order-dependent tasks.
    - **Thread Pool Saturation:** Parallel streams utilize a shared thread pool, potentially leading to contention and reduced performance if the pool is already heavily used.
    - **Data Dependencies:** Parallel streams are most effective with independent operations; data dependencies between elements hinder parallelization.
    - **Side Effects:** Managing side effects in parallel streams is challenging and requires careful synchronization to avoid race conditions and data corruption.
    - **Exception Handling:** Exception handling becomes more intricate in parallel streams, as exceptions from different threads can be difficult to collect and manage.
    - **Load Balancing:** Effective load balancing is crucial; uneven work distribution can lead to some threads idling while others are overloaded.
    - **Memory Consumption:** Parallel streams might increase memory usage due to the creation of copies of data or intermediate results for different threads.
    - **Not Universally Applicable:** Some operations are inherently sequential and cannot be effectively parallelized, making parallel streams unsuitable for all tasks.
- Advantages of Java Stream API
    - **Concise & Readable:** Declarative style, less code.
    - **Functional:** Lambda expressions, less side effects.
    - **Parallelizable:** Easy multi-core utilization.
    - **Lazy Evaluation:** Efficient processing.
    - **Composable:** Reusable pipelines.
    - **Simplified Processing:** Streamlined data manipulation.
- What is serialVersionUID? What is the use case of this in Serialization and Deserialization
    
    It is used for backward compatibility serialVersionUID store in the format of long .
    
    `private static final long serialVersionUID = 1L; // Or any other long value`
    
- Is HashMap sychnoronized if not then how can we used it
    
    No, `HashMap` in Java is *not* synchronized by default. This means that if multiple threads access and modify a `HashMap` concurrently, it can lead to data corruption, race conditions, and other unpredictable behavior.
    
    Here's how you can use `HashMap` in a multithreaded environment safely:
    
    1. **`Collections.synchronizedMap()`:**Java
        
        This is the simplest way to make a `HashMap` thread-safe.  The `Collections.synchronizedMap()` method takes a `HashMap` as an argument and returns a *synchronized wrapper* around it.  All access to the map is then synchronized, ensuring that only one thread can access it at a time.
        
        `Map<String, Integer> synchronizedHashMap = Collections.synchronizedMap(new HashMap<>());`
        
        **Advantages:** Simple and easy to use.
           **Disadvantages:** Can lead to performance bottlenecks because all operations are synchronized, even read operations.
        
    2. **`ConcurrentHashMap`:**Java
        
        This is generally the preferred approach for multithreaded scenarios. `ConcurrentHashMap` is designed for concurrent access. It uses internal *segments* (or buckets) and only synchronizes access within each segment. This allows multiple threads to read and write to different segments concurrently, significantly improving performance compared to a fully synchronized map.
        
        `ConcurrentHashMap<String, Integer> concurrentHashMap = new ConcurrentHashMap<>();`
        
        **Advantages:** Excellent performance for concurrent access.  Fine-grained locking.
        **Disadvantages:** Slightly more complex to use than `Collections.synchronizedMap()`, but worth it for the performance gains.
        
- When we should used Synchornized hashmap and concurrent hashmap
    - **Synchronized HashMap:** Simple, low-concurrency needs, where performance is not critical.
    - **ConcurrentHashMap:** High-concurrency scenarios, where performance is paramount.
- how to print all the keys and all the values of hashmap seperately ?
    
    map.keySet();
    
- Can we override and overload the static methods ?
    
    we cannot override the static method but we can overload the static methods.
    
- What is Space Complexity and Time Complexity ?
- ⚙️ What is the JIT Compiler?
    
    **JIT Compiler = Just-In-Time Compiler**
    
    It's a part of the **Java Virtual Machine (JVM)** that improves performance by **compiling bytecode into native machine code at runtime**.
    
    > 📌 Instead of interpreting code line by line, JIT compiles it into native code just before execution — hence the name "just in time."
    > 
    
    ---
    
    ## 🧠 How It Works (Step-by-Step)
    
    1. You write and compile Java code → it becomes **bytecode (.class files)**.
    2. JVM starts interpreting this bytecode.
    3. **JIT detects frequently used code paths ("hot spots")**.
    4. It **compiles those hot spots into native machine code** (e.g., CPU instructions).
    5. The next time that code runs, it runs **much faster**, because it's now **native code**, not interpreted.
    
    ---
    
    ## 🚀 Why JIT Compiler Is Useful
    
    | Benefit | Explanation |
    | --- | --- |
    | ⚡ **Performance Boost** | Converts repeated bytecode into fast native code |
    | 🔄 **Optimizations** | Inlines functions, removes dead code, etc. |
    | 💻 **Platform Independence + Speed** | JVM stays portable, but JIT makes it fast on any machine |
    
    ---
    
    ## 🔧 Types of JIT in JVM
    
    - **Client Compiler (C1):** Optimized for **quick startup** (used in GUI apps).
    - **Server Compiler (C2):** Optimized for **long-running applications** (like servers).
    - JVM can use **both together** via **Tiered Compilation**.
    
    ---
    
    ## 🧪 Bonus: Enable JIT Logging (for curiosity)
    
    You can run your app with:
    
    ```bash
    bash
    CopyEdit
    java -XX:+UnlockDiagnosticVMOptions -XX:+PrintCompilation MyApp
    
    ```
    
    To see which methods the JIT compiles at runtime.
    
    ---
    
    ## 💡 Real-Life Analogy
    
    Imagine you're interpreting a recipe in a foreign language every time you cook. That’s like the JVM interpreting bytecode. But if you translate it into your native language once and reuse it — that’s JIT compilation!
    
- What is fail fast and fail safe ?
    
    ## 🚨 Fail-Fast vs. ✅ Fail-Safe
    
    | Feature | **Fail-Fast** | **Fail-Safe** |
    | --- | --- | --- |
    | **Behavior** | Throws `ConcurrentModificationException` | Doesn’t throw exception |
    | **When It Happens** | If collection is modified **during iteration** | Can handle modifications during iteration |
    | **Example Classes** | `ArrayList`, `HashSet`, `HashMap` | `CopyOnWriteArrayList`, `ConcurrentHashMap` |
    | **Underlying Structure** | Iterates over the **original collection** | Iterates over a **copy or uses locking** |
    
    ---
    
    ## 🔥 Fail-Fast Example
    
    ```java
    java
    CopyEdit
    List<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    
    for (String s : list) {
        System.out.println(s);
        list.remove(s);  // ❌ throws ConcurrentModificationException
    }
    
    ```
    
    > ❗ You can't structurally modify the list while iterating.
    > 
    
    ---
    
    ## ✅ Fail-Safe Example
    
    ```java
    java
    CopyEdit
    CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
    list.add("A");
    list.add("B");
    
    for (String s : list) {
        System.out.println(s);
        list.remove(s);  // ✅ No exception
    }
    
    ```
    
    > Internally, CopyOnWriteArrayList makes a copy of the array while iterating, so it's safe.
    > 
    
    ---
    
    ## 🧠 Quick Summary:
    
    - **Fail-Fast**:
        - Fast performance.
        - Unsafe in multi-threaded modification unless using `Iterator.remove()`.
        - Throws `ConcurrentModificationException`.
    - **Fail-Safe**:
        - Safe for concurrent modifications.
        - Slightly more memory/performance overhead (copying or locking).
        - Does **not** throw exception.
- What is the purpose of Immutable Class ? why we used it
    
    ## 🧊 What is an Immutable Class?
    
    An **immutable class** is a class **whose objects cannot be modified** once created.
    
    > 📌 In other words: after the object is created, its state (data) can't change.
    > 
    
    ---
    
    ## 🎯 Purpose of Immutable Classes
    
    ### ✅ 1. **Thread Safety**
    
    - Immutable objects are **inherently thread-safe**.
    - No need for synchronization because state doesn't change.
    
    ### ✅ 2. **Caching and Performance**
    
    - Since the data never changes, immutable objects can be **safely reused** (e.g., as keys in `HashMap`).
    - Helps in **memoization** and **flyweight patterns**.
    
    ### ✅ 3. **Safe Sharing**
    
    - You can **freely share** immutable objects without worrying someone else might change them.
    
    ### ✅ 4. **Predictability & Simplicity**
    
    - Easier to reason about and debug.
    - Reduces bugs from unexpected mutations.
    
    ---
    
    ## 🔒 Real-world Immutable Classes
    
    - `String`
    - `Integer`, `Long`, `Double`
    - `LocalDate`, `LocalTime`, `LocalDateTime` (Java 8+ Date/Time API)
    
    ---
    
    ## 🛠️ How to Create an Immutable Class in Java
    
    ```java
    public final class Employee {
    
        private final int id;
        private final String name;
    
        public Employee(int id, String name) {
            this.id = id;
            this.name = name;
        }
    
        // Only getters, no setters
        public int getId() {
            return id;
        }
    
        public String getName() {
            return name;
        }
    }
    ```
    
    ### 🔐 Key Rules:
    
    1. Make class `final` (can't be subclassed).
    2. Make all fields `private` and `final`.
    3. No setters.
    4. Initialize fields only in the constructor.
    5. If a field is mutable (e.g., a list or date), **return a copy**, not the original.
    
    ---
    
    ## ⚠️ Why NOT Mutable?
    
    ```java
    java
    CopyEdit
    employee.setName("Hacked Name");  // Could happen in mutable class
    
    ```
    
    This causes bugs, especially when:
    
    - Multiple threads are using the same object
    - Using it as a key in collections like `HashMap` or `HashSet`
- What is a java HashSet ?
    - HashSet in Java is a class from the Collections Framework.
    - HashSet internally uses hashmap to store the unique elements.
    - It allows you to store multiple values in a collection using a hash table.
    - The hash table stores the values in an unordered method with the help of hashing mechanism.
    Here are some of the methods of HashSet in Java:
    ❖ add(E e) : Adds the specified element to the HashSet.
    ❖ contains(Object o) : Returns true if the specified element is present in the HashSet.
    ❖ remove(Object o) : Removes the specified element from the HashSet.
    ❖ size() : Returns the number of elements in the HashSet.
    ❖ isEmpty() : Returns true if the HashSet is empty.
    ❖ clear() : Removes all elements from the HashSet.
- Difference between Thread and Process ?
    
    
    | **Feature** | **Thread** | **Process** |
    | --- | --- | --- |
    | Memory Space | Shares the process's memory space | Has its own memory space |
    | Stack | Has its own stack | Has its own stack |
    | Creation | Created and destroyed by the JVM | Created and destroyed by the operating system |
    | Communication | Communicates with other threads through shared memory | Communicates with other processes through IPC mechanisms |
- How to debug the stream API ?
    
    ## 🧰 1. **Use `peek()` for Logging Intermediate Values**
    
    The `peek()` method is like a **debug print** for streams. It lets you look at each element as it flows through the pipeline.
    
    ### 🔍 Example:
    
    ```java
    List<String> names = List.of("Alice", "Bob", "Charlie");
    
    List<String> result = names.stream()
        .peek(n -> System.out.println("Original: " + n))
        .map(String::toUpperCase)
        .peek(n -> System.out.println("Mapped: " + n))
        .filter(n -> n.startsWith("A"))
        .peek(n -> System.out.println("Filtered: " + n))
        .collect(Collectors.toList());
    ```
    
    > Output:
    > 
    
    ```
    Original: Alice
    Mapped: ALICE
    Filtered: ALICE
    Original: Bob
    Mapped: BOB
    Original: Charlie
    Mapped: CHARLIE
    ```
    
    ---
    
    ## 🪝 2. **Use Breakpoints in Lambdas (IDE support)**
    
    If you're using **IntelliJ** or **Eclipse**, you can:
    
    - Put breakpoints **inside lambda functions** like `map()`, `filter()`, etc.
    - Step through each element being processed.
    
    Example:
    
    ```java
    
    .map(name -> {
        // Place breakpoint here
        return name.toUpperCase();
    })
    ```
    
    ---
    
    ## 🧪 3. **Extract Lambda Logic to Named Methods**
    
    It’s easier to debug when you extract lambdas into separate methods. You can then add logs or breakpoints there.
    
    ### ✅ Better for Debugging:
    
    ```java
    .stream()
    .map(this::toUpperCase)
    .filter(this::startsWithA)
    .collect(Collectors.toList());
    
    private String toUpperCase(String name) {
        System.out.println("Mapping: " + name);
        return name.toUpperCase();
    }
    
    private boolean startsWithA(String name) {
        boolean result = name.startsWith("A");
        System.out.println("Filtering: " + name + " -> " + result);
        return result;
    }
    ```
    
    ---
    
    ## 🔄 4. **Avoid Parallel Streams While Debugging**
    
    Parallel streams can make debugging confusing due to **non-deterministic execution order**. Stick to **sequential streams** while troubleshooting.
    
    ---
    
    ## 🧱 5. **Unit Test Each Step Separately**
    
    If the pipeline is complex:
    
    - Break it into smaller steps.
    - Test or log each part independently before chaining them.
    
    ---
    
    ## 🧠 Tip:
    
    Streams are lazy, so nothing executes until a **terminal operation** (`collect()`, `forEach()`, `count()`, etc.) is called. If nothing prints in `peek()`, check that your stream ends with a terminal operatio
    
- Java Thread Lifecycle
    
    **New** : A thread is in this state when it is create but now yet started.
    
    **Runnable**: After the start method is called , the thread become runnable. It’s ready to run and is waiting for CPU time.
    
    **Running**: The thread is in the state when it is executing
    
    B**locked /Waiting** : A thread is in the state when it is waiting for a resource or for another thread to perform an action.
    
    **Terminated** : A thread is in this state when it has finished executing 
    
- ClassNotFoundException vs NoClassDefFoundError
    
    ![image.png](assets/image%203.png)
    
- **Why is char[] preferred over String for passwords?**
    
    In Java, `String`s are immutable and are stored in the `String` pool. What this means is that, once a `String` is created, it stays in the pool in memory until being garbage collected. Therefore, even after you're done processing the string value (e.g., the password), it remains available in memory for an indeterminate period of time thereafter (again, until being garbage collected) which you have no real control over.
    
    Therefore, anyone having access to a memory dump can potentially extract the sensitive data and exploit it.
    
    In contrast, if you use a mutable object like a character array, for example, to store the value, you can set it to blank once you are done with it with confidence that it will no longer be available in memory.
    
- **Functional Interface methods**
    
    Here’s a table summarizing the built-in functional interfaces in Java and their methods:
    
    | **Functional Interface** | **Package** | **Abstract Method** | **Description** |
    | --- | --- | --- | --- |
    | **`Runnable`** | `java.lang` | `void run()` | Represents a task to be executed with no result. |
    | **`Supplier<T>`** | `java.util.function` | `T get()` | Supplies a result without taking any input. |
    | **`Consumer<T>`** | `java.util.function` | `void accept(T t)` | Performs an operation on a single input without returning a result. |
    | **`BiConsumer<T, U>`** | `java.util.function` | `void accept(T t, U u)` | Performs an operation on two inputs without returning a result. |
    | **`Function<T, R>`** | `java.util.function` | `R apply(T t)` | Transforms an input into an output. |
    | **`BiFunction<T, U, R>`** | `java.util.function` | `R apply(T t, U u)` | Transforms two inputs into an output. |
    | **`Predicate<T>`** | `java.util.function` | `boolean test(T t)` | Evaluates a condition on a single input and returns `true` or `false`. |
    | **`BiPredicate<T, U>`** | `java.util.function` | `boolean test(T t, U u)` | Evaluates a condition on two inputs and returns `true` or `false`. |
    | **`UnaryOperator<T>`** | `java.util.function` | `T apply(T t)` | Performs an operation on a single input and returns a result of the same type. |
    | **`BinaryOperator<T>`** | `java.util.function` | `T apply(T t1, T t2)` | Performs an operation on two inputs of the same type and returns a result. |
    | **`Callable<V>`** | `java.util.concurrent` | `V call() throws Exception` | Represents a task that returns a result and may throw an exception. |
    | **`Comparator<T>`** | `java.util` | `int compare(T o1, T o2)` | Compares two objects and determines their order. |
    
    ### **Key Characteristics of Functional Interfaces:**
    
    1. They have exactly one abstract method (SAM - Single Abstract Method).
    2. Can have default and static methods.
    3. Annotated with `@FunctionalInterface` (optional but recommended for clarity).
    
    ---
    
    ### **List of Built-in Functional Interfaces and Their Methods:**
    
    ### **1. `Runnable`**
    
    Represents a task that doesn't return a result.
    
    **Method:**
    
    ```java
    void run();
    
    ```
    
    ---
    
    ### **2. `Supplier<T>`**
    
    Represents a supplier of results.
    
    **Method:**
    
    ```java
    T get();
    
    ```
    
    ---
    
    ### **3. `Consumer<T>`**
    
    Represents an operation that accepts a single input and returns no result.
    
    **Method:**
    
    ```java
    void accept(T t);
    
    ```
    
    ---
    
    ### **4. `BiConsumer<T, U>`**
    
    Represents an operation that accepts two inputs and returns no result.
    
    **Method:**
    
    ```java
    void accept(T t, U u);
    
    ```
    
    ---
    
    ### **5. `Function<T, R>`**
    
    Represents a function that accepts one input and produces a result.
    
    **Method:**
    
    ```java
    R apply(T t);
    
    ```
    
    ---
    
    ### **6. `BiFunction<T, U, R>`**
    
    Represents a function that accepts two inputs and produces a result.
    
    **Method:**
    
    ```java
    R apply(T t, U u);
    
    ```
    
    ---
    
    ### **7. `Predicate<T>`**
    
    Represents a predicate (boolean-valued function) of one argument.
    
    **Method:**
    
    ```java
    boolean test(T t);
    
    ```
    
    ---
    
    ### **8. `BiPredicate<T, U>`**
    
    Represents a predicate (boolean-valued function) of two arguments.
    
    **Method:**
    
    ```java
    boolean test(T t, U u);
    
    ```
    
    ---
    
    ### **9. `UnaryOperator<T>`**
    
    Represents an operation on a single operand that produces a result of the same type.
    
    Extends `Function<T, T>`.
    
    **Method:**
    
    ```java
    T apply(T t);
    
    ```
    
    ---
    
    ### **10. `BinaryOperator<T>`**
    
    Represents an operation upon two operands of the same type, producing a result of the same type.
    
    Extends `BiFunction<T, T, T>`.
    
    **Method:**
    
    ```java
    T apply(T t1, T t2);
    
    ```
    
    ---
    
    ### **11. `Callable<V>`**
    
    Represents a task that returns a result and may throw an exception.
    
    **Method:**
    
    ```java
    V call() throws Exception;
    
    ```
    
    ---
    
    ### **12. `Comparator<T>`**
    
    Used for comparing two objects.
    
    **Method:**
    
    ```java
    int compare(T o1, T o2);
    ```
    
    ---
    
    ### **Custom Functional Interface Example:**
    
    You can create your own functional interface:
    
    ```java
    @FunctionalInterface
    public interface MyFunctionalInterface {
        void execute(); // Single abstract method
    }
    ```
    
    ---
    
    ### **How to Use Built-in Functional Interfaces:**
    
    ### Example: `Predicate<T>`
    
    ```java
    Predicate<Integer> isEven = (n) -> n % 2 == 0;
    System.out.println(isEven.test(4)); // Output: true
    ```
    
    ### Example: `Function<T, R>`
    
    ```java
    Function<String, Integer> stringLength = (s) -> s.length();
    System.out.println(stringLength.apply("Hello")); // Output: 5
    ```
    
    ### Example: `Supplier<T>`
    
    ```java
    Supplier<String> greeting = () -> "Hello, World!";
    System.out.println(greeting.get()); // Output: Hello, World!
    
    ```
    
    ### **Advantages of Functional Interfaces:**
    
    1. Simplify the use of lambda expressions.
    2. Reduce boilerplate code for anonymous inner classes.
    3. Increase code readability and functional programming capability in Java.
- **What is dynamic binding in java?**
    
    ### **Dynamic Binding in Java-**
    
    **Dynamic binding**, also called **late binding**, refers to the process of resolving method calls at **runtime** rather than at compile time. It allows Java to decide which method implementation to execute based on the object's actual runtime type, not the reference type.
    
    ---
    
    ### **Key Points about Dynamic Binding**
    
    1. **Runtime Resolution:**
        - The method that gets called is determined at runtime, based on the actual type of the object, even if the reference type is different.
    2. **Polymorphism:**
        - Dynamic binding is a cornerstone of **runtime polymorphism** in Java.
        - It works with **method overriding**, where a subclass provides its own implementation of a method defined in the superclass.
    3. **Only for Methods:**
        - Dynamic binding is applied to methods, not variables. Variable resolution is done at compile time (based on the reference type).
    4. **Enabled by the `virtual` method table:**
        - The JVM uses a virtual method table (vtable) to resolve overridden methods at runtime.
    
    ---
    
    ### **Example of Dynamic Binding**
    
    ```java
    class Animal {
        void sound() {
            System.out.println("Animal makes a sound");
        }
    }
    
    class Dog extends Animal {
        @Override
        void sound() {
            System.out.println("Dog barks");
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Animal animal = new Dog(); // Reference of type Animal, object of type Dog
            animal.sound(); // Dynamic binding happens here
        }
    }
    
    ```
    
    ### **Explanation:**
    
    - The reference variable `animal` is of type `Animal`.
    - The actual object is of type `Dog`.
    - At runtime, the JVM checks the actual type of the object (`Dog`) and calls the `sound` method in the `Dog` class, not in the `Animal` class.
    - **Output:** `Dog barks`
    
    ---
    
    ### **Static Binding vs. Dynamic Binding**
    
    | Feature | Static Binding | Dynamic Binding |
    | --- | --- | --- |
    | **Resolution Time** | Compile-time | Runtime |
    | **Applies To** | Methods that are `static`, `final`, or `private` | Overridden instance methods |
    | **Polymorphism** | Does not support polymorphism | Supports runtime polymorphism |
    | **Example** | `final` or `static` methods | Overridden methods in subclasses |
    
    ---
    
    ### **Example of Static Binding**
    
    ```java
    class Animal {
        static void staticMethod() {
            System.out.println("Static method in Animal");
        }
    }
    
    class Dog extends Animal {
        static void staticMethod() {
            System.out.println("Static method in Dog");
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Animal animal = new Dog();
            animal.staticMethod(); // Output: Static method in Animal
        }
    }
    
    ```
    
    ### **Explanation:**
    
    - Static methods are resolved based on the reference type (`Animal`), not the object type. Thus, the `Animal` class's method is called.
    
    ---
    
    ### **Why Use Dynamic Binding?**
    
    1. **Flexibility:**
        - It enables writing flexible and reusable code by leveraging polymorphism.
        - For example, you can write methods that accept superclass references but call subclass-specific implementations.
    2. **Code Maintainability:**
        - Adding new subclasses does not require modifying existing code.
    
    ---
    
    ### **When Does Dynamic Binding Not Happen?**
    
    - For **static**, **final**, or **private** methods because they are resolved at compile time.
    - Variable access is always determined by the reference type, not the object's runtime type.
- **Why Thread Local Object?**
    
    `ThreadLocal` is a Java class that provides thread-local variables. These variables are unique to each thread accessing them, meaning each thread has its own, isolated copy of the variable. The values stored in `ThreadLocal` are not shared between threads, which makes it a powerful tool for scenarios where thread safety is critical.
    
    ---
    
    ### **Why We Use `ThreadLocal` in This Context**
    
    In the context of dynamically switching databases or managing any context-sensitive data (e.g., tenant identification in multi-tenant systems, user-specific data), we use `ThreadLocal` because:
    
    1. **Thread Safety**: Each thread gets its own instance of the variable, ensuring no data corruption or sharing occurs across threads.
    2. **Context Isolation**: The value of the `ThreadLocal` variable is tied to the lifespan of the thread. This is ideal for storing per-request data in web applications, where each request runs in its own thread.
    3. **Dynamic Context Switching**: In our example, the current database key (`MYSQL` or `POSTGRES`) is stored in `ThreadLocal` so that:
        - The database selection is specific to the ongoing request (thread).
        - The context does not affect other threads handling different requests.
    
    ---
    
    ### **How `ThreadLocal` Works**
    
    Here’s an analogy:
    
    - Imagine a hotel with rooms. Each room has a personal safe (`ThreadLocal`), and each guest (thread) can store their belongings (data) in the safe. The safe in one room cannot be accessed by guests in other rooms.
    
    ---
    
    ### **Code Example for `ThreadLocal`**
    
    ### Example 1: Basic Usage
    
    ```java
    public class ThreadLocalExample {
    
        // Thread-local variable to store thread-specific data
        private static final ThreadLocal<String> threadLocal = new ThreadLocal<>();
    
        public static void main(String[] args) {
            Runnable task1 = () -> {
                threadLocal.set("Data for Thread 1");
                System.out.println(Thread.currentThread().getName() + " => " + threadLocal.get());
            };
    
            Runnable task2 = () -> {
                threadLocal.set("Data for Thread 2");
                System.out.println(Thread.currentThread().getName() + " => " + threadLocal.get());
            };
    
            // Each thread accesses its own value
            new Thread(task1).start();
            new Thread(task2).start();
        }
    }
    
    ```
    
    ### Output:
    
    ```
    Thread-0 => Data for Thread 1
    Thread-1 => Data for Thread 2
    ```
    
    ---
    
    ### **How It Fits into the Database Routing Example**
    
    1. **Why `ThreadLocal`?**
        - In a web application, each incoming request runs in its own thread.
        - To dynamically switch databases (MySQL or PostgreSQL), we need a way to store the selected database key (`MYSQL` or `POSTGRES`) specific to the current thread.
        - `ThreadLocal` is used to store this key (`DatabaseContext.setCurrentDatabase()`).
    2. **Flow in Our Example**:
        - Before processing a request:
            - Set the current database context using `ThreadLocal`.
        - The `RoutingDataSource` fetches this context using `ThreadLocal` when `determineCurrentLookupKey()` is called.
        - Once the request is completed, the context is cleared (`DatabaseContext.clear()`).
    
    ---
    
    ### **Thread Safety of `ThreadLocal`**
    
    `ThreadLocal` is thread-safe because:
    
    1. Each thread gets its own independent copy of the variable.
    2. No synchronization is needed, as there is no sharing of data across threads.
    
    ---
    
    ### **Best Practices with `ThreadLocal`**
    
    1. **Always Clear the Value**:
        - To avoid memory leaks in thread pools (e.g., in servlet containers), always clear the `ThreadLocal` value after the thread is done processing.
        
        ```java
        try {
            DatabaseContext.setCurrentDatabase("MYSQL");
            // Your logic
        } finally {
            DatabaseContext.clear(); // Always clear the value
        }
        
        ```
        
    2. **Avoid Overuse**:
        - Use `ThreadLocal` sparingly. It is most useful for scenarios like:
            - Database routing.
            - Per-request context storage (e.g., authentication data, tenant ID).
    3. **Use with Care in Thread Pools**:
        - In a thread pool, threads are reused. If `ThreadLocal` values are not cleared, they might leak into subsequent tasks executed by the same thread.
    
    ---
    
    ### **Conclusion**
    
    In our database routing example:
    
    - The `ThreadLocal` variable `CONTEXT` stores the database type for the current thread (request).
    - This ensures that each thread (request) uses the correct database without interfering with other threads.
    - `determineCurrentLookupKey()` in `RoutingDataSource` fetches the database key from `ThreadLocal`.
    
    Would you like me to dive deeper into any specific aspect?
    
- **Shard Already Stablished DB with one table or all tables having common key/column**
    
    Yes, it is possible to shard an already established database in an e-commerce scenario, even after it has been running for a while. However, it is a complex process that requires careful planning, implementation, and migration to avoid downtime and data loss.
    
    Below is a step-by-step guide to implementing sharding in an existing database setup with **Java (Spring Boot)** and **MySQL**:
    
    ---
    
    ### **1. Analyze and Plan the Sharding Strategy**
    
    Before implementing sharding, you need to carefully analyze:
    
    - **Why sharding is required?**
        - Is your database suffering from high read/write loads?
        - Are certain tables becoming bottlenecks?
    - **What is the sharding key?**
        - Choose a field that can divide the data evenly across shards. Common examples:
            - User-based sharding: Use `user_id`.
            - Geographic sharding: Use `region`.
            - Product sharding: Use `product_id`.
    
    **Example for e-commerce:**
    
    - Orders: Shard by `user_id` (users' orders are likely queried together).
    - Products: Shard by `category_id` (queries are often by product categories).
    - Customers: Shard by `region` (e.g., North, South, East, West).
    
    ---
    
    ### **2. Set Up the Shards**
    
    1. Create multiple MySQL databases (e.g., `shard1`, `shard2`, `shard3`, etc.).
    2. Partition your existing data into these shards based on the sharding key. For example:
        - For `user_id % 3 = 0`, store in `shard1`.
        - For `user_id % 3 = 1`, store in `shard2`.
        - For `user_id % 3 = 2`, store in `shard3`.
    
    ### **SQL Script for Creating Shards**
    
    ```sql
    CREATE DATABASE shard1;
    CREATE DATABASE shard2;
    CREATE DATABASE shard3;
    
    -- Create tables with the same schema in all shards
    USE shard1;
    CREATE TABLE orders (
        order_id INT AUTO_INCREMENT PRIMARY KEY,
        user_id INT NOT NULL,
        product_id INT NOT NULL,
        order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );
    
    -- Repeat for shard2 and shard3
    
    ```
    
    ---
    
    ### **3. Migrate Existing Data**
    
    ### **3.1 Configure the `JdbcTemplate` Beans**
    
    You need to define beans for each `JdbcTeplate`, connecting to the source and sharded databases. This is typically done in a Spring Boot `@Configuration` class:
    
    ```java
    java
    CopyEdit
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.jdbc.core.JdbcTemplate;
    import org.springframework.jdbc.datasource.DriverManagerDataSource;
    
    import javax.sql.DataSource;
    
    @Configuration
    public class DatabaseConfig {
    
        @Bean
        public DataSource oldDatabase() {
            DriverManagerDataSource dataSource = new DriverManagerDataSource();
            dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
            dataSource.setUrl("jdbc:mysql://localhost:3306/old_database");
            dataSource.setUsername("username");
            dataSource.setPassword("password");
            return dataSource;
        }
    
        @Bean
        public DataSource shard1() {
            DriverManagerDataSource dataSource = new DriverManagerDataSource();
            dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
            dataSource.setUrl("jdbc:mysql://localhost:3307/shard1");
            dataSource.setUsername("username");
            dataSource.setPassword("password");
            return dataSource;
        }
    
        @Bean
        public DataSource shard2() {
            DriverManagerDataSource dataSource = new DriverManagerDataSource();
            dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
            dataSource.setUrl("jdbc:mysql://localhost:3308/shard2");
            dataSource.setUsername("username");
            dataSource.setPassword("password");
            return dataSource;
        }
    
        @Bean
        public DataSource shard3() {
            DriverManagerDataSource dataSource = new DriverManagerDataSource();
            dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
            dataSource.setUrl("jdbc:mysql://localhost:3309/shard3");
            dataSource.setUsername("username");
            dataSource.setPassword("password");
            return dataSource;
        }
    
        @Bean
        public JdbcTemplate oldDbTemplate() {
            return new JdbcTemplate(oldDatabase());
        }
    
        @Bean
        public JdbcTemplate shard1Template() {
            return new JdbcTemplate(shard1());
        }
    
        @Bean
        public JdbcTemplate shard2Template() {
            return new JdbcTemplate(shard2());
        }
    
        @Bean
        public JdbcTemplate shard3Template() {
            return new JdbcTemplate(shard3());
        }
    }
    
    ```
    
    ### **3.2 Implement the `Order` Class**
    
    The `Order` class represents the table structure of the `orders` table:
    
    ```java
    java
    CopyEdit
    public class Order {
        private int orderId;
        private int userId;
        private int productId;
        private java.sql.Timestamp orderDate;
    
        public Order(int orderId, int userId, int productId, java.sql.Timestamp orderDate) {
            this.orderId = orderId;
            this.userId = userId;
            this.productId = productId;
            this.orderDate = orderDate;
        }
    
        public int getOrderId() {
            return orderId;
        }
    
        public int getUserId() {
            return userId;
        }
    
        public int getProductId() {
            return productId;
        }
    
        public java.sql.Timestamp getOrderDate() {
            return orderDate;
        }
    }
    
    ```
    
    Write a migration script in **Java** to move data from the old single database to the new shards.
    
    ### **3.3 Migration Example in Java**
    
    ```java
    import org.springframework.jdbc.core.JdbcTemplate;
    import java.util.List;
    
    public class DataMigration {
        private final JdbcTemplate oldDbTemplate; // Connection to the original database
        private final JdbcTemplate shard1Template;
        private final JdbcTemplate shard2Template;
        private final JdbcTemplate shard3Template;
    
        public DataMigration(JdbcTemplate oldDbTemplate, JdbcTemplate shard1Template,
                             JdbcTemplate shard2Template, JdbcTemplate shard3Template) {
            this.oldDbTemplate = oldDbTemplate;
            this.shard1Template = shard1Template;
            this.shard2Template = shard2Template;
            this.shard3Template = shard3Template;
        }
    
        public void migrateOrders() {
            // Fetch all data from the old database
            List<Order> orders = oldDbTemplate.query("SELECT * FROM orders", (rs, rowNum) -> {
                return new Order(rs.getInt("order_id"), rs.getInt("user_id"),
                                 rs.getInt("product_id"), rs.getTimestamp("order_date"));
            });
    
            // Insert data into the appropriate shard
            for (Order order : orders) {
                int shard = order.getUserId() % 3; // Determine the shard
                JdbcTemplate targetTemplate = (shard == 0) ? shard1Template
                                    : (shard == 1) ? shard2Template : shard3Template;
    
                targetTemplate.update("INSERT INTO orders (order_id, user_id, product_id, order_date) VALUES (?, ?, ?, ?)",
                        order.getOrderId(), order.getUserId(), order.getProductId(), order.getOrderDate());
            }
            System.out.println("Data migration completed!");
        }
    }
    
    ```
    
    ### **3.4 Use the `DataMigration` Class**
    
    Create a `@Component` or a `CommandLineRunner` to invoke the migration logic.
    
    ```java
    java
    CopyEdit
    import org.springframework.boot.CommandLineRunner;
    import org.springframework.stereotype.Component;
    
    @Component
    public class DataMigrationRunner implements CommandLineRunner {
    
        private final DataMigration dataMigration;
    
        public DataMigrationRunner(DataMigration dataMigration) {
            this.dataMigration = dataMigration;
        }
    
        @Override
        public void run(String... args) {
            dataMigration.migrateOrders();
        }
    }
    
    ```
    
    ### **3.5 Run the Application**
    
    Start the Spring Boot application. The `DataMigrationRunner` will automatically execute the migration logic.
    
    ---
    
    ### **4. Update Application Code**
    
    ### **Add Sharding Logic in Data Access Layer**
    
    1. Use `ThreadLocal` to store the current shard context.
    2. Create a `RoutingDataSource` to route queries to the correct shard.
    
    **Example:**
    
    ```java
    @Configuration
    public class ShardingConfig {
    
        @Bean
        public DataSource dataSource() {
            Map<Object, Object> shardDataSources = new HashMap<>();
            shardDataSources.put("shard1", shard1DataSource());
            shardDataSources.put("shard2", shard2DataSource());
            shardDataSources.put("shard3", shard3DataSource());
    
            RoutingDataSource routingDataSource = new RoutingDataSource();
            routingDataSource.setTargetDataSources(shardDataSources);
            return routingDataSource;
        }
    
        @Bean
        @Primary
        public DataSource shard1DataSource() {
            return DataSourceBuilder.create()
                    .url("jdbc:mysql://localhost:3306/shard1")
                    .username("root").password("password")
                    .build();
        }
    
        @Bean
        public DataSource shard2DataSource() {
            return DataSourceBuilder.create()
                    .url("jdbc:mysql://localhost:3306/shard2")
                    .username("root").password("password")
                    .build();
        }
    
        @Bean
        public DataSource shard3DataSource() {
            return DataSourceBuilder.create()
                    .url("jdbc:mysql://localhost:3306/shard3")
                    .username("root").password("password")
                    .build();
        }
    }
    
    ```
    
    ### **Use Shard Context to Route Queries**
    
    ```java
    public class ShardContext {
        private static final ThreadLocal<String> CONTEXT = new ThreadLocal<>();
    
        public static void setCurrentShard(String shardKey) {
            CONTEXT.set(shardKey);
        }
    
        public static String getCurrentShard() {
            return CONTEXT.get();
        }
    
        public static void clear() {
            CONTEXT.remove();
        }
    }
    
    ```
    
    ---
    
    ### **5. Query the Correct Shard**
    
    ### **Example Service Layer**
    
    ```java
    @Service
    public class OrderService {
        public List<Order> getOrdersByUser(int userId) {
            int shard = userId % 3;
            String shardKey = "shard" + (shard + 1);
            ShardContext.setCurrentShard(shardKey); // Set the shard context
    
            // Query the database (automatically routed to the correct shard)
            return jdbcTemplate.query("SELECT * FROM orders WHERE user_id = ?", new Object[]{userId},
                    (rs, rowNum) -> new Order(rs.getInt("order_id"), rs.getInt("user_id"),
                                              rs.getInt("product_id"), rs.getTimestamp("order_date")));
        }
    }
    
    ```
    
    ---
    
    ### **6. Monitoring and Maintenance**
    
    1. **Database Monitoring**: Use tools like **Prometheus** and **Grafana** to monitor the performance of each shard.
    2. **Scaling**: Add new shards as the application grows. You may need to re-distribute data when adding new shards.
    
    ---
    
    ### **Challenges of Retrofitting Sharding**
    
    1. **Data Migration**: Moving large datasets without downtime requires careful planning.
    2. **Application Complexity**: Adding sharding increases the complexity of your application’s logic.
    3. **Cross-Shard Queries**: Queries that span multiple shards (e.g., reports) require additional processing at the application level.
    
    ---
    
    Would you like help with any specific part of the implementation?
    
- **Sharding Already Stablished DB with multiple tables, not all tables having common key.**
    
    Sharding becomes more challenging when there isn't a common key across all tables. In this scenario, you need to evaluate the nature of your application, how the tables are related, and the typical queries executed. Below is a detailed explanation of how to approach sharding in such a scenario:
    
    ---
    
    ### **1. Analyze Table Relationships and Queries**
    
    You must first evaluate:
    
    - **Are the tables related?** (e.g., Foreign Key relationships)
    - **How are queries executed?**
        - If queries frequently join two or more tables, sharding might not work efficiently because cross-shard joins are expensive.
    - **What is the scale of each table?**
        - For instance, `user_order` may grow faster than `user` or `all_product`.
    
    ### Example:
    
    - `all_product`: A relatively static table that stores all product details.
    - `user_order`: A rapidly growing table since orders are frequently placed.
    - `user`: Contains details of registered users.
    - `user_product`: Tracks products purchased by each user.
    
    ---
    
    ### **2. Sharding Strategy**
    
    You can use different sharding keys for different tables if they don't share a common key.
    
    ### Sharding Example:
    
    - **`user` and `user_order`**:
        - Shard by `user_id` since these tables are user-centric.
    - **`all_product`**:
        - Keep this table unsharded or replicate it across all shards, as it's often small and static.
    - **`user_product`**:
        - Shard by `user_id` since this table is tightly coupled with users.
    
    ---
    
    ### **3. Sharding Implementation**
    
    ### **Create Shards**
    
    1. For `user` and `user_order`, distribute users across shards using `user_id % N` (N = number of shards).
    2. For `all_product`, replicate it across all shards (optional).
    3. For `user_product`, use `user_id % N` to ensure related user data stays together.
    
    ### **Schema Design**
    
    **Shard 1 (MySQL database):**
    
    ```sql
    -- user table
    CREATE TABLE user (
        user_id INT PRIMARY KEY,
        name VARCHAR(50),
        email VARCHAR(100)
    );
    
    -- user_order table
    CREATE TABLE user_order (
        order_id INT AUTO_INCREMENT PRIMARY KEY,
        user_id INT,
        product_id INT,
        order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        FOREIGN KEY (user_id) REFERENCES user(user_id)
    );
    
    -- all_product table (replicated)
    CREATE TABLE all_product (
        product_id INT PRIMARY KEY,
        name VARCHAR(50),
        price DECIMAL(10, 2)
    );
    
    -- user_product table
    CREATE TABLE user_product (
        user_id INT,
        product_id INT,
        purchased_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        PRIMARY KEY (user_id, product_id)
    );
    
    ```
    
    ---
    
    ### **4. Data Distribution**
    
    ### Insert Example:
    
    1. **Distribute user data:**
        - Users are distributed based on `user_id % N`.
        - For example, if `user_id = 101` and `N = 2`, `user` and related data (`user_order`, `user_product`) go to **shard 1**.
    2. **Replicate `all_product`:**
        - Each shard contains an identical copy of `all_product`.
    
    ---
    
    ### **5. Java Implementation**
    
    ### **Shard Context and Routing**
    
    ### Define ThreadLocal Context for Shard
    
    ```java
    public class ShardContext {
        private static final ThreadLocal<String> CONTEXT = new ThreadLocal<>();
    
        public static void setCurrentShard(String shardKey) {
            CONTEXT.set(shardKey);
        }
    
        public static String getCurrentShard() {
            return CONTEXT.get();
        }
    
        public static void clear() {
            CONTEXT.remove();
        }
    }
    
    ```
    
    ---
    
    ### **Routing DataSource**
    
    ### Configure Routing for Shards
    
    ```java
    @Configuration
    public class ShardingConfig {
    
        @Bean
        public DataSource routingDataSource() {
            Map<Object, Object> shardDataSources = new HashMap<>();
            shardDataSources.put("shard1", shard1DataSource());
            shardDataSources.put("shard2", shard2DataSource());
    
            RoutingDataSource routingDataSource = new RoutingDataSource();
            routingDataSource.setTargetDataSources(shardDataSources);
            return routingDataSource;
        }
    
        @Bean
        public DataSource shard1DataSource() {
            return DataSourceBuilder.create()
                    .url("jdbc:mysql://localhost:3306/shard1")
                    .username("root").password("password")
                    .build();
        }
    
        @Bean
        public DataSource shard2DataSource() {
            return DataSourceBuilder.create()
                    .url("jdbc:mysql://localhost:3306/shard2")
                    .username("root").password("password")
                    .build();
        }
    }
    
    ```
    
    ### Routing DataSource Class
    
    ```java
    public class RoutingDataSource extends AbstractRoutingDataSource {
        @Override
        protected Object determineCurrentLookupKey() {
            return ShardContext.getCurrentShard();
        }
    }
    
    ```
    
    ---
    
    ### **Service Layer**
    
    ### Select Shard Based on Query
    
    ```java
    @Service
    public class UserService {
    
        @Autowired
        private JdbcTemplate jdbcTemplate;
    
        public User getUserById(int userId) {
            // Determine the shard
            String shardKey = (userId % 2 == 0) ? "shard1" : "shard2";
            ShardContext.setCurrentShard(shardKey);
    
            // Query the database
            return jdbcTemplate.queryForObject("SELECT * FROM user WHERE user_id = ?", new Object[]{userId},
                    (rs, rowNum) -> new User(rs.getInt("user_id"), rs.getString("name"), rs.getString("email")));
        }
    
        public void clearContext() {
            ShardContext.clear();
        }
    }
    
    ```
    
    ---
    
    ### **6. Query Execution**
    
    ### Query Examples:
    
    1. Fetch user details:
        - User with `user_id = 101` -> routed to **shard1**.
    2. Fetch orders:
        - Orders for `user_id = 101` -> routed to **shard1**.
    
    ---
    
    ### **7. Challenges**
    
    1. **Cross-Shard Queries**:
        - If a query involves data from multiple shards, you'll need to aggregate the data in the application layer.
        - Example: Fetch total sales from all shards.
    2. **Consistency**:
        - Ensure that the data replicated across shards (e.g., `all_product`) stays consistent. Use tools like **Debezium** for replication or implement manual synchronization.
    
    ---
    
    ### **Summary**
    
    - Shard each table based on its access patterns.
    - Use `user_id` to shard user-centric tables like `user` and `user_order`.
    - Replicate static data (`all_product`) across all shards.
    - Configure routing logic in your application to determine the shard dynamically.
    - Implement cross-shard aggregation logic where necessary.
    
    Would you like a deeper dive into any specific aspect?
    
- **Partitioning in Database Side: Partitioning on Date**
    
    ### Example with MySQL
    
    MySQL supports **range partitioning**, which is a good fit for date-based data.
    
    ### Steps:
    
    1. **Create a Partitioned Table**
    Use the `PARTITION BY RANGE` clause to create partitions based on a `date` column.
    
    ```sql
    sql
    CopyEdit
    CREATE TABLE user_orders (
        order_id INT NOT NULL,
        user_id INT NOT NULL,
        order_date DATE NOT NULL,
        product_id INT NOT NULL,
        amount DECIMAL(10, 2),
        PRIMARY KEY (order_id, order_date)
    )
    PARTITION BY RANGE (YEAR(order_date)) (
        PARTITION p_before_2020 VALUES LESS THAN (2020),
        PARTITION p_2020 VALUES LESS THAN (2021),
        PARTITION p_2021 VALUES LESS THAN (2022),
        PARTITION p_2022 VALUES LESS THAN (2023),
        PARTITION p_future VALUES LESS THAN MAXVALUE
    );
    
    ```
    
    In this example:
    
    - The table is divided into partitions based on the `YEAR(order_date)` value.
    - Queries will be optimized by automatically targeting the relevant partition(s).
    1. **Insert Data**
    Data will automatically go into the correct partition based on the `order_date` column.
    
    ```sql
    sql
    CopyEdit
    INSERT INTO user_orders (order_id, user_id, order_date, product_id, amount)
    VALUES (1, 101, '2021-06-15', 201, 299.99);
    
    ```
    
    1. **Query Data**
    Queries automatically target the relevant partition.
    
    ```sql
    sql
    CopyEdit
    SELECT * FROM user_orders WHERE order_date BETWEEN '2021-01-01' AND '2021-12-31';
    
    ```
    
- **Java Spring Boot Side: Partitioning**
    
    ### 1. **Java Spring Boot Side: Partitioning**
    
    If your database doesn’t support partitioning or you want to implement logic on the application side, you can use **dynamic table routing**.
    
    ### Steps:
    
    1. **Design Separate Tables for Each Partition**
    Manually create tables like `user_orders_2020`, `user_orders_2021`, etc.
    2. **Dynamic Table Routing Logic**
    Write logic to route queries to the correct table based on the date range.
    
    ### Example Code:
    
    ```java
    import java.time.LocalDate;
    import java.time.format.DateTimeFormatter;
    
    public class PartitionUtils {
        public static String getPartitionTable(LocalDate orderDate) {
            int year = orderDate.getYear();
            return "user_orders_" + year;
        }
    }
    
    ```
    
    1. **Querying the Right Table**
    Use the `getPartitionTable` method to dynamically construct the SQL query.
    
    ```java
    @Service
    public class OrderService {
        @Autowired
        private JdbcTemplate jdbcTemplate;
    
         public void ensurePartitionTableExists(String baseTableName, int year, int month) {
            String tableName = String.format("%s_%d_%02d", baseTableName, year, month); // e.g., user_orders_2021_01
            String checkTableExistsQuery = "SHOW TABLES LIKE ?";
            
            // Check if the table exists
            Boolean tableExists = jdbcTemplate.query(checkTableExistsQuery, 
                                                      new Object[]{tableName}, 
                                                      rs -> rs.next());
    
            if (!Boolean.TRUE.equals(tableExists)) {
                // Create the table dynamically
                String createTableQuery = String.format(
                    "CREATE TABLE %s LIKE %s",
                    tableName, baseTableName
                );
    
                jdbcTemplate.execute(createTableQuery);
                System.out.println("Created partition table: " + tableName);
            }
        }
    }
    
    ```
    
    ---
    
    ### **Usage in Code**
    
    You can invoke the `ensurePartitionTableExists` method whenever you're about to insert data into a specific partition. This ensures the table is created if it doesn't already exist.
    
    ```java
    import org.springframework.stereotype.Service;
    import java.time.LocalDate;
    
    @Service
    public class UserOrderService {
        private final PartitionTableService partitionTableService;
        private final JdbcTemplate jdbcTemplate;
    
        public UserOrderService(PartitionTableService partitionTableService, JdbcTemplate jdbcTemplate) {
            this.partitionTableService = partitionTableService;
            this.jdbcTemplate = jdbcTemplate;
        }
    
        public void saveUserOrder(UserOrder order) {
            LocalDate orderDate = order.getOrderDate();
            int year = orderDate.getYear();
            int month = orderDate.getMonthValue();
    
            // Ensure the partition table exists
            partitionTableService.ensurePartitionTableExists("user_orders", year, month);
    
            // Insert data into the appropriate partition table
            String partitionTableName = String.format("user_orders_%d_%02d", year, month);
            String insertQuery = String.format(
                "INSERT INTO %s (order_id, user_id, product_id, order_date, amount) VALUES (?, ?, ?, ?, ?)",
                partitionTableName
            );
    
            jdbcTemplate.update(
                insertQuery,
                order.getOrderId(),
                order.getUserId(),
                order.getProductId(),
                order.getOrderDate(),
                order.getAmount()
            );
    
            System.out.println("Inserted data into partition: " + partitionTableName);
        }
    }
    
    ```
    
    ### 3. **Routing Searches to a Partition**
    
    If you're using a database that supports partitioning (e.g., MySQL, PostgreSQL), the database engine automatically determines the correct partition for queries.
    
    If you're manually partitioning with multiple tables:
    
    1. Use the logic above to determine the correct table.
    2. If searches span multiple partitions, query all relevant tables and merge results programmatically.
    
    ---
    
    ### 4. **Database Partitioning with Java Configuration**
    
    To enhance maintainability, you can configure table partitioning at the ORM level (e.g., with Hibernate).
    
    ### Example:
    
    ```java
    @Entity
    @Table(name = "user_orders", schema = "partitioned_schema")
    public class UserOrder {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long orderId;
    
        private Long userId;
        private LocalDate orderDate;
        private Long productId;
        private Double amount;
    }
    
    ```
    
    Configure dynamic table routing by creating multiple entity managers (one for each partition) or using native queries to specify table names dynamically.
    
    ---
    
    ### 5. **Partitioning Summary**
    
    - **Database-Side Partitioning**: Use MySQL’s built-in partitioning by range.
    - **Application-Side Partitioning**: Implement table-routing logic in Java.
    - **Redirecting Queries**: Use logic to determine the correct table or partition based on the search query.
    - **Spring Boot Integration**: Use `JdbcTemplate`, dynamic native queries, or a combination of dynamic table name resolution and Hibernate for ORM support.
    
    This setup balances performance and scalability for large datasets in e-commerce or similar scenarios.
    
- What is Executor Framework ?
    
    The Executor framework was introduced in Java 5 as part of the java.util.concurrent package to simply the development of concurrent applications by abstracting away many of the complexities involved in creating and managing threads.
    
    Problems while creating the manual thread
    
    1. Manual Thread Management
    2. Resource Management
    3. Scalability
    4. Thread Reuse
    5. Error Handling
    
    Three parts of Executor framework
    
    1. Executor
    2. ExecutorService
        - Methods of ExecutorService
            - submit(Runnable)
            - submit(Callable)
            - submit(Runnable,returnvalue)
            - shutdown()
            - shutdownnow()
            - awaitTermination()
            - isShutDown()
            - isTerminated()
            - invokeAll()
            - invokeAll(Callable,time,TimeUnit.SECONDS)
            
    3. ScheduleExecutorService
        - Methods
            1. schedule()
            2. scheduleWithFixedDelay()
            3. scheduleWithFixedRate()
    - What is CountDownLatch
        
        It is used to wait until all the thread run and then main thread will have it’s control instead of waiting one by one by using .get() we can use CountDownLatch for the same.
        
        ```java
        package ExecutorFramework;
        
        import java.util.concurrent.Callable;
        import java.util.concurrent.CountDownLatch;
        import java.util.concurrent.ExecutorService;
        import java.util.concurrent.Executors;
        
        public class CountDownLatchExample {
            public static void main(String[] args) throws InterruptedException {
                int numberOfService=3;
                ExecutorService executorService= Executors.newFixedThreadPool(3);
                CountDownLatch latch=new CountDownLatch(numberOfService);
                executorService.submit(new DependentService(latch));
                executorService.submit(new DependentService(latch));
                executorService.submit(new DependentService(latch));
                latch.await();
        
                System.out.println("Main");
                executorService.shutdown();
        
            }
        }
        class DependentService implements Callable<String>{
            private final CountDownLatch latch;
        
            DependentService(CountDownLatch countDownLatch){
                this.latch=countDownLatch;
            }
        
            @Override
            public String call() throws Exception {
                try{
                    System.out.println(Thread.currentThread().getName()+" service started");
                    Thread.sleep(1000);
                }finally {
                    latch.countDown();
                }
                return "ok";
            }
        }
        ```
        
    - What is Java CyclicBarrier
        
        Java CyclicBarrier is used when you want you each thread to reach at specific point then all the thread will complete.
        for example : you have 5 friends with whom you are going to watch movie in theater 3 friends reach at entrance early but you decide to enter inside only when all the friends arrive at entrance this is how cyclicbarrier works.
        
- What is the difference between Runnable and Callable Interface
    
    
    | **Aspect** | **Runnable** | **Callable** |
    | --- | --- | --- |
    | **Package** | `java.lang` | `java.util.concurrent` |
    | **Introduced in** | Java 1.0 | Java 5 (as part of the `java.util.concurrent` package) |
    | **Method to Implement** | `void run()` | `V call() throws Exception` |
    | **Return Value** | Does not return a value (`void`) | Can return a value of a generic type (`V`) |
    | **Exception Handling** | Cannot throw checked exceptions | Can throw checked exceptions |
    | **Usage** | Used for tasks that do not need to return a result | Used for tasks that need to return a result or throw exceptions |
    | **Thread Execution** | Can be executed using a `Thread` or an `ExecutorService` | Can only be executed using an `ExecutorService` |
    | **Future Object** | Does not return a `Future` object | Returns a `Future` object, which can be used to retrieve the result or status |
    | **Example Use Case** | Running a background task without needing a result | Performing a computation or task that returns a result or may throw an exception |
- Difference between wait() and sleep()
    
    
    | **Aspect** | **wait()** | **sleep()** |
    | --- | --- | --- |
    | **Package** | `java.lang.Object` | `java.lang.Thread` |
    | **Method Type** | Instance method | Static method |
    | **Usage** | Used for thread synchronization and inter-thread communication | Used to pause the execution of the current thread for a specified time |
    | **Calling Context** | Must be called from a synchronized context (inside a synchronized block or method) | Can be called from any context |
    | **Lock Release** | Releases the lock on the object it is called on | Does not release any locks |
    | **Wake-Up Condition** | Can be woken up by `notify()` or `notifyAll()` or after a timeout (if specified) | Automatically wakes up after the specified time has elapsed |
    | **Exception Handling** | Throws `InterruptedException` | Throws `InterruptedException` |
    | **Purpose** | Used for coordination between threads (e.g., waiting for a condition) | Used to introduce a delay in thread execution |
    | **Example Use Case** | Waiting for a resource to become available | Pausing a thread for a fixed duration |
- **Diff and flat map and map**
    
    Both works in steams.
    map is used to modify list of any object → primitive or not.
    
    ```java
        //flatmap and map
        public  static void flatMapAndMap(){
            List<Integer> mapList = List.of(1,2,3,4,5,6,6);
            List<List<Integer>> flatMapList1 = List.of(List.of(22,33,44,55,66,77,88));
            List<List<Integer>> flatMapList2 = List.of(List.of(12,23,34,45,56,67,78));
    
            List<Integer> mapResultList =  mapList.stream().map(r-> r*3).toList();
            /**
             * Flat map is used to straighten out the list of list (collection of collection)
             * It applies logic to all stream and convert them into single stream.
             * so that further operation can be done.
             */
            List<Integer> flatMapResultList = flatMapList1.stream().flatMap(wr-> wr.stream().map(r->r*3)).toList();
    
            System.out.println("flatMapAndMap Map : " + mapResultList );
            System.out.println("flatMapAndMap FlatMap : " + flatMapResultList );
    
        }
    ```
    
- **Difference Between Hash Set and ArrayList**
    
    **Array List:**
    
    1. Works on Dynamic Array Implementation
    2. Can store duplicate values
    3. Indexed Collection
    4. Faster for viewing and storing data
    5. can be accessed by index
    6. Order Preserved
    7. Sorting Not Done
    8. Minimum Size = 10
    9. For adding and removing data it has slow manipulation due to index shifting.
    10. Can store any number of Null values
    11. Can Iterate using Forward and Backwards Iterator (List Iterator)
    
    **HashSet**
    
    1. Base Implementation of HashMap
    2. All the Values are Unique
    3. Non-Indexed
    4. Insertion Order is not Preserved
    5. Sorting is not done
    6. Minimum size = 16
    7. Can store 1 null values
    8. Only Forward Iterator
- **Internal Working of HashSet**
    
    Internal Working is based on HashMap → The value part is set to an final object = PRESENT
    
- **Volatile keyword**
    
    Volatile Keyword is a non-access modifier used in for thread.
    
    You can you Volatile keyword with only fields not with method or class.
    
    In Volatile Keyword → value fetched **by thread are not from local cache but directly from the main memory**
    
    ### **Scenario: Thread1 Updates a `volatile` Variable**
    
    If `thread1` updates a `volatile` variable from `true` to `false`, the behavior depends on how `volatile` works in terms of **visibility** and **memory consistency**. Let's break it down step by step.
    
    ---
    
    ### **How `volatile` Works in Memory:**
    
    1. **Main Memory and Thread Caches:**
        - In a multithreaded environment, each thread may use its own local cache for performance.
        - A `volatile` variable ensures **direct interaction with main memory**. This means:
            - When a thread **writes** to a `volatile` variable, the value is immediately updated in main memory.
            - When a thread **reads** a `volatile` variable, it fetches the value **directly from main memory**, not its local cache.
    
    ---
    
    ### **Step-by-Step Analysis:**
    
    1. **Thread1 Updates the Value:**
        - Initially, the `volatile` variable (let's call it `flag`) is `true`.
        - `thread1` updates the value of `flag` to `false`.
        - As soon as `thread1` writes `false` to `flag`, this updated value is **immediately written to main memory**.
    2. **Thread2 Reads the Value:**
        - `thread2` tries to access the value of `flag`.
        - Since `flag` is `volatile`, `thread2` **always reads the value directly from main memory**.
        - At this point, `thread2` will see the updated value, which is `false`.
    
    ---
    
    ### **What If `thread1` Fails Midway?**
    
    - If `thread1` begins the process of updating the variable but **fails (e.g., due to an exception)** before the new value (`false`) is written to main memory, then:
        - The **old value (`true`) remains in main memory**.
        - When `thread2` accesses `flag`, it will still fetch `true` from main memory because `thread1` didn't complete the write operation.
    
    ---
    
    ### **Key Points:**
    
    1. **Volatile Guarantees Visibility, Not Atomicity:**
        - `volatile` ensures that the latest written value is visible to all threads.
        - It does **not guarantee atomicity**, so if the update involves multiple steps (like reading-modifying-writing), another thread might see an intermediate or old state.
    2. **Thread2 Always Reads From Main Memory:**
        - Thanks to `volatile`, `thread2` will never fetch the stale value from a thread-local cache. It always fetches the value from main memory.
    
    ---
    
    ### **Example Code:**
    
    ```java
    class VolatileTest {
        private volatile boolean flag = true;
    
        public void thread1() {
            new Thread(() -> {
                System.out.println("Thread1: Updating flag to false...");
                flag = false; // Writing to the volatile variable
                System.out.println("Thread1: Updated flag to false.");
            }).start();
        }
    
        public void thread2() {
            new Thread(() -> {
                while (flag) {
                    // Loop until flag becomes false
                }
                System.out.println("Thread2: Detected flag change to false!");
            }).start();
        }
    
        public static void main(String[] args) {
            VolatileTest test = new VolatileTest();
            test.thread2(); // Start thread2
            try {
                Thread.sleep(100); // Delay to simulate thread2 running
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            test.thread1(); // Start thread1 to update the flag
        }
    }
    
    ```
    
    ---
    
    ### **Output:**
    
    ```
    Thread1: Updating flag to false...
    Thread1: Updated flag to false.
    Thread2: Detected flag change to false!
    
    ```
    
    ---
    
    ### **Explanation of Output:**
    
    1. `thread2` runs a loop checking the value of `flag`.
    2. Initially, `flag` is `true`, so `thread2` keeps looping.
    3. When `thread1` updates `flag` to `false`, the change is written to main memory because `flag` is `volatile`.
    4. `thread2` detects the updated value (`false`) and breaks out of the loop.
    
    ---
    
    ### **Key Takeaway:**
    
    - **If `thread1` completes its update, `thread2` will see the updated value (`false`).**
    - If `thread1` fails midway (before writing to main memory), `thread2` will still see the old value (`true`) because the update was never completed.
- **Diff between interface and abstract class after java 8**
    
    In Java 8, interfaces were enhanced with **default methods** and **static methods**, allowing them to include concrete implementations. This raises the question of why abstract classes are still necessary when interfaces seem to cover much of the same ground. Here’s the exact reasoning:
    
    **Key Differences That Justify Abstract Classes:**
    
    1.	**State/Fields**:
    
    •	**Abstract Classes**: Can have instance fields (variables) to maintain state.
    
    •	**Interfaces**: Cannot have instance fields; they can only have constants (static final fields).
    
    •	**Why needed**: If you need to manage state, you must use an abstract class.
    
    ### Explaining state of abstract class - Important
    
    To understand the difference between **abstract classes** and **interfaces** concerning "managing state," let’s break it down step by step:
    
    ---
    
    ### **What is "State" in Programming?**
    
    - **State** refers to the data or attributes that an object holds at a particular point in time.
    - For example, if you have a class `Car` with attributes `speed` and `fuelLevel`, the values of these attributes (e.g., `speed = 60`, `fuelLevel = 50`) define the state of the object at that time.
    
    ### **What Does "Manage State" Mean?**
    
    - **Managing state** means maintaining and updating the values of instance variables (attributes) that belong to an object.
    - It allows an object to "remember" information about itself or respond differently based on its current state.
    
    ---
    
    ### **Abstract Classes and Managing State**
    
    - **Abstract classes** can have **instance fields** (non-static variables), which are specific to each instance of a class. These fields store state information.
    - You can use these fields to define default behaviors or maintain object-specific data across method calls.
    
    ### **Example: Abstract Class Managing State**
    
    ```java
    abstract class Vehicle {
        protected int fuelLevel; // Instance field to manage state
    
        public Vehicle(int initialFuel) {
            this.fuelLevel = initialFuel;
        }
    
        public void refuel(int fuel) {
            this.fuelLevel += fuel; // Updating the state
        }
    
        public int getFuelLevel() {
            return fuelLevel; // Accessing the state
        }
    
        // Abstract method
        public abstract void drive();
    }
    
    class Car extends Vehicle {
        public Car(int initialFuel) {
            super(initialFuel); // Initialize state
        }
    
        @Override
        public void drive() {
            if (fuelLevel > 0) {
                fuelLevel--; // Modify state
                System.out.println("Driving. Remaining fuel: " + fuelLevel);
            } else {
                System.out.println("Out of fuel!");
            }
        }
    }
    
    ```
    
    **Usage**:
    
    ```java
    Car car = new Car(10);
    car.drive(); // Updates fuelLevel and prints "Driving. Remaining fuel: 9"
    car.refuel(5); // Updates fuelLevel to 14
    System.out.println(car.getFuelLevel()); // Prints "14"
    
    ```
    
    Here, `fuelLevel` is part of the **state**, and the abstract class `Vehicle` helps manage it.
    
    ---
    
    ### **Interfaces and Managing State**
    
    - **Interfaces** do not have instance fields. They are more focused on defining **behavior** rather than maintaining **state**.
    - Interfaces can only have constants (fields with `static final` modifiers) that are shared across all implementations. They cannot maintain object-specific state.
    
    ### **Example: Interface Without State**
    
    ```java
    interface Drivable {
        void drive();
    }
    
    class Bicycle implements Drivable {
        @Override
        public void drive() {
            System.out.println("Pedaling the bicycle!");
        }
    }
    
    ```
    
    Here, `Bicycle` does not maintain any state specific to an instance (like `fuelLevel` in the earlier example). Instead, it defines the behavior `drive()`.
    
    ---
    
    ### **Why Use Abstract Classes for State Management?**
    
    If your design requires maintaining and updating object-specific data (like fuel levels, speed, etc.), **abstract classes** are the way to go because they:
    
    1. Allow instance fields to store data for each object.
    2. Can provide concrete methods that modify or access these fields.
    
    ### **When to Use Interfaces?**
    
    Use interfaces when:
    
    1. You only need to define behaviors that a class must implement (e.g., `Drivable`, `Runnable`).
    2. You don’t need to manage any state information.
    
    ---
    
    ### **Conclusion**
    
    - **Abstract Classes**: Can hold **state** (through instance fields) and help manage it using methods. Use when your design requires state management.
    - **Interfaces**: Cannot hold instance-specific state. Use them when you only need to define behaviors.
    
    Let me know if you'd like further clarification!
    
    2.	**Constructors**:
    
    •	**Abstract Classes**: Can have constructors to initialize instance variables or perform other setup.
    
    •	**Interfaces**: Cannot have constructors.
    
    •	**Why needed**: Abstract classes allow setup logic for derived classes, which interfaces cannot do.
    
    3.	**Multiple Inheritance Conflict Resolution**:
    
    •	**Abstract Classes**: Allow extending only one class, resolving any ambiguity inherently.
    
    •	**Interfaces**: Allow implementing multiple interfaces, which can lead to method conflicts when multiple default methods exist.
    
    •	**Why needed**: Abstract classes provide a simpler inheritance structure when conflicts or ambiguity in multiple inheritance are an issue.
    
    4.	**Access Modifiers**:
    
    •	**Abstract Classes**: Can define methods and fields with all access levels (public, protected, private, package-private).
    
    •	**Interfaces**: Methods are implicitly public (or private for helper methods in Java 9+); fields are public static final.
    
    •	**Why needed**: Abstract classes provide finer control over access.
    
    5.	**Rich Hierarchy**:
    
    •	**Abstract Classes**: Can act as a base for concrete classes and provide a partial implementation with shared state and behavior.
    
    •	**Interfaces**: Focus on defining a contract or behavior that classes can implement.
    
    •	**Why needed**: Abstract classes allow a richer and more complex shared implementation, which is beyond the scope of interfaces.
    
    **When to Use:**
    
    •	**Use Abstract Classes**:
    
    •	When you need to share **state** and **behavior** among related classes.
    
    •	When you want to provide a common base class for objects with more concrete implementations.
    
    •	When you need **constructors** for setup or initialization.
    
    •	**Use Interfaces**:
    
    •	When you need to define a **contract** that multiple unrelated classes can implement.
    
    •	When you want to leverage **default methods** for optional behavior.
    
    •	When you need **multiple inheritance** for behavior.
    
    **Conclusion:**
    
    Java 8 interface enhancements make interfaces more powerful, but **abstract classes** are still needed for scenarios involving shared state, initialization logic, or richer access control. They complement rather than replace each other.
    
- **How to Override .equals and .hashcode method of object class in any other class**
    
    To properly override the `equals` and `hashCode` methods in Java for the `Rectangle` class, we need to ensure the following:
    
    ### **Steps to Override `equals` and `hashCode`:**
    
    1. **`equals` Method:**
        - Check if the current object (`this`) is compared to itself. If yes, return `true`.
        - Check if the other object (`obj`) is `null` or if it's not of the same class. If yes, return `false`.
        - Cast the `obj` to `Rectangle` and compare the relevant fields (`length` and `breadth`).
    2. **`hashCode` Method:**
        - Compute a unique hash code for the `Rectangle` object based on its fields (`length` and `breadth`) using the formula:
            
            ```java
            int hash = 31 * field1 + field2;
            ```
            
    
    ---
    
    ### **Implementation of `equals` and `hashCode`:**
    
    Here is the properly implemented `Rectangle` class:
    
    ```java
    public class Rectangle {
        int length;
        int breadth;
    
        // Constructor
        public Rectangle(int length, int breadth) {
            this.length = length;
            this.breadth = breadth;
        }
    
        @Override
        public boolean equals(Object obj) {
            // Check if both references point to the same object
            if (this == obj) {
                return true;
            }
    
            // Check if the obj is null or not of the same class
            if (obj == null || getClass() != obj.getClass()) {
                return false;
            }
    
            // Cast obj to Rectangle and compare fields
            Rectangle rectangle = (Rectangle) obj;
            return this.length == rectangle.length && this.breadth == rectangle.breadth;
        }
    
        @Override
        public int hashCode() {
            // Compute hash code using length and breadth
            return 31 * length + breadth;
        }
    }
    
    ```
    
    ---
    
    ### **How It Works:**
    
    1. **`equals` Method:**
        - First, the `this == obj` check ensures that if the same object reference is being compared, it immediately returns `true`.
        - `obj == null || getClass() != obj.getClass()` ensures `null` or objects of different classes don't pass the comparison.
        - Finally, field-by-field comparison is done to check if two `Rectangle` objects are considered "equal."
    2. **`hashCode` Method:**
        - It uses the fields (`length` and `breadth`) to calculate a unique hash code for each `Rectangle` object.
        - Using `31` as a multiplier is a common practice in Java for a good hash distribution.
    
    ---
    
    ### **Example Usage:**
    
    ```java
    public class Main {
        public static void main(String[] args) {
            Rectangle rect1 = new Rectangle(10, 20);
            Rectangle rect2 = new Rectangle(10, 20);
            Rectangle rect3 = new Rectangle(15, 25);
    
            System.out.println("rect1 equals rect2: " + rect1.equals(rect2)); // true
            System.out.println("rect1 equals rect3: " + rect1.equals(rect3)); // false
    
            System.out.println("rect1 hashCode: " + rect1.hashCode());
            System.out.println("rect2 hashCode: " + rect2.hashCode());
            System.out.println("rect3 hashCode: " + rect3.hashCode());
        }
    }
    
    ```
    
    ---
    
    ### **Output:**
    
    ```
    rect1 equals rect2: true
    rect1 equals rect3: false
    rect1 hashCode: 650
    rect2 hashCode: 650
    rect3 hashCode: 800
    
    ```
    
    ---
    
    ### **Key Points to Remember:**
    
    - Always override `hashCode` when overriding `equals` to ensure consistency.
    - Use tools like `Objects.hash()` (from Java 7+) for better readability in large classes.
    - `hashCode` ensures that objects which are "equal" according to `equals()` produce the same hash code, aiding in collection lookups like `HashMap` and `HashSet`.
- **Find out 2nd lastest number in int Array (Not array list)**
    
    ```java
        public static void findSecondLargestNumberInIntegerArray(){
            int[] intArray = {1,2,3,4,5,6,7,33,55};
            // Time-Complexity of sorting -> O(nLog(n))
            Optional<Integer> secondHighestNumber = Arrays.stream(intArray).boxed().sorted(Comparator.reverseOrder()).skip(1).findFirst();
            secondHighestNumber.ifPresent(System.out::println);
    
            // fetching 2nd Largest if not distinct
            // boxed converts primitive stream to wrapper class stream.
            int[] intArray2 = {1,2,3,4,5,6,7,33,55,55,53,66,54};
            Optional<Integer> secondHighestNumberDistinct = Arrays.stream(intArray).boxed().distinct().sorted(Comparator.reverseOrder()).skip(1).findFirst();
            secondHighestNumberDistinct.ifPresent(System.out::println);
        }
    ```
    
- **Find Max and Min from an integer ArrayList**
    
    ```java
        public static void minAndMaxUsingStream() {
            List<Integer> list = new ArrayList<>();
            list.add(1);
            list.add(2);
            list.add(3);
            list.add(4);
    
            Optional<Integer> Max = list.stream().max((a,b) -> a.compareTo(b)); // here you cannot use Integer.compareTo because its not a static method.
            //can also be done in this manner:
            Optional<Integer> Max = list.stream().max(Integer::compareTo);
            Optional<Integer> Min = list.stream().min(Integer::compareTo);
            
            //also you can you Integer Static method i.e. compare (method of comparator)
            Optional<Integer> Max = list.stream().max((a,b) -> Integer.compare(a,b)); // 4
            Optional<Integer> Min = list.stream().min((a,b) -> Integer.compare(a,b)); // 1
            
            // But if we do it -* check explanation below
            
            Optional<Integer> Min = list.stream().**min**((a,b) -> **Integer.compare(b,a)**); // 4
    
            System.out.println("minAndMaxUsingStream : Max : " + Max.get());
            System.out.println("minAndMaxUsingStream : Min : " + Min.get());
        }
    ```
    
    ### **What Happens When You Reverse the Logic?**
    
    1. Normally, `Integer.compare(a, b)` sorts in **ascending order**:
        - 1 → 2 → 3 → 4
        - The minimum value is `1`.
    2. When you reverse it to **`Integer.compare(b, a)`**, the comparison works as descending order:
        - 4 → 3 → 2 → 1
        - The `min()` method **thinks** the "minimum" element is **the largest element**, because descending logic is used.
    
- **Find Max and Min from an integer ArrayList using custom Comparator**
    
    ```java
       public class CustomComparator implements Comparator<Integer> {
    
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1, o2);
            }
        }
        
        
        public static void minAndMaxUsingCustomComparator() {
            List<Integer> list = new ArrayList<>();
            list.add(1);
            list.add(2);
            list.add(3);
            list.add(4);
    
            // Creating object of outer class here because inner class is non-static class, and they are tied to the object not class itself.
            // hence creating object of inner class using outer class
            // Or you can make the inner class static so that this function can access it without making the object, or make this function non-static as well.
            streamQuestions streamQuestions = new streamQuestions();
            CustomComparator customComparator = streamQuestions.new CustomComparator();
            
            Optional<Integer> max2 = list.stream().max(new streamQuestions().new CustomComparator());
            
            Optional<Integer> max = list.stream().max(customComparator);
            // you can do this as well -
            Optional<Integer> max = list.stream().max((a,b)-> customComparator.compare(a,b));
    
            System.out.println("minAndMaxUsingCustomComparator Max : " + max.get());
        }
    ```
    
- **Collect Duplicate Element in a list using stream**
    
    ```java
     public static void collectDuplicateElementUsingStream() {
            List<Integer> list = new ArrayList<>(List.of(1, 1, 2, 2, 3, 4, 5));
            //Collected Duplicate Elements to a list.
            // list1.stream().filter(i->Collections.frequency(list1,i)>1).distinct().forEach(System.out::println);  ---> if you want to directly console the keys
            List<Integer> duplicateElements = list.stream().filter(item -> Collections.frequency(list, item) > 1).distinct().toList();
    //        Long count =  list.stream().collect(Collectors.counting());
    //        System.out.println("Count : " + count); // 7 Output , Count of the elements
    
            //collect Duplicate element in map with its frequency
            HashMap<Integer, Long> frequencyMap = list.stream().collect(Collectors.groupingBy(i -> i, HashMap::new, Collectors.counting()));
            System.out.println("collectDuplicateElementUsingStream : " + duplicateElements);
        }
    ```
    
- **Find the employee with 2nd Largest age from list of employee**
    
    ```java
        public static void secondHighestSalaryUsingStream() {
            List<Employee> employeeList = List.of(new Employee("Jay", 24),
                    new Employee("Hela", 78),
                    new Employee("Sunio", 65),
                    new Employee("greta", 23),
                    new Employee("Phaluki", 76)
            );
    
            Optional<Employee> employee = employeeList.stream().sorted(Comparator.comparingInt(Employee::getAge).reversed()).skip(1).findFirst();
            //or
            Optional<Employee> employee2 = employeeList.stream().sorted((a,b)-> Integer.compare(b.getAge(),a.getAge())).skip(1).findFirst();
            if (employee.isPresent()) {
                System.out.println(employee.get());
            } else {
                System.out.println("not present");
            }
        }
    ```
    
- **Sort Fruits in the basis of their length**
    
    ```java
        public static void sortFruitOnBasisOfLengthOfFruit() {
            List<String> fruits = new ArrayList<>();
            fruits.add("apple");
            fruits.add("banana");
            fruits.add("pear");
    
            // If we want to reversed sorting (desc order)
            fruits.stream().sorted((o1, o2) -> Integer.compare( o2.length(),o1.length())).forEach(System.out::println);
            //Reversing using sorted
            fruits.stream().sorted(Comparator.comparingInt(String::length).reversed()).forEach(System.out::println);
         
            //or in asc order
            fruits.stream().sorted(Comparator.comparingInt(String::length)).forEach(System.out::println);
        }
    ```
    
- **Check whether strings are anagram or not?**
    
    ```java
        //1
        public static boolean isAnagram(String a, String b){
    
            if(a.length() != b.length()) return false;
            return  a.chars().sorted().mapToObj(c->(char)c ).toList().equals(b.chars().sorted().mapToObj(c->(char)c).collect(Collectors.toList()));
        }
        
         boolean isAnagram = stingFilter("listen","silent"); //true
          boolean isAnagram = stingFilter("listed","silent"); //false
          
        
        //2  
        public static boolean isAnagramNotUsingStream(String a, String b){
            a = a.toLowerCase();
            b = b.toLowerCase();
            char[] stringAList = a.toCharArray() ;
            char[] stringBList =b.toCharArray();
    
            Arrays.sort(stringAList);
            Arrays.sort(stringBList);
            return Arrays.equals(stringAList,stringBList);
    
        }   
    ```
    
- **Reverse a String using stream**
    
    ```java
        public static void stringReverser(String input){
        
        
            //reverse a string
             String collect = Stream.of(str).map(word -> 
             new StringBuilder(word).reverse()).collect(Collectors.joining(" "));
    
          // ABCD -> 93 94 92 93 (unicodes using chars()) -> {'A','B','C','D'} -> ['A','B','C','D'] -> ['D','C','B','A'] -> ["D","C","B","A"]-> "DCBA"
            String newString = input.chars().mapToObj(c->(char) c).collect(Collectors.toList()).reversed().stream().map(c -> String.valueOf(c)).collect(Collectors.joining());
            System.out.println(newString); // DCBA
            
            // If I remove .stream().map(c -> String.valueOf(c)).collect(Collectors.joining()); and replace it with toString() -> it will return [D,C,B,A].
            
           //2 
            StringBuilder newString = new StringBuilder(input);
            System.out.println(newString.reverse());
            
            
            //3
            StringBuilder stb = new StringBuilder();
            for(int i=input.length()-1;i>=0;i--){
                stb.append( input.charAt(i));
    //            reversedString = reversedString + input.charAt(i);
            }
            return stb;
            
            //4
            public static void reverseStringUsingReduce(){
            String str = "hello";
            String reversed = str.chars().mapToObj(c -> (char) c).reduce("",(ans,a)->{
                ans = a + ans;
                        return ans;
            },(ans1,ans2)-> ans1+ans2);
    
            System.out.println("ReversedStringUsingReduce : " + reversed);
        }
    
        }
    ```
    
- **isPalindrome**
    
    ```java
        public static boolean ispalindrone(int number){
            int sum = 0;
            int remainder = 0;
            int temp = number;
            while(number != 0){
                remainder = number % 10;
                sum = sum *10+ remainder;
                number= number/10;
            }
            return temp == sum;
    
        }
    ```
    
- **Fibonacci Series**
    
    ```java
        public static void fibonacci(int length){
            int a =0;
            int b =1;
            int c =0;
            System.out.print(a + "  " + b);
            while(length != 0){
                c= a+b;
                a=b;
                b=c;
                System.out.print(" " + c + " ");
                length --;
            }
        }
    ```
    
- **Comparator Vs Comparable?**
    
    Here’s a clear and easy-to-follow comparison between **`Comparable`** and **`Comparator`** in Java:
    
    ---
    
    ### **Definition**
    
    | Feature | **Comparable** | **Comparator** |
    | --- | --- | --- |
    | **Purpose** | Defines a natural ordering for objects of a class. | Allows custom ordering for objects of a class. |
    | **Interface** | `java.lang.Comparable` | `java.util.Comparator` |
    | **Single or Multiple Orderings** | Can define **only one** ordering for a class. | Can define **multiple orderings** for a class. |
    
    ---
    
    ### **Usage**
    
    | Feature | **Comparable** | **Comparator** |
    | --- | --- | --- |
    | **Where to Implement** | Implemented in the class being sorted. | Implemented in a separate class or as a lambda function. |
    | **How to Implement** | Override the `compareTo` method. | Override the `compare` method or use a lambda expression. |
    | **Method Signature** | `int compareTo(T o)` | `int compare(T o1, T o2)` |
    
    ---
    
    ### **Examples**
    
    ### **Using Comparable**
    
    - Implemented **inside the class** to define the natural ordering.
    
    ```java
    class Student implements Comparable<Student> {
        int id;
        String name;
    
        public Student(int id, String name) {
            this.id = id;
            this.name = name;
        }
    
        @Override
        public int compareTo(Student other) {
            return this.id - other.id; // Natural ordering by id
        }
    }
    
    // Usage:
    List<Student> students = new ArrayList<>();
    students.add(new Student(2, "Alice"));
    students.add(new Student(1, "Bob"));
    
    Collections.sort(students); // Sorts using compareTo
    
    students.forEach(student -> System.out.println(student.id)); // Output: 1, 2
    
    ```
    
    ### **Using Comparator**
    
    - Used **outside the class** to define custom ordering.
    
    ```java
    import java.util.*;
    
    class Student {
        int id;
        String name;
    
        public Student(int id, String name) {
            this.id = id;
            this.name = name;
        }
    }
    
    // Custom Comparator for sorting by Name
    class NameComparator implements Comparator<Student> {
        @Override
        public int compare(Student s1, Student s2) {
            return s1.name.compareTo(s2.name);
        }
    }
    
    // Custom Comparator for sorting by ID
    class IdComparator implements Comparator<Student> {
        @Override
        public int compare(Student s1, Student s2) {
            return Integer.compare(s1.id, s2.id);
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            List<Student> students = new ArrayList<>();
            students.add(new Student(2, "Alice"));
            students.add(new Student(1, "Bob"));
    
            // Sort by Name
            Collections.sort(students, new NameComparator());
            System.out.println("Sorted by Name:");
            students.forEach(student -> System.out.println(student.name)); // Output: Alice, Bob
    
            // Sort by ID
            Collections.sort(students, new IdComparator());
            System.out.println("Sorted by ID:");
            students.forEach(student -> System.out.println(student.id)); // Output: 1, 2
        }
    }
    
    ```
    
    ### **Dynamic Selection of Comparator**
    
    If you want to dynamically choose the comparator at runtime (e.g., based on user input), you can use a switch-case or a conditional block.
    
    ### Example:
    
    ```java
    java
    Copy code
    import java.util.*;
    
    public class Main {
        public static void main(String[] args) {
            List<Student> students = new ArrayList<>();
            students.add(new Student(2, "Alice"));
            students.add(new Student(1, "Bob"));
    
            Scanner scanner = new Scanner(System.in);
            System.out.println("Enter sorting criteria (name/id):");
            String criteria = scanner.nextLine();
    
            Comparator<Student> comparator;
            if ("name".equalsIgnoreCase(criteria)) {
                comparator = new NameComparator();
            } else if ("id".equalsIgnoreCase(criteria)) {
                comparator = new IdComparator();
            } else {
                System.out.println("Invalid criteria. Sorting by default (ID).");
                comparator = new IdComparator();
            }
    
            // Sort using the selected comparator
            Collections.sort(students, comparator);
    
            // Display the sorted list
            students.forEach(student -> System.out.println(student.name + " - " + student.id));
        }
    }
    
    ```
    
    ---
    
    ### **Using Lambda Expressions**
    
    If you prefer shorter code, you can use lambda expressions instead of creating separate comparator classes:
    
    This eliminates the need to create a separate `Comparator` class.
    
    ### Example:
    
    ```java
    java
    Copy code
    Collections.sort(students, (s1, s2) -> s1.name.compareTo(s2.name)); // Sort by Name
    Collections.sort(students, (s1, s2) -> Integer.compare(s1.id, s2.id)); // Sort by ID
    
    ```
    
    ---
    
    ### **Key Differences**
    
    | Aspect | **Comparable** | **Comparator** |
    | --- | --- | --- |
    | **Implementation Location** | Inside the class being sorted. | Outside the class being sorted. |
    | **Number of Orderings** | One natural ordering for the class. | Can define multiple custom orderings. |
    | **Flexibility** | Less flexible (built-in ordering). | More flexible (customizable for different criteria). |
    | **Usage in Sorting** | `Collections.sort(list)` (uses `compareTo`). | `Collections.sort(list, comparator)` (uses `compare`). |
    
    ---
    
    ### **When to Use**
    
    | Scenario | Use **Comparable** | Use **Comparator** |
    | --- | --- | --- |
    | If the class has **one natural order** that makes sense. | Yes | No |
    | If the sorting order may change depending on the context or criteria. | No | Yes |
    | If sorting logic should be **inside the class**. | Yes | No |
    | If sorting logic should be **outside the class** or easily reusable. | No | Yes |
    
    ---
    
    ### **How Does `Collections.sort()` Know Which Comparator to Use?**
    
    - **Explicit Comparator**: You must explicitly provide the comparator to `Collections.sort()`.
    - If no comparator is provided, `Collections.sort()` will attempt to use the `compareTo` method of the objects (if they implement the `Comparable` interface).
    - The method dynamically sorts based on the logic defined in the provided comparator, ensuring flexibility.
- **What is Reflection in java?ff**
    
    Basic Use case of Reflection : 
    
    → Access Private members of the class - constructor, variable method.
    
    → Dynamic behaviour - Load and use classes or method that are not known during compilation.
    
    ### **What is Reflection in Java?**
    
    **Reflection** in Java is a powerful feature that allows a program to inspect, analyze, and even modify its structure and behavior at runtime. This includes accessing fields, methods, constructors, annotations, and more, even if they're private.
    
    It is part of the **java.lang.reflect** package and can be used to dynamically interact with a class and its components.
    
    ---
    
    ### **Why Use Reflection?**
    
    Reflection is mainly used for:
    
    1. **Dynamic Class Loading**: Load a class at runtime without knowing its name at compile time.
    2. **Runtime Analysis**: Inspect methods, fields, or constructors of a class.
    3. **Frameworks and Libraries**: Used extensively in frameworks like Spring, Hibernate, and testing libraries like JUnit.
    4. **Debugging/Testing Tools**: Create tools for debugging, testing, or monitoring.
    5. **Access Private Members**: Access private methods and fields (although not recommended for regular use).
    
    ---
    
    ### **Components of Reflection**
    
    1. **Class Object**:
        - Every class in Java has a corresponding `Class` object that stores metadata about the class.
        - You can obtain a `Class` object using:
            
            ```java
            Class<?> clazz = Class.forName("fully.qualified.ClassName");
            
            ```
            
    2. **Methods**:
        - Analyze or invoke methods of a class.
    3. **Fields**:
        - Access or modify fields, including private ones.
    4. **Constructors**:
        - Create new instances of a class using constructors dynamically.
    
    ---
    
    ### **How to Use Reflection?**
    
    ### **1. Get Class Metadata**
    
    You can use the `Class` object to retrieve information about a class.
    
    ```java
    Class<?> clazz = Class.forName("java.util.ArrayList");
    System.out.println("Class Name: " + clazz.getName());
    System.out.println("Is Interface: " + clazz.isInterface());
    System.out.println("Is Array: " + clazz.isArray());
    
    ```
    
    ### **2. Get Methods**
    
    Retrieve and invoke methods of a class.
    
    ```java
    Class<?> clazz = Class.forName("java.util.ArrayList");
    Method[] methods = clazz.getDeclaredMethods();
    
    for (Method method : methods) {
        System.out.println("Method Name: " + method.getName());
    }
    
    ```
    
    ### **3. Get Fields**
    
    Access fields, even private ones.
    
    ```java
    Field field = clazz.getDeclaredField("size");
    field.setAccessible(true); // Allow access to private fields
    int size = (int) field.get(new ArrayList<>());
    System.out.println("Size Field Value: " + size);
    
    ```
    
    ### **4. Get Constructors**
    
    Invoke a constructor to create an object.
    
    ```java
    Constructor<?> constructor = clazz.getConstructor();
    Object instance = constructor.newInstance();
    System.out.println("Instance: " + instance);
    
    ```
    
    ### **5. Invoke Methods**
    
    Invoke methods dynamically.
    
    ```java
    Method method = clazz.getMethod("add", Object.class);
    method.invoke(instance, "Hello, Reflection!");
    System.out.println("Instance after method invocation: " + instance);
    
    ```
    
    ---
    
    ### **Example: Simple Reflection**
    
    ```java
    import java.lang.reflect.*;
    
    class Person {
        private String name;
    
        public Person(String name) {
            this.name = name;
        }
    
        private void sayHello() {
            System.out.println("Hello, my name is " + name);
        }
    }
    
    public class ReflectionExample {
        public static void main(String[] args) throws Exception {
            // Load Class
            Class<?> clazz = Person.class;
    
            // Create an Instance
            Constructor<?> constructor = clazz.getDeclaredConstructor(String.class);
            Object person = constructor.newInstance("John");
    
            // Access Private Method
            Method method = clazz.getDeclaredMethod("sayHello");
            method.setAccessible(true); // Make private method accessible
            method.invoke(person);
        }
    }
    
    ```
    
    **Output:**
    
    ```
    Hello, my name is John
    
    ```
    
    ---
    
    ### **Limitations of Reflection**
    
    1. **Performance Overhead**: Reflection is slower as it works at runtime.
    2. **Security Risks**: Can break encapsulation by accessing private fields and methods.
    3. **Complexity**: Makes code harder to read, maintain, and debug.
    
    ---
    
    ### **Use Cases**
    
    1. **Dependency Injection**:
        - Frameworks like Spring use reflection to inject dependencies dynamically.
    2. **Testing**:
        - Testing tools like JUnit use reflection to call methods.
    3. **Serialization/Deserialization**:
        - Libraries like Jackson or Gson use reflection to map objects to JSON/XML.
    
    ---
    
    Let me know if you'd like to try implementing any of these examples or have questions!
    
- **Break Singleton class using Reflection**
    
    ```java
           /*
            How to break singleton pattern using clone()
             */
            SingletonClass instance1 = SingletonClass.getSingletonInstance();
            SingletonClass instance2 = (SingletonClass) instance1.clone();
            if (instance2 == instance1)
                System.out.println(true);
    
            /*
            Breaking singleton pattern using reflection
             */
            Class<SingletonClass> clazz = SingletonClass.class;
            Constructor<SingletonClass> constructor = clazz.getDeclaredConstructor();
            constructor.setAccessible(true);
            SingletonClass  instance = constructor.newInstance();
    
    //        Constructor<SingletonClass> constructor = SingletonClass.class.getDeclaredConstructor();
    //        constructor.setAccessible(true);
    //        SingletonClass instance3 = constructor.newInstance();
            if (instance == instance1)
                System.out.println("Same");
        }
    ```
    
- **Intersection sort, Bubble Sort, Selection sort**
- **Checked and Unchecked Exception**
    
    **Throwable**
    
    (Base class for all exceptions and errors)
    
    - **Exception** (For exceptional conditions a program should handle)
        - **Checked Exceptions**
            - Direct subclasses of `Exception` (except `RuntimeException`)
        - **Unchecked Exceptions**
            - Subclasses of `RuntimeException`
    - **Error** (Irrecoverable issues not meant to be caught, e.g., `OutOfMemoryError`)
    
    ### **Difference in Class Implementation**
    
    | **Aspect** | **Checked Exceptions** | **Unchecked Exceptions** |
    | --- | --- | --- |
    | **Implemented Class** | Direct subclasses of `Exception`. | Subclasses of `RuntimeException`. |
    | **Hierarchy** | `Throwable → Exception → CheckedException`. | `Throwable → Exception → RuntimeException`. |
    
    ### **Checked and Unchecked Exceptions in Java**
    
    Exceptions are issues that occur during program execution. Java categorizes exceptions into **checked** and **unchecked** exceptions based on how they are handled during compilation and runtime.
    
    ---
    
    ### **Checked Exceptions**
    
    1. **Definition**: Checked exceptions are **checked at compile-time** by the Java compiler.
        - You must either **handle** these exceptions using `try-catch` or **declare** them using the `throws` keyword.
    2. **Examples**:
        - `IOException`
        - `SQLException`
        - `ClassNotFoundException`
        - `FileNotFoundException`
        - `InterruptedException`
    3. **Use Case**: Used for exceptions you can foresee and are expected to handle (e.g., file not found, database connection failure).
    
    **Example**:
    
    ```java
    import java.io.*;
    
    public class CheckedExceptionExample {
        public static void main(String[] args) {
            try {
                FileReader file = new FileReader("nonexistentFile.txt");
                BufferedReader reader = new BufferedReader(file);
                System.out.println(reader.readLine());
            } catch (IOException e) {
                System.out.println("File not found or cannot be read.");
            }
        }
    }
    
    ```
    
    - **Explanation**:
        - `FileReader` and `BufferedReader` may throw `IOException`, so we **must handle it**.
    
    ---
    
    ### **Unchecked Exceptions**
    
    1. **Definition**: Unchecked exceptions are **not checked at compile-time**. They occur at runtime.
        - It’s **optional** to handle or declare them.
    2. **Examples**:
        - `NullPointerException`
        - `ArithmeticException`
        - `ArrayIndexOutOfBoundsException`
        - `IllegalArgumentException`
    3. **Use Case**: Used for programming bugs or unexpected scenarios (e.g., accessing a null object, dividing by zero).
    
    **Example**:
    
    ```java
    public class UncheckedExceptionExample {
        public static void main(String[] args) {
            int[] numbers = {1, 2, 3};
            try {
                System.out.println(numbers[5]); // Accessing invalid index
            } catch (ArrayIndexOutOfBoundsException e) {
                System.out.println("Array index is out of bounds!");
            }
        }
    }
    
    ```
    
    - **Explanation**:
        - `ArrayIndexOutOfBoundsException` is an unchecked exception and doesn’t need to be handled explicitly.
    
    ---
    
    ### **Key Differences**
    
    | **Aspect** | **Checked Exception** | **Unchecked Exception** |
    | --- | --- | --- |
    | **Compile-Time Check** | Checked by the compiler. | Not checked by the compiler. |
    | **Declaration/Handling** | Must be handled or declared. | Optional to handle or declare. |
    | **Examples** | `IOException`, `SQLException`. | `NullPointerException`, `ArithmeticException`. |
    | **Use Case** | Expected and recoverable issues. | Programming bugs or logic errors. |
    
    ---
    
    **In Summary**:
    
    - Use **checked exceptions** for predictable, recoverable errors (e.g., file not found).
    - Use **unchecked exceptions** for programming mistakes (e.g., accessing null).
- **Oops 4 concept**
    
    Object-Oriented Programming (OOP) is a programming paradigm that organizes software design around **objects** rather than functions and logic. OOP is built on four fundamental concepts:
    
    ---
    
    ### **1. Encapsulation**
    
    - **Definition**: Bundling the data (fields) and the methods (functions) that operate on the data into a single unit, known as a class. It also restricts direct access to some components to protect the integrity of the object.
    - **Key Idea**: Hide internal details and only expose what is necessary via public methods (getters/setters).
    
    **Example:**
    
    ```java
    class BankAccount {
        private double balance; // Data is hidden
    
        // Public method to access private data
        public double getBalance() {
            return balance;
        }
    
        public void deposit(double amount) {
            if (amount > 0) {
                balance += amount;
            }
        }
    } 
    ```
    
    Here, the `balance` is private and cannot be accessed directly; it's encapsulated.
    
    ---
    
    ### **2. Inheritance**
    
    - **Definition**: Allows one class (child or subclass) to inherit fields and methods from another class (parent or superclass). It promotes code reuse.
    - **Key Idea**: "Is-a" relationship. A subclass is a specialized version of the parent class.
    
    **Example:**
    
    ```java
    class Vehicle {
        void start() {
            System.out.println("Vehicle started");
        }
    }
    
    class Car extends Vehicle {
        void honk() {
            System.out.println("Car honking");
        }
    }
    
    public class Test {
        public static void main(String[] args) {
            Car car = new Car();
            car.start(); // Inherited from Vehicle
            car.honk();
        }
    }
    
    ```
    
    ---
    
    ### **3. Polymorphism**
    
    - **Definition**: The ability of an object to take on many forms. It allows the same method to perform different actions based on the object it is acting upon.
    - **Key Idea**: "One interface, multiple implementations."
    
    **Types:**
    
    1. **Compile-time Polymorphism (Method Overloading)**: Same method name, different parameter list.
    2. **Runtime Polymorphism (Method Overriding)**: Subclass provides its implementation of a method in the parent class.
    
    **Example:**
    
    ```java
    class Animal {
        void sound() {
            System.out.println("Animal makes a sound");
        }
    }
    
    class Dog extends Animal {
        @Override
        void sound() {
            System.out.println("Dog barks");
        }
    }
    
    public class Test {
        public static void main(String[] args) {
            Animal myAnimal = new Dog(); // Polymorphism
            myAnimal.sound(); // Output: Dog barks
        }
    }
    
    ```
    
    ---
    
    ### **4. Abstraction**
    
    - **Definition**: Hiding implementation details and showing only the essential features of an object. This is achieved using abstract classes or interfaces.
    - **Key Idea**: Focus on "what an object does" instead of "how it does it."
    
    **Example with Abstract Class:**
    
    ```java
    abstract class Shape {
        abstract void draw(); // Abstract method
    }
    
    class Circle extends Shape {
        @Override
        void draw() {
            System.out.println("Drawing a circle");
        }
    }
    
    public class Test {
        public static void main(String[] args) {
            Shape shape = new Circle(); // Abstraction
            shape.draw();
        }
    }
    
    ```
    
    ---
    
    ### **Summary of Concepts**
    
    1. **Encapsulation**: Data hiding (e.g., private fields, public getters/setters).
    2. **Inheritance**: Code reuse (e.g., subclass inherits from parent class).
    3. **Polymorphism**: Different behavior for the same method (e.g., method overriding).
    4. **Abstraction**: Hiding implementation details (e.g., abstract classes or interfaces).
    
    These concepts help create scalable, reusable, and maintainable code!
    
- **Can we create Instance of interface**
    
    **You cannot directly create an instance of an interface** because an interface is abstract by design, meaning it does not provide implementations for its methods.
    
    Using a Class that Implements the Interface
    
    ```java
    interface Animal {
        void sound();
    }
    
    class Dog implements Animal {
        public void sound() {
            System.out.println("Woof! Woof!");
        }
    }
    
    public class Test {
        public static void main(String[] args) {
            Animal dog = new Dog(); // Instance of class implementing the interface
            dog.sound();
        }
    }
    
    ```
    
    Lambda Expression for Functional Interfaces
    
    ```java
    @FunctionalInterface
    interface Greeting {
        void sayHello();
    }
    
    public class Test {
        public static void main(String[] args) {
            Greeting greeting = () -> System.out.println("Hello, World!"); // Lambda expression
            greeting.sayHello();
        }
    }
    
    ```
    
- **Are Rest API’s Stateless?**
    
    Yes
    
    It means that Each and every API hit from client to server should contain all the information required by the server to fulfill the request.
    server doesn’t stores any information regarding client session during request. 
    
- **AtomicIntegerArray**
    
    The **`AtomicIntegerArray`** is a thread-safe array of integers provided by the `java.util.concurrent.atomic` package in Java. It ensures that updates to its elements are atomic, meaning they happen without interference from other threads. This makes it particularly useful in concurrent programming, where multiple threads may need to update shared data without explicit synchronization.
    
    ---
    
    ### **Key Features of `AtomicIntegerArray`:**
    
    1. **Thread-Safe Operations**:
        - Provides atomic operations like `get()`, `set()`, `addAndGet()`, and `compareAndSet()` on array elements.
    2. **No Locks Required**:
        - Uses low-level atomic operations, such as Compare-And-Swap (CAS), eliminating the need for locks.
    3. **Immutable Size**:
        - The size of the array is fixed at the time of creation and cannot be resized.
    4. **Efficient**:
        - Performs better than manual synchronization mechanisms like `synchronized` or `ReentrantLock` for simple atomic updates.
    
    ---
    
    ### **Creating and Using an `AtomicIntegerArray`**
    
    ### **1. Basic Example:**
    
    ```java
    import java.util.concurrent.atomic.AtomicIntegerArray;
    
    public class AtomicIntegerArrayExample {
        public static void main(String[] args) {
            // Create an AtomicIntegerArray with a size of 5
            AtomicIntegerArray atomicArray = new AtomicIntegerArray(5);
    
            // Set values
            atomicArray.set(0, 10);
            atomicArray.set(1, 20);
    
            // Get values
            System.out.println("Value at index 0: " + atomicArray.get(0)); // Output: 10
            System.out.println("Value at index 1: " + atomicArray.get(1)); // Output: 20
    
            // Atomic increment
            atomicArray.getAndIncrement(0);
            System.out.println("Value at index 0 after increment: " + atomicArray.get(0)); // Output: 11
        }
    }
    ```
    
    ---
    
    ### **2. Using Compare-And-Set (CAS):**
    
    The `compareAndSet` method atomically sets a value if the current value matches the expected value.
    
    ```java
    public class CompareAndSetExample {
        public static void main(String[] args) {
            AtomicIntegerArray atomicArray = new AtomicIntegerArray(5);
    
            atomicArray.set(0, 10);
    
            boolean isUpdated = atomicArray.compareAndSet(0, 10, 15);
            System.out.println("Was the value updated? " + isUpdated); // Output: true
            System.out.println("Value at index 0: " + atomicArray.get(0)); // Output: 15
        }
    }
    
    ```
    
    ---
    
    ### **3. Using Add-And-Get:**
    
    The `addAndGet` method atomically adds a value to the current value and returns the updated value.
    
    ```java
    public class AddAndGetExample {
        public static void main(String[] args) {
            AtomicIntegerArray atomicArray = new AtomicIntegerArray(5);
    
            atomicArray.set(2, 5);
    
            int updatedValue = atomicArray.addAndGet(2, 10);
            System.out.println("Updated value at index 2: " + updatedValue); // Output: 15
        }
    }
    
    ```
    
    ---
    
    ### **Common Methods of `AtomicIntegerArray`**
    
    | Method | Description |
    | --- | --- |
    | `get(int index)` | Returns the current value at the specified index. |
    | `set(int index, int value)` | Sets the value at the specified index to the given value. |
    | `getAndSet(int index, int value)` | Atomically sets the value and returns the previous value. |
    | `getAndIncrement(int index)` | Atomically increments the value at the specified index. |
    | `getAndDecrement(int index)` | Atomically decrements the value at the specified index. |
    | `addAndGet(int index, int delta)` | Atomically adds the delta to the value at the specified index. |
    | `compareAndSet(int index, int expected, int update)` | Atomically sets the value if it matches the expected value. |
    
    ---
    
    ### **When to Use `AtomicIntegerArray`**
    
    - **Multi-threaded environments**: When multiple threads need to access and modify an array of integers without explicit synchronization.
    - **High performance**: For atomic operations on integer arrays without the overhead of locks.
    - **Fixed size**: When the size of the array is known and does not need to grow dynamically.
    
    `AtomicIntegerArray` is a robust and efficient choice for thread-safe integer array manipulations, especially in scenarios where synchronization costs need to be minimized.
    
- Difference between Serializable and Externalizable in Java ?
    
    Serializable is a marker interface with no methods defined it but Externalizable interface has two methods on it e.g. readExternal() and writeExternal() which allows you to control the serialization process. Serializable uses default serialization process which can be very slow for some application. 
    
- Difference between transient and volatile in Java?
    
    Transient keyword is used in Serialization while volatile is used in multi-threading. If you want to exclude a variable from serialization process then mark that variable transient. Similar to
    static variable, transient variables are not serialized. On the other hand, volatile variables are signal to compiler that multiple threads are interested on this variable and it shouldn’t reorder its access. volatile variable also follows happens-before relationship, which means any write happens before any read in volatile variable. You can also make non atomic access of double and long variable atomic using volatile.
    
- What is difference between start() and call() ?
    
    ## ✅ `start()` vs `call()` — Key Differences
    
    | Feature | `start()` | `call()` |
    | --- | --- | --- |
    | **Belongs to** | `Thread` class | `Callable` interface |
    | **Purpose** | Starts a new thread and invokes `run()` | Contains logic to execute in a thread |
    | **Returns** | `void` | A result of type `T` |
    | **Throws Exception** | Cannot throw checked exceptions | Can throw checked exceptions |
    | **Usage** | `thread.start()` | Submitted to `ExecutorService` |
    | **Creates New Thread?** | ✅ Yes | ❌ Not directly — only when submitted |
    
    ---
    
    ### 🔧 1. `start()` Example — Using `Thread` or `Runnable`
    
    ```java
    class MyThread extends Thread {
        public void run() {
            System.out.println("Running in thread: " + Thread.currentThread().getName());
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            MyThread thread = new MyThread();
            thread.start();  // Starts a new thread
        }
    }
    ```
    
    - `start()` creates a **new thread** and calls the `run()` method internally.
    - You should **not call `run()` directly** if you want true multithreading.
    
    ---
    
    ### 🌟 2. `call()` Example — Using `Callable`
    
    ```java
    class MyTask implements Callable<String> {
        public String call() {
            return "Executed by: " + Thread.currentThread().getName();
        }
    }
    
    public class Main {
        public static void main(String[] args) throws Exception {
            ExecutorService executor = Executors.newSingleThreadExecutor();
            Future<String> future = executor.submit(new MyTask());
            System.out.println(future.get());  // Waits and gets the result
            executor.shutdown();
        }
    }
    
    ```
    
    - `call()` is used in **`Callable`**, and must be submitted to an `ExecutorService`.
    - It **returns a result** and can **throw checked exceptions**.
    - It doesn't create a thread on its own — the executor does.
    
    ---
    
    ## 🧠 Summary in One Line:
    
    > 🔸 start() → Used with Thread to begin a new thread.
    > 
    > 
    > 🔸 `call()` → Used with `Callable` to run logic in a thread pool and return a value.
    > 
- What is the difference between stack and heap memory?
    
    **Stack Memory:**
    
    - **Purpose:**
        - The stack is used for static memory allocation. This means memory is allocated during compile time.
        - It primarily stores local variables and function call information.
    - **Management:**
        - Memory allocation and deallocation are automatic, following a Last-In, First-Out (LIFO) order.
        - The compiler manages the stack.
    - **Speed:**
        - Stack operations are very fast due to their simple LIFO structure.
    - **Size:**
        - The stack typically has a limited size.
    - **Lifespan:**
        - Variables stored on the stack have a short lifespan, existing only within the scope of their function.
    - **Errors:**
        - If the stack runs out of memory, a "stack overflow" error occurs.
    
    **Heap Memory:**
    
    - **Purpose:**
        - The heap is used for dynamic memory allocation. This means memory is allocated during runtime.
        - It's used to store objects and data structures that require a longer lifespan.
    - **Management:**
        - Memory allocation and deallocation are managed by a garbage collector.
        - This requires more complex memory management.
    - **Speed:**
        - Heap operations are generally slower than stack operations due to the complexity of memory management.
    - **Size:**
        - The heap has a much larger size compared to the stack.
    - **Lifespan:**
        - Variables stored on the heap can have a longer lifespan, persisting until they are explicitly deallocated.
    - **Errors:**
        - If the heap runs out of memory, an "out of memory" error occurs.
        - Memory leaks can also occur if the programmer does not deallocate memory that is no longer needed.
- What is the difference between method overloading and method overriding?
    
    ### **Method Overloading**
    
    - **Definition**: Defining multiple methods with the same name but different parameters within the same class.
    - **Purpose**: To achieve compile-time polymorphism (static binding).
    - **Parameters**: Must be different in type, number, or both.
    - **Return Type**: Can be the same or different.
    - **Access Modifier**: Can be changed.
    - **Static Methods**: Can be overloaded.
    - **Checked Exceptions**: Can be different for overloaded methods.
    - **Occurs In**: The same class.
    
    ### **Example of Method Overloading:**
    
    ```java
    class MathUtils {
        // Method with two int parameters
        int add(int a, int b) {
            return a + b;
        }
    
        // Method with three int parameters
        int add(int a, int b, int c) {
            return a + b + c;
        }
    
        // Method with two double parameters
        double add(double a, double b) {
            return a + b;
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            MathUtils obj = new MathUtils();
            System.out.println(obj.add(5, 10));       // Calls add(int, int)
            System.out.println(obj.add(5, 10, 15));  // Calls add(int, int, int)
            System.out.println(obj.add(5.5, 2.5));   // Calls add(double, double)
        }
    }
    ```
    
    ---
    
    ### **Method Overriding**
    
    - **Definition**: Redefining a method of a parent class in a subclass with the same method signature.
    - **Purpose**: To achieve runtime polymorphism (dynamic binding).
    - **Parameters**: Must be the same as the parent class.
    - **Return Type**: Must be the same or a covariant type (subtype of the return type in the parent class).
    - **Access Modifier**: Cannot reduce visibility but can be more permissive.
    - **Static Methods**: Cannot be overridden (but can be hidden).
    - **Checked Exceptions**: Cannot throw new or broader checked exceptions.
    - **Occurs In**: Parent-child class relationship.
    
    ### **Example of Method Overriding:**
    
    ```java
    class Animal {
        void sound() {
            System.out.println("Animal makes a sound");
        }
    }
    
    class Dog extends Animal {
        @Override
        void sound() {  // Overriding method
            System.out.println("Dog barks");
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Animal myAnimal = new Dog();  // Upcasting
            myAnimal.sound();  // Calls overridden method in Dog class
        }
    }
    ```
    
    **Output:**
    
    ```
    Dog barks
    ```
    
    ---
    
    ### **Key Differences**
    
    | Feature | Method Overloading | Method Overriding |
    | --- | --- | --- |
    | **Binding Type** | Compile-time (Static) | Runtime (Dynamic) |
    | **Method Name** | Same | Same |
    | **Parameters** | Different | Same |
    | **Return Type** | Can be different | Must be the same or covariant |
    | **Access Modifier** | Can be changed | Cannot be more restrictive |
    | **Static Methods** | Can be overloaded | Cannot be overridden (but can be hidden) |
    | **Checked Exceptions** | Can be different | Cannot be broader than in the parent class |
    | **Occurs In** | Same class | Parent-Child relationship |
- What is the difference between static methods, static variables, and static classes
in Java?
    
    ## **1. Static Methods**
    
    ### **Definition:**
    
    A **static method** belongs to the class rather than an instance and can be called without creating an object.
    
    ### **Characteristics:**
    
    - Defined using the `static` keyword.
    - Can be called using the class name (e.g., `ClassName.methodName()`).
    - Cannot access **instance variables** or **instance methods** directly.
    - Can only access **static variables** and **static methods**.
    - Useful for utility or helper methods (e.g., `Math.pow()`, `Collections.sort()`).
    
    ### **Example:**
    
    ```java
    class MathUtils {
        static int square(int num) {  // Static method
            return num * num;
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            int result = MathUtils.square(5); // Calling without creating an object
            System.out.println(result);  // Output: 25
        }
    }
    ```
    
    ---
    
    ## **2. Static Variables (Class Variables)**
    
    ### **Definition:**
    
    A **static variable** is a class-level variable shared across all instances of the class.
    
    ### **Characteristics:**
    
    - Declared using the `static` keyword.
    - **Memory is allocated once**, at the time of class loading.
    - **Shared across all objects** of the class.
    - Can be accessed using the class name (`ClassName.variableName`).
    - Useful for constants or properties that should be the same for all instances.
    
    ### **Example:**
    
    ```java
    class Counter {
        static int count = 0; // Static variable
    
        Counter() {
            count++; // Increments the static variable for every instance created
        }
    
        static void displayCount() {
            System.out.println("Count: " + count);
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Counter obj1 = new Counter();
            Counter obj2 = new Counter();
            Counter obj3 = new Counter();
    
            Counter.displayCount(); // Output: Count: 3
        }
    }
    ```
    
    Here, `count` is **shared** among all objects, so it maintains a cumulative count.
    
    ---
    
    ## **3. Static Classes (Nested Static Classes)**
    
    ### **Definition:**
    
    A **static class** is a **nested** (inner) class that does not require an instance of the outer class.
    
    ### **Characteristics:**
    
    - Declared using `static` within another class.
    - Cannot access **non-static members** of the outer class.
    - Can be instantiated without creating an object of the outer class.
    - Used for grouping helper functions, constants, or nested utilities.
    
    ### **Example:**
    
    ```java
    class OuterClass {
        static class StaticNestedClass { // Static inner class
            void display() {
                System.out.println("Inside Static Nested Class");
            }
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            // Creating an instance of the static nested class
            OuterClass.StaticNestedClass obj = new OuterClass.StaticNestedClass();
            obj.display(); // Output: Inside Static Nested Class
        }
    }
    ```
    
    Here, `StaticNestedClass` is **independent** of `OuterClass` instances.
    
- Real world example of when we can use Abstract class and Interfaces.
    
    ## **Scenario: Payment System (Abstract Class + Interface)**
    
    Imagine we are building a payment system that supports different payment methods like **Credit Card, PayPal, and UPI**.
    
    - **All payment methods should follow a standard structure** (e.g., `processPayment()` method).
    - **Some common behavior should be shared** (e.g., `validatePaymentDetails()` logic for checking empty details).
    - **Different payment methods might have their own unique logic** (e.g., OTP for Credit Card, email verification for PayPal).
    
    ### **Solution Using Abstract Class & Interface**
    
    1. **Use an `interface` (Payment) to define the mandatory behaviors that every payment method must implement.**
    2. **Use an `abstract class` (OnlinePayment) to provide common behavior like validation.**
    
    ---
    
    ### **Step 1: Define an Interface (Payment)**
    
    An interface ensures that all payment methods **must** have `processPayment()`.
    
    ```java
    // Interface: Defines mandatory behavior
    interface Payment {
        void(double amount);  // All payment methods must implement this
    }
    ```
    
    ---
    
    ### **Step 2: Define an Abstract Class (OnlinePayment)**
    
    This class contains **shared logic** for all online payments, such as validating payment details.
    
    ```java
    // Abstract class: Provides shared logic
    abstract class OnlinePayment implements Payment {
        String paymentMethod;
    
        // Constructor
        OnlinePayment(String paymentMethod) {
            this.paymentMethod = paymentMethod;
        }
    
        // Common method for validation (shared across all payments)
        boolean validatePaymentDetails(String details) {
            if (details == null || details.isEmpty()) {
                System.out.println("Invalid payment details for " + paymentMethod);
                return false;
            }
            return true;
        }
    
        // Abstract method (forces subclasses to provide specific implementation)
        abstract void authenticate();
    }
    
    ```
    
    ---
    
    ### **Step 3: Implement Specific Payment Methods**
    
    Each subclass **inherits** the common behavior from `OnlinePayment` and **implements its own logic**.
    
    ### **1️⃣ Credit Card Payment**
    
    - Needs **OTP authentication** before processing the payment.
    
    ```java
    
    class CreditCardPayment extends OnlinePayment {
        String cardNumber;
    
        // Constructor
        CreditCardPayment(String cardNumber) {
            super("Credit Card");
            this.cardNumber = cardNumber;
        }
    
        // Authentication step (unique to Credit Card)
        @Override
        void authenticate() {
            System.out.println("OTP verification done for Credit Card.");
        }
    
        // Implements interface method
        @Override
        public void processPayment(double amount) {
            if (validatePaymentDetails(cardNumber)) {
                authenticate();
                System.out.println("Payment of $" + amount + " processed via Credit Card.");
            }
        }
    }
    
    ```
    
    ### **2️⃣ PayPal Payment**
    
    - Requires **email authentication** instead of OTP.
    
    ```java
    class PayPalPayment extends OnlinePayment {
        String email;
    
        // Constructor
        PayPalPayment(String email) {
            super("PayPal");
            this.email = email;
        }
    
        // Authentication step (unique to PayPal)
        @Override
        void authenticate() {
            System.out.println("Email verification done for PayPal.");
        }
    
        // Implements interface method
        @Override
        public void processPayment(double amount) {
            if (validatePaymentDetails(email)) {
                authenticate();
                System.out.println("Payment of $" + amount + " processed via PayPal.");
            }
        }
    }
    ```
    
    ### **3️⃣ UPI Payment (Implements Interface Directly)**
    
    - UPI doesn't need additional authentication, so it **implements the interface directly** without an abstract class.
    
    ```java
    
    class UpiPayment implements Payment {
        String upiId;
    
        UpiPayment(String upiId) {
            this.upiId = upiId;
        }
    
        @Override
        public void processPayment(double amount) {
            if (upiId == null || upiId.isEmpty()) {
                System.out.println("Invalid UPI ID.");
                return;
            }
            System.out.println("Payment of $" + amount + " processed via UPI.");
        }
    }
    ```
    
    ---
    
    ### **Step 4: Test the Payment System**
    
    ```java
    public class PaymentSystem {
        public static void main(String[] args) {
            Payment creditCard = new CreditCardPayment("1234-5678-9876-5432");
            creditCard.processPayment(100.0);
    
            Payment paypal = new PayPalPayment("user@example.com");
            paypal.processPayment(50.0);
    
            Payment upi = new UpiPayment("user@upi");
            upi.processPayment(30.0);
        }
    }
    
    ```
    
    ---
    
    ### **💡 Why Use Both Abstract Class & Interface?**
    
    | Feature | Interface (Payment) | Abstract Class (OnlinePayment) |
    | --- | --- | --- |
    | **Purpose** | Defines required behaviors (`processPayment()`) | Provides shared logic (`validatePaymentDetails()`) |
    | **Flexibility** | Allows different implementations (UPI doesn’t extend `OnlinePayment`) | Enforces authentication behavior for online payments |
    | **Multiple Inheritance** | Java allows multiple interfaces, so a class can implement multiple behaviors | Since Java does not support multiple inheritance, it avoids unnecessary restrictions |
    
    ---
    
    ### **Final Thoughts**
    
    ✅ **Interfaces** are best for defining a contract (all payments must implement `processPayment()`).
    
    ✅ **Abstract classes** are best when multiple subclasses **share common behavior** (e.g., authentication in online payments).
    
- **Stack**
    
    ### What is a Stack in Data Structure?
    
    A **stack** is a linear data structure that follows the **LIFO** (Last In, First Out) principle. This means the last element added to the stack is the first one to be removed. Think of it as a stack of plates: the last plate you put on top is the first plate you take off.
    
    ---
    
    ### **Key Characteristics of a Stack**
    
    1. **LIFO (Last In, First Out)**: The most recently added item is removed first.
    2. **Restricted Access**:
        - You can only add or remove elements from the top of the stack.
        - You cannot access elements in the middle of the stack directly.
    3. **Basic Operations**:
        - **Push**: Add an element to the top of the stack.
        - **Pop**: Remove an element from the top of the stack.
        - **Peek/Top**: View the top element without removing it.
        - **IsEmpty**: Check if the stack is empty.
        - **Size**: Get the number of elements in the stack.
    
    ---
    
    ### **Real-Life Examples of a Stack**
    
    - **Undo/Redo functionality** in text editors.
    - **Browser History**: The last visited page is at the top of the stack.
    - **Backtracking** in puzzles like mazes or games.
    
    ---
    
    ### **How a Stack Works**
    
    Imagine we are using a stack to store numbers. Initially, the stack is empty:
    
    ### **Operations**
    
    1. **Push (Add)**
        - Push `10` → Stack: `[10]`
        - Push `20` → Stack: `[10, 20]`
        - Push `30` → Stack: `[10, 20, 30]`
    2. **Pop (Remove)**
        - Pop → Removes `30` → Stack: `[10, 20]`
        - Pop → Removes `20` → Stack: `[10]`
    3. **Peek/Top**
        - Peek → Returns `10` (top element) without removing it.
    
    ---
    
    ### **Representation of a Stack**
    
    - Stacks can be implemented using:
        1. **Arrays** (fixed size).
        2. **Linked Lists** (dynamic size).
    
    ### **Using an Array**
    
    Here’s how you might represent a stack using an array:
    
    ```java
    class Stack {
        int[] stack;
        int top;
        int maxSize;
    
        public Stack(int size) {
            stack = new int[size];
            top = -1; // Indicates an empty stack
            maxSize = size;
        }
    
        public void push(int data) {
            if (top == maxSize - 1) {
                System.out.println("Stack Overflow");
                return;
            }
            stack[++top] = data;
        }
    
        public int pop() {
            if (top == -1) {
                System.out.println("Stack Underflow");
                return -1;
            }
            return stack[top--];
        }
    
        public int peek() {
            if (top == -1) {
                System.out.println("Stack is Empty");
                return -1;
            }
            return stack[top];
        }
    
        public boolean isEmpty() {
            return top == -1;
        }
    }
    
    ```
    
    ### **Using a Linked List**
    
    Linked lists allow dynamic memory allocation, so you don’t have to define a fixed size for the stack.
    
    ```java
    package afternewgenprac.DSA.Stack;
    
    public class StackImplUsingLL {
    
        static class Node{
            int val;
            Node next;
            Node(int val){
                this.val = val;
                this.next = null;
            }
        }
        static class stack{
            static Node head ;
            public static boolean isEmpty(){
                return head == null;
            }
    
            public static void push(int data){
                Node newNode = new Node(data);
                if(isEmpty()){
                    head = newNode;
                    return;
                }
                newNode.next = head;
                head = newNode;
            }
    
            public static int pop(){
                if(isEmpty()){
                    return -1;
                }
                int top = head.val;
                head = head.next;
                return top;
            }
    
            public static int peek(){
                if(isEmpty()){
                    return -1;
                }
                return head.val;
            }
    
        }
    
        public static void main(String[] args) {
            stack stackImplUsingLL = new stack();
            stackImplUsingLL.push(10);
            stackImplUsingLL.push(20);
            stackImplUsingLL.push(30);
            stackImplUsingLL.push(40);
            stackImplUsingLL.push(50);
    
            while(!stackImplUsingLL.isEmpty()){
                System.out.print(stackImplUsingLL.peek() + " ");
                stackImplUsingLL.pop();
            }
            System.out.println();
    
        }
    }
    
    ```
    
    ---
    
    ### **Advantages of a Stack**
    
    1. **Easy to implement**.
    2. **Efficient**: Operations like push and pop are O(1).
    
    ---
    
    ### **Disadvantages of a Stack**
    
    1. Limited access: Only the top element is accessible.
    2. Fixed size (in array-based implementations).
    
    ---
    
    ### **Common Applications of Stacks**
    
    1. **Expression Evaluation**:
        - Convert infix to postfix or evaluate postfix expressions.
    2. **Function Call Stack**:
        - The system uses a stack to manage function calls and their return addresses.
    3. **Backtracking**:
        - Maze solving, game moves, etc.
    4. **Parsing**:
        - Check for balanced parentheses in expressions.
    
    ---
    
    Let me know if you’d like a deeper dive into any of these aspects!
    

---

- **Optimistic vs. Pessimistic Locking**
    
    ### 🔐 **1. Pessimistic Locking**
    
    ### 📌 What it is:
    
    - Assumes **conflicts are likely**.
    - Locks the data **before** access, blocking others from modifying it until the lock is released.
    
    ### 📦 Example in a database:
    
    ```sql
    SELECT * FROM products WHERE id = 1 FOR UPDATE;
    ```
    
    - This query **locks** the selected row. No one else can update it until the transaction completes.
    
    ### ✅ Pros:
    
    - Safe for high-contention scenarios.
    - Prevents lost updates.
    
    ### ❌ Cons:
    
    - Can cause deadlocks or slow performance due to locking.
    - Reduces concurrency.
    
    ### 🌈 **2. Optimistic Locking**
    
    ### 📌 What it is:
    
    - Assumes **conflicts are rare**.
    - Doesn't lock the data at read time but checks for changes **before updating**.
    - Uses a **version number** or **timestamp** to detect changes.
    
    ### 📦 Example using versioning (Java JPA/Hibernate):
    
    ```java
    @Entity
    public class Product {
        @Id
        private Long id;
    
        private String name;
    
        @Version  // Enables optimistic locking
        private int version;
    }
    
    ```
    
    - When updating, Hibernate checks if the version has changed since it was read.
    - If it has, it throws an **OptimisticLockException**.
    
    ### ✅ Pros:
    
    - High concurrency.
    - Better performance in low-conflict environments.
    
    ### ❌ Cons:
    
    - Can lead to update failures if conflicts do happen.
    - Requires retry logic.
    
    ---
    
    ### 🧠 Summary:
    
    | Feature | Pessimistic Locking | Optimistic Locking |
    | --- | --- | --- |
    | Assumption | Conflicts are common | Conflicts are rare |
    | Locking | Locks data before access | No locks, checks version/timestamp |
    | Concurrency | Lower | Higher |
    | Performance | May be slower due to locks | Faster (usually) |
    | Conflict handling | Prevents it up front | Detects and resolves on commit |
    | Use cases | High contention | Low contention |
    - **Optimistic Locking:** Assumes conflicts are rare. Checks for modifications at the time of update using a version number or timestamp. If a conflict is detected, the update fails (e.g., OptimisticLockException).
    - **Pessimistic Locking:** Assumes conflicts are likely. Acquires a lock on the data when it's read for update, preventing other transactions from modifying it until the lock is released (e.g., using SELECT FOR UPDATE in databases).
- Explain the purpose of the java.util.concurrent package.
    
    ## 🔹 **Purpose of `java.util.concurrent` Package**
    
    The main goal is to **provide powerful concurrency utilities** that:
    
    - Make multithreaded code **easier to write, read, and maintain**
    - Offer **better performance and scalability** than manual `synchronized` blocks
    - Avoid common pitfalls like **deadlocks, race conditions, and thread starvation**
    
    ---
    
    ## 🔸 **Key Components of `java.util.concurrent`**
    
    ### 1. **Executor Framework**
    
    - Manages threads for you, separating task submission from execution.
    - Avoids manual thread creation.
    
    ```java
    ExecutorService executor = Executors.newFixedThreadPool(3);
    executor.submit(() -> System.out.println("Task running"));
    executor.shutdown();
    ```
    
    ✅ Use for better thread pooling and task management.
    
    ---
    
    ### 2. **Concurrent Collections**
    
    Thread-safe alternatives to standard collections.
    
    | Traditional Collection | Concurrent Version |
    | --- | --- |
    | `ArrayList` | `CopyOnWriteArrayList` |
    | `HashMap` | `ConcurrentHashMap` |
    | `Queue` | `ConcurrentLinkedQueue` |
    | `Deque` | `ConcurrentLinkedDeque` |
    
    ✅ Allow multiple threads to read/write without manual locking.
    
    ---
    
    ### 3. **Atomic Variables**
    
    - Lock-free, thread-safe operations on single variables.
    - Found in `java.util.concurrent.atomic` package.
    
    Examples:
    
    - `AtomicInteger`
    - `AtomicBoolean`
    - `AtomicReference`
    
    ✅ Prevent race conditions with minimal overhead.
    
    ---
    
    ### 4. **Locks and Synchronizers**
    
    More flexible and powerful alternatives to `synchronized`.
    
    - `ReentrantLock`
    - `ReadWriteLock`
    - `Semaphore`
    - `CountDownLatch`
    - `CyclicBarrier`
    - `Phaser`
    
    ✅ Allow fine-grained control over locking, waiting, and coordination between threads.
    
    ---
    
    ### 5. **Futures and Callables**
    
    - `Callable<T>` is like `Runnable`, but it returns a result and can throw exceptions.
    - `Future<T>` lets you retrieve the result of an async task.
    
    ```java
    ExecutorService executor = Executors.newSingleThreadExecutor();
    Future<Integer> future = executor.submit(() -> 10 + 20);
    System.out.println(future.get()); // Output: 30
    ```
    
    ---
    
    ## 🚀 Benefits:
    
    - **Improves performance** and scalability
    - **Reduces complexity** of concurrent code
    - Encourages **best practices** in multithreading
- Explain the Lock interface and its advantages over synchronized.
    
    ## 🔹 **What is the `Lock` Interface?**
    
    The `Lock` interface (in `java.util.concurrent.locks`) provides **explicit locking mechanisms** — similar to `synchronized` blocks, but with more flexibility and control.
    
    ### 🔧 Basic Usage:
    
    ```java
    import java.util.concurrent.locks.Lock;
    import java.util.concurrent.locks.ReentrantLock;
    
    Lock lock = new ReentrantLock();
    
    lock.lock();
    try {
        // critical section
    } finally {
        lock.unlock(); // always unlock in finally block
    }
    
    ```
    
    ---
    
    ## 🔸 **Key Methods in `Lock` Interface**
    
    | Method | Description |
    | --- | --- |
    | `lock()` | Acquires the lock (waits if necessary) |
    | `unlock()` | Releases the lock |
    | `tryLock()` | Tries to acquire the lock **without blocking** |
    | `tryLock(long, TimeUnit)` | Tries to acquire the lock within a timeout |
    | `lockInterruptibly()` | Allows the thread to be interrupted while waiting for the lock |
    
    ---
    
    ## ✅ **Advantages of `Lock` Over `synchronized`**
    
    | Feature | `synchronized` | `Lock` Interface |
    | --- | --- | --- |
    | **Interruptible Locking** | ❌ Not possible | ✅ `lockInterruptibly()` |
    | **Try Without Blocking** | ❌ Not supported | ✅ `tryLock()` |
    | **Timed Locking** | ❌ Not supported | ✅ `tryLock(timeout)` |
    | **Fairness Policy** | ❌ Not configurable | ✅ Can be made fair (`ReentrantLock(true)`) |
    | **Multiple Conditions** | ❌ Only one wait set | ✅ Supports multiple via `Condition` |
    | **Explicit Unlocking** | ❌ Automatic via scope | ✅ Manual via `unlock()` |
    
    ---
    
    ## 🌀 Example: Using `tryLock()` to avoid deadlocks
    
    ```java
    java
    CopyEdit
    if (lock.tryLock()) {
        try {
            // critical section
        } finally {
            lock.unlock();
        }
    } else {
        // do something else instead of waiting
    }
    
    ```
    
    ---
    
    ## ⚠️ Be Careful:
    
    With great power comes responsibility:
    
    - You **must always release the lock** in a `finally` block.
    - Forgetting to `unlock()` can lead to **deadlocks** or **starvation**.
    
    ---
    
    ### 🔁 Common Implementation: `ReentrantLock`
    
    - Allows re-entrant locking (same thread can acquire multiple times)
    - Can be **fair** (first-come, first-served) or **non-fair** (default)
- What is ReentrantLock and how does it differ from synchronized?
    
    ## 🔹 **What is `ReentrantLock`?**
    
    `ReentrantLock` is a class in `java.util.concurrent.locks` that implements the `Lock` interface. It provides an **explicit and flexible locking mechanism**, offering more control than `synchronized`.
    
    > “Reentrant” means a thread can acquire the same lock multiple times without getting blocked.
    > 
    
    ---
    
    ## 🔧 **Basic Example:**
    
    ```java
    import java.util.concurrent.locks.ReentrantLock;
    
    public class SharedResource {
        private final ReentrantLock lock = new ReentrantLock();
    
        public void doWork() {
            lock.lock(); // acquire lock
            try {
                // critical section
                System.out.println(Thread.currentThread().getName() + " is working");
            } finally {
                lock.unlock(); // release lock
            }
        }
    }
    ```
    
    ---
    
    ## 🔄 **Reentrancy Example:**
    
    ```java
    public void outer() {
        lock.lock();
        try {
            inner(); // This works because the lock is reentrant
        } finally {
            lock.unlock();
        }
    }
    
    public void inner() {
        lock.lock();
        try {
            // Do something
        } finally {
            lock.unlock();
        }
    }
    ```
    
    ---
    
    ## ✅ **Differences: `ReentrantLock` vs `synchronized`**
    
    | Feature | `synchronized` | `ReentrantLock` |
    | --- | --- | --- |
    | **Lock Type** | Implicit | Explicit (`lock()` / `unlock()`) |
    | **Reentrancy** | ✅ Yes | ✅ Yes |
    | **Interruptible Lock Acquisition** | ❌ No | ✅ Yes (`lockInterruptibly()`) |
    | **Try Lock Without Blocking** | ❌ No | ✅ Yes (`tryLock()`) |
    | **Timed Lock** | ❌ No | ✅ Yes (`tryLock(timeout)`) |
    | **Fairness Policy** | ❌ Not configurable | ✅ Yes (`new ReentrantLock(true)`) |
    | **Condition Support** | ❌ Only `wait/notify` | ✅ `Condition` objects (`await/signal`) |
    | **Performance (low contention)** | ✅ Slightly better | ⚠️ Slight overhead |
    
    ---
    
    ## 🛠️ When to Use `ReentrantLock` Instead of `synchronized`?
    
    - When you need **try-locking**
    - When you want **interruptible waiting**
    - When you want **timed lock attempts**
    - When you want **fairness** (first-come-first-served thread execution)
    - When you need **multiple conditions** to manage threads (like multiple wait-sets)
    
    ---
    
    ## ⚠️ Caveat:
    
    Unlike `synchronized`, `ReentrantLock` doesn’t automatically release the lock:
    
    ```java
    java
    CopyEdit
    lock.lock();
    // If an exception is thrown here, lock might never be released!
    
    ```
    
    🔁 So **always release the lock in a `finally` block**.
    
- What are ReadWriteLock and StampedLock?
    
    Both `ReadWriteLock` and `StampedLock` are concurrency tools in Java used to improve **performance in read-heavy scenarios**, by allowing **multiple readers** to access a resource **simultaneously**, as long as no writer is active.
    
    Let’s break them down.
    
    ---
    
    ## 🔹 **ReadWriteLock**
    
    ### ✅ Purpose:
    
    `ReadWriteLock` allows:
    
    - **Multiple threads to read** a resource **at the same time**
    - **Only one thread to write**, and **no readers** allowed during writing
    
    ### 🔧 Key Implementation: `ReentrantReadWriteLock`
    
    ```java
    import java.util.concurrent.locks.ReentrantReadWriteLock;
    
    ReentrantReadWriteLock lock = new ReentrantReadWriteLock();
    Lock readLock = lock.readLock();
    Lock writeLock = lock.writeLock();
    ```
    
    ### 🔁 Usage Example:
    
    ```java
    // Reading
    readLock.lock();
    try {
        // safe read
    } finally {
        readLock.unlock();
    }
    
    // Writing
    writeLock.lock();
    try {
        // safe write
    } finally {
        writeLock.unlock();
    }
    ```
    
    ### ✅ Benefits:
    
    - Great for scenarios where **reads outnumber writes**
    - Reduces contention compared to using a regular `ReentrantLock` or `synchronized`
    
    ---
    
    ## 🔸 **StampedLock** (Java 8+)
    
    ### ✅ Purpose:
    
    An advanced version of `ReadWriteLock` that supports:
    
    - **Optimistic reading** (very lightweight)
    - **Read and write locks**, similar to `ReadWriteLock`
    
    ### 🔧 How It Works:
    
    - Every lock returns a **stamp (long)**, which must be passed back when unlocking.
    - Optimistic read doesn’t block writes but requires **validation**.
    
    ### 🔁 Usage Example:
    
    ```java
    import java.util.concurrent.locks.StampedLock;
    
    StampedLock lock = new StampedLock();
    
    // Optimistic read
    long stamp = lock.tryOptimisticRead();
    int value = sharedData;
    // validate if no write occurred
    if (!lock.validate(stamp)) {
        stamp = lock.readLock();
        try {
            value = sharedData;
        } finally {
            lock.unlockRead(stamp);
        }
    }
    
    // Write lock
    stamp = lock.writeLock();
    try {
        sharedData = newValue;
    } finally {
        lock.unlockWrite(stamp);
    }
    ```
    
    ---
    
    ## 🔍 **ReadWriteLock vs StampedLock**
    
    | Feature | `ReadWriteLock` | `StampedLock` |
    | --- | --- | --- |
    | Supports multiple readers | ✅ Yes | ✅ Yes |
    | Optimistic read | ❌ No | ✅ Yes (`tryOptimisticRead()`) |
    | Requires stamp to unlock | ❌ No | ✅ Yes |
    | Fairness option | ✅ (optional in `ReentrantReadWriteLock`) | ❌ No fairness control |
    | Lock reentrancy | ✅ Yes | ❌ No (non-reentrant) |
    
    ---
    
    ## ⚠️ When to Use:
    
    - Use **`ReadWriteLock`** when:
        - You need simple shared read / exclusive write behavior
        - You want reentrant support
    - Use **`StampedLock`** when:
        - You have **many more reads than writes**
        - You want **maximum performance**, especially with optimistic reads
        - You don’t need reentrancy
- Explain CountDownLatch with an example.
    
    ## 🔹 What is `CountDownLatch`?
    
    `CountDownLatch` is a synchronization aid in `java.util.concurrent` that lets **one or more threads wait until a set of operations being performed in other threads completes**.
    
    ### 🔧 How It Works:
    
    - Initialized with a count (e.g., `new CountDownLatch(3)`)
    - Each time `countDown()` is called, the count decreases
    - Threads waiting on `await()` will block **until the count reaches zero**
    
    ---
    
    ## ✅ Use Case:
    
    Imagine you have **3 worker threads** doing some task, and the **main thread should wait until all workers finish**.
    
    ---
    
    ## 🧑‍💻 Code Example:
    
    ```java
    import java.util.concurrent.CountDownLatch;
    
    public class CountDownLatchExample {
    
        public static void main(String[] args) throws InterruptedException {
            int numberOfWorkers = 3;
            CountDownLatch latch = new CountDownLatch(numberOfWorkers);
    
            for (int i = 1; i <= numberOfWorkers; i++) {
                int workerId = i;
                new Thread(() -> {
                    try {
                        System.out.println("Worker " + workerId + " is doing work...");
                        Thread.sleep(1000); // Simulate work
                        System.out.println("Worker " + workerId + " finished work.");
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    } finally {
                        latch.countDown(); // Decrement the latch count
                    }
                }).start();
            }
    
            System.out.println("Main thread is waiting for workers to finish...");
            latch.await(); // Wait until count reaches 0
            System.out.println("All workers are done. Main thread proceeds.");
        }
    }
    ```
    
    ---
    
    ## 🧠 Output (Order may vary slightly):
    
    ```
    Main thread is waiting for workers to finish...
    Worker 1 is doing work...
    Worker 2 is doing work...
    Worker 3 is doing work...
    Worker 1 finished work.
    Worker 2 finished work.
    Worker 3 finished work.
    All workers are done. Main thread proceeds.
    ```
    
    ---
    
    ## 🔍 Summary:
    
    - Use `CountDownLatch` when **a thread must wait for others** to complete tasks.
    - It’s **one-time use only**. Once the count reaches 0, it can’t be reset.
- What is CyclicBarrier and how does it differ from CountDownLatch?
    
    ## What is `CyclicBarrier`?
    
    `CyclicBarrier` is a synchronization aid that allows **a group of threads to wait for each other** to reach a common barrier point.
    
    - It's called **cyclic** because it **can be reused** after the waiting threads are released.
    - Unlike `CountDownLatch`, which is a **one-time-use**, `CyclicBarrier` can be **reset and reused**.
    
    ---
    
    ## ✅ Use Case:
    
    You have **multiple threads**, and you want them to **start the next phase of processing together** after all reach a certain point.
    
    ---
    
    ## 🧑‍💻 Code Example:
    
    ```java
    import java.util.concurrent.BrokenBarrierException;
    import java.util.concurrent.CyclicBarrier;
    
    public class CyclicBarrierExample {
    
        public static void main(String[] args) {
            int numThreads = 3;
    
            // Barrier action runs when all threads reach the barrier
            CyclicBarrier barrier = new CyclicBarrier(numThreads, () -> {
                System.out.println("All threads reached the barrier. Let's proceed together!");
            });
    
            for (int i = 1; i <= numThreads; i++) {
                int threadId = i;
                new Thread(() -> {
                    System.out.println("Thread " + threadId + " is doing initial work...");
                    try {
                        Thread.sleep(1000); // Simulate work
                        System.out.println("Thread " + threadId + " waiting at the barrier...");
                        barrier.await(); // Wait for other threads
                        System.out.println("Thread " + threadId + " continues after the barrier.");
                    } catch (InterruptedException | BrokenBarrierException e) {
                        e.printStackTrace();
                    }
                }).start();
            }
        }
    }
    ```
    
    ---
    
    ## 🧠 Output (example):
    
    ```
    Thread 1 is doing initial work...
    Thread 2 is doing initial work...
    Thread 3 is doing initial work...
    Thread 2 waiting at the barrier...
    Thread 1 waiting at the barrier...
    Thread 3 waiting at the barrier...
    All threads reached the barrier. Let's proceed together!
    Thread 3 continues after the barrier.
    Thread 1 continues after the barrier.
    Thread 2 continues after the barrier.
    ```
    
    ---
    
    ## 🔄 `CyclicBarrier` vs `CountDownLatch`
    
    | Feature | `CountDownLatch` | `CyclicBarrier` |
    | --- | --- | --- |
    | **Resettable** | ❌ One-time use only | ✅ Can be reused (cyclic) |
    | **Waits for** | 1 or more threads to finish | A fixed number of threads to arrive |
    | **Barrier Action** | ❌ No | ✅ Optional runnable task on barrier |
    | **Typical Use Case** | Waiting for tasks to complete | All threads sync at one point |
    | **Thread coordination** | One or more wait, others count down | All threads wait at the barrier |
    
    ---
    
    ## 💡 Summary:
    
    - Use `*CountDownLatch**` when you want one or more threads to **wait for others to complete**.
    - Use `*CyclicBarrier**` when you want a group of threads to **wait for each other**, and potentially **do it again**.
- What is Semaphore and when would you use it?
    
    ## 🔹 What is a `Semaphore`?
    
    A **`Semaphore`** controls access to a **shared resource** by **limiting the number of threads** that can access it at the same time.
    
    - You initialize it with a **number of permits**.
    - Threads call `acquire()` to get a permit and `release()` to return it.
    
    ---
    
    ## ✅ Use Case:
    
    You want to **limit concurrent access** — for example:
    
    - Only 3 users can access a printer at the same time.
    - A thread pool must limit task execution to a fixed number of slots.
    
    ---
    
    ## 🧑‍💻 Code Example: Limiting Access to 2 Threads
    
    ```java
    import java.util.concurrent.Semaphore;
    
    public class SemaphoreExample {
    
        public static void main(String[] args) {
            Semaphore semaphore = new Semaphore(2); // Only 2 threads can access at a time
    
            for (int i = 1; i <= 5; i++) {
                int threadId = i;
                new Thread(() -> {
                    try {
                        System.out.println("Thread " + threadId + " is trying to acquire a permit...");
                        semaphore.acquire(); // Acquire a permit
                        System.out.println("Thread " + threadId + " acquired a permit.");
    
                        Thread.sleep(2000); // Simulate some work
    
                        System.out.println("Thread " + threadId + " is releasing the permit.");
                        semaphore.release(); // Release the permit
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }).start();
            }
        }
    
    ```
    
    ---
    
    ### 🧠 Sample Output:
    
    ```
    
    Thread 1 is trying to acquire a permit...
    Thread 2 is trying to acquire a permit...
    Thread 1 acquired a permit.
    Thread 2 acquired a permit.
    Thread 3 is trying to acquire a permit...
    Thread 4 is trying to acquire a permit...
    Thread 5 is trying to acquire a permit...
    Thread 1 is releasing the permit.
    Thread 3 acquired a permit.
    Thread 2 is releasing the permit.
    Thread 4 acquired a permit.
    ...
    ```
    
    ---
    
    ## 🔸 Key Methods:
    
    | Method | Description |
    | --- | --- |
    | `acquire()` | Blocks if no permit is available |
    | `tryAcquire()` | Tries to acquire without blocking |
    | `release()` | Releases a permit |
    | `availablePermits()` | Returns current number of free permits |
    
    ---
    
    ## 🔄 When to Use `Semaphore`?
    
    Use it when:
    
    - You need to **limit concurrent access** to a **resource** (DB connections, API calls, etc.)
    - You’re building something like **rate limiters**, **resource pools**, or **bounded task processors**
    
    ---
    
    ## ✅ Bonus Tip:
    
    Semaphores can also be used to implement **binary semaphores** (1 permit), which act like **mutual exclusion locks** — similar to `ReentrantLock`.
    
- What is Exchanger and when would you use it?
    
    ## What is `Exchanger`?
    
    An **`Exchanger<T>`** is a synchronization point where **two threads** can **exchange data** with each other.
    
    > It's like saying:
    > 
    > 
    > “I’ll give you mine, you give me yours — and we swap at the same time.”
    > 
    
    ---
    
    ## ✅ Use Case:
    
    You have **two threads** doing work in parallel, and at some point, they **need to exchange results**, e.g.:
    
    - Two threads processing chunks of data and swapping intermediate results.
    - One thread produces data, the other consumes, and they need to sync up in pairs.
    
    ---
    
    ## 🧑‍💻 Code Example: Two threads exchanging strings
    
    ```java
    import java.util.concurrent.Exchanger;
    
    public class ExchangerExample {
    
        public static void main(String[] args) {
            Exchanger<String> exchanger = new Exchanger<>();
    
            // Thread A
            new Thread(() -> {
                String dataA = "Data from Thread A";
                try {
                    System.out.println("Thread A is waiting to exchange...");
                    String received = exchanger.exchange(dataA);
                    System.out.println("Thread A received: " + received);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }).start();
    
            // Thread B
            new Thread(() -> {
                String dataB = "Data from Thread B";
                try {
                    System.out.println("Thread B is waiting to exchange...");
                    String received = exchanger.exchange(dataB);
                    System.out.println("Thread B received: " + received);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
    
    ```
    
    ---
    
    ### 🧠 Sample Output:
    
    ```
    Thread A is waiting to exchange...
    Thread B is waiting to exchange...
    Thread B received: Data from Thread A
    Thread A received: Data from Thread B
    ```
    
    ---
    
    ## 🔄 Key Method:
    
    | Method | Description |
    | --- | --- |
    | `exchange(V value)` | Waits for another thread, then swaps data |
    | `exchange(V value, timeout, unit)` | Same but with timeout |
    
    ---
    
    ## 🔥 When to Use `Exchanger`?
    
    - When you have **exactly two threads** that need to **meet and exchange data**.
    - For **pipeline** or **double-buffering** scenarios:
        - One thread fills a buffer
        - Another processes it
        - They **exchange buffers** via `Exchanger`
    
    ---
    
    ## 🚫 What `Exchanger` is NOT for:
    
    - It’s **not for general data sharing** across multiple threads (use `BlockingQueue` or shared locks instead).
    - **Only works with pairs** — if a third thread tries to exchange, it will block until a fourth joins.
    
    ---
    
    ## 📝 Summary:
    
    | Feature | `Exchanger` |
    | --- | --- |
    | Type | Pairwise data exchange between threads |
    | Synchronization | Both threads must arrive to proceed |
    | Use Cases | Double buffering, producer-consumer pairs |
    | Thread-safe | ✅ Yes |
- What is a thread pool? Why use them?
    
    ## What is a Thread Pool?
    
    A **thread pool** is a collection of **pre-instantiated, reusable threads** that can be used to execute tasks concurrently.
    
    > Instead of creating a new thread for every task, you reuse threads — which is much more efficient, especially under high load.
    > 
    
    ---
    
    ## ✅ Why Use a Thread Pool?
    
    | Benefit | Description |
    | --- | --- |
    | ✅ Better Performance | Avoids overhead of thread creation/destruction |
    | ✅ Resource Management | Controls how many threads run at once (prevents out-of-memory issues) |
    | ✅ Task Queueing | Automatically queues extra tasks if all threads are busy |
    | ✅ Easy to Use | Java provides built-in support via `ExecutorService` and `Executors` |
    | ✅ Cleaner Code | Simplifies managing multiple threads and lifecycle |
    
    ---
    
    ## 📦 Real-World Use Case:
    
    - Handling **web requests** in a server
    - Running **parallel computations**
    - Scheduling **background tasks**
    - Managing **batch jobs** or **file processing**
    
    ---
    
    ## 🧑‍💻 Code Example Using `ExecutorService`
    
    ```java
    java
    CopyEdit
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    
    public class ThreadPoolExample {
    
        public static void main(String[] args) {
            // Create a thread pool with 3 threads
            ExecutorService executor = Executors.newFixedThreadPool(3);
    
            // Submit 5 tasks to the pool
            for (int i = 1; i <= 5; i++) {
                int taskId = i;
                executor.submit(() -> {
                    System.out.println("Task " + taskId + " is running on " + Thread.currentThread().getName());
                    try {
                        Thread.sleep(2000); // Simulate task execution
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println("Task " + taskId + " completed.");
                });
            }
    
            // Shutdown the executor after task submission
            executor.shutdown();
        }
    }
    
    ```
    
    ---
    
    ### 🧠 Sample Output:
    
    ```
    arduino
    CopyEdit
    Task 1 is running on pool-1-thread-1
    Task 2 is running on pool-1-thread-2
    Task 3 is running on pool-1-thread-3
    Task 1 completed.
    Task 4 is running on pool-1-thread-1
    Task 2 completed.
    Task 5 is running on pool-1-thread-2
    ...
    
    ```
    
    ---
    
    ## 🔧 Common Thread Pool Types in Java:
    
    | Method | Description |
    | --- | --- |
    | `Executors.newFixedThreadPool(n)` | Fixed number of threads |
    | `Executors.newCachedThreadPool()` | Dynamic thread count (reuses idle threads) |
    | `Executors.newSingleThreadExecutor()` | One thread only |
    | `Executors.newScheduledThreadPool(n)` | For delayed or periodic tasks |
    
    ---
    
    ## 📝 Summary:
    
    - A **thread pool** is a reusable group of threads used to execute tasks concurrently.
    - It improves performance, reduces resource usage, and simplifies multi-threaded programming.
- What is the difference between Executor and Executors and ExecutorService ?
    
    ## 1. `Executor` (Interface)
    
    - Introduced in Java 5 (`java.util.concurrent`)
    - **Minimal interface** with a single method:
        
        ```java
        void execute(Runnable command);
        ```
        
    - It provides a simple way to **decouple task submission from thread creation**.
    
    ### ✅ Example:
    
    ```java
    Executor executor = command -> new Thread(command).start();
    executor.execute(() -> System.out.println("Running task!"));
    ```
    
    > Think of it as a functional interface — like a basic contract to execute something.
    > 
    
    ---
    
    ## 🔹 2. `ExecutorService` (Interface)
    
    - **Sub-interface of `Executor`**.
    - Provides a much more **powerful and flexible framework** for managing and controlling threads.
    - Supports:
        - **Lifecycle management** (shutdown, awaitTermination)
        - **Task submission with return values** via `Callable`
        - **Futures** to track task results
    
    ### ✅ Example:
    
    ```java
    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.submit(() -> System.out.println("Task executed!"));
    executor.shutdown();
    ```
    
    ---
    
    ## 🔹 3. `Executors` (Utility Class)
    
    - It’s a **factory class** that provides **static methods** to create implementations of `Executor` and `ExecutorService`.
    
    ### ✅ Common Factory Methods:
    
    | Method | Description |
    | --- | --- |
    | `Executors.newFixedThreadPool(n)` | Thread pool with a fixed number of threads |
    | `Executors.newCachedThreadPool()` | Creates new threads as needed (unbounded) |
    | `Executors.newSingleThreadExecutor()` | Only one thread to execute tasks serially |
    | `Executors.newScheduledThreadPool(n)` | Schedule tasks with delay or at intervals |
    
    ---
    
    ## 🎯 Summary Table
    
    | Term | Type | Purpose |
    | --- | --- | --- |
    | `Executor` | Interface | Basic interface with `execute()` |
    | `ExecutorService` | Interface | Adds more features: lifecycle, `submit()`, `shutdown()` |
    | `Executors` | Utility Class | Factory to create different thread pools |
    
    ---
    
    ## 🧑‍💻 Quick Code Example (Using All Three)
    
    ```java
    import java.util.concurrent.*;
    
    public class ExecutorHierarchyExample {
        public static void main(String[] args) {
            // Using Executors (factory)
            ExecutorService executorService = Executors.newFixedThreadPool(2); // ExecutorService extends Executor
    
            // Submit a task (Callable returns result)
            Future<String> future = executorService.submit(() -> {
                Thread.sleep(1000);
                return "Task Result";
            });
    
            try {
                System.out.println("Result: " + future.get()); // wait for result
            } catch (Exception e) {
                e.printStackTrace();
            }
    
            executorService.shutdown(); // shut down the executor
        }
    }
    
    ```
    
- If you need to perform a task on your **server**, would you choose `newCachedThreadPool()` or `newFixedThreadPool(int n)`? And **why**?
    
    ## ✅ Best Answer:
    
    **I would choose `newFixedThreadPool(int n)`** in most server-side scenarios.
    
    ---
    
    ## 🔹 Why?
    
    ### `newFixedThreadPool(n)`:
    
    - **Limits the number of concurrent threads**.
    - Prevents the server from being **overwhelmed by too many tasks**.
    - Useful when you want **predictable resource usage** (CPU, memory).
    - Helps avoid **OutOfMemoryError** or **CPU thrashing** under high load.
    
    ### `newCachedThreadPool()`:
    
    - Creates **new threads as needed** (potentially unbounded).
    - **Reuses idle threads**, but can **spawn thousands of threads** under heavy load.
    - May lead to **resource exhaustion** if the server is hit by too many requests at once.
    
    ---
    
    ## 🧑‍💻 Example:
    
    Let’s say you’re building an API server that processes file uploads.
    
    - ✅ With `newFixedThreadPool(10)`:
        - You allow up to 10 file processing tasks in parallel.
        - Others will **wait in a queue**, keeping memory/CPU usage in check.
    - ❌ With `newCachedThreadPool()`:
        - Every incoming file request creates a **new thread if no idle one exists**.
        - 1000 requests = 1000 threads → Risk of server crash!
    
    ---
    
    ## 📌 Summary:
    
    | Criteria | `newFixedThreadPool(n)` | `newCachedThreadPool()` |
    | --- | --- | --- |
    | Thread limit | Fixed | Unbounded |
    | Resource usage | Predictable | Can grow quickly |
    | Best for | Controlled, stable workloads | Lightweight, bursty, short-lived tasks |
    | Server-safe (under load) | ✅ Yes | ❌ Risky under high load |
    
    ---
    
    ## ✅ Final Answer (Interview Style):
    
    > On a server, I would prefer newFixedThreadPool(n) because it provides bounded concurrency, avoids resource exhaustion, and ensures stable, predictable behavior under load.
    > 
    > 
    > `newCachedThreadPool()` is better suited for **short-lived**, **bursty** tasks where you’re confident the load will not overwhelm system resources.
    > 
- Explain the Executor framework and its components.
    
    ## What is the Executor Framework?
    
    The **Executor Framework** is a part of the `java.util.concurrent` package that provides a **standardized way to manage and control thread execution** — without manually creating or managing threads.
    
    > It decouples task submission from task execution, letting you manage thread lifecycles more cleanly and efficiently.
    > 
    
    ---
    
    ## ⚙️ Core Components of the Executor Framework
    
    | Component | Type | Purpose |
    | --- | --- | --- |
    | `Executor` | Interface | Basic contract with `execute(Runnable)` |
    | `ExecutorService` | Interface | Extends Executor, adds lifecycle & `submit()`, `invokeAll()`, etc. |
    | `ScheduledExecutorService` | Interface | ExecutorService + supports scheduled and periodic tasks |
    | `Executors` | Utility Class | Factory methods to create executor instances |
    | `Future<T>` | Interface | Represents the result of an asynchronous task |
    | `Callable<V>` | Interface | Like `Runnable` but returns a result and can throw exceptions |
    | `ThreadPoolExecutor` | Class | Core implementation of the thread pool with full control |
    
    ---
    
    ## 🔧 Common Factory Methods (`Executors` class):
    
    ```java
    java
    CopyEdit
    Executors.newFixedThreadPool(5);         // Fixed number of threads
    Executors.newCachedThreadPool();         // Scales dynamically (risky under heavy load)
    Executors.newSingleThreadExecutor();     // One task at a time
    Executors.newScheduledThreadPool(2);     // Supports delayed or periodic execution
    
    ```
    
    ---
    
    ## 🧑‍💻 Example: Fixed Thread Pool + Callable + Future
    
    ```java
    java
    CopyEdit
    import java.util.concurrent.*;
    
    public class ExecutorFrameworkExample {
    
        public static void main(String[] args) {
            // Create a thread pool of 3 threads
            ExecutorService executor = Executors.newFixedThreadPool(3);
    
            // Submit a task using Callable to return a result
            Future<String> future = executor.submit(() -> {
                Thread.sleep(1000);
                return "Task completed by " + Thread.currentThread().getName();
            });
    
            try {
                // Get the result (blocks until the result is available)
                String result = future.get();
                System.out.println(result);
            } catch (Exception e) {
                e.printStackTrace();
            }
    
            // Shutdown the executor
            executor.shutdown();
        }
    }
    
    ```
    
    ---
    
    ### ✅ Output:
    
    ```
    arduino
    CopyEdit
    Task completed by pool-1-thread-1
    
    ```
    
    ---
    
    ## 📦 Summary of Interfaces
    
    | Interface | Key Method(s) | Purpose |
    | --- | --- | --- |
    | `Executor` | `execute(Runnable task)` | Basic task execution |
    | `ExecutorService` | `submit()`, `shutdown()`, `invokeAll()` | Managed thread pool execution |
    | `ScheduledExecutorService` | `schedule()`, `scheduleAtFixedRate()` | Scheduling tasks with delays |
    | `Callable<V>` | `call()` | Tasks that return results |
    | `Future<V>` | `get()`, `cancel()` | Track task status/result |
    
    ---
    
    ## ✅ Advantages of Executor Framework:
    
    - Thread reuse → Better performance
    - Thread control → Avoids creating too many threads
    - Asynchronous execution → Via `Future` and `Callable`
    - Easy to schedule tasks (`ScheduledExecutorService`)
    - Cleaner, scalable, and more maintainable code
- What are the different types of thread pools in Java?
    
    ## 1. `newFixedThreadPool(int n)`
    
    ### ✅ Description:
    
    - Creates a thread pool with a **fixed number of threads**.
    - Excess tasks wait in a **queue** until a thread becomes available.
    
    ### 📌 Use Case:
    
    - You want **bounded concurrency**.
    - Ideal when you know the number of threads needed (like CPU cores).
    
    ### 🧑‍💻 Example:
    
    ```java
    
    ExecutorService executor = Executors.newFixedThreadPool(3);
    ```
    
    ---
    
    ## 🔹 2. `newCachedThreadPool()`
    
    ### ✅ Description:
    
    - Creates a **dynamic thread pool**.
    - Threads are **reused if idle**, otherwise new ones are created.
    - No upper bound on threads! 😨
    
    ### 📌 Use Case:
    
    - Lightweight, **short-lived** tasks.
    - Use only when task count is **not excessive**.
    
    ### 🧑‍💻 Example:
    
    ```java
    ExecutorService executor = Executors.newCachedThreadPool();
    ```
    
    ---
    
    ## 🔹 3. `newSingleThreadExecutor()`
    
    ### ✅ Description:
    
    - Uses **only one thread** to execute tasks **sequentially**.
    - Tasks are queued and run **one at a time**, in order.
    
    ### 📌 Use Case:
    
    - You need **guaranteed sequential execution**.
    - Suitable for logging, background sync, or single-user tasks.
    
    ### 🧑‍💻 Example:
    
    ```java
    ExecutorService executor = Executors.newSingleThreadExecutor();
    ```
    
    ---
    
    ## 🔹 4. `newScheduledThreadPool(int n)`
    
    ### ✅ Description:
    
    - A thread pool that can **schedule tasks** to run after a delay or periodically.
    
    ### 📌 Use Case:
    
    - Repeated tasks like **cron jobs**, **heartbeat pings**, or **timeouts**.
    
    ### 🧑‍💻 Example:
    
    ```java
    ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(2);
    scheduler.scheduleAtFixedRate(() -> {
        System.out.println("Running task every 2 seconds");
    }, 0, 2, TimeUnit.SECONDS);
    ```
    
    ---
    
    ## 🔹 5. `newWorkStealingPool()`
    
    ### ✅ Description:
    
    - Uses **ForkJoinPool** under the hood.
    - **Parallelism level = number of available processors**.
    - Steals tasks from other threads' queues to balance the load.
    
    ### 📌 Use Case:
    
    - **CPU-intensive parallel processing**, divide-and-conquer algorithms.
    
    ### 🧑‍💻 Example:
    
    ```java
    
    ExecutorService executor = Executors.newWorkStealingPool();
    ```
    
    > ⚠️ Requires tasks to be independent and non-blocking.
    > 
    
    ---
    
    ## ✅ Comparison Summary
    
    | Thread Pool Type | Description | When to Use |
    | --- | --- | --- |
    | `newFixedThreadPool(n)` | Fixed-size pool | Controlled task rate |
    | `newCachedThreadPool()` | Grows as needed, reuses idle threads | Short-lived, many tasks |
    | `newSingleThreadExecutor()` | Single thread, sequential execution | Ordered task execution |
    | `newScheduledThreadPool(n)` | Scheduled or periodic tasks | Timers, retries, background checks |
    | `newWorkStealingPool()` | Parallelism via ForkJoin | Parallel, divide-and-conquer tasks |
- What is ForkJoinPool and when would you use it?
    
    `ForkJoinPool` is a special kind of thread pool designed to **split tasks into smaller subtasks**, execute them in parallel, and **combine the results**.
    
    > It was introduced in Java 7 (java.util.concurrent) and is used by the Fork/Join Framework and parallel streams.
    > 
- How do you handle exceptions in thread pools?
    
    ## 1. **Exceptions in `execute(Runnable)`**
    
    When you submit a task using `execute()`:
    
    ```java
    executor.execute(() -> {
        throw new RuntimeException("Oops!");
    });
    ```
    
    ➡️ **Unhandled exceptions are silently swallowed** by the thread — they won’t be visible unless you **log them manually** or use a custom `ThreadFactory`.
    
    ### ✅ Recommended Solution: Use an `UncaughtExceptionHandler`
    
    ```java
    ThreadFactory customFactory = runnable -> {
        Thread t = new Thread(runnable);
        t.setUncaughtExceptionHandler((thread, throwable) ->
            System.err.println("Uncaught exception: " + throwable.getMessage())
        );
        return t;
    };
    
    ExecutorService executor = Executors.newFixedThreadPool(2, customFactory);
    executor.execute(() -> {
        throw new RuntimeException("Boom in execute()");
    });
    ```
    
    ---
    
    ## 🔹 2. **Exceptions in `submit(Callable)` or `submit(Runnable)`**
    
    If you use `submit()`, **exceptions are captured in the `Future`**, not thrown immediately.
    
    ```java
    ExecutorService executor = Executors.newFixedThreadPool(2);
    
    Future<?> future = executor.submit(() -> {
        throw new RuntimeException("Boom in submit()");
    });
    
    try {
        future.get(); // This will throw ExecutionException
    } catch (ExecutionException e) {
        System.out.println("Caught exception: " + e.getCause());
    }
    ```
    
    > 📌 Key point: Always call future.get() to catch exceptions if you're using submit()!
    > 
    
    ---
    
    ## 🔹 3. **ScheduledExecutorService Exception Handling**
    
    If a task in a **scheduled executor** throws an exception, **it will suppress all future executions** of that task (in `scheduleAtFixedRate()` or `scheduleWithFixedDelay()`).
    
    ### ✅ Fix: Catch & log exceptions within the task itself
    
    ```java
    ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
    
    scheduler.scheduleAtFixedRate(() -> {
        try {
            throw new RuntimeException("Boom in scheduled task");
        } catch (Exception e) {
            System.err.println("Caught exception in scheduled task: " + e.getMessage());
        }
    }, 0, 1, TimeUnit.SECONDS);
    ```
    
    ---
    
    ## 🔧 Summary Table
    
    | Submission Method | How Exception is Handled | Best Practice |
    | --- | --- | --- |
    | `execute(Runnable)` | Exception is swallowed unless manually handled | Use custom `ThreadFactory` |
    | `submit(Runnable)` | Exception is wrapped in `Future` | Always call `future.get()` |
    | `submit(Callable)` | Same as above |  |
    | `ScheduledExecutorService` | Stops future executions if task crashes | Catch & log exceptions inside task |
    
    ---
    
    ## ✅ Pro Tip: Custom ThreadPoolExecutor for Global Exception Handling
    
    You can also extend `ThreadPoolExecutor` to handle exceptions globally:
    
    ```java
    class ExceptionHandlingThreadPool extends ThreadPoolExecutor {
        public ExceptionHandlingThreadPool(int corePoolSize, int maxPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue) {
            super(corePoolSize, maxPoolSize, keepAliveTime, unit, workQueue);
        }
    
        @Override
        protected void afterExecute(Runnable r, Throwable t) {
            super.afterExecute(r, t);
            if (t == null && r instanceof Future<?>) {
                try {
                    ((Future<?>) r).get();
                } catch (ExecutionException ee) {
                    t = ee.getCause();
                } catch (InterruptedException | CancellationException ignored) {}
            }
            if (t != null) {
                System.err.println("Caught exception: " + t.getMessage());
            }
        }
    }
    ```
    
- What is composition in OOP?
    
    **Composition** in **Object-Oriented Programming (OOP)** is a design principle in which one class contains a reference to another class (i.e., it **"has-a"** relationship). Instead of inheriting behavior from a parent class (as in **inheritance**), composition allows one class to be composed of one or more objects of other classes, enabling greater flexibility and modularity.
    
    ---
    
    ### 🔑 Key Point:
    
    **Composition** means a class contains an instance of another class to reuse its functionality.
    
    ---
    
    ### ✅ Example in Java:
    
    ```java
    
    // Engine class
    class Engine {
        public void start() {
            System.out.println("Engine started");
        }
    }
    
    // Car class that uses Engine (composition)
    class Car {
        private Engine engine;
    
        public Car() {
            this.engine = new Engine();  // Car HAS-A Engine
        }
    
        public void startCar() {
            engine.start();  // Delegating functionality to Engine
            System.out.println("Car is ready to go!");
        }
    }
    
    // Main class
    public class Main {
        public static void main(String[] args) {
            Car car = new Car();
            car.startCar();
        }
    }
    ```
    
    ### 🧠 Why Use Composition Over Inheritance?
    
    | Composition | Inheritance |
    | --- | --- |
    | "Has-a" relationship | "Is-a" relationship |
    | More flexible | Less flexible |
    | Supports runtime behavior changes | Fixed behavior |
    | Favors delegation | Favors extension |
    
    ---
    
    ### 💡 Benefits of Composition:
    
    - Promotes **code reuse** without tightly coupling classes.
    - Easier to **change behavior** by changing composed objects.
    - Avoids problems of **deep inheritance hierarchies**.
- What is Aggregation in OOP ?
    
    **Aggregation** is a special form of association in Object-Oriented Programming where one class contains a reference to another class, but both can exist independently. It represents a **"Has-A" relationship**, just like composition, but **with weaker coupling**.
    
    ---
    
    ### 🔍 **Definition:**
    
    Aggregation is when one object **"has a"** another object, but that object can **exist independently** of the parent.
    
    ---
    
    ### 📌 **Real-world Example:**
    
    - A **Department** has **Teachers**.
    - If the **Department** is deleted, the **Teachers** still exist (they may belong to another department).
    - Hence, this is **Aggregation**, not Composition.
    
    ---
    
    ### ✅ **Key Points:**
    
    | Feature | Aggregation | Composition |
    | --- | --- | --- |
    | Type | Weak "Has-A" | Strong "Has-A" |
    | Lifetime | Independent | Dependent |
    | Ownership | Shared | Owned |
    | Example | Department ↔ Teacher | House → Room |
    
    ---
    
    ### 💡 **Code Example (Aggregation in Java):**
    
    ```java
    class Teacher {
        String name;
    
        Teacher(String name) {
            this.name = name;
        }
    }
    
    class Department {
        String name;
        List<Teacher> teachers;
    
        Department(String name, List<Teacher> teachers) {
            this.name = name;
            this.teachers = teachers;
        }
    }
    
    public class Test {
        public static void main(String[] args) {
            Teacher t1 = new Teacher("Alice");
            Teacher t2 = new Teacher("Bob");
    
            List<Teacher> teacherList = new ArrayList<>();
            teacherList.add(t1);
            teacherList.add(t2);
    
            Department dept = new Department("Computer Science", teacherList);
    
            System.out.println("Department: " + dept.name);
            for (Teacher t : dept.teachers) {
                System.out.println("Teacher: " + t.name);
            }
        }
    }
    ```
    
    ### 🔄 What happens if `Department` is deleted?
    
    Teachers `t1` and `t2` still exist in memory (if referenced elsewhere). That’s **Aggregation**.
    
- What are the changes in garbage collector after java 8 which is the defualt garbage collector
    
    ## 🧹 **Default Garbage Collector in Java (by version)**
    
    | Java Version | Default GC |
    | --- | --- |
    | Java 8 | **Parallel GC** |
    | Java 9–14 | **G1 GC** |
    | Java 15+ | **G1 GC**, then options like ZGC, Shenandoah |
    | Java 21 (LTS) | **G1 GC**, ZGC and Shenandoah are now mature and stable |
    
    ---
    
    ## ✅ **Java 8 Garbage Collector (Default: Parallel GC)**
    
    ### 🔹 Parallel GC (aka Throughput Collector)
    
    - Uses **multiple threads** for minor and major GC.
    - **Stop-the-world**: Pauses all application threads during collection.
    - Prioritizes **throughput** (CPU efficiency) over low pause time.
    
    ---
    
    ## 🔄 **Changes and New GC Features After Java 8**
    
    ### 🚀 **Java 9+: G1 GC becomes the default**
    
    - G1 (Garbage-First) GC aims for **low-pause times** and **predictable latency**.
    - Divides heap into **regions** instead of contiguous generations.
    - Performs **concurrent and parallel** collection.
    - Prioritizes collecting regions with most garbage first.
    
    ---
    
    ### 🧪 **Java 10+: Improvements in G1**
    
    - **Automatic heap sizing** improvements.
    - Improved **pause time predictability**.
    - Concurrent reference processing.
    
    ---
    
    ### 🧪 **Java 11+: Flight Recorder and GC logging enhancements**
    
    - Better **diagnostic information** for GC.
    - `Xlog:gc*` replaces old `XX:+PrintGCDetails`.
    
    ---
    
    ### 🌀 **Java 12: Abortable Mixed GC in G1**
    
    - G1 can now **abort long mixed collections** if pause target is exceeded.
    
    ---
    
    ### ⚡ **Java 14+:**
    
    - Introduction of **ZGC (Z Garbage Collector)** and **Shenandoah GC** as production-ready:
        - **ZGC:** Very low pause times (<10ms) for large heaps (multi-terabyte).
        - **Shenandoah:** Low-pause GC designed by Red Hat, works well for applications needing responsiveness.
    
    ---
    
    ### ⚙️ **Java 15+: Parallel GC Improvements**
    
    - Improvements in **Parallel Old GC** phase.
    - **Text blocks**, new `Records`, and other features are GC-friendly.
    
    ---
    
    ## 🧠 **How to Check the Default GC in your Java Version**
    
    You can check the default GC by using this command:
    
    ```bash
    bash
    CopyEdit
    java -XX:+PrintCommandLineFlags -version
    
    ```
    
    Look for flags like:
    
    - `UseParallelGC` (Java 8 default)
    - `UseG1GC` (Java 9+ default)
    
    ---
    
    ## 🧪 **How to Change the GC Manually**
    
    You can specify a different GC like this:
    
    | GC Type | JVM Option |
    | --- | --- |
    | Serial GC | `-XX:+UseSerialGC` |
    | Parallel GC | `-XX:+UseParallelGC` |
    | G1 GC | `-XX:+UseG1GC` |
    | ZGC | `-XX:+UseZGC` (Java 11+) |
    | Shenandoah GC | `-XX:+UseShenandoahGC` (Java 12+) |
    
    ---
    
    ## 📌 Summary
    
    | Java Version | Default GC | Major GC Enhancements |
    | --- | --- | --- |
    | Java 8 | ParallelGC | Good throughput, long pauses |
    | Java 9+ | G1GC | Predictable pause times, region-based collection |
    | Java 11+ | ZGC, Shenandoah (optional) | Ultra-low latency, concurrent compaction |
    | Java 17/21 | G1GC still default, ZGC stable | Multiple GCs mature for various workloads |
- Difference between concurrent and parallel
    
    ## 🔄 **Concurrent vs Parallel – Key Difference**
    
    | Feature | **Concurrent** | **Parallel** |
    | --- | --- | --- |
    | **Definition** | Tasks are started and progressed **together** (interleaved), but not necessarily at the same time. | Tasks are **executed at the same time** on multiple processors/cores. |
    | **Goal** | **Responsiveness** – keeping the system interactive. | **Speed** – doing more in less time. |
    | **Execution** | May happen on a **single or multiple threads**. | Happens on **multiple threads/cores**. |
    | **Example** | Switching between downloading and UI updates. | Performing multiple downloads simultaneously. |
    | **Real-life analogy** | A single chef preparing multiple dishes by switching between them. | Multiple chefs each making a dish simultaneously. |
    
    ---
    
    ## 📘 **Examples in Java**
    
    ### 🔁 **Concurrent Example**
    
    A garbage collector running **concurrently** with the application means:
    
    - The GC thread runs **alongside** the application threads.
    - The application keeps running (with minimal pause).
    
    ```java
    java
    CopyEdit
    // Pseudo example
    // App thread and GC thread take turns accessing memory
    
    ```
    
    ### ⚡ **Parallel Example**
    
    A GC with **parallel** processing:
    
    - Uses **multiple threads** to perform garbage collection **simultaneously**.
    - All application threads are typically paused (stop-the-world).
    
    ```java
    java
    CopyEdit
    // JVM spawns 4 threads to collect different parts of the heap in parallel
    
    ```
    
    ---
    
    ## 🔄 Garbage Collector Example
    
    | GC Phase | **Concurrent** | **Parallel** |
    | --- | --- | --- |
    | **G1 GC** | Some phases (marking) are concurrent | Some phases (young GC) are parallel |
    | **ZGC / Shenandoah** | Mostly concurrent to keep pauses <10ms | Internal work may be parallelized |
    | **Parallel GC** | Not concurrent – application is paused | Fully parallel – uses multiple threads |
    
    ---
    
    ## 🧠 Summary in One Line:
    
    > Concurrent = Progress together (not necessarily simultaneously).
    > 
    > 
    > **Parallel** = Execute simultaneously.
    > 
- **what happens behind the scenes** when you call `.sort()` in Java
    
    ## 🔁 What Happens Internally in `.sort()`?
    
    When you call:
    
    ```java
    List<Integer> list = Arrays.asList(5, 2, 9, 1, 3);
    Collections.sort(list); // or list.sort(null);
    ```
    
    Java sorts the list using the **natural order** defined by the elements' class — in this case, `Integer`.
    
    ---
    
    ### 🔧 How does it work?
    
    1. `Integer` implements `Comparable<Integer>` interface.
    2. Inside it, this method is defined:
        
        ```java
        public int compareTo(Integer anotherInteger) {
            return this.value - anotherInteger.value;
        }
        ```
        
    
    ---
    
    ## 📊 Now let’s break down:
    
    ### `a.compareTo(b)` returns:
    
    | Return Value | Meaning | Action Taken (by sort) |
    | --- | --- | --- |
    | **Negative** (`< 0`) | `a < b` → `a` comes **before** `b` | Swap if needed |
    | **Zero** (`== 0`) | `a == b` → **No change** in order | No swap |
    | **Positive** (`> 0`) | `a > b` → `a` comes **after** `b` | Swap if needed |
    
    ---
    
    ### ✅ Example Walkthrough
    
    Let’s take this list:
    
    ```java
    List<Integer> list = Arrays.asList(5, 2, 9);
    Collections.sort(list);
    ```
    
    ### Step-by-step:
    
    1. Compares 5 and 2:
        
        ```java
        5.compareTo(2) → 5 - 2 = 3 (positive)
        ```
        
        → 5 should come **after** 2 → swap them.
        
    2. Compares 2 and 9:
        
        ```java
        2.compareTo(9) → 2 - 9 = -7 (negative)
        ```
        
        → 2 comes **before** 9 → keep as is.
        
    3. Compares 5 and 9 (after first swap):
        
        ```java
        5.compareTo(9) → 5 - 9 = -4 (negative)
        ```
        
        → 5 comes **before** 9 → done.
        
    
    ✅ Result: `[2, 5, 9]`
    
    ---
    
    ## ⚙️ What Algorithm is Used?
    
    - Before Java 7: **Merge Sort**
    - Java 7+: Uses **TimSort** (a hybrid of Merge Sort and Insertion Sort)
        - Very fast for **partially sorted** data
        - Stable and efficient
    
    ---
    
    ## 📌 Summary
    
    - `.sort()` uses `compareTo()` (or `compare()` if a Comparator is provided).
    - Returns:
        - `1` or negative: left comes before right
        - `0`: both are equal
        - `+1` or positive: left comes after right
    - Sorting uses **TimSort** (optimized for real-world data)

---

# Java Collection

- What is the difference between ArrayList and Vector
- What is Comparable and Comparator Interface ?
    
    ## 🔍 **What is Comparable?**
    
    `Comparable` is an interface used to define the **natural ordering** of objects.
    
    ### ✅ Key Points:
    
    - Found in `java.lang` package.
    - Contains only **one method**:
        
        ```java
        int compareTo(T o);
        ```
        
    - You implement this in the class whose objects you want to sort.
    
    ---
    
    ### 🧪 **Example using Comparable**
    
    ```java
    class Student implements Comparable<Student> {
        int id;
        String name;
    
        Student(int id, String name) {
            this.id = id;
            this.name = name;
        }
    
        // Sorting by id (natural order)
        public int compareTo(Student s) {
            return this.id - s.id;
        }
    
        public String toString() {
            return id + " " + name;
        }
    }
    
    public class ComparableExample {
        public static void main(String[] args) {
            List<Student> list = new ArrayList<>();
            list.add(new Student(102, "John"));
            list.add(new Student(101, "Alice"));
            list.add(new Student(103, "Bob"));
    
            Collections.sort(list); // Uses compareTo
    
            for (Student s : list) {
                System.out.println(s);
            }
        }
    }
    ```
    
    **Output:**
    
    ```
    101 Alice
    102 John
    103 Bob
    ```
    
    ---
    
    ## 🔍 **What is Comparator?**
    
    `Comparator` is used to define **custom or multiple sorting logic** outside the class.
    
    ### ✅ Key Points:
    
    - Found in `java.util` package.
    - Useful when:
        - You can’t modify the class.
        - You want multiple sorting strategies.
    - Has method:
        
        ```java
        java
        CopyEdit
        int compare(T o1, T o2);
        
        ```
        
    
    ---
    
    ### 🧪 **Example using Comparator**
    
    ```java
    java
    CopyEdit
    class Student {
        int id;
        String name;
    
        Student(int id, String name) {
            this.id = id;
            this.name = name;
        }
    
        public String toString() {
            return id + " " + name;
        }
    }
    
    // Sort by name
    class SortByName implements Comparator<Student> {
        public int compare(Student a, Student b) {
            return a.name.compareTo(b.name);
        }
    }
    
    public class ComparatorExample {
        public static void main(String[] args) {
            List<Student> list = new ArrayList<>();
            list.add(new Student(102, "John"));
            list.add(new Student(101, "Alice"));
            list.add(new Student(103, "Bob"));
    
            Collections.sort(list, new SortByName()); // Uses Comparator
    
            for (Student s : list) {
                System.out.println(s);
            }
        }
    }
    
    ```
    
    **Output:**
    
    ```
    CopyEdit
    101 Alice
    103 Bob
    102 John
    
    ```
    
    ---
    
    ## 🆚 **Difference: Comparable vs Comparator**
    
    | Feature | `Comparable` | `Comparator` |
    | --- | --- | --- |
    | Package | `java.lang` | `java.util` |
    | Method | `compareTo(T o)` | `compare(T o1, T o2)` |
    | Sorting logic | Defined **inside** the class | Defined **outside** the class |
    | Flexibility | One sort order | Multiple sort orders possible |
    | Use case | Natural ordering | Custom/multiple ordering |
    
    ---
    
    ## ✅ Java 8+ Comparator with Lambda
    
    ```java
    java
    CopyEdit
    list.sort((s1, s2) -> s1.name.compareTo(s2.name));
    
    ```
    
- How do you remove elements during Iteration?
    
    Iterator also has a method remove() when remove is called, the current element in the iteration is deleted.
    
- What are the main implementation of the List Interface
    1. ArrayList
    2. LinkedList
    3. Vector
    4. CopyOnWriteArrayList
- What are the advantages of ArrayList over Arrays ?
    
    ArrayList is resizable , can accommodate any data type.
    
    Array is Strong typed collection and of Fixed Length.
    
- How to obtain Array from an ArrayList ?
    
    ```java
    List<Integer> arrayList=new ArrayList<>();
    arrayList.add(10);
    Object a[]=arrayList.toArray();
    ```
    
- Why are Iterator returned by ArrayList called Fail Fast ?
    
    Because, if list is structurally modified at any time after the iterator is created , in any way except through the iterator’s own remove or add methods, the iterator will throw a ConcurrentModifiedException.
    
- Why we override equals() method ?
    
    The equals method is used to check whether two objects are same or not. it need to be overridden if we want to check the objects based on property.
    
- What is hash-collision in Hashtable and how it is handled in Java ?
    
    2 different keys with same hashValue is known as hash collisions .
    
- What is Garbage Collection ?
    
    Garbage collection is regaining the unused memory from a variable or object.
    
- What is the diff b/w Collection and Collections
    
    Collection is an interface which give implementation to List, Set , 
    
    Collections is a class → It’s a Utility class which has some methods for Sorting searching Collection object.
    
- How to synchronize List, Set and Map elements ?
    
    Synchronized List
    
    1. List<String> synchronizedList=Collections.synchronizedList(new ArrayList<>());
    2. `Set<String> synchronizedSet = Collections.synchronizedSet(new HashSet<>());`
    3. `Map<String, String> synchronizedMap = Collections.synchronizedMap(new HashMap<>());`
    
    ### Important Notes:
    
    - These methods return a thread-safe version of the collection.
    - You must manually synchronize when iterating over the collection to avoid `ConcurrentModificationException`:
        
        java
        
        Copy
        
        ```java
        synchronized (synchronizedList) {
            Iterator<String> iterator = synchronizedList.iterator();
            while (iterator.hasNext()) {
                String item = iterator.next();
                // Process item
            }
        }
        ```
        
- What is the advantage of generic collection ?
    
    Generics is used for typecasting and type saftey.
    
- What is the default size of load factor in hashing based collection ?
    
    The default size of load factor is 0.75 . The default capacity is computed as initial capacity * load factor. For example , 16 * 0.75 =12 . So , 12 is the default capacity of Map.
    
- What are the basic interfaces of Java Collections Framework ?
    
    Collection - >Set → SortedSet, List, Queue, Deque
    
    Map→ SortedMap
    
- What is difference between Iterator and ListIterator ?
- What is the difference between Fail-fast and Fail-safe ?
    
    Fail fast Iterator :
    
    We need to remember two points about this . Point one is, this behaviour is not guaranteed. Detecting modification in the collection and parsing the collection is not done synchronously. Modification on the collection may go unnoticed under certain circumstances. So while programming , this behaviour should not be banked upon. Example for fail fast iterators are ArrayList, Vector , HashSet.
    
    Fail safe Iterators :
    
    Point two is , not all collections are fail fast. Even if the underlying collection is modified , it does not fail by throwing ConcurrentModificationException. When an iterator is created, either it is directly created on the collection , or created on a clone of that collection. One example which suppports failsafe iterator is ConcurrenHashMap.
    
- What is the importance of hashCode() and equals() methods ?
    
    ![image.png](assets/image%204.png)
    
- What is the difference between HashSet and TreeSet ?
    
    ![image.png](assets/image%205.png)
    
- how to add custom sort in TreeSet ?
- What does System.gc() and Runtime.gc() methods do ?
    
    These methods can be used as a hint to the JVM, in order to start a garbage collection . However, this it is up to the Java Virtual Machine (JVM) to start the garbage collection immediately or later in time.
    
- When is the finalize() called ?
    
    finalize() method is called just before releasing the objecet memory.
    
- If an object reference is set to null , will the Garbage Collector immediately free the memory held by that object ?
    
    No, the object will be available for garbage collection in the next cycle of the garbage collector .
    
- What is the difference between Enumeration and Iterator ?
    1. Enumeration doesn't have a remove() method while Iterator has a remove() method
    2. Enumeration acts as Read-only interface, because it has the methods only to traverse and fetch the objects
    
- Diff b/w HashMap and Hashtable ?
    
    ![image.png](assets/image%206.png)
    
- What are the different Collection Views That Map provide ?
    1. Key Set() → allow a map’s contents to be viewed as a set of keys.
    2. Value Collection → allows a map’s contents to be viewed at a set of values.
    3. Entry Set()→ allow a map’s contents to be viewed as a set of key-value mappings.
- What’s difference between Stack and Queue ?
    
    Stack :→ it’s a non-primitive linear data structure in which insertion and deletion of elements takes place from only one end known as top.
    
    Queue :→ It is non - primitive linear data structure in which insertion and deletion of elements takes place from two opposite ends rear and front respectively.
    
- In a class implementing an interface, can we change the value of any variable defined in the interface ?
    
    No, we can’t change the value of any variable of an interface in the implementing class as all variables defined in the interface are by default public ,static and final and final variables are like constants which can’t be changed later.
    
- Why do we need wrapper classes ?
    
    It is sometimes easier to deal with primitives as objects . Moreover most of the collection classes store objects and not primitive data types. And also the wrapper classes provide many utility methods also.
    
- Describe, in general , how java’s garbage collector works ?
    
    It works on the principle of mark and sweep method.
    
- What is Serialization ? why do we need it ?
- What is Weakhashmap ?
    
    In HashMap even though after adding the values and later we initialized it null then it’s not garbage collected but in weakhashmap it is garbage collected suppose you have an Employee object “e1” and you are storing this as key after adding this in both HashMap and WeakHashMap after adding if you re-initialize e1=null then WeakhashMap will allow this to be garbage collected and the key will be removed but this in not in the case of HashMap
    
- If synchronized map is there why concurrent hash map was introduced ?
- How to make a list as Read Only List ?
    
    An arrayList can be made read-only easily with the help of Collections.unmodifiableList() method. This method takes the modifiable ArrayList as a parameter and returns the read-only unmodifiable view of the ArrayList.
    
    readOnlyArrayList=Collections.unmodifiableList(ArrayList);
    
- What is Java Reflection API ?
    
    At run time you have an object ,you wanna know details of the class (Blueprint of it ) Then use Reflection.
    
- What is the difference between Serializable and Externalizable Interfaces ?
    
    ![image.png](assets/image%207.png)
    
    ![image.png](assets/image%208.png)
    
- When can an object Reference  be cast to an interface reference ?
    
    If you implement an interface and provide body to its methods from a class. You can hold object of that class using the reference variable of the interface i.e. cast and object reference to an interface reference.
    
    Animal a = new Dog();
    
- How substring works in Java or sometimes asked as to how does substring creates memory leak in Java.
    
    If the original string is very long, and has array of size 1GB, no matter how small a substring is, it will hold 1GB array. this will also stop original string to be garbage collected, in case if doesn’t have any live reference. This is a clear case of memory leak in Java, where memory is retained even if it’s not required. That’s how substring method creates memory leak.
    
    ### to prevent this we use intern() method it will create a new substring in string pool.
    
    Java fixed this issue after java 1.7 now they create a new String() when you used substring()
    
- What are the different ways we can break a singleton pattern in Java ?
    - Reflection
    - Serialization
    - Cloning
    - By Executor service
- Difference between NoClassDefFoundError and ClassNotFoundException ?
- Difference between TreeSet and TreeMap in Java?
    
    Though both are sorted collection, TreeSet is essentially a Set
    data structure which doesn’t allow duplicate and TreeMap is an
    implementation of Map interface. In reality, TreeSet is
    implemented via a TreeMap, much like how HashSet is
    implemented using HashMap.
    

# Spring Boot

- **Spring Vs Spring boot**
    
    ### **Difference Between Spring and Spring Boot**
    
    | **Aspect** | **Spring** | **Spring Boot** |
    | --- | --- | --- |
    | **Definition** | A comprehensive framework for building enterprise-level Java applications. | A module of Spring that simplifies application development by providing auto-configuration and starters. |
    | **Configuration** | Requires extensive manual configuration using XML or Java-based configurations. | Provides auto-configuration, reducing the need for manual setup. |
    | **Ease of Use** | More complex due to verbose configurations. | Simplifies development with defaults, embedded servers, and starters. |
    | **Dependencies** | Dependencies must be added and managed manually. | Uses **Spring Boot Starters** to bundle dependencies for specific purposes. |
    | **Standalone Apps** | Requires external servers like Tomcat or Jetty to deploy applications. | Includes **embedded servers** (Tomcat, Jetty) for running standalone applications. |
    | **Main Class** | Doesn't provide a main method; applications are deployed as WAR files. | Provides a `main()` method to run applications directly using `SpringApplication.run()`. |
    | **Production Readiness** | Requires additional configuration and tools for monitoring, logging, etc. | Comes with built-in **Spring Boot Actuator** for monitoring and management. |
    | **Starter Templates** | Not available. | Provides **Spring Boot Starters** for various modules like Web, JPA, and Security. |
    | **Learning Curve** | Steeper learning curve due to complexity in configuration. | Easier to learn for beginners due to reduced boilerplate code. |
    | **Build Tools** | Compatible with Maven and Gradle but requires additional configuration. | Provides pre-configured Maven/Gradle plugins for building applications easily. |
    | **Microservices Support** | Supports microservices but requires custom setup. | Built with microservices architecture in mind; includes support for REST, monitoring, and distributed systems. |
    
    ---
    
    ### **When to Use Spring vs Spring Boot**
    
    - **Use Spring**: For complex enterprise applications requiring fine-grained control and custom setups.
    - **Use Spring Boot**: For fast development, standalone applications, microservices, and prototyping.
- Hibernate Isolation Levels
    
    **What are Isolation Levels?**
    
    Isolation levels define the visibility of changes made by one transaction to other concurrent transactions before the changes are committed. Different databases support different isolation levels.
    
    ---
    
    ## **Types of Isolation Levels in Hibernate**
    
    Hibernate uses the isolation levels defined by the **JDBC specification** and **underlying database**. These are:
    
    ### **1. Read Uncommitted** (`Connection.TRANSACTION_READ_UNCOMMITTED`)
    
    - The lowest isolation level.
    - Transactions can **read uncommitted** (dirty) data from other transactions.
    - Leads to **Dirty Reads, Non-Repeatable Reads, and Phantom Reads**.
    - **Use Case**: Suitable when performance is prioritized over accuracy, e.g., logging or analytics.
    
    ### **Example: Dirty Read Issue**
    
    - **Transaction 1**: Updates a row but does not commit.
    - **Transaction 2**: Reads the updated row.
    - **Transaction 1**: Rolls back the change.
    - **Transaction 2**: Now has incorrect data (dirty read).
    
    ---
    
    ### **2. Read Committed** (`Connection.TRANSACTION_READ_COMMITTED`)
    
    - Ensures transactions can only read **committed** data.
    - **Dirty Reads are prevented**, but **Non-Repeatable Reads and Phantom Reads** may still occur.
    - **Use Case**: Most common level in databases like **PostgreSQL and Oracle**.
    
    ### **Example: Non-Repeatable Read Issue**
    
    - **Transaction 1**: Reads a record.
    - **Transaction 2**: Updates and commits the record.
    - **Transaction 1**: Reads the same record again but gets a different value.
    
    ---
    
    ### **3. Repeatable Read** (`Connection.TRANSACTION_REPEATABLE_READ`)
    
    - Ensures transactions **always read the same data** if executed multiple times.
    - **Prevents Dirty Reads and Non-Repeatable Reads** but **not Phantom Reads**.
    - **Use Case**: E-commerce transactions where consistency is crucial (e.g., bank balances).
    
    ### **Example: Phantom Read Issue**
    
    - **Transaction 1**: Selects all employees with `salary > 50,000`.
    - **Transaction 2**: Inserts a new employee with `salary = 60,000` and commits.
    - **Transaction 1**: Runs the same query and sees one extra record (phantom read).
    
    ---
    
    ### **4. Serializable** (`Connection.TRANSACTION_SERIALIZABLE`)
    
    - The highest level of isolation.
    - Transactions are executed **one after another** (like serially).
    - **Prevents Dirty Reads, Non-Repeatable Reads, and Phantom Reads**.
    - **Use Case**: Financial applications where integrity is more important than performance.
        
        ## **Summary Table of Isolation Levels**
        
        | Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read | Performance |
        | --- | --- | --- | --- | --- |
        | Read Uncommitted | ✅ Yes | ✅ Yes | ✅ Yes | 🔥 Fastest |
        | Read Committed | ❌ No | ✅ Yes | ✅ Yes | ⚡ Fast |
        | Repeatable Read | ❌ No | ❌ No | ✅ Yes | ⚖️ Moderate |
        | Serializable | ❌ No | ❌ No | ❌ No | 🐢 Slowest |
    
- What is the difference between War and Jar file ? which one is best in spring boot while making the build ?
    
    ### 🔸 **What is a JAR File?**
    
    **JAR (Java ARchive)** is used to package **Java classes**, libraries, and resources into a **single file** to distribute Java applications.
    
    - **Used For**: Standalone Java applications.
    - **Structure**:
        
        ```
        
        MyApp.jar
        ├── META-INF/
        ├── com/example/...
        └── application.properties
        ```
        
    - **Execution**: You can run it directly using:
        
        ```
        java -jar MyApp.jar
        ```
        
    
    ---
    
    ### 🔸 **What is a WAR File?**
    
    **WAR (Web Application Archive)** is used to package **web applications** that run in a **servlet container** (like Tomcat, Jetty, etc.).
    
    - **Used For**: Web applications (usually deployed to an external app server).
    - **Structure**:
        
        ```
        MyApp.war
        ├── WEB-INF/
        │   ├── classes/
        │   └── lib/
        └── index.jsp
        ```
        
    - **Deployment**: Deployed to an external servlet container like:
        
        ```
        /tomcat/webapps/MyApp.war
        ```
        
    
    ---
    
    ### ✅ **Which One is Best for Spring Boot?**
    
    **JAR** is generally **preferred and recommended** in Spring Boot for the following reasons:
    
    ### ✅ Pros of JAR in Spring Boot:
    
    1. **Embedded Server**: Spring Boot includes an embedded server (like Tomcat/Jetty) → No need to deploy to external server.
    2. **Simplified Deployment**: Just run `java -jar yourapp.jar`.
    3. **Microservice Ready**: Perfect for containerization (Docker) and cloud deployment.
    4. **Production Friendly**: Easier CI/CD pipeline integration.
    
    ### 🔧 When to Use WAR:
    
    - If you're in a legacy environment **where you must deploy to an external app server** (e.g., shared Tomcat server).
    - Or if you are **integrating with existing enterprise systems** that require WAR format.
    
    ---
    
    ### 🔨 How to Build Each in Spring Boot:
    
    **JAR (default)** – in `pom.xml`:
    
    ```xml
    <packaging>jar</packaging>
    ```
    
    **WAR** – change packaging:
    
    ```xml
    <packaging>war</packaging>
    ```
    
    Also, you’ll need to:
    
    - Remove `SpringBootServletInitializer` auto-run from `main()`
    - Add `SpringBootServletInitializer` subclass to support WAR:
    
    ```java
    @SpringBootApplication
    public class MyApp extends SpringBootServletInitializer {
        @Override
        protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
            return builder.sources(MyApp.class);
        }
    
        public static void main(String[] args) {
            SpringApplication.run(MyApp.class, args);
        }
    }
    ```
    
    ---
    
    ### 🧠 Summary:
    
    | Feature | JAR | WAR |
    | --- | --- | --- |
    | Use Case | Standalone apps, microservices | Traditional web applications |
    | Server | Embedded (Tomcat/Jetty) | External server |
    | Deployment | `java -jar app.jar` | Deploy `.war` to web server |
    | Spring Boot | Default and recommended | Needs custom setup |
- Why choose Spring Boot over Spring ?
    1. Less Configuration
    2. Embedded Web Servers
        1. In traditional Spring, setting up a web applications requires you to configure an external web server like Tomcat or Jetty.
        2. With Spring Boot, embedded web servers come built-in . You can run your application with an embedded server like Tomcat or Jetty our of the box.
    3. Starter Dependencies
        1. Managing dependencies in traditional Spring can be overwhelming, as you have to manually include each library and it’s transitive dependencies. Hard to manage the compatibility between libraries and versions.
        2. Spring Boot introduces starter dependencies, which are predefined sets of libraries that are commonly used together. Automatically manages the compatibility between libraries and versions.
    
    4. Rapid Development
    
    1. When you’re developing with spring, setting up a new project can take time due to the amount of configuration involved.
    2. Spring boot speeds up the development process significantly. With tools like Spring Initializr, you can generate a new Spring Boot project in minutes, complete with your preferred dependencies.
    
    5. Production - Ready Features
    
    Spring boot includes production-ready features out of the box: Acutator, Logging , Security
    
- What is Circular Dependency ? How will you resolve it ?
    
    A **circular dependency** occurs when **two or more beans depend on each other**, creating a loop that **prevents the Spring container from resolving the dependencies**.
    
    ---
    
    ### **🔹 How Does It Occur? (Example)**
    
    ```java
    import org.springframework.stereotype.Component;
    import org.springframework.beans.factory.annotation.Autowired;
    
    @Component
    class A {
        @Autowired
        private B b;
    }
    
    @Component
    class B {
        @Autowired
        private A a;
    }
    
    ```
    
    🔴 **Issue:**
    
    - **`A` depends on `B`** (`@Autowired private B b;`).
    - **`B` depends on `A`** (`@Autowired private A a;`).
    - Spring cannot resolve which bean should be created first.
    
    🛑 **Error During Startup:**
    
    ```
    org.springframework.beans.factory.BeanCurrentlyInCreationException:
    Error creating bean with name 'A': Requested bean is currently in creation.
    ```
    
    ---
    
    ## **✅ How to Resolve Circular Dependency?**
    
    ### **1️⃣ Use `@Lazy` to Break the Circular Reference**
    
    ```java
    @Component
    class A {
        @Autowired
        @Lazy  // Inject B lazily to avoid circular dependency
        private B b;
    }
    
    @Component
    class B {
        @Autowired
        private A a;
    }
    ```
    
    ✅ **How It Works?**
    
    - `@Lazy` tells Spring to **delay creating `B` until needed**, breaking the cycle.
    
    ---
    
    ### **2️⃣ Use Constructor Injection with `@Lazy`**
    
    ```java
    @Component
    class A {
        private final B b;
    
        @Autowired
        public A(@Lazy B b) { // Lazy-loaded dependency
            this.b = b;
        }
    }
    
    @Component
    class B {
        private final A a;
    
        @Autowired
        public B(A a) {
            this.a = a;
        }
    }
    ```
    
    ✅ **Why?**
    
    - **Field injection (`@Autowired private X x;`) is not recommended** in general.
    - Constructor injection **forces proper dependency management**.
    
    ---
    
    ### **3️⃣ Use `@PostConstruct` for Late Dependency Injection**
    
    ```java
    @Component
    class A {
        private B b;
    
        @Autowired
        public A() {}
    
        @Autowired
        public void setB(B b) { // Inject after object creation
            this.b = b;
        }
    }
    
    @Component
    class B {
        private final A a;
    
        @Autowired
        public B(A a) {
            this.a = a;
        }
    }
    ```
    
    ✅ **Why?**
    
    - Setter injection allows **Spring to create objects first, then inject dependencies**.
    
    ---
    
    ### **4️⃣ Restructure Code (Decouple Dependencies)**
    
    If **circular dependency** exists, it usually means **poor design**.
    
    🔹 **Solution:** Introduce a **third class (`Service`)** to handle dependencies.
    
    ```java
    
    @Component
    class A {
        private final Service service;
    
        @Autowired
        public A(Service service) {
            this.service = service;
        }
    }
    
    @Component
    class B {
        private final Service service;
    
        @Autowired
        public B(Service service) {
            this.service = service;
        }
    }
    
    @Component
    class Service {
        // Common logic that A and B need
    }
    ```
    
    ✅ **Why?**
    
    - Removes **direct dependency** between `A` and `B`.
    - Improves **scalability and maintainability**.
    
    ---
    
    ## **🚀 Summary**
    
    | Solution | Approach | Best For |
    | --- | --- | --- |
    | `@Lazy` | **Delays bean creation** | Quick fix but not ideal |
    | Constructor Injection | **Breaks direct dependency** | Best practice |
    | Setter Injection | **Injects later using `@PostConstruct`** | When restructuring isn’t possible |
    | Extract Service | **Refactors logic into a separate class** | Best for clean architecture |
- Can Spring boot application run without the **@SpringBootApplication** annotation ?
    
    Yes we can start without @SpringBootApplication as well but need to add other annotations in the class.
    
    ```java
    @Target(ElementType.TYPE)
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @Inherited
    @SpringBootConfiguration
    @EnableAutoConfiguration
    @ComponentScan
    public @interface SpringBootApplication {
    }
    ```
    
- What is Dispatcher servlet ?
    
    The **`DispatcherServlet`** is the **front controller** in Spring MVC that handles **all incoming HTTP requests**. It acts as the central point for request processing and delegates requests to appropriate controllers.
    
    ---
    
    ## **🔹 How `DispatcherServlet` Works?**
    
    1. **Receives HTTP Requests**
        - It intercepts all incoming requests (e.g., `GET`, `POST`).
    2. **Delegates to Handlers (Controllers)**
        - It forwards requests to the correct **Controller** based on URL mappings.
    3. **Processes Business Logic & Data**
        - Calls services, repositories, and processes data.
    4. **Chooses View Resolver**
        - Determines the correct view (HTML, JSON, etc.) for response.
    5. **Returns Response**
        - Sends the processed response back to the client.
    
    ---
    
    ## **🔹 How `DispatcherServlet` is Configured in Spring Boot?**
    
    In **Spring Boot**, `DispatcherServlet` is **automatically configured** via **Spring Boot’s auto-configuration**.
    
    By default, it maps all requests to `/` and handles them.
    
    **🚀 You don’t need to manually configure it!**
    
    ---
    
    ## **🔹 Example: Handling Requests with `DispatcherServlet`**
    
    ```java
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    
    @RestController
    @RequestMapping("/api")
    public class MyController {
    
        @GetMapping("/hello")
        public String sayHello() {
            return "Hello from DispatcherServlet!";
        }
    }
    
    ```
    
    🔹 **When you hit:** `http://localhost:8080/api/hello`
    
    🔹 **DispatcherServlet will:**
    
    - Intercept the request.
    - Find the `@GetMapping("/hello")` handler method.
    - Execute it and return `"Hello from DispatcherServlet!"`.
    
    ---
    
    ## **🔹 Customizing `DispatcherServlet` Mapping**
    
    By default, `DispatcherServlet` is mapped to `/`. You can change it in `application.properties`:
    
    ```
    spring.mvc.servlet.path=/myapp
    ```
    
    Now, all requests start with `/myapp`.
    
    e.g., `http://localhost:8080/myapp/api/hello`
    
    ---
    
    ## **🚀 Summary**
    
    - **`DispatcherServlet` is the Front Controller in Spring MVC**.
    - **Automatically configured in Spring Boot** (No XML needed).
    - **Intercepts requests, calls controllers, and returns responses**.
    - **Customizable via `application.properties`**.
- Explain Spring MVC architecture .
    
    [Client]  ---> (1) HTTP Request ---> [DispatcherServlet]
    |
    |---> (2) Controller
    |       |
    |       |---> (3) Service Layer
    |                |
    |                |---> (4) Repository (DB)
    |
    |---> (5) View Resolver
    |       |
    |       |---> (6) View (JSP/HTML)
    |
    [Client]  <--- (7) HTTP Response <--- [DispatcherServlet]
    
- Explain Rest Principles
    
    ## 🔹 What is REST?
    
    **REST (Representational State Transfer)** is an architectural style for designing networked applications. It is widely used for building **web services** that are **lightweight, scalable, and stateless**.
    
    ---
    
    ## 🔹 Core REST Principles (aka REST Constraints)
    
    To be considered truly RESTful, an API must follow **six architectural constraints**:
    
    ---
    
    ### 1. **Client-Server Architecture**
    
    - Separation of **client** and **server** responsibilities.
    - Client handles **UI and user experience**, while the server manages **data and business logic**.
    
    ✅ **Benefits**: Independent development, scalability.
    
    ---
    
    ### 2. **Statelessness**
    
    - Each **HTTP request** from the client must contain all the information needed to understand and process the request.
    - **No session or user context is stored on the server**.
    
    ✅ **Benefits**: Easier scaling, reduced server memory usage.
    
    ---
    
    ### 3. **Cacheability**
    
    - Responses must define whether they are **cacheable** or not (using HTTP headers like `Cache-Control`).
    - Helps improve performance by avoiding redundant processing.
    
    ✅ **Benefits**: Reduced server load, faster client response.
    
    ---
    
    ### 4. **Uniform Interface**
    
    - A consistent and standard way of interacting with the server using **HTTP methods**.
    
    ### Key elements of uniform interface:
    
    | HTTP Method | Operation |
    | --- | --- |
    | `GET` | Read resource |
    | `POST` | Create resource |
    | `PUT` | Update resource |
    | `DELETE` | Remove resource |
    
    ✅ **Benefits**: Simplicity, discoverability.
    
    ---
    
    ### 5. **Layered System**
    
    - REST allows for a layered system architecture, where a client can interact with an intermediary (e.g., load balancer, proxy) instead of directly talking to the server.
    
    ✅ **Benefits**: Scalability, security, modularity.
    
    ---
    
    ### 6. **Code on Demand (Optional)**
    
    - Servers can optionally send **executable code** (like JavaScript) to the client.
    
    ✅ **Benefits**: Client-side extension without modification.
    
    ---
    
    ## 🔸 Real-World REST API Example
    
    Let’s say we’re building a RESTful service for a **Book Store**:
    
    | Action | HTTP Method | Endpoint |
    | --- | --- | --- |
    | Get all books | `GET` | `/books` |
    | Get a book by ID | `GET` | `/books/{id}` |
    | Create a new book | `POST` | `/books` |
    | Update a book | `PUT` | `/books/{id}` |
    | Delete a book | `DELETE` | `/books/{id}` |
    
    ---
    
    ## 🔚 Summary
    
    | Principle | Description |
    | --- | --- |
    | Client-Server | Separation of concerns |
    | Stateless | No server-side session |
    | Cacheable | Responses should indicate cacheability |
    | Uniform Interface | Standard HTTP methods and resource structure |
    | Layered System | Middleware support |
    | Code on Demand | Optional: send code to client |
- What are the best pratices for developing api ?
    
    ## ✅ **Best Practices for API Development**
    
    ---
    
    ### 🔹 1. **Use Proper HTTP Methods**
    
    | Operation | Method |
    | --- | --- |
    | Retrieve data | `GET` |
    | Create data | `POST` |
    | Update data | `PUT` or `PATCH` |
    | Delete data | `DELETE` |
    
    👉 Stick to REST semantics so your APIs are intuitive and standard.
    
    ---
    
    ### 🔹 2. **Use Meaningful, Consistent URIs**
    
    Bad ❌: `/getUserData?id=5`
    
    Good ✅: `/users/5`
    
    - Use **nouns** not verbs.
    - Use **plural names** (`/products`, `/users`).
    - Nest resources logically: `/users/5/orders`.
    
    ---
    
    ### 🔹 3. **Return Proper HTTP Status Codes**
    
    | Code | Meaning |
    | --- | --- |
    | 200 | OK |
    | 201 | Created (after POST) |
    | 204 | No Content (DELETE) |
    | 400 | Bad Request |
    | 401 | Unauthorized |
    | 404 | Not Found |
    | 500 | Internal Server Error |
    
    Make your responses **meaningful and standardized**.
    
    ---
    
    ### 🔹 4. **Use Standard Response Structure**
    
    Design a consistent response format for success & error.
    
    **Example – Success:**
    
    ```json
    {
      "timestamp": "2025-04-19T15:30:00Z",
      "status": 200,
      "data": {
        "id": 1,
        "name": "John"
      },
      "message": "User fetched successfully"
    }
    ```
    
    **Example – Error:**
    
    ```json
    {
      "timestamp": "2025-04-19T15:30:00Z",
      "status": 404,
      "error": "User Not Found",
      "path": "/users/99"
    }
    ```
    
    Use a **GlobalExceptionHandler** to standardize errors (you already asked about this 😉).
    
    ---
    
    ### 🔹 5. **Validate Request Data**
    
    Use **Java Bean Validation** (`@Valid`, `@NotNull`, etc.) to validate input.
    
    ```java
    public class UserDTO {
        @NotBlank
        private String name;
    
        @Email
        private String email;
    }
    ```
    
    And annotate controller methods:
    
    ```java
    @PostMapping("/users")
    public ResponseEntity<?> createUser(@RequestBody @Valid UserDTO userDTO) { ... }
    ```
    
    ---
    
    ### 🔹 6. **Secure Your API**
    
    - Use **HTTPS** always.
    - Add **authentication** (JWT, OAuth2, etc.).
    - Add **authorization** based on roles.
    - Prevent common attacks (e.g., SQL Injection, CSRF, XSS).
    
    ---
    
    ### 🔹 7. **Pagination & Filtering**
    
    For large data sets, never return everything at once.
    
    ```
    GET /users?page=1&size=10&sort=name
    ```
    
    Spring Data makes this super easy with `Pageable`.
    
    ---
    
    ### 🔹 8. **Version Your API**
    
    Use versioning to avoid breaking changes:
    
    ```
    GET /api/v1/users
    ```
    
    You can version via:
    
    - URI: `/v1/resource`
    - Header: `Accept: application/vnd.myapi.v1+json`
    
    ---
    
    ### 🔹 9. **Use Proper Logging and Monitoring**
    
    - Log requests, responses (without sensitive data).
    - Track errors and performance (e.g., with **Actuator**, **Micrometer**, or tools like **Prometheus**, **Grafana**, **ELK stack**).
    
    ---
    
    ### 🔹 10. **Use Swagger/OpenAPI**
    
    Generate API documentation using:
    
    - **SpringDoc OpenAPI**
    - **Swagger UI**
    
    E.g., with Spring Boot + Maven:
    
    ```xml
    
    <dependency>
      <groupId>org.springdoc</groupId>
      <artifactId>springdoc-openapi-ui</artifactId>
      <version>1.7.0</version>
    </dependency>
    ```
    
    Then access docs at `/swagger-ui.html`.
    
    ---
    
    ## 💡 Bonus Tips
    
    - ❌ Don’t return raw exceptions or stack traces.
    - ✅ Use DTOs instead of exposing JPA entities directly.
    - ✅ Write unit & integration tests for your APIs.
    - ✅ Maintain good API documentation (Swagger, Postman collection, or markdown).
- Is Rest always stateless ? Can a REST API maintain a session ?
    
    ## 🤔 Is REST always stateless?
    
    **Yes** – by *definition*, **REST is stateless**.
    
    ### 📘 According to REST principles (as defined by Roy Fielding):
    
    > "Each request from client to server must contain all the information the server needs to understand and fulfill the request."
    > 
    
    This means:
    
    - The server **doesn’t store client context between requests**.
    - Every request is **independent** — no memory of previous interactions.
    
    ---
    
    ## 🤯 So… can a REST API maintain a session?
    
    Here’s the interesting part:
    
    > ❗️You can technically use sessions or tokens with REST, but it breaks or bends the "pure" statelessness rule.
    > 
    
    ### 💡 Let's break it down:
    
    ---
    
    ## ✅ Ways to "Maintain Session-Like State" with REST APIs
    
    ### 1. **Token-based authentication (Recommended)**
    
    Use a token like **JWT (JSON Web Token)**:
    
    - The client sends a token with each request (usually in `Authorization` header).
    - The server verifies it but **doesn’t store session data** → **still stateless!**
    
    ```
    http
    CopyEdit
    GET /profile
    Authorization: Bearer eyJhbGciOiJIUzI1NiIsIn...
    
    ```
    
    ✅ Still follows REST principles.
    
    ✅ Scales well (no session memory needed on server).
    
    ---
    
    ### 2. **Session-based authentication (Not RESTful)**
    
    Store session ID in a **cookie** (like traditional web apps):
    
    - The server **maintains session data** on backend (e.g., in-memory, Redis).
    - Client sends session ID with each request via cookie.
    
    ```
    http
    CopyEdit
    Cookie: JSESSIONID=abc123
    
    ```
    
    ❌ This **breaks statelessness**.
    
    ❌ Doesn’t scale well without sticky sessions or shared storage.
    
    ---
    
    ## ⚖️ RESTful vs Practical
    
    | Feature | RESTful Style | Traditional Sessions |
    | --- | --- | --- |
    | Stateless | ✅ Yes | ❌ No |
    | Stores server-side state | ❌ No | ✅ Yes |
    | Scalable | ✅ Highly | ❌ Depends |
    | Common Use | ✅ APIs (JWT) | ✅ Web apps |
    
    ---
    
    ## 🧠 TL;DR
    
    - ✅ **REST is stateless by design**.
    - ❌ **You *can* maintain sessions**, but it **breaks REST principles**.
    - ✅ Use **token-based auth (like JWT)** to preserve statelessness **while managing authentication**.
    - ⚠️ Session-based auth is fine for traditional web apps, but it’s not RESTful.
- What is first level caching and second level caching in spring boot ?
    
    ## 🔄 What is Caching in Hibernate?
    
    Hibernate (which Spring Data JPA uses under the hood) supports two types of caching:
    
    | Cache Level | Scope | Default? | Description |
    | --- | --- | --- | --- |
    | **First-Level** | Per Session | ✅ Yes | Always enabled, specific to a single Hibernate session (`EntityManager`). |
    | **Second-Level** | Across Sessions | ❌ No (you enable it manually) | Shared across sessions, improves performance by avoiding DB hits for repeated queries. |
    
    ---
    
    ## ✅ 1. First-Level Cache (Default)
    
    ### 📌 Description:
    
    - It is **enabled by default**.
    - Every `EntityManager` (or Hibernate `Session`) has its own cache.
    - If you fetch the same entity twice in the same session, the second time it comes from the cache.
    
    ### 🔍 Example:
    
    ```java
    @Autowired
    private EntityManager entityManager;
    
    public void demoFirstLevelCache() {
        // Start transaction
        User user1 = entityManager.find(User.class, 1L); // Hits DB
        User user2 = entityManager.find(User.class, 1L); // Comes from first-level cache
    
        System.out.println(user1 == user2); // true, same instance
        // End transaction
    }
    ```
    
    ---
    
    ## ✅ 2. Second-Level Cache (Manual Setup)
    
    ### 📌 Description:
    
    - **Optional and must be configured**.
    - Works across sessions (shared cache).
    - Useful when the same data is fetched often, e.g., `User`, `Country`, `Product`, etc.
    
    ### 📦 Libraries: You need a cache provider like:
    
    - **EhCache**
    - **Hazelcast**
    - **Infinispan**
    - **Caffeine**
    
    ---
    
    ### 🚀 Step-by-step Setup Example with **EhCache**
    
    ### 1. **Add Maven Dependency**
    
    ```xml
    <!-- Add to your pom.xml -->
    <dependency>
        <groupId>org.ehcache</groupId>
        <artifactId>ehcache</artifactId>
    </dependency>
    ```
    
    ---
    
    ### 2. **Enable Second-Level Cache in `application.properties`**
    
    ```
    # Enable second-level cache
    spring.jpa.properties.hibernate.cache.use_second_level_cache=true
    spring.jpa.properties.hibernate.cache.region.factory_class=org.hibernate.cache.jcache.JCacheRegionFactory
    spring.cache.jcache.config=classpath:ehcache.xml
    ```
    
    ---
    
    ### 3. **Configure `ehcache.xml`**
    
    Place this file in `src/main/resources/ehcache.xml`:
    
    ```xml
    <configxmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'
            xmlns='http://www.ehcache.org/v3'
            xsi:schemaLocation="http://www.ehcache.org/v3 http://www.ehcache.org/schema/ehcache-core.xsd">
    
        <cache alias="com.example.User">
            <heap unit="entries">100</heap>
            <expiry>
                <ttl unit="minutes">10</ttl>
            </expiry>
        </cache>
    
    </config>
    ```
    
    ---
    
    ### 4. **Annotate Your Entity**
    
    ```java
    import jakarta.persistence.Cacheable;
    import org.hibernate.annotations.Cache;
    import org.hibernate.annotations.CacheConcurrencyStrategy;
    
    @Entity
    @Cacheable
    @Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
    public class User {
        @Id
        private Long id;
    
        private String name;
    }
    
    ```
    
    ---
    
    ### 🧪 How it Works
    
    ```java
    // First session
    User u1 = entityManager.find(User.class, 1L); // Hits DB and puts in 2nd-level cache
    
    // Close and open new session
    User u2 = entityManager.find(User.class, 1L); // Comes from 2nd-level cache
    ```
    
    ✅ Even though it's a different session, it doesn't hit the DB again!
    
- What happens if a circuit breaker remains open for too long ?
- What is callback function ? can callback cause circular dependency.
    
    yes
    
- **@RequestParams @PathVariable @RequestBody**
    
    ### **Comparison**
    
    account?dob=”13”
    
    | Annotation | Use Case | Source of Data | Example URL/Body |
    | --- | --- | --- | --- |
    | **`@RequestParam`** | Query parameters or form data | URL query string | `/greet?name=John&age=30` |
    | **`@PathVariable`** | Dynamic parts of the URL path | URL path | `/user/123` |
    | **`@RequestBody`** | Entire request body | JSON/XML data | `{ "name": "John", "age": 30 }` |
    
    ---
    
    ### **When to Use?**
    
    1. Use **`@RequestParam`** when you want to capture optional or multiple query parameters.
    2. Use **`@PathVariable`** when the value is part of the URL path (e.g., `/user/{id}`).
    3. Use **`@RequestBody`** when you need to handle structured data (e.g., JSON payload).
- **`@Webfilter and @ServletComponentScan`. diff `GlobalFilter`**
    
    ### **Comparison: `@WebFilter` + `@ServletComponentScan` vs `GlobalFilter`**
    
    ### **1. `@WebFilter` + `@ServletComponentScan`**
    
    - **Context**: Used in **Spring MVC** or traditional servlet-based applications.
    - **Purpose**: To define filters for intercepting requests and responses in a **blocking (synchronous)** way.
    - **How It Works**:
        - `@WebFilter`: Annotates a filter class to handle specific URL patterns or servlets.
        - `@ServletComponentScan`: Enables scanning for servlet-related components like `@WebFilter`, `@WebServlet`, and `@WebListener`.
    - **Execution Model**:
        - Filters run in a **synchronous**, servlet-based model.
        - Filters handle HTTP requests and responses using `HttpServletRequest` and `HttpServletResponse`.
    
    **Example**:
    
    ```java
    @WebFilter(urlPatterns = "/*")
    public class MyWebFilter implements Filter {
        @Override
        public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
                throws IOException, ServletException {
            // Pre-processing logic
            System.out.println("Filter executed before controller");
            chain.doFilter(request, response);
            // Post-processing logic
            System.out.println("Filter executed after controller");
        }
    }
    
    @SpringBootApplication
    @ServletComponentScan
    public class MyApplication {
        public static void main(String[] args) {
            SpringApplication.run(MyApplication.class, args);
        }
    }
    
    ```
    
    ---
    
    ### **2. `GlobalFilter`**
    
    - **Context**: Used in **Spring WebFlux**, which is designed for **reactive (non-blocking)** applications.
    - **Purpose**: To define global filters for handling requests and responses in a **non-blocking, asynchronous** way.
    - **How It Works**:
        - Filters handle reactive request and response streams using `ServerWebExchange`.
        - Designed to work with the WebFlux framework and the reactive programming model.
    - **Execution Model**:
        - Filters process requests/reactive streams asynchronously, making them suitable for high-throughput, event-driven applications.
    
    **Example**:
    
    ```java
    @Component
    public class MyGlobalFilter implements GlobalFilter {
        @Override
        public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
            // Pre-processing logic
            System.out.println("Reactive filter executed before handler");
            return chain.filter(exchange)
                    .then(Mono.fromRunnable(() -> {
                        // Post-processing logic
                        System.out.println("Reactive filter executed after handler");
                    }));
        }
    }
    
    ```
    
    ---
    
    ### **Key Differences**
    
    | **Aspect** | **`@WebFilter` + `@ServletComponentScan`** | **`GlobalFilter`** |
    | --- | --- | --- |
    | **Framework** | Spring MVC (Servlet-based) | Spring WebFlux (Reactive-based) |
    | **Programming Model** | Blocking, synchronous | Non-blocking, asynchronous |
    | **Annotation** | `@WebFilter` (requires `@ServletComponentScan`) | `@Component` implementing `GlobalFilter` |
    | **Request/Response API** | Uses `HttpServletRequest` and `HttpServletResponse` | Uses `ServerWebExchange` |
    | **Application Type** | Traditional, servlet-based apps | Reactive, event-driven apps |
    | **Performance** | Limited by the servlet thread model | Optimized for high-throughput, non-blocking scenarios |
    | **Use Case** | Apps with synchronous workflows, simpler logic | Apps requiring scalability and reactive streams |
    
    ---
    
    ### **When to Use Each**:
    
    - **`@WebFilter` + `@ServletComponentScan`**:
        - Use in traditional servlet-based applications built with **Spring MVC**.
        - Suitable for apps that do not require the reactive model.
    - **`GlobalFilter`**:
        - Use in **Spring WebFlux** for reactive applications.
        - Ideal for modern, high-concurrency applications requiring scalability and event-driven architectures.
- **Full Flow of a 1st /login request of the page**
    
    ```
    API Gateway → Authorization Server → API Gateway → User
    ```
    
    Your Payload:
    
    ```yaml
    POST http://api-gateway:8110/login -> user-auth
    Headers:
      Content-Type: application/x-www-form-urlencoded
      Authorization: Basic <Base64(clientId:clientSecret)>
    Body:
      grant_type=password
      username=user
      password=password
      
      # this is this request :::
      curl -X POST http://localhost:8110/login \
      -H "Authorization: Basic Y2xpZW50OnNlY3JldA==" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -d "grant_type=password&username=user&password=password"
    ```
    
    ## **1. Registering Microservices with Eureka**
    
    Eureka is a service registry that helps microservices discover each other. Each microservice (e.g., Overdraft and Uninvested) must register itself with the Eureka server.
    
    ### **Setup Eureka Server**
    
    **Eureka Server Code (Port 8100):**
    
    ```java
    @SpringBootApplication
    @EnableEurekaServer
    public class EurekaServerApplication {
        public static void main(String[] args) {
            SpringApplication.run(EurekaServerApplication.class, args);
        }
    }
    ```
    
    **Eureka Server `application.yml`:**
    
    ```yaml
    server:
      port: 8100
    
    eureka:
      client:
        register-with-eureka: false
        fetch-registry: false
      server:
        enable-self-preservation: false
    
    ```
    
    - **`@EnableEurekaServer`**: Enables Eureka server functionality.
    - **`register-with-eureka` & `fetch-registry`**: The server doesn’t register itself or fetch its own registry.
    
    ### **Setup Overdraft Microservice (Port 8080):**
    
    **Overdraft Code:**
    
    ```java
    @SpringBootApplication
    @EnableEurekaClient // not complusory
    public class OverdraftServiceApplication {
        public static void main(String[] args) {
            SpringApplication.run(OverdraftServiceApplication.class, args);
        }
    }
    
    @RestController
    @RequestMapping("/overdraft")
    public class OverdraftController {
        @GetMapping("/fetchOverdraft")
        public String fetchOverdraft() {
            return "Overdraft details";
        }
    }
    ```
    
    **Overdraft `application.yml`:**
    
    ```yaml
    server:
      port: 8080
    
    spring:
      application:
        name: overdraft-service
    
    eureka:
      client:
        service-url:
          defaultZone: http://localhost:8100/eureka/
    ```
    
    - **`@EnableEurekaClient`**: Registers the service with Eureka.
    - **`spring.application.name`**: The name under which this service is registered.
    - **`eureka.client.service-url.defaultZone`**: Points to the Eureka server.
    
    ### **Setup Uninvested Microservice (Port 8090):**
    
    Follow the same structure as Overdraft. Just update the `application.yml`:
    
    ```yaml
    server:
      port: 8090
    
    spring:
      application:
        name: uninvested-service
    
    eureka:
      client:
        service-url:
          defaultZone: http://localhost:8100/eureka/
    
    ```
    
    ---
    
    ## **2. API Gateway as Entry Point**
    
    The API Gateway acts as a single entry point. It redirects requests to respective services using routes.
    
    ### **Setup API Gateway (Port 8110):**
    
    **API Gateway Code:**
    
    **Given 2 ways to redirect or reroute the API -(/login) hitting the gateway to the user-auth microservice, using YAML and using RouteLocator.**
    
    ```java
    @SpringBootApplication
    @EnableEurekaClient // not complusory
    @EnableDiscoveryClient
    public class ApiGatewayApplication {
        public static void main(String[] args) {
            SpringApplication.run(ApiGatewayApplication.class, args);
        }
    
        @Bean
        public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
            return builder.routes()        
                    .route("login-route", r -> r.path("/login")
                        .uri("http://localhost:8120/oauth/token")) // Redirect to Authorization Server
                        // or lb://user-auth
                .route("overdraft_route", r -> r.path("/overdraft/**")
                    .uri("lb://overdraft-service"))
                .route("uninvested_route", r -> r.path("/uninvested/**")
                    .uri("lb://uninvested-service"))
                .build();
        }
    }
    
    ```
    
    **API Gateway `application.yml`:**
    
    ```yaml
    server:
      port: 8110
    
    spring:
      application:
        name: api-gateway
    
    eureka:
      client:
        service-url:
          defaultZone: http://localhost:8100/eureka/
    
    spring:
      cloud:
        gateway:
          routes:
            - id: login-route
              uri: http://localhost:8120/oauth/token # Authorization Server's token endpoint
              predicates:
                - Path=/login
            - id: overdraft
              uri: lb://overdraft-service
              predicates:
                - Path=/overdraft/**
            - id: uninvested
              uri: lb://uninvested-service
              predicates:
                - Path=/uninvested/**
    
    ```
    
    ### Explanation:
    
    1. **`lb://`**: Load balancing for the microservice via Eureka.
    2. **Predicates**: Define conditions for routing (e.g., paths).
    3. **RouteLocator Bean**: Allows Java-based route definitions.
    
    ### **Custom Filters in Gateway:**
    
    Filters intercept requests and can modify them before or after routing.
    
    **Example Custom Filter:**
    
    ### **This filter is for the request which are not /login because here we are validating the token.**
    
    ### **1. Will `/login` Hit `TokenValidationFilter` in the API Gateway?**
    
    No, the `/login` request **will not go through the `TokenValidationFilter`** in this case because `/login` is a public endpoint meant to authenticate the user and fetch tokens. Filters like `TokenValidationFilter` are designed to validate access tokens for **protected endpoints**, such as `/overdraft/fetchOverdraft`.
    
    If `/login` needs to bypass token validation, we configure the Gateway to exclude it from the filter. For example, in the Gateway configuration, you can define routes or use conditions to skip token validation for specific paths like `/login`.
    
    **How to Skip the Filter for `/login`:**
    
    ```java
    @Component
    public class TokenValidationFilter implements GatewayFilter, Ordered {
    
        @Override
        public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
            String path = exchange.getRequest().getPath().toString();
    
            // Skip filter for login endpoint
            if (path.equals("/login")) {
                return chain.filter(exchange); // Bypass token validation
            }
    
            // Token validation logic for protected endpoints
            String token = extractToken(exchange.getRequest().getHeaders().getFirst("Authorization"));
            if (isTokenValid(token)) {
                return chain.filter(exchange); // Forward the request
            } else {
                exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
                return exchange.getResponse().setComplete(); // Reject the request
            }
        }
    
        @Override
        public int getOrder() {
            return -1; // Priority of the filter
        }
    
        private boolean isTokenValid(String token) {
            // Call the Authorization Server's /check_token endpoint for validation
            return true; // Simplified for explanation
        }
    
        private String extractToken(String authHeader) {
            return authHeader != null && authHeader.startsWith("Bearer ")
                ? authHeader.substring(7)
                : null;
        }
    }
    
    ```
    
    ### **Filter For validating the /login request**
    
    ```java
    @Component
    public class LoginRequestFilter implements GatewayFilter, Ordered {
    
        @Override
        public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
            String path = exchange.getRequest().getPath().toString();
    
            if (path.equals("/login")) {
                // Log or process login-specific logic
                System.out.println("Login request received");
            }
    
            return chain.filter(exchange);
        }
    
        @Override
        public int getOrder() {
            return 0; // Higher priority than TokenValidationFilter
        }
    }
    
    ```
    
    ---
    
    ## **3. UserAuth Service Configuration**
    
    Basic Arrow Flow 
    
    ```
    Request → WebSecurityConfigurerAdapter (SecurityConfig)
            → configure(HttpSecurity) (Permits /oauth/token)
            → AuthorizationServerConfigurerAdapter (AuthorizationServerConfig)
            → configure(ClientDetailsServiceConfigurer) (Validates client_id and client_secret)
            → configure(AuthorizationServerEndpointsConfigurer) (Validates username and password via AuthenticationManager)
            → Token Generated → Response
    
    ```
    
    ### **User Auth Service Basic Configuration:**
    
    The User Auth service (Port 8120) authenticates users and issues tokens.
    
    **User Auth Code:**
    
    **`@EnableAuthorizationServer`**: Configures this application as an **OAuth 2.0 Authorization Server**. It will handle token issuance and validation.
    
    ```java
    @SpringBootApplication
    @EnableEurekaClient // not complusory
    @EnableAuthorizationServer
    public class UserAuthServiceApplication {
        public static void main(String[] args) {
            SpringApplication.run(UserAuthServiceApplication.class, args);
        }
    }
    ```
    
    Yaml Configuration
    
    ```yaml
    server:
      port: 8120
    
    spring:
      application:
        name: user-auth-service
    
    eureka:
      client:
        service-url:
          defaultZone: http://localhost:8100/eureka/
    
    ```
    
    ### **Authorization Server Configuration:**
    
    - **`@EnableAuthorizationServer`**: Enables this application as an OAuth 2.0 Authorization Server.
    - **`AuthorizationServerConfigurerAdapter`**: A helper class that allows you to override specific methods to configure the Authorization Server.
    
    ```java
    @Configuration
    @EnableAuthorizationServer
    public class AuthorizationServerConfig extends AuthorizationServerConfigurerAdapter {
    
        @Override
        public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
            clients.inMemory()
                .withClient("client")
                .secret("{noop}secret")
                .authorizedGrantTypes("password", "refresh_token") // Supports refresh tokens
                .scopes("read", "write")
                .accessTokenValiditySeconds(900) // 15 minutes
                .refreshTokenValiditySeconds(2592000); // 30 days
        }
    
        @Override
        public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
            endpoints.authenticationManager(authenticationManager);
        }
    
        @Autowired
        private AuthenticationManager authenticationManager;
    }
    
    ```
    
    ### **`configure(ClientDetailsServiceConfigurer)` (Client Validation)**
    
    This method defines the **clients** that are allowed to request tokens
    
    **Purpose**:
    
    - Validates `client_id` and `client_secret` when `/oauth/token` is hit.
    
    ### **Security Configuration Class:**
    
    - **`@EnableWebSecurity`**: Enables web security, allowing us to customize authentication and access control.
    - **`WebSecurityConfigurerAdapter`**: A base class to customize Spring Security’s behavior.
    
    ```java
    @Configuration
    @EnableWebSecurity
    public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
        @Override
        protected void configure(AuthenticationManagerBuilder auth) throws Exception {
            auth.inMemoryAuthentication()
                .withUser("user").password("{noop}password").roles("USER"); // In-memory user
        }
    
        @Bean
        @Override
        public AuthenticationManager authenticationManagerBean() throws Exception {
            return super.authenticationManagerBean(); // Exposes AuthenticationManager as a bean
        }
    
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeRequests()
                .antMatchers("/oauth/token").permitAll() // Allow access to token endpoint
                .anyRequest().authenticated()
                .and()
                .csrf().disable(); // Disable CSRF for simplicity
        }
    }
    
    ```
    
    ```java
    protected void configure(AuthenticationManagerBuilder auth) 
    //Explanation below
    ```
    
    ### **`configure(AuthenticationManagerBuilder)` (User Validation)**
    
    This method defines how **user authentication** is performed:
    
    1. **In-Memory Authentication**:
        - Adds hardcoded users for testing.
        - Example:
            
            ```java
            java
            Copy code
            protected void configure(AuthenticationManagerBuilder auth) throws Exception {
                auth.inMemoryAuthentication()
                    .withUser("user") // Username
                    .password("{noop}password") // Password
                    .roles("USER"); // Role
            }
            
            ```
            
        - This creates a user `user` with the password `password`.
    2. **Database Authentication** (for real-world apps):
        - Fetches user data from the database.
        - Example:
            
            ```java
            java
            Copy code
            protected void configure(AuthenticationManagerBuilder auth) throws Exception {
                auth.jdbcAuthentication()
                    .dataSource(dataSource)
                    .usersByUsernameQuery(
                        "SELECT username, password, enabled FROM users WHERE username = ?"
                    )
                    .authoritiesByUsernameQuery(
                        "SELECT username, role FROM user_roles WHERE username = ?"
                    );
            }
            
            ```
            
        - **Flow**:
            - When `username` and `password` are sent, Spring queries the database to:
                - Check if the user exists (`users` table).
                - Retrieve user roles (`user_roles` table).
    
    ---
    
    ```java
    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
    
    ```
    
    - **`@Bean`**: Exposes the `AuthenticationManager` as a Spring bean, so it can be used in other parts of the application (e.g., the Authorization Server).
    - **`authenticationManagerBean()`**: Provides the authentication mechanism configured here to other components (like OAuth endpoints).
    
    1. **`configure(HttpSecurity)`**:
        - Secures the Authorization Server by defining access rules.
        - Allows `/oauth/token` without authentication and secures other endpoints.
    
    ```java
    java
    Copy code
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
            .antMatchers("/oauth/token").permitAll() // Allow token endpoint
            .anyRequest().authenticated() // Secure other endpoints
            .and()
            .csrf().disable(); // Disable CSRF
    }
    
    ```
    
    ### **Flow Overview**
    
    Let me clarify the flow when the **API Gateway** hits the `/oauth/token` endpoint. This is a critical piece to understand how Spring Security validates the client (`client_id`, `client_secret`) and the user (`username`, `password`).
    
    ### **Flow: Validating `client_id` and `client_secret`**
    
    1. **Request from API Gateway**:
        - The API Gateway forwards the `/login` request to `/oauth/token` on the `userAuth` service.
        - The request includes:
            - `client_id` and `client_secret` in the `Authorization` header (Base64-encoded).
            - `grant_type`, `username`, and `password` in the request body.
        
        **Example request**:
        
        ```
        POST /oauth/token HTTP/1.1
        Authorization: Basic Y2xpZW50OnNlY3JldA==  // client:secret
        Content-Type: application/x-www-form-urlencoded
        
        grant_type=password&username=user&password=password
        
        ```
        
    2. **First Stop: `SecurityConfig` (WebSecurityConfigurerAdapter)**:
        - When the request hits `/oauth/token`, the `SecurityConfig` handles securing this endpoint via:
            
            ```java
            @Override
            protected void configure(HttpSecurity http) throws Exception {
                http.authorizeRequests()
                    .antMatchers("/oauth/token").permitAll() // Permit token requests
                    .anyRequest().authenticated() // Secure other endpoints
                    .and().csrf().disable(); // Disable CSRF for simplicity
            }
            
            ```
            
        - **What happens here?**
            - The `/oauth/token` endpoint is permitted without additional restrictions (`permitAll()`).
            - The request moves to the **Authorization Server** for further validation.
    
    ---
    
    1. **Second Stop: `AuthorizationServerConfig` (AuthorizationServerConfigurerAdapter)**:
        - The `AuthorizationServerConfig` ensures that the provided `client_id` and `client_secret` are valid.
        
        ```java
        @Override
        public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
            clients.inMemory()
                .withClient("client") //Valid client ID -> "client" can be vaibale above in this class whose value being fetch from .yml.
                .secret("{noop}secret") // Valid client secret -> "secret" can be a vaiable whose value being fetched from yml
                .authorizedGrantTypes("password", "refresh_token")
                .scopes("read", "write")
                .accessTokenValiditySeconds(900) // Access token validity
                .refreshTokenValiditySeconds(2592000); // Refresh token validity
        }
        
        ```
        
        **What happens here?**
        
        - The `ClientDetailsServiceConfigurer` sets up valid clients at application startup.
        - When `/oauth/token` is hit:
            - Spring Security extracts the `client_id` and `client_secret` from the `Authorization` header.
            - It compares these values against the configured clients (`inMemory()` in this case).
            - If the `client_id` and `client_secret` match, the request proceeds.
        
        ### **Where are the credentials matched?**
        
        The matching happens internally within Spring Security’s **ClientDetailsService**. This service is automatically wired into the Authorization Server when you define client details using `ClientDetailsServiceConfigurer`.
        
        Here’s the flow:
        
        1. The Authorization Server extracts the `client_id` and `client_secret` from the request header.
        2. Spring Security uses the `ClientDetailsService` implementation to retrieve the client details (defined in `ClientDetailsServiceConfigurer`).
        3. It compares the extracted credentials against those stored in memory (or a database if configured).
        
        ### **What object contains the credentials during validation?**
        
        When the `/oauth/token` endpoint is hit, the client credentials are encapsulated in a **`ClientAuthenticationToken`** object, which is part of Spring Security's flow.
        
        **Extract and Validate Client Credentials**:
        
        - The `AuthorizationServer` extracts the `client_id` and `client_secret` from the `Authorization` header.
        - Internally, Spring Security calls `ClientDetailsService.loadClientByClientId(clientId)` to retrieve the details for the provided `client_id`.
        - If the `client_secret` matches and the client is allowed to use the specified `grant_type`, validation passes.
    
    ---
    
    ### **Flow: Validating `username` and `password`**
    
    1. **Third Stop: User Authentication**:
        - If the client is valid, Spring Security validates the `username` and `password` using the `AuthenticationManager`.
        - The `AuthenticationManager` is defined in `SecurityConfig`:
            
            ```java
            @Override
            protected void configure(AuthenticationManagerBuilder auth) throws Exception {
                auth.jd inMemoryAuthentication()
                    .withUser("user").password("{noop}password").roles("USER");
            }
            
            ```
            
        - **What happens here?**
            - The `AuthenticationManager` checks if the provided `username` and `password` match the user configured in memory (in this case, `user:password`).
            - If valid, authentication succeeds; otherwise, an error is returned.
    
    ---
    
    1. **Token Generation**:
        - Once the user is authenticated, the `AuthorizationServerEndpointsConfigurer` links the `AuthenticationManager` to the Authorization Server:
            
            ```java
            @Override
            public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
                endpoints.authenticationManager(authenticationManager);
            }
            
            ```
            
        - The `AuthorizationServerEndpointsConfigurer`:
            - Generates an access token.
            - Optionally generates a refresh token if `refresh_token` is in the `grant_type`.
        
        **Generated Token Response Example**:
        
        ```json
        {
            "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI...",
            "token_type": "bearer",
            "expires_in": 900,
            "refresh_token": "d82dfd9e-22c6-4b1a-9fd8..."
        }
        
        ```
        
    
    ---
    
    ### **To Summarize the Flow**
    
    1. The `/oauth/token` request hits the `SecurityConfig` class:
        - `configure(HttpSecurity)` permits access to `/oauth/token`.
        - The request proceeds to the Authorization Server.
    2. The `AuthorizationServerConfig` validates the `client_id` and `client_secret`:
        - Configured using `ClientDetailsServiceConfigurer` (`inMemory()` in this example).
        - If valid, the request continues.
    3. The `AuthenticationManager` (in `SecurityConfig`) validates the `username` and `password`:
        - Configured using `AuthenticationManagerBuilder` (`inMemoryAuthentication()` in this example).
        - If valid, the user is authenticated.
    4. The `AuthorizationServerEndpointsConfigurer` generates the access token:
        - Uses the `AuthenticationManager` to link authentication logic to the token generation process.
    
    ---
    
    ### **Your Questions Answered**
    
    1. **Does the request first go to `SecurityConfig`?**
        - Yes. It ensures the `/oauth/token` endpoint is accessible without requiring additional authentication.
    2. **Does it check the `client_id` and `client_secret` in `AuthorizationServerConfig`?**
        - Yes. The `ClientDetailsServiceConfigurer` checks if the provided `client_id` and `client_secret` match the pre-configured values.
    3. **How does it validate `username` and `password`?**
        - Through the `AuthenticationManager` in `SecurityConfig`. This is linked to the Authorization Server via `AuthorizationServerEndpointsConfigurer`.
    4. **Where is the token generated?**
        - In the `AuthorizationServerEndpointsConfigurer`. It handles token generation after successful authentication of the client and user.
    
    ### **What Endpoint Does the Authorization Server Use for `/login`?**
    
    The Authorization Server automatically exposes `/oauth/token` for token generation when configured with `AuthorizationServerConfigurerAdapter`.
    
    In this setup:
    
    - The Gateway maps `/login` to `/oauth/token` in the Authorization Server.
    - The Authorization Server processes the request to `/oauth/token` using the details provided in `AuthorizationServerConfig`
    
    ### **Does `UserAuthService` Have a Controller?**
    
    No, the `/oauth/token` endpoint is automatically provided by Spring Security OAuth2 when you annotate your application with `@EnableAuthorizationServer`.
    
    The framework handles the routing to `/oauth/token` without requiring you to define a controller explicitly. Instead, the behavior is defined in the `AuthorizationServerConfig` and `SecurityConfig`.
    
    ### **Does the Request Hit `WebSecurityConfigurerAdapter` First?**
    
    Yes. When a request (e.g., `/oauth/token`) is sent to the Authorization Server:
    
    1. It first passes through the `WebSecurityConfigurerAdapter`.
    2. The `WebSecurityConfigurerAdapter`:
        - Ensures the request has valid basic authentication (`clientId:clientSecret`).
        - Authenticates the user credentials (`username`, `password`) for the token request.
    
    ### To What the Client ID and Client Secret compared to?
    
    The code inside the `configure(ClientDetailsServiceConfigurer clients)` method **does not create new clients dynamically or generate tokens**. Instead, it **defines and registers the client details** (such as `clientId`, `clientSecret`, etc.) for the Authorization Server to use when validating incoming requests.
    
    This configuration ensures that when a request to `/oauth/token` is made, the provided `clientId` and `clientSecret` are compared against the details defined in this method.
    
    ---
    
    ### Key Points:
    
    1. **What the Code Does:**
        - It **registers client details** (like `clientId`, `clientSecret`, grant types, and scopes) into the Authorization Server's internal registry.
        - These details are stored in memory (or in a database, if you use a `JdbcClientDetailsService`) and used for validating incoming token requests.
    2. **When the Comparison Happens:**
        - During a `/oauth/token` request, the provided `clientId` and `clientSecret` are extracted from the `Authorization` header.
        - The Authorization Server retrieves the registered client details (from in-memory or database storage) and **compares** them against the incoming values.
    
    ---
    
    ### **How It Works:**
    
    ### **1. Registration (Configuration Stage)**
    
    When the application starts, the `configure(ClientDetailsServiceConfigurer clients)` method runs once to:
    
    - Register one or more clients (`clientId` and `clientSecret` pairs).
    - Define which grant types, scopes, and token validity rules apply to those clients.
    
    For example:
    
    ```java
    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        clients.inMemory()
            .withClient("client") // This registers "client" as a valid clientId.
            .secret("{noop}secret") // This registers "secret" as its corresponding clientSecret.
            .authorizedGrantTypes("password") // Specifies the "password" grant type is allowed.
            .scopes("read", "write") // Specifies the client has these scopes.
            .accessTokenValiditySeconds(900); // Sets the token validity.
    }
    
    ```
    
    ### **2. Validation (During Token Request)**
    
    When a request is made to `/oauth/token`:
    
    - The `clientId` and `clientSecret` provided in the request are extracted (e.g., from the `Authorization: Basic` header).
    - The Authorization Server retrieves the registered details for the given `clientId` from the configured `ClientDetailsService` (in this case, `InMemoryClientDetailsService`).
    - A comparison is made:
        - Does the `clientId` exist?
        - Does the `clientSecret` match the stored value for that `clientId`?
        - Are the requested grant type and scopes allowed for this client?
    
    If the validation is successful, the token generation process continues.
    
    ---
    
    ### **Does It Create a New Client?**
    
    No. The `configure(ClientDetailsServiceConfigurer clients)` method **does not create new clients dynamically**. Instead:
    
    - It defines which clients are valid and their associated settings during application startup.
    - These settings remain constant unless you restart the application or reconfigure the `ClientDetailsService` (e.g., to fetch client details from a database).
    
    ---
    
    ### **Comparison vs. Token Generation**
    
    - **Client Validation**:
        - Happens during a `/oauth/token` request.
        - The provided `clientId` and `clientSecret` are compared against the details registered in the `configure(ClientDetailsServiceConfigurer clients)` method.
    - **Token Generation**:
        - Happens after client validation succeeds.
        - Tokens are generated by the Authorization Server using the `AuthorizationServerEndpointsConfigurer`.
    
    ---
    
    ### Conclusion:
    
    The `configure(ClientDetailsServiceConfigurer clients)` method **defines client details** and stores them in memory (or another backend). During a token request, the stored details are compared with the incoming `clientId` and `clientSecret` to validate the request.
    
    ### **Answering Specific Questions**
    
    1. **Where Is `username` and `password` Validated?**
        - In the `AuthenticationManager`:
            - Configured in `SecurityConfig` using `configure(AuthenticationManagerBuilder)`.
            - The manager checks the user credentials against:
                - **In-Memory Users** (in the example).
                - **Database Users** (if configured).
    2. **Where Is the Token Being Generated?**
        - Spring Security OAuth2 handles token generation automatically inside the `/oauth/token` endpoint.
        - It uses the configurations in:
            - `AuthorizationServerEndpointsConfigurer` (for authentication).
            - `ClientDetailsServiceConfigurer` (for client validation).
    
    ---
    
- What is Spring Boot ? Why did you use spring boot in your project ? Why not spring ?
    
    Spring boot is a spring module . Spring boot is a framework for RAD build using spring framework with extra support of auto-configuration and embedded application server ( like tomcat , jetty)
    
    It helps us in creating efficient fast stand-alone application which you can just run it basically removes a lot of configuration and dependencies
    
- **How Does spring boot application works**
    
    Spring Boot simplifies the development and deployment of Java applications. Here's a step-by-step breakdown of how a Spring Boot application works, from startup to running:
    
    ---
    
    ### **1. The `@SpringBootApplication` Annotation**
    
    When you run a Spring Boot application, it starts with a class annotated with `@SpringBootApplication`. This annotation is a **convenience annotation** that combines:
    
    - **`@SpringBootConfiguration`**: Indicates that this class provides configuration for the Spring application.
    - **`@EnableAutoConfiguration`**: Enables Spring Boot's auto-configuration mechanism to set up beans automatically based on the classpath dependencies and configurations.
    - **`@ComponentScan`**: Scans the package and its sub-packages for components, configurations, and services.
    
    ---
    
    ### **2. Application Startup**
    
    The `main()` method is the entry point to the Spring Boot application:
    
    ```java
    @SpringBootApplication
    public class DemoApplication {
        public static void main(String[] args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
    
    ```
    
    ### What happens in `SpringApplication.run()`?
    
    1. **Bootstrap the Application Context**:
        - The method starts by creating an instance of `SpringApplication`.
        - It prepares the application context, a central container in Spring that manages beans and dependencies.
    2. **Environment Preparation**:
        - It prepares the environment (`ApplicationContext`) by loading properties from files like `application.properties` or `application.yml`.
        - Profiles (like `dev`, `prod`) are also determined at this stage.
    3. **Auto-Configuration**:
        - Spring Boot auto-configures the application based on dependencies in the classpath. For example:
            - If `spring-boot-starter-web` is present, it sets up a web server (e.g., Tomcat or Jetty).
            - If `spring-boot-starter-data-jpa` is present, it configures a JPA entity manager.
    4. **Bean Scanning and Initialization**:
        - The `@ComponentScan` annotation scans the current package and sub-packages for `@Component`, `@Service`, `@Repository`, `@Controller`, etc., and registers them as beans in the application context.
        - The `@Bean` annotations in the configuration classes are also registered.
    5. **Application Listeners and Initializers**: → **Ignore**
        - Spring triggers `ApplicationListeners` and `ApplicationContextInitializers` if any are registered.
    6. **Embedded Server Initialization**:
        - If it’s a web application, Spring Boot initializes the embedded web server (e.g., Tomcat, Jetty).
        - The server starts and listens for HTTP requests on the configured port (default: 8080).
    7. **CommandLineRunner and ApplicationRunner**:
        - If beans implementing `CommandLineRunner` or `ApplicationRunner` interfaces are present, they are executed at this point.
        - This is useful for executing custom logic at startup.
    
    ---
    
    ### **3. Dependency Injection and Bean Lifecycle**
    
    Spring Boot handles **Dependency Injection** (DI) to provide the required beans wherever needed.
    
    - During the startup:
        - All required beans are instantiated.
        - Beans are injected into classes where they are needed (`@Autowired`).
        - Post-construction hooks like `@PostConstruct` are executed.
    
    ---
    
    ### **4. Handling HTTP Requests**
    
    For web applications:
    
    - **DispatcherServlet**:
        - Spring Boot configures a `DispatcherServlet` that routes incoming HTTP requests to the appropriate controller methods.
        - This servlet is part of the **Spring MVC framework**.
    - **Controllers**:
        - Controllers annotated with `@RestController` or `@Controller` handle the requests.
        - Mappings like `@GetMapping`, `@PostMapping` define the endpoints.
    
    ---
    
    ### **5. Application Context**
    
    The **ApplicationContext** is the central container for the Spring application. It:
    
    - Manages the lifecycle of beans.
    - Provides configuration and resolves dependencies.
    
    ---
    
    ### **6. Running and Monitoring**
    
    Spring Boot sets up tools like **Actuator** for monitoring the application. Actuator provides endpoints like:
    
    - `/actuator/health`: Application health.
    - `/actuator/metrics`: Performance metrics.
    
    ---
    
    ### **7. Shutdown Process**
    
    When the application is stopped:
    
    1. The Spring Boot context is closed.
    2. Beans implementing `DisposableBean` or annotated with `@PreDestroy` are given a chance to release resources.
    3. Embedded servers are shut down.
    
    ---
    
    ### **Example**
    
    Here’s a simple Spring Boot application:
    
    ### **Controller**
    
    ```java
    @RestController
    @RequestMapping("/api")
    public class DemoController {
        @GetMapping("/hello")
        public String sayHello() {
            return "Hello, Spring Boot!";
        }
    }
    
    ```
    
    ### **Main Class**
    
    ```java
    @SpringBootApplication
    public class DemoApplication {
        public static void main(String[] args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
    
    ```
    
    ### **Properties File**
    
    `application.properties`:
    
    ```
    server.port=8081
    spring.application.name=DemoApplication
    ```
    
    ---
    
    ### **Summary**
    
    - **Startup**: The `SpringApplication.run()` initializes the application context and configures everything automatically.
    - **Dependency Injection**: Beans are created and injected.
    - **Request Handling**: DispatcherServlet routes HTTP requests to the appropriate controllers.
    - **Monitoring**: Tools like Actuator help monitor the application.
    - **Shutdown**: Beans and resources are gracefully closed.
    
    This streamlined process is what makes Spring Boot so powerful and developer-friendly.
    
- **Dependency Injection**
    
    Spring Boot handles **Dependency Injection** (DI) to provide the required beans wherever needed.
    
    - During the startup:
        - All required beans are instantiated.
        - Beans are injected into classes where they are needed (`@Autowired`).
        - Post-construction hooks like `@PostConstruct` are executed.
- What is dialect and what is it’s use?
    
    ## 🧾 What is a Dialect?
    
    In **Hibernate**, a **Dialect** is a class that tells Hibernate **how to generate SQL** queries for a specific type of database (like MySQL, PostgreSQL, Oracle, etc.).
    
    > 🔁 Think of it like a translator — it knows the “language” of your database.
    > 
    
    ---
    
    ### 🔍 Why is it needed?
    
    Different databases have different:
    
    - SQL syntax
    - Functions (`LIMIT` vs `ROWNUM`)
    - Data types (`VARCHAR2` vs `VARCHAR`)
    - Identity generation strategies
    
    The **dialect ensures Hibernate generates SQL** that is compatible with the specific database you're using.
    
    ---
    
    ### 🛠️ Common Dialects
    
    | Database | Dialect Class |
    | --- | --- |
    | MySQL | `org.hibernate.dialect.MySQLDialect` |
    | PostgreSQL | `org.hibernate.dialect.PostgreSQLDialect` |
    | Oracle | `org.hibernate.dialect.OracleDialect` |
    | H2 | `org.hibernate.dialect.H2Dialect` |
    | SQL Server | `org.hibernate.dialect.SQLServerDialect` |
    
    ---
    
    ## ✅ Example in Spring Boot
    
    You can specify it in your `application.properties`:
    
    ```
    spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
    ```
    
    > 🎯 This is optional in Spring Boot — it often auto-detects the dialect based on the JDBC URL.
    > 
    
    ---
    
    ### 🔁 What happens behind the scenes?
    
    Let’s say you're using `MySQLDialect`. When Hibernate sees this query:
    
    ```java
    entityManager.find(User.class, 1L);
    ```
    
    It might generate:
    
    ```sql
    SELECT * FROM users WHERE id = 1 LIMIT 1;
    ```
    
    If you used Oracle, the dialect would change the SQL to use:
    
    ```sql
    SELECT * FROM (SELECT * FROM users WHERE id = 1) WHERE ROWNUM <= 1;
    ```
    
    ---
    
    ## 🧠 TL;DR
    
    | Concept | Explanation |
    | --- | --- |
    | **Dialect** | A Hibernate class that defines how SQL is generated for a specific database |
    | **Use** | Makes Hibernate DB-agnostic by handling syntax differences |
    | **Set in Spring Boot?** | Optional (auto-detected), but can be set manually for control |
    | **Why care?** | To avoid weird SQL errors and ensure your app runs smoothly on your DB |
- **Spring Security**
    
    Spring Security is a powerful framework for securing applications, providing mechanisms for **authentication** (verifying who the user is) and **authorization** (determining what the user is allowed to do). It integrates seamlessly with Spring Boot applications and offers flexibility for various use cases.
    
    ---
    
    ### **Key Concepts**
    
    1. **Authentication**: Verifies user credentials (e.g., username/password, token).
    2. **Authorization**: Checks if a user has permission to access specific resources or APIs.
    3. **Filters**: Spring Security uses filters to process requests and enforce security.
    4. **Filter Chain**: A series of filters that process a request sequentially.
    
    ---
    
    ### **How Authentication & Authorization Work**
    
    1. **Open Pages** (e.g., About Us, Contact):
        
        Publicly accessible APIs/pages require no authentication or authorization.
        
    2. **Protected Pages** (e.g., Account Info, Transaction History):
        
        APIs for these pages require:
        
        - **Authentication**: User must be logged in.
        - **Authorization**: User must have specific roles/permissions to access the page.
    3. **Filter Chaining**:
        - Requests pass through a series of filters (e.g., authentication filters, token validation filters).
        - Each filter performs specific checks before allowing the request to proceed.
    
    ---
    
    ### **Scenario: Securing APIs Using API Gateway, Filters, and Tokens**
    
    1. **API Gateway**: Acts as a single entry point for all client requests.
        - Routes requests to appropriate microservices.
        - Verifies tokens for authentication and handles authorization logic.
    2. **Filters**: Applied in the API Gateway to:
        - Authenticate incoming requests.
        - Validate user tokens.
        - Check user roles/permissions.
    3. **Tokens**:
        - **Access Tokens**: Short-lived tokens used to authenticate API requests.
        - **Refresh Tokens**: Long-lived tokens used to renew access tokens.
    
    ---
    
    ### **Step-by-Step Implementation**
    
    ### **1. Adding Dependencies**
    
    Add the following dependencies to your `pom.xml` or `build.gradle` file for Spring Security and JWT support:
    
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
        <version>0.9.1</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-gateway</artifactId>
    </dependency>
    
    ```
    
    ---
    
    ### **2. Configuring Security in API Gateway**
    
    Create a filter in your API Gateway to handle token validation.
    
    ```java
    @Component
    public class JwtAuthenticationFilter implements GatewayFilter {
    
        @Override
        public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
            String token = extractToken(exchange.getRequest().getHeaders());
    
            if (token == null || !isValidToken(token)) {
                exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
                return exchange.getResponse().setComplete();
            }
    
            // If valid, proceed with the request
            return chain.filter(exchange);
        }
    
        private String extractToken(HttpHeaders headers) {
            return headers.getFirst("Authorization");
        }
    
        private boolean isValidToken(String token) {
            // Logic to validate JWT (e.g., check signature, expiry)
            return true;
        }
    }
    
    ```
    
    ---
    
    ### **3. Configuring Routes in API Gateway**
    
    Define routes in `application.yml`:
    
    ```yaml
    spring:
      cloud:
        gateway:
          routes:
            - id: about-page
              uri: http://localhost:8081/about
              predicates:
                - Path=/about/**
            - id: account-service
              uri: http://localhost:8082/accounts
              predicates:
                - Path=/accounts/**
              filters:
                - JwtAuthenticationFilter
    
    ```
    
    ---
    
    ### **4. Securing APIs in Microservices**
    
    Add a security configuration to the `AccountsService`:
    
    ```java
    @Configuration
    @EnableWebSecurity
    public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .csrf().disable()
                .authorizeRequests()
                .antMatchers("/public/**").permitAll()  // Open endpoints
                .antMatchers("/accounts/**").authenticated()  // Secured endpoints
                .and()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .addFilter(new JwtAuthorizationFilter(authenticationManager()));
        }
    }
    
    ```
    
    ---
    
    ### **5. Handling Roles/Permissions**
    
    In your controller, annotate methods to define access roles:
    
    ```java
    @RestController
    @RequestMapping("/accounts")
    public class AccountsController {
    
        @PreAuthorize("hasRole('USER')")
        @GetMapping("/info")
        public String getAccountInfo() {
            return "Account Information";
        }
    
        @PreAuthorize("hasRole('ADMIN')")
        @PostMapping("/add")
        public String addAccount() {
            return "Account Added";
        }
    }
    
    ```
    
    ---
    
    ### **How This Works in a Banking Scenario**
    
    1. **Open Pages**:
        
        Routes like `/about` are open to the public and do not require authentication.
        
    2. **Protected APIs**:
        - When a user accesses `/accounts/info`, the request is routed through the API Gateway.
        - The `JwtAuthenticationFilter` in the Gateway checks the token.
        - If valid, the request proceeds to the `AccountsService`, where:
            - The security configuration checks if the user is authenticated.
            - The `@PreAuthorize` annotation checks the user's roles/permissions.
    3. **Authorization Logic**:
        
        Only users with the required roles can access specific endpoints.
        
    
    ---
    
    ### **Filter Chain**
    
    The **filter chain** in Spring Security is a sequence of filters that process a request. Each filter performs a specific task, such as:
    
    - Validating tokens.
    - Checking roles/permissions.
    - Enforcing security policies.
    
    Example of a filter chain:
    
    1. `SecurityContextPersistenceFilter`: Restores the security context for the request.
        - `SecurityContextPersistenceFilter` Explanation →
            
            The **`SecurityContextPersistenceFilter`** is part of the **Spring Security Filter Chain** and is responsible for managing the `SecurityContext` across multiple requests. Let's break it down:
            
            ---
            
            ### **Role of `SecurityContext`**
            
            The `SecurityContext` is a container for holding security-related information about the currently authenticated user. It includes:
            
            1. The principal (authenticated user).
            2. Credentials (if available).
            3. Authorities/roles granted to the user.
            
            The `SecurityContext` is stored in the `SecurityContextHolder` during the processing of a request.
            
            ---
            
            ### **What Does `SecurityContextPersistenceFilter` Do?**
            
            1. **Restores SecurityContext:**
                - At the start of a request, it attempts to restore the `SecurityContext` from a persistent store (e.g., the HTTP session).
                - If no `SecurityContext` exists, it initializes an empty one.
            2. **Stores SecurityContext:**
                - At the end of the request, it ensures the current `SecurityContext` is saved back to the persistent store if necessary.
            3. **Manages the Lifecycle:**
                - It ensures that security information is consistently available throughout the lifecycle of a request.
                - If an authentication occurs during the request, it updates the `SecurityContext`.
            
            ---
            
            ### **Flow of `SecurityContextPersistenceFilter`:**
            
            1. **Request Initiation:**
                - Checks if a `SecurityContext` exists in the persistent store (e.g., `HttpSessionSecurityContextRepository`).
                - If found, loads it into the `SecurityContextHolder`.
            2. **Processing Request:**
                - The `SecurityContext` is available to other filters and components during the request.
            3. **Request Completion:**
                - If the `SecurityContext` was modified (e.g., a user logged in), it is saved back to the persistent store.
            
            ---
            
            ### **Why Is It Important?**
            
            - It provides continuity of security information across requests.
            - Without it, every request would need to re-authenticate the user, which is inefficient.
            
            ---
            
            ### **How Does It Work Internally?**
            
            Here’s a simplified explanation of its mechanism:
            
            1. **Restoration Logic:**
                - Uses an implementation of `SecurityContextRepository` (e.g., `HttpSessionSecurityContextRepository`) to retrieve the `SecurityContext`.
                - Restores the context to the `SecurityContextHolder`.
            2. **Persistence Logic:**
                - At the end of the request, it checks if the `SecurityContext` has changed or if it’s no longer empty.
                - If necessary, it saves the `SecurityContext` back to the repository.
            
            ---
            
            ### **Code Illustration:**
            
            ### 1. Customizing `SecurityContextPersistenceFilter`:
            
            You typically don’t modify this filter directly, but you can customize its behavior by providing your own `SecurityContextRepository`.
            
            ```java
            @Bean
            public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
                http
                    .securityContext(securityContext ->
                        securityContext.securityContextRepository(myCustomRepository())
                    );
                return http.build();
            }
            
            public SecurityContextRepository myCustomRepository() {
                return new MyCustomSecurityContextRepository();
            }
            
            ```
            
            ### 2. Custom `SecurityContextRepository`:
            
            You can create a custom repository if you want to store the `SecurityContext` somewhere other than the HTTP session.
            
            ```java
            public class MyCustomSecurityContextRepository implements SecurityContextRepository {
            
                @Override
                public SecurityContext loadContext(HttpRequestResponseHolder requestResponseHolder) {
                    // Load SecurityContext from a custom source
                    return new SecurityContextImpl();
                }
            
                @Override
                public void saveContext(SecurityContext context, HttpServletRequest request, HttpServletResponse response) {
                    // Save SecurityContext to a custom source
                }
            
                @Override
                public boolean containsContext(HttpServletRequest request) {
                    // Check if the context exists
                    return false;
                }
            }
            
            ```
            
            ---
            
            ### **Key Points to Remember:**
            
            1. The `SecurityContextPersistenceFilter` works with the `SecurityContextHolder` to manage security context data.
            2. It is typically placed **early in the filter chain** to ensure that security information is available to all subsequent filters.
            3. By default, it uses the **HTTP session** as the storage mechanism for `SecurityContext`.
            4. It simplifies stateless or stateful security management by consistently restoring and persisting the security context.
            
            ### **When Do You Customize It?**
            
            - When you don’t want to use HTTP sessions and need to persist security contexts elsewhere, like a database or a cache.
    2. `UsernamePasswordAuthenticationFilter`: Handles username/password authentication.
    3. `JwtAuthenticationFilter`: Validates JWT tokens.
    
    ---
    
    ### **Key Takeaways**
    
    - **API Gateway**: Centralized entry point for all requests, responsible for routing and token validation.
    - **Filters**: Used for authentication and authorization.
    - **Tokens**: Provide stateless authentication for microservices.
    - **Roles/Permissions**: Controlled using annotations like `@PreAuthorize`.
    
    Let me know if you'd like any part of this clarified further!
    
- Is this possible to change the port of embedded tomcat server in spring boot ?
    
    put spring.port in application.properties
    
- Can we override or replace the embedded tomcat server in spring boot ?
    
    yes we can remove the existing server steps are enter maven repository of tomcat in exclusion of spring-starter-web and add the jetty dependency.
    
- How to disable a specific auto - configuration class ?
    
    @EnableAutoConfiguration(exclude=[DataSourceDataJPA.class])
    
- What does the @SpringBootApplication annotations do internally ?
    
    Consisits of 3 parts 
    
    1. @Configuration
    2. @EnableAutoConfiguration
    3. @ComponentScan
    
- How can we use values of [application.properties](http://application.properties) in java class
    
    we can use with the help of @Value(”${port}”) Annotations.
    
- Explain RestController annotations in Spring Boot ?
    
    Contain 2 controller → @Controller , @ResponseBody
    
- Difference between @RestController and @Controller annotations.
    
    Controller Map of the model object to view or template and makes it human readable but @RestController simply returns the object and object data is directly written into HTTP response as JSON or XML.
    
- What are the major differences between RequestMapping and GetMapping ?
    
    RequestMapping can be used with GET , POST, PUT and many other request methods using the method on the annotation . Whereas GetMapping is only an extension of RequestMapping which helps you to improve clarity on requests.
    
- What is the use of profiles in Spring boot ?
    
    ## What is the use of Profiles in Spring Boot?
    
    **Spring Profiles** allow you to:
    
    - Load different **beans**, **configurations**, or **properties** based on the active environment.
    - Avoid hardcoding environment-specific details like DB URLs, API keys, etc.
    
    ---
    
    ## 🧪 Real-life Example
    
    Let’s say you want:
    
    - **H2 database in dev**
    - **MySQL in production**
    
    Using profiles, you can configure each separately and activate the right one depending on where the app is running.
    
    ---
    
    ## 🏷️ What is `@Profile` annotation?
    
    The `@Profile` annotation tells Spring:
    
    > "Only load this bean/configuration if a specific profile is active."
    > 
    
    ### ✅ Syntax:
    
    ```java
    @Component
    @Profile("dev")
    public class DevDataSourceConfig {
        // This will only load when "dev" profile is active
    }
    ```
    
    ```java
    @Component
    @Profile("prod")
    public class ProdDataSourceConfig {
        // This will only load when "prod" profile is active
    
    ```
    
    ---
    
    ## 🚀 How to activate a profile?
    
    ### 1. **In `application.properties`**:
    
    ```
    spring.profiles.active=dev
    ```
    
    ---
    
    ### 2. **Via command line**:
    
    ```bash
    java -jar app.jar --spring.profiles.active=prod
    ```
    
    ---
    
    ### 3. **In code** (less common):
    
    ```java
    System.setProperty("spring.profiles.active", "dev");
    ```
    
    ---
    
    ## 📁 Profile-Specific Property Files
    
    You can create different config files for each profile:
    
    - `application-dev.properties`
    - `application-prod.properties`
    
    Spring Boot automatically picks the one that matches the active profile.
    
    ---
    
    ### 🧠 Example Use Case
    
    Let’s say you have these files:
    
    ```
    # application-dev.properties
    spring.datasource.url=jdbc:h2:mem:testdb
    
    # application-prod.properties
    spring.datasource.url=jdbc:mysql://localhost:3306/proddb
    ```
    
    Now, when you set `spring.profiles.active=prod`, your app will connect to MySQL instead of H2.
    
    ---
    
    ## ✅ Summary
    
    | Concept | Description |
    | --- | --- |
    | `@Profile` | Loads a class/bean only for a specific profile |
    | `spring.profiles.active` | Tells Spring which profile is currently active |
    | Use case | Environment-specific configs (dev vs prod vs test) |
    | Benefits | Cleaner code, more flexibility, environment separation |
- What is Spring Actuator ?  What are its advantage ?
- Steps to deploy Spring Boot web applications as JAR and WAR file ?
- How to implement custom Validation
    
    Spring Boot provides **JPA validation (Hibernate Validator)** using `@Valid` and `@Validated`, but sometimes we need **custom validation** for complex business rules.
    
    ---
    
    ## **🔹 Steps to Implement Custom Validation**
    
    ### **✅ Step 1: Create a Custom Annotation**
    
    Define an annotation for validation.
    
    ```java
    import javax.validation.Constraint;
    import javax.validation.Payload;
    import java.lang.annotation.*;
    
    @Documented
    @Constraint(validatedBy = MyCustomValidator.class) // Link to Validator class
    @Target({ElementType.FIELD, ElementType.PARAMETER})
    @Retention(RetentionPolicy.RUNTIME)
    public @interface MyCustomValidation {
        String message() default "Invalid input!"; // Default error message
        Class<?>[] groups() default {};  // For grouping constraints
        Class<? extends Payload>[] payload() default {}; // Additional metadata
    }
    
    ```
    
    ---
    
    ### **✅ Step 2: Create a Validator Class**
    
    Implement `ConstraintValidator<Annotation, Type>`.
    
    ```java
    import javax.validation.ConstraintValidator;
    import javax.validation.ConstraintValidatorContext;
    
    public class MyCustomValidator implements ConstraintValidator<MyCustomValidation, String> {
    
        @Override
        public boolean isValid(String value, ConstraintValidatorContext context) {
            // Custom validation logic (e.g., must start with "ABC")
            return value != null && value.startsWith("ABC");
        }
    }
    ```
    
    ---
    
    ### **✅ Step 3: Apply the Custom Validation in a DTO**
    
    Use the annotation in a **DTO (Data Transfer Object)**.
    
    ```java
    import javax.validation.constraints.NotNull;
    
    public class UserDTO {
    
        @NotNull(message = "Username cannot be null")
        @MyCustomValidation(message = "Username must start with 'ABC'")
        private String username;
    
        // Constructor, Getters & Setters
    }
    ```
    
    ---
    
    ### **✅ Step 4: Use @Valid in the Controller**
    
    ```java
    import org.springframework.http.ResponseEntity;
    import org.springframework.web.bind.annotation.*;
    
    import javax.validation.Valid;
    
    @RestController
    @RequestMapping("/users")
    public class UserController {
    
        @PostMapping("/create")
        public ResponseEntity<String> createUser(@Valid @RequestBody UserDTO user) {
            return ResponseEntity.ok("User Created: " + user.getUsername());
        }
    }
    ```
    
    ---
    
    ### **✅ Step 5: Test the API**
    
    📌 **Valid Input:**
    
    ```json
    {
      "username": "ABCJohn"
    }
    ```
    
    ✔️ **Response:**
    
    ```json
    "User Created: ABCJohn"
    ```
    
    📌 **Invalid Input:**
    
    ```json
    {
      "username": "XYZJohn"
    }
    ```
    
    ❌ **Error Response:**
    
    ```json
    
    {
      "error": "Username must start with 'ABC'"
    }
    ```
    
    ---
    
    ## **🚀 Summary Table**
    
    | **Step** | **Description** |
    | --- | --- |
    | **1️⃣ Create Annotation** | Define `@MyCustomValidation` |
    | **2️⃣ Implement Validator** | Implements `ConstraintValidator<T, FieldType>` |
    | **3️⃣ Use in DTO** | Apply annotation in DTO |
    | **4️⃣ Validate in Controller** | Use `@Valid` in Spring Boot REST Controller |
    | **5️⃣ Test API** | Send valid and invalid requests |
- What are the different layers in spring boot ?
    
    ***3 layers***
    
    1. Controller layer.
    2. Service layer.
    3. Repository layer.
- How to connect 2 databases
- What is Caching in Spring boot ?
- What are the different @Bean Scope in spring boot ?
- What is the lifecycle of spring boot ?
- What are the lifecycle of Beans
- SOLID Principle
    
    The **SOLID principles** are a set of design principles in object-oriented programming aimed at creating software that is easy to maintain, extend, and understand. Each letter in **SOLID** stands for a principle:
    
    ---
    
    ### 1. **S - Single Responsibility Principle (SRP)**
    
    **Definition**: A class should have only one reason to change, meaning it should only have one responsibility.
    
    **Example**:
    
    ```java
    class ReportGenerator {
        public void generateReport() {
            System.out.println("Report generated.");
        }
    }
    
    class ReportPrinter {
        public void printReport() {
            System.out.println("Report printed.");
        }
    }
    
    ```
    
    **Explanation**:
    
    - `ReportGenerator` is responsible for generating reports.
    - `ReportPrinter` is responsible for printing reports.
    - They have distinct responsibilities, making the code modular and maintainable.
    - 2. **O - Open/Closed Principle (OCP)**
    
    **Definition**: A class should be open for extension but closed for modification.
    
    **Example**:
    
    ```java
    abstract class Shape {
        abstract void draw();
    }
    
    class Circle extends Shape {
        public void draw() {
            System.out.println("Drawing Circle");
        }
    }
    
    class Rectangle extends Shape {
        public void draw() {
            System.out.println("Drawing Rectangle");
        }
    }
    
    class DrawingTool {
        public void drawShape(Shape shape) {
            shape.draw();
        }
    }
    
    ```
    
    **Explanation**:
    
    - New shapes like `Triangle` can be added by extending the `Shape` class without modifying existing code.
    
    ### **3. L - Liskov Substitution Principle (LSP)**
    
    **Definition**: Subtypes should be substitutable for their base types without affecting the program's behavior.
    
    **Example**:
    
    Java
    
    ```java
    class Bird {
        public void fly() {
            System.out.println("Flying...");
        }
    }
    
    class Sparrow extends Bird {}
    
    class Ostrich extends Bird {
        @Override
        public void fly() {
            throw new UnsupportedOperationException("Ostriches can't fly");
        }
    }
    ```
    
    ​
    
    **Violation**:
    
    Ostrich violates LSP because it cannot be used as a substitute for Bird (it can't fly).
    
    **Fix**:
    
    ```java
    abstract class Bird {}
    
    class FlyingBird extends Bird {
        public void fly() {
            System.out.println("Flying...");
        }
    }
    
    class Ostrich extends Bird {}
    ```
    
    ### 4. **I - Interface Segregation Principle (ISP)**
    
    **Definition**: A class should not be forced to implement interfaces it does not use.
    
    **Example**:
    
    ```java
    interface Animal {
        void eat();
        void fly(); // Not all animals can fly
    }
    
    class Dog implements Animal {
        public void eat() {
            System.out.println("Dog eats.");
        }
    
        public void fly() {
            throw new UnsupportedOperationException("Dogs can't fly");
        }
    }
    
    ```
    
    **Violation**:
    
    - `Dog` is forced to implement `fly()` even though it cannot fly.
    
    **Fix**:
    
    ```java
    interface Eater {
        void eat();
    }
    
    interface Flyer {
        void fly();
    }
    
    class Dog implements Eater {
        public void eat() {
            System.out.println("Dog eats.");
        }
    }
    
    ```
    
    ### 5. **D - Dependency Inversion Principle (DIP)**
    
    - Dependency Inversion Logger Example and Explanation for Deep Understanding
        
        **Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions.
        
        **Example:** A car should depend on an engine interface, not a specific engine type.
        
        ![image.png](assets/image%209.png)
        
        ```java
        // Abstraction (Interface)
        interface Database {
            void save(String data);
        }
        
        // Low-level module
        class MySQLDatabase implements Database {
            public void save(String data) {
                System.out.println("Saving " + data + " to MySQL database");
            }
        }
        
        // Another low-level module
        class InMemoryDatabase implements Database {
            public void save(String data) {
                System.out.println("Saving " + data + " to in-memory database");
            }
        }
        
        // High-level module
        class OrderService {
            private Database database;
        
            // Dependency injection through the constructor
            public OrderService(Database database) {
                this.database = database;
            }
        
            public void processOrder(String order) {
                database.save(order);
            }
        }
        
        public class Main {
            public static void main(String[] args) {
                // Using MySQLDatabase
                Database mySQLDatabase = new MySQLDatabase();
                OrderService orderService1 = new OrderService(mySQLDatabase);
                orderService1.processOrder("Order1");
        
                // Switching to InMemoryDatabase
                Database inMemoryDatabase = new InMemoryDatabase();
                OrderService orderService2 = new OrderService(inMemoryDatabase);
                orderService2.processOrder("Order2");
            }
        }
        ```
        
        [https://www.notion.so](https://www.notion.so)
        
    
    **Definition**: High-level modules should not depend on low-level modules; both should depend on abstractions.
    
    **Example**:
    
    ```java
    class Keyboard {}
    class Monitor {}
    
    class Computer {
        private Keyboard keyboard;
        private Monitor monitor;
    
        public Computer() {
            keyboard = new Keyboard();
            monitor = new Monitor();
        }
    }
    
    ```
    
    **Violation**:
    
    - `Computer` is tightly coupled with `Keyboard` and `Monitor`.
    
    **Fix**:
    
    ```java
    interface InputDevice {}
    class Keyboard implements InputDevice {}
    
    interface OutputDevice {}
    class Monitor implements OutputDevice {}
    
    class Computer {
        private InputDevice inputDevice;
        private OutputDevice outputDevice;
    
        public Computer(InputDevice inputDevice, OutputDevice outputDevice) {
            this.inputDevice = inputDevice;
            this.outputDevice = outputDevice;
        }
    }
    
    ```
    
    **Explanation**:
    
    - Now `Computer` depends on abstractions (`InputDevice` and `OutputDevice`), not concrete implementations.
    
    ---
    
    ### Summary:
    
    **SOLID principles** ensure your code is:
    
    - Easy to **extend** without breaking existing functionality.
    - **Reusable** and **maintainable**.
    - Designed with proper abstractions and minimal dependencies.
    
    Using these principles leads to robust, scalable, and flexible systems.
    
- How does spring boot handles asynchronous operation
- In spring boot we have @RestController , @Service and @Repository Annotations can we use @Component in place of them if yes why and if now why ?
- When we can use @Component Annotations
    
    We can use for utitilty classes and helper classes.
    
- What is JPA and ORM
- What are spring boot starter name
- How do you connect to database in Spring Boot
- How do you handle Exception handling in spring boot
- Whenever we need to store some confidential information we store in character Array why ?
- In java we have public static void main why we use static here  ?
- What is method hiding in java can we override the static methods ?
- What is SpringBootStarter ?
- What is the process you follow to deploy the spring boot application ?
- What are the various way we can handle the
- What is @Primary Annotations and @Qualifier
- If in the spring boot some classes are out of the package in which spring starter class is there if we want to compile those classes as well how can we do it ?
- What is Core Container in Spring ? (IOC )
- What is Spring Boot Admin ?
- Difference between PUT and PATCH
    
    PUT and PATCH are both HTTP methods used to update resources on a server, but they differ in how they modify those resources. **PUT replaces an entire resource, while PATCH updates specific fields**. 
    
- What are the response code you are aware of and it’s uses.
- What is Cache in Spring have you used it in spring boot ? what all annotations you have used
- Diff b/w @ControllerAdvice and @Controller
- How to load the data file in Spring Boot ?
- How to load the CSV file in spring boot ?
- IOC vs Dependency
- Spring boot 3 Layer Architecture
- How do we manage JWT token if it expires in sometime?
- What is CSRF ?
- When we should go for default methods and when we should go for static methods in interface ?
- What are maven build lifecycle ?
- What is default scope of maven
- What is maven plugins why you have used and where ?
- **What are conditional annotations and explain the purpose of conditional annotations in Spring Boot?**
    
    Conditional annotations in Spring Boot help us create beans or configurations only if certain
    conditions are met.
    It's like setting rules: "If this condition is true, then do this." A common example is
    @ConditionalOnClass, which creates a bean only if a specific class is present.
    This makes our application flexible and adaptable to different environments without changing the
    code, enhancing its modularity and efficiency.
    
- **How can we secure the actuator endpoints?**
    
    **Limit Exposure:** By default, not all actuator endpoints are exposed. We can control which ones are available over the web. It's like choosing what parts of your diary are okay to share.
    
    **Use Spring Security:** We can configure Spring Security to require authentication for accessing actuator endpoints.
    
    **Use HTTPS instead of HTTP.**
    
    **Actuator Role:** Create a specific role, like ACTUATOR_ADMIN, and assign it to users who should have access. This is like giving a key to only trusted people.
    
- **What strategies would you use to optimize the performance of a Spring Boot application?**
    
    Let’s say my Spring Boot application is taking too long to respond to user requests. I could:
    • Implement caching for frequently accessed data.
    • Optimize database queries to reduce the load on the database.
    • Use asynchronous methods for operations like sending emails.
    • Load Balancer if traffic is high
    • Optimize the time complexity of the code
    • Use webFlux to handle a large number of concurrent connections.
    
- **How can we handle multiple beans of the same type?**
    
    To handle multiple beans of the same type in Spring, we can use @Qualifier annotation. This lets us specify which bean to inject when there are multiple candidates.
    For example, if there are two beans of type DataSource, we can give each a name and use
    @Qualifier("beanName") to tell Spring which one to use.
    Another way is to use @Primary on one of the beans, marking it as the default choice when
    injecting that type.
    
- What is Lazy Initialization in Hibernate ?
- Difference between first level and Second level cache in hibernate
- **Pagination in spring boot?**
    
    Pagination in Spring Boot helps manage and retrieve large datasets in smaller, manageable chunks. Here’s a detailed guide covering:
    
    1. **Table Structure and Sample Data**
    2. **Entity Class**
    3. **Repository Layer**
    4. **Service Layer**
    5. **Controller Layer**
    6. **Usage of `Page` and `Pageable`**
    7. **Dynamic Query Filtering Using QueryDSL**
    
    ---
    
    ### **1. Table Structure and Sample Data**
    
    ### SQL Table: `employees`
    
    ```sql
    CREATE TABLE employees (
        id BIGINT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(100),
        department VARCHAR(50),
        age INT
    );
    
    -- Insert sample data
    INSERT INTO employees (name, department, age) VALUES
    ('Alice', 'HR', 30),
    ('Bob', 'Finance', 40),
    ('Charlie', 'IT', 25),
    ('David', 'HR', 35),
    ('Eve', 'Finance', 28);
    
    ```
    
    ---
    
    ### **2. Entity Class**
    
    The `Employee` class maps to the `employees` table.
    
    ```java
    @Entity
    @Table(name = "employees")
    public class Employee {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        private String name;
        private String department;
        private int age;
    
        // Getters and setters
    }
    
    ```
    
    ---
    
    ### **3. Repository Layer**
    
    The `PagingAndSortingRepository` provides out-of-the-box pagination and sorting.
    
    ```java
    public interface EmployeeRepository extends PagingAndSortingRepository<Employee, Long>, JpaSpecificationExecutor<Employee> {
    }
    
    ```
    
    ---
    
    ### **4. Service Layer**
    
    The service layer handles business logic and interacts with the repository.
    
    ### Basic Pagination
    
    ```java
    @Service
    public class EmployeeService {
        private final EmployeeRepository employeeRepository;
    
        @Autowired
        public EmployeeService(EmployeeRepository employeeRepository) {
            this.employeeRepository = employeeRepository;
        }
    
        public Page<List<Employee>> getAllEmployees(Pageable pageable) {
            return employeeRepository.findAll(pageable);
        }
    }
    
    ```
    
    ### Dynamic Query with QueryDSL
    
    For dynamic queries, use `QueryDSL`'s `Predicate`.
    
    ```java
    @Service
    public class EmployeeService {
        private final EmployeeRepository employeeRepository;
    
        @Autowired
        public EmployeeService(EmployeeRepository employeeRepository) {
            this.employeeRepository = employeeRepository;
        }
    
        public Page<Employee> getEmployeesByPredicate(Predicate predicate, Pageable pageable) {
            return employeeRepository.findAll(predicate, pageable);
        }
    }
    
    ```
    
    ---
    
    ### **5. Controller Layer**
    
    The controller exposes API endpoints.
    
    ### Endpoint for Basic Pagination
    
    ```java
    @RestController
    @RequestMapping("/api/employees")
    public class EmployeeController {
        private final EmployeeService employeeService;
    
        @Autowired
        public EmployeeController(EmployeeService employeeService) {
            this.employeeService = employeeService;
        }
    
        @GetMapping
        public ResponseEntity<Page<List<Employee>>> getEmployees(
                @RequestParam(defaultValue = "0") int page,
                @RequestParam(defaultValue = "5") int size,
                @RequestParam(defaultValue = "id") String sortBy
        ) {
            Pageable pageable = PageRequest.of(page, size, Sort.by(sortBy));
            Page<List<Employee>> employees = employeeService.getAllEmployees(pageable);
            return ResponseEntity.ok(employees);
        }
    }
    
    ```
    
    ### Endpoint with Dynamic Query
    
    ```java
    @GetMapping("/filter")
    public ResponseEntity<Page<Employee>> getFilteredEmployees(
            @RequestParam(required = false) String department,
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "5") int size,
            @RequestParam(defaultValue = "id") String sortBy
    ) {
        Pageable pageable = PageRequest.of(page, size, Sort.by(sortBy));
    
        // Dynamic Query Predicate
        QEmployee employee = QEmployee.employee; // Generated by QueryDSL
        BooleanBuilder builder = new BooleanBuilder();
    
        if (department != null) {
            builder.and(employee.department.eq(department));
        }
    
        Page<Employee> employees = employeeService.getEmployeesByPredicate(builder.getValue(), pageable);
        return ResponseEntity.ok(employees);
    }
    
    ```
    
    ---
    
    ### **6. Usage of `Page` and `Pageable`**
    
    - **`Page`**: Represents a page of data fetched from the database. Contains:
        - `getContent()`: The list of entities.
        - `getTotalPages()`: Total number of pages.
        - `getNumber()`: Current page number.
        - `getSize()`: Page size.
    - **`Pageable`**: Represents pagination information, such as:
        - Page number.
        - Page size.
        - Sorting information.
    
    ---
    
    ### **7. QueryDSL Setup**
    
    ### Maven Dependency
    
    ```xml
    <dependency>
        <groupId>com.querydsl</groupId>
        <artifactId>querydsl-jpa</artifactId>
        <version>5.0.0</version>
    </dependency>
    <dependency>
        <groupId>com.querydsl</groupId>
        <artifactId>querydsl-apt</artifactId>
        <version>5.0.0</version>
        <scope>provided</scope>
    </dependency>
    
    ```
    
    ### QueryDSL Configuration
    
    ```java
    @Configuration
    @EnableJpaRepositories(basePackages = "com.example.repository")
    public class QueryDSLConfig {
        @PersistenceContext
        private EntityManager entityManager;
    
        @Bean
        public JPAQueryFactory jpaQueryFactory() {
            return new JPAQueryFactory(entityManager);
        }
    }
    
    ```
    
    ---
    
    ### **8. Example Queries**
    
    ### Fetch All Employees (Basic Pagination)
    
    ```bash
    GET /api/employees?page=0&size=2&sortBy=name
    
    ```
    
    **Response**:
    
    ```json
    {
      "content": [
        {"id": 1, "name": "Alice", "department": "HR", "age": 30},
        {"id": 2, "name": "Bob", "department": "Finance", "age": 40}
      ],
      "pageable": {...},
      "totalPages": 3,
      "totalElements": 5
    }
    
    ```
    
    ### Filter by Department
    
    ```bash
    GET /api/employees/filter?department=HR&page=0&size=2&sortBy=name
    
    ```
    
    **Response**:
    
    ```json
    {
      "content": [
        {"id": 1, "name": "Alice", "department": "HR", "age": 30},
        {"id": 4, "name": "David", "department": "HR", "age": 35}
      ],
      "pageable": {...},
      "totalPages": 1,
      "totalElements": 2
    }
    
    ```
    
    ---
    
    ### **Key Notes**
    
    - Use `Pageable` for pagination logic.
    - Use `QueryDSL` for dynamic query generation.
    - Return `Page<T>` in service and controller to encapsulate both data and metadata.
    - Optimize database queries by combining pagination with filtering.
- **Spring AOP**
    - **What is AOP, How to set it up**
        
        Absolutely! Let's break down Spring AOP step by step, covering all its concepts and how it can be applied, including for authentication. I'll also include relatable analogies and practical examples.
        
        ### 1. **What is Spring AOP?**
        
        Spring AOP (Aspect-Oriented Programming) is a programming paradigm in Spring Framework that allows you to separate cross-cutting concerns from your core business logic. Cross-cutting concerns are functionalities that span multiple modules, such as logging, authentication, transaction management, and error handling.
        
        ---
        
        ### **Analogy**
        
        Imagine a restaurant where different chefs cook specialized dishes, but there's a cleaning team that handles the entire restaurant space. Instead of asking every chef to clean their kitchen, the cleaning team works separately to clean all areas. The cleaning task is a "cross-cutting concern."
        
        In software, AOP plays the role of the cleaning team. It handles common functionalities without repeating them in every business class.
        
        ---
        
        ### **Core Terminology of AOP**
        
        - **Aspect:** The cleaning service in our analogy. It encapsulates cross-cutting concerns like logging or security.
        - **Advice:** What the cleaning team does (mopping, wiping, etc.). In AOP, advice is a specific action that needs to be applied.
            - **Before:** Executes before the method.
            - **After:** Executes after the method.
            - **Around:** Wraps around a method, controlling both before and after execution.
            - **AfterReturning:** Executes only if the method returns successfully.
            - **AfterThrowing:** Executes when a method throws an exception.
        - **Join Point:** Any spot in the kitchen where cleaning can be performed. In AOP, it’s a point where an advice can be applied (like method execution).
        - **Pointcut:** A decision on which specific areas to clean. In AOP, it’s an expression that defines where an advice should be applied.
        - **Weaving:** Integrating the cleaning team with the chefs. In AOP, it's the process of injecting aspects into the application code.
        
        ---
        
        ### **Simple Example: Logging Using Spring AOP**
        
        ### **Step 1: Add Dependency**
        
        In Maven:
        
        ```xml
        <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
        
        ```
        
        ### **Step 2: Define an Aspect**
        
        ```java
        @Aspect
        @Component
        public class LoggingAspect {
        
            @Before("execution(* com.example.service.*.*(..))")
            public void logBeforeMethodExecution(JoinPoint joinPoint) {
                System.out.println("Before execution of: " + joinPoint.getSignature().getName());
            }
        
            @After("execution(* com.example.service.*.*(..))")
            public void logAfterMethodExecution(JoinPoint joinPoint) {
                System.out.println("After execution of: " + joinPoint.getSignature().getName());
            }
        }
        
        ```
        
        ### **Explanation**
        
        This aspect applies logging before and after all methods in classes under `com.example.service` package.
        
        - `@Before` and `@After`: Automatically run before and after method execution.
        - Pointcut `"execution(* com.example.service.*.*(..))"`: Targets all methods in the `com.example.service` package.
        
        ---
        
        ### **Authentication with Spring AOP**
        
        AOP can be used to enforce authentication by applying security checks at certain points in the application.
        
        ### **Example for User Authentication**
        
        ```java
        @Aspect
        @Component
        public class SecurityAspect {
        
            @Before("execution(* com.example.controller.*.*(..))")
            public void checkAuthentication() {
                if (!isUserAuthenticated()) {
                    throw new SecurityException("User is not authenticated!");
                }
            }
        
            private boolean isUserAuthenticated() {
                // Simplified check
                return SecurityContextHolder.getContext().getAuthentication() != null;
            }
        }
        
        ```
        
        ### **Explanation**
        
        - Before any controller method executes, the system checks if the user is authenticated.
        - If authentication fails, an exception is thrown.
        
        ---
        
        ### **When to Use AOP?**
        
        - **Logging:** Automatically log method calls.
        - **Security:** Apply authentication and authorization checks.
        - **Transaction Management:** Handle transactions consistently.
        - **Performance Monitoring:** Record execution time for methods.
        - **Exception Handling:** Handle exceptions uniformly.
        
        ---
        
        ### **Best Practices for AOP**
        
        1. **Pointcut Granularity:** Avoid broad pointcuts that match too many methods.
        2. **Idempotent Advice:** Ensure advice does not alter the business logic.
        3. **Performance:** Avoid computationally heavy tasks in aspects.
        4. **Readability:** Document and organize aspects for clarity.
        
        ---
        
        Would you like me to provide detailed examples on any specific advice types or more use cases such as exception handling or performance monitoring?
        
    - **Do we need to manually link to the method for which we are writing the AOP?**
        
        Great question! No, **you don't need to manually link an aspect to the target method** in Spring AOP. Spring handles this automatically through **annotations and configuration**.
        
        ### How Spring AOP Links Aspects to Methods
        
        When you define an **aspect** using `@Aspect` and specify a **pointcut** (which defines the methods to intercept), Spring dynamically creates proxies of the target beans. These proxies intercept the method calls and inject the required advice (like `@Before`, `@After`, etc.).
        
        ---
        
        ### **Key Requirements for Automatic Linking**
        
        1. **Enable AOP Support**
        Ensure that your Spring Boot application has AOP enabled by adding the necessary dependency:
            
            ```xml
            <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-aop</artifactId>
            </dependency>
            
            ```
            
        2. **Component Scanning**
        Annotate your aspect class with `@Component` so that Spring detects it during component scanning:
            
            ```java
            @Aspect
            @Component
            public class LoggingAspect {
                @Before("execution(* com.example.service.*.*(..))")
                public void logBefore(JoinPoint joinPoint) {
                    System.out.println("Executing method: " + joinPoint.getSignature());
                }
            }
            
            ```
            
        3. **Pointcut Definition**
        Use pointcut expressions like:
            
            ```java
            execution(* com.example.service.*.*(..))
            
            ```
            
            This expression tells Spring AOP to automatically apply the advice to all methods in the `com.example.service` package.
            
        
        ---
        
        ### **No Manual Invocation Required**
        
        You don't have to explicitly call the aspect from the target methods:
        
        ```java
        public void targetMethod() {
            // No need to manually link or call the aspect
        }
        
        ```
        
        Spring handles everything through proxies.
        
        ---
        
        ### **Example: Without Linking in the Target Class**
        
        ### **Aspect Class**
        
        ```java
        @Aspect
        @Component
        public class ExecutionTimeAspect {
        
            @Around("execution(* com.example.service.*.*(..))")
            public Object calculateExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
                long start = System.currentTimeMillis();
                Object result = joinPoint.proceed(); // Proceed to the actual method
                long end = System.currentTimeMillis();
                System.out.println("Execution time: " + (end - start) + " ms");
                return result;
            }
        }
        
        ```
        
        ### **Target Class**
        
        ```java
        @Service
        public class SampleService {
        
            public void performTask() {
                System.out.println("Task is being performed...");
            }
        }
        
        ```
        
        Running this will automatically log the execution time without changing `SampleService`.
        
        ---
        
        ### **In Summary**
        
        - **No manual linking** between aspects and methods.
        - Spring automatically handles this through proxies when `@Aspect`, `@Component`, and pointcut expressions are defined.
        
        Would you like a detailed explanation of how pointcuts are matched or any more examples for clarity?
        
- **Serializable Related Questions**
    
    Great point! Let's thoroughly cover each topic and question to make sure you can confidently answer them in interviews. I'll break down each topic, provide explanations, and include examples when needed.
    
    ---
    
    ### **1. What is Serialization in Java, and why is it used?**
    
    ### **Explanation:**
    
    Serialization is the process of converting a **Java object into a byte stream**. The byte stream can be saved to a file, transmitted over a network, or stored in databases.
    
    ### **Why Use Serialization?**
    
    - **Persistence:** Save objects to disk for later retrieval.
    - **Network Communication:** Send objects between systems, like in distributed applications.
    - **Caching:** Save the state of an object in memory.
    
    ### **Example:**
    
    ```java
    import java.io.FileOutputStream;
    import java.io.ObjectOutputStream;
    import java.io.Serializable;
    
    class Employee implements Serializable {
        private String name;
        private int id;
    
        public Employee(String name, int id) {
            this.name = name;
            this.id = id;
        }
    
        @Override
        public String toString() {
            return "Employee{name='" + name + "', id=" + id + "}";
        }
    }
    
    public class SerializationDemo {
        public static void main(String[] args) {
            Employee emp = new Employee("Alice", 101);
            try (FileOutputStream fileOut = new FileOutputStream("employee.ser");
                 ObjectOutputStream out = new ObjectOutputStream(fileOut)) {
                out.writeObject(emp);  // Serialization
                System.out.println("Object Serialized: " + emp);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    
    ```
    
    ---
    
    ### **2. How does Deserialization work in Java?**
    
    ### **Explanation:**
    
    Deserialization is the process of converting a **byte stream back into a Java object.**
    
    ### **How It Works:**
    
    1. Java reads the byte stream from the file/network.
    2. It reconstructs the object in memory.
    
    ### **Example:**
    
    ```java
    import java.io.FileInputStream;
    import java.io.ObjectInputStream;
    
    public class DeserializationDemo {
        public static void main(String[] args) {
            try (FileInputStream fileIn = new FileInputStream("employee.ser");
                 ObjectInputStream in = new ObjectInputStream(fileIn)) {
    
                Employee emp = (Employee) in.readObject();  // Deserialization
                System.out.println("Object Deserialized: " + emp);
    
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    
    ```
    
    ---
    
    ### **3. What is `serialVersionUID`, and why is it important?**
    
    ### **Explanation:**
    
    `serialVersionUID` is a unique identifier for each Serializable class. It helps during deserialization to ensure that the serialized object is compatible with the current class version.
    
    ### **Why Important?**
    
    If a class is modified (like adding/removing fields) after an object has been serialized, deserialization will fail if the `serialVersionUID` does not match.
    
    ### **Example:**
    
    ```java
    private static final long serialVersionUID = 1L;
    
    ```
    
    ### **What Happens Without It?**
    
    If you don't declare `serialVersionUID`, Java generates one automatically based on the class structure. However, if you modify the class, the auto-generated ID will change, causing compatibility issues.
    
    ---
    
    ### **4. What happens if `serialVersionUID` is not defined?**
    
    If `serialVersionUID` is not explicitly defined, Java will generate one at runtime based on the class's structure. Any change in the class (even minor) will result in a new `serialVersionUID`, causing deserialization to fail with an `InvalidClassException`.
    
    ---
    
    ### **5. Can you explain how `transient` works in Serialization?**
    
    ### **Explanation:**
    
    The `transient` keyword is used to skip certain fields during serialization.
    
    ### **Why Use `transient`?**
    
    - **Sensitive Information:** Avoid serializing fields like passwords or security tokens.
    - **Non-Serializable Dependencies:** Skip fields that cannot or should not be serialized.
    
    ### **Example:**
    
    ```java
    class User implements Serializable {
        private String username;
        private transient String password;  // Will not be serialized
    
        public User(String username, String password) {
            this.username = username;
            this.password = password;
        }
    
        @Override
        public String toString() {
            return "User{username='" + username + "', password='" + password + "'}";
        }
    }
    
    ```
    
    **Deserialization Result:** The `password` field will be `null`.
    
    ---
    
    ### **6. How does `Externalizable` differ from `Serializable`?**
    
    ### **Serializable:**
    
    - Default serialization is automatic.
    - Uses the default `writeObject()` and `readObject()` methods.
    
    ### **Externalizable:**
    
    - Provides **full control over serialization.**
    - Requires implementing two methods:
        
        ```java
        void writeExternal(ObjectOutput out)
        void readExternal(ObjectInput in)
        
        ```
        
    
    ### **Example of `Externalizable`:**
    
    ```java
    import java.io.*;
    
    class CustomEmployee implements Externalizable {
        private String name;
        private int id;
    
        public CustomEmployee() {}  // Mandatory no-arg constructor
    
        public CustomEmployee(String name, int id) {
            this.name = name;
            this.id = id;
        }
    
        @Override
        public void writeExternal(ObjectOutput out) throws IOException {
            out.writeUTF(name);
            out.writeInt(id);
        }
    
        @Override
        public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
            name = in.readUTF();
            id = in.readInt();
        }
    
        @Override
        public String toString() {
            return "CustomEmployee{name='" + name + "', id=" + id + "}";
        }
    }
    
    ```
    
    ---
    
    ### **7. How can you customize the serialization process in Java?**
    
    ### **Using `writeObject()` and `readObject()` Methods**
    
    These methods allow custom logic during serialization/deserialization.
    
    ### **Example:**
    
    ```java
    private void writeObject(ObjectOutputStream out) throws IOException {
        out.defaultWriteObject();
        out.writeInt(age + 5);  // Custom logic to serialize age
    }
    
    private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
        in.defaultReadObject();
        this.age = in.readInt() - 5;  // Reverse custom logic
    }
    
    ```
    
    ---
    
    ### **8. Serialization in Distributed Systems and Security Concerns**
    
    ### **Challenges**
    
    - Serialization can cause security vulnerabilities (like Remote Code Execution).
    - Serialized data is not encrypted by default.
    
    ### **Best Practices**
    
    - **Encrypt Sensitive Data:** Use encryption before serialization.
    - **Validation:** Verify object types during deserialization.
    - **Use `readResolve()` Method:** Prevent duplicate instances during deserialization.
    
    ---
    
    This covers a detailed explanation of the core questions. Want me to continue with advanced and practical coding questions or need further clarification on specific concepts? 😊
    
- Monitoring tools used in your project ?
- CICD pipeline i have used in my project .
- What is SDLC ?
- Why is Agile preferred over the waterfall model ?
- **Cascading in Spring boot entity**
    
    Cascading in JPA and Hibernate refers to the propagation of operations (such as persist, merge, remove, etc.) from a parent entity to its associated child entities. This is achieved using the `cascade` attribute of the `@OneToOne`, `@OneToMany`, `@ManyToOne`, or `@ManyToMany` annotations. Cascading ensures that operations performed on the parent entity are automatically applied to related entities.
    
    ---
    
    ## **Cascade Types**
    
    1. **`CascadeType.PERSIST`**
        - Propagates the persist operation (i.e., saving a new entity) from the parent to the child entities.
        - If a parent entity is saved, its associated entities will also be saved automatically.
        
        **Example**:
        
        ```java
        @OneToMany(mappedBy = "department", cascade = CascadeType.PERSIST)
        private List<Employee> employees;
        
        ```
        
        - When a `Department` is saved using `entityManager.persist(department)`, all associated `Employee` entities are also saved.
    
    ---
    
    1. **`CascadeType.MERGE`**
        - Propagates the merge operation, updating both the parent and child entities.
        - If a detached parent entity is merged, its associated child entities will also be updated or merged.
        
        **Example**:
        
        ```java
        @OneToOne(cascade = CascadeType.MERGE)
        private Address address;
        
        ```
        
        - When a detached `Employee` entity is merged, its `Address` will also be merged.
    
    ---
    
    1. **`CascadeType.REMOVE`**
        - Propagates the remove operation, deleting the child entities when the parent entity is deleted.
        - Useful when child entities should not exist without the parent.
        
        **Example**:
        
        ```java
        @OneToMany(mappedBy = "department", cascade = CascadeType.REMOVE)
        private List<Employee> employees;
        
        ```
        
        - When a `Department` is deleted, all associated `Employee` entities are also deleted.
    
    ---
    
    1. **`CascadeType.REFRESH`**
        - Propagates the refresh operation, reloading the state of the entity from the database.
        - Ensures the parent and child entities are updated to reflect the latest state in the database.
        
        **Example**:
        
        ```java
        @ManyToOne(cascade = CascadeType.REFRESH)
        private Company company;
        
        ```
        
        - When a `Department` is refreshed, its `Company` will also be refreshed.
    
    ---
    
    1. **`CascadeType.DETACH`**
        - Propagates the detach operation, detaching both the parent and child entities from the persistence context.
        - Useful for removing entities from the current session.
        
        **Example**:
        
        ```java
        @OneToOne(cascade = CascadeType.DETACH)
        private Address address;
        
        ```
        
        - When an `Employee` entity is detached, its `Address` will also be detached.
    
    ---
    
    1. **`CascadeType.ALL`**
        - Combines all cascade types: `PERSIST`, `MERGE`, `REMOVE`, `REFRESH`, and `DETACH`.
        - Simplifies configuration when all operations should propagate to child entities.
        
        **Example**:
        
        ```java
        @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
        private List<Employee> employees;
        
        ```
        
        - Operations performed on a `Department` entity will cascade to all associated `Employee` entities.
    
    ---
    
    ## **Examples of Cascading in Relationships**
    
    ### **1. `@OneToOne`**
    
    ```java
    @Entity
    class Employee {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @OneToOne(cascade = CascadeType.ALL)
        private Address address;
    }
    
    ```
    
    - When an `Employee` entity is persisted, its `Address` entity will also be persisted.
    
    ---
    
    ### **2. `@OneToMany`**
    
    ```java
    @Entity
    class Department {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
        private List<Employee> employees;
    }
    
    ```
    
    - Deleting a `Department` will delete all associated `Employee` entities.
    
    ---
    
    ### **3. `@ManyToOne`**
    
    ```java
    @Entity
    class Employee {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @ManyToOne(cascade = CascadeType.PERSIST)
        private Department department;
    }
    
    ```
    
    - Persisting an `Employee` will also persist the `Department` entity if it does not already exist.
    
    ---
    
    ### **4. `@ManyToMany`**
    
    ```java
    @Entity
    class Project {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @ManyToMany(cascade = CascadeType.MERGE)
        private List<Employee> employees;
    }
    
    ```
    
    - Merging a `Project` entity will also merge the associated `Employee` entities.
    
    ---
    
    ## **When to Use Cascading**
    
    - **`PERSIST`**: Use when child entities should be created automatically with the parent.
    - **`MERGE`**: Use when updating parent entities and their children together.
    - **`REMOVE`**: Use when child entities should be deleted along with the parent.
    - **`REFRESH`**: Use when the parent and children should reflect the latest state in the database.
    - **`DETACH`**: Use when the parent and children need to be detached together.
    - **`ALL`**: Use when all operations should propagate to child entities.
    
    ---
    
    ## **Considerations**
    
    1. **Avoid Unintended Deletions**:
        
        Be cautious with `REMOVE` in bidirectional relationships to prevent unintended cascading deletions.
        
    2. **Control Performance**:
        
        Cascading can result in extra queries. Use it only when necessary to avoid performance issues.
        
    3. **Relationship Ownership**:
        
        Use cascading only on the owning side of a relationship.
        
    
    ---
    
    Cascading simplifies parent-child entity management but should be used judiciously to prevent unwanted behavior.
    
- **Logging In depth in Java**
    - **How does Logging works in Spring boot (R)**
        
        Setting up logging using **SLF4J (Simple Logging Facade for Java)** provides a consistent API for logging in Java applications, while allowing you to plug in any underlying logging implementation (e.g., Logback, Log4j2, or java.util.logging).
        
        Here's a step-by-step guide to set up logging using **SLF4J** in a Spring Boot application:
        
        ---
        
        ### **Step 1: Add Dependencies**
        
        1. Add the SLF4J API dependency to your project.
        2. Include an implementation of SLF4J. In most Spring Boot applications, **Logback** is the default logging implementation.
        
        If you're using Maven, add the following dependencies to your `pom.xml`:
        
        ```xml
        <dependencies>
            <!-- SLF4J API -->
            <dependency>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-api</artifactId>
            </dependency>
        
            <!-- Logback Classic (SLF4J Binding) -->
            <dependency>
                <groupId>ch.qos.logback</groupId>
                <artifactId>logback-classic</artifactId>
            </dependency>
        
            <!-- Optionally, exclude default Spring Boot logging dependencies -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
                <scope>runtime</scope>
                <exclusions>
                    <exclusion>
                        <groupId>ch.qos.logback</groupId>
                        <artifactId>logback-classic</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>
        </dependencies>
        
        ```
        
        ---
        
        ### **Step 2: Create a Logback Configuration File**
        
        Spring Boot automatically configures logging if you provide a `logback-spring.xml` or `logback.xml` file in your `src/main/resources` folder.
        
        ### Example `logback-spring.xml`:
        
        ```xml
        <configuration>
            <!-- Console Appender: Logs to the console -->
            <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
                <encoder>
                    <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
                </encoder>
            </appender>
        
            <!-- File Appender: Logs to a file -->
            <appender name="FILE" class="ch.qos.logback.core.FileAppender">
                <file>logs/application.log</file>
                <append>true</append>
                <encoder>
                    <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
                </encoder>
            </appender>
        
            <!-- Root Logger -->
            <root level="info">
                <appender-ref ref="CONSOLE"/>
                <appender-ref ref="FILE"/>
            </root>
        </configuration>
        
        ```
        
        - **CONSOLE Appender**: Logs to the console with timestamps, thread names, and log levels.
        - **FILE Appender**: Logs to a file (`logs/application.log`) with the same format.
        
        ---
        
        ### **Step 3: Write Code Using SLF4J**
        
        In your code, use SLF4J to log messages at various levels (`info`, `debug`, `error`, etc.):
        
        ### Example `LoggingExample.java`:
        
        ```java
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import org.springframework.web.bind.annotation.GetMapping;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RestController;
        
        @RestController
        @RequestMapping("/api/logging")
        public class LoggingExample {
        
            private static final Logger logger = LoggerFactory.getLogger(LoggingExample.class);
        
            @GetMapping("/test")
            public String testLogging() {
                logger.info("INFO: This is an informational message");
                logger.debug("DEBUG: This is a debug message (useful for development)");
                logger.warn("WARN: This is a warning message");
                logger.error("ERROR: This is an error message");
                return "Logging messages have been generated. Check the logs!";
            }
        }
        
        ```
        
        ---
        
        ### **Step 4: Log Levels**
        
        Set specific log levels in the `application.properties` file to control the verbosity of logs for different packages.
        
        ### Example `application.properties`:
        
        ```
        # Root logger level
        logging.level.root=INFO
        
        # Log level for specific packages
        logging.level.org.springframework=DEBUG
        logging.level.com.yourcompany=TRACE
        
        # Log output file
        logging.file.name=logs/application.log
        
        # Log pattern for console output
        logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
        
        ```
        
        ---
        
        ### **Step 5: Where Are Logs Stored?**
        
        1. **Console**:
            - Logs will appear in your terminal or IDE console during application runtime.
        2. **Log Files**:
            - Based on the `logback-spring.xml` configuration, logs will be stored in the specified file, e.g., `logs/application.log`.
        3. **Log Rotation**:
            - You can configure log rotation to limit file size and archive old logs:
                
                ```xml
                <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                    <fileNamePattern>logs/application.%d{yyyy-MM-dd}.log</fileNamePattern>
                    <maxHistory>7</maxHistory> <!-- Keep logs for 7 days -->
                </rollingPolicy>
                
                ```
                
        
        ---
        
        ### **Best Practices for Writing Logs**
        
        1. **Use Appropriate Log Levels**:
            - Use `INFO` for general messages.
            - Use `DEBUG` for debugging information during development.
            - Use `WARN` for non-critical issues.
            - Use `ERROR` for serious problems.
        2. **Avoid Logging Sensitive Data**:
            - Do not log sensitive information like passwords, tokens, or PII (Personally Identifiable Information).
        3. **Use Structured Logging**:
            - Log messages in JSON format for better analysis (e.g., when sending logs to Splunk).
        4. **Use Placeholders for Performance**:
            - Avoid string concatenation; use placeholders in log messages:
                
                ```java
                logger.info("User {} has logged in successfully", username);
                
                ```
                
        
        ---
        
        ### **Full Example**
        
        ### `logback-spring.xml`:
        
        ```xml
        <configuration>
            <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
                <encoder>
                    <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
                </encoder>
            </appender>
        
            <root level="info">
                <appender-ref ref="CONSOLE"/>
            </root>
        </configuration>
        
        ```
        
        ### `LoggingExample.java`:
        
        ```java
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import org.springframework.web.bind.annotation.GetMapping;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RestController;
        
        @RestController
        @RequestMapping("/api/logging")
        public class LoggingExample {
        
            private static final Logger logger = LoggerFactory.getLogger(LoggingExample.class);
        
            @GetMapping("/test")
            public String testLogging() {
                logger.info("INFO: Application is running");
                logger.warn("WARN: Example warning");
                logger.error("ERROR: Example error");
                return "Logging example executed!";
            }
        }
        
        ```
        
        ---
        
        With this setup, your application will log messages using SLF4J, providing flexibility and consistent logging behavior.
        
    - **Setting up Splunk for checking logs (R)**
        
        ### **Splunk Setup in Depth**
        
        Splunk is a powerful platform used for searching, monitoring, and analyzing machine data (logs, events, metrics, etc.). To integrate your application logs into Splunk, you need to set up both **Splunk Enterprise** (or **Splunk Cloud**) and **Splunk Universal Forwarder** (to send logs from your Spring Boot application to Splunk). Below is a detailed guide on how to set up Splunk and configure it to capture and analyze logs from your microservices.
        
        ---
        
        ### **Steps to Set Up Splunk**
        
        ### **Step 1: Install Splunk Enterprise or Splunk Cloud**
        
        1. **Download Splunk**:
            - Go to the [Splunk Downloads Page](https://www.splunk.com/en_us/download.html) and download the Splunk installation package for your platform (Windows, Linux, or macOS).
        2. **Install Splunk**:
            - For **Windows**:
                - Run the installer (`.msi` file) and follow the instructions.
            - For **Linux**:
                - Download the `.tgz` file and extract it.
                - Run the Splunk installer using commands like `./splunk start` to begin using the Splunk server.
        3. **Access Splunk**:
            - After installation, you can access Splunk’s web interface by navigating to `http://localhost:8000` (default port).
            - Log in using the default username (`admin`) and password you set during the installation process.
        
        ---
        
        ### **Step 2: Set Up the HTTP Event Collector (HEC)**
        
        The **HTTP Event Collector (HEC)** is an endpoint in Splunk that receives events (logs) from external systems (like your Spring Boot application).
        
        1. **Enable HEC in Splunk**:
            - Log in to the Splunk Web interface (`http://localhost:8000`).
            - Go to **Settings** -> **Data Inputs** -> **HTTP Event Collector**.
            - Click on **New Token** to create a new HEC token.
            - Provide a name for your token, set the **Source Type** (default is `_json`), and select an index (default is `main`).
            - **Enable HEC** and save the token. This token will be used to authenticate the logs sent to Splunk.
            
            Example:
            
            - **Token Name**: `spring-logs`
            - **Index**: `main`
            - **Source Type**: `_json`
        2. **Note down the HEC token and Splunk HEC URL**:
            - **HEC URL**: `https://<splunk_server>:8088` (default port is `8088` for HEC)
            - **Token**: The token you just created (copy it).
        
        ---
        
        ### **Step 3: Install and Configure Splunk Universal Forwarder**
        
        The **Splunk Universal Forwarder** is a lightweight version of Splunk that forwards log data from your servers (or in this case, from your Spring Boot application) to the Splunk server.
        
        1. **Download and Install Splunk Universal Forwarder**:
            - Go to [Splunk Universal Forwarder Downloads](https://www.splunk.com/en_us/download/universal-forwarder.html).
            - Install the universal forwarder on the same machine that runs your Spring Boot application or any server where your application logs are stored.
        2. **Configure Splunk Universal Forwarder**:
            - Once installed, configure the forwarder to send logs to your Splunk instance.
            
            Example configuration:
            
            - Open the Splunk Universal Forwarder’s configuration file located at: `/opt/splunkforwarder/etc/system/local/inputs.conf`.
            - Add the following configuration to monitor log files:
                
                ```
                [monitor:///path/to/your/application/logs]
                disabled = false
                index = main
                sourcetype = _json
                
                ```
                
            - **/path/to/your/application/logs**: Specify the directory where your Spring Boot application’s logs are stored.
        3. **Forward Logs to Splunk**:
            - You can also configure the universal forwarder to send logs to your Splunk server over HEC.
            - In the Universal Forwarder’s configuration, specify the HEC endpoint:
                
                ```
                [http]
                disabled = false
                server = https://<splunk_server>:8088
                token = <your-hec-token>
                
                ```
                
        4. **Restart Splunk Universal Forwarder**:
            - After configuring the forwarder, restart the service:
                
                ```bash
                ./splunk restart
                
                ```
                
        
        ---
        
        ### **Step 4: Configuring Spring Boot to Send Logs to Splunk**
        
        You can send logs from your Spring Boot application to Splunk using the HTTP Event Collector (HEC).
        
        1. **Add Splunk Logback Appender**:
        Add the `logback-splunk-http` dependency in your Spring Boot `pom.xml` to enable logging directly to Splunk:
            
            ```xml
            <dependency>
                <groupId>com.splunk</groupId>
                <artifactId>logback-splunk-http</artifactId>
                <version>1.7.0</version>
            </dependency>
            
            ```
            
        2. **Configure Logback to Send Logs to Splunk**:
        Add the following configuration in `logback-spring.xml`:
            
            ```xml
            <configuration>
            
                <!-- Console Appender for Local Logging -->
                <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
                    <encoder>
                        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
                    </encoder>
                </appender>
            
                <!-- Splunk HEC Appender -->
                <appender name="SPLUNK" class="com.splunk.logging.SplunkHttpEventCollectorAppender">
                    <url>https://<splunk-hec-server>:8088</url> <!-- Splunk HEC URL -->
                    <token>your-hec-token</token> <!-- Your Splunk HEC Token -->
                    <index>main</index> <!-- Splunk Index -->
                    <sourcetype>_json</sourcetype> <!-- Sourcetype (can be customized) -->
                    <enabled>true</enabled>
                </appender>
            
                <!-- Root Logger -->
                <root level="info">
                    <appender-ref ref="SPLUNK"/>
                    <appender-ref ref="CONSOLE"/>
                </root>
            
            </configuration>
            
            ```
            
            - : The HEC URL of your Splunk instance.
            - : The HEC token you created in Splunk.
            - : The Splunk index where the logs will be stored.
            - : Type of the log source (set to `_json` for structured logs).
        3. **Test Logging**:
        Run your Spring Boot application and check if the logs are sent to Splunk. You can verify this by querying logs in the Splunk Web interface.
            
            Example Splunk query:
            
            ```
            index="main" sourcetype="_json" level="ERROR"
            
            ```
            
        
        ---
        
        ### **Step 5: Query and Visualize Logs in Splunk**
        
        Once the logs are being sent to Splunk, you can perform the following actions:
        
        1. **Search Logs**:
            - Use Splunk’s search interface to query logs. Example:
                
                ```
                index="main" sourcetype="_json"
                
                ```
                
        2. **Create Dashboards**:
            - You can create custom dashboards in Splunk to visualize your logs. Dashboards help you monitor key metrics like request rates, error rates, and system performance.
            
            Example:
            
            - Create a dashboard that shows the count of `ERROR` logs over time.
        3. **Set Up Alerts**:
            - You can configure alerts in Splunk to notify you if certain conditions are met. For example, you can set an alert if there is a spike in error logs:
            
            ```
            index="main" sourcetype="_json" level="ERROR" | stats count by _time
            
            ```
            
            - You can configure Splunk to send an email or an HTTP webhook whenever the condition is met.
        
        ---
        
        ### **Summary**
        
        - **Splunk** provides powerful log management capabilities, including indexing, searching, and visualizing logs.
        - You can **integrate Splunk with Spring Boot** by configuring **HTTP Event Collector (HEC)** and using **Splunk Universal Forwarder** for log forwarding.
        - **Logback** can be configured to send logs to Splunk via HEC, and you can also monitor and analyze logs in the Splunk Web interface.
        - Once the logs are in Splunk, you can perform searches, create dashboards, and set up alerts based on your logging data.
        
        This setup is ideal for centralized log management in microservice architectures, enabling easier monitoring and troubleshooting across distributed systems.
        
    - **Logging Check → Splunk**
        
        Let’s dive into implementing logging in a Spring Boot microservice from scratch. I’ll explain it step-by-step with an analogy, cover configurations in `application.yml` and XML, show how to change log storage locations, and walk you through integrating with Splunk. By the end, you’ll have a solid grasp of logging and how to see your logs in Splunk. Ready? Let’s go!
        
        ---
        
        ### What is Logging? (With an Analogy)
        
        Imagine you’re a chef running a busy restaurant (your microservice). Every day, customers place orders, food gets cooked, and deliveries go out. Now, logging is like keeping a detailed kitchen journal. You jot down what happens: "Customer ordered pasta at 6 PM," "Oven broke at 6:05 PM," "Pasta delivered at 6:15 PM." This journal helps you troubleshoot problems (e.g., why the oven broke), track performance (e.g., how fast deliveries are), and spot patterns (e.g., pasta is popular on Fridays). In a Spring Boot microservice, logging records events—requests, errors, or debug info—so you can monitor and fix issues.
        
        Spring Boot uses **Logback** as its default logging framework (via the `spring-boot-starter-logging` dependency), layered over **SLF4J** (a logging facade). It’s like having a standard notebook format (SLF4J) and a specific brand of notebook (Logback) to write in.
        
        ---
        
        ### Step 1: Basic Logging Setup in Your Microservice
        
        Let’s assume you have a simple Spring Boot microservice with a REST endpoint. Here’s how to start logging.
        
        ### Code Example
        
        ```java
        package com.example.demo;
        
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import org.springframework.boot.SpringApplication;
        import org.springframework.boot.autoconfigure.SpringBootApplication;
        import org.springframework.web.bind.annotation.GetMapping;
        import org.springframework.web.bind.annotation.RequestParam;
        import org.springframework.web.bind.annotation.RestController;
        
        @SpringBootApplication
        @RestController
        public class DemoApplication {
        
            // Create a logger instance
            private static final Logger logger = LoggerFactory.getLogger(DemoApplication.class);
        
            public static void main(String[] args) {
                SpringApplication.run(DemoApplication.class, args);
            }
        
            @GetMapping("/greet")
            public String greet(@RequestParam String name) {
                logger.info("Received request to greet: {}", name); // Log the event
                if (name == null || name.isEmpty()) {
                    logger.warn("Name parameter is empty or null");
                    return "Please provide a name!";
                }
                logger.debug("Processing greeting for: {}", name);
                return "Hello, " + name + "!";
            }
        }
        
        ```
        
        - **What’s Happening?**
            - `LoggerFactory.getLogger(DemoApplication.class)` gives you a logger tied to this class.
            - `logger.info()`, `logger.warn()`, `logger.debug()` write messages at different levels (INFO, WARN, DEBUG).
            - The `{}` is a placeholder for `name`, making logs efficient (no string concatenation unless the log level is enabled).
        - **Analogy**: In your kitchen journal, `info` is like noting "Order received," `warn` is "Running low on sauce," and `debug` is "Checking oven temp at 350°F."
        
        ### Default Behavior
        
        - Run this app (`mvn spring-boot:run`), and logs appear in the **console**.
        - Example output:
            
            ```
            2025-02-25 01:24:36.123  INFO 12345 --- [nio-8080-exec-1] com.example.demo.DemoApplication : Received request to greet: Alice
            2025-02-25 01:24:36.124 DEBUG 12345 --- [nio-8080-exec-1] com.example.demo.DemoApplication : Processing greeting for: Alice
            
            ```
            
        - **Where are logs stored?** By default, nowhere—they’re just printed to the console (stdout). No files yet.
        
        ---
        
        ### Step 2: Configuring Logging with `application.yml`
        
        To customize logging—like setting levels or saving logs to a file—use `application.yml`. Create it in `src/main/resources`.
        
        ### Basic `application.yml`
        
        ```yaml
        logging:
          level:
            root: INFO          # Default level for all loggers
            com.example.demo: DEBUG  # Specific level for your package
          file:
            name: logs/myapp.log  # Save logs to this file
        
        ```
        
        - **What’s Happening?**
            - `logging.level.root`: Sets the default log level (INFO means INFO and above—WARN, ERROR—show up).
            - `logging.level.com.example.demo`: Overrides the root level for your package to DEBUG (includes DEBUG, INFO, WARN, ERROR).
            - `logging.file.name`: Tells Spring Boot to write logs to `logs/myapp.log` in your project directory.
        - **Analogy**: This is like deciding your journal’s detail level (INFO = key events, DEBUG = every step) and choosing to keep it in a specific binder labeled "logs/myapp.log."
        - **Run the App**: Logs now go to both console and `logs/myapp.log`. The file is created automatically, and logs rotate at 10 MB by default.
        
        ---
        
        ### Step 3: Advanced Configuration with `logback-spring.xml`
        
        For more control (e.g., custom formats, multiple destinations), use a Logback configuration file. Spring Boot looks for `logback-spring.xml` in `src/main/resources`.
        
        ### Create `logback-spring.xml`
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>
            <!-- Define variables -->
            <property name="LOG_PATH" value="logs" />
            <property name="LOG_FILE" value="${LOG_PATH}/myapp.log" />
        
            <!-- Console appender -->
            <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
                <encoder>
                    <pattern>%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n</pattern>
                </encoder>
            </appender>
        
            <!-- File appender with rolling policy -->
            <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
                <file>${LOG_FILE}</file>
                <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
                    <fileNamePattern>${LOG_PATH}/myapp-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
                    <maxFileSize>10MB</maxFileSize>
                    <maxHistory>30</maxHistory> <!-- Keep 30 days of logs -->
                </rollingPolicy>
                <encoder>
                    <pattern>%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n</pattern>
                </encoder>
            </appender>
        
            <!-- Root logger -->
            <root level="INFO">
                <appender-ref ref="CONSOLE" />
                <appender-ref ref="FILE" />
            </root>
        
            <!-- Specific logger for your package -->
            <logger name="com.example.demo" level="DEBUG" additivity="false">
                <appender-ref ref="CONSOLE" />
                <appender-ref ref="FILE" />
            </logger>
        </configuration>
        
        ```
        
        - **What’s Happening?**
            - **Appenders**: Define where logs go (`CONSOLE` for stdout, `FILE` for a file).
            - **Rolling Policy**: Rotates logs daily or at 10 MB, keeping 30 days’ worth (e.g., `myapp-2025-02-25.0.log`).
            - **Pattern**: Customizes log format (timestamp, level, logger name, message).
            - **Loggers**: `root` sets the default level; `com.example.demo` overrides it.
        - **Analogy**: This is like upgrading your journal to a fancy system: one copy stays on the counter (console), another goes into dated folders (files), with old entries archived after 30 days.
        - **Where Logs Are Stored?** In `logs/myapp.log` (relative to your app’s root directory). Old logs are in `logs/myapp-YYYY-MM-DD.X.log`.
        
        ### Can You Use `@Configuration` Instead?
        
        Yes, but it’s less common. You’d programmatically configure Logback, which is more complex. Example:
        
        ```java
        import ch.qos.logback.classic.Level;
        import ch.qos.logback.classic.Logger;
        import ch.qos.logback.classic.encoder.PatternLayoutEncoder;
        import ch.qos.logback.core.ConsoleAppender;
        import org.slf4j.LoggerFactory;
        import org.springframework.context.annotation.Configuration;
        
        @Configuration
        public class LogbackConfig {
            public LogbackConfig() {
                Logger rootLogger = (Logger) LoggerFactory.getLogger(org.slf4j.Logger.ROOT_LOGGER_NAME);
                rootLogger.setLevel(Level.INFO);
        
                PatternLayoutEncoder encoder = new PatternLayoutEncoder();
                encoder.setPattern("%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n");
                encoder.setContext(rootLogger.getLoggerContext());
                encoder.start();
        
                ConsoleAppender<?> appender = new ConsoleAppender<>();
                appender.setEncoder(encoder);
                appender.setContext(rootLogger.getLoggerContext());
                appender.start();
        
                rootLogger.addAppender(appender);
            }
        }
        
        ```
        
        - **Pros**: Dynamic control in code.
        - **Cons**: Harder to maintain than XML/YML. Stick to `logback-spring.xml` unless you have a specific need.
        
        ---
        
        ### Step 4: Changing the Log Storage Location
        
        To store logs elsewhere (e.g., `/var/log/myapp`):
        
        1. **In `application.yml`**:
            
            ```yaml
            logging:
              file:
                name: /var/log/myapp/myapp.log
            
            ```
            
        2. **In `logback-spring.xml`**:
            
            ```xml
            <property name="LOG_PATH" value="/var/log/myapp" />
            <property name="LOG_FILE" value="${LOG_PATH}/myapp.log" />
            
            ```
            
        - **Run the App**: Ensure the directory exists and the app has write permissions (e.g., `sudo mkdir -p /var/log/myapp && sudo chown $USER /var/log/myapp` on Linux).
        - **Analogy**: You’re moving your journal from the kitchen to a secure storage room.
        
        ---
        
        ### Step 5: Integrating Splunk
        
        Splunk is a powerful tool for collecting, searching, and visualizing logs—like a digital librarian who organizes and analyzes your journals.
        
        ### Prerequisites
        
        - Install Splunk (e.g., Splunk Enterprise locally or use Splunk Cloud).
        - Get Splunk running (default: `http://localhost:8000`).
        
        ### Option 1: Send Logs via File Monitoring
        
        1. **Configure Splunk to Monitor Your Log File**
            - Log into Splunk.
            - Go to **Settings > Data Inputs > Files & Directories > New Local File & Directory**.
            - Set the path to your log file (e.g., `/var/log/myapp/myapp.log`).
            - Set the sourcetype to `log4j` or `_json` (if you format logs as JSON—see below).
            - Create an index (e.g., `microservice_logs`) to store these logs.
        2. **No App Changes Needed**
            - Splunk reads the file directly. Your app keeps writing to `myapp.log`.
        
        ### Option 2: Send Logs Directly to Splunk (HTTP Event Collector)
        
        1. **Enable HTTP Event Collector (HEC) in Splunk**
            - Go to **Settings > Data Inputs > HTTP Event Collector > New Token**.
            - Name it (e.g., `spring-boot-token`), assign it to your index (`microservice_logs`), and save.
            - Copy the token (e.g., `123e4567-e89b-12d3-a456-426614174000`).
            - Note the HEC URL (e.g., `http://localhost:8088/services/collector`).
        2. **Add Splunk Dependency**
        In `pom.xml`:
            
            ```xml
            <dependency>
                <groupId>com.splunk.logging</groupId>
                <artifactId>splunk-library-javalogging</artifactId>
                <version>1.11.8</version>
            </dependency>
            
            ```
            
        3. **Update `logback-spring.xml`**
            
            ```xml
            <appender name="SPLUNK" class="com.splunk.logging.HttpEventCollectorLogbackAppender">
                <url><http://localhost:8088></url>
                <token>123e4567-e89b-12d3-a456-426614174000</token>
                <source>spring-boot-app</source>
                <sourcetype>_json</sourcetype>
                <index>microservice_logs</index>
                <encoder class="ch.qos.logback.classic.encoder.JsonEventLayout" />
            </appender>
            
            <root level="INFO">
                <appender-ref ref="CONSOLE" />
                <appender-ref ref="FILE" />
                <appender-ref ref="SPLUNK" />
            </root>
            
            ```
            
            - **Note**: You may need `logstash-logback-encoder` for JSON:
            Then use `<encoder class="net.logstash.logback.encoder.LogstashEncoder" />`.
                
                ```xml
                <dependency>
                    <groupId>net.logstash.logback</groupId>
                    <artifactId>logstash-logback-encoder</artifactId>
                    <version>7.4</version>
                </dependency>
                
                ```
                
        4. **Run the App**
            - Logs now go to Splunk via HEC, alongside console and file.
        - **Analogy**: Instead of leaving your journal for the librarian to pick up, you’re now mailing it directly to their office.
        
        ---
        
        ### Step 6: Viewing Logs in Splunk
        
        1. **Log into Splunk** (e.g., `http://localhost:8000`).
        2. **Search**
            - Go to **Search & Reporting**.
            - Use a query like: `index=microservice_logs sourcetype="_json"`.
            - Example results:
                
                ```
                2025-02-25 01:24:36 INFO "Received request to greet: Alice"
                2025-02-25 01:24:36 DEBUG "Processing greeting for: Alice"
                
                ```
                
        3. **Create Dashboards**
            - Click **Dashboards > Create New Dashboard**.
            - Add a panel with a query (e.g., `index=microservice_logs | stats count by level`).
            - Visualize log levels over time.
        - **Analogy**: The librarian (Splunk) organizes your journal entries, letting you search for "pasta orders" or graph "oven issues" per day.
        
        ---
        
        ### Summary of Configurations
        
        - **`application.yml`**: Quick setup for levels and file output.
        - **`logback-spring.xml`**: Fine-grained control (appenders, rolling, Splunk).
        - **Log Location**: Change via `logging.file.name` or XML properties.
        - **Splunk**: Monitor files or use HEC with an appender.
        
        ### Deep Tips
        
        - **Permissions**: Ensure your app can write to custom log paths.
        - **JSON Logs**: Use `LogstashEncoder` for structured data in Splunk.
        - **Testing**: Hit your endpoint (e.g., `curl <http://localhost:8080/greet?name=Alice`>) and check console, file, and Splunk.
        
        Let me know if you want to tweak anything further—like adding MDC for request tracing or setting up alerts in Splunk!
        
    - **Basic Queries for Splunk**
        
        ### **Splunk Queries in a Production-Level Microservices Architecture**
        
        Let's assume a **real-world production** setup where multiple microservices work together:
        
        - **API Gateway** → Handles incoming requests
        - **User Auth Service** → Manages authentication
        - **Overdraft Service** → Handles overdraft requests
        - **Accounts Service** → Manages bank accounts
        - **Uninvested Funds Service** → Manages uninvested funds
        
        Each service generates logs that are sent to **Splunk** for monitoring and debugging.
        
        ---
        
        ## **🔹 Example 1: Tracking API Requests at API Gateway**
        
        ### **Scenario**:
        
        You want to check how many API calls are made to the API Gateway **per minute** and the **status codes returned**.
        
        ### **Splunk Query**:
        
        ```
        index=api_gateway_logs
        | timechart span=1m count by status_code
        
        ```
        
        🔹 **What it does?**
        
        - Extracts logs from `api_gateway_logs` index
        - Groups them **per minute (`span=1m`)**
        - Counts the number of logs for each `status_code`
        
        ✅ **Use Case**: Helps track spikes in traffic and API failures.
        
        ---
        
        ## **🔹 Example 2: Identifying User Login Failures in Authentication Service**
        
        ### **Scenario**:
        
        You want to find out how many login failures happened in the **User Auth Service** in the last **24 hours**.
        
        ### **Splunk Query**:
        
        ```
        index=user_auth_logs "login failed" earliest=-24h
        | stats count by user_id
        
        ```
        
        🔹 **What it does?**
        
        - Searches for `"login failed"` in `user_auth_logs`
        - Filters logs from **last 24 hours**
        - Groups the count of failed logins by `user_id`
        
        ✅ **Use Case**: Helps identify **which users** are experiencing login issues and may need assistance.
        
        ---
        
        ## **🔹 Example 3: Monitoring Overdraft Service for Declined Transactions**
        
        ### **Scenario**:
        
        You want to check how many **overdraft requests were declined** in the last **7 days**.
        
        ### **Splunk Query**:
        
        ```
        index=overdraft_logs action="declined" earliest=-7d
        | stats count by reason
        
        ```
        
        🔹 **What it does?**
        
        - Searches for `"declined"` overdraft requests
        - Filters logs for **last 7 days**
        - Groups results by `reason` (e.g., `"insufficient funds"`, `"credit limit reached"`)
        
        ✅ **Use Case**: Helps analyze why customers are **failing overdraft requests**.
        
        ---
        
        ## **🔹 Example 4: Finding Slowest API Calls in Accounts Service**
        
        ### **Scenario**:
        
        You want to identify API calls in the **Accounts Service** that took more than **3 seconds**.
        
        ### **Splunk Query**:
        
        ```
        index=accounts_logs response_time>3
        | table _time, api_name, response_time, user_id
        
        ```
        
        🔹 **What it does?**
        
        - Filters logs where `response_time > 3` seconds
        - Displays results in a table with `_time`, `api_name`, `response_time`, and `user_id`
        
        ✅ **Use Case**: Helps optimize **slow API endpoints**.
        
        ---
        
        ## **🔹 Example 5: Detecting Errors Across All Microservices**
        
        ### **Scenario**:
        
        You want to track **error logs across all microservices** and see where most errors are happening.
        
        ### **Splunk Query**:
        
        ```
        index=* "error" OR "exception" earliest=-1h
        | stats count by index, service_name
        
        ```
        
        🔹 **What it does?**
        
        - Searches across **all services** (`index=*`) for `"error"` or `"exception"`
        - Filters logs from **last 1 hour**
        - Counts errors per `service_name`
        
        ✅ **Use Case**: Helps **prioritize debugging** in the most error-prone services.
        
        ---
        
        ## **🔹 Example 6: Debugging a Specific User’s Transactions in Uninvested Funds**
        
        ### **Scenario**:
        
        A customer **John Doe (user_id=1234)** reports missing funds. You want to trace all their transactions.
        
        ### **Splunk Query**:
        
        ```
        index=uninvested_funds_logs user_id=1234
        | table _time, transaction_id, amount, status
        
        ```
        
        🔹 **What it does?**
        
        - Filters logs for `user_id=1234`
        - Displays a table with **time, transaction_id, amount, and status**
        
        ✅ **Use Case**: Helps track **missing transactions** for a specific user.
        
        ---
        
        ## **🔹 Example 7: API Response Time Analysis in API Gateway**
        
        ### **Scenario**:
        
        You want to monitor the **average response time of API calls** in the API Gateway.
        
        ### **Splunk Query**:
        
        ```
        index=api_gateway_logs
        | stats avg(response_time) as avg_time by api_name
        
        ```
        
        🔹 **What it does?**
        
        - Calculates the **average response time** for each API
        - Groups results by `api_name`
        
        ✅ **Use Case**: Helps identify **slow APIs** that need optimization.
        
        ---
        
        ### **🎯 Key Takeaways**
        
        1️⃣ **Splunk helps monitor and debug production microservices efficiently**
        
        2️⃣ **Use indexing (`index=...`) to filter logs for specific services**
        
        3️⃣ **Combine `stats`, `timechart`, and `table` to get insights**
        
        4️⃣ **Analyze logs per service (`api_gateway`, `auth_service`, etc.)**
        
        Would you like **custom Splunk queries** for a specific **microservice scenario** in your project? 🚀
        
- **Cascade** and **FetchType** in JPA/Hibernate
    
    ## 🚀 1. What is `Cascade` in JPA?
    
    **Cascade** tells JPA **what to do to child entities when an operation is performed on the parent entity**.
    
    For example:
    
    If you save or delete an `Author`, should it **also save or delete its Books**? That’s what cascade handles.
    
    ### ✅ Common Cascade Types:
    
    | Type | Description |
    | --- | --- |
    | `PERSIST` | Saves the child when the parent is saved. |
    | `MERGE` | Merges changes to the child when the parent is merged. |
    | `REMOVE` | Deletes the child when the parent is deleted. |
    | `REFRESH` | Refreshes the child when parent is refreshed. |
    | `DETACH` | Detaches the child when the parent is detached from persistence context. |
    | `ALL` | Applies all the above. |
    
    ---
    
    ### ✅ Code Example: Cascade
    
    ```java
    java
    CopyEdit
    @Entity
    public class Author {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        private String name;
    
        @OneToMany(mappedBy = "author", cascade = CascadeType.ALL)
        private List<Book> books = new ArrayList<>();
    }
    
    ```
    
    ```java
    
    @Entity
    public class Book {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        private String title;
    
        @ManyToOne
        @JoinColumn(name = "author_id")
        private Author author;
    }
    
    ```
    
    ### Now, in code:
    
    ```java
    java
    CopyEdit
    Author author = new Author();
    author.setName("Chetan Bhagat");
    
    Book book1 = new Book();
    book1.setTitle("2 States");
    book1.setAuthor(author);
    
    Book book2 = new Book();
    book2.setTitle("Revolution 2020");
    book2.setAuthor(author);
    
    author.getBooks().add(book1);
    author.getBooks().add(book2);
    
    // Only save author. Books will be saved automatically due to CascadeType.ALL
    authorRepo.save(author);
    
    ```
    
    ### Without `CascadeType.PERSIST` or `ALL`, you’d have to save each book manually:
    
    ```java
    java
    CopyEdit
    authorRepo.save(author);
    bookRepo.save(book1);
    bookRepo.save(book2);
    
    ```
    
    ---
    
    ## 🕵️‍♂️ 2. What is `FetchType`?
    
    **FetchType** defines **how JPA loads related entities**:
    
    ### 🟢 Types:
    
    | Type | Description |
    | --- | --- |
    | `LAZY` | Load the child **only when accessed explicitly** (default for `@OneToMany`, `@ManyToMany`) |
    | `EAGER` | Load the child entity **immediately** with the parent (default for `@ManyToOne`, `@OneToOne`) |
    
    ---
    
    ### ✅ Code Example: FetchType
    
    ```java
    java
    CopyEdit
    @OneToMany(mappedBy = "author", fetch = FetchType.LAZY)
    private List<Book> books;
    
    ```
    
    ```java
    java
    CopyEdit
    Author author = authorRepo.findById(1L).get();
    // books are not loaded yet
    
    System.out.println(author.getBooks());
    // now it triggers a SQL query to fetch books
    
    ```
    
    ### With `EAGER`:
    
    ```java
    java
    CopyEdit
    @OneToMany(mappedBy = "author", fetch = FetchType.EAGER)
    private List<Book> books;
    
    ```
    
    Then:
    
    ```java
    java
    CopyEdit
    Author author = authorRepo.findById(1L).get();
    // books are also fetched immediately
    
    ```
    
    ---
    
    ### 🔄 Cascade + FetchType – Combined Behavior
    
    | Operation | Cascade | FetchType | Behavior |
    | --- | --- | --- | --- |
    | Save | `PERSIST` / `ALL` | - | Saves child with parent |
    | Delete | `REMOVE` / `ALL` | - | Deletes child with parent |
    | Get | - | `EAGER` | Loads child immediately |
    | Get | - | `LAZY` | Loads child only when accessed |
    
    ---
    
    ## 🧠 Important Notes:
    
    1. **Use `LAZY` by default** to avoid N+1 problem and performance issues.
    2. Don't use `CascadeType.REMOVE` in `@ManyToOne` or `@OneToMany` unless you're sure. Deleting a parent could unintentionally delete many children.
    3. If using `FetchType.LAZY` in web apps, beware of `LazyInitializationException` if the session is closed.
    4. Use `@Transactional` to keep the session open when accessing LAZY properties.
    
    ---
    
    ## 🧪 Practical Example: What Happens When...
    
    ### 🧨 Case 1: `CascadeType.ALL`, `FetchType.LAZY`, `authorRepo.save(author)`
    
    - Books will be saved automatically.
    - Books will **not be loaded** with author unless accessed.
    
    ### 💥 Case 2: `CascadeType.ALL`, `FetchType.EAGER`, `authorRepo.findById()`
    
    - Author and Books are both loaded in one go (join or separate queries).
    - Useful if you need everything together.
    
    ### 🔥 Case 3: `CascadeType.REMOVE`, `authorRepo.delete(author)`
    
    - All books will be deleted too.
    - Dangerous if books are shared or should remain!
    
    ---
    
    ## 🤔 When to use DTOs instead of Entities?
    
    When sending data from your controller to the client (REST API), always prefer **DTOs** to avoid:
    
    - Infinite loops
    - Lazy loading issues
    - Over-exposing entity structure
    
    ---
    
    ## 🗣️ Interview Questions: Cascade & FetchType
    
    ### ❓ Cascade
    
    1. What is cascade in JPA? Explain different cascade types.
    2. What does `CascadeType.ALL` include?
    3. Why might `CascadeType.REMOVE` be dangerous?
    4. Can we cascade from child to parent?
    5. What happens if you forget to set cascade when saving nested objects?
    
    ### ❓ FetchType
    
    1. What’s the difference between `LAZY` and `EAGER`?
    2. What is the default FetchType for `@ManyToOne` and `@OneToMany`?
    3. What are the risks of using `EAGER` fetching?
    4. What is the N+1 select problem and how does FetchType affect it?
    5. How do you resolve `LazyInitializationException` in Spring Boot?
- **@Transactional**
    
    ## ✅ What is `@Transactional`?
    
    `@Transactional` is a Spring annotation that defines **transaction boundaries** for methods.
    
    ### 🔄 Why use it?
    
    It ensures **atomicity** — either **all the operations succeed** or **none do**. If any exception occurs, the whole transaction is rolled back.
    
    ---
    
    ### ✅ Basic Example
    
    ```java
    java
    CopyEdit
    @Service
    public class UserService {
    
        @Autowired
        private UserRepository userRepo;
    
        @Transactional
        public void createUserAndFail(String name) {
            User user = new User();
            user.setName(name);
            userRepo.save(user);
    
            // simulate failure
            if (true) {
                throw new RuntimeException("Something went wrong!");
            }
        }
    }
    
    ```
    
    Here, **no user will be saved** in the DB due to the exception. The transaction is rolled back.
    
    ---
    
    ## ⚙️ How `@Transactional` Works
    
    - **Spring creates a proxy** for your class.
    - When a method with `@Transactional` is called from **outside the bean**, Spring starts a transaction.
    - On **success**, it commits.
    - On **unchecked exception (RuntimeException)**, it rolls back.
    
    > ❗ Note: It won't rollback for checked exceptions (like IOException) unless you configure it explicitly.
    > 
    
    ---
    
    ## 📍 Where can you put `@Transactional`?
    
    - On **methods** (preferred)
    - On **classes** (applies to all public methods)
    
    ```java
    java
    CopyEdit
    @Transactional
    public class OrderService {
       public void createOrder() { ... } // transactional
    }
    
    ```
    
    ---
    
    ## 🔁 Transaction Propagation
    
    Propagation tells Spring **what to do when a transactional method calls another transactional method.**
    
    ---
    
    ### ✅ Propagation Types
    
    | Type | Description |
    | --- | --- |
    | **REQUIRED** (default) | Use the current transaction, or create a new one if none exists |
    | **REQUIRES_NEW** | Always starts a new transaction. Suspends the existing one. |
    | **NESTED** | Starts a nested transaction. Can rollback only the inner part. |
    | **SUPPORTS** | Joins existing transaction if exists, else executes non-transactionally |
    | **NOT_SUPPORTED** | Suspends any existing transaction. Executes non-transactionally |
    | **NEVER** | Throws exception if a transaction exists |
    | **MANDATORY** | Must run in a transaction. Throws exception if none exists |
    
    ---
    
    ### 🧪 Examples
    
    ### 🟢 REQUIRED (default)
    
    ```java
    java
    CopyEdit
    @Transactional(propagation = Propagation.REQUIRED)
    public void serviceA() {
        serviceB(); // joins same transaction
    }
    
    ```
    
    ### 🟠 REQUIRES_NEW
    
    ```java
    java
    CopyEdit
    @Transactional
    public void serviceA() {
        // TX1 starts
        serviceB(); // TX1 suspended, TX2 starts
        // TX2 committed
        // TX1 continues
    }
    
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void serviceB() {
        // Runs in a new, separate transaction
    }
    
    ```
    
    Use case: `audit logging`, `retrying a failure`, etc.
    
    ### 🟡 NESTED
    
    ```java
    java
    CopyEdit
    @Transactional(propagation = Propagation.NESTED)
    public void serviceB() {
        // Can rollback this part only without affecting outer transaction
    }
    
    ```
    
    Use when you want to rollback a **part of a method** but keep the outer transaction alive.
    
    > Requires DB support (e.g., savepoints)
    > 
    
    ---
    
    ### ❌ Common Pitfall
    
    ```java
    java
    CopyEdit
    @Transactional
    public void outer() {
        inner(); // no transaction starts here
    }
    
    @Transactional
    public void inner() {
        // won't work if called from same class
    }
    
    ```
    
    **Why?** Spring proxies don’t intercept internal method calls. You need to call it from another bean.
    
    ---
    
    ## 🧠 Interview Questions on `@Transactional`
    
    ### Basics
    
    1. What is `@Transactional` and why is it used?
    2. On what layers can you use `@Transactional`?
    3. What happens if an exception is thrown in a `@Transactional` method?
    4. Does `@Transactional` rollback on all exceptions?
    
    ### Advanced
    
    1. What are different transaction propagation types?
    2. What’s the difference between `REQUIRED` and `REQUIRES_NEW`?
    3. Can you use `@Transactional` on private methods?
    4. What happens if a `@Transactional` method calls another in the same class?
    5. What’s the difference between `NESTED` and `REQUIRES_NEW`?
    6. What is `rollbackFor` and `noRollbackFor` in `@Transactional`?
    
    ```java
    java
    CopyEdit
    @Transactional(rollbackFor = IOException.class)
    public void method() throws IOException {
       ...
    }
    
    ```
    
- What is the difference between save and saveAndFlush() ?
    
    

---

# MySQL

- What is the difference between Where and having clause.
    
    If you want to filter before group by then use where and after group by if you want to filter then use having.
    
    ```sql
    SELECT
    FROM WHERE (filters individual rows before grouping)
    GROUP BY
    HAVING ( filters groups after they're created)
    ORDER BY
    ```
    
- **Joins in DB?**
    
    To explain SQL joins, I'll use two example tables, **Employees** and **Departments**, and demonstrate different types of joins.
    
    ---
    
    ### Example Tables:
    
    **Employees Table:**
    
    | EmployeeID | Name | DepartmentID |
    | --- | --- | --- |
    | 1 | John | 101 |
    | 2 | Alice | 102 |
    | 3 | Bob | 103 |
    | 4 | Carol | NULL |
    
    **Departments Table:**
    
    | DepartmentID | DepartmentName |
    | --- | --- |
    | 101 | HR |
    | 102 | IT |
    | 103 | Sales |
    | 104 | Marketing |
    
    ---
    
    ### Types of Joins:
    
    ### 1. **INNER JOIN**
    
    - Combines rows from both tables where there is a match in the `DepartmentID`.
    
    **Query:**
    
    ```sql
    SELECT Employees.Name, Departments.DepartmentName
    FROM Employees
    INNER JOIN Departments
    ON Employees.DepartmentID = Departments.DepartmentID;
    
    ```
    
    **Result:**
    
    | Name | DepartmentName |
    | --- | --- |
    | John | HR |
    | Alice | IT |
    | Bob | Sales |
    
    ---
    
    ### 2. **LEFT JOIN** (or LEFT OUTER JOIN)
    
    - Returns all rows from the **left table** (Employees), and matching rows from the **right table** (Departments). If no match, `NULL` is returned.
    
    **Query:**
    
    ```sql
    SELECT Employees.Name, Departments.DepartmentName
    FROM Employees
    LEFT JOIN Departments
    ON Employees.DepartmentID = Departments.DepartmentID;
    
    ```
    
    **Result:**
    
    | Name | DepartmentName |
    | --- | --- |
    | John | HR |
    | Alice | IT |
    | Bob | Sales |
    | Carol | NULL |
    
    ---
    
    ### 3. **RIGHT JOIN** (or RIGHT OUTER JOIN)
    
    - Returns all rows from the **right table** (Departments), and matching rows from the **left table** (Employees). If no match, `NULL` is returned.
    
    **Query:**
    
    ```sql
    SELECT Employees.Name, Departments.DepartmentName
    FROM Employees
    RIGHT JOIN Departments
    ON Employees.DepartmentID = Departments.DepartmentID;
    
    ```
    
    **Result:**
    
    | Name | DepartmentName |
    | --- | --- |
    | John | HR |
    | Alice | IT |
    | Bob | Sales |
    | NULL | Marketing |
    
    ---
    
    ### 4. **FULL JOIN** (or FULL OUTER JOIN)
    
    - Combines rows from both tables, returning `NULL` where there is no match in either table.
    
    **Query:**
    
    ```sql
    SELECT Employees.Name, Departments.DepartmentName
    FROM Employees
    FULL JOIN Departments
    ON Employees.DepartmentID = Departments.DepartmentID;
    
    ```
    
    **Result:**
    
    | Name | DepartmentName |
    | --- | --- |
    | John | HR |
    | Alice | IT |
    | Bob | Sales |
    | Carol | NULL |
    | NULL | Marketing |
    
    ---
    
    ### 5. **CROSS JOIN**
    
    - Returns the Cartesian product of both tables, pairing every row of the first table with every row of the second.
    
    **Query:**
    
    ```sql
    SELECT Employees.Name, Departments.DepartmentName
    FROM Employees
    CROSS JOIN Departments;
    
    ```
    
    **Result:**
    
    | Name | DepartmentName |
    | --- | --- |
    | John | HR |
    | John | IT |
    | John | Sales |
    | John | Marketing |
    | Alice | HR |
    | Alice | IT |
    | Alice | Sales |
    | Alice | Marketing |
    | Bob | HR |
    | Bob | IT |
    | Bob | Sales |
    | Bob | Marketing |
    | Carol | HR |
    | Carol | IT |
    | Carol | Sales |
    | Carol | Marketing |
    
    ---
    
    By understanding and using these types of joins, you can retrieve related data from multiple tables as required. Let me know if you'd like to see more advanced examples!
    
    ### 6. Self Joins
    
    ### **Understanding Self Join Using an Employee Table**
    
    A **self join** is a regular join where a table is joined with itself. This is useful when rows within the same table are related to each other.
    
    ---
    
    ### **Example Employee Table**
    
    | **Employee ID** | **Name** | **Salary** | **Manager ID** |
    | --- | --- | --- | --- |
    | 1 | Alice | 90,000 | NULL |
    | 2 | Bob | 70,000 | 1 |
    | 3 | Charlie | 60,000 | 1 |
    | 4 | David | 50,000 | 2 |
    | 5 | Eva | 45,000 | 2 |
    | 6 | Frank | 40,000 | 3 |
    
    In this table:
    
    - **Employee ID** uniquely identifies an employee.
    - **Manager ID** references the **Employee ID** of the manager.
    - **Alice** is a top-level manager (no manager, `Manager ID = NULL`).
    - **Bob and Charlie** report to **Alice**.
    
    ---
    
    ### **Why Self Join?**
    
    To find hierarchical relationships, such as:
    
    1. Employees and their respective managers.
    2. Employees who work under a particular manager.
    3. Manager-employee relationships at different levels.
    
    ---
    
    ### **Self Join Query to Find Employees and Their Managers**
    
    ```sql
    SELECT
        E1.EmployeeID AS EmployeeID,
        E1.Name AS EmployeeName,
        E2.Name AS ManagerName
    FROM
        Employee E1
    LEFT JOIN
        Employee E2 ON E1.ManagerID = E2.EmployeeID;
    
    ```
    
    ---
    
    ### **Result Explanation**
    
    | **Employee ID** | **EmployeeName** | **ManagerName** |
    | --- | --- | --- |
    | 1 | Alice | NULL |
    | 2 | Bob | Alice |
    | 3 | Charlie | Alice |
    | 4 | David | Bob |
    | 5 | Eva | Bob |
    | 6 | Frank | Charlie |
    
    ---
    
    ### **Query Breakdown**
    
    1. **FROM Employee E1:** Alias `E1` refers to the employee table as the main reference.
    2. **LEFT JOIN Employee E2 ON E1.ManagerID = E2.EmployeeID:**
    Join the same table, where `E2` represents the manager for each `E1` row.
    3. **E1.Name AS EmployeeName:** Display employee names.
    4. **E2.Name AS ManagerName:** Display the corresponding manager's name. If no manager exists (`NULL`), it remains empty.
    
    ---
    
    ### **Real-Life Analogy**
    
    Imagine a **school organization chart**:
    
    - The principal (Alice) has teachers (Bob, Charlie) reporting to them.
    - These teachers have assistants (David, Eva, Frank) reporting to them.
    A **self join** helps build this chart by mapping each teacher or assistant to their superior.
    
    ---
    
    ### **Additional Examples**
    
    ### **Employees Managed by a Specific Manager (e.g., Alice)**
    
    ```sql
    SELECT E1.Name AS EmployeeName, E2.Name AS ManagerName
    FROM Employee E1
    JOIN Employee E2 ON E1.ManagerID = E2.EmployeeID
    WHERE E2.Name = 'Alice';
    
    ```
    
    **Result:**
    
    | **EmployeeName** | **ManagerName** |
    | --- | --- |
    | Bob | Alice |
    | Charlie | Alice |
    
    Would you like more complex examples, such as multi-level hierarchies or recursive queries using **CTE (Common Table Expressions)**?
    
- Find the third(3rd) highest salary;
    
    ```sql
    SELECT salary from salary ORDER BY salary DESC LIMIT 2,1;
    ```
    
- Difference between Delete / Truncate / Drop
- What is stored procedure ?
- Suppose you have a table it has grown so huge that any insert update query is taking a lot of time ? How will you overcome this ?
- While optimising why we cannot apply indexing on all the columns ?
- What is clusterd index and non clustered index in java
- What is View in SQL and why it’s different from SQL QUERY
- What is store procedures ?
- What are joins in MySQL and it’s type
    1. Inner Join : - used to retrieve the matching values of both the table.
    2. Right Join :→ get all the records from the right and only the matching ones from the left table.
    3. Left Join :→ get all the records from left and only the matching ones from the right.
    4. Full ( Outer ) Join → not supported in MySQL 
- What are Indexes and How to create an index in SQL ?
    
    Indexes are database objects which help in retrieving records quickly and more efficiently. Column indexes can be created on both Tables and Views. By declaring a Column as an index within a table/ view, the user can access those records quickly by executing the index. Indexes with more than one column are called Clustered indexes.
    
    Create index Name_Index ON employee_Test(Name)
    
    Drop index Index_name on employee_Test
    
- Disadvantages of indexes ?
    - Index takes up additional space, so the larger the table, the bigger the index.
    - Every time you perform an add, delete , or update operation, the same operation will need to be performed on the index as well.

---

# Managerial round

1. What is your role in team ?
2. If you make a mistake , how do you fix it ?
3. What are some of the workplace success that make you proud ?
4. How do you resolve workplace conflicts ?
5. If you team resists your idea , what will you do ?
6. What motivates you at work ?
7. What are your long-term career goals ?
8. Have you ever received recognition for anything ?
9. What are you strength and weaknesses ?
10. Tell me something about yourself ?
11. What is your role and responsibility in your project ?
12. Tell me something about your project ?