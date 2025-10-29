## 🧠 1️⃣ What is Generics in Java?

- Generics means writing classes, interfaces, and methods that can work with any data type, but safely (checked at compile-time).

- Without generics, you’d have to use Object type, which leads to type-casting and runtime errors.

### ✅ With Generics: Type-safe, reusable code
### ❌ Without Generics: Type casting, risk of ClassCastException

## 🏷️ 2️⃣ Generic Class

- A generic class has one or more type parameters (like placeholders for data types).
```java
🔹 Syntax:
class ClassName<T> {
    T data;
    void setData(T data) {
        this.data = data;
    }
    T getData() {
        return data;
    }
}
```
### 🔹 Example:
```java
class Box<T> {
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}

public class Main {
    public static void main(String[] args) {
        Box<Integer> intBox = new Box<>();
        intBox.set(10);
        System.out.println(intBox.get());

        Box<String> strBox = new Box<>();
        strBox.set("Hello");
        System.out.println(strBox.get());
    }
}
```

>✅ Output:

>10
>Hello


### 👉 T can be replaced by any type (Integer, String, Double, etc.) when creating the object.

## ⚙️ 3️⃣ Generic Method

- A generic method defines its own type parameter inside the method signature.

>🔹 Syntax:
>public <T> void display(T data) {
>    System.out.println(data);
>}

### 🔹 Example:
```java class Printer {
    public <T> void printData(T item) {
        System.out.println("Data: " + item);
    }
}

public class Main {
    public static void main(String[] args) {
        Printer p = new Printer();
        p.printData(100);
        p.printData("Hello Generics");
        p.printData(12.5);
    }
}
```

>✅ Output:

>Data: 100
>Data: Hello Generics
>Data: 12.5

## 🔒 4️⃣ Bounded Type Parameters

- Sometimes, you want to restrict what types can be used in generics.
- This is done with bounded types using the extends keyword.

### 🔹 Example (Upper Bound)
```java class MathBox<T extends Number> {
    private T num;

    public MathBox(T num) {
        this.num = num;
    }

    public double square() {
        return num.doubleValue() * num.doubleValue();
    }
}

public class Main {
    public static void main(String[] args) {
        MathBox<Integer> intBox = new MathBox<>(5);
        MathBox<Double> doubleBox = new MathBox<>(3.5);

        System.out.println(intBox.square());
        System.out.println(doubleBox.square());
        // MathBox<String> strBox = new MathBox<>("Hi"); ❌ Error
    }
}
```
**
✅ Output:

25.0
12.25**


> 🔸 Here, T extends Number → only subclasses of Number (Integer, Float, Double, etc.) are allowed.
```java
🔹 Lower Bound (using super)
public void addIntegers(List<? super Integer> list) {
    list.add(10);  // allowed
}
```

>It means the list can accept Integer or any of its superclasses (like Number, Object).

## 🎭 5️⃣ Wildcards ?

- Wildcards are used when you don’t know the exact type parameter.

| Type          | Meaning               |
| ------------- | --------------------- |
| `?`           | Any type (unknown)    |
| `? extends T` | Any subclass of `T`   |
| `? super T`   | Any superclass of `T` |

### 🔹 Example:

```java
import java.util.*;

class WildcardDemo {

    public static void printList(List<?> list) {
        for (Object obj : list) {
            System.out.println(obj);
        }
    }

    public static void main(String[] args) {
        List<Integer> intList = Arrays.asList(1, 2, 3);
        List<String> strList = Arrays.asList("A", "B", "C");

        printList(intList);
        printList(strList);
    }
}
```

**✅ Output:

1
2
3
A
B
C**

### 🔹 Example of bounded wildcard
```java
public void sumNumbers(List<? extends Number> list) {
    double sum = 0.0;
    for (Number n : list) {
        sum += n.doubleValue();
    }
    System.out.println("Sum = " + sum);
}
```

> ✅ Accepts List<Integer>, List<Double>, etc.

