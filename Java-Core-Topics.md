#  Java `Object` Class – The Root of All Classes

The `java.lang.Object` class is the **superclass of all classes in Java**. It defines the **common behavior** that every Java object inherits, either directly or indirectly.

---

## Why is `Object` Class Important?

- It provides **default implementations** for basic methods like:
  - `equals()`
  - `hashCode()`
  - `toString()`
  - `clone()`
  - `getClass()`
  - `wait()`, `notify()`, `notifyAll()`

- Every class you write automatically **inherits from `Object`**, unless you explicitly extend another class.

---

## List of Methods in `Object` Class

| Method Signature                                | Description |
|--------------------------------------------------|-------------|
| `public boolean equals(Object obj)`             | Compares this object to another for logical equality |
| `public native int hashCode()`                  | Returns hash code for this object |
| `public String toString()`                      | Returns a string representation of this object |
| `protected Object clone() throws CloneNotSupportedException` | Creates a shallow copy of this object |
| `public final Class<?> getClass()`              | Returns the runtime class of this object |
| `protected void finalize() throws Throwable`    | Called by the GC before object is reclaimed *(deprecated)* |
| `public final void wait()`                      | Causes thread to wait until notified |
| `public final void wait(long timeout)`          | Waits for specified time |
| `public final void wait(long timeout, int nanos)` | Waits with nanosecond precision |
| `public final void notify()`                    | Wakes up one thread waiting on this object's monitor |
| `public final void notifyAll()`                 | Wakes up all threads waiting on this object's monitor |

---

## Detailed Explanation of Key Methods

### 1. `equals(Object obj)`
Checks if two objects are **logically equal**. Default implementation checks for **reference equality**.

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true;
    if (obj == null || getClass() != obj.getClass()) return false;
    MyClass other = (MyClass) obj;
    return id == other.id && name.equals(other.name);
}
```

---

### 2. `hashCode()`
Returns a hash code, used in hashing data structures (`HashMap`, `HashSet`).

- Must be **consistent with `equals()`**.
- If `equals()` is overridden, `hashCode()` **must** be overridden too.

---

### 3. `toString()`
Provides a **textual representation** of an object. Default is:
```
ClassName@HexHashCode
Ex: HDFCLoan@heytbdhg5534gas

```

 Recommended: Override it for logging/debugging.

---

### 4. `clone()`
Performs a **shallow copy** of the object.

- Class must implement `Cloneable` interface.
- Otherwise, throws `CloneNotSupportedException`.

```java
@Override
protected Object clone() throws CloneNotSupportedException {
    return super.clone();
}
```

---

### 5. `getClass()`
Returns a `Class` object that represents the **runtime class** of the object.

```java
Class<?> clazz = obj.getClass();
```

---

### 6. `finalize()` (Deprecated since Java 9)
Used to perform cleanup before GC. Not recommended due to:
- Unpredictability
- Performance overhead

Use `AutoCloseable` instead.

---

### 7. `wait()`, `notify()`, `notifyAll()`
Used for **inter-thread communication** (low-level concurrency control).

- Must be called inside **synchronized** block or method
- `wait()` pauses the thread
- `notify()` wakes one waiting thread
- `notifyAll()` wakes all

```java
synchronized (lock) {
    while (!condition) lock.wait();
    // do work
}
```

---

##  Best Practices

| Action                 | Recommendation |
|------------------------|----------------|
| Override `equals()`    | Yes, for domain models |
| Override `hashCode()`  | Always with `equals()` |
| Override `toString()`  | Yes, for readability |
| Use `clone()`          | Prefer copy constructors or static factory |
| Avoid `finalize()`     | Yes, use `AutoCloseable` instead |
| Use `wait/notify`      | Prefer `java.util.concurrent` utilities |

---

##  Example Class Using Object Methods

```java
public class Employee implements Cloneable {
    private int id;
    private String name;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Employee)) return false;
        Employee e = (Employee) o;
        return id == e.id && name.equals(e.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }

    @Override
    public String toString() {
        return "Employee{id=" + id + ", name='" + name + "'}";
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```
