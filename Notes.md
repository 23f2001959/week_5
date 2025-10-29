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