### 🧩 Summary Table
| Concept        | Syntax                          | Meaning                                    |
| -------------- | ------------------------------- | ------------------------------------------ |
| Generic Class  | `class Box<T> {}`               | Class works with any type                  |
| Generic Method | `<T> void show(T x)`            | Method works with any type                 |
| Bounded Type   | `<T extends Number>`            | Only allows subclasses of a specific class |
| Wildcard       | `?`, `? extends T`, `? super T` | Unknown type / flexible bounds             |

---

Excellent 👍 — this is a great next step!
Let’s understand **covariance and contravariance** in **Generics** and how **ArrayList** behaves with it.

---

# 🧠 Understanding Covariance, Invariance, and Contravariance in Java Generics

These terms describe **how type relationships behave** when we use generics — i.e., whether a `List<Subclass>` can be treated as a `List<Superclass>`.

---

## 🧩 1️⃣ Example Base Setup

```java
class Animal {
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    void sound() {
        System.out.println("Dog barks");
    }
}
```

---

## 🐾 2️⃣ Covariance (Allowed in Arrays but NOT in Generic Lists)

### 🔹 Covariance = “Subclass can go where Superclass is expected.”

### ✅ Works with Arrays

```java
Animal[] animals = new Dog[2];
animals[0] = new Dog();      // OK
// animals[1] = new Cat();   ❌ Runtime Error: ArrayStoreException
```

👉 Arrays are **covariant** — `Dog[]` is considered a subtype of `Animal[]`.
But this can cause **runtime errors**, so it’s unsafe.

---

### ❌ Doesn’t work with Generics

```java
List<Animal> animals = new ArrayList<Dog>(); // ❌ Compile-time error
```

✅ Reason: Generics are **invariant**, meaning `List<Dog>` is **not** a subtype of `List<Animal>`.
This prevents runtime problems.

---

## ⚙️ 3️⃣ Why Invariance in Generics?

Consider this:

```java
List<Dog> dogs = new ArrayList<>();
List<Animal> animals = dogs; // ❌ Not allowed
animals.add(new Cat());      // logically allowed if it were Animal
```

Now `dogs` would contain a `Cat`, which breaks type safety.
Hence, **Java forbids this at compile-time**.

---

## 🧱 4️⃣ How to Make It Work — Wildcards

We can use **wildcards (`? extends` and `? super`)** to handle this.

---

### 🔹 Covariance with Generics (`? extends`)

Used when you **only read** from the collection.

```java
List<? extends Animal> animals = new ArrayList<Dog>();
// animals.add(new Dog()); ❌ Not allowed
// animals.add(new Animal()); ❌ Not allowed
Animal a = animals.get(0);  // ✅ Allowed
```

✔️ You can **read** elements as `Animal`
❌ You cannot **add** anything (except `null`) because the exact subtype is unknown.

---

### 🔹 Contravariance with Generics (`? super`)

Used when you **write** to the collection.

```java
List<? super Dog> animals = new ArrayList<Animal>();
animals.add(new Dog());  // ✅ Allowed
// Dog d = animals.get(0); ❌ Not safe (returns Object)
```

✔️ You can **add** `Dog` or its subclass
❌ You cannot **read** elements as `Dog` (only as `Object`)

---

### 🔹 Invariance (Normal Generic)

```java
List<Animal> list = new ArrayList<Animal>();
List<Dog> dogs = new ArrayList<Dog>();
```

✅ Both valid
❌ But not assignable to each other.

---

## 🐕 5️⃣ ArrayList in Generics

An `ArrayList` is **invariant** like other generic types:

```java
ArrayList<Dog> dogList = new ArrayList<>();
ArrayList<Animal> animalList = new ArrayList<>();

// animalList = dogList; ❌ Not allowed
```

✅ But you can use wildcards to generalize:

```java
ArrayList<? extends Animal> animals = new ArrayList<Dog>(); // OK
```

---

## 🧩 6️⃣ Summary Table

