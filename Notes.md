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


