# LLDRevisionAndLearning

Oops is an acronym that stands for **Object-Oriented Programming**. It's a programming paradigm based on the concept of "objects," which are data structures that contain both **data (fields or attributes)** and **code (methods or behaviors)**. The primary goal of OOP is to design software in a way that is modular, reusable, and easy to maintain.

## Core Concepts of OOP in Java

There are four main pillars of OOP:

### 1\. Encapsulation

**Encapsulation** is the bundling of data and the methods that operate on that data into a single unit, known as a class. It also involves restricting direct access to some of the object's components. This is achieved using access modifiers like `public`, `private`, and `protected`. The primary benefit is **data hiding**, which protects an object's internal state from outside interference.

**Example:**

```java
public class BankAccount {
    private double balance; // Data is hidden

    // Public methods to access the data
    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            this.balance += amount;
        }
    }
}
```

In this example, the `balance` is `private`, so it can't be changed directly. You must use the `deposit` method to modify it, which ensures the data remains valid (e.g., you can't deposit a negative amount).

-----

### 2\. Inheritance

**Inheritance** is a mechanism where one class (the **subclass** or **child class**) can inherit fields and methods from another class (the **superclass** or **parent class**). This promotes **code reusability** and establishes a "is-a" relationship between classes.

**Example:**

```java
public class Animal {
    public void eat() {
        System.out.println("This animal eats food.");
    }
}

public class Dog extends Animal { // Dog inherits from Animal
    public void bark() {
        System.out.println("The dog barks.");
    }
}

// Usage
Dog myDog = new Dog();
myDog.eat(); // Inherited from Animal
myDog.bark(); // Specific to Dog
```

Here, `Dog` inherits the `eat()` method from `Animal`. This means you don't have to write the same method again in the `Dog` class.

-----

### 3\. Polymorphism

**Polymorphism** means "many forms." In Java, it allows objects of different classes to be treated as objects of a common superclass. This is often achieved through method **overriding** and **overloading**.

  * **Method Overriding:** A subclass provides a specific implementation of a method that is already defined in its superclass.
  * **Method Overloading:** Multiple methods in the same class have the same name but different parameters.

**Example (Method Overriding):**

```java
public class Animal {
    public void makeSound() {
        System.out.println("The animal makes a sound.");
    }
}

public class Cat extends Animal {
    @Override // Annotation to indicate overriding
    public void makeSound() {
        System.out.println("The cat meows.");
    }
}

// Usage
Animal myCat = new Cat();
myCat.makeSound(); // Prints "The cat meows."
```

Even though `myCat` is declared as type `Animal`, the specific `makeSound()` method of the `Cat` class is called at runtime. This is called **runtime polymorphism**.

-----

### 4\. Abstraction

**Abstraction** is the process of hiding complex implementation details and showing only the essential features of an object. It focuses on what an object does, rather than how it does it. This is typically achieved using **abstract classes** and **interfaces**.

**Example:**

```java
// Abstract class
public abstract class Shape {
    public abstract double getArea(); // Abstract method with no body

    public void display() {
        System.out.println("This is a shape.");
    }
}

public class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
}
```

The `Shape` class is `abstract` and defines an abstract method `getArea()`. It forces any subclass (like `Circle`) to provide its own implementation of the `getArea()` method. Users of the `Shape` class don't need to know the specific formula for the area; they only need to know that the `getArea()` method exists.