| Concept        | Syntax        | Meaning                     | Read | Write              |
| -------------- | ------------- | --------------------------- | ---- | ------------------ |
| Covariance     | `? extends T` | Accepts subclasses of `T`   | ✅    | ❌                  |
| Contravariance | `? super T`   | Accepts superclasses of `T` | ❌    | ✅                  |
| Invariance     | `<T>`         | Exact type only             | ✅    | ✅                  |
| Arrays         | Covariant     | Can store subtype arrays    | ✅    | ❌ (runtime unsafe) |
| Generics       | Invariant     | Type-safe compile time      | ✅    | ✅                  |

---

### ✅ Example combining all:

```java
import java.util.*;

class Animal { void sound() { System.out.println("Animal sound"); } }
class Dog extends Animal { void sound() { System.out.println("Dog barks"); } }
class Cat extends Animal { void sound() { System.out.println("Cat meows"); } }

public class Demo {
    static void makeSound(List<? extends Animal> list) {
        for (Animal a : list) a.sound();
    }

    static void addDog(List<? super Dog> list) {
        list.add(new Dog());
    }

    public static void main(String[] args) {
        List<Dog> dogs = new ArrayList<>();
        dogs.add(new Dog());

        makeSound(dogs);         // ✅ Using ? extends
        List<Animal> animals = new ArrayList<>();
        addDog(animals);         // ✅ Using ? super
        makeSound(animals);
    }
}
```

✅ Output:

```
Dog barks
Dog barks
```

---

Perfect 🎯 — let’s focus on **college-exam-level important Java Generic codes** — short, conceptual, and frequently asked in viva or written exams.

Below are the **top 8 must-know programs** on **Generics, Bounded Types, Wildcards, and Covariance** 👇

---

## 🧩 **1️⃣ Generic Class Example**

```java
class Box<T> {
    private T value;

    public void setValue(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }
}

public class Main {
    public static void main(String[] args) {
        Box<Integer> intBox = new Box<>();
        intBox.setValue(10);
        System.out.println("Integer value: " + intBox.getValue());

        Box<String> strBox = new Box<>();
        strBox.setValue("Hello");
        System.out.println("String value: " + strBox.getValue());
    }
}
```

📘 **Concept:** Generic class can handle multiple data types safely.

---

## ⚙️ **2️⃣ Generic Method Example**

```java
class GenericMethodDemo {
    public static <T> void printArray(T[] array) {
        for (T element : array)
            System.out.print(element + " ");
        System.out.println();
    }

    public static void main(String[] args) {
        Integer[] intArr = {1, 2, 3, 4};
        String[] strArr = {"A", "B", "C"};

        printArray(intArr);
        printArray(strArr);
    }
}
```

📘 **Concept:** Method works with any type using `<T>` before return type.

---

## 🔒 **3️⃣ Bounded Type Example (`extends`)**

```java
class Calculator<T extends Number> {
    private T num1, num2;

    public Calculator(T num1, T num2) {
        this.num1 = num1;
        this.num2 = num2;
    }

    public double add() {
        return num1.doubleValue() + num2.doubleValue();
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator<Integer> c1 = new Calculator<>(10, 20);
        Calculator<Double> c2 = new Calculator<>(5.5, 6.5);

        System.out.println("Sum1: " + c1.add());
        System.out.println("Sum2: " + c2.add());
    }
}
```

📘 **Concept:** `T extends Number` → restricts to numeric types only.

---

## 🪄 **4️⃣ Wildcard Example (`?`)**

```java
import java.util.*;

public class WildcardExample {
    public static void printList(List<?> list) {
        for (Object obj : list)
            System.out.print(obj + " ");
        System.out.println();
    }

    public static void main(String[] args) {
        List<Integer> intList = Arrays.asList(1, 2, 3);
        List<String> strList = Arrays.asList("A", "B", "C");
        printList(intList);
        printList(strList);
    }
}
```

📘 **Concept:** `?` allows unknown type — read-only access.

---

## 🧱 **5️⃣ Upper Bound Wildcard (`? extends`)**

```java
import java.util.*;

public class UpperBoundExample {
    public static double sumNumbers(List<? extends Number> list) {
        double sum = 0;
        for (Number n : list)
            sum += n.doubleValue();
        return sum;
    }

    public static void main(String[] args) {
        List<Integer> ints = Arrays.asList(10, 20, 30);
        List<Double> doubles = Arrays.asList(2.5, 3.5);
        System.out.println(sumNumbers(ints));
        System.out.println(sumNumbers(doubles));
    }
}
```

📘 **Concept:** Use `? extends Number` when you only want to **read** data.

---

## ⚡ **6️⃣ Lower Bound Wildcard (`? super`)**

```java
import java.util.*;

public class LowerBoundExample {
    public static void addNumbers(List<? super Integer> list) {
        list.add(10);
        list.add(20);
    }

    public static void main(String[] args) {
        List<Number> nums = new ArrayList<>();
        addNumbers(nums);
        System.out.println(nums);
    }
}
```

📘 **Concept:** Use `? super T` when you want to **write** data of type `T`.

---

## 🧠 **7️⃣ Covariance Example (Using Wildcards)**

```java
import java.util.*;

class Animal { void sound() { System.out.println("Animal sound"); } }
class Dog extends Animal { void sound() { System.out.println("Dog barks"); } }

public class CovarianceExample {
    static void makeSound(List<? extends Animal> list) {
        for (Animal a : list)
            a.sound();
    }

    public static void main(String[] args) {
        List<Dog> dogs = Arrays.asList(new Dog(), new Dog());
        makeSound(dogs); // Covariant using ? extends
    }
}
```

📘 **Concept:** `? extends` allows reading from a subtype list (`List<Dog>` treated as `List<? extends Animal>`).

---

## 🧮 **8️⃣ Generic Interface Example**

```java
interface Container<T> {
    void show(T item);
}

class Display<T> implements Container<T> {
    public void show(T item) {
        System.out.println("Item: " + item);
    }
}

public class Main {
    public static void main(String[] args) {
        Display<String> d = new Display<>();
        d.show("Generics Interface Example");
    }
}
```

📘 **Concept:** Interfaces can also be generic.

---

## ✅ **Exam Tip Summary**

| No. | Concept            | Must Know For Exam                |
| --- | ------------------ | --------------------------------- |
| 1   | Generic Class      | Basic syntax & use of `<T>`       |
| 2   | Generic Method     | Type parameter before return type |
| 3   | Bounded Type       | `<T extends ClassName>`           |
| 4   | Wildcard `?`       | Used for unknown types            |
| 5   | `? extends`        | For reading (covariance)          |
| 6   | `? super`          | For writing (contravariance)      |
| 7   | Covariance Example | With `List<? extends Animal>`     |
| 8   | Generic Interface  | Common in theory questions        |

---

Nice — I’ll explain each question, show why the accepted answer is correct (or why the compiler behaves the way it does), and give the relevant Java concepts (autoboxing, erasure, overload resolution, bounded type parameters, `getClass()` with generics, etc.). I’ll also point out small typos in the multiple-choice options where they exist.

---

# 1) Primitive types ↔ wrapper classes (concept)

**Correct mapping (Java):**

* `byte` → `Byte`
* `short` → `Short`
* `int` → `Integer`
* `long` → `Long`
* `float` → `Float`
* `double` → `Double`
* `char` → `Character`
* `boolean` → `Boolean`

**Notes / typos in the question:** the MCQ options included `7.Char` and `4.Longer` — in Java the wrapper for `char` is `Character` (not `Char`) and for `long` is `Long` (not `Longer`). So if options had typos, map based on the real Java class names.

**Concept — why wrappers?**
Primitives (like `int`) are not objects. Java provides *wrapper classes* (like `Integer`) so primitive values can be used where objects are required (collections, generics, reflection APIs, etc.). Since Java 5 the compiler automatically converts between primitives and their wrappers — this is called **autoboxing** (primitive → wrapper) and **unboxing** (wrapper → primitive).
Example:

```java
Integer x = 5;   // autoboxing: compiler creates Integer.valueOf(5)
int y = x + 1;   // unboxing: compiler uses x.intValue()
```

---

# 2) Program with `class Example<T1,T2>` and two `push` methods (first version)

**Code (simplified):**

```java
class Example<T1,T2>{
    T1 ob1;
    T2 ob2;
    void push(T1 obj){ this.ob1 = obj; System.out.println(ob1); }
    void push(T2 obj){ this.ob2 = obj; System.out.println(ob2); }
}
```

Trying to compile this produces a **compile-time error**: *both methods erase to the same signature*.

**Why? (Type erasure)**
At compile time Java generics are implemented by *type erasure*. The type variables `T1` and `T2` are erased to their *erasure* types. If no bound is given, the erasure is `Object`. So both `push(T1 obj)` and `push(T2 obj)` become `push(Object obj)` after erasure — i.e. identical signatures — which is illegal (duplicate methods). The compiler therefore rejects the class.

**Accepted answer:** *Compile time error because T1 and T2 have the same erasure.* — correct.

---

# 3) Same class but `T2 extends Number` (unbounded vs bounded)

**Code:**

```java
class Example<T1, T2 extends Number> {
    void push(T1 obj) { ... }    // erases to push(Object)
    void push(T2 obj) { ... }    // erases to push(Number)
}
```

**Effect:** Now the erasures are different:

* `T1` erases to `Object`
* `T2 extends Number` erases to `Number`

So after erasure the two methods are `push(Object)` and `push(Number)` — distinct signatures. The class compiles fine. When you instantiate `Example<String,Integer>` and call `s.push("Hello")`, the `push(T1)` version is invoked and prints `Hello`.

**Accepted answer:** `Hello` — correct.

---

# 4) `Example<Number,Number>` with `T2 extends Number` and calling `n.push(5.5)`

**Code situation:**

```java
class Example<T1, T2 extends Number>{
    void push(T1 obj) { ... }   // erases to push(Object)
    void push(T2 obj) { ... }   // erases to push(Number)
}

Example<Number, Number> n = new Example<Number, Number>();
n.push(5.5);
```

**What happens?**
When you instantiate `Example<Number,Number>` the *compile-time* types of the two push methods become:

* `push(T1)` → `push(Number)` because `T1` is `Number`
* `push(T2)` → `push(Number)` because `T2` is `Number` (bound also `Number`)

So at the point of overload resolution the compiler sees *two applicable methods with identical signatures* (both `push(Number)`), causing ambiguity. The compiler issues a **compile-time error**: *reference to push is ambiguous*.

**Accepted answer:** *Compile time error because reference to method is ambiguous.* — correct.

**Intuition:** Overload resolution picks the best matching method at compile time. If two methods have the same parameter type after substitution and neither is more specific, the call is ambiguous.

---

# 5) Generic class returning runtime class name — `Example<T>` with `getClass()` check

**Code:**

```java
class Example<T>{
    T ob;
    Example(T x){ this.ob = x; }
    public String show(){ return "" + ob.getClass().getName(); }
    public T get(){ return ob; }
}

Example<String> n = new Example<String>("Hello");
Example<Double> e = new Example<Double>(10.5);

if(n.getClass() == e.getClass()){
    System.out.print(n.show() + "\n" + n.get());
} else {
    System.out.print(e.show() + "\n" + e.get());
}
```

**Output and why:**

* `n.getClass() == e.getClass()` returns `true`.
  **Reason:** Generics use *type erasure* — at runtime both `n` and `e` are instances of the raw class `Example`. The type parameters (`String`, `Double`) are compile-time only; they are not retained as separate runtime classes. So `n.getClass()` and `e.getClass()` point to the same `Example` `Class` object.

* Inside the `if` you call `n.show()` and `n.get()`:

  * `n.show()` executes `ob.getClass().getName()` where `ob` is the actual object `"Hello"` at runtime → prints `java.lang.String`.
  * `n.get()` prints `Hello`.

So the printed output is:

```
java.lang.String
Hello
```

**Accepted answer:** that pair — correct.

**Concept recap:** Even though generics provide compile-time type safety, runtime is erased. But the runtime objects placed in the generic still remember their own runtime type (`"Hello"` is a `String`), so `ob.getClass()` reports `String`.

---

# 6) Which wrapper classes extend `Number`?

**Wrapper classes that extend `Number`:**

* `Byte`
* `Short`
* `Integer`
* `Long`
* `Float`
* `Double`

**Do NOT extend `Number`:**

* `Boolean`
* `Character` (wrapper is `Character`)

**Why it matters:** `Number` is an abstract class with methods like `intValue()`, `doubleValue()`, etc. Only numeric wrappers extend it. That’s why you can write `Number n = Integer.valueOf(5)` and call `n.doubleValue()`.

**Accepted answer in your quiz (Byte, Integer)** — correct but incomplete if question allowed multiple choices; the full set is as above.

---

# 7) Match the terms: Erasure, Autoboxing, Wrapper class, Generics

**Correct mapping and short explanations:**

* **Erasure → 2.**
  *At runtime all type variables are promoted to `Object` or their upper bound.*
  (This is *type erasure* — generics compile-time only; the JVM sees non-generic classes with casts inserted.)

* **Autoboxing → 1.**
  *Implicit conversion between base types and wrapper types.*
  (Compiler converts `int` ↔ `Integer`, etc.)

* **Wrapper class → 4.**
  *Unlike primitives, compatible with the object hierarchy.*
  (Wrapper classes are true objects and subclasses of `Object`.)

* **Generics → 3.**
  *Single class, interface, and method that can be used with different data type.*
  (Parametrized types to increase code reuse and type safety.)

**Accepted answer:** `A-2, B-1, C-4, D-3` — correct.

---

## Quick summary of the main Java concepts referenced

* **Primitive vs Wrapper**: primitives are not objects. Wrappers (`Integer`, `Double`, etc.) wrap primitive values so they behave like objects.
* **Autoboxing / Unboxing**: automatic conversion between primitives and wrappers by the compiler.
* **`Number`**: abstract superclass for numeric wrappers (`Byte`, `Short`, `Integer`, `Long`, `Float`, `Double`).
* **Generics**: provide compile-time type checks and enable code reuse for multiple data types.
* **Type erasure**: generics are implemented by erasing type parameters at compile time. Type information is not available as separate parameterized types at runtime (unless you use reflection on `Field`/`Method` generic signatures or store `Class` objects explicitly).
* **Method overloading + generics**: when type variables are erased to the same type (e.g., both `Object`), two methods can collide and cause compile-time errors. Also, after substitution into concrete type arguments, overload resolution can produce ambiguity if two methods have identical signatures.

---

Excellent question 👍 — **“erasure”** (or **type erasure**) is one of the most important — and often confusing — parts of **Java generics**.
Let’s break it down step by step with examples, diagrams, and reasons.

---

## 🧩 What is *Type Erasure*?

> **Definition:**
> Type erasure is the process by which the Java compiler removes all **generic type information** (like `<T>`, `<E>`, `<K,V>`) at **compile time**, and replaces them with their **upper bounds** (usually `Object`) in the generated bytecode.

In simpler words:

> Generics exist only **at compile time** for **type safety** and **readability**, but at **runtime**, they **disappear**.

---

## ⚙️ Why does Java erase types?

Java’s generics were added in **Java 5 (2004)** — *after* the language was already widely used.
To remain **backward-compatible** with older (non-generic) code and the JVM, Java had to implement generics **without changing the JVM itself**.

So, instead of creating a new kind of “generic-aware JVM”, Java chose a **compiler trick**:

* The compiler enforces type safety using generics.
* After compilation, it **removes** (erases) all the generic type parameters.

---

## 🧠 Example 1 — What happens at compile time and runtime

```java
// Code with generics
List<String> names = new ArrayList<String>();
names.add("Alice");
String s = names.get(0);
```

➡️ **At compile time:**
The compiler ensures:

* You can only insert `String` objects into `names`.
* `names.get(0)` returns a `String` (no cast needed).

➡️ **After compilation (bytecode / runtime):**
The JVM sees something like:

```java
List names = new ArrayList(); // no generic info
names.add("Alice");
String s = (String) names.get(0); // compiler inserted cast
```

✅ Type safety is enforced by the compiler.
❌ But the generic type information (`<String>`) is erased at runtime.

---

## 🧩 Example 2 — Type erasure and method overloading conflict

```java
class Example<T1, T2> {
    void push(T1 obj) { System.out.println(obj); }
    void push(T2 obj) { System.out.println(obj); }
}
```

During **type erasure**:

* Both `T1` and `T2` are erased to `Object`
* So both methods become:

  ```java
  void push(Object obj) { ... }
  void push(Object obj) { ... }
  ```

Duplicate methods — ❌ compile-time error.
Hence the message:

> “Compile-time error because T1 and T2 have the same erasure.”

---

## 🧩 Example 3 — With bounds

```java
class Example<T1, T2 extends Number> {
    void push(T1 obj) { }
    void push(T2 obj) { }
}
```

Now erasure replaces:

* `T1` → `Object`
* `T2` → `Number`

So the bytecode has:

```java
void push(Object obj) { }
void push(Number obj) { }
```

✅ Different signatures — code compiles.

---

## 🕵️‍♀️ Example 4 — Why `n.getClass() == e.getClass()` is true

```java
Example<String> n = new Example<String>("Hello");
Example<Double> e = new Example<Double>(10.5);
```

At runtime:

* Both become instances of the **same raw type** `Example`.
* Because `Example<String>` and `Example<Double>` both erase to just `Example`.

Hence:

```java
n.getClass() == e.getClass() // true
```

---

## 🧮 Quick Summary Table

| Phase                       | What happens                               | Example result                             |
| --------------------------- | ------------------------------------------ | ------------------------------------------ |
| **Compile time**            | Compiler checks types using generics       | `List<String>` only stores Strings         |
| **After erasure**           | Type parameters (`<T>`) are removed        | Code uses raw `List`                       |
| **Upper bound replacement** | Unbounded → `Object`; bounded → bound type | `<T extends Number>` → `Number`            |
| **Runtime**                 | Generic type info is gone (mostly)         | Reflection sees `List`, not `List<String>` |

---

## 💡 Key Points to Remember

1. **Erasure = Generics don’t exist at runtime.**
   (They are a compile-time feature.)

2. **Erasure replaces type parameters with their bounds.**

   * Unbounded `<T>` → `Object`
   * Bounded `<T extends Number>` → `Number`

3. **Erasure causes:**

   * Ambiguity in overloaded methods (as seen in your question)
   * `n.getClass() == e.getClass()` returning `true`
   * No `instanceof` checks with generic types (e.g. `if (x instanceof List<String>)` ❌ illegal)

4. **You can use reflection** to read generic *declarations* (like `Field` type parameters), but **not runtime instances**.

---

## ⚡ Mini Code Demo to Visualize Erasure

```java
import java.util.*;

public class ErasureDemo {
    public static void main(String[] args) {
        List<String> list1 = new ArrayList<>();
        List<Integer> list2 = new ArrayList<>();

        System.out.println(list1.getClass() == list2.getClass()); // true
        System.out.println(list1.getClass().getName()); // java.util.ArrayList
    }
}
```

**Output:**

```
true
java.util.ArrayList
```

✅ Because after erasure, both lists are simply `ArrayList`.

---

